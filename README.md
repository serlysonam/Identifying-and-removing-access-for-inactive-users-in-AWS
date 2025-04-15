# Identifying-and-removing-access-for-inactive-users-in-AWS

**Introduction**

This document explains an automated process for identifying and removing access for inactive users in AWS. As Virufy grows and evolves, managing user access becomes increasingly complex. 

This system automatically identifies and disables IAM users without activity recorded in CloudTrail for a specified period (default: 31 days). It uses a Lambda function triggered on a weekly schedule to perform this task. Additionally, it sends a detailed report to a specific user via Amazon SNS, listing all users who were disabled.

Architecture Diagram
![image](https://github.com/user-attachments/assets/fc8ee518-c3c6-414f-95e2-c3cf1542c4f0)


Sequence Diagram
![image](https://github.com/user-attachments/assets/8dd97b47-3f43-4388-b978-645512440d73)


**Components**

-> AWS Lambda function: Identifying inactive users and disabling their access.

-> AWS CloudTrail: Provides user activity logs.

-> AWS IAM: Provides user management capabilities.

-> Amazon EventBridge: Schedules the Lambda function to run monthly.

-> Amazon SNS: Sends reports about disabled users to designated administrators.

**Key Features:**

-> Checks CloudTrail logs for user activity within the last 31 days.

-> Excludes specified users from deactivation.

-> Disables both console and programmatic access for inactive users.

-> Logs action for auditing purposes.

-> Sends detailed reports via SNS.

**Deployment Steps**
1. Enable CloudTrail: 

Ensure CloudTrail is enabled and configured to log user activities.

For each inactive user, follow these steps to delete them:

Delete the user's access keys using aws iam delete-access-key

Deactivate and delete any MFA devices using aws iam deactivate-mfa-device and aws iam delete-virtual-mfa-device

Remove the user from any IAM groups using aws iam remove-user-from-group

Detach any managed policies using aws iam detach-user-policy

Delete any inline policies using aws iam delete-user-policy

Delete the user's login profile (if exists) using aws iam delete-login-profile

Finally, delete the user using aws iam delete-user

2. Create a new Lambda function:

3. Create an IAM role for the Lambda function

Attach the necessary permissions.

4. Set up an EventBridge rule:

Create a new rule to trigger the Lambda function weekly.

Use the cron expression.

5. Test the function:

Use the AWS Lambda console to test with sample event data.

Monitor CloudWatch Logs for function output and any errors.

6. Configure SNS:

Create an SNS topic for sending reports.

Subscribe the designated administrator’s email to the topic.
