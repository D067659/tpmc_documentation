# Service Provider and its underlying Mapping Concept

In short, the Service Provider is the microservice of the MTP platform which is responsible for executing functions using external APIs. Its responsibility is cleary decoupled from the Master Service which orchestrates the execution of a user routine (docu see [Master Service Documentation](master_service.md)). In the context of the overall microservice architecture of our prototype, the Service Provider is accessed solely via the Master Service, mainly to execute functions, which may have some input and may return some result. Besides retrieving all existing functions and APIs, the Service Provider allows to create, update or delete functions during runtime.

Since the Service Provider is so to say the implementation of our overall mapping concept for dynamic adding (i.e. during runtime) of new functions and external APIs to execute those functions. Therefore, in order to be able to understand the architecture of the Service Provider it is indispensable to understand the underlying Mapping Concept, i.e. the way how functions and APIs are represented, first. Thus, this documentation at first explains in detail how the underlying Mapping Concept works and afterwards focusses on the technical implementation of this concept by the Service Provider.

The structure of this documentation is as follows: 
* [Underlying Mapping Concept](#underlying-mapping-concept)
* [Technical Implementation](#technical-implementation)
  * [Endpoints](#endpoints)
  * [Flow of Function Execution](#flow-of-function-execution)
* [Appendix 1 - Exemplary Fixtures of Function and API Specifications](#appendix-1---exemplary-fixtures-of-function-and-api-specifications)
* [Appendix 2 - Source Code for main part of Function Execution](#appendix-2---source-code-for-main-part-of-function-execution) 

## Underlying Mapping Concept
The motivation for the following concept stems from one of the core goals of this project, namely to enable the "execution of service compositions independent of the available API-endpoints". Whereas the flexible composition of services is the responsibility of the Master Service, the latter part, i.e. the execution of individual services, which are represented by **functions** in terms of the Mapping Concept, is the responsibility of the Service Provider. Thereby, on the one hand, we wanted to be able to add new functions which then can be used as part of a routine. On the other hand, we wanted to be able to add new external providers, which are represented by **APIs** in terms of the Mapping Concept, which offer the functionality to execute the function by accessing certain endpoints of a external API. Both should be possible <u>dynamically during runtime</u>. 

In order to understand the concept we developed to achieved these goals, it is easiest to take a look at the two data models of a function and an API one by one.

### Data model - Functions
<table>
   <tr>
      <th>Attribute</th>
      <th>Type</th>
      <th>Description</th>
  </tr>
   <tr>
      <td>user</td>
      <td>id</td>
      <td>Some id representing the user this function belongs to.</td>
   </tr>
   <tr>
      <td>category</td>
      <td>String</td>
      <td>Area this function fits into. Has only a descriptive meaning and is used to structure the available functions by categories in the UI.</td>
   </tr>
   <tr>
      <td>function_name</td>
      <td>String</td>
      <td>Internal name of the function. It is displayed in the developer view of the UI and must be unique under the functions of a given user.</td>
   </tr>
   <tr>
      <td>function_label</td>
      <td>String</td>
      <td>Label for this function which is displayed in the UI when searching for available functions while creating or editing a routine.</td>
   </tr>
   <tr>
      <td>result</td>
      <td>Object</td>
      <td>
         An object representing the result of this function if it has one. If a function has no result, this is respresented by an empty object. 
	 The attributes of a non empty <b>result</b> object are as follows: 
	 <details>
		 <summary><b>Show/Hide structure</b><br/>&nbsp;</summary>
            <table>
               <tr>
                  <th>Attribute</th>
                  <th>Type</th>
                  <th>Description</th>
               </tr>
               <tr>
                  <td>name</td>
                  <td>String</td>
                  <td>Internal name of the result variable.</td>
               </tr>
               <tr>
                  <td>type</td>
                  <td>String ("number", "text" or "boolean")</td>
                  <td>The datatype of the result variable. Used for validation of the results from API calls.</td>
               </tr>
               <tr>
                  <td>label</td>
                  <td>String</td>
                  <td>Label for the result variable which is displayed in the UI when this function is to be used as part of a routine.</td>
               </tr>
               <tr>
                  <td>pattern</td>
                  <td>String</td>
                  <td><i>Optional</i>. A valid Regular Expression for the <i>Python</i> library <i>re</i> (docu see <a href="https://docs.python.org/3/library/re.html">here</a>). Also used for validation of the results from API calls if present.</td>
               </tr>
               <tr>
                  <td>help_text</td>
                  <td>String</td>
                  <td><i>Optional</i>. Detailed explanation of the result of this function.</td>
               </tr>
		</table>
         </details>
      </td>
   </tr>
   <tr>
      <td>fields</td>
      <td>List</td>
      <td>
         List of the fields (or parameters) that may or have to be passed to this function when it should be executed. If a function has no fields, this should be an empty list. The attributes of each <b>field</b> in the list are as follows:
	      <details>
		      <summary><b>Show/Hide structure</b><br/>&nbsp;</summary>
         <table>
            <tr>
               <th>Attribute</th>
               <th>Type</th>
               <th>Description</th>
            </tr>
            <tr>
               <td>name</td>
               <td>String</td>
               <td>Internal name of this field. Used within API specifications.</td>
            </tr>
            <tr>
               <td>type</td>
               <td>String ("number", "text" or "boolean")</td>
               <td>The datatype of this. Used for validation.</td>
            </tr>
            <tr>
               <td>label</td>
               <td>String</td>
               <td>Label for this field which is displayed in the UI when this function is to be used as part of a routine.</td>
            </tr>
            <tr>
               <td>required</td>
               <td>Boolean</td>
               <td>If set to <i>True</i> this field must be specified when this function is invoked, otherwise not.</td>
            </tr>
            <tr>
               <td>help_text</td>
               <td>String</td>
               <td><i>Optional</i>. Detailed explanation of this field.</td>
            </tr>
         </table>
		      </details>
      </td>
   </tr>
</table>

### Data model - APIs
<table>
	<tr>
		<th>Attribute</th>
  <th>Type</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>user</td>
  <td>id</td>
		<td>Some id representing the user this API belongs to.</td>
	</tr>
	<tr>
		<td>function_name</td>
  <td>String</td>
  <td>Name of the function this API belongs to, i.e. the value of the field <i>function_name</i> of the corresponding function.</td>
	</tr>
 <tr>
		<td>name</td>
  <td>String</td>
  <td>Internal name of the API. It is displayed in the developer view of the UI and must be unique under the APIs of a given user.</td>
	</tr>
 <tr>
  <td>url</td>
  <td>String (must be valid <i>URL</i>)</td>
  <td>URL to which the request is made when this API is accessed. May contain placeholders (see below).</td>
	</tr>
 <tr>
  <td>header</td>
  <td>Object</td>
  <td>Header that should be sent together with the request to the API. Must be a valid JSON Object (might be empty). May contain placeholders (see below).</td>
	</tr>
 <tr>
  <td>request_params_template</td>
  <td>Object</td>
  <td>Parameters that should be sent together with the request to the API. Must be a valid JSON Object (might be empty). May contain placeholders (see below).</td>
	</tr>
 <tr>
  <td>request_body_template</td>
  <td>Object</td>
  <td>Body that should be sent together with the request to the API depending on request method. Must be a valid JSON Object (might be empty). May contain placeholders (see below).</td>
	</tr>
 <tr>
  <td>response_result_path</td>
  <td>String</td>
  <td>
   The path from which the value of the functions's result variable must be extracted from the API's response JSON after a successful call. Should be an empty string in case no result should be extracted. May contain placeholders (see below).
   
   Must comply with the following syntax: <i>&lt;key1&gt;.&lt;key2&gt;.&lt;key3&gt;[&lt;index&gt;].&lt;key4&gt;</i>, where <i>key3</i> holds a list. 
   More formally, it must match the following Regular Expression: <i>((\[\d+\])+(\.\w+(\[\d+\])*)*)|(\w+(\[\d+\])*(\.\w+(\[\d+\])*)*)</i>.
  </td>
	</tr>
 <tr>
  <td>request_method</td>
  <td>String ("GET", "POST", "PUT", "PATCH", "DELETE")</td>
  <td>Request method to be used when requesting the API.</td>
	</tr>
 <tr>
  <td>priority</td>
  <td>Number (0,1,2,3)</td>
  <td>The priority of this API which will be considered in the selection process when a function in invoked
   <ul> 
    <li>"0" corresponds to "low"</li>
    <li>"1" corresponds to "medium"</li>
    <li>"2" corresponds to "high"</li>
    <li>"3" corresponds to "preferred"</li>
   </ul>
   In case there are several APIs that can be applied to execute a given function, APIs with higher priority are chosen first. Only if none of the APIs with high priority is applicable resp. available, APIs with a lower priority class will be chosen. Priority "3" can be assigned only to one API for a given function and if there is any API at all there must be one preferred API. This preferred API determines which fields of the function are defined as required fields, namely, those fields that are required to apply this API.</td>
	</tr>
 <tr>
  <td>enabled</td>
  <td>Boolean</td>
  <td>If set to <i>True</i> this API will be considered in the selection process, otherwise not.</td>
	</tr>
 <tr>
  <td>placeholders</td>
  <td>List</td>
  <td>List of placeholders used in prior attributes that need to be replaced by "real" values before the API is accessed. If no placeholders were used, this is respresented by an empty list. The attributes of each <b>placeholder</b> object are as follows:
  <details>
		      <summary><b>Show/Hide structure</b><br/>&nbsp;</summary>
  <table>
	   <tr>
		    <th>Attribute</th>
      <th>Type</th>
		    <th>Description</th>
	   </tr>
	   <tr>
		    <td>id</td>
      <td>number</td>
		    <td>Id of this placeholder. Used to match it with it's usages in the aforementioned attributes. A placeholder is inserted into a template (JSON file or String) via <i>§&lt;id&gt;§</i>. At exactly this position the value of the placeholder is put when the template is filled (see later).</td>
	   </tr>
 	  <tr>
		    <td>value</td>
      <td>Object</td>
		    <td>Object which specifies how the placeholder shall be evaluated, i.e. how its value must be derived by potentialy using (some of) the fields of the invoked function specified by the user. There are two types of placeholder values: Those which correspond directly to one of the specified fields and those which are the result of another function that needs to be invoked first. In the latter case the specified fields may be used as parameters of this function. <br/>
The structure of the <b>value</b> object in either cases is shown in <a href=#appendix-1---exemplary-fixtures-of-function-and-api-specifications>Appendix 1</a> (For case 1 see step 2) and for case 2 see step 4)). <br/>		    
In both cases a boolean attribute <i>apply_function</i> specifies in which case we are. This information could also be derived from the structure of the object, but this attribute simplifies processing and validation. In the first case, i.e. if we do not apply a function, the name of the function field (attribute <i>name</i>) whose user specified value should be used as value of this placeholder is given in an attribute called <i>field</i>. In the latter case, besides the <i>apply_function</i> attribute, there are two more attributes: One, named <i>function_name</i> holding the internal name of the function to be invoked for evaluation. The second, named <i>function_fields</i> holding a list of objects specifying which fields are passed to the function for invocation together with their values. In case, a value is sourrounded by "§" signs, this means that this value should be the value of the corresponding field of the originally invoked function.
     </td>
	   </tr>
 	   <tr>
		    <td>replace_as_string</td>
      <td>Boolean</td>
		    <td>If set to <i>True</i> the value of the placeholder will always be converted to a string before it is inserted at the specified location.</td>
	   </tr>

  </table></details>
  
  </td>
	</tr>

</table>

The **flow for the execution of a function with a given API** can be roughly described as follows:
1. Check, if all fields required for applying this API were specified. 
2. If so, evaluate all placeholders and store their values.
3. Replace all placeholders in the corresponding attributes (*request_params_template* etc.) by their evaluated values.
4. Make the actual API call using the specified URL, request method etc.
5. If the call was successful and there is a result path specified, try to extract the result value from the response JSON according to specified path. 

## Technical Implementation

As already said, the Service Provider is a straightforward implementation of the Mapping Concept discussed in the prior section. Consequently the core tasks of the Service Provider are, one the hand, to manage a database of functions respectively API specifications and, on the other hand, to execute functions when requested. This is reflected in the three endpoints the Service Provider provides:
* `/services/manage_functions/`
* `/services/manage_apis/`
* `/services/invoke_function/`
Thus, at first the functionality of each endpoint is explained. After that, the flow of function execution including possible errors is depicted.


### Endpoints
#### Manage Functions Endpoint
This endpoint offers the possibility to create new functions, get a list of all existing functions as well as retrieve, update or delete an existing functions. In order to prevent as many errors as possible at this early stage all requests are validated exhaustively (see file `function_serializer.py`) before they are executed.

* **Functionality for *GET* request to `/services/manage_functions/`:**
This request results in a list of all stored functions the requesting user has access to. <!--An example for a function as it might occur in this list is depicted in [Appendix 1](#appendix-1---exemplary-fixtures-of-function-and-api-specifications).--> It should be mentioned that the *user* attribute is not included since there are anyway only functions returned which are owned by the requesting user.

* **Functionality for *POST* request to `/services/manage_functions/`:**
Via this request new functions can be added to the user's list of functions. An example for a valid request is depicted in [Appendix 1](#appendix-1---exemplary-fixtures-of-function-and-api-specifications), step 1).

* **Functionality for *GET* request to `/services/manage_functions/{functionId}`:**
This request results in the specification of the function with the specified id, if present in the list of accessible functions.

* **Functionality for *PUT* request to `/services/manage_functions/{functionId}`:**
Via this request the function with the specified id can be updated, if present in the list of accessible functions. To avoid inconsistencies with any existing associated APIs, only selected attributes (*category*, *function_label*) can be updated and additional fields can be added to the list of fields. An example for a valid request is depicted in [Appendix 1](#appendix-1---exemplary-fixtures-of-function-and-api-specifications), step 3).
* **Functionality for *DELETE* request to `/services/manage_functions/{functionId}`:**
This request results in the deletion of the function with the specified id, if present in the list of accessible functions. Deleting a function results in the deletion of all associated APIs in order to prevent inconsistent data.

#### Manage APIs Endpoint
This endpoint offers the possibility to create new APIs, get a list of all existing APIs as well as retrieve, update or delete an existing API. In order to prevent as many errors as possible at this early stage all requests are validated exhaustively (see file `api_serializer.py`) before they are executed.

* **Functionality for *GET* request to `/services/manage_apis/`:**
This request results in a list of all stored APIs the requesting user has access to. <!--An example for an API as it might occur in this list is depicted in [Appendix 1](#appendix-1---exemplary-fixtures-of-function-and-api-specifications).--> Again, the *user* attribute is not included since there are anyway only APIs returned which are owned by the requesting user. 

* **Functionality for *POST* request to `/services/manage_apis/`:**
Via this request new APIs can be added to the user's list of APIs. An example for a valid request is depicted in [Appendix 1](#appendix-1---exemplary-fixtures-of-function-and-api-specifications), step 2) and 4).

* **Functionality for *GET* request to `/services/manage_apis/{ApiId}`:**
This request results in the specification of the API with the specified id, if present in the list of accessible APIs.

* **Functionality for *PUT* request to `/services/manage_apis/{ApiId}`:**
Via this request the API with the specified id can be updated, if present in the list of accessible APIs. All attributes can be edited. 

* **Functionality for *DELETE* request to `/services/manage_apis/{ApiId}`:**
This request results in the deletion of the API with the specified id, if present in the list of accessible APIs.

#### Invoke Function Endpoint
This endpoint offers the possibility to invoke a certain function by making a appropriate *POST* request. 
* **Functionality for *POST* request to `/services/invoke_function/`:**
Via this request the Service Provider can be advised to execute a certain function. Therefore, the request's body must hold the name of the function (attribute *function_name*) to be executed as well as the values of the fields that were specified by the user. An example for a valid request is depicted in [Appendix 1](#appendix-1---exemplary-fixtures-of-function-and-api-specifications), step 5). In order to prevent as many errors as possible at this early stage the provided body is validated exhaustively (applied serializer see file `invoke_function_serializer.py`) against the corresponding function specification before the request is executed.

If the request is valid (i.e. the function exists, all mandatory fields are specified, all specified fields have the correct type, etc.) the Service Provider tries to execute the function by using associated APIs. This process is discussed in the next section.

### Flow of Function execution
When executing a function, at first, all available APIs for the given function are selected and ordered according to the selection logic described above (see description of attribute *priority*). Then a method called *invoke_api* (see file `api_utils.py` and [Appendix 2](#appendix-2---source-code-for-main-part-of-function-execution)) is executed one after the other for all APIs until an execution is successful. As parameters, the method is basically told which API is to be used and which function fields are given including the specified values: 
1) Check if the given API is applicable, i.e. if all fields mandatory to use this API, i.e. to evaluate its placeholders, are specified
2) Evaluate all placeholders and store their values
3) Fill all templates belonging to attributes which required for making the request and the result extraction (*url*, *header*, *request_params_template*, *request_body_template*, *response_result_path*) by replacing all placeholders (*§{placeholderId}§*) with the values that were computed in the prior step.
4) Perform the actual API call
5) Extract the result value from the response JSON returned by the API according to the *response_result_path*, if not empty, and validate the extracted result against the function specification (i.e. check type and pattern, if one specifed) before it is returned.

Since the service provider is very generic and in principle allows the addition of any functions and APIs, erros can still occur during the execution of the method despite all previous validation. The possible exceptions (see file `exceptions.py`), all of which inheriting from an exception called *ApiInvocationError*, that might be thrown by the *invoke_api* method are explained below:

<table>
   <tr>
      <th>Error</th>
      <th>Description</th>
  </tr>
  <tr>
     <td>ApiNotApplicableError</td>
     <td>Raised when the specified inputs are not sufficient to use this API.</td>
  </tr>
  <tr>
     <td>ApiRequestError</td>
     <td>Raised when the request to the external API itself lead to an exception.</td>
  </tr>
  <tr>
     <td>ApiCallNotSuccessfulError</td>
     <td>Raised when the status code of the response object of the external API call is not 2xx.</td>
  </tr>
  <tr>
     <td>InvalidResultPathError</td>
     <td>Raised when result couldn't be extracted from the API response according to the specified result path.</td>
  </tr>
  <tr>
     <td>InvalidResponseBodyTypeError</td>
     <td>Raised when the response body does not contain valid JSON.</td>
  </tr>
  <tr>
     <td>PlaceholderEvaluationError</td>
     <td>Raised when the evaluation of a placeholder failed.</td>
  </tr>
  <tr>
     <td>ResultValidationError</td>
     <td>Raised when the validation of the extracted result fails, e.g. because it has the wrong type and cannot be converted into the desired type.</td>
  </tr>	
</table>
	

## Appendix 1 - Exemplary Fixtures of Function and API Specifications
**Initial Remark:** Many more fixtures for *POST* requests of Functions and associated APIs along with exemplary function invocations can be found [here](exemplary_function_fixtures). There you can find the fixtures of several "built-in" functions resp. APIs, i.e. functions and APIs that will be added initially to a new user account:
* [stock_price](exemplary_function_fixtures/stock_price.txt)
* [currency_converter](exemplary_function_fixtures/currency_converter.txt)
* [retrieve_company_symbol](exemplary_function_fixtures/retrieve_company_symbol.txt)
* [send_email](exemplary_function_fixtures/send_email.txt)
* [send_discord_message](exemplary_function_fixtures/send_discord_message.txt)
* [similarity_between_two_texts](exemplary_function_fixtures/similarity_between_two_texts.txt)
* [weather_for_location](exemplary_function_fixtures/weather_for_location.txt)
* [latest_news](exemplary_function_fixtures/latest_news.txt)
* [coronavirus_confirmed_cases](exemplary_function_fixtures/coronavirus_confirmed_cases.txt)
* [switch_led_on_off](exemplary_function_fixtures/switch_led_on_off.txt)
* [get_led_power](exemplary_function_fixtures/get_led_power.txt)
* [change_led_color](exemplary_function_fixtures/change_led_color.txt)

All of the following representations refer to the scenario that we want to add a function named **"stock_price"**, which should allow to query the stock price of a company which is passed as a parameter. 

1) At first we add the *stock_price* function via a **POST** request to the **manage_functions** endpoint:
```
{
	"category":"Finance",
	"function_name":"stock_price",
	"function_label":"Retrieve Stock Price of a Company",
	"result":{
		"name":"stock_price",
		"type":"text",
		"label":"Stock Price",
		"pattern":"\\d+([.]?\\d+)?"
	},
	"fields":[
		{
			"name":"company_symbol",
			"type":"text",
			"label":"Company Symbol",
			"required":false,
			"help_text":"Symbol of the company"
		}
	]
}
```
The function has one parameter which allows to specify the company of interest a via its stock symbol. 

2) Now we add an API that allows to execute this function via a **POST** request to the **manage_apis** endpoint:
```
{
	"function_name":"stock_price",
	"name":"Yahoo Finance (stock/get-detail by Symbol)",
	"url":"https://apidojo-yahoo-finance-v1.p.rapidapi.com/stock/get-detail",
	"header":{
		"x-rapidapi-key":"<secret_api_key>",
		"x-rapidapi-host":"apidojo-yahoo-finance-v1.p.rapidapi.com"
	},
	"request_params_template":{
		"lang":"en",
		"region":"US",
		"symbol":"§1§"
	},
	"request_body_template":{

	},
	"response_result_path":"price.regularMarketOpen.raw",
	"request_method":"GET",
	"priority":3,
	"enabled":true,
	"placeholders":[
		{
			"id":1,
			"value":{
				"field":"company_symbol",
				"apply_function":false
			},
			"replace_as_string":true
		}
	]
}
```
3) In order to use this function more conveniently without looking up the stock symbol, we want to have the possibility to specify the company of interest also via its name. Therefore, we add another field to the function via a **PUT** request to the **manage_functions** endpoint:
```
{
	"category":"Finance",
	"function_label":"Retrieve Stock Price of a Company",
	"additional_fields":[
		{
			"name":"company_name",
			"type":"text",
			"label":"Company Name",
			"required":false,
			"help_text":"Name of the company"
		}
	]
}
```
4) Now we can use the same external API as before by transforming the *company_name* value to the stock symbol of the corresponding company by using another function called *retrieve_company_symbol*. The specification of this function is not depicted here but can be found [here](exemplary_function_fixtures/retrieve_company_symbol.txt). Therefore, we add another API (which we set as the new preferred one) which allows to execute this function if the company name is given via a **POST** request to the **manage_apis** endpoint:
```
{
	"function_name":"stock_price",
	"name":"Yahoo Finance (stock/get-detail by Name)",
	"url":"https://apidojo-yahoo-finance-v1.p.rapidapi.com/stock/get-detail",
	"header":{
		"x-rapidapi-key":"<secret_api_key>",
		"x-rapidapi-host":"apidojo-yahoo-finance-v1.p.rapidapi.com"
	},
	"request_params_template":{
		"lang":"en",
		"region":"US",
		"symbol":"§1§"
	},
	"request_body_template":{

	},
	"response_result_path":"price.regularMarketOpen.raw",
	"request_method":"GET",
	"priority":3,
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
As alredy mentioned this API uses a placeholder which is evaluated by executing another function.

5) Finally, we want to invoke our function which can be done via a **POST** request to the **invoke_function** endpoint:
```
{
	"function_name":"stock_price",
	"specified_fields":[
		{
			"name":"company_name",
			"value":"Apple"
		},
		{
			"name":"company_symbol",
			"value":"AAPL"
		}
	]
}
```
It is mentionable, that the specification of the company symbol is optional since our preferred API only requires the company name only the company name is mandatory. However, by specifying optional parameters, we increase the chance of successful function execution since we could also apply the API specified in step 2) if the preferred API is not available.

## Appendix 2 - Source Code for main part of Function Execution 
**Source code of *invoke_api* method:**

```python
def invoke_api(api: Api, specified_fields, invocation_stack):
    """
    Tries to invoke the passed API with the specified fields.
    :param Api api: the services to be invoked
    :param list specified_fields: specified fields for function execution
    :param list invocation_stack: list of functions waiting for the result of this function invocation
                                  (maintained to detect cycles due to placeholder evaluations)
    :return result of the underlying function if the API call was successful
    :exception raises exception if the API can't be accessed (e.g. specified_fields not sufficient, API not available etc.)
    """
    # Append the current function to the invocation stack
    invocation_stack.append(api.function_name)

    # 1) Check if this API is applicable, i.e. if all fields required to use this API are specified
    specified_fields_names = []
    for field in specified_fields:
        specified_fields_names.append(field["name"])

    required_fields_names = api.required_fields_names()
    if not all(field_name in specified_fields_names for field_name in required_fields_names):
        missing_fields = list(set(required_fields_names) - set(specified_fields_names))
        raise ApiNotApplicableError(api_name=api.name, missing_fields=missing_fields)

    # 2) Evaluate all placeholders and store values in a dict composed of id-value pairs
    placeholder_values = {}
    for placeholder in api.placeholders:
        value = _evaluate_placeholder(invocation_stack, placeholder, specified_fields, api.user)
        if value is None:
            raise PlaceholderEvaluationError(api_name=api.name, placeholder_id=placeholder["id"])
        else:
            placeholder_values[placeholder["id"]] = value

    # 3) Fill all templates required for the request, i.e. replace all placeholders by the computed values
    request_url = _fill_template(api.url, placeholder_values)
    request_params = {}
    if api.request_params_template:
        request_params = _fill_template(api.request_params_template, placeholder_values)

    request_body = {}
    if api.request_body_template:
        request_body = _fill_template(api.request_body_template, placeholder_values)

    # 4) Make API Call
    try:
        if api.request_method == "GET":
            response = requests.get(url=request_url, headers=api.header, params=request_params)
        else:
            response = requests.request(api.request_method, url=request_url, headers=api.header, params=request_params,
                                        json=request_body)
    except Exception as ex:
        raise ApiRequestError(api_name=api.name, exception_string=str(ex))

    if not status.is_success(response.status_code):
        raise ApiCallNotSuccessfulError(api_name=api.name, response_string=str(response), response_text=response.text)

    # 5) Extract result value from response JSON according to the response_result_path if not empty
    if api.response_result_path != "":
        try:
            response_data = response.json()
        except ValueError:
            raise InvalidResponseBodyTypeError(api.name)
        else:
            try:
                response_result_path = _fill_template(api.response_result_path, placeholder_values)
                result = _extract_response(response_result_path, response_data)
            except ValueError:
                raise InvalidResultPathError(api_name=api.name, result_path=api.response_result_path)
            else:
                #  validate result using the information given in the function specification
                result = _validate_result(result, api.function.result.get("type"),
                                          api.function.result.get("pattern"))
                if result is None:
                    raise ResultValidationError(api_name=api.name)
    else:
        result = None

    return result
```
</details>

