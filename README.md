# How to scale the server as per **CPU-Utilization**
## Prerequisites<a name="ec2-getstarted-prereqs"></a>

Before you begin, be sure that you've completed the steps in [Set up to use Amazon EC2](get-set-up-for-amazon-ec2.md)\.

## Step 1: Launch an instance MAIN SERVER<a name="ec2-launch-instance"></a>

You can launch a Linux instance using the AWS Management Console as described in the following procedure\. This tutorial is intended to help you quickly launch your first instance, so it doesn't cover all possible options\. For information about advanced options, see [Launch an instance using the new launch instance wizard](ec2-launch-instance-wizard.md)\. For information about other ways to launch your instance, see [Launch your instance](LaunchingAndUsingInstances.md)\.

**To launch an instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the EC2 console dashboard, in the **Launch instance** box, choose **Launch instance**, and then choose **Launch instance** from the options that appear\.

1. Under **Name and tags**, for **Name**, enter a descriptive name for your instance\.

1. Under **Application and OS Images \(Amazon Machine Image\)**, do the following:

   1. Choose **Quick Start**, and then choose Amazon Linux\. This is the operating system \(OS\) for your instance\.

   1. From **Amazon Machine Image \(AMI\)**, select an HVM version of Amazon Linux 2\. Notice that these AMIs are marked **Free tier eligible**\. An *Amazon Machine Image \(AMI\)* is a basic configuration that serves as a template for your instance\.

1. Under **Instance type**, from the **Instance type** list, you can select the hardware configuration for your instance\. Choose the `t2.micro` instance type, which is selected by default\. The `t2.micro` instance type is eligible for the free tier\. In Regions where `t2.micro` is unavailable, you can use a `t3.micro` instance under the free tier\. For more information, see [AWS Free Tier](https://aws.amazon.com/free/)\.

1. Under **Key pair \(login\)**, for **Key pair name**, choose the key pair that you created when getting set up\.
**Warning**  
Do not choose **Proceed without a key pair \(Not recommended\)**\. If you launch your instance without a key pair, then you can't connect to it\.

1. Next to **Network settings**, choose **Edit**\. For **Security group name**, you'll see that the wizard created and selected a security group for you\. You can use this security group, or alternatively you can select the security group that you created when getting set up using the following steps:

   1. Choose **Select existing security group**\.

   1. From **Common security groups**, choose your security group from the list of existing security groups\.

1. Keep the default selections for the other configuration settings for your instance\. 

1. Review a summary of your instance configuration in the **Summary** panel, and when you're ready, choose **Launch instance**\.

1. A confirmation page lets you know that your instance is launching\. Choose **View all instances** to close the confirmation page and return to the console\.

1. On the **Instances** screen, you can view the status of the launch\. It takes a short time for an instance to launch\. When you launch an instance, its initial state is `pending`\. After the instance starts, its state changes to `running` and it receives a public DNS name\. If the **Public IPv4 DNS** column is hidden, choose the settings icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/settings-icon.png) \) in the top\-right corner, toggle on **Public IPv4 DNS**, and choose **Confirm**\.

1. It can take a few minutes for the instance to be ready for you to connect to it\. Check that your instance has passed its status checks; you can view this information in the **Status check** column\.

## Step 2: Connect to your instance<a name="ec2-connect-to-instance-linux"></a>

There are several ways to connect to your Linux instance\. For more information, see [Connect to your Linux instance](AccessingInstances.md)\.

**Important**  
You can't connect to your instance unless you launched it with a key pair for which you have the `.pem` file and you launched it with a security group that allows SSH access from your computer\. If you can't connect to your instance, see [Troubleshoot connecting to your instance](TroubleshootingInstancesConnecting.md) for assistance\.

## Step 3: Clean up your instance<a name="ec2-clean-up-your-instance"></a>

After you've finished with the instance that you created for this tutorial, you should clean up by terminating the instance\. If you want to do more with this instance before you clean up, see [Next steps](#ec2-next-steps)\.

**Important**  
Terminating an instance effectively deletes it; you can't reconnect to an instance after you've terminated it\.

