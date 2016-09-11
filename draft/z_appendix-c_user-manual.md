# Introduction
This user manual provides instructions on how to use QuantiTeam's mobile app, which serves as a basic showcase of the system's current features and its API in the context of a real application, as well as the web uploader which can be used to attach text files to tasks registered in the mobile app.  

# Registration
## Login
Upon opening the app, the user is greeted with QuantiTeam's login view. If the user has previously created an account in the QuantiTeam blockchain, they can simply enter their username and password, followed by a tap on "Login" to be forwarded to their task overview. If the login fails for any reason, the user is informed via dialog window.

## Signup
If this is the first time using QuantiTeam, the user can tap on "Sign up here" under the login form. This forwards the user to a signup form, requiring details such as a name, email, username, and password. Once the user is happy with their entries, tapping "Sign up" will validate the entered information. If mandatory fields have been left blank, the app will notify the user by marking them red. If the entered username is already taken or the entered passwords don't match, the user will be informed via a dialog window.


# Team
## Creating a Team
Using the navigation bar at the bottom of the app, the user can navigate to the "Team" tab. If the user is currently not part of a team, the main view will contain instructions on how to create a team. By tapping "Create Team" in the app's header, the user is forwarded to a simple text entry where they can enter their chosen team name and tap "Create Team" again.  
After a short delay to a account for the event being registered in a transaction block within the blockchain, the user should receive confirmation whether creating the team succeeded.

When navigating back to the team overview tab, the user should now see their team's details, as well as a list of team members. Instead of "Create Team", the header now contains an "Add Member" action button.

## Adding Members
To add a member to their team, a user can tap "Add Member" in the header of the app's "Team" tab. This forwards the user to a text entry where they are able to enter the username of another QuantiTeam user. After entering a username, the user taps the "Add Member" button and, after a short delay to register the transaction in the blockchain, receives confirmation whether adding the user to their team was successful or not.

When navigating back to the team overview tab, the user should now see the new member in the list of team members.

# Tasks
Using the navigation bar at the bottom of the app, the user can navigate to the "Tasks" tab. By pulling downwards on the screen, the user can refresh the list of existing tasks if any are currently registered to their username in the blockchain.

## Adding a Task
To add a task, the user can tap "Add Task" in the app's header, which forwards the user to a task form. Here the user is able to enter a title and short description of the task they want to register in the QuantiTeam blockchain, as well as set the status of the task as "To Do" or "Completed". After filling in these details, tapping the "Add Task" button attempts to register the new task in the blockchain.

Upon returning to the "Tasks" tab via the back-arrow icon in the app's header, the user can refresh the task list, which should now display the newly registered task.

## Inspecting a Task
When tapping a task in the task list, the user is forwarded to detailed overview of the task. Here the user can inspect the task's details.

## Marking a Task as Complete
Once a task has been completed, the user is able to mark it as complete in the blockchain. This can be done by tapping a specific task in the task list, followed by a tap on the "Mark as complete" button in the detailed task view.


# Profile
## Inspecting the User Profile
Using the navigation bar at the bottom of the app, the user can navigate to the "Me" tab. Here the user is able to see their details, such as their name, username and email address.

## Logging out
To log out of the mobile app, the user can tap on "Log out" in the app's header within the "Me" tab. This resets the app's state and returns the user to the "Login" view.


# Attachments
A user is able to attach text files related to a task via the web uploader.

## Prerequisites
To add a text file attachment to the blockchain, the user requires a task token, which can be copied from a task's "Token" field in the task detail view within the mobile app.

## Attaching Text Files to a Task
When opening a browser window and navigating to the QuantiTeam web server, the user is shown a file attachment form. The user can enter the previously copied task token and select a text (`.txt`) file to attach to the task associated with the token. After tapping submit, the user receives confirmation whether attaching the file was successful. Multiple files can be attached to a single task.
