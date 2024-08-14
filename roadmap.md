
# Lambda Forge
Lambda Forge is an advanced and internally devised framework tailored to enhance the deployment and management of AWS Lambda functions, building upon a solid foundation established over one and a half years ago. It offers a streamlined process for automating the deployment pipeline from code retrieval to production. The framework ensures a consistent directory structure for Lambda functions, which is crucial for their effortless management, deployment, and scalability. Leveraging the AWS CloudFormation stack and the AWS CDK, Lambda Forge efficiently orchestrates the build, test, and deployment processes for code sourced from GitHub repositories across various environments.

One of the key features of Lambda Forge is its focus on best practices for code organization, testing, and integration, alongside a commitment to high-quality and maintainable code standards. It introduces a command-line interface tool, Gaia, that simplifies the initial setup, creating a standardized folder structure and pre-configuring essential settings for Lambda functions. This approach supports a modular code architecture, facilitating unit and integration testing, and importantly, the automatic generation of Swagger documentation from dataclasses defined within each Lambda function. This documentation feature enriches the framework, enabling developers to produce and access detailed API documentation effortlessly, further enhancing the development, management, and deployment process of Lambda functions with Lambda Forge.

### Before tapping into the full potential of the framework, there are some initial steps we need to complete.

## Install and Configure AWS CDK:

Execute the following commands to install the AWS CDK globally and set up your AWS credentials:

```
npm install -g aws-cdk
aws configure
```


You will be prompted to enter your AWS Access Key ID, Secret Access Key, default region name, and output format, granting the CDK access to manage your AWS resources. Skip this step if your credentials are already configured.

## Create a GitHub Personal Access Token:

To enable CodePipeline to interact with your GitHub repository, generate a GitHub personal access token by following these steps:

Navigate to "Developer Settings" in your GitHub account.
Select "Personal access tokens," then "Tokens (classic)."
Click "Generate new token," ensuring the "repo" scope is selected for full control of private repositories.
Complete the token generation process.

## Store the token on AWS Secrets Manager

Store this token in AWS Secrets Manager under the name **github-token**. It's crucial to use this exact name because that's the default identifier the CDK will search for in your AWS account.

### With all prerequisites in place, we are now ready to make use of the framework.

## Create a new directory:
```
mkdir samsung
cd samsung
````

## Create a new venv:
```
python3 -m venv venv
source venv/bin/activate
```
## Install lambda-forge from TestPYPI:
```
pip install lambda-forge --extra-index-url https://pypi.org/simple --extra-index-url https://test.pypi.org/simple/
````

## Create a new project:
```
forge project samsung --repo-owner "GuiPimenta-Dev" --repo-name "samsung" --bucket "gui-docs" --region us-east-2 --account 211125768252
````

## Create a new public function:

```
forge function hello_world --method "GET" --description "A simple hello word" --public
```

## Create a repository called samsung on GitHub:

```
git init
git add .
git commit -m "Initial commit"
git branch -M dev
git remote add origin git@github.com:GuiPimenta-Dev/samsung.git
git push -u origin dev
```

## Sync and deploy the dev stack to the cloud:

```
cdk synth
cdk deploy Dev-Samsung-Stack
````

# Go to the Pipeline section on the AWS Console and see the magic happening!

After the pipeline has successfully completed its execution, navigate to the AWS Lambda Functions console. Search for Dev-Samsung-HelloWorld among your Lambda functions.

Locate the API Gateway trigger associated with this function and click on the URL provided.

This action will lead you to a page demonstrating that we have successfully deployed a "Hello World" Lambda function, achieved with no more and no less than two commands.
