# Introduction to Snyk IaC Workshop

Snyk Infrastructure as Code allows you to find and fix vulnerabilities in your Kubernetes, Helm, Terraform and CloudFormation configuration files

Developer-focused infrastructure as code security with Snyk allows you to test and monitor Terraform modules and Kubernetes YAML, JSON, and Helm charts to detect configuration issues that could open your deployments to attack and malicious behavior.

In this **hands-on** workshop we will achieve the follow:

* [Step 1 Fork a GitHub IaC repository](#step-1-fork-a-github-iac-repository)
* [Step 2 Configure GitHub Integration](#step-2-configure-github-integration) 
* [Step 3 Add project to find vulnerabilities](#step-3-add-project-to-find-vulnerabilities)
* [Step 4 Test using the Snyk CLI - Terraform Files](#step-4-test-using-the-snyk-cli---terraform-files)
* [Step 5 Test using the Snyk CLI - AWS CloudFormation files](#step-5-test-using-the-snyk-cli---aws-cloudformation-files)
* [Step 6 Test using the Snyk CLI - Kubernetes YAML files](#step-6-test-using-the-snyk-cli---kubernetes-yaml-files)
* [Step 7 View Snyk IaC Rules](#step-7-view-snyk-iac-rules)

## Prerequisites

* public GitHub account - http://github.com
* git CLI - https://git-scm.com/downloads
* snyk CLI - https://support.snyk.io/hc/en-us/articles/360003812538-Install-the-Snyk-CLI
* Registered account on Snyk App - http://app.snyk.io

# Workshop Steps

_Note: It is assumed your using a mac for these steps but it should also work on windows or linux with some modifications to the scripts potentially_

## Step 1 Fork a GitHub IaC repository

Navigate to the following GitHub repo - https://github.com/papicella/snyk-iac-workshop

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

* Before we get started we need to make sure IaC is enabled navigate to "**Settings -> Infrastructure as code**" and ensure it is enabled as shown below.

![alt tag](https://i.ibb.co/bRLppWW/snyk-iac-7.png)

Now that Snyk is connected to your GitHub Account, import the Repo into Snyk as a Project.

* Navigate to Projects 
* Click "**Add Project**" then select "**GitHub**"
* Click on the Repo you forked "**snyk-iac-workshop**"

![alt tag](https://i.ibb.co/pWJW1VK/snyk-iac-1.png)

__Note: Once complete you should see various IaC scans as shown below_

![alt tag](https://i.ibb.co/YNC5rfd/snyk-iac-2.png)

* Go ahead and click on "**big_data.tf**" terraform file as shown below

![alt tag](https://i.ibb.co/DKY911V/snyk-iac-3.png)

For each Vulnerability, Snyk displays the following ordered by Line no:

1. Each Vulnerability grouped by line no and severity 
1. Each Vulnerability links to the Snyk policy it was defined against including the path to the issue, what the issue is, the impact and how to resolve it
1. The ability to ignore issues you wish to remove from the list

![alt tag](https://i.ibb.co/3dJHzrm/snyk-iac-4.png)

Note: We will resolve some of these issues shortly for now just browse through some of them to get familiar with what was raised and why including clicking on the Snyk Policy links

## Step 4 Test using the Snyk CLI - Terraform Files

Snyk tests and monitors your Terraform files from your source code repositories, guiding you with advice for how you can better secure your cloud environment--catching misconfigurations before you push to production and helping you to fix them

In addition to the Snyk App UI we also have, snyk - CLI and build-time tool to find & fix known vulnerabilities in open-source dependencies and IaC configuration files. 

_Note: Please ensure you have the latest version of the Snyk CLI installed a version equal to or greater than the version below. https://docs.snyk.io/features/snyk-cli/install-the-snyk-cli_

```bash
$ snyk --version
1.675.0
```

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

* Edit the file "**./terraform/big_data.tf**" as shown below and add ip_configuration setting "**require_ssl = true**" as shown below.

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

* To output the test format as JSON issue a command as follows. This provides more detailed information including links to issue references as well as the ability to upload the data into other system for reporting purposes.

```json 
$ snyk iac test ./terraform/big_data.tf --json
{
  "meta": {
    "isPrivate": true,
    "isLicensesEnabled": false,
    "ignoreSettings": null,
    "org": "pas.apicella-41p",
    "projectId": "",
    "policy": ""
  },
  "filesystemPolicy": false,
  "vulnerabilities": [],
  "dependencyCount": 0,
  "licensesPolicy": null,
  "ignoreSettings": null,
  "targetFile": "./terraform/big_data.tf",
  "projectName": "terraform",
  "org": "pas.apicella-41p",
  "policy": "",
  "isPrivate": true,
  "targetFilePath": "/Users/pasapicella/temp/snyk-iac-workshop/terraform/big_data.tf",
  "packageManager": "terraformconfig",
  "path": "./terraform/big_data.tf",
  "projectType": "terraformconfig",
  "ok": false,
  "infrastructureAsCodeIssues": [
    {
      "severity": "high",
      "resolve": "Set `settings.ip_configuration.ipv4_enabled` attribute to `false`",
      "id": "SNYK-CC-TF-242",
      "impact": "Database can be accessed remotely over public Internet",
      "msg": "resource.google_sql_database_instance[master_instance].settings.ip_configuration.ipv4_enabled",
      "subType": "Cloud SQL",
      "issue": "Public IP will be assigned to the SQL database",
      "publicId": "SNYK-CC-TF-242",
      "title": "Public IP assigned to SQL database instance",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.6 Ensure that Cloud SQL database instances do not have public IPs",
        "https://cloud.google.com/sql/docs/mysql/configure-private-ip",
        "https://cloud.google.com/sql/docs/sqlserver/configure-ip"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "Public IP will be assigned to the SQL database",
        "impact": "Database can be accessed remotely over public Internet",
        "resolve": "Set `settings.ip_configuration.ipv4_enabled` attribute to `false`"
      },
      "lineNumber": 9,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-TF-242",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings",
        "ip_configuration",
        "ipv4_enabled"
      ]
    },
    {
      "severity": "low",
      "resolve": "Set `settings.database_flags.name` attribute to `\"log_connections\"`, and `settings.database_flags.value` attribute to `\"on\"`",
      "id": "SNYK-CC-GCP-288",
      "impact": "Connection logs will not be available for troubleshooting or investigations",
      "msg": "resource.google_sql_database_instance[master_instance].settings",
      "subType": "Cloud SQL",
      "issue": "PostgreSQL 'log_connections' is disabled",
      "publicId": "SNYK-CC-GCP-288",
      "title": "The log_connections setting is disabled on Postgresql DB",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.2.2 Ensure that the 'log_connections' database flag for Cloud SQL PostgreSQL instance is set to 'on'",
        "https://cloud.google.com/sql/docs/postgres/flags"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "PostgreSQL 'log_connections' is disabled",
        "impact": "Connection logs will not be available for troubleshooting or investigations",
        "resolve": "Set `settings.database_flags.name` attribute to `\"log_connections\"`, and `settings.database_flags.value` attribute to `\"on\"`"
      },
      "lineNumber": 6,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-GCP-288",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings"
      ]
    },
    {
      "severity": "high",
      "resolve": "Remove `allAuthenticatedUsers` and `allUsers` values from `access.special_group` attribute",
      "id": "SNYK-CC-TF-236",
      "impact": "Potentially anyone can access data in the dataset",
      "msg": "resource.google_bigquery_dataset[dataset].access[0].special_group",
      "subType": "BigQuery",
      "issue": "BigQuery dataset is publicly accessible",
      "publicId": "SNYK-CC-TF-236",
      "title": "BigQuery dataset is publicly accessible",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 7.1 Ensure that BigQuery datasets are not anonymously or publicly accessible",
        "https://cloud.google.com/bigquery/public-data"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "BigQuery dataset is publicly accessible",
        "impact": "Potentially anyone can access data in the dataset",
        "resolve": "Remove `allAuthenticatedUsers` and `allUsers` values from `access.special_group` attribute"
      },
      "lineNumber": 22,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-TF-236",
      "path": [
        "resource",
        "google_bigquery_dataset[dataset]",
        "access[0]",
        "special_group"
      ]
    },
    {
      "severity": "low",
      "resolve": "Set the `settings.database_flags.name` attribute to `\"log_disconnections\"` and `settings.database_flags.value` attribute to `\"on\"`",
      "id": "SNYK-CC-GCP-289",
      "impact": "Disconnection logs will not be available for troubleshooting or investigations",
      "msg": "resource.google_sql_database_instance[master_instance].settings",
      "subType": "Cloud SQL",
      "issue": "PostgreSQL 'log_disconnections' is disabled",
      "publicId": "SNYK-CC-GCP-289",
      "title": "The log_disconnections setting is disabled on PostgreSQL DB",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.2.3 Ensure that the 'log_disconnections' database flag for Cloud SQL PostgreSQL instance is set to 'on'",
        "https://cloud.google.com/sql/docs/postgres/flags"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "PostgreSQL 'log_disconnections' is disabled",
        "impact": "Disconnection logs will not be available for troubleshooting or investigations",
        "resolve": "Set the `settings.database_flags.name` attribute to `\"log_disconnections\"` and `settings.database_flags.value` attribute to `\"on\"`"
      },
      "lineNumber": 6,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-GCP-289",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings"
      ]
    },
    {
      "severity": "low",
      "resolve": "Set the `settings.database_flags.name` attribute to `\"log_min_duration_statement\"` and `settings.database_flags.value` attribute to `-1`",
      "id": "SNYK-CC-GCP-292",
      "impact": "Some SQL statements may be logged and expose sensitive information",
      "msg": "resource.google_sql_database_instance[master_instance].settings",
      "subType": "Cloud SQL",
      "issue": "PostgreSQL 'log_min_duration_statement' is not set to -1",
      "publicId": "SNYK-CC-GCP-292",
      "title": "SQL statements may be logged",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.2.7 Ensure that the 'log_min_duration_statement' database flag for Cloud SQL PostgreSQL instance is set to '-1' (disabled)",
        "https://cloud.google.com/sql/docs/postgres/flags"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "PostgreSQL 'log_min_duration_statement' is not set to -1",
        "impact": "Some SQL statements may be logged and expose sensitive information",
        "resolve": "Set the `settings.database_flags.name` attribute to `\"log_min_duration_statement\"` and `settings.database_flags.value` attribute to `-1`"
      },
      "lineNumber": 6,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-GCP-292",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings"
      ]
    },
    {
      "severity": "low",
      "resolve": "Set `settings.database_flags.name` attribute to `\"log_lock_waits\"`, and `settings.database_flags.value` attribute to `\"on\"`",
      "id": "SNYK-CC-GCP-290",
      "impact": "Deadlock timeouts logs will not be available for troubleshooting, or investigations",
      "msg": "resource.google_sql_database_instance[master_instance].settings",
      "subType": "Cloud SQL",
      "issue": "PostgreSQL 'log_lock_waits' is disabled",
      "publicId": "SNYK-CC-GCP-290",
      "title": "The log_lock_waits setting is disabled on PostgreSQL DB",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.2.4 Ensure that the 'log_lock_waits' database flag for Cloud SQL PostgreSQL instance is set to 'on' ",
        "https://cloud.google.com/sql/docs/postgres/flags"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "PostgreSQL 'log_lock_waits' is disabled",
        "impact": "Deadlock timeouts logs will not be available for troubleshooting, or investigations",
        "resolve": "Set `settings.database_flags.name` attribute to `\"log_lock_waits\"`, and `settings.database_flags.value` attribute to `\"on\"`"
      },
      "lineNumber": 6,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-GCP-290",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings"
      ]
    },
    {
      "severity": "medium",
      "resolve": "Set `settings.backup_configuration.enabled` attribute to `true`",
      "id": "SNYK-CC-GCP-283",
      "impact": "Data will not be recoverable in the event of failure or malicious attack",
      "msg": "resource.google_sql_database_instance[master_instance].settings.backup_configuration",
      "subType": "Cloud SQL",
      "issue": "Automated backup is explicitly disabled",
      "publicId": "SNYK-CC-GCP-283",
      "title": "Cloud SQL instance backup disabled",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.7 Ensure that Cloud SQL database instances are configured with automated backups",
        "https://cloud.google.com/sql/docs/sqlserver/backup-recovery/backups"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "Automated backup is explicitly disabled",
        "impact": "Data will not be recoverable in the event of failure or malicious attack",
        "resolve": "Set `settings.backup_configuration.enabled` attribute to `true`"
      },
      "lineNumber": 16,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-GCP-283",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings",
        "backup_configuration"
      ]
    },
    {
      "severity": "low",
      "resolve": "Set `settings.database_flags.name` attribute to `\"log_checkpoints\"`, and `settings.database_flags.value` attribute to `\"on\"`",
      "id": "SNYK-CC-GCP-287",
      "impact": "Verbose logging information of database will not be collected",
      "msg": "resource.google_sql_database_instance[master_instance].settings",
      "subType": "Cloud SQL",
      "issue": "PostgreSQL 'log_checkpoints' is disabled",
      "publicId": "SNYK-CC-GCP-287",
      "title": "The log_checkpoints is disabled on PostgreSQL DB",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.2.1 Ensure that the 'log_checkpoints' database flag for Cloud SQL PostgreSQL instance is set to 'on'",
        "https://cloud.google.com/sql/docs/postgres/flags"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "PostgreSQL 'log_checkpoints' is disabled",
        "impact": "Verbose logging information of database will not be collected",
        "resolve": "Set `settings.database_flags.name` attribute to `\"log_checkpoints\"`, and `settings.database_flags.value` attribute to `\"on\"`"
      },
      "lineNumber": 6,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-GCP-287",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings"
      ]
    },
    {
      "severity": "low",
      "resolve": "Set the `settings.database_flags.name` attribute to `\"log_temp_files\"`, and `settings.database_flags.value` attribute to `0`",
      "id": "SNYK-CC-GCP-291",
      "impact": "Some temporary files information may not be logged, which may impact ability to identify potential performance issues",
      "msg": "resource.google_sql_database_instance[master_instance].settings",
      "subType": "Cloud SQL",
      "issue": "PostgreSQL 'log_temp_files' is not set to 0",
      "publicId": "SNYK-CC-GCP-291",
      "title": "Temporary file information is not logged",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.2.6 Ensure that the 'log_temp_files' database flag for Cloud SQL PostgreSQL instance is set to '0'",
        "https://cloud.google.com/sql/docs/postgres/flags"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "PostgreSQL 'log_temp_files' is not set to 0",
        "impact": "Some temporary files information may not be logged, which may impact ability to identify potential performance issues",
        "resolve": "Set the `settings.database_flags.name` attribute to `\"log_temp_files\"`, and `settings.database_flags.value` attribute to `0`"
      },
      "lineNumber": 6,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-GCP-291",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings"
      ]
    },
    {
      "severity": "high",
      "resolve": "Set `settings.ip_configuration.authorized_networks` attribute to specific value e.g. `192.168.0.0/24`",
      "id": "SNYK-CC-TF-235",
      "impact": "Potentially anyone can establish network connectivity to the database instance",
      "msg": "resource.google_sql_database_instance[master_instance].settings.ip_configuration.authorized_networks[0]",
      "subType": "Cloud SQL",
      "issue": "Cloud SQL database instance allows public access",
      "publicId": "SNYK-CC-TF-235",
      "title": "Cloud SQL instance is publicly accessible",
      "references": [
        "CIS Google Cloud Platform Foundation v1.1.0 - 6.5 Ensure that Cloud SQL database instances are not open to the world",
        "https://cloud.google.com/sql/docs/mysql/configure-ip",
        "https://cloud.google.com/sql/docs/mysql/configure-private-ip"
      ],
      "isIgnored": false,
      "iacDescription": {
        "issue": "Cloud SQL database instance allows public access",
        "impact": "Potentially anyone can establish network connectivity to the database instance",
        "resolve": "Set `settings.ip_configuration.authorized_networks` attribute to specific value e.g. `192.168.0.0/24`"
      },
      "lineNumber": 8,
      "documentation": "https://snyk.io/security-rules/SNYK-CC-TF-235",
      "path": [
        "resource",
        "google_sql_database_instance[master_instance]",
        "settings",
        "ip_configuration",
        "authorized_networks[0]"
      ]
    }
  ]
}
```

* With Snyk Infrastructure as Code, you can scan both your static configuration files and Terraform Plan output using the CLI, [Test your Terraform files with our CLI tool](https://support.snyk.io/hc/en-us/articles/360013723877-Test-your-Terraform-files-with-our-CLI-tool).

Terraform Plan is the step run between writing your configuration files and deploying those changes.

Note: terraform plan identifies the changes that need to be made to your target environment in order to match your desired state.

If you have written a custom terraform module and are referencing it in your deployment, then it will be included in the terraform plan output and scanned   accordingly.

This means the Terraform plan output provides a complete artefact to be scanned from a security perspective. 

_Note: For this workshop we won't be doing a terraform plan scan but it's important to know we can do that_

![alt tag](https://i.ibb.co/gDLFYcH/snyk-iac-6.png)

## Step 5 Test using the Snyk CLI - AWS CloudFormation files

_Note: Please ensure you have the latest version of the Snyk CLI installed a version equal to or greater than the version below. https://docs.snyk.io/features/snyk-cli/install-the-snyk-cli_

```bash
$ snyk --version
1.675.0
```

Snyk tests and monitors CloudFormation files from source code repositories. It gives advice on how to better secure cloud environments by catching misconfigurations before they are pushed to production along with assistance on how best to fix them

* Run the following IaC scan of any files inside the folder "**CloudFormation**". This will pick up all the IaC config files which exist in this directory in our case we have two files

```bash
$ snyk iac test ./CloudFormation/

