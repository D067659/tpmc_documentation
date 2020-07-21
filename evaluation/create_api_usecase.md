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
You, as an advanced user, want to add your favourite finance API for receiving current stock prices: **Yahoo Finance** to your account.
Create a new API based on the following information:

Map the new API to the function `stock_price_by_name`. Name it `My Evaluation Yahoo Finance API`. Set the priority to `High`. We skip the placeholder option for now. But do not forget to activate the API via the checkbox. The URL which belongs to the API is `https://apidojo-yahoo-finance-v1.p.rapidapi.com/stock/get-detail`. As a header, you need to specify an [RapidAPI](https://rapidapi.com/) key and value pair. If you have an account, use the provided credentials. Else, you can use this as the required header data:
```
x-rapidapi-key: 3f72aac6ecmsh8671cd5f7cf96c3p1f8535jsn0adae9475a6d
x-rapidapi-host: apidojo-yahoo-finance-v1.p.rapidapi.com
```
To tell the API what request need to be sent, you provide the following outgoing request JSON template:
```
"lang": "en",
"region": "US",
"symbol": "§1§"
```
The langauge is english by default. We target the US market. The symbol might looks strange to you, nothing like a real stock price symbol (shortcut for a public listed company). This is where the interplay with the previously skipped placeholder takes place. Numbers, covered in two paragraph signs (**§**) are tagged as placeholders. As we want to provide a company name instead of a company symbol (or do you know ad-hoc, what the company symbol of Microsoft is?), we want to apply another function call at exactly this place, to tranform the given company name into a symbol and put it in the right place: §1§.
Therefore, add a new placeholder and keep the suggested ID. Click on the checkbox, so that it states that a function is applied at the placeholder. Select `retrieve_company_symbol` as the applied function. Furthermore, we now need a function field, which shows the new function what to translate:
`Name: "search_name", Value: §company_name*`
We replace it as a string, so click on this checkbox.

Last, add some more information to make the API run: The response path to result is `price.regularMarketOpen.raw`. It just follows the response node by node to extract the interesting information: The stock price.
The request method is `GET`.

Save the created API to complete this use-case. Feel free to try it out!
*And P.S.: The company symbol for Microsoft is MSF.*

#### Control criteria