If you launched an instance that is not within the [AWS Free Tier](https://aws.amazon.com/free/), you'll stop incurring charges for that instance as soon as the instance status changes to `shutting down` or `terminated`\. To keep your instance for later, but not incur charges, you can stop the instance now and then start it again later\. For more information, see [Stop and start your instance](Stop_Start.md)\.

**To terminate your instance**

1. In the navigation pane, choose **Instances**\. In the list of instances, select the instance\.

1. Choose **Instance state**, **Terminate instance**\.

1. Choose **Terminate** when prompted for confirmation\.

   Amazon EC2 shuts down and terminates your instance\. After your instance is terminated, it remains visible on the console for a short while, and then the entry is automatically deleted\. You cannot remove the terminated instance from the console display yourself\.
   
   
   
   

##                                              **Load balancer configuration**

# Create a target group for your Application Load Balancer<a name="create-target-group"></a>

You register targets for your Application Load Balancer with a target group\. By default, the load balancer sends requests to registered targets using the port and protocol that you specified for the target group\. You can override this port when you register each target with the target group\.

After you create a target group, you can add tags\.

To route traffic to the targets in a target group, create a listener and specify the target group in the default action for the listener\. For more information, see [Listener rules](load-balancer-listeners.md#listener-rules)\. You can specify the same target group in multiple listeners, but these listeners must belong to the same Application Load Balancer\. To use a target group with a load balancer, you must verify that the target group is not in use by a listener for any other load balancer\. 

You can add or remove targets from your target group at any time\. For more information, see [Register targets with your target group](target-group-register-targets.md)\. You can also modify the health check settings for your target group\. For more information, see [Modify the health check settings of a target group](target-group-health-checks.md#modify-health-check-settings)\.

------
#### [ New console ]

**To create a target group using the new console**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Choose **Create target group**\.

1. For **Choose a target type**, select **Instances** to register targets by instance ID; **IP addresses** to register targets by IP address; or **Application Load Balancer** to register an Application Load Balancer as a target\.

1. For **Target group name**, enter a name for the target group\. This name must be unique per Region per account, can have a maximum of 32 characters, must contain only alphanumeric characters or hyphens, and must not begin or end with a hyphen\.

1. For **Protocol**, choose a protocol as follows:
   + If the listener protocol is TCP, choose **TCP** or **TCP\_UDP**\.
   + If the listener protocol is TLS, choose **TCP** or **TLS**\.
   + If the listener protocol is UDP, choose **UDP** or **TCP\_UDP**\.
   + If the listener protocol is TCP\_UDP, choose **TCP\_UDP**\.

1. \(Optional\) For **Port**, modify the default value as needed\.

1. If the target type is **IP addresses**, choose **IPv4** or **IPv6** as the **IP address type**, otherwise skip to the next step\.


1. \(Optional\) Add one or more tags as follows:

   1. Expand the **Tags** section\.

   1. Choose **Add tag**\.

   1. Enter the tag **Key** and **Value**\. Allowed characters are letters, spaces, numbers \(in UTF\-8\), and the following special characters: \+ \- = \. \_ : / @\. Do not use leading or trailing spaces\. Tag values are case\-sensitive\.

1. Choose **Next**\.

1. In the **Register targets** page, add one or more targets as follows:
   + If the target type is **Instances**, select one or more instances, enter one or more ports, and then choose **Include as pending below**\.
   + If the target type is **IP addresses**, do the following:

     1. Select a network **VPC** from the list, or choose **Other private IP addresses**\.

     1. Enter the IP address manually, or find the IP address using instance details\. You can enter up to five IP addresses at a time\.

     1. Enter the ports for routing traffic to the specified IP addresses\. 

     1. Choose **Include as pending below**\. 

1. Choose **Create target group**\.

------
# Create an Application Load Balancer<a name="create-application-load-balancer"></a>

A load balancer takes requests from clients and distributes them across targets in a target group\.

Before you begin, ensure that you have a virtual private cloud \(VPC\) with at least one public subnet in each of the Availability Zones used by your targets\.

To create a load balancer using the AWS CLI, see [Tutorial: Create an Application Load Balancer using the AWS CLI](tutorial-application-load-balancer-cli.md)\.

To create a load balancer using the AWS Management Console, complete the following tasks\.

**Topics**
+ [Step 1: Configure a target group](#configure-target-group)
+ [Step 2: Register targets](#select-targets)
+ [Step 3: Configure a load balancer and a listener](#configure-load-balancer)
+ [Step 4: Test the load balancer](#test-load-balancer)

## Step 1: Configure a target group<a name="configure-target-group"></a>

Configuring a target group allows you to register targets such as EC2 instances\. The target group that you configure in this step is used as the target group in the listener rule when you configure your load balancer\. For more information, see [Target groups for your Application Load Balancers](load-balancer-target-groups.md)\.

**To configure your target group**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Choose **Create target group**\.

1. In the **Basic configuration** section, set the following parameters:

   1. For **Choose a target type**, select **Instance** to specify targets by instance ID or **IP addresses** to specify targets by IP address\. If the target type is a **Lambda function**, you can enable health checks by selecting **Enable** in the **Health checks** section\.

   1. For **Target group name**, enter a name for the target group\.

   1. Modify the **Port** and **Protocol** as needed\. 

   1. If the target type is **IP addresses**, choose **IPv4** or **IPv6** as the **IP address type**, otherwise skip to the next step\.

      Note that only targets that have the selected IP address type can be included in this target group\. The IP address type cannot be changed after the target group is created\.

   1. For VPC, select a virtual private cloud \(VPC\) with the targets that you want to include in your target group\.

   1. For **Protocol version**, select **HTTP1** when the request protocol is HTTP/1\.1 or HTTP/2; select **HTTP2**, when the request protocol is HTTP/2 or gRPC; and select **gRPC**, when the request protocol is gRPC\. 

1. In the **Health checks** section, modify the default settings as needed\. For **Advanced health check settings**, choose the health check port, count, timeout, interval, and specify success codes\. If health checks consecutively exceed the **Unhealthy threshold** count, the load balancer takes the target out of service\. If health checks consecutively exceed the **Healthy threshold** count, the load balancer puts the target back in service\. For more information, see [Health checks for your target groups](target-group-health-checks.md)\.

1. \(Optional\) Add one or more tags as follows:

   1. Expand the **Tags** section\.

   1. Choose **Add tag**\.

   1. Enter the tag **Key** and tag **Value**\. Allowed characters are letters, spaces, numbers \(in UTF\-8\), and the following special characters: \+ \- = \. \_ : / @\. Do not use leading or trailing spaces\. Tag values are case\-sensitive\.

1. Choose **Next**\.

## Step 2: Register targets<a name="select-targets"></a>

You can register EC2 instances, IP addresses, or Lambda functions as targets in a target group\. This is an optional step to create a load balancer\. However, you must register your targets to ensure that your load balancer routes traffic to them\.

1. In the **Register targets** page, add one or more targets as follows:
   + If the target type is **Instances**, select one or more instances, enter one or more ports, and then choose **Include as pending below**\.
   + If the target type is **IP addresses**, do the following:

     1. Select a network **VPC** from the list, or choose **Other private IP addresses**\.

     1. Enter the IP address manually, or find the IP address using instance details\. You can enter up to five IP addresses at a time\.

     1. Enter the ports for routing traffic to the specified IP addresses\. 

     1. Choose **Include as pending below**\. 
   + If the target type is **Lambda**, select a Lambda function, or enter a Lambda function ARN, and then choose **Include as pending below**\.

1. Choose **Create target group**\.

## Step 3: Configure a load balancer and a listener<a name="configure-load-balancer"></a>

To create an Application Load Balancer, you must first provide basic configuration information for your load balancer, such as a name, scheme, and IP address type\. Then, you provide information about your network, and one or more listeners\. A listener is a process that checks for connection requests\. It is configured with a protocol and a port for connections from clients to the load balancer\. For more information about supported protocols and ports, see [Listener configuration](load-balancer-listeners.md#listener-configuration)\.

**To configure your load balancer and listener**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Choose **Create Load Balancer**\.

1. Under **Application Load Balancer**, choose **Create**\.

1. **Basic configuration**

   1. For **Load balancer name**, enter a name for your load balancer\. For example, **my\-alb**\. The name of your Application Load Balancer must be unique within your set of Application Load Balancers and Network Load Balancers for the Region\. Names can have a maximum of 32 characters, and can contain only alphanumeric characters and hyphens\. They can not begin or end with a hyphen, or with `internal-`\.

   1. For **Scheme**, choose **Internet\-facing** or **Internal**\. An internet\-facing load balancer routes requests from clients to targets over the internet\. An internal load balancer routes requests to targets using private IP addresses\.

   1. For **IP address type**, choose **IPv4** or **Dualstack**\. Use **IPv4** if your clients use IPv4 addresses to communicate with the load balancer\. Choose **Dualstack** if your clients use both IPv4 and IPv6 addresses to communicate with the load balancer\. 

1. **Network mapping**

   1. For **VPC**, select the VPC that you used for your EC2 instances\. If you selected **Internet\-facing** for **Scheme**, only VPCs with an internet gateway are available for selection\.

   1. For **Mappings**, select two or more Availability Zones and corresponding subnets\. Enabling multiple Availability Zones increases the fault tolerance of your applications\. 

      For an internal load balancer, you can assign a private IP address from the IPv4 or IPv6 range of each subnet instead of letting AWS assign one for you\.

      Select one subnet per zone to enable\. If you enabled **Dualstack** mode for the load balancer, select subnets with associated IPv6 CIDR blocks\. You can specify one of the following:
      + Subnets from two or more Availability Zones
      + Subnets from one or more Local Zones
      + One Outpost subnet

1. For **Security groups**, select an existing security group, or create a new one\. 

   The security group for your load balancer must allow it to communicate with registered targets on both the listener port and the health check port\. The console can create a security group for your load balancer on your behalf with rules that allow this communication\. You can also create a security group and select it instead\. For more information, see [Recommended rules](load-balancer-update-security-groups.md#security-group-recommended-rules)\.

   \(Optional\) To create a new security group for your load balancer, choose **Create a new security group**\.

1. For **Listeners and routing**, the default listener accepts HTTP traffic on port 80\. You can keep the default protocol and port, or choose different ones\. For **Default action**, choose the target group that you created\. You can optionally choose **Add listener** to add another listener \(for example, an HTTPS listener\)\.

   If you create an HTTPS listener, configure the required **Secure listener settings**\. Otherwise, go to the next step\.

   When you use HTTPS for your load balancer listener, you must deploy an SSL certificate on your load balancer\. The load balancer uses this certificate to terminate the connection and decrypt requests from clients before sending them to the targets\. For more information, see [SSL certificates](create-https-listener.md#https-listener-certificates)\. Additionally, specify the security policy that the load balancer uses to negotiate SSL connections with the clients\. For more information, see [Security policies](create-https-listener.md#describe-ssl-policies)\.

   For **Default SSL certificate**, do one of the following:
   + If you created or imported a certificate using AWS Certificate Manager, select **From ACM**, and then select the certificate\.
   + If you uploaded a certificate using IAM, select **From IAM**, and then select the certificate\.
   + If you want to import a certificate to ACM or IAM , enter a certificate name\. Then, paste the PEM\-encoded private key and body\.

1. \(Optional\) You can use **Add\-on services**, such as the **AWS Global Accelerator** to create an accelerator and associate the load balancer with the accelerator\. The accelerator name can have up to 64 characters\. Allowed characters are a\-z, A\-Z, 0\-9, \. and \- \(hyphen\)\. Once the accelerator is created, you can use the **AWS Global Accelerator** console to manage it\. 

1. **Tag and create**

   1. \(Optional\) Add a tag to categorize your load balancer\. Tag keys must be unique for each load balancer\. Allowed characters are letters, spaces, numbers \(in UTF\-8\), and the following special characters: \+ \- = \. \_ : / @\. Do not use leading or trailing spaces\. Tag values are case\-sensitive\.

   1. Review your configuration, and choose **Create load balancer**\. A few default attributes are applied to your load balancer during creation\. You can view and edit them after creating the load balancer\. For more information, see [Load balancer attributes](application-load-balancers.md#load-balancer-attributes)\.

## Step 4: Test the load balancer<a name="test-load-balancer"></a>

 After creating your load balancer, you can verify that your EC2 instances pass the initial health check\. You can then check that the load balancer is sending traffic to your EC2 instance\. To delete the load balancer, see [Delete an Application Load Balancer](load-balancer-delete.md)\.

**To test the load balancer**

1. After the load balancer is created, choose **Close**\.

1. In the navigation pane, under **Load Balancing**, choose **Target Groups**\.

1. Select the newly created target group\.

1. Choose **Targets** and verify that your instances are ready\. If the status of an instance is `initial`, it's typically because the instance is still in the process of being registered\. This status can also indicate that the instance has not passed the minimum number of health checks to be considered healthy\. After the status of at least one instance is healthy, you can test your load balancer\. For more information, see [Target health status](target-group-health-checks.md#target-health-states)\.

1. In the navigation pane, under **Load Balancing**, choose **Load Balancers**\.

1. Select the newly created load balancer\.

1. Choose **Description** and copy the DNS name of the load balancer \(for example, my\-load\-balancer\-1234567890abcdef\.elb\.us\-east\-2\.amazonaws\.com\)\. Paste the DNS name into the address field of an internet\-connected web browser\. If everything is working, the browser displays the default page of your server\.

------


## Create a launch configuration for AUTOSCALLING GROUP \(console\)<a name="create-launch-configuration"></a>

**To create a launch configuration \(console\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **Auto Scaling**, choose **Launch Configurations**\. 

1. In the navigation bar, select your AWS Region\. 

1. Choose **Create launch configuration**, and enter a name for your launch configuration\. 

1. For **Amazon machine image \(AMI\) **, choose an AMI\. To find a specific AMI, you can[ find a suitable AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html), make note of its ID, and enter the ID as search criteria\.

   To get the ID of the Amazon Linux 2 AMI:

   1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. In the navigation pane, under **Instances**, choose **Instances**, and then choose **Launch instances**\.

   1. On the **Quick Start** tab of the **Choose an Amazon Machine Image** page, note the ID of the AMI next to **Amazon Linux 2 AMI \(HVM\)**\.

1. For **Instance type**, select a hardware configuration for your instances\.

1. Under **Additional configuration**, pay attention to the following fields:

   1. \(Optional\) For **Purchasing option**, you can choose **Request Spot Instances** to request Spot Instances at the Spot price, capped at the On\-Demand price\. Optionally, you can specify a maximum price per instance hour for your Spot Instances\. 
**Note**  
Spot Instances are a cost\-effective choice compared to On\-Demand Instances, if you can be flexible about when your applications run and if your applications can be interrupted\. For more information, see [Request Spot Instances for fault\-tolerant and flexible applications](launch-template-spot-instances.md)\. 

   1. \(Optional\) For **IAM instance profile**, choose a role to associate with the instances\. For more information, see [IAM role for applications that run on Amazon EC2 instances](us-iam-role.md)\.

   1. \(Optional\) For **Monitoring**, choose whether to enable the instances to publish metric data at 1\-minute intervals to Amazon CloudWatch by enabling detailed monitoring\. Additional charges apply\. For more information, see [Configure monitoring for Auto Scaling instances](enable-as-instance-metrics.md)\.

   1. \(Optional\) For **Advanced details**, **User data**, you can specify user data to configure an instance during launch, or to run a configuration script after the instance starts\. 

   1. \(Optional\) For **Advanced details**, **IP address type**, choose whether to assign a [public IP address](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-instance-addressing.html#public-ip-addresses) to the group's instances\. If you do not set a value, the default is to use the auto\-assign public IP settings of the subnets that your instances are launched into\.

1. \(Optional\) For **Storage \(volumes\)**, if you don't need additional storage, you can skip this section\. Otherwise, to specify volumes to attach to the instances in addition to the volumes specified by the AMI, choose **Add new volume**\. Then choose the desired options and associated values for **Devices**, **Snapshot**, **Size**, **Volume type**, **IOPS**, **Throughput**, **Delete on termination**, and **Encrypted**\.

1. For **Security groups**, create or select the security group to associate with the group's instances\. If you leave the **Create a new security group** option selected, a default SSH rule is configured for Amazon EC2 instances running Linux\. A default RDP rule is configured for Amazon EC2 instances running Windows\. 

1. For **Key pair \(login\)**, choose an option under **Key pair options**\. 

   If you've already configured an Amazon EC2 instance key pair, you can choose it here\. 

   If you don't already have an Amazon EC2 instance key pair, choose **Create a new key pair** and give it a recognizable name\. Choose **Download key pair** to download the key pair to your computer\. 
**Important**  
If you need to connect to your instances, do not choose **Proceed without a key pair**\.

1. Select the acknowledgment check box, and then choose **Create launch configuration**\.

