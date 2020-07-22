## Edit An API
### Primary Actor: Advanced User

### Trigger
The user wants to change an existing API which is linked to a given function.

### Preconditions
1. There is at least one (built-in or previously created via the Developer Suite) function available for the user's account.
2. There is at least one (built-in or previously created via the Developer Suite) API to be editted available for the user's account.

### Main success scenarion
1. The user clicks on "Switch to Developer Suite".
2. The user confirms the warning pop-up to enter the advanced section Developer Suite.
3. The user switches to the second tab "Manage APIs".
4. The user switches to the second sub-tab "Edit existing APIs".
5. The user selects a target API by clicking on "Show Details".
6. The user unlocks the protection mode to enter the edit mode.
7. The user applies changes as far as fields are not protected.
8. The user saves and finishes the API edit process.

### Evaluation
#### Task
You, as an advanced user, are not satisfied with some information of the corona API for receiving latest news of corona confirmed cases.
Edit an existing API based on the following information:

In the list, select the API `COVID-19 data`, click on "Show Detail" and switch to edit mode.
Some fields are not changeable in order to prevent errors by changing mandatory information which should be kept reliable consistent.
Change the name of the API to `COVID-19 Cases` and set the priorty to `Medium`.

Save the editted API to complete this use-case. Feel free to try it out!

#### Control criteria
A editted API is changed correctly at the users account. The name should be different than before and the priority changed (this would only affect the selection of an API for a given function if there are multiple APIs for one function).
The saved API will look like this in the database after a succesful save (in JSON form):
```
{
   "url_api_detail":"http://127.0.0.1:8001/services/manage_apis/a6c661ac-a530-4792-a743-47bb912b5ba4/",
   "id":"a6c661ac-a530-4792-a743-47bb912b5ba4",
   "function_name":"coronavirus_confirmed_cases",
   "name":"COVID-19 Cases",
   "url":"https://covid-19-data.p.rapidapi.com/country",
   "header":{
      "x-rapidapi-key": FOR SECURITY REASONS THE KEY IS NOT VISIBLE HERE,
      "x-rapidapi-host":"covid-19-data.p.rapidapi.com"
   },
   "request_params_template":{
      "name":"§1§",
      "format":"json"
   },
   "request_body_template":{

   },
   "response_result_path":"[0].confirmed",
   "request_method":"GET",
   "priority":2,
   "enabled":true,
   "placeholders":[
      {
         "id":1,
         "value":{
            "field":"country_name",
            "apply_function":false
         },
         "replace_as_string":true
      }
   ]
}
```