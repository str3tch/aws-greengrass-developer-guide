# Quick Start: Greengrass Device Setup<a name="quick-start"></a>

Greengrass device setup is a script that sets up your core device in minutes, so you can quickly start using AWS IoT Greengrass\. The script:

1. Configures your device and installs the AWS IoT Greengrass Core software\.

1. Configures your cloud\-based resources\.

1. Deploys a Greengrass group with a Hello World Lambda function that sends MQTT messages to AWS IoT\. This optional step sets up the Greengrass environment shown in the following diagram\.  
![\[Hello World Lambda function sending an MQTT message to AWS IoT from the AWS IoT Greengrass core.\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/quick-start-gg-architecture.png)

## Requirements<a name="gg-device-setup-requirements"></a>

Greengrass device setup has the following requirements:
+ Your core device must use a [supported platform](what-is-gg.md#gg-platforms)\. The device must have an appropriate package manager installed: `apt`, `yum`, or `opkg`\.

   
+ The Linux user who runs the script must have permissions to run as `sudo`\.

   
+ You must provide your AWS account credentials\. For more information, see [Provide AWS Account Credentials](#gg-device-setup-credentials)\.
**Note**  
Greengrass device setup installs the [latest version](what-is-gg.md#ggc-versions) of the AWS IoT Greengrass Core software on the device\. By installing the AWS IoT Greengrass Core software, you agree to the [Greengrass Core Software License Agreement](https://s3-us-west-2.amazonaws.com/greengrass-release-license/greengrass-license-v1.pdf)\.

## Run Greengrass Device Setup<a name="run-gg-device-setup"></a>

You can run Greengrass device setup in just a few steps\. After you provide your AWS account credentials, the script provisions your Greengrass core device and deploys a Greengrass group in minutes\. Run the following commands in a terminal window on the target device\.

1. [Provide your credentials](#gg-device-setup-credentials)\. In this procedure, we assume you provide temporary security credentials as AWS CLI environment variables\.

   ```
   export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
   export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   export AWS_SESSION_TOKEN=AQoDYXdzEJr1K...o5OytwEXAMPLE=
   ```
**Note**  
If you're running Greengrass device setup on a Raspbian or OpenWrt platform, make a copy of these commands\. You use them again after you reboot the device\.

1. Download and start the script\. You can use `wget` or `curl` to download the script\.  
  
`wget`:  

   ```
   wget -q -O ./gg-device-setup-latest.sh https://d1onfpft10uf5o.cloudfront.net/greengrass-device-setup/downloads/gg-device-setup-latest.sh && chmod +x ./gg-device-setup-latest.sh && sudo -E ./gg-device-setup-latest.sh bootstrap-greengrass-interactive
   ```  
  
`curl`:  

   ```
   curl https://d1onfpft10uf5o.cloudfront.net/greengrass-device-setup/downloads/gg-device-setup-latest.sh > gg-device-setup-latest.sh && chmod +x ./gg-device-setup-latest.sh && sudo -E ./gg-device-setup-latest.sh bootstrap-greengrass-interactive 
   ```

1. Proceed through the command prompts for [input values](#gg-device-setup-input)\. You can press the **Enter** key to use the default value or type a custom value and then press **Enter**\.

   The script writes status messages to the terminal that are similar to the following\.  
![\[Output messages in the terminal.\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/quick-start-in-progress.png)

1. If your core device is running Raspbian or OpenWrt, reboot the device when prompted, provide your credentials, and then restart the script\.

   1. When prompted to reboot the device, run one of the following commands\.  
  
For Raspbian platforms:  

      ```
      sudo reboot
      ```  
  
For OpenWrt platforms:  

      ```
      reboot
      ```

   1. After the device boots up, open the terminal and provide your credentials as environment variables\.

      ```
      export AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
      export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
      export AWS_SESSION_TOKEN=AQoDYXdzEJr1K...o5OytwEXAMPLE=
      ```

   1. Restart the script\.

      ```
      sudo -E ./gg-device-setup-latest.sh bootstrap-greengrass-interactive
      ```

   1. When prompted whether to use your input values from the previous session or start a new installation, enter `yes` to reuse your input values\.
**Note**  
On platforms that require a reboot, all of your input values \(except credentials\) from the previous session are temporarily stored in the `GreengrassDeviceSetup.config.info` file\.

   When the setup is complete, the terminal displays a success status message that's similar to the following\.  
![\[Success message in the terminal output.\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/quick-start-completed.png)

1. If you chose to include the Hello World Lambda function, Greengrass device setup deploys the Greengrass group to your core device\. To test the Lambda function, or to learn how to remove the Lambda function from the group, continue to [Verify the Lambda Function Is Running on the Core Device](lambda-check.md) in Module 3\-1 of the Getting Started tutorial\.
**Note**  
Make sure that the AWS Region selected in the console is the same one that you used to configure your Greengrass environment \(by default, US West \(Oregon\)\)\.

   If you didn't include the Hello World Lambda function, you can [create your own Lambda function](create-lambda.md) or try out other Greengrass features\. For example, you can add the [Docker application deployment](docker-app-connector.md) connector to your group and use it to deploy Docker containers to your core device\.

    

### Troubleshooting Issues<a name="gg-device-setup-troubleshooting"></a>

To troubleshoot issues with running Greengrass device setup, you can look for debug information in the log files:
+ For issues with the Greengrass device setup configuration, check the `/tmp/greengrass-device-setup-bootstrap-epoch-timestamp.log` file\.
+ For issues with the Greengrass group or core environment setup, check the `GreengrassDeviceSetup-date-time.log` file in the same directory as `gg-device-setup-latest.sh` or in the location you specified\.

## Greengrass Device Setup Configuration Options<a name="gg-device-setup-options"></a>

You configure Greengrass device setup to access your AWS resources and set up your Greengrass environment\.

### Provide AWS Account Credentials<a name="gg-device-setup-credentials"></a>

Greengrass device setup uses your AWS account credentials to access your AWS resources\. It supports long\-term credentials for an IAM user or temporary security credentials from an IAM role\.
+ To use long\-term credentials, provide the access key ID and secret access key for your IAM user\. For information about creating access keys for long\-term credentials, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\.

   
+ To use temporary security credentials \(recommended\), provide the access key ID, secret access key, and session token from an assumed IAM role\. For information about extracting temporary security credentials from the AWS STS `assume-role` command, see [Using Temporary Security Credentials with the AWS CLI](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html#using-temp-creds-sdk-cli) in the *IAM User Guide*\.

**Note**  
For the purposes of this tutorial, we assume that the IAM user or IAM role has administrator access permissions\.

You can provide your credentials in one of two ways:
+ **As environment variables\.** Set the `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, and `AWS_SESSION_TOKEN` \(if required\) environment variables before you start the script, as shown in step 1 of [Run Greengrass Device Setup](#run-gg-device-setup)\.

   
+ **As input values\.** When prompted for your access key ID, secret access key, and session token \(if required\), enter the values directly in the terminal\.

Greengrass device setup doesn't save or store your credentials\.

 

### Provide Input Values<a name="gg-device-setup-input"></a>

Greengrass device setup prompts you for input values\. You can press the **Enter** key to use the default value or type a custom value and then press **Enter**\.

#### Input values<a name="gg-device-setup-input-values"></a>

**AWS access key ID**  
The access key ID from the long\-term or temporary security credentials\. Specify this option as an input value only if you don't provide your credentials as environment variables\. For more information, see [Provide AWS Account Credentials](#gg-device-setup-credentials)\.

**AWS secret access key**  
The secret access key from the long\-term or temporary security credentials\. Specify this option as an input value only if you don't provide your credentials as environment variables\. For more information, see [Provide AWS Account Credentials](#gg-device-setup-credentials)\.

**AWS session token**  
The session token from the temporary security credentials\. Specify this option as an input value only if you don't provide your credentials as environment variables\. For more information, see [Provide AWS Account Credentials](#gg-device-setup-credentials)\.

**AWS Region**  
The AWS Region where you want to create the Greengrass group\. For the list of supported AWS Regions, see [AWS IoT Greengrass](https://docs.aws.amazon.com/general/latest/gr/greengrass.html) in the *Amazon Web Services General Reference*\.  
Default value: `us-west-2`

**Group name**  
The name for the Greengrass group\.  
Default value: `GreengrassDeviceSetup_Group_guid`

**Core name**  
The name for the Greengrass core\. The core is an AWS IoT device \(thing\) that runs the AWS IoT Greengrass Core software\. The core is added to the AWS IoT registry and the Greengrass group\. If you provide a name, it must be unique in the AWS account and AWS Region\.  
Default value: `GreengrassDeviceSetup_Core_guid`

**AWS IoT Greengrass Core software installation path**  
The location in the device file system where you want to install the AWS IoT Greengrass Core software\.  
Default value: `/`

**Hello World Lambda function**  <a name="option-hello-world-lambda-function"></a>
Indicates whether to include a Hello World Lambda function in the Greengrass group\. The function publishes an MQTT message to the `hello/world` topic every five seconds\.  
The script creates and publishes this user\-defined Lambda function in AWS Lambda and adds it to your Greengrass group\. The script also creates a subscription in the group that allows the function to send MQTT messages to AWS IoT\.  
This is a Python 3\.7 Lambda function\. If Python 3\.7 isn't installed on the device and the script is unable to install it, the script prints an error message in the terminal\. To include the Lambda function in the group, you must install Python 3\.7 manually and restart the script\. To create the Greengrass group without the Lambda function, restart the script and enter `no` when prompted to include the function\.
Default value: `no`

**Deployment timeout**  
The number of seconds before Greengrass device setup stops checking the status of the [Greengrass group deployment](deployments.md)\. This is used only when the group includes the Hello World Lambda function\. Otherwise, the group is not deployed\.  
The deployment time depends on your network speed\. For slow network speeds, you can increase this value\.  
Default value: `180`

**Log path**  
The location of the log file that contains information about Greengrass group and core setup operations\. Use this log to troubleshoot deployment and other issues with the Greengrass group and core setup\.  
Default value: `./`