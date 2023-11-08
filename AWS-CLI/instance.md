State of instance

      0 : pending
      16 : running
      32 : shutting-down
      48 : terminated
      64 : stopping
      80 : stopped

list  instances
    
-       aws ec2 describe-instances

-       aws ec2 describe-instances --filters "Name=tag:Name,Values=MyInstance"

-       aws ec2 describe-instances --filters "Name=instance-type,Values=t2.micro" --query "Reservations[].Instances[].InstanceId"

-       aws ec2 describe-instances \
        --filters Name=tag-key,Values=Name \
        --query 'Reservations[*].Instances[*].{Instance:InstanceId,Name:Tags[?Key==`Name`]|[0].Value}' \
        --output table
    
    ![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/fcac3f01-6cd5-4583-acac-096fc209c837)


start instances

    aws ec2 start-instances --instance-ids i-1234567890abcdef0

stop instances
    
    aws ec2 stop-instances \
    --instance-ids i-1234567890abcdef0

hibernate mode

    aws ec2 stop-instances \
    --instance-ids i-1234567890abcdef0 \
    --hibernate


restart instances

    aws ec2 reboot-instances --instance-ids i-1234567890abcdef5

terminate  instance

    aws ec2 terminate-instances --instance-ids i-5203422c
