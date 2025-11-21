Project Deployment Guide

This README provides step-by-step instructions on how to deploy the project using the provided GitHub repositories. Follow the steps below to download the required files, configure your self-hosted runner, and set up the deployment.

Prerequisites

Ensure you have access to the following public GitHub repositories:

Frontend: Frontend Repository

Backend: Backend Repository

Deployment: Deployment Repository

Install GitHub Actions Runner
 on your local or remote machine.

Make sure you have Kubernetes
 set up for port-forwarding the backend service.

Step 1: Download the Required Files

Create a Folder to store the GitHub Actions runner:

mkdir actions-runner && cd actions-runner


Download the Latest Runner Package:

curl -o actions-runner-linux-x64-2.329.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.329.0/actions-runner-linux-x64-2.329.0.tar.gz


(Optional) Validate the Hash:

echo "194f1e4bd028f80b7e963cf5406848d84e19f3928a324d512ea53430102e1d actions-runner-linux-x64-2.329.0.tar.gz" | shasum -a 256 -c


Extract the Installer:

tar xzf ./actions-runner-linux-x64-2.329.0.tar.gz

Step 2: Configure the Runner

Run the Configuration Script:

./config.sh --url https://github.com/YOUR_USERNAME/YOUR_REPO --token YOUR_TOKEN


Replace YOUR_USERNAME/YOUR_REPO with your GitHub repository and YOUR_TOKEN with your GitHub runner token.

Start the Runner:

./run.sh

Step 3: Use Your Self-Hosted Runner

Once the runner is configured and running, you can use it in your GitHub Actions workflows. To use the self-hosted runner in your workflows, add the following YAML to the jobs section of your GitHub Actions workflow file:

runs-on: self-hosted

Step 4: Kubernetes Port-Forwarding (for Backend Deployment)

To access the backend service in Kubernetes, use the following command to forward the port:

kubectl port-forward service/arainea-backend 8080:8080 -n arainea


This command will allow you to access the backend at http://localhost:8080.

Step 5: Running the Deployment Job

After configuring your runner and forwarding the necessary ports, you can deploy your project by triggering the appropriate deployment job from your GitHub Actions. The runner will automatically pick up and execute the job:

2025-11-21 12:16:21Z: Running job: deploy
2025-11-21 12:17:41Z: Job deploy completed with result: Succeeded

Additional Information

You can find more details and links to the repositories below:

Repo 1 - Frontend: Frontend Repository

Repo 2 - Backend: Backend Repository

Repo 3 - Deployment: Deployment Repository
