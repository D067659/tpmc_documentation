# User Guide

## Register User
Before you can start with the MTPP you have to register your own user.

*Click on the Button "You're new to MTPP? Sign Up!".*

![Login Page](../resources/images/login_page.png)

*Enter a username and your password. Confirm by clicking "Sign UP".*

![Register Page](../resources/images/register_page.png)

Now your account is created successfully. You can add our bunlde of predefined Functions and APIs to your account, if you acquired the activation code.

*Check "Add bundles to my account (activation code required)" and enter the activation code in the field. To confirm click "Load built-in features!"*

*After the bundle was added to your account go to the Control page by clicking "Go to Control Page!"*

## Create a routine

After a user account was created and some Functions and APIs were load to the account either manually or by using the predefined bundle, routines can be created by the user.

*On the Control Page click on "Routine Page" or "+ Add routine" to create a new routine.*

![Control Page](../resources/images/control_page.png)

*Enter the routine name and confirm with "Next".*

![Routine Name Page](../resources/images/routine_name.png)

On the appearing page you can add and edit the routines components.

![Add Components Page](../resources/images/create_routine1.png)

A Routine behaves like a workflow in which each Component is treated sequentially. A Routine can consists of the components Conditions and Operations. Both are structured in categories where similar types of Components are clustered. Conditions have to be fullfilled for the Routine to continue. If the condition is not fullfilled the Routine will interrupt. One very important Condition is the timer Condition "Routine start time" which checks checks if the current time equals the specified time to start the Routine. An Operation can be anykind of action or request to an external service. Each operation can have different input fields and one return value.

Let's start with adding the timer Condition "Routine start time".

*Click on the "Add a new Condition" button. Then select the timer category in the appearing dropdown menu. Finally select the "Routine start time" Condition in the second dropdown.*

![Timer – Routine start time Condition](../resources/images/create_routine2.png)

*Specify a time, day and timezone at which the Routine should start.*

Next we can choose an Operation we want to happend next.

*Click on the "Add a new operation" button.*

You can now choose any category you like to add the corresponding operations. We want to retrieve the stock price of Google.

*Select "Finance" in the dropdown and select "Retrieve Stock Price of a Company".*

![Finance – Retrieve Stock Price Operation](../resources/images/create_routine3.png)

In Operations the results can be saved in variables so they can be used in other components. The result variable name is specified in the "Result variable name" field. The variable name has to start with a Dollar sign "$" to indicate that it is an variable and can be used in other Component's fields. In string fields we can just write the variable name including the Dollar sign. In number fields we have to change the field to variable mode by clicking the variable button and then selecting the variable name. The variable mode can be left by clicking the variable button again.

*Enter "Google" in the Company Name field and "$stockprice" in the "Result variable name" field.*

![Finance – Retrieve Stock Price Operation](../resources/images/create_routine4.png)

Next we want to compare the stockprice to the fixed number 445 with the Numbers Condition "Relation between two numbers".

*Click on the "Add a new Condition" button. Then select the Numbers category in the appearing dropdown menu. Finally select the "Relations between numbers" Condition in the second dropdown.*

This condition has three fields, the two values to compare and the operator. Remember that the value fields are normally numbers only fields.

*Click the "Variable buttong below the "Left Value" field and select the "$stockprice" variable. Select "Greater than" in the Operator dropdown. Finally type "445" in the "Right Value" field.*

![Numbers – Relation between numbers](../resources/images/create_routine5.png)

If the Condition is satisfied i.e. the Apple stock price is greater than 445, the Routine will continue with the following components. If not, it would stop.

We now specified our first Routine with three Components. It would make sense to continue the Routine with additional Operations after the Condition was successful but for now this is enough to show you all functionalities.

Before starting the Routine we want to save it so our account.

*Click the "Next" button on the bottom right corner.*

![Finished Routine creation](../resources/images/create_routine6.png)

*Finally click the "Finish" button.*

The Routine is now visible in the Control Page. If not visible immediately, try refreshing the page.

![Control Page – Finished Routine creation](../resources/images/create_routine7.png)

## Manage Routines

When a Routine is created it is automatically started when the timer condition is satistfied.

On the Control Page the existing Routines can be stated manually by clicking the play button, edited by clicking the pencil button or deleted by clicking the bin button.

![Control Page](../resources/images/create_routine7.png)

## Devloper Suite

The Developer Suite allows to create, edit and delete Functions i.e. Operations or APIs.

*Click the "Switch to Developer Suite" button.*

![Control Page](../resources/images/control_page.png)

### Functions

#### Create

#### Edit

#### Delete

### APIs

#### Create

#### Edit

#### Delete
