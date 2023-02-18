# Week 0 — Billing and Architecture

## Required Homework

### Install AWS CLI

I was able to use Gitpod but for some reason I dont' see the code here. I had trouble with Github and commit, I'm still learning to use Github, but I can provide the details of the instructions below

I am able to see only this code below in .gitpod.yml.

`vscode:
  extensions:
    - 42Crunch.vscode-openapi`

### Instructions of CLI installation in gitpod (Linux)

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip'
sudo ./aws/install

```

![Proof of using CLI](assets/CreateBudget.png)

I was able to set up the environment variables using the following code 

```
export AWS_ACCESS_KEY_ID= (I used my access key I created earlier on my IAM account)
export AWS_SECRET_ACCESS_KEY= (I used my secret access key I created earlier on my IAM account)
export AWS_DEFAULT_REGION=us-east-1

```
I followed the steps from the video and commited by creating a Week -1 branch and was not successful. The code did not save and I was not able to see the commit changes in the gitpod.yml . I was helped by a team mate and now I am able to see the Week-1 branch on github however, I don't have the details in it. 

### Created Json file to create - budget

```
aws budgets create-budget \
    --account-id 111122223333 \
    --budget file://budget.json \
    --notifications-with-subscribers file://notifications-with-subscribers.json
```
![Proof of creating budget using CLI](assets/CreateBudgetCLI.png)
)

## Homework Challenges

### I've set up MFA's for root and IAM. 
Created Admin access and set up Admin policies. 
