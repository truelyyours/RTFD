In this lab, you will configure and deploy a highly available and secure website by using Amazon Web Services (AWS). First, you will configure network security for your environment, and then you will create and configure an Amazon Simple Storage Service (Amazon S3) bucket to store static assets. Next, you will create a fleet of EC2 instances by using an Auto Scaling group. Finally, you will integrate an Application Load Balancer into the Auto Scaling group.
## Understand your environments

You will be using a newly launched AWS environment that contains a VPC named Lab VPC A.
# Configure network security

In this exercise, you will configure security for the resources in a VPC. First, you will create a network **access control list (ACL)**, and then you will create a security group.
## Create a network access control list

In this task, you will create a network ACL.
1.  In the AWS Management Console, on the navigation bar, ensure that the selected region is **Ohio**.
    ![Ohio.png](https://labondemand.blob.core.windows.net/content/lab185444/Ohio.png)
> If US East (Ohio) is not selected as the region, you will not see the appropriate VPCs in the steps that follow.

2.  In the AWS Management Console, in Search for services, search for and select VPC.

3.  In the navigation pane, in Security, select **Network ACLs**.
    ![NetworkACLs.png](https://labondemand.blob.core.windows.net/content/lab185444/NetworkACLs.png)

4.  Select **Create network ACL**.
    ![CreateNetworkACL.png](https://labondemand.blob.core.windows.net/content/lab185444/CreateNetworkACL.png)

> A [network ACL](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html "Network ACL overview") acts as a **firewall** to control the flow of traffic in and out of the subnets in a VPC.

5.  On the Create network ACL page, in Name, enter WebServerACL.

6.  In VPC, select **Lab VPC A**, and then select **Create network ACL**.
    ![The Lab VPC A option](https://labondemand.blob.core.windows.net/content/lab185444/gprm2mli.jpg)

7.  Select the **WebServerACL** check box, and then clear any other check boxes.

8.  In the details pane, on the Inbound rules tab, select **Edit inbound rules**.

    ![EditInboundRules.png](https://labondemand.blob.core.windows.net/content/lab185444/EditInboundRules.png)

9.  Select **Add new rule**.

10.  In Rule number, enter 100, and then in Type, select **HTTP (80)**.

11.  Select **Add new rule**, in Rule number, enter 200, in Port Range, enter 32768-65535, and then select **Save changes**.

>  `Rule numbers are processed from the lowest number to the highest number, with the default rule of * being processed last. If a rule matches the incoming request, processing stops. Adding rule numbers that are separated by 100 allows you to insert other rules before or after the rule in order to affect processing.

12.  In the details pane, on the Outbound rules tab, select **Edit outbound rules**.

13.  Select **Add new rule**.

14.  In Rule number, enter 100, in Type, select **All Traffic**, and then select **Save changes**.

15.  On the Subnet associations tab, select **Edit subnet associations**.

16.  Select all of the available subnets, and then select **Save changes** to complete the subnet association.

>  `You must create an outbound rule on a network ACL since network ACLs are stateless. This means that incoming traffic is not automatically allowed to be responded to.
## Create a security group

In this task, you will create a security group.

1.  In the VPC Management Console, in the navigation pane, in Security, select **Security Groups**.

2.  Select **Create security group**.

3.  In Security group name, enter WebSG.

4.  In Description, enter Security group for HTTP access.

5.  In VPC, ensure that **Lab VPC A** is selected.

6.  In Inbound rules, select **Add rule**.

> A security group acts as a *virtual firewall* for your instance to control inbound and outbound traffic.

7.  In Type, select **HTTP**, in Source, select **Anywhere-IPv4**, and then in the lower-right corner of the page, select **Create security group**.

> Security groups are stateful. If a request is sent from an instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. A response to allowed inbound traffic is allowed to flow out, regardless of outbound rules.
