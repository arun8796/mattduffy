=== Virtual Private Cloud (VPC)
Limited to 5 vpc per region

subnets do not cross availability zones: 1 subnet = 1 AZ
create a custom vpc:
* create new subnets as appropriate in the new vpc
 * by default, new subnets in the same vpc should have a router table configured so that instance in each subnet can connect to other subnets in vpc
* create an internet gateway
 * can have only 1 igw attached to a vpc at a time.
 * add a route table to igw to the subnets
  * destination for this route is 0.0.0.0/0 (the dirty dirty public internet)
  * target for this route is the newly created igw for this vpc
  * associate this new igw to which ever subnets in vpc you want to be internet accessible
  * now you can launch ec2 instances into the newly created subnets

* Create a NAT instance inside a custom vpc
 * create a new security group in the custom vpc that will be used to control ingress/egress traffic
 * this allows our private subnet to communicate with the nat instance
 * create NAT instance from EC2
  * if your nat is too small, it may become a network bootle neck, choose a larger AMI instance type for greater throughput
 * assign it the custom vpc
  * NAT instance either needs an elastic IP address or an elastic load balancer in order to access the open internet
 * assign it the custom security group created earlier to bring the two subnets
 * allocate an elastic IP address if necessary and associate it to the NAT instance
 * NAT instances my have their networking Source/Destination checks disabled to work properly (can do in ec2 console from action menu)
  * there is also a CLI way to disable this: 
	aws modify-instance-attribute --no-source-dest-check --instance-id <instance_id>
    or google search linux kernel nat forwarding for instructions like:
	echo '#!/bin/sh
	echo 1 > /proc/sys/net/ipv4/ip_forward
	iptables -t nat -A POSTROUTING -s 10.0.0.0/16 -j MASQUERADE
	' | sudo tee /etc/network/if-pre-up.d/nat-setup
	sudo chmod +x /etc/network/if-pre-up.d/nat-setup
	sudo /etc/network/if-pre-up.d/nat-setup

  * now create a new route in the vpc from the private subnet to the NAT to open access to internet

* ACLs in the VPC overrule any rules specified in the vpc security group
* by default, existing ACLs allow all ingres/egres traffic
* each new custom ACLs you create defaults to blocking ALL ingres/egres traffic
* ACLs rules are evaluated from the lowest number first, ascending (aws recommends adding new rules with multiples of 100 so you can insert rules later if necessary)
* can NOT have multiple network ACLs associated to a single vpc subnet at one time, however, a network ACLs can have more than 1 vpc subnet associated with it

VPC Restrictions
* 5 elastic IP addresses per vpc
* 5 internet gateways per vpc
* 5 vpcs per region (can be increased upon request)
* 50 vpn connections per region
* 50 customer gateways per region
* 200 route tables per region
* 100 security groups per region
* 50 rules per security group


