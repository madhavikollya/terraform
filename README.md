# Terraform

Here i am introducing you all to terraform and do check all the codes mentioned separately as folders.            

- Terraform is a magical third party tool. we use terraform to build a infrastructure as code in aws/azure/gcp. It is IaC (Infrastructure as code) tool. It is written in Harshicorp configuration language.                           

# Cloud alternatives for terraform
AWS- Cloud formation template (JSON/YAML)                     
AZURE- ARM Templates                    
GCP- GDE               

# 3 important Blocks
Provider , region info

Resource configuration

Variable declaration

# To download terraform on linux

wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

- Go to terraform install on google and select which ever machine u want to!
- After installing terraform on amazon linux             
* aws configure            
* access key         
* secret key       
* Region        
* Format

# Defining variable block

1) Instance               
  variable - "instance_type"                  

2) Count - to create number of instances             
   variable - "count_servers"                     

3) Map/Set - to provide tags to instance created                   
 variable - "instance_tags"            
 use type = map{string}

4) List - to create IAM users            
  variable - "iam_users"             
  type = list{string}

we can create variables separately as variable.tf files also.        
If we want to change the infrastructure we can do it in variable.tf           

# Multi tf var files

Tf var files :- When we want use same main.tf with different variables. For example lets create                        
- vim main.tf                
- vim variable.tf                     
- vim swiggy.tfvars , vim zomato.tfvars - here we define all the values required for creating infrastructure.              
- when we give terraform apply by changing the file names overridding may occur.           
- so for this we have to use workspaces.                 
create different workspaces for each infrastructure and then try terraform apply. Then no overriding will take place.         

# Commands used

- terraform init -  To initialize ang install all plugins.          
- terraform validate - will chesk the configuration.              
- terraform plan - will execute and also check the code errors.             
- terraform apply - will check values and show errors during runtime.            
- terraform destroy - to destroy the infrastructure.          
- terraform apply --auto-approve - permission given prior to directly create infrastructure.           
- terraform state list - will show the  running resources list                        
- terraform destroy --auto-approve -target="aws_instance.one[0]" - to destroy particular instance. to destroy 2 instances 2 times target, instance 
   name should be given.                  
- terraform workspace show - to see current workspace and take you into it directly.           
- terraform workspace new swiggy - to create new workspace.            
- terraform workspace list - will show the list of workspace.           
- terraform workspace select swiggy - to go into that particular workspace.           
- terraform apply --var-file="swiggy.tfvars" --auto-approve - to create swiggy.tfvars
- terraform import - used to import the resources or existing infrastructure which are created outside the terraform.                     

# To delete workspaces

* To delete swiggy workspace, first go into the                      
- terraform workspace select swiggy                  
- terraform destroy --var-file="swiggy.tfvars" --auto-approve             
- now we can't directly delete swiggy workspace by staying inside it. so come out of it.          
- terraform workspace select default                 
- terraform workspace delete swiggy                    
 In this way we have to delete all the workspaces we created.              

# Locals
we can define locals once and can be changed multiple times.         
differnce between locals and variables.                  

* Locals-         
-Should always be mentioned only in main.tf files         
-can"t be changed once defined.       
-can be used multiple times.           
-values which are static should be kept here.           

* Variables-      
-can also be written as a separate file.              
-will change time to time.                           
-vaues which are dynamic should be kept here.                        

# Terraform CLI

we use it to pass values from commandline during the runtime. Bymistake if we provide wrong information in the command line, it compares with statefile and will replace with right one. For example if you give instance type as t2.medium instead of t2.micro, terraform will first refresh, then compares with statefile finds a right insstance type and then deletes it.                        
terraform apply --auto-approve -var="instance_type=t2.micro" - will create instance                

# Terraform outputs variables

Used to show metadata of resources. While creating resources if you want to print the metadata belonged to it the we can use output.           

# Loops Concept

- It will take duplicate values and will show error.                   
- Here if we try to create same iam user twice the terraform will first accept it and later rejects it and will show it as a error.                   
type = list(string)

# For_each loop

- It will eliminate the duplicate value.             
- It is not like loop. It doesn't even accept same iam user twice, it rejects and will not show error because it will not take the input.        
 type =set(string)   or   type = map(string)            

# Alias & Providers

- From one main.tf file we can create 2 server resources in different regions at same time using alias & providers.         
- Provider is a plugin, that helps terraform to understand where the project has to be created.              

# RBAC (Role Based Access Control)

providing permissions to IAM users to create or delete s3 bucket, EC2, etc.                    

# S3 Backend setup and statefile

- In terraform We need to store the statefile because all the information we provide in code is stored/present in statefile.             
- we can store statefile in s3 bucket, Dynamo db, Vault etc. so that nobody can access it.             
- If we want to update the tffile by adding some tags & give terraform apply, it will go to statefile & check the infrastructure that is already created. If we didn't store the statefile it will create a new infrastructure. (If there is no statefile terraform will never know that it has to update infra, rather it creates a new one).           
- devops eng. -> will modify the code ->  terraform init -> terraform apply -> automatically the statefile is also updated in s3bucket


# Dynamic Block        

- Its purpose is reusablity of code.              
- If we write a code, want to use it again and again.                 
- For example, this might be most useful for creating security groups.          
- Generally we write a code to create an instance & set up security group,so to skip rewriting the ingress and egress block we write only one dynamic block and use for_each loop to take block by block information.                        

Mandatory commands:-
* terraform init -reconfigure
* terraform init -migrate-state
* terraform plan
* terraform apply --auto-approve       

# EFS (Elastic File System)

- It is chargeable.
- used for storage sharing purpose.
- Generally we have to maintain 2 servers(appserver,webserver,DB) in different regions for safety purpose, so if we upload the content in one server it will automatically replicate in other server also. (Browse terraform efs code -> copy&paste -> EFS created).                          
- Goto EFS in AWS -> attach -> mount via DNS -> copy the command -> paste it on terminal                    
  * Enable EFS on AWS for the content to get copied.


# Modules     

- Similar to ansible roles.                       
- Here main.tf file is divided into multiple subfiles using module.                    
- In module we can create multiple resources.              
- It is not a best practise
               
create --
 * vim provider.tf                  
 * vim main.tf
 * mkdir -p modules/instance
 * mkdir -p modules/buckets
 * vim modules/instances/main.tf
 * vim modules/instances/variable.tf
 * vim modules/buckets/main.tf
 * vim modules/buckets/variable.tf             
   
- In variable we just mention the type of variables                
- values are given in main.tf                      

# meta arguments

- Generally we create EC2 server, if we give terraform apply & destroy on terminal it acts accordingly. But here in the code we mention prevent_destroy=true in lifecycle, now if we give terraform destroy it does't work.            
- we also have ignore_changes lifecycle. Even if some one change the "name" in code it doesn't reflects in instance.
- for example - give (Name = db_server), and give terraform apply. It shows the changes on cli, bt doesn't reflect the changes in real world infrastructure.                              
