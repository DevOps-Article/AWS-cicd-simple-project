# AWS-cicd-simple-project
Simple AWS CI/CD project demonstrating automated deployment pipelines using AWS services like, Codecommit, CodeDeploy, CodeBuild, Codepipeline.



Architecture:

<img width="1254" alt="AWS-cicd-simple-project" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/d4145d12-00ab-4e5c-baa7-83020af592e4">


**Steps: **

1. Create a Repository in AWS CodeCommit:
   In my case Repo name is : mydemo-cicd
   
2. Create IAM user if you have not created already and then creates HTTPS Git credentials for AWS CodeCommit
   **Important point:** Attach "AWSCodeCommitPowerUser" permission to the IAM User so that you can perform git clone, git push activities 
   (IAM -> Users -> SelectUser -> Add permissions -> Attach policies directly -> 'AWSCodeCommitPowerUser')

3. Create a project directory on your local machine e.g.  cicd-project

4. Clone the Codecommit repository in to cicd-project directory. It will ask you Git credentails, you can provide that and clone the repo.
   
5. Go inside the Git project directory

6. You can configure your git by using below commands, so that you don't have to repeat the same commands.
   git config --global user.name <YOUR ACTUAL NAME HERE>      [To Set global git user name]
   git config --global user.email <YOUR ACTUAL EMAIL HERE>    [To Set global git user email]

   git config --local  user.name <YOUR ACTUAL NAME HERE>      [To Set local git user name]
   git config --local  user.email <YOUR ACTUAL EMAIL HERE>    [To Set local git user email]
   

7. If you want to do any changes in the project files you can do it and then you can initiate below commands,
   git status
   git add
   git commit -m "project files comiited"
   git push
   
8. Check the AWS Codecommit repository and the code that you have just pushed.

9. Create S3 bucket to store the build Artifacts and copy the erns from bucket properties.

10. Go to CodeBuild and create CodeBuild Project  
    In my case Codebuild project name is:  simple-cicd-demo
    Source- AWS Codecommite
    Repository- Select your created repository
    Branch-  main
    Provisioning model- on demand
    Environment image- Managed image
    Compute-  EC2
    Operating system-  Select accordingly (in my case I am using Ubuntu )
    Runtime(s) standard
    Image- standard7.0
    Image version-  Always use latest images for this runtime version
    Buildspec-  Select use a Buildspec files
    Artifact 1 - Primary-  Amazon s3
    Bucket name-  give your actual bucket name (carfully, it is case sensative)
    Artifacts packaging-  Select Zip
    Service role permissions-   Select Create New Scervice Role (It will automatically create new service role for CodeBuild)
    

11. Now, Go to IAM and find the role which created for Codebuild, You have to attach additional policies to this role like below:
    AmazonS3FullAccess (to upload build artifacts on S3 bucket)
    Also verify "Trust Relationship" for this role, it should be like below,

 
       
<img width="653" alt="Screenshot 2024-03-24 at 5 11 47 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/226632be-b7c6-4843-acff-7a4bac2a0b69">
