## Edit An API
### Primary Actor: Advanced User

### Trigger
The user wants to change an existing API which is linked to a given function.

### Preconditions
1. The user is logged in.
2. There is at least one (built-in or previously created via the Developer Suite) function available for the user's account.
3. There is at least one (built-in or previously created via the Developer Suite) API to be edited available for the user's account.

### Postconditions
1. Some values of the API are changed.

### Main success scenarion
1. The user clicks on "Switch to Developer Suite".
2. The user confirms the warning pop-up to enter the advanced section Developer Suite.
3. The user switches to the second tab "Manage APIs".
4. The user switches to the second sub-tab "Edit existing APIs".
5. The user selects a target API by clicking on "Show Details".
6. The user unlocks the protection mode to enter the edit mode.
7. The user applies desired changes.
8. The user saves and finishes the API edit process.

### Evaluation
#### Task
You, as an advanced user, are not satisfied with the name of the corona API for receiving latest news of corona confirmed cases.
Edit an existing API based on the following information:

In the list, select the API `COVID-19 data`, click on "Show Detail" and switch to edit mode.
Change the name of the API to `COVID-19 Cases`.

Save the edited API to complete this use-case. Feel free to try it out!

#### Further Tasks
Navigate to the next task ([Delete API](delete_api_usecase.md)) or go back to the previous task ([Create API](create_api_usecase.md)).

#### Control criteria for current task
<details>
<summary>Show solution</summary>
<p>
A edited API is changed correctly at the users account. The saved API will look like this in the database after a succesful save (in JSON form):
   
<pre>
{
   "url_api_detail":"&lt;URL_To_Access_API&gt;",
   "id":"&lt;INTERNAL_ID_OF_API&gt;",
   "function_name":"coronavirus_confirmed_cases",
   "name":"COVID-19 Cases",
   "url":"https://covid-19-data.p.rapidapi.com/country",
   "header":{
      "x-rapidapi-key": &lt;FOR SECURITY REASONS THE KEY IS NOT VISIBLE HERE&gt;,
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
   "priority":3,
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
</pre></p>
</details>
