---
tags:
  - aws
  - AMI
  - blog
  - SCP
  - CI/CD
  - platform
  - tagging
title: Tagging EC2 AMIs and Cross-Account Sharing
date: 2024-06-08T14:30:37.692Z
lastmod: 2024-06-11T23:28:13.694Z
---
*Let's paint a picture:*

You manage a vast array of AWS accounts across many different OUs and environments. Your team has spent a lot of time coming up with an excellent CI/CD solution to pump out hardened AMIs. Here's some 'gotchas I have ran into recently surrounding this topic.

> You can share an AMI with specific AWS accounts without making the AMI public. All you need are the AWS account IDs.

"All you need" makes things sound super easy. In general, they are. But if you are working with a large organization which utilizes [multiple AWS accounts](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html), it is generally considered best practice that you create "golden" AMIs in one account and share them "out" to other accounts or OUs for use in application deployment or infrastructure rollout.

> You can't share user-defined tags (tags that you attach to an AMI). When you share an AMI, your user-defined tags are not available to any AWS account that the AMI is shared with.

To be clear, this means that any kind of tagging metadata that you attach to your "golden" images is effectively wiped when you share the AMIs with other accounts. This means you cannot use these tags with tools such as [Cloud Custodian](https://cloudcustodian.io/) (c7n) to automate cleanup or reporting. Nor can you [create SCPs](https://stackoverflow.com/a/77573062) which would prevent `ec2:RunInstances` action.

The solution to all this of course is to make sure that when you copy AMIs across accounts to also copy any tags that are attached to the AMI resource. How will we accomplish this? Hopefully coming in another post very soon...

* [Official AWS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/sharingamis-explicit.html)
