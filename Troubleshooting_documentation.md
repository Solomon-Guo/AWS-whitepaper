http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-troubleshoot.html

Error connecting to your instance: Connection timed out:
- Check your security group rules
- [EC2-VPC] Check the route table for the subnet. 
- [EC2-VPC] Check the network access control list (ACL) for the subnet. 
- Check that your instance has a public IP address.
- Check the CPU load on your instance; the server may be overloaded.
- Check your network
- 

Error: User key not recognized by server
- Use ssh -vvv to get triple verbose debugging information
- 

Error: Host key not found, Permission denied (publickey), or Authentication failed, permission denied or
Error: Server refused our key or No supported authentication methods available
- Check your username and private key are both proper
- 

Error: Unprotected Private Key File
- chmod 0400 .ssh/my_private_key.pem
- 

Troubleshooting Instance Recovery Failures
The following issues can cause automatic recovery of your instance to fail:

    Temporary, insufficient capacity of replacement hardware.

    The instance has an attached instance store storage, which is an unsupported configuration for automatic instance recovery.

    There is an ongoing Service Health Dashboard event that prevented the recovery process from successfully executing. Refer to http://status.aws.amazon.com for the latest service availability information.

    The instance has reached the maximum daily allowance of three recovery attempts.

Troubleshooting Instances with Failed Status Checks
Initial Steps
- For an instance using an Amazon EBS-backed AMI, stop and restart the instance.
- For an instance using an instance-store backed AMI, terminate the instance and launch a replacement.


http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-recover.html

Important

The recover action is only supported on:

    C3, C4, M3, M4, R3, T2, and X1 instance types.

    Instances in a VPC

    Note

    If your instance has a public IP address, it retains the same public IP address after recovery.

    Instances with shared tenancy (where the tenancy attribute of the instance is set to default)

    Instances that use Amazon EBS storage exclusively

Currently, the recover action is not supported on:

    EC2-Classic instances

    Dedicated tenancy instances

    Instances running on dedicated hosts

    Instances with encrypted EBS volumes

    Instances that use any instance store volumes, including instances launched with block device mappings for instance store volumes

http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-restoring-volume.html
snapshot
