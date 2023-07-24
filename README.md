# of-start-toggl-timer
Start a toggl timer from within OmniFocus.

## setup/installation
To add this automation to OmniFocus download this repository as a zip file or clone it. Then right click on the file start-timer.omnifocusjs and open in OmniFocus.

First, you will need to add your Toggl API key at the beginning of the file. To open the file, go to the Automation menu in OmniFocus, click Configure, select this automation, click Reveal in Finder, and open the file in any text editor. You should be able to find your API key at https://track.toggl.com/profile. Paste in into the api_key variable to allow the automation to start timers in your Toggl workspace.

## features/how to use
To run the automation, select a task then go to the Automation menu and press Start Toggl Timer. (Or you can set a keyboard shortcut for it) If the project exists already in your Toggl workspace, then that will be the project used for your timer. The title of the OmniFocus task will be used the Toggl timer's description. Finally, any tags on the task that start with the text "Toggl" will be added to the Toggl timer without the "Toggl" text. Like the project, if any of those tags do not exist in your Toggl workspace, they will be automatically created.

https://user-images.githubusercontent.com/62226606/117526904-ee139b80-af7c-11eb-9794-1ef7a539407a.mov

### colon in project name
If it doesn't already exist, it will be automatically created. If you have a project in OmniFocus that is part of a larger project and if you want your Toggl timer to use that, you can put the name of your Toggl project before the OmniFocus project name with a ":". For example, running the Start Toggl Timer automation on a task within an OmniFocus project called "Coding: OmniFocus Toggl Automation" will start a Toggl timer with the project "Coding." Then, any text after the ":" (in this case "OmniFocus Toggl Automation") will be put at the start of the Toggl timer's description, again followed by a colon.

https://user-images.githubusercontent.com/62226606/117527010-8873df00-af7d-11eb-8939-fa20396968f7.mov

### running automation on project
You can now run the Start Toggl Timer automation on an OmniFocus project instead of a task. It will work the same as running the automation on a task, but your Toggl timer, won't have a description.
