# of-start-toggl-timer v2
Start a toggl timer from within OmniFocus.

## What's New in v2

- **Toggle Functionality:** Run the automation once to start a timer, run it again on the same task to stop it
- **API v9 Compliance:** Updated to use Toggl Track API v9 (v8 is being deprecated)
- **Automatic Project Activation:** Archived projects are automatically reactivated when needed
- **User Feedback:** Clear alerts showing whether timer started or stopped
- **Improved Error Handling:** Better validation and error messages

## setup/installation

To add this automation to OmniFocus download this repository as a zip file or clone it. Then right click on the file start-timer.omnifocusjs and open in OmniFocus.

First, you will need to add your Toggl API key at the beginning of the file. To open the file, go to the Automation menu in OmniFocus, click Configure, select this automation, click Reveal in Finder, and open the file in any text editor. You should be able to find your API key at https://track.toggl.com/profile. Paste it into the api_key variable to allow the automation to start timers in your Toggl workspace.

## features/how to use

To run the automation, select a task then go to the Automation menu and press Start Toggl Timer. (Or you can set a keyboard shortcut for it) If the project exists already in your Toggl workspace, then that will be the project used for your timer. The title of the OmniFocus task will be used as the Toggl timer's description. Finally, any tags on the task that start with the text "Toggl" will be added to the Toggl timer without the "Toggl" text. Like the project, if any of those tags do not exist in your Toggl workspace, they will be automatically created.

### NEW: toggle timer on/off

**v2 feature:** Running the automation on a task that already has a running timer will stop that timer instead of creating a duplicate. You'll see an alert indicating whether the timer was started or stopped.

- **First run on a task:** Starts a new timer → Shows "Timer Started" alert
- **Second run on same task:** Stops the running timer → Shows "Timer Stopped" alert
- **Run on different task while timer running:** Starts new timer for the new task (previous timer keeps running)

### colon in project name

If it doesn't already exist, it will be automatically created. If you have a project in OmniFocus that is part of a larger project and if you want your Toggl timer to use that, you can put the name of your Toggl project before the OmniFocus project name with a ":". For example, running the Start Toggl Timer automation on a task within an OmniFocus project called "Coding: OmniFocus Toggl Automation" will start a Toggl timer with the project "Coding." Then, any text after the ":" (in this case "OmniFocus Toggl Automation") will be put at the start of the Toggl timer's description, again followed by a colon.

### running automation on project

You can now run the Start Toggl Timer automation on an OmniFocus project instead of a task. It will work the same as running the automation on a task, but your Toggl timer won't have a description unless you use the colon notation in the project name.

## Troubleshooting

### "No workspace found" Error
- Verify your API token is correct
- Check that your Toggl account has at least one workspace
- Ensure you have access to the default workspace

### Timer Not Starting
- Check console logs in OmniFocus (Window > Show Log) for detailed error messages
- Verify project isn't archived (plugin should handle this automatically in v2)
- Confirm you have permission to create time entries in the workspace

### Timer Not Stopping
- Ensure the task description exactly matches the running timer description
- Check if timer is running in Toggl Track web interface
- Timer matching is case-sensitive and exact

### "Project is archived" Error
- v2 automatically reactivates archived projects
- If error persists, manually activate the project in Toggl Track
- Check console logs for activation errors

## Technical Details

This plugin uses **Toggl Track API v9** (updated from v8):

- **Authentication:** Basic Auth with API token
- **Rate Limits:**
  - Free: 30 requests/hour
  - Starter: 240 requests/hour
  - Premium: 600 requests/hour
- **Timestamps:** ISO 8601 / RFC 3339 (UTC)

For detailed technical changes, see `start-timer-CHANGES.md`

## Security Note

⚠️ **Important:** Your API token is stored in plain text in the plugin file. Keep this file secure and never commit it to public repositories. The OmniFocus Plug-Ins folder is automatically encrypted by iCloud.

## Credits

- **Original Author:** Henry Gustafson
- **Repository:** github.com/lizard-heart/omni-focus-start-toggl-timer
- **v2 Updates:** API v9 migration, toggle functionality, archived project handling
