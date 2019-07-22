# Tutorial: Creating and Configuring an Amazon MQ Broker<a name="amazon-mq-creating-configuring-broker"></a>

A *broker* is a message broker environment running on Amazon MQ\. It is the basic building block of Amazon MQ\. The combined description of the broker instance *class* \(`m5`, `t2`\) and *size* \(`large`, `micro`\) is a *broker instance type* \(for example, `mq.m5.large`\)\. For more information, see [Broker](broker.md)\.

The first and most common Amazon MQ task is creating a broker\. The following example shows how you can use the AWS Management Console to create and configure a broker using the AWS Management Console\.

**Topics**
+ [Configure Basic Broker Settings](#configure-basic-broker-settings-console)
+ [Configure Advanced Broker Settings](#configure-advanced-broker-settings-console)
+ [Step 3: Finish Creating the Broker](#finish-creating-broker-console)
+ [Accessing the ActiveMQ Web Console of a Broker without Public Accessibility](accessing-web-console-of-broker-without-private-accessibility.md)

## Step 1: Configure Basic Broker Settings<a name="configure-basic-broker-settings-console"></a>

1. Sign in to the [Amazon MQ console](https://console.aws.amazon.com/amazon-mq/)\.

1. Do one of the following:
   + If this is your first time using Amazon MQ, in the **Create a broker** section, type `MyBroker` for **Broker name** and then choose **Next step**\.
   + If you have created a broker before, on the **Create a broker** page, in the **Broker details** section, type `MyBroker` for **Broker name**\.

1. In the **Broker details** section, choose a **Broker instance type** \(for example, **mq\.m5\.large**\)\. For more information, see [Instance Types](broker.md#broker-instance-types)\.

1. Choose a **Deployment mode**:
   + A **Single\-instance broker** is comprised of one broker in one Availability Zone\. The broker communicates with your application and with an AWS storage location\. For more information, see [Amazon MQ Single\-Instance Broker](single-broker-deployment.md)\.
   + An **Active/standby broker for high availability** is comprised of two brokers in two different Availability Zones, configured in a *redundant pair*\. These brokers communicate synchronously with your application, and with a shared storage location\. For more information, see [Amazon MQ Active/Standby Broker for High Availability](active-standby-broker-deployment.md)\.
   + For more information on the sample blueprints for a network of brokers, see [Sample Blueprints](network-of-brokers.md#sample-deployments)\.
**Note**  
Currently, Amazon MQ supports only `ActiveMQ` broker engine versions `5.15.8`, `5.15.6` and `5.15.0`\.

1. Choose a **Broker engine** version\.
**Note**  
Currently, Amazon MQ supports only `ActiveMQ` broker engine versions `5.15.8`, `5.15.6` and `5.15.0`\.

1. In the **ActiveMQ Web Console access** section, type a **Username** and **Password**\.

## Step 2: \(Optional\) Configure Advanced Broker Settings<a name="configure-advanced-broker-settings-console"></a>

**Important**  
**Subnet\(s\)** – A single\-instance broker requires one subnet \(for example, the default subnet\)\. An active/standby broker requires two subnets\.
**Security group\(s\)** – Both single\-instance brokers and active/standby brokers require at least one security group \(for example, the default security group\)\.
**VPC** – A broker's subnet\(s\) and security group\(s\) must be in the same VPC\. EC2\-Classic resources aren't supported\. Amazon MQ only supports default VPC tenancy, and does not support dedicated VPC tenancy\.
**Public accessibility** – Disabling public accessibility makes the broker accessible only within your VPC\. For more information, see [Prefer Brokers without Public Accessibility](using-amazon-mq-securely.md#prefer-brokers-without-public-accessibility) and [Accessing the ActiveMQ Web Console of a Broker without Public Accessibility](accessing-web-console-of-broker-without-private-accessibility.md)\.

1. Expand the **Advanced settings** section\.

1. In the **Configuration** section, choose **Create a new configuration with default values** or **Select an existing configuration**\. For more information, see [Configuration](configuration.md) and [Amazon MQ Broker Configuration Parameters](amazon-mq-broker-configuration-parameters.md)\.

1. In the **Logs** section, choose whether to publish **General** logs and **Audit** logs to Amazon CloudWatch Logs\. For more information, see [Configuring Amazon MQ to Publish General and Audit Logs to Amazon CloudWatch Logs](amazon-mq-configuring-cloudwatch-logs.md)\.
**Important**  
If you don't [add the `CreateLogGroup` permission to your Amazon MQ user](amazon-mq-configuring-cloudwatch-logs.md#add-createloggroup-permission-to-user) before the user creates or reboots the broker, Amazon MQ doesn't create the log group\.  
If you don't [configure a resource\-based policy for Amazon MQ](amazon-mq-configuring-cloudwatch-logs.md#configure-resource-based-policy), the broker can't publish the logs to CloudWatch Logs\.

1. In the **Network and security section**, configure your broker's connectivity:

   1. Do one of the following:
      + Choose **Use the default VPC, subnet\(s\), and security group\(s\)\.**
      + Choose **Select existing VPC, subnet\(s\), and security group\(s\)\.**

        1. If you choose this option, you can create a new **Virtual Private Cloud \(VPC\)** on the Amazon VPC console, select an existing VPC, or select the default VPC\. For more information, see [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html) in the *Amazon VPC User Guide*\.

        1. After you create or select a VPC, you can create new **Subnet\(s\)** on the Amazon VPC console or select existing ones\. For more information, see [VPCs and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide*\.

        1. After you create or select subnets, you can select the **Security group\(s\)**\.

   1. Choose the **Public accessibility** of your broker\.

1. In the **Maintenance section**, configure your broker's maintenance schedule:

   1. To upgrade the broker to new versions as Apache releases them, choose **Enable automatic minor version upgrades**\. Automatic upgrades occur during the *maintenance window* defined by the day of the week, the time of day \(in 24\-hour format\), and the time zone \(UTC by default\)\.
**Note**  
For an active/standby broker, if one of the broker instances undergoes maintenance, it takes Amazon MQ a short while to take the inactive instance out of service, allowing the healthy standby instance to become active and to begin accepting incoming communications\.

   1. Do one of the following:
      + To allow Amazon MQ to select the maintenance window automatically, choose **No preference**\.
      + To set a custom maintenance window, choose **Select maintenance window** and then specify the **Start day** and **Start time** of the upgrades\.

## Step 3: Finish Creating the Broker<a name="finish-creating-broker-console"></a>

1. Choose **Create broker**\.

   While Amazon MQ creates your broker, it displays the **Creation in progress** status\. 

   Creating the broker takes about 15 minutes\.

   When your broker is created successfully, Amazon MQ displays the **Running** status\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazon-mq/latest/developer-guide/images/amazon-mq-getting-started-create-broker-running.png)

1. Choose ***MyBroker***\.

   On the ***MyBroker*** page, in the **Connect** section, note your broker's **[ActiveMQ Web Console](http://activemq.apache.org/web-console.html)** URL, for example:

   ```
   https://b-1234a5b6-78cd-901e-2fgh-3i45j6k178l9-1.mq.us-east-2.amazonaws.com:8162
   ```

   Also, note your broker's [wire\-level protocol **Endpoints**](http://activemq.apache.org/configuring-transports.html)\. The following is an example of an OpenWire endpoint:

   ```
   ssl://b-1234a5b6-78cd-901e-2fgh-3i45j6k178l9-1.mq.us-east-2.amazonaws.com:61617
   ```

**Note**  
For an active/standby broker, Amazon MQ provides two ActiveMQ Web Console URLs, but only one URL is active at a time\. Likewise, Amazon MQ provides two endpoints for each wire\-level protocol, but only one endpoint is active in each pair at a time\. The `-1` and `-2` suffixes denote a redundant pair\. For more information, see [Amazon MQ Broker Architecture](amazon-mq-broker-architecture.md)\)\.  
For wire\-level protocol endpoints, you can allow your application to connect to either endpoint by using the [Failover Transport](http://activemq.apache.org/failover-transport-reference.html)\.