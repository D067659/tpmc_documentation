## Delete An API
### Primary Actor: Advanced User

### Trigger
The user wants to delete a wrongly created or obsolete API which is linked to a given function.

### Preconditions
1. The user is logged in.
2. There is at least one (built-in or previously created via the Developer Suite) function available for the user's account.
3. There is at least one (built-in or previously created via the Developer Suite) API to be editted available for the user's account.

### Postconditions
1. The API is deleted.

### Main success scenarion
1. The user clicks on "Switch to Developer Suite".
2. The user confirms the warning pop-up to enter the advanced section Developer Suite.
3. The user switches to the second tab "Manage APIs".
4. The user switches to the second sub-tab "Edit existing APIs".
5. The user selects a target API by clicking on "Show Details".
6. The user unlocks the protection mode to enter the edit mode.
7. The user clicks on "Delete API".
8. The user confirms the warning pop-up to delete the selected API irrevocably.

### Evaluation
#### Task
You, as an advanced user, notices that your existing API is too costly to use anymore. Therefore, you want to get rid of it, so that this API is never used for a given function again.
Delete an existing API based on the following information:

In the list, select the API `Way too costly - Delete me`, click on "Show Detail" and switch to edit mode.
Imagine, that this API costs $2 each call and you have to call it twice a hour to monitor e.g. the weather very regularly. Quite costly!
Delete the API by clicking on "Delete API" and confirm the warning pop-up.

Now, this API should not be listed in the Developer Suite anymore. Feel free to try it out!

#### Control criteria
A deleted API is removed from the users account. Thus, it should be not discoverable anymore when searching the available API list in "Manage APIs".
