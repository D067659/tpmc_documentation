## Delete A Function
### Primary Actor: Advanced User

### Trigger
The user wants to delete a wrongly created or obsolete Function.

### Preconditions
1. The user is logged in.
2. There is at least one (built-in or previously created via the Developer Suite) function available for the user's account.

### Main success scenarion
1. The user clicks on "Switch to Developer Suite".
2. The user confirms the warning pop-up to enter the advanced section Developer Suite.
3. The user switches to the second tab "Manage Functions".
4. The user switches to the second sub-tab "Edit existing Functions".
5. The user selects a target Function by clicking on "Show Details".
6. The user unlocks the protection mode to enter the edit mode.
7. The user clicks on "Delete Function".
8. The user confirms the warning pop-up to delete the selected Function irrevocably.

### Evaluation
#### Task
You, as an advanced user, notices that the function has two times the same field and want to delete it.
Delete an existing Function based on the following information:

In the list, select the Function `Typo - Delete me`, click on "Show Detail" and switch to edit mode.
Notice, that there are two times the same field.
Delete the Function by clicking on "Delete Function" and confirm the warning pop-up.

#### Control criteria
The Function is removed from the users account in the database. Thus, it should be not discoverable anymore when searching the available Functions list in "Manage Functions".
