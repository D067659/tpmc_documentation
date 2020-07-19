# Frontend
The Frontend is the single contact point with the user and should thereby be documented as well. It is mentionable that this documentation is more focused on a technical perspective and is thereby not aggregated with screenshots. For a walkthrough demo, explaining the functionality step by step, please refer to the respective section of this GitHub Repository (**LINK FOLLOWS HERE**).

## General Information about the technology used
The underlying technology for the frontend is VueJS. It is a very lightweight JavaScript framework. The benefit of using such JavaScript frameworks is the easy plug-and-play functionality, like having all necessary components ready to integrate or the perfect adoption to mobile devices (responsiveness).
VueJS, just as ReactJS, follows the principle of a component-based design. This means, one single page is made up of different blogs, written and coded in multiple JavaScript files, containing HTML for the visual design and JavaScript for their logical behavior. A huge benefit of this approach is the resuability of those componenets in multiple different positions of the website (multiple examples of this reusability is provided in the following chapters). All created components can be found in the [Frontend GitHub Repository, folder COMPONENTS](https://github.com/RalucaChis/tpmc_frontend/tree/master/components).
Furthermore, it is common practice to not strictly separate HTML and JS in different files, but to divide the first part in HTML and aggregate the second with the proper logic.

## Functionality explained by Actions
### Create or edit a Routine
After the login a user is able to either edit existing routines or create a new one from scratch. Through the model binding (binding a JSON to specific input fields of the HTML), there is no specical effort in creating a valid JSON for a routine. Through the two-way binding concept of VueJS, all the necessary information is handed-in by the user automatically by filling the required input fields. The information about the available components and their metadata is described in the [master service documentation, section AVAILABLE_COMPONENTS](https://github.com/D067659/tpmc_documentation/blob/master/technical_documentation/master_service.md#available_components-endpoint).
The creation of a routine follows a wizard component, providing a step-by-step walkthrough when creating or editing a routine. The main part, defining components for the execution of a routine, **mandatorily starts with a timer conditions** to state out a specific time when the routine shall start. This leads to the first example of the component-based idea of VueJS. The definition of components are divided into those componenets, allowing the programmer to use the coding at multiple places without any effort. Thus, the coding is used both at creating a routine and editing a routine. For this use case, 4 components are used (the order of the list shows the order of component-consumption):

1. `MTPRoutineComponent`: Facet of the whole logic, allows to add new conditions and operations and shows all inserted routine parts. 

2. `MTPCategoryComponent`: Is a part of the selection of new conditions and operations. This component provides a selection of a function by category and shows the possible fields based on the selected dropdown.

3. `MTPFunctionComponent`:  Shows all fields and allows the input, this is the main level of shaping the desired routine JSON.

4. `MTPInputField`: The most granular level of an input field: The "physical" entering of values happens here.


Nowadays, each component of the routine has to be defined by oneself. Additionally, in further time there are predefined routines available, which is out of scope for this team project but already illustrated at the frontend.

### Create or edit a Function
Navigation to the Developer Suite, the possibility exists to edit a function or an API.
This part of the frontend uses again, the model binding for defining the new JSON for a created or updated function. The concept of component-based programming is applied, too:

1. `MTPDevelopmentFunction`: This high-level component allows to decide whether a new function shall be created or an existing shall be updated. The underlying components are the same, but some parameter are different, like the setting of not-allowing to edit key fields for an existing function. 

2. `MTPDevelopmentEditFunctionForm`: After either an existing function is selected to edit or a new function should be created, this component shows all the reuqired labels and input fields (either empty, if a new function is created or pre-fileld with existing information from an existing function).
 
The changes can then be either discarded and therefore resetted to the default state or saved and thereby stored in the database.

### Create or edit an API
The same concept as described in the previous chapter applies to the second part of the Developer Suite, the edit API section. As the definition of APIs are more complex than the ones of functions, more than 2 components are used to make up the page:

1. `MTPDevelopmentApi`: This high-level component allows to decide whether a new API shall be created or an existing shall be updated. The underlying components are the same, but some parameter are different, like the setting of not-allowing to edit key fields for an existing API. 

2. `MTPDevelopmentEditApiForm`: After either an existing API is selected to edit or a new API for a function should be created, this component shows reuqired labels and input fields (either empty, if a new function is created or pre-fileld with existing information from an existing function), which are in turn multiple components, as described in the following 3a - 3c.

3a. `MTPDevelopmentKeyValueGenerator`:  Shows all fields and allows the input, this is the main level of shaping the desired routine JSON.
3b. `MTPDevelopmentTemplateForm`: The most granular level of an input field: The "physical" entering of values happens here.
3c. `MTPDevelopmentEditApiPlaceholder`: The most granular level of an input field: The "physical" entering of values happens here.
