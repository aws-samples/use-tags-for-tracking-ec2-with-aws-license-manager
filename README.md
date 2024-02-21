# Accelerate Amazon EC2 Auto Scaling for Microsoft Windows workloads

This repo hosts templates written for the AWS Blog Post "[Automatically create self-managed licenses in multiple accounts using tags](https://aws.amazon.com/blogs/modernizing-with-aws/automatically-create-self-managed-licenses-in-multiple-accounts-using-tags/)" published on the [Microsoft Workloads on AWS](https://aws.amazon.com/blogs/modernizing-with-aws/) blog channel.

## Overview
The solution will demonstrate how you can set up self-managed licenses to be tracked automatically through tagging. Managing licenses for software running on [Amazon Elastic Cloud Compute](https://aws.amazon.com/ec2/) (Amazon EC2) is critical for compliance and auditing purposes. Amazon Web Services (AWS) provides a free tool, [AWS License Manager](https://aws.amazon.com/license-manager/), to help you manage your licenses. However, license tracking and visibility can become challenging in multi-account and multi-Region environments. 

With [AWS CloudFormation](https://aws.amazon.com/cloudformation/) template [provided](/Templates/CloudFormation/AutodiscoverLicensesBasedOnTag.yml), you can automatically find resources that are using self-managed licenses (such as in a [Bring Your Own License solution](https://aws.amazon.com/blogs/mt/simplified-byol-experience-using-aws-license-manager/)) using tags, which can be discovered by an AWS Lambda function.

![Architectural Diagram](/Images/ArchitecturalDiagram.png)


## Deployment
### Cloudformation
### Prerequisites
To deploy the application to AWS you need the following:
* An active AWS account
* License Manager needs to be [onboarded](https://docs.aws.amazon.com/license-manager/latest/userguide/getting-started.html).
* For multiple account solutions, you will need to [configure AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_tutorials_basic.html), as well as set up [License Management delegation](https://aws.amazon.com/blogs/mt/license-management-using-delegated-administrator-feature-of-aws-license-manager/).
* All resources that are part of this solution will need to have [tags](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) applied.



### Getting Started
1. Download deployment template.  For this example we'll use the [AutodiscoverLicensesBasedOnTag.yml](/Templates/CloudFormation/AutodiscoverLicensesBasedOnTag.yml) CloudFormation template.
2. [Create a stack from the AWS CloudFormation Console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html).  The CloudFormation template will create the resources shown in the architectural diagram above.

### Template Parameters
The stack template includes the following parameters:

| Parameter | Required | Description |
| --- | --- | --- |
| AWSAccountId | Optional | The AWS account id of the self-managed licenses. The parameter needed with multi accounts deployment. |
| EventBridgeRuleScheduleToAdd | Yes | [Cron expression](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-cron-expressions.html) to control how frequently the automation will run to check for tags to add to License Manager. By default, it will run every Sunday at 12:00 AM. |
| EventBridgeRuleScheduleToRemove | Yes | [Cron expression](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-cron-expressions.html) to control how frequently the automation will run to check for resources that have had the tags removed. This will remove the resources from the license. By default, it will run every Sunday at 1 AM. |
| TagKey | Yes | The tag key used to link the Amazon EC2 instance with the self-managed license. |

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.