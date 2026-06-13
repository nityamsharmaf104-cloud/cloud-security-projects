# CI/CD Pipeline with AWS CodePipeline and S3

Fifth project in my cloud security and DevOps series. This one shifts 
from pure security into DevOps territory — setting up a continuous 
deployment pipeline so every code push to GitHub automatically deploys 
to a live website on S3. No manual uploads, no console clicking after 
setup.

## What I Built

A CI/CD pipeline using AWS CodePipeline connected to a GitHub repository. 
Every time code is pushed to the main branch, the pipeline automatically 
detects the change and deploys the updated files to an S3 static website 
bucket. Tested by pushing a live change and watching the pipeline trigger 
and deploy automatically.

The application deployed is a meme memory matching game — forked from 
Tiny Technical Tutorials and used as the deployment target to keep 
focus on the pipeline infrastructure rather than the application code.

## Architecture

GitHub (source) → AWS CodePipeline → Amazon S3 (static website)

Any push to main branch triggers the pipeline automatically.

## Pipeline Stages

1. **Source** — CodePipeline watches the GitHub repo via GitHub App 
   connection. Triggers on every push to main.
2. **Deploy** — Pipeline deploys directly to S3 bucket configured 
   for static website hosting.

## Security Decisions

- Connected GitHub using **GitHub App** instead of OAuth — more secure, 
  doesn't require sharing GitHub credentials with AWS
- CodePipeline IAM service role scoped to only the permissions needed — 
  S3 write access to the specific bucket, nothing broader
- S3 bucket public access enabled only for website hosting — 
  GetObject allowed for all principals, no write access exposed
- All resources deleted after project completion — no idle infrastructure 
  left running

## What I Learned

The pipeline itself is straightforward to set up. The interesting part 
is understanding what's happening underneath — CodePipeline needs an 
IAM role to interact with S3, that role needs exactly the right 
permissions, and the GitHub connection needs to be scoped correctly. 
Get any of those wrong and the pipeline fails silently or with 
cryptic errors.

The bigger lesson is why this matters for DevSecOps: manual deployments 
are inconsistent and hard to audit. Every deployment through a pipeline 
is logged, traceable, and repeatable. That auditability is a security 
property, not just a DevOps convenience.

## Tools Used
- AWS CodePipeline
- Amazon S3
- AWS IAM
- GitHub (via GitHub App connection)
- AWS CodeConnections

## Resources Deleted After Completion
- CodePipeline (meme-pipeline)
- S3 bucket (nityam-meme-game)
- GitHub CodeConnection
- IAM service role (AWSCodePipelineServiceRole-ap-south-1-meme-pipeline)
