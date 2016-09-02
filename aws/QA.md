 On-Demand  Reserved Instance
 On-Demand  let you pay for your computing capacity needs by the hour

 Spot Instances - decide on a maximum and minimum hourly price and bid for it  These are spare unused Amazon EC2 instances that you can bid for. Once your bid exceeds the current spot price (which fluctuates in real time based on demand-and-supply) the instance is launched. The instance can go away anytime the spot price becomes greater than your bid price.
 Spot instances allow you to bid for unused capacity and save up to 90% of EC2 costs. 

 You can decide your optimal bid price based on spot price history of last 90 days available on AWS console or through the EC2 APIs.

 Advantage you get over reserved and on-demand instances is they are cost effective. Downside is, they can be interrupted anytime. Therefore, these are more suitable for 
 1. flexible workloads that can be run at any point in time when you have enough computing capacity 
 2. tasks that can use some extra computing capacity to enhance the performance
 3. optional nice to have tasks.

Command will request a spot block for 1 instance running 1 hours with a maximum price of USD 0.15 per hour.
aws ec2 request-spot-instances --block-duration-minutes 60 --instance-count 1 --spot-price "0.15" ..


The cost of RI’s is determined by 3 main parameters 
1. Instance type: 
2. Region/Availability Zone:
3. Utilization level: 
aws ec2 describe-instances --region=us-east-1 \
--filters Name=instance-state-name,Values=running Name="instance-type,Values=m3.xlarge" \
--output json | jq -r '.Reservations[].Instances[].Placement.AvailabilityZone' | sort | uniq -c