Testing fargate-service.yml...


Infrastructure as code issues:
  ✗ S3 block public policy control is disabled [High Severity] [SNYK-CC-TF-96] in S3
    introduced by Resources[CodePipelineArtifactBucket] > Properties > PublicAccessBlockConfiguration > BlockPublicPolicy

  ✗ S3 ignore public ACLs control is disabled [High Severity] [SNYK-CC-TF-97] in S3
    introduced by Resources[CodePipelineArtifactBucket] > Properties > PublicAccessBlockConfiguration > IgnorePublicAcls

  ✗ S3 block public ACLs control is disabled [High Severity] [SNYK-CC-TF-95] in S3
    introduced by Resources[CodePipelineArtifactBucket] > Properties > PublicAccessBlockConfiguration > BlockPublicAcls

  ✗ S3 restrict public bucket control is disabled [High Severity] [SNYK-CC-TF-98] in S3
    introduced by Resources[CodePipelineArtifactBucket] > Properties > PublicAccessBlockConfiguration > RestrictPublicBuckets

  ✗ ECR image scanning is disabled [Low Severity] [SNYK-CC-TF-61] in ECR
    introduced by Resources[EcrDockerRepository] > Properties > ImageScanningConfiguration

  ✗ S3 bucket versioning disabled [Low Severity] [SNYK-CC-TF-124] in S3
    introduced by Resources[CodePipelineArtifactBucket] > Properties > VersioningConfiguration > Status

  ✗ CloudWatch log group not encrypted with managed key [Low Severity] [SNYK-CC-AWS-415] in CloudWatch
    introduced by Resources[LogGroup] > Properties > KmsKeyId

  ✗ ECR repository is not encrypted with customer managed key [Low Severity] [SNYK-CC-AWS-418] in ECR
    introduced by Resources[EcrDockerRepository] > Properties > EncryptionConfiguration > KmsKey

  ✗ ECR Registry allows mutable tags [Low Severity] [SNYK-CC-TF-126] in ECR
    introduced by Resources[EcrDockerRepository] > Properties > ImageTagMutability


