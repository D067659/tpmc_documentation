# Frontend
The Frontend is the single contact point with the user and should thereby be documented as well. It is mentionable that this documentation is more focused on a technical perspective and is thereby not aggregated with screenshots. For a walkthrough demo, explaining the functionality step by step, please refer to the respective section of this GitHub Repository (**LINK FOLLOWS HERE**).

## General Information about the technology used
The underlying technology for the frontend is VueJS. It is a very lightweight JavaScript framework. The benefit of using such JavaScript frameworks is the easy plug-and-play functionality, like having all necessary components ready to integrate or the perfect adoption to mobile devices (responsiveness).
VueJS, just as ReactJS, follows the principle of a component-based design. This means, one single page is made up of different blogs, written and coded in multiple JavaScript files, containing HTML for the visual design and JavaScript for their logical behavior. A huge benefit of this approach is the resuability of those componenets in multiple different positions of the website (multiple examples of this reusability is provided in the following chapters). Furthermore, it is common practice to not strictly separate HTML and JS in different files, but to divide the first part in HTML and aggregate the second with the proper logic.

## Functionality explained by Actions
### Create or edit a Routine
After the login a user is able to either edit existing routines or create a new one from scratch. Through the model binding (binding a JSON to specific input fields of the HTML), there is no specical effort in creating a valid JSON for a routine. Through the two-way binding concept of VueJS, all the necessary information is handed-in by the user automatically by filling the required input fields. The information about the available components and their metadata is described [in the Master Service documentation](https://github.com/D067659/tpmc_documentation/blob/master/technical_documentation/master_service.md#available_components-endpoint)



### Create or edit a Function

### Create or edit an API
