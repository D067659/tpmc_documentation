# Use Case
## Create Routine
### Primary Actor: Standard User
### Trigger
The user wants to create a new Routine and logs into to platform.
### Preconditions
1. The Advanced User added one or more Functions and one or more APIs to the platform.
2. The user has an user account.

### Main success scenarion
1. The user clicks on "Add routine".
2. The user enters a routine name and confirms.
3. The user clicks on "Add a new condition".
4. The user selects the "timer" category and the "routine start time" condition.
5. The user specifies a start time, the weekdays and timezone.
6. The user adds a combination of custom conditions and operations.
7. The user confirms and finishes the routine creation process.

### Evaluation
#### Task
Create a routine that holds the following components:
1. Timer condition
2. A stock price search of "Apple"
3. An email containing the stockprice search result, a subject and a recipient

#### Control criteria
A routine is saved correctly at the right user account. The first component is a timer condition. The second component is the Finance operation "Stockprice by Company Name" with "Apple" in the field "Company Name" and a result variable starting with a "$". The third component is a messaging operation "Send Email" with a valid email address and an arbitrary subject and message text with at least on containing the stock price result variable starting with a "$".

#### Evaluation criteria
The test subject stops the time from starting to the successful saving of the routine.
