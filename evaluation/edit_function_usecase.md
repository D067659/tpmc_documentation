## Edit A Function
### Primary Actor: Advanced User

### Trigger
The user wants to change an existing Function.

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
7. The user applies changes as far as fields are not protected.
8. The user saves and finishes the Function edit process.

### Evaluation
#### Task
You, as an advanced user, are not satisfied with the Function label of the `Thermometer Temperature` Function.
Chnage the label to `Thermometer Temperature Home`.

#### Control criteria
The edit is saved correctly at the users account. The label should be different than before.
The saved Function will look like this in the database after a succesful save (in JSON form):
```
{
           "category": "IoT",
           "function_name": "thermometer_temperature",
           "function_label": "Thermometer Temperature Home",
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
