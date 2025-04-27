# terraform
we use terraform to build a infrastructure in aws/azure/gcp. 
It is IaC (Infrastructure as code) tool.  
we use Harshicorp configuration language to write it.  

# cloud alternatives for terraform
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

# defining variable block

1) Instance

2)Count - to create number of instances        

3)Map/Set - to provide tags to instance created                   
 use type = map{string}

4)List - to create IAM users            
type = list{string}
