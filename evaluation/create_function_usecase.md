## Create A Function
### Primary Actor: Advanced User

### Trigger
The user wants to use a specific, not included function to solve a given task.

### Preconditions
1. The user is logged in.

### Main success scenarion
1. The user clicks on "Switch to Developer Suite".
2. The user confirms the warning pop-up to enter the advanced section Developer Suite.
3. The user switches to the second tab "Manage Functions".
4. The user switches to the second sub-tab "Create a new Function".
5. The user unlocks the protection mode to enter the edit mode.
6. The user specifies general information about the function, information about the result of the function and specifies fields.
7. The user saves and finishes the Function creation process.

### Postconditions
1. The Function is added to the database.
2. APIs can be added for the which allows the user to use the Function in the Routine building process.

### Evaluation
#### Task
You, as an advanced user, want to add a function for retrieving the temperature of your Smart Home thermometer to your account.
The new function should have the following characteristics:

Function name: `thermometer_temperature`
Category: `IoT`
Function label: `Thermometer Temperature`

The result is a floating point number.
The result should be labeled `Temperature`and have the help text `This result contains the temperature of the choosen thermometer.`.

The function should have the following fields:
1. A field should specify the exact thermometer by its number with the label `Thermometer Number` and the helptext `Specify the thermometer by its numeric id.`.
2. A field should specify whether the temperature should be in Celcius or Fahrenheit by a boolean value with the label `Temperature Unit` and the helptext `Specify the unit of the temperature. Checked means Celcius and Unchecked means Fahrenheit.`.

#### Control criteria
A new Function is created correctly at the users account.

The saved Function will look like this in the database after a succesful save (in JSON form):
```
{
           "category": "IoT",
           "function_name": "thermometer_temperature",
           "function_label": "Thermometer Temperature",
           "result": {
               "name": "Temperature",
               "type": "number",
               "label": "This result contains the temperature of the choosen thermometer."
           },
           "fields": [
               {
                   "name": "1",
                   "type": "number",
                   "label": "Thermometer Number",
                   "required": true,
                   "help_text": "Specify the thermometer by its numeric id."
               },
               {
                   "name": "2",
                   "type": "text",
                   "label": "Temperature Unit",
                   "required": true,
                   "help_text": "Specify the unit of the temperature. Checked means Celcius and Unchecked means Fahrenheit."
               }
           ]
       }
```

#### Further Tasks
Navigate to the next task ([Edit Function](edit_function_usecase.md)) or go back to the previous task ([Create Routine](create_routine_usecase.md)).
