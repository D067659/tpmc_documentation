## Create An API
### Primary Actor: Advanced User
### Trigger
The user wants to use a specific, not included API to solve a given task using a given function.
### Preconditions
1. There are (built-in or previously created via the Developer Suite) functions available for the user's account.

### Main success scenarion
1. The user clicks on "Switch to Developer Suite".
2. The user confirms the warning pop-up to enter the advanced section Developer Suite.
3. The user switches to the second tab "Manage APIs".
4. The user switches to the second sub-tab "Create a new API".
5. The user unlocks the protection mode to enter the edit mode.
6. The user specifies general information about the API, information about the connection and additional parameter, required for the API to work.
7. The user saves and finishes the API creation process.

### Evaluation
#### Task
You, as an advanced user, want to add your favourite finance API: **Yahoo Finance** to your account.
Create a new API based on the following information:

Map the new API to the function ´stock_price_by_name´




#### Control criteria
A routine is saved correctly at the users account. The first component is a timer condition. The second component is the Finance operation "Stockprice by Company Name" with "Apple" in the field "Company Name" and a result variable starting with a "$". The third component is a messaging operation "Send Email" with a valid email address and an arbitrary subject and message text with at least on containing the stock price result variable starting with a "$".
The routine JSON of one possible solution would look like this:
