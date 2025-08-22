# cloudpipe-deploy

<p align="center">
<img src="https://i.imgur.com/6h47eNz.png"/>
</p>

Automated CI/CD pipeline for deploying static websites to AWS S3.

[![pages-build-deployment](https://github.com/iibluearth/cloudpipe-deploy/actions/workflows/pages/pages-build-deployment/badge.svg)](https://github.com/iibluearth/cloudpipe-deploy/actions/workflows/pages/pages-build-deployment)

<h1>Project Overview: Automating Deployments for CloudPipe</h1>
<h2> 1. Understanding the Client's Vision </h2>
<i>CloudPipe is a small web development company that specializes in building websites for local businesses. Their developers currently rely on manual deployment methods, uploading files directly to production servers whenever changes are made.

This approach is slow, error-prone, and stressful, often leading to inconsistencies between code and production.

CloudPipe’s vision is to modernize its workflow by implementing a basic CI/CD pipeline. By connecting their GitHub repository to automated deployments on AWS S3, every code push will trigger a reliable and consistent release. This will allow their team to focus on development instead of deployment.</i>

<h2> 2. The Problem Landscape </h2>
<h3> Manual Deployment Issues </h3>

Currently, every update requires developers to: 

1. Download the latest files from GitHub.

2. Manually upload them to the production server.

3. Verify changes while hoping nothing was forgotten.

This manual approach often introduces human error.

<i>Example: Last week, a developer forgot to upload a JavaScript file, breaking the contact form for several hours.</i>

<h3> Time Wastage: </h3>

- A small change (e.g., updating a text string) can take 15–20 minutes to deploy.
- This is inefficient, especially when deployments should take seconds with automation.

<h2> 3. Solution Requirements </h2>

CloudPipe needs a simple, reliable CI/CD setup that eliminates manual steps.

<h3> Core Requirements: </h3>

- Automated Deployment: Every GitHub push triggers a deployment.
- Deployment Visibility: Developers can see if the pipeline succeeded or failed.

<h3> Infrastructure Needs: </h3>

- AWS S3: To host static websites securely and scalably.
- GitHub Actions: To automate build and deployment pipelines.
- Deployment Logs: For easy troubleshooting and tracking.

<h2> 4. Why These Requirements Matter </h2>

- Developer Productivity: Focus shifts from repetitive deployment to feature development.

- Consistency: Every deployment follows the same automated steps, reducing human error.

- Reliability: Ensures updates are delivered consistently, without missing files.

- Transparency: The team can monitor pipeline runs and know exactly when/why failures occur.

<h2> 5. Success Criteria </h2>

The solution will be considered successful if:

1. Deployments occur automatically whenever code is pushed to GitHub.

2. Developers receive immediate feedback if a deployment fails.

3. Website updates are delivered reliably without missing files.

4. Deployment speed is significantly faster than manual uploads.

<h2> 6. Conclusion </h2>

This project focuses on building a streamlined foundation for CloudPipe’s delivery workflow:

- Code commit → Automated pipeline → Live website.

By removing manual steps and introducing automation, CloudPipe will achieve faster, more reliable, and less stressful deployments.
- Error Notifications: The system alerts the team if something goes wrong.

<h1> CloudPipe-Deploy Project Setup & CI/CD Pipeline </h1>

<h2> Phase 1: Project Initialization </h2>
<h4> 1. Create a GitHub Repository </h4>

1. Create a new repository called cloudpipe-deploy
2. Clone the repository locally:
`git clone https://github.com/<your-username>/cloudpipe-deploy.git`
`cd cloudpipe-deploy`

4. Create a basic `README.md` describing your project:

<h4> 2. Set Up Project Structure </h4>
cloudpipe-deploy/

├── website/                # Website files

├── .github/workflows/      # GitHub Actions workflow

├── infrastructure/         # CloudFormation template

└── README.md

<h4> 3. Initialize Git Locally </h4>

<p>
<img src=https://i.imgur.com/uyOliZR.png/>
</p>

<h2> Phase 2: Infrastructure Setup </h2>

<h4> 1. AWS Configuration </h4>

- Configure your AWS CLI: `aws configure`
- Create AWS IAM access keys for GitHub Actions and note the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`

<h4> CloudFormation Template </h4>

Inside the `infrastructure/` folder, create `template.yaml` for S3 hosting:

<p>
<img src=https://i.imgur.com/yyoJ5N8.png/>
</p>

<p>
<img src=https://i.imgur.com/gkNDBaj.png/>
</p>

- Add bucket policies for public access.
- Enable Static Website Hosting in the template.

<h2> Phase 3: Pipeline Development </h2>

<h4> 1. GitHub Actions Workflow </h4>

Create `.github/workflows/deploy.yaml`

<p>
<img src=https://i.imgur.com/gLBljbj.png/>
</p>

<h4> 2. Repository Configurations </h4>

- Add AWS secrets (AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY) to GitHub.

<p>
<img src=https://i.imgur.com/u2gDsrO.png/>
</p>

- Configure branch protection rules for main.

<p>
<img src=https://i.imgur.com/ClZKhn7.png/>
</p>

- Add status badges in README.md.

<p>
<img src=https://i.imgur.com/1zV2XVS.png/>
</p>

- Configure basic error notifications.

  ` - name: Notify on failure`
  
  `if: failure()`
  
  `run: echo "Deployment Failed"`

<h2> Phase 4: Integration and Testing </h2>

<h4> 1. Local Testing and AWS Deployment </h4>

- Test website locally
- Verify AWS CLI access and bucket permissions
- Deploy CloudFormation template

<p>
<img src=https://i.imgur.com/p4wrQXz.png/>
</p>

<p>
<img src=https://i.imgur.com/okoIRF3.png/>
</p>

<p>
<img src=https://i.imgur.com/Sl35uJM.png/>
</p>

<h2> Phase 5: Cleanup </h2>
When finished or for practice teardown:

- `aws cloudformation delete-stack --stack-name cloudpipe-website-kk-123`

<h1> Troubleshooting Guide - Static Website on AWS S3 + GitHub Actions </h1>

<h2> 1. CloudFormation Issues </h2>

<h3> Problem: Stack stuck in ROLLBACK_COMPLETE </h3>

- Cause: Invalid resource configuration (wrong bucket policy, missing principal, invalid property).

- Fix:

1. Check the Events tab in CloudFormation → see which resource failed.

2. Update your template and re-deploy.

3. If stack is ROLLBACK_COMPLETE, delete it and re-deploy.

<h3> Problem: Missing required field Principal (S3 BucketPolicy) </h3>

- Cause: You must define Principal under your Statement

<h3> Problem: Bucket name already exists </h3>

- Cause: S3 bucket names are globally unique across AWS.
- Fix: Use a unique name, e.g., cloudpipe-website-kk-12345.

<h2> 2. S3 Bucket Issues </h2>

<h3> Problem: aws s3 sync → An error occurred (NoSuchBucket) </h3>

- Cause: Bucket not created or name mismatch.
- Fix:

1. Confirm bucket exists
2. Make sure your s3://bucket-name matches CloudFormation.

<h3> Problem: Website shows “Access Denied” </h3>

- Cause: Public access not allowed.
- Fix:

1. Add bucket policy
2. Ensure Block Public Access is disabled in the console.

<h3> Problem: Website loads HTML but not CSS/JS </h3>

- Cause: Wrong folder structure.
- Fix:

1. Ensure your files are uploaded as:
website/
  index.html
  css/
  js/

2. Check S3 console that css/style.css exists.

<h2> 3. GitHub Actions Issues </h2>

<h3> Problem: GitHub Action fails at AWS credentials </h3>

- Cause: Secrets not set or misnamed.
- Fix:

1. Go to repo → Settings → Secrets → Actions.

- Add:

- `AWS_ACCESS_KEY_ID`

- `AWS_SECRET_ACCESS_KEY`

2. Ensure names match your workflow file.

<h3> Problem: GitHub Action runs, but files don’t upload </h3>

- Cause: Wrong bucket name in workflow.
- Fix: Update the line to match your bucket name.

<h2> 4. Local Testing Issues </h2>

<h3> Problem: Site looks fine locally, broken on S3 </h3>

- Cause: Case sensitivity.
- Fix:

- `index.html` `≠` `Index.html.`

- Rename files to lowercase and re-upload.

<h3> Problem: Deployment pipeline succeeds but website doesn’t update </h3>

- Cause: Browser cache.
- Fix: Open the site in Incognito or clear the cache.
