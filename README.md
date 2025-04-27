# Terraform
we use terraform to build a infrastructure in aws/azure/gcp. 
It is IaC (Infrastructure as code) tool.  
we use Harshicorp configuration language to write it.  

# Cloud alternatives for terraform
AWS- Cloud formation template (JSON/YAML)

AZURE- ARM Templates

GCP- GDE

# 3 important compartments
Provider , region info

Resource confiruration

Variable declaration

# To download terraform on linux

wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform

Go to terraform install on google and select which ever machine u want to!

aws configure            
access key         
secret key       
Region        
Format

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
create different workspaces for each infrastructure and then try terraform apply. now no overriding take place.         

# Commands used

terraform init -  To initialize ang install all plugins.          
terraform validate - will chesk the configuration.              
terraform plan - will execute and also check the code errors.             
terraform apply - will check values and show errors during runtime.            
terraform destroy - to destroy the infrastructure.          
terraform apply --auto-approve - permission given prior to directly create infrastructure.           
terraform state list - will show the list                        
terraform destroy --auto-approve -target="aws_instance.one[0]" - to destroy particular instance. to destroy 2 instances 2 times target, instance name should be given.                  
terraform workspace show - to see current workspace and take you into it directly.           
terraform workspace new swiggy - to create new workspace.            
terraform workspace list - will show the list of workspace.           
terraform workspace select swiggy - to go into that particular workspace.           
terraform apply --var-file="swiggy.tfvars" --auto-approve - to create swiggy.tfvars                    

# To delete workspaces

* To delete swiggy workspace, first go into the         
-terraform workspace select swiggy           
-terraform destroy --var-file="swiggy.tfvars" --auto-approve         
-now we can't direct delete swiggy workspace by staying inside it. so come out of it.
-terraform workspace select default
terraform workspace delete swiggy
 In this way we have to delete all the workspaces we created.      

# Locals
we can define locals once and can be changed multiple times.         
differnce between locals and variables.                  

*Locals-         
-Should always be mentioned only in main.tf files         
-can"t be changed once defined.       
-can be used multiple times.           
-values which are static should be kept here.           

*variables-      
-can be written separately also.         
-will change time to time.                    
-vaues which are dynamis should kept here.                   

# Terraform CLI

we use it to pass values from commandline during the runtime. Bymistake if we provide wrong information, it compares with statefile and replace with right one. For example if you give instance type as t2.medium instead of t2.micro, terraform will first refresh, then compares with statefile and the deletes.                  
terraform apply --auto-approve -var="instance_type=t2.micro" - will create instance             

