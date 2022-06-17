# zpa_demo_aws
## Prequisite
- Prepare Zscaler Private Access license
- Create AWS IAM account who is assigned PowerUserAccess policy
- Subscribe Zscaler Private Access Connector on AWS

## Software Requirements
Install the following packages
- python3-pip
- ansible
- boto3
- awscli

## Overview
This will make following AWS environment for simple testing of ZPA by Ansible. Also App Connector will connect to ZPA Serivce Edge automatically.  

<img width="384" alt="image" src="https://user-images.githubusercontent.com/39214022/174228072-c4444a6f-6cb3-4d26-b26f-2f8b88ef9012.png">



## Directory structure
    ├── README.md
    ├── app-connector.yaml
    ├── aws.yaml
    ├── ec2.yaml
    ├── create_aws
    │   ├── tasks
    │   │   ├── main.yaml
    │   │   ├── igw.yaml
    │   │   ├── key_pair.yaml
    │   │   ├── nat_gw.yaml
    │   │   ├── route_table_private.yaml
    │   │   ├── route_table_public.yaml
    │   │   ├── sg.yaml
    │   │   ├── subnet_private.yaml
    │   │   ├── subnet_public.yaml
    │   │   └── vpc.yaml
    │   └── vars
    │       └── main.yaml
    ├── deploy_ec2
    │   ├── tasks
    │   │   ├── main.yaml
    │   │   └── ec2.yaml
    │   └── vars
    │       └── main.yaml
    ├── deploy_app-connector
    │   ├── tasks
    │   │   ├── main.yaml
    │   │   ├── asg.yaml
    │   │   └── launch_template.yaml
    │   └── vars
    │       └── main.yaml
    └── aws_vpc_delete
        ├── vpc_delete.yaml
        ├── tasks
        │   ├── main.yaml
        └── vars
            └── main.yaml

## Instrucsions
### Step1 Create AWS environment(VPC, Subnet, IGW, NGW, etc)
Open `create_aws/vars/main.yaml` and modify variables for your environment.　　
Sample variables are already defined, but need to define **tag** at least. 
Tag name is used for most of resource name and also attached tag to all of resource.
By default, these resource will be created on **ap-northeast-1**.  
  
Run the playbook.  
`ansible-playbook aws.yaml`

### Step2 Deploy App Connector with launch group and AutoScale group setting
Open `deploy_app-connector/vars/main.yaml` and modify variables for your environment.　　
This is also sample variables are already defined, but need to define **tag** and **provision_key** at least. 

Run the playbook.  
`ansible-playbook app-connector.yaml`

### Step3 Create EC2
Open `deploy_ec2/vars/main.yaml` and modify variables for your environment.　　
This is also sample variables are already defined, but need to define **tag** at least. 

Run the playbook.  
`ansible-playbook ec2.yaml`

## （Optional) Delete all of Demo environment when finish using this environment
 !!!　***This instruction is not recommended to use production and shared environment.*** !!!  
This playbook delete all of the resources(VPC, NATGW, IGW, EC2, App Connector, etc) which is attached specified tag. If any resouce which are not attached tag or attached another tag name are existed, it is not working.  
  
Move to *aws_vpc_delete* directory and open `vars/main.yaml` and modify variables for your environment.　　
This is also sample variables are already defined, but need to define **tag** at least. 

Run the playbook.  
`ansible-playbook vpc_delete.yaml`
