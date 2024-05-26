# Calculate_AWS_Daily_Cost
# For Centos
````sudo yum update -y
sudo yum install -y python3
python3 --version
yum install -y python3-pip
pip3 --version
````
# For Ubuntu
````sudo apt update -y
sudo apt upgrade -y
sudo apt install -y python3
python3 --version
sudo apt install -y python3-pip
pip3 --version
````
## Install AWS CLI
## Create Directory "AWS_Cost"
````
mkdir AWS_Script
````
## Give Permissions to file and directory
# Bash_Script
````
#!/bin/bash

# Variables
TODAY=$(date +"%Y-%m-%d")
YESTERDAY=$(date -d "yesterday" +"%Y-%m-%d")
PROFILE="abhipray"  # Replace with your AWS CLI profile name if needed

# Fetch costs
aws ce get-cost-and-usage \
    --time-period Start=$YESTERDAY,End=$TODAY \
    --granularity DAILY \
    --metrics "BlendedCost" \
    --query 'ResultsByTime[0].Total.BlendedCost.Amount' \
    --profile $PROFILE \
    --output text
                
````

# Python_Script
````
import boto3
import datetime
 
def get_aws_daily_cost():
    # Initialize AWS Cost Explorer
    client = boto3.client('ce', region_name='us-east-1')
 
    # Get yesterday's date
    end = datetime.datetime.utcnow().date()
    start = end - datetime.timedelta(days=1)
 
    # Retrieve cost data
    response = client.get_cost_and_usage(
        TimePeriod={
            'Start': start.strftime('%Y-%m-%d'),
            'End': end.strftime('%Y-%m-%d')
        },
        Granularity='DAILY',
        Metrics=['BlendedCost']
    )
 
    # Extract and return the cost
    return float(response['ResultsByTime'][0]['Total']['BlendedCost']['Amount'])
 
def save_cost_to_file(cost):
    with open('daily_cost.txt', 'a') as f:
        f.write(f"{datetime.datetime.utcnow().strftime('%Y-%m-%d')}: ${cost}\n")
 
if __name__ == "__main__":
    daily_cost = get_aws_daily_cost()
    print(f"Daily AWS Account Cost: ${daily_cost}")
    save_cost_to_file(daily_cost)
````
