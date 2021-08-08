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
* Click "**Add Project**" then select "**GitHub**"
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

In addition to the Snyk App UI we also have, snyk - CLI and build-time tool to find & fix known vulnerabilities in open-source dependencies. 

* Authorize the Snyk CLI with your account as follows

```bash
$ snyk auth

Now redirecting you to our auth page, go ahead and log in,
and once the auth is complete, return to this prompt and you'll
be ready to start using snyk.

If you can't wait use this url:
https://snyk.io/login?token=ff75a099-4a9f-4b3d-b75c-bf9847672e9c&utm_medium=cli&utm_source=cli&utm_campaign=cli&os=darwin&docker=false


Your account has been authenticated. Snyk is now ready to be used.
```

* Clone your forked repository as shown below. You would use your own GitHub repo here instead of the one shown below

```bash
$ git clone https://github.com/papicella/snyk-iac-workshop
Cloning into 'snyk-iac-workshop'...
remote: Enumerating objects: 27, done.
remote: Counting objects: 100% (27/27), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 27 (delta 5), reused 25 (delta 3), pack-reused 0
Receiving objects: 100% (27/27), 12.64 KiB | 1.05 MiB/s, done.
Resolving deltas: 100% (5/5), done.
```

* Change to the "**snyk-iac-workshop**" directory

```bash
$ cd snyk-iac-workshop
```

* At this point let's go ahead and test "**big_data.tf**" to do that issue a command as shown below. In this example we are testing that file itself by specifically referring to it in the command. 

```bash
$ snyk iac test ./terraform/big_data.tf

Testing big_data.tf...


Infrastructure as code issues:
  ✗ Public IP assigned to SQL database instance [High Severity] [SNYK-CC-TF-242] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings > ip_configuration > ipv4_enabled

  ✗ BigQuery dataset is publicly accessible [High Severity] [SNYK-CC-TF-236] in BigQuery
    introduced by google_bigquery_dataset[dataset] > access[0] > special_group

  ✗ Cloud SQL instance is publicly accessible [High Severity] [SNYK-CC-TF-235] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings > ip_configuration > authorized_networks[0]

  ✗ SSL is not enabled on CloudSQL instance [Medium Severity] [SNYK-CC-GCP-270] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings > ip_configuration > require_ssl

  ✗ Cloud SQL instance backup disabled [Medium Severity] [SNYK-CC-GCP-283] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings > backup_configuration

  ✗ The log_connections setting is disabled on Postgresql DB [Low Severity] [SNYK-CC-GCP-288] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ The log_disconnections setting is disabled on PostgreSQL DB [Low Severity] [SNYK-CC-GCP-289] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ SQL statements may be logged [Low Severity] [SNYK-CC-GCP-292] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ The log_lock_waits setting is disabled on PostgreSQL DB [Low Severity] [SNYK-CC-GCP-290] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ The log_checkpoints is disabled on PostgreSQL DB [Low Severity] [SNYK-CC-GCP-287] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ Temporary file information is not logged [Low Severity] [SNYK-CC-GCP-291] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings


Organization:      pas.apicella-41p
Type:              Terraform
Target file:       ./terraform/big_data.tf
Project name:      terraform
Open source:       no
Project path:      ./terraform/big_data.tf

Tested big_data.tf for known issues, found 11 issues
```

* Let's go ahead and fix the following

```bash
  ✗ SSL is not enabled on CloudSQL instance [Medium Severity] [SNYK-CC-GCP-270] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings > ip_configuration > require_ssl
```

* Edit the file "**./terraform/big_data.tf**" as shown below and add ip_configuration setting

```yaml
  settings {
    tier = "db-f1-micro"
    ip_configuration {
      ipv4_enabled = true
      require_ssl = true
      authorized_networks {
        name  = "WWW"
        value = "0.0.0.0/0"
      }
    }
```

* Go ahead and test "**./terraform/big_data.tf**" as shown below and verify that you now have resolved this issue

```bash
$ snyk iac test ./terraform/big_data.tf

Testing big_data.tf...


Infrastructure as code issues:
  ✗ Public IP assigned to SQL database instance [High Severity] [SNYK-CC-TF-242] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings > ip_configuration > ipv4_enabled

  ✗ BigQuery dataset is publicly accessible [High Severity] [SNYK-CC-TF-236] in BigQuery
    introduced by google_bigquery_dataset[dataset] > access[0] > special_group

  ✗ Cloud SQL instance is publicly accessible [High Severity] [SNYK-CC-TF-235] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings > ip_configuration > authorized_networks[0]

  ✗ Cloud SQL instance backup disabled [Medium Severity] [SNYK-CC-GCP-283] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings > backup_configuration

  ✗ The log_connections setting is disabled on Postgresql DB [Low Severity] [SNYK-CC-GCP-288] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ The log_disconnections setting is disabled on PostgreSQL DB [Low Severity] [SNYK-CC-GCP-289] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ SQL statements may be logged [Low Severity] [SNYK-CC-GCP-292] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ The log_lock_waits setting is disabled on PostgreSQL DB [Low Severity] [SNYK-CC-GCP-290] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ The log_checkpoints is disabled on PostgreSQL DB [Low Severity] [SNYK-CC-GCP-287] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings

  ✗ Temporary file information is not logged [Low Severity] [SNYK-CC-GCP-291] in Cloud SQL
    introduced by google_sql_database_instance[master_instance] > settings


Organization:      pas.apicella-41p
Type:              Terraform
Target file:       ./terraform/big_data.tf
Project name:      terraform
Open source:       no
Project path:      ./terraform/big_data.tf

Tested big_data.tf for known issues, found 10 issues
```

Note: The IaC policy for this issue is defined here [Policy SNYK-CC-GCP-270](https://snyk.io/security-rules/SNYK-CC-GCP-270)

![alt tag](https://i.ibb.co/chbWtx9/snyk-iac-5.png)

That's one less issue to worry about and when our Cloud SQL database is provisioned it will have SSL enabled making it for more secure than it previously was.

Go ahead and fix others if you have time and optionally commit your changes back to the GitHub repo if you like

## Step 5 Test using the Snyk CLI - AWS CloudFormation files

TODO://

## Step 6 Test using the Snyk CLI - Kubernetes YAML files

TODO://

## Step 7 View Snyk IaC Rules

TODO://

Thanks for attending and completing this workshop

![alt tag](https://i.ibb.co/7tnp1B6/snyk-logo.png)

<hr />
Pas Apicella [pas at snyk.io] is an Solution Engineer at Snyk APJ
