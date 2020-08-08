# aws

# find elasticIps based on name 
aws ec2 describe-addresses --filters "Name=tag:Name,Values=$VALUE" --region us-east-1 --query 'Addresses[*].PublicIp[]'

# terminante instances from output
aws ec2 describe-instances --filters "Name=tag:aws:autoscaling:groupName,Values=$VALUE" --query 'Reservations[*].Instances[*].[InstanceId]' --output text | xargs -I '{}' aws ec2 terminate-instances --instance-ids '{}'

# aws attach instances to elb:
aws --profile prod --region us-east-1 elb register-instances-with-load-balancer --load-balancer-name $LOAD_BALANCER_NAME --instances $INSTANCE_IDS

# aws describe instances by tags and get InstanceID
aws ec2 describe-instances --region us-east-1 --filters "Name=tag:ROLE,Values=$VALUE" | grep InstanceId

# RDS
aws rds describe-db-instances --query 'DBInstances[*].[Engine,EngineVersion]'

# EC2
aws elb describe-load-balancers --load-balancer-names $ELB_NAMES
aws ec2 describe-instance-status --instance-ids $INSTANCES

aws ec2 describe-instances --filters 'Name=tag:Name,Values=$VALUE'

# ELB
# find load balancer by instance
aws elb describe-load-balancers --output text --query  'LoadBalancerDescriptions[?Instances[?InstanceId==`$INSTANCE_ID`]].[LoadBalancerName]'