Organization:      pas.apicella-41p
Type:              CloudFormation
Target file:       fargate-service.yml
Project name:      CloudFormation
Open source:       no
Project path:      ./CloudFormation/

Tested fargate-service.yml for known issues, found 9 issues

-------------------------------------------------------

Testing vpc.json...


Infrastructure as code issues:
  ✗ Security Group allows open ingress [Medium Severity] [SNYK-CC-TF-1] in VPC
    introduced by Resources > ELBSecurityGroup > Properties > SecurityGroupIngress[0]

  ✗ AWS Security Group allows open egress [Low Severity] [SNYK-CC-TF-73] in VPC
    introduced by Resources[BastionSecurityGroup] > Properties > SecurityGroupEgress[1] > CidrIp

  ✗ Rule allows open egress [Low Severity] [SNYK-CC-TF-72] in VPC
    introduced by Resources[BastionSecurityGroup] > Properties > SecurityGroupEgress[1]

  ✗ AWS Security Group allows open egress [Low Severity] [SNYK-CC-TF-73] in VPC
    introduced by Resources[BastionSecurityGroup] > Properties > SecurityGroupEgress[2] > CidrIp

  ✗ Rule allows open egress [Low Severity] [SNYK-CC-TF-72] in VPC
    introduced by Resources[BastionSecurityGroup] > Properties > SecurityGroupEgress[2]

  ✗ AWS Security Group allows open egress [Low Severity] [SNYK-CC-TF-73] in VPC
    introduced by Resources[DbSecurityGroup] > Properties > SecurityGroupEgress[1] > CidrIp

  ✗ Rule allows open egress [Low Severity] [SNYK-CC-TF-72] in VPC
    introduced by Resources[DbSecurityGroup] > Properties > SecurityGroupEgress[0]

  ✗ AWS Security Group allows open egress [Low Severity] [SNYK-CC-TF-73] in VPC
    introduced by Resources[BastionSecurityGroup] > Properties > SecurityGroupEgress[0] > CidrIp

  ✗ Rule allows open egress [Low Severity] [SNYK-CC-TF-72] in VPC
    introduced by Resources[BastionSecurityGroup] > Properties > SecurityGroupEgress[0]

  ✗ Rule allows open egress [Low Severity] [SNYK-CC-TF-72] in VPC
    introduced by Resources[DbSecurityGroup] > Properties > SecurityGroupEgress[1]

  ✗ AWS Security Group allows open egress [Low Severity] [SNYK-CC-TF-73] in VPC
    introduced by Resources[DbSecurityGroup] > Properties > SecurityGroupEgress[0] > CidrIp


