# Setting up for AWS Database Migration Service<a name="CHAP_GettingStarted.SettingUp"></a>

**Topics**
+ [Sign up for AWS](#CHAP_SettingUp.SignUp)
+ [Create an IAM user](#CHAP_SettingUp.IAM)

## Sign up for AWS<a name="CHAP_SettingUp.SignUp"></a>

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all services in AWS, including AWS DMS\. You are charged only for the services that you use\.

With AWS DMS, you pay only for the resources you use\. The AWS DMS replication instance that you create will be live \(not running in a sandbox\)\. You incur the standard AWS DMS usage fees for the instance until you stop it\. For more information about AWS DMS usage rates, see the [AWS DMS product page](http://aws.amazon.com/dms)\. If you are a new AWS customer, you can get started with AWS DMS for free; for more information, see [AWS free usage tier](http://aws.amazon.com/free/)\.

If you close your AWS account, all AWS DMS resources and configurations associated with your account are deleted after two days\. These resources include all replication instances, source and target endpoint configuration, replication tasks, and Secure Sockets Layer \(SSL\) certificates\. If you decide to use AWS DMS again later than two days after closing your account, you recreate the resources that you need\.

If you have an AWS account already, skip to the next task\.

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

Note your AWS account number, because you need it for the next task\.

## Create an IAM user<a name="CHAP_SettingUp.IAM"></a>

Services in AWS, such as AWS DMS, require that you provide credentials when you access them\. Doing this allows the service to determine whether you have permission to access its resources\. The console requires your password\. You can create access keys for your AWS account to access the AWS Command Line Interface \(AWS CLI\) or the AWS DMS API\. 

However, we don't recommend that you access AWS using the credentials for your AWS account\. Instead, we recommend that you use AWS Identity and Access Management \(IAM\) instead\. Create an IAM user, and then add the user to an IAM group with administrative permissions and grant this user administrative permissions\. You can then access AWS using a special URL and the credentials for the IAM user\.

If you signed up for AWS but have not created an IAM user for yourself, you can create one using the IAM console\.

**To create an administrator user for yourself and add the user to an administrators group \(console\)**

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user that follows and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane, choose **Users** and then choose **Add user**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** enter **Administrators**\.

1. Choose **Filter policies**, and then select **AWS managed \- job function** to filter the table contents\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.
**Note**  
You must activate IAM user and role access to Billing before you can use the `AdministratorAccess` permissions to access the AWS Billing and Cost Management console\. To do this, follow the instructions in [step 1 of the tutorial about delegating access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html)\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users and to give your users access to your AWS account resources\. To learn about using policies that restrict user permissions to specific AWS resources, see [Access management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

To sign in as this new IAM user, sign out of the AWS console\. Then use the following URL, where `your_aws_account_id` is your AWS account number without the hyphens\. For example, if your AWS account number is `1234-5678-9012`, your AWS account ID is `123456789012`\.

```
https://your_aws_account_id.signin.aws.amazon.com/console/
```

Enter the IAM user name and password that you just created\. When you're signed in, the navigation bar displays "*your\_user\_name*@*your\_aws\_account\_id*"\.

If you don't want the URL for your sign\-in page to contain your AWS account ID, you can create an account alias\. On the IAM dashboard, choose **Customize** and type an alias, such as your company name\. To sign in after you create an account alias, use the following URL\.

```
https://your_account_alias.signin.aws.amazon.com/console/
```

To verify the sign\-in link for IAM users for your account, open the IAM console and check under **AWS Account Alias** on the dashboard\.