# Learning Google Cloud Platform (GCP)

This repository is a small learning project for exploring Google Cloud and
automating a simple `gcloud` command with GitHub Actions.

## What this project does

The workflow in [`.github/workflows/cicd.yaml`](.github/workflows/cicd.yaml):

1. Runs when code is pushed to the `main` branch.
2. Checks out the repository.
3. Authenticates to Google Cloud using Workload Identity Federation.
4. Runs `gcloud compute instances list`.

It currently lists Compute Engine instances; it does not create or deploy an
application.

## Before you run it

You will need:

- A Google Cloud project with billing enabled, if required for the resources you
  use.
- A Google Cloud service account configured for Workload Identity Federation
  with GitHub Actions.
- A GitHub repository with the workflow settings below.

Configure these in the repository's **Settings → Secrets and variables →
Actions**:

- **Variable** `PROJECT_ID`: your Google Cloud project ID.
- **Secret** `WIF_PROVIDER`: the full Workload Identity Provider resource name
  for your GitHub identity provider.

The service account used by the identity provider must have permission to list
Compute Engine instances in the project. For this workflow, grant only the
permissions needed for that operation; avoid broad project-wide roles.

Workload Identity Federation lets GitHub Actions obtain short-lived Google
Cloud credentials without storing a long-lived service-account key in GitHub.
The provider's attribute conditions should also restrict which GitHub
repository or branch can authenticate.

## Learning path

If you are new to GCP, these topics are useful to explore in order:

1. **Projects and IAM** — projects, principals, roles, and least privilege.
2. **gcloud CLI** — install the CLI, select a project, and inspect resources.
3. **Compute Engine** — virtual machine instances, zones, and machine types.
4. **GitHub Actions** — workflows, triggers, jobs, and repository variables.
5. **Workload Identity Federation** — trust between GitHub Actions and Google
   Cloud without service-account key files.

## Try it

After setting up the GitHub and Google Cloud configuration, push a commit to
`main` and open the repository's **Actions** tab to inspect the workflow run.
If authentication or the command fails, check the workflow logs, project ID,
provider resource name, provider attribute conditions, and service-account
permissions.

You can also run the command locally after installing and authenticating the
Google Cloud CLI:

```sh
gcloud config set project YOUR_PROJECT_ID
gcloud compute instances list
```

Replace `YOUR_PROJECT_ID` with your project ID. The local CLI uses your local
Google Cloud credentials; it does not use the GitHub Actions federation setup.

## Cost and safety

Listing instances does not create Compute Engine resources. Creating VMs or
other billable resources in future experiments may incur charges. Review
pricing, clean up resources you no longer need, and never commit credentials,
service-account keys, or other secrets to the repository.
