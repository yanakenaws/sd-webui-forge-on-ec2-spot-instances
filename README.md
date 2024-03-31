# Stable Diffusion WebUI Forge on Amazon EC2 Spot Instances

This CloudFormation template launches a GPU instance using EC2 [Spot Instances](https://aws.amazon.com/ec2/spot/) at a low cost, allowing you to run the Stable Diffusion Web UI Forge. Spot Instances leverage AWS's unutilized surplus capacity at a discounted rate, making them ideal for personal use cases.

## Usage

1. Open the AWS CloudFormation console in **N. Virginia (us-east-1)** region.
2. Click "Create Stack".
3. Upload the `sd-webui-stack.yml` template file.
4. Provide the required parameters:
  - `Ec2ImageId`: The default AMI ID is for the N. Virginia region. If you need to use a different region, you'll need to find and provide the appropriate AMI ID for that region.
  - `Ec2InstanceType`: Select the desired instance type with GPU support.
  - `SubnetAZ`: Choose the Availability Zone for the subnet.
  - `GradioUsername` and `GradioPassword`: Set the credentials for Stable Diffusion Web UI authentication.
  - `AccessCidrIp`: CIDR IP range allowed to access the instance.
5. Review and create the stack.
6. After the stack creation is complete, note the `SSMCommandFor[Your OS]` output. Run this command on your local machine to establish a port forwarding session.
7. In your browser, access the Stable Diffusion Web UI at `WebUIUrl` using the `GradioUsername` and `GradioPassword` set during stack creation.


## Outputs

After the stack creation is complete, the following outputs will be available:

- `SSMCommandForWindows`: AWS Systems Manager command to start a port forwarding session from Windows.
- `SSMCommandForLinuxAndMacOS`: AWS Systems Manager command to start a port forwarding session from Linux or macOS.
- `WebUIUrl`: The URL to access the Stable Diffusion Web UI (http://localhost:7860).
- `InstanceID`: The ID of the EC2 instance.

## Usage Notes

- The Stable Diffusion Web UI will be accessible at the provided `WebUIUrl` output using AWS Systems Manager port forwarding.
- Follow [these instructions](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html) to set up your environment for using the `start-session` command with AWS Systems Manager.
- A new VPC, subnet, security group (allowing all outbound traffic and no inbound traffic), and other necessary resources are created for the deployment.
- The instance will have a 300GB gp3 volume attached for storage.
- The AWS Systems Manager Managed Instance Core policy is attached to the instance for easy management.
- Ensure you have sufficient IAM permissions and service quotas for the required resources.
- After the CloudFormation stack reaches the CREATE_COMPLETE state, wait approximately 10 minutes before accessing the Web UI. This time is needed for software installation and data download.


## Cleanup

To remove the resources created by this CloudFormation stack, including any generated images, simply delete the stack from the AWS CloudFormation console.

## License

This CloudFormation template is provided as-is, without any warranties or guarantees.