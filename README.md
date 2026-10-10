# Deploying a Node.js App to AWS EC2 using CI/CD

A Node.js application deployment project that uses **GitHub Actions** to
automate deployment to an **AWS EC2** instance over SSH. The workflow
helps reduce manual deployment steps by running the deployment process
from the repository's GitHub Actions pipeline.

## Overview

This project demonstrates a simple CI/CD workflow for deploying a
Node.js application to an Ubuntu-based EC2 instance.

## CI/CD Workflow

![CI/CD Workflow](CI%20CD%20workflow.gif)

### Tech Stack

-   **Node.js** --- application runtime
-   **AWS EC2** --- cloud server hosting the application
-   **GitHub Actions** --- CI/CD workflow automation
-   **SSH** --- secure remote access to the EC2 instance
-   **rsync** --- file synchronization during deployment
-   **PM2** --- process management for keeping the Node.js app running

## How It Works

1.  Code is pushed to the GitHub repository.
2.  GitHub Actions starts the deployment workflow.
3.  The workflow connects to the EC2 instance using SSH credentials
    stored in GitHub Actions Secrets.
4.  Application files are synchronized to the configured remote
    directory.
5.  The deployment commands run on the EC2 instance to update or start
    the application.

### 3. Review the workflow

Check the files under `.github/workflows/` to confirm:

-   Which branch or event triggers deployment
-   Which deployment action is used
-   The destination directory
-   The commands used to install dependencies and restart the app with
    PM2

## Important: EC2 Public IP Changes

If you stop and start an EC2 instance, its automatically assigned public
IPv4 address can change. If `REMOTE_HOST` still points to the old
address, GitHub Actions may fail with an SSH error such as:

``` text
ssh: connect to host <host> port 22: Connection timed out
```

To fix this:

1.  Check the instance's current **Public IPv4 DNS** or **Public IPv4
    address** in the AWS EC2 console.
2.  Update the matching GitHub Actions secret (`REMOTE_HOST`).
3.  Re-run the workflow.

To keep a stable public address, consider assigning an **Elastic IP**
and associating it with the instance. Review AWS pricing for public IPv4
addresses before using one.

## Security Best Practices

-   Store deployment credentials in GitHub Actions Secrets.
-   Never commit the `.pem` private key.
-   Keep inbound rules as restrictive as practical.
-   Avoid printing secrets in workflow logs.
-   Keep the EC2 operating system and application dependencies updated.