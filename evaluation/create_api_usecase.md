## Create An API
### Primary Actor: Advanced User

### Trigger
The user wants to use a specific, not included API to solve a given task using a given function.

### Preconditions
1. The user is logged in.
2. There is at least one (built-in or previously created via the Developer Suite) function available for the user's account.

### Main success scenarion
1. The user clicks on "Switch to Developer Suite".
2. The user confirms the warning pop-up to enter the advanced section Developer Suite.
3. The user switches to the second tab "Manage APIs".
4. The user switches to the second sub-tab "Create a new API".
5. The user unlocks the protection mode to enter the edit mode.
6. The user specifies general information about the API, information about the connection and additional parameter, required for the API to work. This step is highly dependent on several circumstances which are based on the selected API and its capabilities or requirements in terms of required request and response.
7. The user saves and finishes the API creation process.

### Evaluation
#### Task
You, as an advanced user, want to add your favourite finance API for receiving current stock prices, **Yahoo Finance**, to your account.
Create a new API based on the following information:

Map the new API to the function `stock_price_by_name`. The name is `My Evaluation Yahoo Finance API` and its priority is `High`. As we want to use the API, it should be active. The URL which belongs to the API is `https://apidojo-yahoo-finance-v1.p.rapidapi.com/stock/get-detail`. The mandatory header data will be provided for your evaluation task separately.
To tell the API what request need to be sent, you provide the following outgoing request parameter template (JSON template):
```
"lang": "en",
"region": "US",
"symbol": "§1§"
```
The langauge is english by default. We target the US market. The symbol might looks strange to you, nothing like a real stock price symbol (shortcut for a public listed company). This is where the interplay with the previously skipped placeholder takes place. Numbers, covered in two paragraph signs (**§**) are tagged as placeholders. As we want to provide a company name instead of a company symbol (or do you know ad-hoc, what the company symbol of Microsoft is?), we want to apply another function call at exactly this place, to tranform the given company name into a symbol and put it in the right place: §1§.
Therefore, add a new placeholder and click on the checkbox, so that it states that a function is applied at the placeholder. Select `retrieve_company_symbol` as the corresponding function. Furthermore, we now need a function field, which shows the new function what to translate:
`Name: "search_name", Value: §company_name§`
We replace it as a string, so click on this checkbox.

Last, add some more information to make the API run: The response path to result is `price.regularMarketOpen.raw`. It just follows the response node by node to extract the interesting information: The stock price.
The request method is `GET`.

Save the created API to complete this use-case. Feel free to try it out!
*And P.S.: The company symbol for Microsoft is MSF.*

#### Control criteria
A new API is created correctly at the users account. All information are saved and the API is able to connect correctly to the given URL, authenticated through the RapidAPI headers. The response is extracted correctly, too. This is ensured by the correct usage of the outgoing request parameter template, combined with the response path to result. It is important to understand the concept of placeholders and use them correctly in the API creation.

The saved API will look like this in the database after a succesful save (in JSON form):
```
{
   "url_api_detail":"http://127.0.0.1:8001/services/manage_apis/72a42931-fad6-4e17-ad5f-2c0d9cdf2a0e/",
   "id":"72a42931-fad6-4e17-ad5f-2c0d9cdf2a0e",
   "function_name":"stock_price_by_name",
   "name":"Yahoo Finance (stock/get-detail by Name)",
   "url":"https://apidojo-yahoo-finance-v1.p.rapidapi.com/stock/get-detail",
   "header":{
      "x-rapidapi-key": FOR SECURITY REASONS THE KEY IS SENT VIA PRIVATE CHAT,
      "x-rapidapi-host":"apidojo-yahoo-finance-v1.p.rapidapi.com"
   },
   "request_params_template":{
      " lang":"en",
      "region":"US",
      "symbol":"§1§"
   },
   "request_body_template":{

   },
   "response_result_path":"price.regularMarketOpen.raw",
   "request_method":"GET",
   "priority":1,
   "enabled":true,
   "placeholders":[
      {
         "id":1,
         "value":{
            "function_name":"retrieve_company_symbol",
            "apply_function":true,
            "function_fields":[
               {
                  "name":"search_name",
                  "value":"§company_name§"
               }
            ]
         },
         "replace_as_string":true
      }
   ]
}
```
