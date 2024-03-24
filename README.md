# AWS-cicd-simple-project
Simple AWS CI/CD project demonstrating automated deployment pipelines using AWS services like, Codecommit, CodeDeploy, CodeBuild, Codepipeline.



Architecture:

<img width="1254" alt="AWS-cicd-simple-project" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/d4145d12-00ab-4e5c-baa7-83020af592e4">


**Steps: **

1. Establish a Repository in AWS CodeCommit:
   In my case Repo name is : mydemo-cicd
   
2. If not already created, generate an IAM user and obtain HTTPS Git credentials for AWS CodeCommit.
**Key Note:**  Ensure the IAM User is assigned the "AWSCodeCommitPowerUser" permission to enable Git clone and push operations.
(Navigate to IAM -> Users -> Select User -> Add permissions -> Attach policies directly -> 'AWSCodeCommitPowerUser')

3. Set up a project directory on your local machine, e.g., "cicd-project".

4. Clone the CodeCommit repository into the your local directory, you can do it by selecting "Clone HTTPS" at your Codecommit repository, provide Git credentials when it's prompted and clone the repository into your local directory.
   
   
6. Navigate into the Git project directory.

7. Optionally, configure Git using the following commands to avoid repetitive setup:

   
   

8. If you want to do any changes in the project files you can do it and then you can initiate below commands,
   git status
   git add
   git commit -m "project files comiited"
   git push
   
9. Check the AWS Codecommit repository and the code that you have just pushed.

10. Create S3 bucket to store the build Artifacts and copy the erns from bucket properties.

11. Creates Ec2 instance:
    public IP: enabled,
    Key pair: required to login to instance
    Security Group- open ssh, http and https ports (you can open with anywhere or select my ip only)

12. Install AWS Code Deploy Agent on Ec2 with the given script.
    You can copy all data from EC2_script.txt, login to Ec2 and make the new file called EC2_script.sh, make this file executable by giving permission
    e.g.
    chmod +x EC2_script.sh and then run this file with the command
    ./EC2_script.sh
    IMP: before run, make sure aws region is correct to yours if not you can change it accordingly on line no 6 of the EC2_script.sh file.

13. Go to CodeBuild and create CodeBuild Project  
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
    

14. Now, Go to IAM and find the role which created for Codebuild, You have to attach additional policies to this role like below:
    AmazonS3FullAccess (to upload build artifacts on S3 bucket)
    Also verify "Trust Relationship" for this role, it should be like below,

 
       
<img width="653" alt="Screenshot 2024-03-24 at 5 11 47 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/226632be-b7c6-4843-acff-7a4bac2a0b69">

14. Create Build Project and check all the status of Build, also if buld runs successfully artifacts shoud be stored on S3 bucket


15. Go to IAM and create Code Deploy service role, this role should have below permissions attahed. (ofcourse it can be restricted to least) but for simplicity you can attach all below managed policies. THIS ROLE ALLOW CODEDEPLOY & EC2 TO PERFORM ALL CODE DEPLOY OPERATIONS

<img width="859" alt="Screenshot 2024-03-24 at 5 17 18 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/396bc97c-e254-46a1-af79-63fee633e647">

Also verify Trust Relationship for this role and it should be like below,

<img width="658" alt="Screenshot 2024-03-24 at 5 18 11 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/eb75b65d-247e-422a-9429-f39cd74d7ecd">


16. Go to Code Deployment and creates:
     Aew Application:  give any names
     Compute platform:  Ec2

17. Go inside the application, select application and Create deployment group
    Deployment group name:   give any names
    Service role: Select service role that already we have created
    Deployment type: In place
    Environment configuration:  Amazon EC2 instances
    Deployment settings:  Select AllAtOnce
    Finally- click on create Deployment group.

18 Go to Pipeline and create new pipeline,
   Pipeline name: give any name
   Pipeline type: V2
   Execution mode: Queued (Pipeline type V2 required)
   Service role: Seletc New service role (it will automatically create new role and required permissions) make sure to enable Allow AWS CodePipeline 
   to create a service role so it can be used with this new pipeline
   Advanced settings: Artifact store: You can choose default or custome location of S3 to store build artifacts
   Encryption key:  AWS Managed Key
   Sources:  Give the codecommit repository
   Build:  Select AWS CodeBuild
   Deploy: Select AWSCodeDeply
   Review and finally creates the pipeline.

19. Monitor your piple line stages like, Source, Build & Deploy, if everything well then pipeline should be success and code
    Should be deploy on Ec2.

Testing:
Now, for testing you can take Ec2 public IP and check the application is working or not in web the browser.
Additionally- you can make a changes in your code, push the code to codecommite and pipeline should trigger automatically.


Thanks
Devops Artile
    
    
