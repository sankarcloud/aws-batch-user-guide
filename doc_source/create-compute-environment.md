# Creating a Compute Environment<a name="create-compute-environment"></a>

Before you can run jobs in AWS Batch, you need to create a compute environment\. You can create a managed compute environment, where AWS Batch manages the instances within the environment based on your specifications, or you can create an unmanaged compute environment where you handle the instance configuration within the environment\.

**To create a managed compute environment**

1. Open the AWS Batch console at [https://console\.aws\.amazon\.com/batch/](https://console.aws.amazon.com/batch/)\.

1. From the navigation bar, select the region to use\.

1. In the navigation pane, choose **Compute environments**, **Create environment**\.

1. Configure the environment\.

   1. For **Compute environment type**, choose **Managed**\.

   1. For **Compute environment name**, specify a unique name for your compute environment\. You can use up to 128 letters \(uppercase and lowercase\), numbers, hyphens, and underscores\.

   1. For **Service role**, choose to create a new role or use an existing role\. The role allows the AWS Batch service to make calls to the required AWS APIs on your behalf\. For more information, see [AWS Batch Service IAM Role](service_IAM_role.md)\. If you choose to create a new role, the required role \(`AWSBatchServiceRole`\) is created for you\.

   1. For **Instance role**, choose to create a new instance profile or use an existing instance profile that has the required IAM permissions attached\. This instance profile allows the Amazon ECS container instances that are created for your compute environment to make calls to the required AWS APIs on your behalf\. For more information, see [Amazon ECS Instance Role](instance_IAM_role.md)\. If you choose to create a new instance profile, the required role \(`ecsInstanceRole`\) is created for you\.

   1. For **EC2 key pair** choose an existing Amazon EC2 key pair to associate with the instance at launch\. This key pair allows you to connect to your instances with SSH \(ensure that your security group allows ingress on port 22\)\.

   1. Ensure that **Enable compute environment** is selected so that your compute environment can accept jobs from the AWS Batch job scheduler\.

1. Configure your compute resources\.

   1. For **Provisioning model**, choose **On\-Demand** to launch Amazon EC2 On\-Demand Instances or **Spot** to use Amazon EC2 Spot Instances\.

   1. If you chose to use Spot Instances:

      1. \(Optional\) For **Maximum Price**, choose the maximum percentage that a Spot Instance price can be when compared with the On\-Demand price for that instance type before instances are launched\. For example, if your maximum price is 20%, then the Spot price must be below 20% of the current On\-Demand price for that EC2 instance\. You always pay the lowest \(market\) price and never more than your maximum percentage\. If you leave this field empty, the default value is 100% of the On\-Demand price\.

      1. For **Spot fleet role**, choose an existing Amazon EC2 Spot Fleet IAM role to apply to your Spot compute environment\. If you do not already have an existing Amazon EC2 Spot Fleet IAM role, you must create one first\. For more information, see [Amazon EC2 Spot Fleet Role](spot_fleet_IAM_role.md)\.
**Important**  
To tag your Spot Instances on creation \(see [Step 7](#compute-environment-tag-step)\), your Amazon EC2 Spot Fleet IAM role must use the newer **AmazonEC2SpotFleetTaggingRole** managed policy\. The **AmazonEC2SpotFleetRole** managed policy does not have the required permissions to tag Spot Instances\. For more information, see [Spot Instances Not Tagged on Creation](troubleshooting.md#spot-instance-no-tag)\.

   1. For **Allowed instance types**, choose the Amazon EC2 instance types that may be launched\. You can specify instance families to launch any instance type within those families \(for example, `c4` or `p3`\), or you can specify specific sizes within a family \(such as `c4.8xlarge`\)\. You can also choose `optimal` to pick instance types \(from the latest C, M, and R instance families\) on the fly that match the demand of your job queues\.

   1. \(Optional\) For **Launch template**, select an existing Amazon EC2 launch template to configure your compute resources; the default version of the template is automatically populated\. For more information, see [Launch Template Support](launch-templates.md)\.

   1. \(Optional\) For **Launch template version**, enter `$Default`, `$Latest`, or a specific version number to use\.

   1. For **Minimum vCPUs**, choose the minimum number of EC2 vCPUs that your compute environment should maintain, regardless of job queue demand\.

   1. For **Desired vCPUs**, choose the number of EC2 vCPUs that your compute environment should launch with\. As your job queue demand increases, AWS Batch can increase the desired number of vCPUs in your compute environment and add EC2 instances, up to the maximum vCPUs\. As demand decreases, AWS Batch can decrease the desired number of vCPUs in your compute environment and remove instances, down to the minimum vCPUs\.

   1. For **Maximum vCPUs**, choose the maximum number of EC2 vCPUs that your compute environment can scale out to, regardless of job queue demand\.

   1. <a name="enable-custom-ami-step"></a>\(Optional\) Check **Enable user\-specified AMI ID** to use your own custom AMI\. By default, AWS Batch managed compute environments use a recent, approved version of the Amazon ECS\-optimized AMI for compute resources\. You can create and use your own AMI in your compute environment by following the compute resource AMI specification\. For more information, see [Compute Resource AMIs](compute_resource_AMIs.md)\.

      1. For **AMI ID**, paste your custom AMI ID and choose **Validate AMI**\.

1. Configure networking\.
**Important**  
Compute resources need external network access to communicate with the Amazon ECS service endpoint, so if your compute resources do not have public IP addresses, then they must use network address translation \(NAT\) to provide this access\. For more information, see [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) in the *Amazon VPC User Guide*\. For more information, see [Tutorial: Creating a VPC with Public and Private Subnets for Your Compute Environments](create-public-private-vpc.md)\.

   1. For **VPC ID**, choose a VPC into which to launch your instances\.

   1. For **Subnets**, choose which subnets in the selected VPC should host your instances\. By default, all subnets within the selected VPC are chosen\.

   1. For **Security groups**, choose a security group to attach to your instances\. By default, the default security group for your VPC is chosen\.

1. <a name="compute-environment-tag-step"></a>\(Optional\) Tag your instances\. For example, you can specify `"Name": "AWS Batch Instance - C4OnDemand"` as a tag so that each instance in your compute environment has that name\. This is helpful for recognizing your AWS Batch instances in the Amazon EC2 console\.

1. Choose **Create** to finish\.

**To create an unmanaged compute environment**

1. Open the AWS Batch console at [https://console\.aws\.amazon\.com/batch/](https://console.aws.amazon.com/batch/)\.

1. From the navigation bar, select the region to use\.

1. In the navigation pane, choose **Compute environments**, **Create environment**\.

1. For **Compute environment type**, choose **Unmanaged**\.

1. For **Compute environment name**, specify a unique name for your compute environment\. You can use up to 128 letters \(uppercase and lowercase\), numbers, hyphens, and underscores\.

1. For **Service role**, choose to create a new role or use an existing role that allows the AWS Batch service to make calls to the required AWS APIs on your behalf\. For more information, see [AWS Batch Service IAM Role](service_IAM_role.md)\. If you choose to create a new role, the required role \(`AWSBatchServiceRole`\) is created for you\.

1. Ensure that **Enable compute environment** is selected so that your compute environment can accept jobs from the AWS Batch job scheduler\.

1. Choose **Create** to finish\.

1. \(Optional\) Retrieve the Amazon ECS cluster ARN for the associated cluster\. The following AWS CLI command provides the Amazon ECS cluster ARN for a compute environment:

   ```
   aws batch describe-compute-environments --compute-environments unmanagedCE --query computeEnvironments[].ecsClusterArn
   ```

1. \(Optional\) Launch container instances into the associated Amazon ECS cluster\. For more information, see [Launching an Amazon ECS Container Instance](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/launch_container_instance.html) in the *Amazon Elastic Container Service Developer Guide*\. When you launch your compute resources, specify the Amazon ECS cluster ARN that the resources should register with the following Amazon EC2 user data\. Replace *ecsClusterArn* with the cluster ARN you obtained with the previous command\.

   ```
   #!/bin/bash
   echo "ECS_CLUSTER=ecsClusterArn" >> /etc/ecs/ecs.config
   ```
**Note**  
Your unmanaged compute environment does not have any compute resources until you launch them manually\.