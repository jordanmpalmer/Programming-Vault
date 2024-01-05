## How to make a DataRef and Display in DRE

```cpp
#define MSG_ADD_DATAREF 0x01000000     //  Add dataref to DRE message

// reference for custom dataref
XPLMDataRef Name_of_Dataref = NULL;

// give the custom dataref value 
int Name_of_Value;

// register dataref in dataref editor
float Reg_Name_of_Dataref_InDRE(float  elapsedMe, 
								float  elapsedSim, 
								int    counter, 
								void*  refcon);

// getter for dataref
int Get_Name_of_Dataref(void* inRefcon);
// setter for dataref
void Set_Name_of_Dataref(void* inRefcon, int outValue);


// inside of XPluginStart
// This routine creates a new item of data that can be read and written
// also tie setters/getters so they become XPLMGetData_f/XPLMSetData_f
// You are returned a dataref for the new item of data created.
Name_of_Dataref = XPLMRegisterDataAccessor("Vertex/Name_of_Dataref",
                                             xplmType_Int,                                  // The types we support
                                             1,                                             // Writable
                                             Get_Name_of_Dataref, Set_Name_of_Dataref,      // Integer accessors
                                             NULL, NULL,                                    // Float accessors
                                             NULL, NULL,                                    // Doubles accessors
                                             NULL, NULL,                                    // Int array accessors
                                             NULL, NULL,                                    // Float array accessors
                                             NULL, NULL,                                    // Raw data accessors
                                             NULL, NULL);                                   // Refcons not used

// This routine looks up the actual opaque XPLMDataRef that you use to read and write the data.
// Not necessary if Name_of_Dataref just pulled XPLMRegisterDataAccessor
Name_of_Dataref = XPLMFindDataRef("Vertex/Name_of_Dataref");

// initialize custom dataref
XPLMSetDatai(Name_of_Dataref, 0);

// register callback, this FLCB will register our custom dataref in DRE
XPLMRegisterFlightLoopCallback(Reg_Name_of_Dataref_InDRE, 1, NULL);




//  This single shot FLCB registers our custom dataref in DRE
float Reg_Name_of_Dataref_InDRE(float elapsedMe, float elapsedSim, int counter, void * refcon)
 {
     XPLMPluginID PluginID = XPLMFindPluginBySignature("xplanesdk.examples.DataRefEditor");
     if (PluginID != XPLM_NO_PLUGIN_ID)
     {
          XPLMSendMessageToPlugin(PluginID, MSG_ADD_DATAREF, (void)"BSUB/CounterDataRef");   
     }
     return 0;  // Flight loop is called only once!  

 }


// in XPluginStop(void)
XPLMUnregisterDataAccessor(Name_of_Dataref);
XPLMUnregisterFlightLoopCallback(Reg_Name_of_Dataref_InDRE, NULL);
```

## How to make a CommandRef

```cpp
// reference for custom command 
XPLMCommandRef Name_of_Command = NULL;

// handler
int Name_of_Command_Handler(XPLMCommandRef   inCommand,
						    XPLMCommandPhase  inPhase,
						    void *            inRefcon)


// inside of XPluginStart
// create our commands
Name_of_Command = XPLMCreateCommand("Vertex/Name_of_Command", "Command");

// register our custom commands
XPLMRegisterCommandHandler (Name_of_Command,            // in Command name
                            Name_of_Command_Handler,    // in Handler
                            1,                          // Receive input before plugin windows.
                            (void *) 0);                // inRefcon.


// inside of XPluginStop
XPLMUnregisterCommandHandler(Name_of_Command, Name_of_Command_Handler, 0, 0);
```

## What is a Callback

As discussed above, a callback is a function in the code that the simulator calls to accomplish specific behaviors. For example, when the simulator needs to draw the window, it calls the draw callback you specify when you create the window.

Typically callbacks are specified when an object is created and provide unique behaviors for that object. Providing a callback is similar to overriding a virtual function; it lets you customize an object’s behavior.

## What is a Refcon

All of the APIs that take callbacks also take “reference constants.” These are pointers that will be passed back to the callback when the callback is called. This allows you to specify specific data of some kind with different uses of the callback. A few uses of refcons include:

- For object-oriented programming languages, you can pass a pointer to an object that the callback should work on. When the callback is called, it will know what object to operate on.
- For callbacks that provide behavior to multiple windows, you can provide a pointer to data for that specific window.
- For callbacks that execute commands, you can specify the command using data of our choosing.

You don’t have to use refcons (you can simply pass in NULL); they are provided only for convenience. Generally if the data the callback operates on is global, you won’t need a refcon, but if multiple copies of that data exist per object, you will.

## What is an Opaque Handle

An opaque handle is a value used to identify an object created within the plug-in system. Opaque handles are usually defined as integers or _void_ pointers. The handles are ‘opaque’ because the plugin does not know what they mean or what internal structures they represent. You should never try to do anything with an opaque handle except pass it back to the SDK APIs.

The typical life of an object goes something like this: you first create the object and receive an opaque handle. You then make function calls passing in that handle to manipulate it. Finally, you call a function to free the object, passing the handle in again. From that point on the handle is no longer valid.