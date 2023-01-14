# Deploying-application-using-codedeploy

### INTRODUCTION

AWS CodeDeploy is a service provided by Amazon Web Services (AWS) that automates code deployments to servers. It allows developers to deploy code to various environments, such as test, staging, and production, with minimal downtime. CodeDeploy supports various languages and platforms, including Java, .NET, and PHP, and can deploy code to on-premises servers or servers in the cloud, including AWS EC2 and Lambda. 
CodeDeploy can be integrated with other AWS services, such as CodePipeline and CodeBuild, to automate the entire software release process.
An example of using AWS CodeDeploy to deploy an application would be as follows:
Create an application in CodeDeploy by specifying the name and type of the application.
Create a deployment group by specifying the environment (e.g., test, staging, production) and the target instances or servers.
Create a deployment configuration that specifies the deployment rules, such as the number of instances to update at a time, and the maximum number of failures allowed.
Prepare the application bundle by packaging the application code and any necessary files and configurations.
Upload the application bundle to an S3 bucket or GitHub repository.
Create a deployment by specifying the application, deployment group, and the location of the application bundle.
CodeDeploy will then perform the deployment by updating the specified instances or servers with the new code. The service will also monitor the deployment and rollback if necessary.
After the deployment is complete, you can use CodeDeploy's monitoring and reporting features to view the status and performance of the deployment.

In practice there could be many more steps and considerations, this example is just to give you an idea on how CodeDeploy works.

### prequisites

An aws account

IAM role with codedeploy access

terraform apply <https://github.com/Frankpromise/terraform-spacelift.git> to quickly set up an ubuntu server. 
When you clone the repo, make sure to switch branch to dev. Alternatively you can manually set up your ec2 instance. But make sure to install codedeploy agent
When server is up and running, ssh into the server.
git clone <https://github.com/Frankpromise/Deploying-application-using-codedeploy.git>

#### STEPS
create an s3 bucket

a. install awscli 

sudo apt install awscli

b. configure aws with your credentials

c. create a unique bucket name: 

```aws s3 mb s3://<insert bucket name> - profile default - region us-west-2```

d. Upload the code to the bucket using the bucket name created: 

aws s3 cp webapp.zip s3://test3456790
Navigate to your s3 and make sure read and write access is enabled on the bucket. If not, edit the bucket policy and paste this:

```{
 "Version": "2012–10–17",
 "Statement": [
 {
 "Effect": "Allow",
 "Principal": "*",
 "Action": [
 "s3:GetObject"
 ],
 "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
 }
 ]
}
```

##### Deploy application

Navigate to codedeploy in the aws console and click getting started.

click create application

set the following parameters:

applicatio name: webapp

compute platform: EC2/on-premises

4. click create application

5. On the next page, click create deployment group and set the following parameters:

a. Deployment group name: mydeploymentg

b. service role: click on iam role you created with codedeploy access

c. Deployment type: in-place

d. Envionment configuration: Amazon EC2 Instances

Key: select name

Value: prom-server(if you used my terraform template else use the tag for your ec2 instance).

e. Deployment settings: select CodedeploymentDefault.AllAtOnce

f. Load balancer: deselect the enable load balancing box

h. click Create deployment group

i. Click Create deployment

j. Copy and paste the URI of your s3 bucket in Revision location

3. Test deployment

After application has been deployed, navigate to the public ip address of your instance and test it.

your end result is this:

<img width="928" alt="image" src="https://user-images.githubusercontent.com/99150197/212492440-407e2277-2a0c-4f13-a010-ab0b472ad14e.png">

If you encounter any difficulties in making it work, drop a comment and i will try to help you.
