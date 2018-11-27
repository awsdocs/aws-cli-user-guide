# List of Amazon SWF Commands by Category<a name="swf-commands-by-category"></a>

This section lists the reference topics for Amazon SWF commands in the AWS CLI\. The commands here are listed by *functional category*\.

For an *alphabetic* list of commands, see the [Amazon SWF section](https://docs.aws.amazon.com/cli/latest/reference/swf) of the *AWS CLI Command Reference*, or use the following command\.

```
$ aws swf help
```

To get help for a particular command, use the `help` directive after the command name\. The following shows an example\.

```
$ aws swf register-domain help
```

**Topics**
+ [Commands Related to Activities](#swf-commands-activities)
+ [Commands Related to Deciders](#swf-commands-deciders)
+ [Commands Related to Workflow Executions](#swf-commands-executions)
+ [Commands Related to Administration](#swf-commands-administration)
+ [Visibility Commands](#swf-commands-visibility)

## Commands Related to Activities<a name="swf-commands-activities"></a>

Activity workers use `poll-for-activity-task` to get new activity tasks\. After a worker receives an activity task from Amazon SWF, it performs the task and responds using `respond-activity-task-completed` if successful or `respond-activity-task-failed` if unsuccessful\.

The following are commands that are performed by activity workers\.
+ [poll\-for\-activity\-task](https://docs.aws.amazon.com/cli/latest/reference/swf/poll-for-activity-task.html)
+ [respond\-activity\-task\-completed](https://docs.aws.amazon.com/cli/latest/reference/swf/respond-activity-task-completed.html)
+ [respond\-activity\-task\-failed](https://docs.aws.amazon.com/cli/latest/reference/swf/respond-activity-task-failed.html)
+ [respond\-activity\-task\-canceled](https://docs.aws.amazon.com/cli/latest/reference/swf/respond-activity-task-canceled.html)
+ [record\-activity\-task\-heartbeat](https://docs.aws.amazon.com/cli/latest/reference/swf/record-activity-task-heartbeat.html)

## Commands Related to Deciders<a name="swf-commands-deciders"></a>

Deciders use `poll-for-decision-task` to get decision tasks\. After a decider receives a decision task from Amazon SWF, it examines its workflow execution history and decides what to do next\. It calls `respond-decision-task-completed` to complete the decision task and provides zero or more next decisions\.

The following are commands that are performed by deciders\.
+ [poll\-for\-decision\-task](https://docs.aws.amazon.com/cli/latest/reference/swf/poll-for-decision-task.html)
+ [respond\-decision\-task\-completed](https://docs.aws.amazon.com/cli/latest/reference/swf/respond-decision-task-completed.html)

## Commands Related to Workflow Executions<a name="swf-commands-executions"></a>

The following commands operate on a workflow execution\.
+ [request\-cancel\-workflow\-execution](https://docs.aws.amazon.com/cli/latest/reference/swf/request-cancel-workflow-execution.html)
+ [start\-workflow\-execution](https://docs.aws.amazon.com/cli/latest/reference/swf/start-workflow-execution.html)
+ [signal\-workflow\-execution](https://docs.aws.amazon.com/cli/latest/reference/swf/signal-workflow-execution.html)
+ [terminate\-workflow\-execution](https://docs.aws.amazon.com/cli/latest/reference/swf/terminate-workflow-execution.html)

## Commands Related to Administration<a name="swf-commands-administration"></a>

Although you can perform administrative tasks from the Amazon SWF console, you can use the commands in this section to automate functions or build your own administrative tools\.

### Activity Management<a name="w3aac15c21b9c21b5"></a>
+ [register\-activity\-type](https://docs.aws.amazon.com/cli/latest/reference/swf/register-activity-type.html)
+ [deprecate\-activity\-type](https://docs.aws.amazon.com/cli/latest/reference/swf/deprecate-activity-type.html)

### Workflow Management<a name="w3aac15c21b9c21b7"></a>
+ [register\-workflow\-type](https://docs.aws.amazon.com/cli/latest/reference/swf/register-workflow-type.html)
+ [deprecate\-workflow\-type](https://docs.aws.amazon.com/cli/latest/reference/swf/deprecate-workflow-type.html)

### Domain Management<a name="w3aac15c21b9c21b9"></a>
+ [register\-domain](https://docs.aws.amazon.com/cli/latest/reference/swf/register-domain.html)
+ [deprecate\-domain](https://docs.aws.amazon.com/cli/latest/reference/swf/deprecate-domain.html)

For more information and examples of these domain management commands, see [Working with Amazon SWF Domains Using the AWS Command Line Interface](cli-using-swf-domains.md)\.

### Workflow Execution Management<a name="w3aac15c21b9c21c11"></a>
+ [request\-cancel\-workflow\-execution](https://docs.aws.amazon.com/cli/latest/reference/swf/request-cancel-workflow-execution.html)
+ [terminate\-workflow\-execution](https://docs.aws.amazon.com/cli/latest/reference/swf/terminate-workflow-execution.html)

## Visibility Commands<a name="swf-commands-visibility"></a>

Although you can perform visibility actions from the Amazon SWF console, you can use the commands in this section to build your own console or administrative tools\.

### Activity Visibility<a name="swf-commands-activity-visibility"></a>
+ [list\-activity\-types](https://docs.aws.amazon.com/cli/latest/reference/swf/list-activity-types.html)
+ [describe\-activity\-type](https://docs.aws.amazon.com/cli/latest/reference/swf/describe-activity-type.html)

### Workflow Visibility<a name="swf-commands-workflow-visibility"></a>
+ [list\-workflow\-types](https://docs.aws.amazon.com/cli/latest/reference/swf/list-workflow-types.html)
+ [describe\-workflow\-type](https://docs.aws.amazon.com/cli/latest/reference/swf/describe-workflow-type.html)

### Workflow Execution Visibility<a name="swf-commands-workflow-execution-visibility"></a>
+ [describe\-workflow\-execution](https://docs.aws.amazon.com/cli/latest/reference/swf/describe-workflow-execution.html)
+ [list\-open\-workflow\-executions](https://docs.aws.amazon.com/cli/latest/reference/swf/list-open-workflow-executions.html)
+ [list\-closed\-workflow\-executions](https://docs.aws.amazon.com/cli/latest/reference/swf/list-closed-workflow-executions.html)
+ [count\-open\-workflow\-executions](https://docs.aws.amazon.com/cli/latest/reference/swf/count-open-workflow-executions.html)
+ [count\-closed\-workflow\-executions](https://docs.aws.amazon.com/cli/latest/reference/swf/count-closed-workflow-executions.html)
+ [get\-workflow\-execution\-history](https://docs.aws.amazon.com/cli/latest/reference/swf/get-workflow-execution-history.html)

### Domain Visibility<a name="swf-commands-domain-visibility"></a>
+ [list\-domains](https://docs.aws.amazon.com/cli/latest/reference/swf/list-domains.html)
+ [describe\-domain](https://docs.aws.amazon.com/cli/latest/reference/swf/describe-domain.html)

For more information and examples of these domain visibility commands, see [Working with Amazon SWF Domains Using the AWS Command Line Interface](cli-using-swf-domains.md)\.

### Task List Visibility<a name="swf-commands-task-list-visibility"></a>
+ [count\-pending\-activity\-tasks](https://docs.aws.amazon.com/cli/latest/reference/swf/count-pending-activity-tasks.html)
+ [count\-pending\-decision\-tasks](https://docs.aws.amazon.com/cli/latest/reference/swf/count-pending-decision-tasks.html)