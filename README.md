# codepipeline-node
This is an example of CodePipeline using CodeBuild for a Node application.  It will run tests for node, then create a Docker image from the application and push it to an ECR Repository.

## Usage

To integrate this workflow into your own Node project:

1. Copy the `Dockerfile`, `buildspec.yml`, and `cloudformation.yml` files to the root of your project.
2. Ensure `npm run lint` will run a linter (adjust desired rules for .eslintrc.json)
3. Ensure `npm run test` will run your unit tests
4. Ensure `npm start` will run your node application
5. Run the Cloud Formation script to create your stack.

Once this is done, CodePipeline will automatically create new builds when there are GitHub changes in the repo.

## Create Stack

The command below will create the CodePipeline stack. This stack will create the following resources:

1. S3 Bucket to hold build artifact
2. CodePipeline Role
3. CodeBuild Role
4. ECR Repository named after the Git Repository
5. CodeBuild Project for Linting/Tests
6. CodeBuild Project for Container Build
7. CodePipeline that integrates Source > Test > Container Build

To run this command, you will need fill in the necessary parameters

```
aws cloudformation create-stack --stack-name test-stack --template-body file://cloudformation.yml --capabilities CAPABILITY_IAM --parameters ParameterKey=GitHubToken,ParameterValue=<GitHub Token> ParameterKey=GitHubRepoOwner,ParameterValue=<GitHub Owner> ParameterKey=GitHubRepoName,ParameterValue=<Repo Name> ParameterKey=GitHubBranch,ParameterValue=<Branch Name> ParameterKey=S3ArtifactStore,ParameterValue=codepipeline-<Naming Convention>

```
