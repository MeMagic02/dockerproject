#Infrastructure Deployment Notes (AWS Console UI)

# 1. Docker Image Pulled

-PowerShell: docker pull nginx:latest

# 2. ECR Repository created

 - ECR > Create repository
 - Name: dockerapp (private)

# 3. Image tag and push to ECR 

 - PowerShell (variables):
 $region = "eu-central-1"
 $repoName = "dockerapp"
 $imageTag = "v1"
 $accountId = 
 $ecrUrl = 

#Authenticate Docker to ECR
aws ecr get-login-password --region $region | docker login --username AWS --password-stdin $ecrUrl

#Tag Image
docker tag nginx:latest "$ecrUrl:$imageTag"

#Push image
docker push "$ecrUrl:$imageTag"

# 4. ECS Cluster created

 - ECS > Clusters > Create cluster
 - Name: dockercluster
 - Type: Fargate

# 5. Task Definition created

 - ECS > Task Definitions > Create new
 - Name: dockertask:1
 - Type: Fargate
 - Container name: dockercontainer
 - Container port: 80
 - Image URI: (from ECR step)
 - Role: ecsTaskExRole

# 6. ECS Service with Public IP

 - ECS > Clusters > dockercluster > Create service
 - Task Definition:  dockertask:1
 - Service name: dockerservice2
 - Number of tasks: 1
 - Assign Public IP enabled
 - Subnets: 2 public
 - Security Group: HTTP (port 80)

# 7. Accesed the app via Public IP

 - ECS > Cluster > Service > Tasks > Public IP > Open in browser






