Organization:      pas.apicella-41p
Type:              CloudFormation
Target file:       vpc.json
Project name:      CloudFormation
Open source:       no
Project path:      ./CloudFormation/

Tested vpc.json for known issues, found 11 issues


Tested 2 projects, 2 contained issues.
```

Go ahead and fix others if you have time and optionally commit your changes back to the GitHub repo if you like.

* To output the test format as JSON issue a command as follows. This provides more detailed information including links to issue references as well as the ability to upload the data into other system for reporting purposes.

```bash
$ snyk iac test ./CloudFormation --json
```

For more information on AWS Cloud Formation scanning with Snyk see the following blog post
[Scan for AWS CloudFormation misconfigurations with Snyk IaC](https://snyk.io/blog/scan-aws-cloudformation-misconfigurations-snyk-iac/)

## Step 6 Test using the Snyk CLI - Kubernetes YAML files

Snyk tests and monitors Kubernetes configurations stored in your source code repositories and provides information, tips, and tricks to better secure a Kubernetes environment--catching misconfigurations before they are pushed to production as well as providing fixes for vulnerabilities

Snyk Infrastructure as Code for Kubernetes supports:

1. Deployments, Pods and Services.
1. CronJobs, Jobs, StatefulSet, ReplicaSet, DaemonSet, and ReplicationController
1. Helm Charts

* Let's scan our Kubernetes YAML file which is just a basic K8s deployment as shown below

```bash
$ snyk iac test ./Kubernetes/employee-K8s.yaml

