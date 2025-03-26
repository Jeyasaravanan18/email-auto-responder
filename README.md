# Email Auto-Responder  

This repository automates email responses using GitHub Actions and an SMTP server.  

## Features  
- Automatically sends a predefined email response when triggered.  
- Uses GitHub Actions to run the workflow.  
- Supports customization of email content and recipient.  

## Setup  

### 1. Configure Repository Secrets  
Go to your repository **Settings > Secrets and variables > Actions > New repository secret** and add the following secrets:  

| Secret Name       | Description                           | Example Value          |  
|------------------|-----------------------------------|------------------------|  
| SMTP_SERVER      | SMTP server address              | `smtp.gmail.com`       |  
| SMTP_PORT        | SMTP server port (TLS: 587, SSL: 465) | `587`                  |  
| SMTP_USERNAME    | Your email username (full email) | `your-email@gmail.com` |  
| SMTP_PASSWORD    | Your email password or app password | `your-app-password`    |  
| RECIPIENT_EMAIL  | Email address to send responses to | `recipient@example.com`|  

### 2. Create GitHub Actions Workflow  

Inside your repository, create the folder **`.github/workflows/`** and add a YAML file (e.g., **`auto-responder.yml`**):  

```yaml
name: Email Auto-Responder  

on:  
  workflow_dispatch:  
  push:  
    branches:  
      - main  

jobs:  
  send_email:  
    runs-on: ubuntu-latest  
    steps:  
      - name: Checkout Repository  
        uses: actions/checkout@v4  

      - name: Send Email  
        uses: dawidd6/action-send-mail@v3  
        with:  
          server_address: ${{ secrets.SMTP_SERVER }}  
          server_port: ${{ secrets.SMTP_PORT }}  
          username: ${{ secrets.SMTP_USERNAME }}  
          password: ${{ secrets.SMTP_PASSWORD }}  
          subject: "Auto-Response from GitHub"  
          body: "Hello, this is an automated response."  
          to: ${{ secrets.RECIPIENT_EMAIL }}  
          from: "GitHub Auto-Responder <${{ secrets.SMTP_USERNAME }}>"