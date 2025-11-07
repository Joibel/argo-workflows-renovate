Description: Added CRD validation rules
Authors: [Alan Clucas](https://github.com/Joibel
Component: General
Issues: 13503

Added some validation rules to the full CRDs which allow some simpler validation to happen as the object is added to the kubernetes cluster.
This is useful if you're using a mechanism which bypasses the validator such as kubectl apply.
It will inform you of

#### Template Reference Exclusivity:
* exactly one of template, inline, or templateRef must be specified in WorkflowStep and DAGTask.

#### Mutual Exclusivity Rules:
* only one template type per template
* only one of withItems/withParam/withSequence
* only one of sequence count/end
* only one of manifest/manifestFrom
* cannot use both depends and dependencies in DAG tasks.

#### DAG Task Constraints:
* task names cannot start with digit when using depends/dependencies
* cannot use continueOn with depends.

#### Timeout on Non-Leaf Templates:
* Timeout cannot be set on steps or dag templates (only on leaf templates).

#### Cron Schedule Format:
* CronWorkflow schedules must be valid 5-field cron expressions, specialdescriptors (@yearly, @hourly, etc.), or interval format (@every).

#### Metric Validation:
* metric and label names validation
* help and value fields required
* real-time gauges cannot use resourcesDuration metrics

#### Artifact Mode Validation:
* Artifact.Mode must be between 0 and 511 (0777 octal) for file permissions.

#### Enum Validations:
* PodGC strategy
* ConcurrencyPolicy
* RetryPolicy
* GaugeOperation
* Resource action
* MergeStrategy
  all have restricted allowed values.

#### Name Pattern Constraints:
* Template/Step/Task names: max 128 chars, pattern ^[a-zA-Z0-9][-a-zA-Z0-9]*$;
* Parameter/Artifact names: pattern ^[a-zA-Z0-9_][-a-zA-Z0-9_]*$.

#### Minimum Array Sizes:
* Template.Steps requires at least one step group
* Parameter.Enum requires at least one value
* CronWorkflow.Schedules requires at least one schedule
* DAG.Tasks requires at least one task.

#### Numeric Constraints:
* Parallelism minimum 1
* StartingDeadlineSeconds minimum 0.
