--------

--------

# List of Amazon SWF commands by category<a name="cli-services-swf-commands"></a>

You can use the AWS Command Line Interface \(AWS CLI\) to create, display, and manage workflows in Amazon Simple Workflow Service \(Amazon SWF\)\.

This section lists the reference topics for Amazon SWF commands in the AWS CLI, grouped by *functional category*\.

For an *alphabetic* list of commands, see the [Amazon SWF section](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/index.html) of the *AWS CLI Command Reference*, or use the following command\.

```
$ aws swf help
```

You can also get help for an individual command, by placing the `help` directive after the command name\. The following shows an example\.

```
$ aws swf register-domain help
```

**Topics**
+ [Commands related to activities](#swf-commands-activities)
+ [Commands related to deciders](#swf-commands-deciders)
+ [Commands related to workflow executions](#swf-commands-executions)
+ [Commands related to administration](#swf-commands-administration)
+ [Visibility commands](#swf-commands-visibility)

## Commands related to activities<a name="swf-commands-activities"></a>

Activity workers use `poll-for-activity-task` to get new activity tasks\. After a worker receives an activity task from Amazon SWF, it performs the task and responds using `respond-activity-task-completed` if successful or `respond-activity-task-failed` if unsuccessful\.

The following are commands that are performed by activity workers:
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/poll-for-activity-task.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/poll-for-activity-task.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/respond-activity-task-completed.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/respond-activity-task-completed.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/respond-activity-task-failed.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/respond-activity-task-failed.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/respond-activity-task-canceled.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/respond-activity-task-canceled.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/record-activity-task-heartbeat.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/record-activity-task-heartbeat.html)

## Commands related to deciders<a name="swf-commands-deciders"></a>

Deciders use `poll-for-decision-task` to get decision tasks\. After a decider receives a decision task from Amazon SWF, it examines its workflow execution history and decides what to do next\. It calls `respond-decision-task-completed` to complete the decision task and provides zero or more next decisions\.

The following are commands that are performed by deciders:
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/poll-for-decision-task.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/poll-for-decision-task.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/respond-decision-task-completed.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/respond-decision-task-completed.html)

## Commands related to workflow executions<a name="swf-commands-executions"></a>

The following commands operate on a workflow execution:
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/request-cancel-workflow-execution.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/request-cancel-workflow-execution.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/start-workflow-execution.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/start-workflow-execution.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/signal-workflow-execution.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/signal-workflow-execution.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/terminate-workflow-execution.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/terminate-workflow-execution.html)

## Commands related to administration<a name="swf-commands-administration"></a>

Although you can perform administrative tasks from the Amazon SWF console, you can use the commands in this section to automate functions or build your own administrative tools\.

### Activity management<a name="swf-commands-administration-activity-management"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/register-activity-type.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/register-activity-type.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/deprecate-activity-type.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/deprecate-activity-type.html)

### Workflow management<a name="swf-commands-administration-workflow-management"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/register-workflow-type.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/register-workflow-type.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/deprecate-workflow-type.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/deprecate-workflow-type.html)

### Domain management<a name="swf-commands-administration-domain-management"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/register-domain.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/register-domain.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/deprecate-domain.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/deprecate-domain.html)

For more information and examples of these domain management commands, see [Working with Amazon SWF domains using the AWS CLI](cli-services-swf-domains.md)\.

### Workflow execution management<a name="swf-commands-administration-execution-management"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/request-cancel-workflow-execution.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/request-cancel-workflow-execution.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/terminate-workflow-execution.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/terminate-workflow-execution.html)

## Visibility commands<a name="swf-commands-visibility"></a>

Although you can perform visibility actions from the Amazon SWF console, you can use the commands in this section to build your own console or administrative tools\.

### Activity visibility<a name="swf-commands-activity-visibility"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-activity-types.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-activity-types.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/describe-activity-type.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/describe-activity-type.html)

### Workflow visibility<a name="swf-commands-workflow-visibility"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-workflow-types.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-workflow-types.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/describe-workflow-type.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/describe-workflow-type.html)

### Workflow execution visibility<a name="swf-commands-workflow-execution-visibility"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/describe-workflow-execution.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/describe-workflow-execution.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-open-workflow-executions.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-open-workflow-executions.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-closed-workflow-executions.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-closed-workflow-executions.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/count-open-workflow-executions.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/count-open-workflow-executions.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/count-closed-workflow-executions.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/count-closed-workflow-executions.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/get-workflow-execution-history.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/get-workflow-execution-history.html)

### Domain visibility<a name="swf-commands-domain-visibility"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-domains.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/list-domains.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/describe-domain.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/describe-domain.html)

For more information and examples of these domain visibility commands, see [Working with Amazon SWF domains using the AWS CLI](cli-services-swf-domains.md)\.

### Task list visibility<a name="swf-commands-task-list-visibility"></a>
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/count-pending-activity-tasks.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/count-pending-activity-tasks.html)
+ [https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/count-pending-decision-tasks.html](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/swf/count-pending-decision-tasks.html)