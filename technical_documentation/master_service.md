# Master Service
The Master Service in the only contact point with the Frontend, besides the User Service for authentication. Its main responsibility is to compute a given routine, validate the outcome and send it back to the Frontend.
Since this is only a high-level described goal, it is reasonable to have a look at all the exposed endpoints of the Master Service in order to gain a holistic overview of the capabilities of the Master Service:

`/api/routine/`

`/api/functions`

`/api/apis/`

`/api/available_components` 

## Functionality explained by Endpoints
### ROUTINE Endpoint
*Functionality for GET Requests*:
A GET request to the endpoint results in a list of all the saved routines which are created by the specific user, requesting the endpoint.

*Functionality for GET Requests /routine/{routineId}/start_routine*:
The routine endpoint is used by the Frontend after finishing the creation of a specific routine, whose ID is also a part of the URI for the endpoint. It receives a JSON object of a routine. The most important parts of a routine are:
* The ID of a routine
* Several components which describe a order and a logic for executing specific actions, like checking the weather (1. component) at a given time (2. component)
* A routine name for a better UI representation than only a ID
* A user id belonging to the user who triggered the execution of a routine.

In the first place, each part of the JSONs content is evaluated. This means, operations are executed (each operations has information stored what an action should look like) and the result is stored in a variable, if a variable name was specified via the UI (to hand-over information from one action to another).
If the current component is a list, this means that a OR operation needs to be performed on those components, which are present in the identified list.
An operation (action) is explained by the parameters. After resolving and transforming those parameters into required fields for this action, the `/invoke_function/` endpoint of the Service Provider handles the real execution of the action (and the forwarding of the result back). It is mentionable that the services were desigend in a way, that the **Master Service is always the brain** of the project, while the **Service Provider is the mechanical hand**.
Besides the mentioned operations, other componenets of a given routine are conditions, specifying a momentum of execution for the operations. Currently supported condition types are:

* Number-Relations: Triggers if one number is bigger than another (e.g. stock prices, exchange rate, degree celcius)
* Timer-Based: Triggers if a specific time in a given timezone is reached

The process flow of conditions is comparable to operations: The required information is extracted from handed-in parameters and it is then decidable whether a operation is required to be executed. Those evaluable components are checked regularly until the condition is met and the operation executed. 

The algorithm continues with this logic untill no more components have been handed-over from the Frontend to the Backend. If all of the componenets were computed succesfully, the Backend returns the success message `Successfully finished routine!`. In an error case, the specific error, caused by a component, is presented as the response.

In Appendix 1 the python source code of the most relevant functions for this mechanism is added.

### FUNCTIONS Endpoint


### APIS Endpoint


### AVAILABLE_COMPONENTS Endpoint



### Appendix 1 - Source Code of Core Components Mechanism
**Starting point: start_routine**
```python
def start_routine(routine, manual=False):
    """
    Go through all routine components and execute their respective operations or evaluate
    their respective conditions one at a time.
    :param Routine routine:
    :param bool manual: Skip the first component (timer condition) in case the user manually started the routine.
    :return: String message which lets the user know if everything went well or an exception
    happened.
    :rtype: str
    """
    
    message = "Successfully finished routine!"
    try:
        routine.state = 'running'
        routine.save()
        
        variable_store = {}
        
        # skip the first component (routine.components[1:] if the user
        # manually started this routine
        components = routine.components[1 if manual else 0:]
        for component in components:
            # copy the component and lose the initial reference
            # we don't want to accidentally alter it and save it later
            component = deepcopy(component)
            evaluate_component(component, variable_store)
            
        logger.info(f"Finished execution of: {routine}")
    except Exception as ex:
        logger.warning(f"Stopped execution of: {routine}")
        logger.warning(ex)
        message = str(ex)
    finally:
        routine.state = 'standby'
        routine.save()
        
    return message
```

**Function evalute_component**
```python        
def evaluate_component(component, variable_store):
    """
    Execute an operation and store its result in a variable, if a variable name is provided.
    Evaluate a condition and throw an exception if the result is "False".
    If the current component is a list, it means an OR operation needs to be performed on
    the components inside the list. Recursively call this function to evaluate/execute the
    components within.
    :param Union[dict, list] component:
    :param dict variable_store:
    :return:
    """
    
    if isinstance(component, dict):
        component["parameters"] = replace_variables(component["parameters"], variable_store)
        
        if component["type"] == "operation":
            result = execute_operation(component)
            if "result" in component:
                variable_store[component["result"]] = result
                
        elif component["type"] == "condition":
            result = evaluate_condition(component)
            if not result:
                # throw this to so that the user knows that he can stop executing
                # further routine components.
                raise FalseConditionException(f"Condition was false: {component}")
                
    elif isinstance(component, list):
        # go through the list and recursively evaluate/execute the components
        for item in component:
            try:
                evaluate_component(item, variable_store)
                # it's enough that one condition doesn't throw an exception
                # for all conditions to be true in an OR operator
                break
            except FalseConditionException as ex:
                # if this exception was thrown, it means that the condition was "False"
                continue
        else:
            # for loop ended without reaching "break". this means that no
            # component was "True" and all of them threw the "FalseConditionException"
            raise FalseConditionException(f"Conditions were false: {component}")
```
**Example for a split of components: execute_operation**
```python
def execute_operation(operation):
    """
    Get the function which is needed to execute this operation and pass in the provided parameters.
    To find the function we need it's category name and function name.
    :param dict operation:
    :return:
    """

    function_name = operation["function_name"]
    parameters = operation["parameters"]
    return invoke_function.invoke_function(function_name, parameters)  " Targets service provider
```
