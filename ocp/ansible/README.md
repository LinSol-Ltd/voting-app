# Voting OpenShift Ansibles

This directory contains automation for running Voting in OpenShift.

# Pre-requisites

In order to run ```voting.yml``` playbook you need to add service account
into OCP:

```
oc create sa ansible
oc adm policy add-role-to-user admin -z ansible
oc sa get-token ansible
```

You also need to have the variables set for the site. They live in group_vars
-directory, and are described below in this README.md.

# Running playbook

For staging:

```
ansible-playbook -i staging voting.yml
```

For production:

```
ansible-playbook -i production voting.yml
```

# Variables explained

Each environment may override role variables. Here are the ones we use
explained.


* **ansible_connection**: Where to run the ansible scripts, normally local

## OpenShift (OCP)

* **api_url**: OCP API address 'https://api.lab.linsol.local:6443'
* **api_key**: Token to use for authenticating to OCP


## Labels

Labels are applied to all created resources:

* **env**: e.g. dev, staging, production
* **app**: Used for "app=" label, will become e.g. app=voting

## Project

* **project_name**: The OpenShift project name, e.g. fevermap-staging
* **manage_projects**: Whether on not to create/delete projects. In OCP-Online
  we don't.
* **redhat_io_pull_token**: While using Red Hat container base images, one
  needs to have pull token created at registry.redhat.io and inserted as secret
  to OpenShift.
* **quay_push_passwd**: Credentials for pushing images to Quay.io.
* **quay_push_user**: Credentials for pushing images to Quay.io.

## Redis

* **REDIS_PASSWORD**: Redis Password

## Database

* **db_name**: Database name to be created within MariaDB
* **db_user**: Database credentials
* **db_password**: Database credentials
* **db_root_password**: Database credentials for root
* **db_memory_limit**: Pod memory limit

## APP

* **app_build**: true/false whether to build APP. E.g. false in production.
* **app_fqdn**: Public FQDN for your APP. This will be set with SSL certs too.
  Note: This is a list, as we do have several domains for app.
* **app_google_analytics_code**: "{{ vaut_app_google_analytics_code }}"
* **ws_api_url**: "https://{{ api_fqdn }}"
* **ws_app_url**: 'https://app-staging.fevermap.net'
* **app_replicas**: How many instances of APP you need?
* **apm_monitoring_js**: vault_apm_monitoring_js
* **app_image**: Which image to use for APP? In production we fix this to
* **app_memory_limit**: Pod memory limit


## Certbot

* **cb_extra_opts**: Extra parameters for certbot. Usually --test
* **cb_email**: Email address used for Let's Encrypt registration

## Pipelines
* **slack_webhook_url**: Slack incoming webhook url.
