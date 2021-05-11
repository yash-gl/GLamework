# GLamework

The purpose of this library is to provide a better starting point for UiPath RPA processes than the out-of-box REFramework. Currently, the only type of framework is 1 Dispatcher-1 Performer.

## Key Benefits:
- **Config File and Organization**
  - Dynamically create your configuration structures based on a bottom-up syntax of JToken paths
  - Dynamically assign configuration variables based on other variables after assets are loaded
  - Added support for more developer-friendly .json file
- **Overall**
  - Added in_EnableInitialization, in_EnableDispatcher, and in_EnablePerformer for completely modular scalability
  - Added Stop Requested checks at the beginning of every state, so as to keep the bot responsive to Orchestrator
  - Added HandleStateException workflow to take screenshots at all exceptions throughout the process and send emails if the maximum Initialization or Dispatching errors are reached
  - Added maintenance time window checks to shut down the process when within certain hours of a day
  - Added a loop back to dispatch feature to go back to dispatcher state after no transactions are found; configurable via a config variable
  - Implemented a dynamic logging framework that allows us to pass context between all workflows
  - Added in_RunMode argument and associated logic for all nested workflows to allow running it at the unit level



## Config File Tutorial:

You will notice that there is a unique object at the beginning of the config.json file  starting with an underscore. This is the object where we define how we want our structures to be. When the bot reads the JSON for the first time, it will iterate through and find all of the leaf nodes. Then it will organize the leaf nodes in the structures defined here. 

In the template provided, all of the structures are single-root structures. This means that we will be flattening all of the leaf nodes found in each of the root elements (assets, settings, systemExceptions, etc.) and compile them into a single dictionary for us to use in the process.

In effect, this means that our Settings dictionary will have a value of 'FrameworkQueue' with the key of 'process.queue'.

There is also support to combine multi-root structures into a single Dictionary. Simply set the structure definition to concatenate all of the root node elements that you would like to include in that structure and voila! 

Ie. If I create a new struc with the definition: <code>"testStruct":"settings.framework + systemExceptions"</code>, then this structure will contain the flattened contents of both settings.framework's children leaf nodes, as well as systemExceptions'. 
