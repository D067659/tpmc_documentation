## Create Routine
### Primary Actor: Standard User
### Trigger
The user wants to create a new Routine.
### Preconditions
1. The user is logged in. 
2. The user added one or more Functions and one or more APIs to the platform.
3. The user has an user account.

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

![Timer Condition Screenshot](../resources/images/Timer_condition.png)
2. A stock price search of "Apple"

![Stock Price Operation](../resources/images/Stockprice_by_name_operation.png)
3. An email containing the stockprice search result, a subject and a recipient

![Email Operation](../resources/images/Email_operation.png)

#### Further Tasks
Navigate to the next task ([Create Function](create_function_usecase.md)).

#### Control criteria for current task
<details>
<summary>Show solution</summary>
<p>
A routine is saved correctly at the users account. The first component is a timer condition. The second component is the Finance operation "Stockprice by Company Name" with "Apple" in the field "Company Name" and a result variable starting with a "$". The third component is a messaging operation "Send Email" with a valid email address and an arbitrary subject and message text with at least on containing the stock price result variable starting with a "$".
The routine JSON of one possible solution would look like this:

<pre>
{
    "url": "http://master.gsq.ro/api/routine/<routine_id>/",
    "id": "<routine_id>",
    "components": [
        {
            "category_name": "timer",
            "function_name": "start_at",
            "type": "condition",
            "parameters": {
                "weekday": [
                    "monday"
                ],
                "timezone": "Europe/Berlin",
                "time": "12:00"
            }
        },
        {
            "category_name": "Finance",
            "function_name": "stock_price_by_name",
            "type": "operation",
            "parameters": {
                "company_name": "Apple"
            },
            "result": "$"
        },
        {
            "category_name": "Messaging",
            "function_name": "send_email",
            "type": "operation",
            "parameters": {
                "recipient": "test@email.com",
                "subject": "Current Apple Stock price",
                "content": "This is the current stock price of Apple: $result_variable"
            },
            "result": "$"
        }
    ],
    "routine_name": "test",
    "state": "standby",
    "created_at": "2020-08-<date>T<hh>:<mm>:<sec,ns>Z",
    "updated_at": "2020-08-<date>T<hh>:<mm>:<sec,ns>Z"
}
</pre></p>
</details>