Testing employee-K8s.yaml...


Infrastructure as code issues:
  ✗ Container is running without privilege escalation control [Medium Severity] [SNYK-CC-K8S-9] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > securityContext > allowPrivilegeEscalation

  ✗ Container is running without root user control [Medium Severity] [SNYK-CC-K8S-10] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > securityContext > runAsNonRoot

  ✗ Container does not drop all default capabilities [Medium Severity] [SNYK-CC-K8S-6] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > securityContext > capabilities > drop

  ✗ Service does not restrict ingress sources [Medium Severity] [SNYK-CC-K8S-15] in Service
    introduced by service > spec > loadBalancerSourceRanges

  ✗ Container is running without liveness probe [Low Severity] [SNYK-CC-K8S-41] in Deployment
    introduced by spec > template > spec > containers[snyk-employee-api] > livenessProbe

  ✗ Container is running with writable root filesystem [Low Severity] [SNYK-CC-K8S-8] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > securityContext > readOnlyRootFilesystem

  ✗ Container is running without AppArmor profile [Low Severity] [SNYK-CC-K8S-32] in Deployment
    introduced by metadata > annotations['container.apparmor.security.beta.kubernetes.io/snyk-employee-api']

  ✗ Container is running without memory limit [Low Severity] [SNYK-CC-K8S-4] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > resources > limits > memory

  ✗ Container is running without cpu limit [Low Severity] [SNYK-CC-K8S-5] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > resources > limits > cpu


