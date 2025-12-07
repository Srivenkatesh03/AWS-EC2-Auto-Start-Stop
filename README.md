🟢 EC2 Auto Start/Stop Automation (Python + AWS + Cron)

This project automatically starts and stops AWS EC2 instances at scheduled times using:

Python (boto3)

AWS CLI

IAM with least-privilege policy

Cron job scheduler

Linux (Arch / Ubuntu)

This saves cloud cost by ensuring EC2 runs only when needed.

🚀 Features

Automated EC2 start at fixed time

Automated EC2 stop at fixed time

Python boto3-based EC2 control

Cron-based scheduling

Logs stored locally

Clean architecture and secure IAM usage

Production-ready folder structure

🏗 Architecture
Cron ---> Python Script ---> boto3 ---> AWS EC2 API ---> EC2 Instance


See docs/architecture-diagram.txt.

🧩 Python Script

Path: scripts/start-stop-ec2.py
```bash
import boto3

REGION = "ap-south-1"
INSTANCE_IDS = ['i-0b6fda8837abee2e0']

ec2 = boto3.client('ec2', region_name=REGION)

def start_instances():
    print("Starting EC2 instances...")
    ec2.start_instances(InstanceIds=INSTANCE_IDS)

def stop_instances():
    print("Stopping EC2 instances...")
    ec2.stop_instances(InstanceIds=INSTANCE_IDS)

if __name__ == "__main__":
    import sys
    if len(sys.argv) != 2:
        print("Usage: python3 start-stop-ec2.py start|stop")
        exit()

    action = sys.argv[1]

    if action == "start":
        start_instances()
    elif action == "stop":
        stop_instances()
    else:
        print("Invalid command: start or stop")
```
🛠 Setting Up Cron

Edit cron:
```
crontab -e
```

Add these:
```
0 9 * * * /usr/bin/python3 /home/sri/Downloads/ec2_automation/start-stop-ec2.py start >> /home/sri/Downloads/ec2_automation/log.txt 2>&1
0 21 * * * /usr/bin/python3 /home/sri/Downloads/ec2_automation/start-stop-ec2.py stop >> /home/sri/Downloads/ec2_automation/log.txt 2>&1
```
🔐 IAM Permission (Least Privilege)
```
Create custom IAM policy:

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances",
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    }
  ]
}
```
🧪 Testing

Start EC2:
```
python3 start-stop-ec2.py start
```

Stop EC2:
```
python3 start-stop-ec2.py stop
```
Check logs/log.txt for results.
