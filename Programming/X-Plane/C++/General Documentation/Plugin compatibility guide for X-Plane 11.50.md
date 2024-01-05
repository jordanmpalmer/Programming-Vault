Starting with X-Plane 11.50, users have the ability to change the graphics API that is used by X-Plane. They now have the option to choose between OpenGL (all platforms), Vulkan (Windows and Linux) and Metal (macOS). This has potential implications for third party code that interacts with X-Plane’s rendering. The main goal with Vulkan and Metal is to deliver a more consistent and higher framerate, while also maintaining compatibility with the existing add on ecosystem as much as possible.

---

## Compatibility with existing plugins

As a general note, existing plugins running under the OpenGL renderer in X-Plane 11.50 should behave exactly the same as in X-Plane 11.41. Assuming the plugins behaved nicely and played by the rules of the SDK, compatibility between 11.50 OpenGL and 11.41 is provided.

Whether a user selects OpenGL, Vulkan or Metal, the following kinds of operations don’t require any changes to existing add-ons:

- Scenery add ons. All renderers support the existing scenery features and texture formats and will look the same on all platforms.
- Plugins that don’t interact with the rendering system and instead provide additional functionality without the need to draw in the world.
- Window drawing using the XPLM 3 SDK windows. This includes UI elements drawn using the [XPWidgets](https://developer.x-plane.com/sdk/XPWidgets/) library as well as custom OpenGL drawing.
- 2D drawing callbacks. This includes UI elements drawn using the [XPWidgets](https://developer.x-plane.com/sdk/XPWidgets/) library as well as custom OpenGL drawing.
- Panel drawing callbacks. This includes 2D panels as well as 3D panels drawn inside the aircraft.
- 3D drawing using the XPLM instancing API.

Breaking changes have been made to the 3D drawing callbacks. All 3D drawing phases have been deprecated as of X-Plane 11.50 and will no longer be called under either the Vulkan or Metal renderer. Additionally, the [XPLMDrawObjects](https://developer.x-plane.com/sdk/XPLMDrawObjects/)() and [XPLMDrawAircraft](https://developer.x-plane.com/sdk/XPLMDrawAircraft/)() APIs have been deprecated and also no longer works with either Vulkan or Metal. 3D drawing callbacks, [XPLMDrawObjects](https://developer.x-plane.com/sdk/XPLMDrawObjects/)(), and [XPLMDrawAircraft](https://developer.x-plane.com/sdk/XPLMDrawAircraft/)() will continue to work under OpenGL in 11.50 like they did in 11.41.

X-Plane has offered an alternative to the [XPLMDrawObjects](https://developer.x-plane.com/sdk/XPLMDrawObjects/)() and [XPLMDrawAircraft](https://developer.x-plane.com/sdk/XPLMDrawAircraft/)() APIs since X-Plane 11.10 in the form of the instancing API, which we highly encourage developers to adopt. While not a direct, drop in replacement, it offers much more flexibility for plugin authors and X-Plane than the previous system did. You can read the documentation for the instancing API [here](https://developer.x-plane.com/sdk/XPLMInstance/) and check out our example code [here](https://developer.x-plane.com/code-sample/instanced-drawing-sample/).

### OpenGL compatibility notes

When a user runs X-Plane with either Vulkan or Metal, X-Plane itself will exclusively run with the selected backend and no rendering is done using OpenGL. To provide compatibility with plugins X-Plane will create a OpenGL context that is exclusively for use by plugins. X-Plane will create minimal resources in this OpenGL context to enable support for APIs like [XPLMGetTexture](https://developer.x-plane.com/sdk/XPLMGetTexture/)() and to facilitate sharing of framebuffers across Vulkan/Metal and OpenGL. Plugins should not assume the existence of any resources in an OpenGL context and should instead use public APIs to retrieve resources. No guarantee is made with regards to availability of resources in the OpenGL context across versions or even runs of X-Plane. Plugins that try to sniff resources from the OpenGL context, whether running under Vulkan, Metal or OpenGL, are considered misbehaved and no compatibility guarantees are made.

Plugins that use Vulkan/Metal compatible drawing operations don’t need to do any special processing or precautions. X-Plane provides all synchronization and resource sharing across the rendering APIs as part of its internal plugin bridge system. We expect this OpenGL plugin bridge to exist for the foreseeable future as something that plugins can be authored against. OpenGL provides an easy to use API for plugin needs without any of the complications that come with Vulkan or Metal.

---

## Alternatives to 3D drawing

As mentioned above, the X-Plane 11.41 3D drawing phases have been deprecated in X-Plane 11.50 and are no longer called on Vulkan and Metal. This should not be a problem for most plugins, although a few might require changing to the XPLM instancing API. However, we have found that a lot of third party plugins rely on the 3D drawing phases to read datarefs or do internal bookkeeping, without actually doing any real drawing. This will no longer work under either Vulkan or Metal with 11.50 due to these drawing phases no longer being dispatched, although it is still possible under OpenGL.

We strongly recommend authors avoid doing this in general, even on OpenGL. 3D drawing phases are meant exclusively for 3D drawing, and dispatching them isn’t free in terms of CPU overhead on X-Plane’s end. Plugins that don’t require 3D drawing capability should instead use the flight loop callbacks to do all of their bookkeeping.

We also discovered that a lot of 3D drawing is actually in the form of overlays or other kinds of markers or labels on 3D objects. These can be done from a 2D drawing callback instead of a 3D drawing callback by transforming the 3D world coordinates onto the 2D screen and then using those coordinates to draw. This will work regardless of whether a user runs OpenGL, Vulkan or Metal. Example code that deals with the 3D world to 2D screen coordinate transform can be found [here](https://developer.x-plane.com/code-sample/coachmarks/). Note though that this will always draw over world objects since there is no depth information available.

We expect most plugins to be able to entirely phase out their usage of the 3D drawing phases with minimal code changes necessary, either by adopting the instancing API, moving bookkeeping to the flight loop, or using a 2D drawing callback to draw overlays. We highly recommend exploring these alternatives to the previous 3D drawing approaches as they allow X-Plane to run much more efficiently.

---

## 3D drawing using the modern drawing phase

Plugins that require completely custom drawing in the X-Plane world with the ability to be occluded by other objects won’t work with the above work arounds. We don’t expect very many plugins to be in this category, with custom weather rendering plugins being the notable exception. For plugins like this, X-Plane 11.50 offers a new 3D drawing phase as part of the XPLM 302 SDK: _[](https://developer.x-plane.com/sdk/XPLMDrawingPhase/)[xplm_Phase_Modern3D](https://developer.x-plane.com/sdk/xplm_Phase_Modern3D/)_. For technical reasons this drawing phase is not available on Metal and only runs on Vulkan and OpenGL!

This new drawing phase has many caveats and gotchas and comes with the big fat warning that it isn’t very cheap in terms of CPU overhead. We highly discourage the use of it unless it’s absolutely necessary to perform real, custom 3D drawing! As mentioned before, the biggest caveat is that this drawing phase will not be called under Metal. Additionally there is only a single drawing phase now which conceptually sits right where the old before _[xplm_Phase_Airplanes](https://developer.x-plane.com/sdk/xplm_Phase_Airplanes/)_ draw phase used to be. The before and after phase of the modern 3D drawing phase are called directly in succession and no X-Plane rendering happens in between.

There are two additional things to watch out for when doing 3D rendering in general:

First, X-Plane can run with reverse-z semantincs. Meaning that the depth range is [0, 1] as opposed to the traditional [-1, 1] and the depth comparison is _GEQUAL_ instead of _LEQUAL_. As a side effect, this means that the depth buffer is cleared to 0 instead of 1. When in reverse-z mode, the depth buffer will be in floating point format (_DEPTH_32F_) instead of the traditional 24bit integer format. X-Plane will set up the OpenGL state and projection matrices for you to reflect these changes when dispatching plugins, but it’s something you might want to be aware of if you do any depth buffer readbacks, set the depth state yourself, or calculate your own projection matrices. Whether X-Plan is in reverse-z mode can be observed through the _sim/graphics/view/is_reverse_float_z_ dataref. When running under Vulkan, X-Plane will always run in reverse-z mode, when running under OpenGL it’ll only be in reverse-z mode for VR rendering.

Second, X-Plane can run with reverse-y semantics. In reverse-y mode, the colour and depth buffer are flipped along the y-axis (the origin is top left and grows downward). When running in reverse-y mode, it’s the responsibility of the plugin author to flip the front face winding to avoid polygons from getting incorrectly backface culled. The projection matrix provided by X-Plane takes care of the changed projection parameters, but if you do your own projection matrix set-up you’ll have to do this change yourself. Whether X-Plane is in reverse-y mode can be observed through the _sim/graphics/view/is_reverse_y_ dataref. When running under Vulkan, only 3D drawing with the HDR renderer is in reverse-y mode. 2D rendering phases and OpenGL rendering are never in reverse-y mode.