Organization:      pas.apicella-41p
Type:              Kubernetes
Target file:       ./Kubernetes/employee-K8s.yaml
Project name:      Kubernetes
Open source:       no
Project path:      ./Kubernetes/employee-K8s.yaml

Tested employee-K8s.yaml for known issues, found 9 issues
```

* Let's go ahead and fix the following

```bash
  ✗ Container is running without root user control [Medium Severity] [SNYK-CC-K8S-10] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > securityContext > runAsNonRoot
```

* Edit the file "**./Kubernetes/employee-K8s.yaml**" and add "**securityContext > runAsNonRoot**" as shown below.

```yaml
    spec:
      securityContext:
        runAsNonRoot: true
      containers:
        - name: snyk-employee-api
          image: pasapples/springbootemployee:jib
          imagePullPolicy: Always
          ports:
            - containerPort: 8080
```

* Run a scan of "**./Kubernetes/employee-K8s.yaml**" again to verify you have fixed that issue
  
```bash
$ snyk iac test ./Kubernetes/employee-K8s.yaml

Testing employee-K8s.yaml...


Infrastructure as code issues:
  ✗ Container is running without privilege escalation control [Medium Severity] [SNYK-CC-K8S-9] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > securityContext > allowPrivilegeEscalation

  ✗ Container does not drop all default capabilities [Medium Severity] [SNYK-CC-K8S-6] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > securityContext > capabilities > drop

  ✗ Service does not restrict ingress sources [Medium Severity] [SNYK-CC-K8S-15] in Service
    introduced by service > spec > loadBalancerSourceRanges

  ✗ Container is running without liveness probe [Low Severity] [SNYK-CC-K8S-41] in Deployment
    introduced by spec > template > spec > containers[snyk-employee-api] > livenessProbe

  ✗ Container is running with writable root filesystem [Low Severity] [SNYK-CC-K8S-8] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > securityContext > readOnlyRootFilesystem

  ✗ Container is running without AppArmor profile [Low Severity] [SNYK-CC-K8S-32] in Deployment
    introduced by metadata > annotations['container.apparmor.security.beta.kubernetes.io/snyk-employee-api']

  ✗ Container is running without memory limit [Low Severity] [SNYK-CC-K8S-4] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > resources > limits > memory

  ✗ Container is running without cpu limit [Low Severity] [SNYK-CC-K8S-5] in Deployment
    introduced by input > spec > template > spec > containers[snyk-employee-api] > resources > limits > cpu


