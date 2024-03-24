# AWS-cicd-simple-project
Simple AWS CI/CD project demonstrating automated deployment pipelines using AWS services like, Codecommit, CodeDeploy, CodeBuild, Codepipeline.

Architecture:



<img width="1254" alt="AWS-cicd-simple-project" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/d4145d12-00ab-4e5c-baa7-83020af592e4">




**Steps to perform: **

1. Establish a Repository in AWS CodeCommit:
   In my case Repo name is : mydemo-cicd
   
2. If not already created, generate an IAM user and obtain HTTPS Git credentials for AWS CodeCommit.
**Key Note:**  Ensure the IAM User is assigned the "AWSCodeCommitPowerUser" permission to enable Git clone and push operations.
(Navigate to IAM -> Users -> Select User -> Add permissions -> Attach policies directly -> 'AWSCodeCommitPowerUser')

3. Set up a project directory on your local machine, e.g., "cicd-project".

4. Clone the CodeCommit repository into the your local directory, you can do it by selecting "Clone HTTPS" at your Codecommit repository, provide Git credentials when it's prompted and clone the repository into your local directory.
   
   
5. Navigate into the Git project directory.

6. Optionally, configure Git using the following commands to avoid repetitive setup:

   <img width="728" alt="Screenshot 2024-03-24 at 6 06 58 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/2fd32c38-35ca-4ac0-a745-db9278d0f980">

7. If modifications to project files are necessary, you can do it accordingly and then execute the following commands:

   <img width="721" alt="Screenshot 2024-03-24 at 6 07 22 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/fc133419-0a85-4cc1-938b-2716a958e01e">

8. Now, you can verify the presence of the pushed code in the AWS CodeCommit repository.

9. Create an S3 bucket for storing build artifacts by goign to S3 console.

10. Deploy an EC2 instance with the following specifications:
    Public IP enabled
    Key pair required for instance access/login
    Security Group configured to open SSH, HTTP, and HTTPS ports (adjust for specific IP or open to any IP)

11. Install the AWS CodeDeploy Agent on the EC2 instance using the provided script.
    Copy the contents from EC2_script.txt,
    create a new file named EC2_script.sh on the EC2 instance,
    make it executable with below command,
    chmod +x EC2_script.sh
    execute the script withe below command
    ./EC2_script.sh
    **Note:**  Before execution, ensure the AWS region in the script (line no 6) matches your region.

12. Proceed to CodeBuild and create a CodeBuild Project with the following settings:
    Project name: you can give any name, in my case I have given "simple-cicd-demo"
    Source: AWS CodeCommit
    Repository: Select the your created repository
    Branch: main
    Provisioning model: On demand
    Environment image: Managed image
    Compute: EC2
    Operating system: Choose appropriate OS (in my case Ubuntu)
    Runtime: Standard
    Image: standard7.0
    Image version: Latest for the selected runtime version
    Buildspec: Use a Buildspec file
    Primary Artifact: Amazon S3
    Bucket name: Specify your bucket name carefully (case sensitive)
    Artifact packaging: Zip
    Service role permissions: Create a new Service Role (This automatically generates a new service role for CodeBuild)
    

13. Navigate to IAM and locate the role created for CodeBuild. Attach additional policies to this role, including:
    AmazonS3FullAccess (for uploading build artifacts to S3 bucket)
    Also, verify the "Trust Relationship" for this role to ensure it resembles the following:

    <img width="715" alt="Screenshot 2024-03-24 at 6 20 00 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/230603f8-879e-4459-84c7-6ba1e718e6f7">

14. "Establish a Build Project to monitor the status of builds comprehensively. Upon successful build execution, ensure artifacts are securely stored in an S3 bucket.


15. Navigate to IAM to generate a CodeDeploy service role, equipped with the following permissions. While it's possible to restrict permissions to the bare minimum, for simplicity, attach all relevant managed policies to this role. This role enables CodeDeploy and EC2 to execute all CodeDeploy operations seamlessly."

<img width="859" alt="Screenshot 2024-03-24 at 5 17 18 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/396bc97c-e254-46a1-af79-63fee633e647">

Also verify Trust Relationship for this role and it should be like below,

<img width="658" alt="Screenshot 2024-03-24 at 5 18 11 PM" src="https://github.com/DevOps-Article/AWS-cicd-simple-project/assets/73771495/eb75b65d-247e-422a-9429-f39cd74d7ecd">


16. Proceed to the CodeDeploy service and initiate the creation of a new application:
    Application Name: Choose a suitable name.
    Compute Platform: Select EC2.

17. Within the newly created application, navigate to the deployment group section and create a new deployment group:
    Deployment Group Name: Provide a distinctive name.
    Service Role: Choose an existing service role.
    Deployment Type: Opt for 'In place'.
    Environment Configuration: Specify 'Amazon EC2 instances'.
    Deployment Settings: Choose 'AllAtOnce'.
    Finally, click on 'Create Deployment Group'.

18 Move to the AWS CodePipeline service and commence the creation of a new pipeline:
   Pipeline Name: Assign a descriptive name.
   Pipeline Type: Select V2.
   Execution Mode: Opt for 'Queued' (Pipeline type V2 required).
   Service Role: Choose 'Create a new service role', ensuring to enable 'Allow AWS CodePipeline to create a service role' to facilitate its usage 
   with the new pipeline.
   Advanced Settings:
   Artifact Store: Default or custom S3 location for storing build artifacts.
   Encryption Key: Utilize AWS Managed Key.
   Sources: Specify the CodeCommit repository.
   Build: Choose AWS CodeBuild.
   Deploy: Select AWS CodeDeploy.
   Review and finalize the pipeline creation.

19. Monitor the stages of your pipeline, including Source, Build, and Deploy. A successful pipeline execution will result in the deployment of the 
   code onto EC2 instances.

**Testing:**
To test the application, acquire the public IP of the EC2 instance and verify its functionality via a web browser.
Additionally, you can introduce changes to your code, push them to CodeCommit, and observe the pipeline triggering automatic deployments.


Thank you,
DevOps Article
    
    
