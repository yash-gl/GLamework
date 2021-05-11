# GLamework

The purpose of this library is to provide a better starting point for UiPath RPA processes than the out-of-box REFramework. Currently, the only type of framework is 1 Dispatcher-1 Performer.

Key Benefits:
- Config File and Organization
  - Dynamically create your configuration structures based on a bottom-up syntax of JToken paths
  - Dynamically assign configuration variables based on other variables after assets are loaded
  - Added support for more developer-friendly .json file
- Overall
  - Added in_EnableInitialization, in_EnableDispatcher, and in_EnablePerformer for completely modular scalability
  - Added Stop Requested checks at the beginning of every state, so as to keep the bot responsive to Orchestrator
  - Added HandleStateException workflow to take screenshots at all exceptions throughout the process and send emails if the maximum Initialization or Dispatching errors are reached
  - Added maintenance time window checks to shut down the process when within certain hours of a day
  - Added a loop back to dispatch feature to go back to dispatcher state after no transactions are found; configurable via a config variable
  - Implemented a dynamic logging framework that allows us to pass context between all workflows
  - Added in_RunMode argument and associated logic for all nested workflows to allow running it at the unit level



Config File Tutorial:
1. Starting config file:

    {
      "_config":{
        "assets":"assets",
        "settings":"settings",
        "systemExceptions":"systemExceptions",
        "businessExceptions":"businessExceptions",
        "credentials":"credentials"
    },     
      "assets": {
        "_useLocalValues": "true",
        "mailServer": "outlook365.office.com",
        "ITEmail": "it@domain.com",
        "BusinessEmail": "business@domain.com",
        "ScreenshotPath": "Screenshots\\"
    },
    "credentials": {
        "webapp1": "webapp1"
    },
    "settings": {
        "process": {
            "queue": "FrameworkQueue",
            "name": "GLamework",
            "readOnlyMode": "true",
            "performerMaxRetry":"3",
            "howToAddStringsInConfig":"${settings.process.queue + `: ` + settings.process.name}"
        },
        "network": {
            "retryCount": {
                "low": "1",
                "med": "3",
                "high": "5"
            },
            "retryTimeout": {
                "low": "00:00:02",
                "med": "00:00:05",
                "high": "00:00:10"
            }
        },
        "framework": {
            "enableDispatchLoop": "false",
            "dispatchMode": "QA",
            "TestDataPath": "Data\\test.xlsx",
            "maintenanceStart": "2:00:00",
            "maintenanceEnd": "4:00:00",
            "failJobOnScreenshot": "true"
        },
        "initialize": {
            "id": "initialize",
            "delay": "00:15:00",
            "max": "3",
            "email":{
                "to": "${assets.ITEmail}",
                "subject": "",
                "body": ""
            }
        },
        "dispatcher": {
            "id": "dispatch",
            "delay": "00:15:00",
            "max": "3",
            "email":{
                "to": "${assets.ITEmail}",
                "subject": "",
                "body": ""
            }
        },
        "performer":{
            "id": "perform"
        }
    },
    "systemExceptions": {
        "dispatcher":{
            "invalidMode":"Invalid framework.dispatchMode: QA, PROD"
        },
        "handleStateExceptions": {
            "invalidRoute": "HandleStateExceptions encountered an unexpected state."
        }
    },
    "businessExceptions":{
        "step1":{
            "exception1":"Message for exception 1",
            "exception2":"Message for exception 2"
        }
    }
    }