Organization:      pas.apicella-41p
Type:              Kubernetes
Target file:       ./Kubernetes/employee-K8s.yaml
Project name:      Kubernetes
Open source:       no
Project path:      ./Kubernetes/employee-K8s.yaml

Tested employee-K8s.yaml for known issues, found 8 issues
```

Go ahead and fix others if you have time and optionally commit your changes back to the GitHub repo if you like.

* To output the test format as JSON issue a command as follows. This provides more detailed information including links to issue references as well as the ability to upload the data into other system for reporting purposes.

```bash
$ snyk iac test ./Kubernetes/employee-K8s.yaml --json
```

## Step 7 View Snyk IaC Rules

Snyk IaC has a comprehensive set of security rules across AWS, Azure, GCP & Kubernetes with support for Terraform, CloudFormation, Kubernetes, and Helm configuration formats. The details of these issues, their impact, and how to fix them are all built-in to Snyk IaC, so developers get feedback directly in their own tools. For reference, we have also documented the security rules that we support for each provider below, along with relevant benchmarks and authoritative third-party references

Navigate to [Snyk Infrastructure as Code](https://snyk.io/security-rules)

Thanks for attending and completing this workshop

![alt tag](https://i.ibb.co/7tnp1B6/snyk-logo.png)

<hr />
Pas Apicella [pas at snyk.io] is an Solution Engineer at Snyk APJ
