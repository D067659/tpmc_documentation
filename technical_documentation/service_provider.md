# Service Provider and its underlying Mapping Concept

In short, the Service Provider is the part of our Prototype which is responsible for executing pre-defined functions using external APIs. Its responsibility is cleary decoupled from the Master Service which orchestrates the execution of a user routine (docu see [here](master_service.md)). In the context of the overall microservice architecture of our prototype, the Service Provider is accessed solely via the Master Service, mainly to execute functions, i.e. certain actions possibly having some input and returning some output. 

Since the Service Provider is so to say the implementation of our overall mapping concept for dynamic adding (i.e. during runtime) of new functions and external APIs to execute those functions. Therefore, in order to be able to understand the architecture of the Service Provider it is indispensable to understand the underlying mapping concept, i.e. the way how functions and APIs are represented, first. Thus, this documentation in a first step explains how our Mapping Concept works and afterwards focusses on the technical implementation of this concept by the Service Provider.

The structure of this documentation is as follows: 
* [Underlying Mapping Concept](#underlying-mapping-concept)
* [Technical Implementation by the Service Provider](#technical-implementation-by-the-service-provider)
  * [Functionality offered by the individual Endpoints](#functionality-offered-by-the-individual-endpoints)
  * [Function Execution Flow](#function-execution-flow)
* [Appendix 1 - Exemplary Fixtures of Function and API Specifications](#appendix-1---exemplary-fixtures-of-function-and-api-specifications)
* [Appendix 2 - Source Code for main part of Function Execution](#appendix-2---source-code-for-main-part-of-function-execution) 

## Underlying Mapping Concept
The motivation for the following concept stems from one of the core goals of this project, namely to enable the "execution of service compositions independent of the available API-endpoints (that is “plug-and-play” at runtime)". Whereas the flexible composition of services is the responsibility of the Master Service, the latter part, i.e. the execution of individual services, which are represented by **functions** in terms of the Mapping Concept, is the responsibility of the Service Provider. Thereby, on the one hand, we wanted to be able to add new functions which then can be used as part of a routine . On the other hand, we wanted to be able to add new external providers, which are represented by **APIs** in terms of the Mapping Concept, which offer the functionality to execute the function by accessing certain endpoints of a *REST API*. Both should be possible <u>dynamically during runtime</u>. 

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
      <td>Area this function fits into. Has only a "semantic" meaning and is used to structure the available functions by categories in the UI.</td>
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
  <td>URL to which the request is made when this API is accessed.</td>
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
   In case there are several APIs that can be applied to execute a given function, APIs with higher priority are chosen first. Only if none of the APIs with high priority is applicable resp. available, APIs with a lower priority class will be chosen. Priority "3" can be assigned only to one API for a given function. Also, this preferred API determines which fields of the function are defined as required fields, namely, those fields that are required to apply this API.</td>
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
		    <td>Id of this placeholder. Used to match it with it's usages in the aforementioned attributes. A placeholder is inserted into a template (JSON file or String) via <i>§&lt;id&gt;§. At exactly this position the value of the placeholder is put when the template is filled (see later).</td>
	   </tr>
 	  <tr>
		    <td>value</td>
      <td>Object</td>
		    <td>Object which specifies how the placeholder shall be evaluated, i.e. how its value must be derived by potentialy using (some of) the fields of the invoked function specified by the user. There are two types of placeholder values: Those which correspond directly to one of the specified fields and those which are the result of another function that needs to be invoked first. In the latter case the specified fields may be used as parameters of this function. 
       
The structure of the <b>value</b> object in either cases is shown in Appendix 1. In both cases a boolean attribute <i>apply_function</i> specifies in which case we are. This information could also be derived from the structure of the object, but this attribute simplifies processing and validation. In the first case, i.e. if we do not apply a function, the name of the function field (attribute <i>name</i>) whose user specified value should be used as value of this placeholder is given in an attribute called <i>field</i>. In the latter case, besides the <i>apply_function</i> attribute, there are two more attributes: One, named <i>function_name</i> holding the internal name of the function to be invoked for evaluation. The second, named <i>function_fields</i> holding a list of objects specifying which fields are passed to the function for invocation together with their values. In case, a value is sourrounded by "§" signs, this means that this value should be the value of the corresponding field of the originally invoked function.
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

## Technical Implementation by the Service Provider

As already said, the Service Provider is a straightforward implementation of the Mapping Concept discussed in the prior section. Consequently the core tasks of the Service Provider are, one the hand, to manage a database of functions respectively API specifications and, on the other hand, to execute functions when requested. This is reflected in the three endpoints the Service Provider provides:
* `/services/manage_functions/`
* `/services/manage_apis/`
* `/services/invoke_function/`
Thus, at first the functionality of each endpoint is explained. After that, the flow of function execution including possible errors is depicted.


### Functionality offered by the individual Endpoints
#### Manage Functions Endpoint
This endpoint offers the possibility to create new functions, retrieve a list of all existing functions as well as retrieve, update or delete an existing functions:
* Functionality for *GET* request to `/services/manage_functions/`:
This request results in a list of all the saved functions which are created by the specific user requesting the endpoint. An example for a function as it might occur in this list is depicted in [Appendix 1](#appendix-1---exemplary-fixtures-of-function-and-api-specifications). It should be mentioned that the user field is not included since there are anyway only functions returned which are either owned by the requesting user or *built-in*, i.e. available for every user.

* Functionality for *POST* request to `/services/manage_functions/`:
Via this request new functions can be added to the user's list of functions. Since the Service Provider is very generic and thus error-prone, the function to be added is validated exhaustively (see file `function_serializer.py`) before it is stored in the database. An example for a valid request is depicted in [Appendix 1](#appendix-1---exemplary-fixtures-of-function-and-api-specifications).

* Functionality for *GET* request to `/services/manage_functions/{functionId}`:
This request results in the specification of the function with the specified id, if exists.

* Functionality for *PUT* request to `/services/manage_functions/{functionId}`:
Via this request XXXXXXXXXXXXXX.

* Functionality for *DELETE* request to `/services/manage_functions/{functionId}`:
This request results in the deletion of the function with the specified id, if exists. Deleting a function results in the deletion of dependent APIs in order to prevent inconsitent data.


Manage Functions and APIs and execute functions.
built in erklären, sonst immer nur für einen nutzer
### Function Execution Flow 
#### Flow
#### Possible Errors
Very error prone

## Appendix 1 - Exemplary Fixtures of Function and API Specifications

## Appendix 2 - Source Code for main part of Function Execution 



