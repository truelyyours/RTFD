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
# Configure an Amazon S3 bucket

In this exercise, you will configure an Amazon Simple Storage Service (Amazon S3) bucket that can withstand a region failure. First, you will create a set of S3 buckets in different regions. Next, you will enable cross-region replication (CRR) for the bucket. Finally, you will configure public access for the bucket.

An [Amazon S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html "Using a bucket") is an object-based storage repository. The files are accessible from the internet by using a traditional HTML URL string. Objects have a maximum size of 5 TB each with no limit on total storage
## Create an Amazon S3 bucket

In this task, you will create two Amazon S3 buckets in different regions.

1.  On the AWS Console Home page, select the hamburger menu in the upper left-hand corner, and then select **All services**.
    ![update1.jpg](https://labondemand.blob.core.windows.net/content/lab185444/instructions207730/update1.jpg)

2.  In All services, in Services by category, in Storage, select **S3**.
    ![update2.jpg](https://labondemand.blob.core.windows.net/content/lab185444/instructions207730/update2.jpg)

3.  On the S3 Homepage, select **Create bucket**.

4.  In AWS Region, ensure that **US East (Ohio)** is selected.

5.  In Bucket name, enter bucket-53639020-ohio.
> The bucket name must be globally unique.

6.  In Object Ownership, select **ACLs enabled**.

> Ensure that **ACLs enabled** is selected or you will not be able to make objects public in a future task.

7.  Review the default settings, and then select **Create bucket** to create the bucket.

    > You should now be able to see your bucket listed.

8.  In AWS Region, select **US West (Oregon)**.

9.  Select **Create bucket** to create a second bucket.

> Ensure that **US West (Oregon)** is selected. Both buckets must be in different regions to enable cross-region replication in the next task.

10.  In Bucket name, enter bucket-53639020-oregon.

11.  Review the default settings, and then select **Create bucket** to create the bucket.
# Enable cross-region replication

In this task, you will enable cross-region replication (CRR) for an S3 bucket.

1.  In the S3 Management Console, select **bucket-53639020-ohio**.

2.  On the Properties tab, in Bucket Versioning, select **Edit**.

3.  In Bucket Versioning, select **Enable**, and then select **Save changes**.

4.  On the Management tab, in Replication rules, select **Create replication rule**.

> You can use [CRR](https://docs.aws.amazon.com/AmazonS3/latest/dev/replication.html#replication-requirements "Types of object replication") to replicate your Amazon S3 objects to another AWS region anywhere in the world. Amazon S3 buckets are already highly durable and highly available in a single region. However, you may need even higher levels of durability or availability with data stored in another region. This allows users who are closer to a given region to access the data with lower latency.

5.  In Replication rule name, enter EastWest.

6.  In Source bucket, ensure that the Source bucket name is **bucket-53639020-ohio**.

7.  In Source bucket, in Choose a rule scope, select **Apply to all objects in the bucket**.

8.  In the Destination, ensure that **Choose a bucket in this account** is selected, and then in Bucket name, enter bucket-53639020-oregon.
    
    ![Destination Bucket selection](https://labondemand.blob.core.windows.net/content/lab185444/l50i6k4r.jpg)

9.  When prompted, select **Enable bucket versioning** to enable versioning in the destination bucket.

10.  In the IAM role drop-down list, select **Choose from existing IAM roles**, and then select S3Replication Role-53639020.

11.  Review the replication rule, and then select **Save**. When prompted to Replicate existing objects, select **No, do not replicate existing objects**, and then select **Submit**.

12.  In the upper-left corner, select **Amazon S3** to return to the S3 Management Console page.

13.  Review the list of **General purpose buckets**. 

14.  Select **bucket-53639020-ohio**.

15.  On your local computer, create a new document named welcome.txt.

16.  At the beginning of the file, enter Welcome to the Lab Bucket!, and then save the file.

17.  On the Objects tab, select **Upload**, and then select **Add fi020-oregon** bucket.

18.  Ensure that the bucket contains the **welcome.txt** file.

> `You may need to refresh the bucket contents to see the file, the replication should take less than one minute.

19.  Open [image.jpg](https://raw.githubusercontent.com/LODSContent/ChallengeLabs_ArmResources/master/Labs/CCP-000/image.jpg "image.jpg"), and then save it on your local computer.

20.  Switch to the **bucket-53639020-ohio** bucket, and then upload the **image.jpg** file.

