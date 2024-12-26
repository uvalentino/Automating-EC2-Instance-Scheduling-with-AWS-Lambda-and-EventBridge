# Automating-EC2-Instance-Scheduling-with-AWS-Lambda-and-EventBridge

Scenario

A company operates a development EC2 server that is only used during office hours, from 9:00 AM to 5:00 PM, Monday to Friday. To reduce costs, the company wants to automate the stopping and starting of this EC2 instance outside of these hours, which is estimated to save approximately 60% in costs.

Objective

Automate the start and stop of the EC2 instance:

Start the instance at 9:00 AM, Monday to Friday.

Stop the instance at 5:00 PM, Monday to Friday.

Solution Overview

Lambda Functions

Create two Lambda functions written in Python using the AWS SDK for Python (boto3).

Start Lambda: Script to start the EC2 instance.

Stop Lambda: Script to stop the EC2 instance.

Event Triggers

Use Amazon EventBridge (formerly CloudWatch Events) to schedule the execution of these Lambda functions:

Trigger the Start Lambda at 9:00 AM, Monday to Friday.

Trigger the Stop Lambda at 5:00 PM, Monday to Friday.

Steps to Implement

1. Create Lambda Functions

Prerequisites

An IAM role with permissions to manage EC2 instances (e.g., ec2:StartInstances and ec2:StopInstances).

Start Lambda Function

A Python script using boto3 to start the EC2 instance:
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    instance_id = 'i-xxxxxxxxxxxxxxxxx'  # Replace with your EC2 instance ID
    ec2.start_instances(InstanceIds=[instance_id])
    return {
        'statusCode': 200,
        'body': f'Instance {instance_id} started successfully.'
    }

Stop Lambda Function

A Python script using boto3 to stop the EC2 instance:
import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    instance_id = 'i-xxxxxxxxxxxxxxxxx'  # Replace with your EC2 instance ID
    ec2.stop_instances(InstanceIds=[instance_id])
    return {
        'statusCode': 200,
        'body': f'Instance {instance_id} stopped successfully.'
    }


  2. Configure EventBridge Triggers

Create Two Scheduled Rules

Start Event

Schedule Expression: cron(0 9 ? * MON-FRI *)

Target: Start Lambda Function

Stop Event

Schedule Expression: cron(0 17 ? * MON-FRI *)

Target: Stop Lambda Function

Steps to Configure

Go to the EventBridge Console.

Create a new rule for each schedule.

Set the target for each rule to the corresponding Lambda function.

3. Testing

Test the Lambda functions manually to ensure they start and stop the instance as expected.

Verify the EventBridge triggers fire at the correct times.

4. Monitor and Optimize

Use Amazon CloudWatch Logs to monitor Lambda execution.

Adjust the schedule or instance settings if necessary.

Expected Outcome

With this solution:

The EC2 instance will automatically start at 9:00 AM and stop at 5:00 PM, Monday to Friday.

The company will save costs by not running the instance during non-working hours.

Benefits

Cost Savings: Significant reduction in EC2 running costs.

Automation: No manual intervention required for starting/stopping the instance.

Scalability: Easily extendable to manage additional instances or different schedules.
