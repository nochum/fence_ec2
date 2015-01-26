# fence_ec2

fence_ec2 is an I/O Fencing agent which can be used with Amazon EC2 instances.  In order to function, the agent needs the AWS access key and private key and used by the Amazon EC2 API.

This work implements extensive adaptations by Nochum Klein to Andrew Beekhof's original work.

There are a few important items to note.

  1.  AWS charges a full hour for an EC2 instance in advance every time you start an instance.  If you plan on testing fencing and start and stop an instance 5 times during the course of an hour, you will pay for 5 hours.  Fencing usually involves at least 2 instances, so you can multiply that number by two.
  2.  The time it takes for an EC2 instance to stop is "inconsistent" at best.  Stopping an instance can sometimes take minutes.  If you Google, you will see many posts about instances staying in the "stopping" state.  This behavior means that reliance on the EC2 interface for stopping instances  may not be satisfactory for the purposes of fencing.

API functions used by this agent:
  - `ec2-describe-instances`
  - `ec2-describe-tags`
  - `ec2-start-instances`
  - `ec2-stop-instances`

The agent should be able to automatically discover the instances it can control if the unique name used by the cluster node is any of:
 - Public DNS name (or part there of),
 - Private DNS name (or part there of),
 - Instance ID (eg. i-4f15a839)
 - Contents of tag associated with the instance
 
Anyone looking to better understand how to implement a fence agent might to take a look at the [Fedora Hosted Cluster Wiki - Fence Agent API].

[Fedora Hosted Cluster Wiki - Fence Agent API]:https://fedorahosted.org/cluster/wiki/FenceAgentAPI