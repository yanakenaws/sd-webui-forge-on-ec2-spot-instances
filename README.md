# Stable Diffusion WebUI Forge on Amazon EC2 Spot Instances

This CloudFormation template launches a GPU instance using EC2 [Spot Instances](https://aws.amazon.com/ec2/spot/) at a low cost, allowing you to run the Stable Diffusion Web UI Forge. Spot Instances leverage AWS's unutilized surplus capacity at a discounted rate, making them ideal for personal use cases.

## Usage

1. Open the AWS CloudFormation console in either the **N. Virginia (us-east-1)** or **Oregon (us-west-2)** region.
2. Click "Create Stack".
3. Upload the `sd-webui-stack.yml` template file.
4. Provide the required parameters:
  - The AMI ID is automatically selected based on the region. Supported regions are:
    - us-east-1 (N. Virginia)
    - us-west-2 (Oregon)
  - `Ec2InstanceType`: Select the desired instance type with GPU support. Available options are:
    - g4dn.2xlarge
    - g5.xlarge
    - g6.xlarge
5. Review and create the stack.
6. After the stack creation is complete, note the `SSMCommandFor[Your OS]` output. Run this command on your local machine to establish a port forwarding session.
7. In your browser, access the Stable Diffusion Web UI at `WebUIUrl`.


## Outputs

After the stack creation is complete, the following outputs will be available:

- `SSMCommandForWindows`: AWS Systems Manager command to start a port forwarding session from Windows.
- `SSMCommandForLinuxAndMacOS`: AWS Systems Manager command to start a port forwarding session from Linux or macOS.
- `WebUIUrl`: The URL to access the Stable Diffusion Web UI (http://localhost:7860).
- `InstanceID`: The ID of the EC2 instance.

Note: The SSM commands include the specific instance ID and region, so you can copy and paste them directly into your terminal.

## Usage Notes

- The Stable Diffusion Web UI will be accessible at the provided `WebUIUrl` output using AWS Systems Manager port forwarding.
- This stack uses the FLUX.1 model instead of SDXL. The following model files are automatically downloaded:
  - FLUX.1 model: flux1-dev-fp8.safetensors (stored in /home/ubuntu/stable-diffusion-webui-forge/models/Stable-diffusion/)
  - VAE: ae.safetensors (stored in /home/ubuntu/stable-diffusion-webui-forge/models/VAE/)
  - CLIP model: clip_l.safetensors (stored in /home/ubuntu/stable-diffusion-webui-forge/models/text_encoder/)
  - Text encoder: t5xxl_fp8_e4m3fn.safetensors (stored in /home/ubuntu/stable-diffusion-webui-forge/models/text_encoder/)
- Follow [these instructions](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html) to set up your environment for using the `start-session` command with AWS Systems Manager.
- A new VPC, subnet, security group (allowing all outbound traffic and no inbound traffic), and other necessary resources are created for the deployment.
- The instance will have a 200GB gp3 volume attached for storage.
- The AWS Systems Manager Managed Instance Core policy is attached to the instance for easy management.
- Ensure you have sufficient IAM permissions and service quotas for the required resources.
- After the CloudFormation stack reaches the CREATE_COMPLETE state, wait approximately 10 minutes before accessing the Web UI. This time is needed for software installation and data download.


## Cleanup

To remove the resources created by this CloudFormation stack, including any generated images, simply delete the stack from the AWS CloudFormation console.

## License

This CloudFormation template is provided as-is, without any warranties or guarantees.