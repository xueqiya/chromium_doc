## chromium

#### ChromeLauncherActivity.
启动`ChromeTabbedActivity`

#### ChromeTabbedActivity
```
ChromeTabbedActivity extends ChromeActivity
ChromeActivity extends AsyncInitializationActivity

AsyncInitializationActivity onCreate()   ---->
ChromeBrowserInitializer.getInstance(this).handlePreNativeStartup(this);

ChromeBrowserInitializer handlePreNativeStartup() ---->

AsyncInitializationActivity setContentViewAndLoadLibrary() --->
mNativeInitializationController.startBackgroundTasks();

NativeInitializationController startBackgroundTasks(), new Thread() ---> 

libraryLoader.ensureInitialized()
catch ProcessInitException, if catch it, throw new RuntimeException(e), browser re-start().

LibraryLoader ensureInitialized() throws ProcessInitException--->

LibraryLoader loadAlreadyLocked() ---> System.load("libchrome_public.so");
```