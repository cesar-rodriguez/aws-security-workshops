# Serverless Round

Welcome to the world of serverless!  Now you may be asking yourself, *What is serverless*? Well, it is an architecture paradigm that allows you to create your applications without provisioning or managing any servers.  Sounds great, right?  Organizations look at building serverless applications as a way of improving their scalability and reducing their operational overhead.  The responsibility of the underlying infrastructure is shifted off your plate so you can spend more time focusing on building their applications.

So with less infrastructure management you are no longer responsible for patching  your operating systems and the attack surface you need to worry about has been significantly reduced.  But with the use of serverless technologies comes *great/other* responsibility.  When you hear the word serverless you may think specifically of [AWS Lambda](https://aws.amazon.com/lambda/) but it is important to remember that there are other services used within a serverless application and securing an application involves more than just securing your Lambda functions.  

In this round you will be focused on improving the identity controls of the WildRydes serverless application (which is borrowed from [aws-serverless-workshops](https://github.com/aws-samples/aws-serverless-workshops/tree/master/WebApplication) and retrofitted for the purposes of this round).  You will get exposed to different identity concepts through the use of a variety of services such as [Amazon S3](https://aws.amazon.com/s3/), [Amazon CloudFront](https://aws.amazon.com/cloudfront/), and [Amazon Cognito](https://aws.amazon.com/cognito/).  Upon completion you should have a better idea of how to use native AWS identity controls to improve the security posture of a serverless application.

**Topic Coverage**: S3 Bucket Policies, S3 ACLs, CloudFront Origin Access Identities, Cognito User Pools, Cognito Hosted UI

### Agenda

The round is broken down into the following phases:

* **BUILD** (45 min): The Build phase involves evaluating, implementing, and enhancing the identity controls of the WildRydes application based on a set of business level functional and non-functional requirements.
* **OPERATE** (15 min):  The Operate phase involves putting on the hat of an end user and testing the controls you put in place to ensure the requirements were met.  

> If this round is done as part of an AWS sponsored event then you'll be split into teams and have a different team complete the Operate phase for your application.

<!---
### Setup

Launch the CloudFormation stack below to setup the WildRydes application:

> If this round is done as part of an AWS sponsored event then you'll be using managed accounts with the CloudFormation template already created so you can skip this step.

Region| Deploy
------|-----
US East 1 (N. Virginia) | [![Deploy in us-east-1](./images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=Identity-RR-Wksp-Serverless-Round&templateURL=https://s3-us-west-2.amazonaws.com/sa-security-specialist-workshops-us-west-2/identity-workshop/serverless/serverless-round.yml)

1. Click the **Deploy to AWS** button above (right click and open in a new tab).  This will automatically take you to the console to run the template.  
2. Click **Next** on the **Specify Template** section.
3. Click **Next** on the **Specify Details** section.
4. Click **Next** on the **Options** section.
4. Finally, acknowledge that the template will create IAM roles under **Capabilities and click **Create**.

This will bring you back to the CloudFormation console. You can refresh the page to see the stack starting to create. Before moving on, make sure the stack is in a **CREATE_COMPLETE**.

-->

## Scenario - WildRydes Identity Overhaul

You just joined a new DevOps team who manages a suite of animal-based ride sharing applications.  Given your security background you've been embedded on the team to take the lead on security related tasks, evangelize security best practices, and represent your team when interacting with your security organization.  Recently, your team inherited a new application; WildRydes.

### View your new application
1. Open the [CloudFormation console](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks?filter=active)
2. Click on the **Identity-RR-Wksp-Serverless-Round** stack.
3. Click on **Outputs** and click on **WebsiteCloudFrontURL**.

As part of the hand off to your team, the product team shared their vision for the application and stated that feature iterations will include more dynamic features.  After doing an evaluation of the architecture you determined that the WildRydes application is a static website hosted in an S3 bucket.  There is a CloudFront Distribution setup to be used as a content delivery network and a Cognito User Pool for user management.

### Current Application Architecture

![Architecture](./images/architecture-start.png)

After thoroughly evaluating the architecture and doing a threat modeling exercise your team has identified a number of broken features and misconfigurations.  It looks as though someone started putting in place certain security controls but was not able to fully implement them. These reviews resulted in the creation of a couple tasks that were added to the backlog and given a high priority.

## Build phase

Since you are championing the security tasks for your team, you pick up the two tasks for the WildRydes application.  Please read through and complete the following tasks.  Good Luck!

#### Task 1 - Reduce the attack surface of the origin

Ensure the application serves content out through the CloudFront Distribution and that your end users can **only** access the application through CloudFront URLs and not Amazon S3 URLs. As part of this configuration your end users should **not** be able to affect the availability or integrity of the application. Lastly, your Administrators and Operators (Role name: identity-wksp-serverless-operate) should still be able to perform the functions as permitted by their IAM policies.

##### Key Security Benefits

* Obfuscates the S3 origin
* Forces traffic over HTTPS
* Adds DDoS protection to your application and enables the use of [AWS WAF](https://aws.amazon.com/waf/) and [AWS Shield](https://aws.amazon.com/shield/)

> **Hint**: Not sure where to start? How do you usually figure out things you don't know? Google it (Serving Private content through CloudFront)!  For this task you don't need to worry about signed URLs or cookies.

#### Task 2 - Setup application user management 

Set up user management for the application using Cognito User pools.  To reduce the operational overhead of creating and maintaining forms and custom logic for authentication, the decision has been made to use the Cognito hosted-UI to integrate the application with the User Pool.

As part of the user experience users should be able to sign themselves up, they should have to validate their email address, and be required to create a password that meets the *password complexity requirements for applications* set in your security standards.

#### Password Complexity Requirements for Applications
* Minimum length of 10 characters
* Must include symbols
* Must include numbers
* Must include uppercase characters

#### Additional Requirements
* Implicit grant OAuth flow
* Scopes: email openid
* Upon successful authentication the user should be redirected to ride.html

> The only application file you will need to modify is index.html

***

After you have completed these tasks you can move on to the Operate Phase.

> If you are doing this as part of an AWS sponsored event STOP here and wait for further instructions on the hand off to the next team.

### **[Operate Phase](./serverless-round-operate.md)**