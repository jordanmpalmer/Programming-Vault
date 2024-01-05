[![](https://developer.x-plane.com/wp-content/uploads/2017/06/menu-sample-plugin-preview.png)](https://developer.x-plane.com/wp-content/uploads/2017/06/menu-sample-plugin-preview.png)

A basic demonstration for adding submenus to the in-sim Plugin menu.

- Download as a [project for Xcode 14 or newer (64-bit Intel)](https://developer.x-plane.com/sdk/sample/?sdk_template=xcode_140&sdk_post_id=7775)
- Download as a [project for Microsoft Visual Studio 2017 (64-bit; requires Windows 8.1 SDK)](https://developer.x-plane.com/sdk/sample/?sdk_template=msvc_2017&sdk_post_id=7775)
- Download as a [project for GCC 4.x/Linux (64-bit)](https://developer.x-plane.com/sdk/sample/?sdk_template=gcc_4&sdk_post_id=7775)

```cpp
#include "XPLMMenus.h"
#include <string.h>
#if IBM
	#include <windows.h>
#endif

#ifndef XPLM300
	#error This is made to be compiled against the XPLM300 SDK
#endif

int g_menu_container_idx; // The index of our menu item in the Plugins menu
XPLMMenuID g_menu_id; // The menu container we'll append all our menu items to
void menu_handler(void *, void *);

PLUGIN_API int XPluginStart(
						char *		outName,
						char *		outSig,
						char *		outDesc)
{
	strcpy(outName, "MenuPlugin");
	strcpy(outSig, "xpsdk.examples.menuplugin");
	strcpy(outDesc, "A sample plug-in that demonstrates and exercises the X-Plane menu API.");

	g_menu_container_idx = XPLMAppendMenuItem(XPLMFindPluginsMenu(), "Sample Menu", 0, 0);
	g_menu_id = XPLMCreateMenu("Sample Menu", XPLMFindPluginsMenu(), g_menu_container_idx, menu_handler, NULL);
	XPLMAppendMenuItem(g_menu_id, "Toggle Settings", (void *)"Menu Item 1", 1);
	XPLMAppendMenuSeparator(g_menu_id);
	XPLMAppendMenuItem(g_menu_id, "Toggle Shortcuts", (void *)"Menu Item 2", 1);
	XPLMAppendMenuItemWithCommand(g_menu_id, "Toggle Flight Configuration (Command-Based)", XPLMFindCommand("sim/operation/toggle_flight_config"));
	
	// Changed your mind? You can destroy the submenu you created with XPLMDestroyMenu(),
	// then remove the "Sample Menu" item from the "Plugins" menu with XPLMRemoveMenuItem().
	//XPLMDestroyMenu(g_menu_id);
	//XPLMRemoveMenuItem(XPLMFindPluginsMenu(), g_menu_container_idx);

	XPLMMenuID aircraft_menu = XPLMFindAircraftMenu();
	if(aircraft_menu) // This will be NULL unless this plugin was loaded with an aircraft (i.e., it was located in the current aircraft's "plugins" subdirectory)
	{
		XPLMAppendMenuItemWithCommand(aircraft_menu, "Toggle Settings (Command-Based)", XPLMFindCommand("sim/operation/toggle_settings_window"));
	}
	
	return 1;
}

PLUGIN_API void	XPluginStop(void)
{
	// Since we created this menu, we'll be good citizens and clean it up as well
	XPLMDestroyMenu(g_menu_id);
	// If we were able to add a command to the aircraft menu, it will be automatically removed for us when we're unloaded
}

PLUGIN_API void XPluginDisable(void) { }
PLUGIN_API int XPluginEnable(void) { return 1; }
PLUGIN_API void XPluginReceiveMessage(XPLMPluginID inFrom, int inMsg, void * inParam) { }

void menu_handler(void * in_menu_ref, void * in_item_ref)
{
	if(!strcmp((const char *)in_item_ref, "Menu Item 1"))
	{
		XPLMCommandOnce(XPLMFindCommand("sim/operation/toggle_settings_window"));
	}
	else if(!strcmp((const char *)in_item_ref, "Menu Item 2"))
	{
		XPLMCommandOnce(XPLMFindCommand("sim/operation/toggle_key_shortcuts_window"));
	}
}
```