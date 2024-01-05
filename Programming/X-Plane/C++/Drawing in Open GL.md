https://forums.x-plane.org/index.php?/forums/topic/214939-implementing-modern-opengl/

https://forums.x-plane.org/index.php?/forums/topic/210082-modern-opengl-for-rendering-in-2d-to-a-vr-custom-window/

https://forums.x-plane.org/index.php?/forums/topic/185579-drawing-2d-opengl-on-a-3d-panel/#comment-1705321

I started making baby steps into OpenGL about a year ago and then got distracted for another year (it's a bit of a theme) but this is a minimal example, I think.

It will one day be a weather radar but for now it's a very crude CRT artificial horizon, drawn into the bottom left corner of the 3D panel texture. The dark orange colour is part of Panels_General.png.

[![2019-07-18-132752_1366x768_scrot.jpg](https://forums.x-plane.org/uploads/monthly_2019_07/2019-07-18-132752_1366x768_scrot.jpg.e41457934449829aba186ff5a40e6e59.jpg)](https://forums.x-plane.org/uploads/monthly_2019_07/2019-07-18-132752_1366x768_scrot.jpg.e41457934449829aba186ff5a40e6e59.jpg "Enlarge image")

```cpp
// ekco190.h
#include <vector>
#include <GL/glew.h>
#include <XPLMDisplay.h>
#include <dataref.h> // PPL::DataRef

class Ekco190 {
public:
  Ekco190();
  ~Ekco190();

private:
  const size_t WIDTH{400};
  const size_t HEIGHT{200};
  void draw_();
  XPLMDrawCallback_f draw_cb_ptr_;

  // PPL::DataRef is an RAII wrapper around the X-Plane dataref SDK
  // basically lets me read a dataref as if it was a native type
  PPL::DataRef<float> pitch_{
      "sim/cockpit2/gauges/indicators/pitch_electric_deg_pilot"};
  PPL::DataRef<float> roll_{
      "sim/cockpit2/gauges/indicators/roll_electric_deg_pilot"};
};
// ekco190.cpp
Ekco190::Ekco190() {
  // this is a relatively unclunky way to connect a class instance's member function
  // to the C-style function pointer used by the API
  draw_cb_ptr_ = [](XPLMDrawingPhase, int, void* refcon) {
    reinterpret_cast<Ekco190*>(refcon)->draw_();
    return 1;
  };
  
  XPLMRegisterDrawCallback(draw_cb_ptr_, xplm_Phase_Gauges, 0, this);
}

Ekco190::~Ekco190() {
  XPLMUnregisterDrawCallback(draw_cb_ptr_, xplm_Phase_Gauges, 0, this);
}

void Ekco190::draw_() {
  // enable alpha blending but nothing else - YMMV
  XPLMSetGraphicsState(0, 0, 0, 0, 1, 0, 0);

  // set up to draw some orange lines
  glColor4f(0.8f, 0.4f, 0.05f, 1.0f);
  glLineWidth(1.8f);
  glBegin(GL_LINES);
  
  // draw a vertical line positioned according to roll angle
  glVertex2f((1 + roll_ / 60.f) * WIDTH / 2, 0);
  glVertex2f((1 + roll_ / 60.f) * WIDTH / 2, HEIGHT);
  
  // draw a horizonal line positioned according to pitch angle
  glVertex2f(0, (1 + pitch_ / 30.f) * HEIGHT / 2);
  glVertex2f(WIDTH, (1 + pitch_ / 30.f) * HEIGHT / 2);
  
  // finish drawing lines
  glEnd();

  return;
}
```

The pattern I've adopted for plugins is to sling everything into a class and then instantiate a unique_ptr of that class in XPluginStart. Oh: don't forget to initiate glewInit().


```cpp
// plugin.cpp
#include <XPLMPlugin.h>
#include <XPLMUtilities.h>

#include "ekco190.h"
  
std::unique_ptr<Ekco190> ekco190;

PLUGIN_API int XPluginStart(char* outName, char* outSig, char* outDesc) {
  {
    char name[] = "Ekco190";
    char sig[] = "Ekco190";
    strcpy(outName, name);
    strcpy(outSig, sig);
    strcpy(outDesc, GIT_REVISION);
    // meanwhile in the project build config doohickey, words to the effect of:
    // DEFINES += GIT_REVISION='\\"$$system(git rev-parse --short HEAD)\\"'
  }

  glewInit();

  // setting everything up in a try/catch block saves a lot of frustrating CTDs
  try {
    ekco190 = std::make_unique<Ekco190>();
  } catch (const std::exception& e) {
    XPLMSpeakString(e.what());
  } catch (...) {
    XPLMSpeakString("Some non-std::exception initialising Ekco190");
  }
  return 1;
}

PLUGIN_API void XPluginStop(void) {}
PLUGIN_API void XPluginDisable(void) {}
PLUGIN_API int XPluginEnable(void) {
  return 1;
}
```

PLUGIN_API void XPluginReceiveMessage(XPLMPluginID, long, void*) {}
(Incidentally I have no idea if there's any downside to calling glewInit multiple times, in which case it'd make sense to move it out of plugin.cpp and into the initialiser of any class that does OpenGL-y things. Nor if I ought to reset the unique_ptr in XPluginStop…)


>Hm I don't understand the code above. Where's the part that sets up the projection matrix and stuff. How do you get the coordinates of the 3d panel surface that you want to be drawing onto. How do you transform from 2d space coordinates to 3d space coordinates?


Ah right. On my aircraft the 3D panel texture file is <\aircraft folder>/cockpit_3d/-PANELS-/Panel_General.png. In PlaneMaker's menus under Standard/Viewport/General/Cockpit, it's set to General Aviation. I'm pretty sure these two things are related but I'm not familiar enough with PlaneMaker to be certain.

I made the 3D model in Blender. In the layer which creates the cockpit object, there is a Blender object consisting of a single quad for the screen, and with "Part Of Cockpit Panel" ticked in the X-Plane section of its Material properties, indicating it is to be textured by the 3D panel texture and not by the cockpit object texture. The UV-wrapping for that quad is for the same section of the texture that gets drawn to by the plugin. Panels_General.png is 1024x1024 and the plugin draws onto the rectangle between (0,0) and (400,200), so the quad's UV coords are the corners of the rectangle between (0,0) and (400/1024, 200,1024) or (0.390625, 0.1953125).

(The cockpit .obj file is generally textured by the .png file with the same name, except for bits which are flagged as part of the panel, which are textured by the 3D panel.)

The projection matrix etc is all handled by X-Plane. By drawing during xplm_Phase_Gauges we're just drawing directly onto the texture used by some triangles in the aircraft's cockpit object. X-Plane handles the projection of those triangles the same way as all the other triangles which have a static texture. The plugin's not creating and texturing a new polygon and placing it within a pre-existing 3D cockpit.

--- 

```cpp
#include "XPLMPlugin.h"
#include "XPLMDisplay.h"
#include "XPLMGraphics.h"
#include "XPLMProcessing.h"
#include "XPLMDataAccess.h"
#include "XPLMMenus.h"

#include <string.h>
#include <string>

#if IBM
#define WIN32_LEAN_AND_MEAN
    #include <windows.h>
#endif
#if LIN
    #include <GL/gl.h>
#elif __GNUC__
    #include <OpenGL/gl.h>
#else
    #include <GL/gl.h>
#endif
	#ifndef XPLM301
    #error This is made to be compiled against the XPLM300 SDK
#endif
//------------------------------------------------------------------------------------

    XPLMDrawCallback_f fp_draw_panel;

//------------------------------------------------------------------------------------

int draw_panel(XPLMDrawingPhase phase, int before, void *userdata)
{
    // enable alpha blending but nothing else - YMMV
    XPLMSetGraphicsState(0, 0, 0, 0, 1, 0, 0);
    
    // set up to draw some orange lines
    glColor4f(1.0f, 1.0f, 1.0f, 1.0f);
    glLineWidth(1.8f);
    
    glBegin(GL_LINES);
    
    GLfloat roll = 0.0;
    GLfloat pitch = 0.0;
    GLfloat WIDTH = 360;
    GLfloat HEIGHT = 250;
    
    // draw a vertical line positioned according to roll angle
    glVertex2f((1 + roll / 60.f) * WIDTH / 2, 0);
    glVertex2f((1 + roll / 60.f) * WIDTH / 2, HEIGHT);
    
    // draw a horizonal line positioned according to pitch angle
    glVertex2f(0, (1 + pitch / 30.f) * HEIGHT / 2);
    glVertex2f(WIDTH, (1 + pitch / 30.f) * HEIGHT / 2);
    
    // finish drawing lines
    glEnd();

    return 1; // Allow XPL to draw afterwards
}

PLUGIN_API int XPluginStart(char *outName,char *outSig,char *outDesc)
{
    fp_draw_panel = draw_panel;
    XPLMRegisterDrawCallback(fp_draw_panel,xplm_Phase_Gauges,0,0);
}

PLUGIN_API void XPluginStop(void)
{
    XPLMUnregisterDrawCallback(fp_draw_panel,xplm_Phase_Gauges,0,0);
}
```