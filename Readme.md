Step 1 :
Create the Dockerfile

Step 2 : 
Create DockerHUB repo : <DockerHubAccount>/nginxappmine
Create Azure Container Registry repo : nginxappmine.azurecr.io
Create AWS Elastic Container Registry repo : nginxappmine

Step 3 :
Define Jenkins file to Build and push image to these repos .

step 4 :
Create an IAM user in AWS with all the necessary permissions to push docker image to ECR.
Fetch/Copy the ACCESS KEY , SECRET ACCESS KEY of IAM user and add these credentials to jenkins through 'Configgre Crerdentials' menu.
Fetch access tokens for ACR from the TOKENS menu.
Resources :
plugins : withCredentials, Git
Configuring ACR authentication : https://learn.microsoft.com/en-us/azure/container-registry/container-registry-repository-scoped-permissions
Confiuring ECR authentication : See the ECR page for PUSH COMMANDS .

<img width="736" alt="image" src="https://user-images.githubusercontent.com/96813823/226123303-795eb8e0-6811-4e3e-af48-a1939bc8ce7f.png">
