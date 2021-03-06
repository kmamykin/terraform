---
layout: "aws"
page_title: "AWS: cloudwatch_metric_alarm"
sidebar_current: "docs-aws-resource-cloudwatch-metric-alarm"
description: |-
  Provides an AutoScaling Scaling Group resource.
---

# aws\_cloudwatch\_metric\_alarm

Provides a CloudWatch Metric Alarm resource.

## Example Usage
```
resource "aws_cloudwatch_metric_alarm" "foobar" {
    alarm_name = "terraform-test-foobar5"
    comparison_operator = "GreaterThanOrEqualToThreshold"
    evaluation_periods = "2"
    metric_name = "CPUUtilization"
    namespace = "AWS/EC2"
    period = "120"
    statistic = "Average"
    threshold = "80"
    alarm_description = "This metric monitor ec2 cpu utilization"
    insufficient_data_actions = []
}
```

## Example in Conjunction with Scaling Policies
```
resource "aws_autoscaling_policy" "bat" {
    name = "foobar3-terraform-test"
    scaling_adjustment = 4
    adjustment_type = "ChangeInCapacity"
    cooldown = 300
    autoscaling_group_name = "${aws_autoscaling_group.bar.name}"
}

resource "aws_cloudwatch_metric_alarm" "bat" {
    alarm_name = "terraform-test-foobar5"
    comparison_operator = "GreaterThanOrEqualToThreshold"
    evaluation_periods = "2"
    metric_name = "CPUUtilization"
    namespace = "AWS/EC2"
    period = "120"
    statistic = "Average"
    threshold = "80"
    dimensions {
        AutoScalingGroupName = "${aws_autoscaling_group.bar.name}"
    }
    alarm_description = "This metric monitor ec2 cpu utilization"
    alarm_actions = ["${aws_autoscaling_policy.bat.arn}"]
}
```
## Argument Reference

See [related part of AWS Docs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_PutMetricAlarm.html)
for details about valid values.

The following arguments are supported:

* `alarm_name` - (Required) The descriptive name for the alarm. This name must be unique within the user's AWS account
* `comparison_operator` - (Required) The arithmetic operation to use when comparing the specified Statistic and Threshold. The specified Statistic value is used as the first operand. Either of the following is supported: `GreaterThanOrEqualToThreshold`, `GreaterThanThreshold`, `LessThanThreshold`, `LessThanOrEqualToThreshold`.
* `evaluation_periods` - (Required) The number of periods over which data is compared to the specified threshold.
* `metric_name` - (Required) The name for the alarm's associated metric.
  See docs for [supported metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CW_Support_For_AWS.html).
* `namespace` - (Required) The namespace for the alarm's associated metric. See docs for the [list of namespaces](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/aws-namespaces.html).
  See docs for [supported metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CW_Support_For_AWS.html).
* `period` - (Required) The period in seconds over which the specified `statistic` is applied.
* `statistic` - (Required) The statistic to apply to the alarm's associated metric.
   Either of the following is supported: `SampleCount`, `Average`, `Sum`, `Minimum`, `Maximum`
* `threshold` - (Required) The value against which the specified statistic is compared.
* `actions_enabled` - (Optional) Indicates whether or not actions should be executed during any changes to the alarm's state. Defaults to `true`.
* `alarm_actions` - (Optional) The list of actions to execute when this alarm transitions into an ALARM state from any other state. Each action is specified as an Amazon Resource Number (ARN).
* `alarm_description` - (Optional) The description for the alarm.
* `dimensions` - (Optional) The dimensions for the alarm's associated metric.
* `insufficient_data_actions` - (Optional) The list of actions to execute when this alarm transitions into an INSUFFICIENT_DATA state from any other state. Each action is specified as an Amazon Resource Number (ARN).
* `ok_actions` - (Optional) The list of actions to execute when this alarm transitions into an OK state from any other state. Each action is specified as an Amazon Resource Number (ARN).
* `unit` - (Optional) The unit for the alarm's associated metric.

## Attributes Reference

The following attributes are exported:

* `id` - The ID of the health check

