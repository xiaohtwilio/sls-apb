# SLS-APB

Serverless Plugin for SOCless Playbook Builder (apb). Automatically renders State Machines from playbook.json files when a socless-playbooks stack is deployed.

It auto-generates the:

- Helper states which Task and Parallel States need to work properly in Socless
- The Yaml config required to upload the State Machine to SOCless via Cloudformation
- Task Failure handlers for all task states that will trigger when a Lambda fails
- Retry logic for all task states that will handle AWS Lambda service exceptions

## Usage

Create your Socless Playbook Definition in a file called playbook.json.

The default retry object assigned to each `Task` looks like this:

```json
{
  "ErrorEquals": [ "Lambda.ServiceException", "Lambda.AWSLambdaException", "Lambda.SdkClientException"],
  "IntervalSeconds": 2,
  "MaxAttempts": 6,
  "BackoffRate": 2
}
```

To **disable** default retries on certain tasks or all tasks, use the Decorators object DisableDefaultRetry:

```json
"Decorators" : {
   "DisableDefaultRetry" : {
      "tasks" : [ "End_Cheer_Up", "<other_task_name>" ],
   }
}
```

```json
"Decorators" : {
   "DisableDefaultRetry" : {
      "all" : true
   }
}
```

To automatically add a Task Failure Handler to each `Task` state that will trigger when a Lambda raises an unhandled exception, timeout, out of memory, etc :

```json
"Decorators": {
   "TaskFailureHandler": {
      Task that runs when any step in the playbook fails...
   }
}
```
