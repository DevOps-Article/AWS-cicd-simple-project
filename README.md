# AWS-cicd-simple-project
Simple AWS CI/CD project demonstrating automated deployment pipelines using AWS services like, Codecommit, CodeDeploy, CodeBuild, Codepipeline.



Architecture:

<img width="1254" alt="AWS-cicd-simple-project" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/d4145d12-00ab-4e5c-baa7-83020af592e4">


Steps: 
1. Create a Repository in AWS CodeCommit:
   Repo name: simpleawscicdproject
   
2. Create IAM user if you have not created already and then creates HTTPS Git credentials for AWS CodeCommit
   **Important point:** Attach "AWSCodeCommitPowerUser" permission to the IAM User so that you can perform git clone, git push activities 
   (IAM -> Users -> SelectUser -> Add permissions -> Attach policies directly -> 'AWSCodeCommitPowerUser')

3. Create a project directory on your local machine e.g.  cicd-project

4. Clone the Codecommit repository in to cicd-project directory. It will ask you Git credentails, you can provide that and clone the repo.
   
6. Go inside the Git project directory

7. 
