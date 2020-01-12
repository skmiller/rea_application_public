# Sinatra Application for REA

Application for Sean Miller to REA

## What's Needed
- An AWS Account
- Optional: AWS CLI access

## How to run
### Option 1
AWS Console
- Upload included Cloudformation file into AWS
- Update Parameter to use your account's default VPC (or whatever VPC you choose)
- Retrieve the IP address from the "Outputs" tab and enter it into the browser. Voila!

### Option 2
AWS CLI
Assumptions Made:
- You have AWS CLI access configured on your machine
- You are running in a bash console

Steps:
- Navigate to this folder, or wherever you've stored the Cloudformation file and run this command to create the stack (any of these arguments can be changed as needed)
    ```aws cloudformation create-stack --stack-name sinatra-simple-deployment --template-body file://sinatra_cloudformation.yaml --parameters ParameterKey=VPC,ParameterValue=vpc-7dd04705 --region us-west-2```
- Check on the stack status by running the command
	```aws cloudformation describe-stacks --stack-name sinatra-simple-deployment --region us-west-2```
- If StackStatus is listed as "CREATE_COMPLETE", then retrieve the IP address from "Outputs"
- Either enter in that IP address in the console, or perform a simple curl on it. Fin.


## How to access the Sinatra App
Once the Cloudformation is finished creating, take the newly created EC2 instance's IP address from the "Outputs" tab. Put it into the browser and it will give you the "Hello World" Sinatra response.

## Why I decided to use this route
A cloudformation with a simple UserData script seemed the easiest route to me. This cloudformation will allow the user to have everything they need to run the code with only two CLI commands. I was able to specify the SecurityGroup to only allow port 80 access. No SSH access to the instance is allowed. There is also no IAM Instance Role assigned to the instance, so it has no way to access other AWS resources. It should be effectively orphaned and only able to return this "Hello World!" response. 

A shortcoming would perhaps be if the github repository with the Sinatra code went down, then there would be nothing to pull from.
