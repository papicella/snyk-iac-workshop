# Introduction to Snyk IaC Workshop

Snyk Infrastructure as Code allows you to find and fix vulnerabilities in your Kubernetes, Helm, Terraform and CloudFormation configuration files

Developer-focused infrastructure as code security with Snyk allows you to test and monitor Terraform modules and Kubernetes YAML, JSON, and Helm charts to detect configuration issues that could open your deployments to attack and malicious behavior.

In this **hands-on** workshop we will achieve the follow

* Step 1 Fork a GitHub IaC repository
* Step 2 Configure GitHub Integration 
* Step 3 Add project to find vulnerabilities
* Step 4 Test using the Snyk CLI - Terraform Files
* Step 5 Test using the Snyk CLI - AWS CloudFormation files
* Step 6 Test using the Snyk CLI - Kubernetes YAML files
* Step 7 View Snyk IaC Rules

## Prerequisites

* public GitHub account - http://github.com
* git CLI - https://git-scm.com/downloads
* snyk CLI - https://support.snyk.io/hc/en-us/articles/360003812538-Install-the-Snyk-CLI
* Registered account on Snyk App - http://app.snyk.io

# Workshop Steps

_Note: It is assumed your using a mac for these steps but it should also work on windows or linux with some modifications to the scripts potentially_

## Step 1 Fork a GitHub IaC repository

Navigate to the following GitHub repo - https://github.com/papicella/snyk-iac-workshop.git

* Click on the "**Fork**" button
* Ensure you are forking this repo to your public GitHub account
* Click done

## Step 2 Configure GitHub Integration

First we need to connect Snyk to GitHub so we can import our Repository. Do so by:

* Login to http://app.snyk.io Sign up if you haven't already.
* Navigating to Integrations -> Source Control -> GitHub
* Fill in your Account Credentials to Connect your GitHub Account.

![alt tag](https://i.ibb.co/bPqqybM/snyk-starter-open-source-1.png)

## Step 3 Add project to find vulnerabilities

Now that Snyk is connected to your GitHub Account, import the Repo into Snyk as a Project.

* Navigate to Projects 
* Click "Add Project" then select "**GitHub**"
* Click on the Repo you forked "snyk-iac-workshop"

![alt tag](https://i.ibb.co/pWJW1VK/snyk-iac-1.png)

__Note: Once complete you should see various IaC scans as shown below_

![alt tag](https://i.ibb.co/YNC5rfd/snyk-iac-2.png)

* Go ahead and click on "**big_data.tf**" terraform file as shown below

![alt tag](https://i.ibb.co/DKY911V/snyk-iac-3.png)

For each Vulnerability, Snyk displays the following ordered by our Line no :

1. Each Vulnerability grouped by line no. and severity 
1. Each Vulnerability links to the Snyk policy it was defined against including the path to the issue, what the issue is, the impact and how to resolve it
1. The ability to ignore issues you wish to remove from the list

![alt tag](https://i.ibb.co/3dJHzrm/snyk-iac-4.png)

Note: We will resolve some of these issues shortly for now just browse through some of them to get familiar with what was raised and why including clicking on the Snyk Policy links

## Step 4 Test using the Snyk CLI - Terraform Files


## Step 5 Test using the Snyk CLI - AWS CloudFormation files


## Step 6 Test using the Snyk CLI - Kubernetes YAML files


## Step 7 View Snyk IaC Rules

Thanks for attending and completing this workshop

![alt tag](https://i.ibb.co/7tnp1B6/snyk-logo.png)

<hr />
Pas Apicella [pas at snyk.io] is an Solution Engineer at Snyk APJ
