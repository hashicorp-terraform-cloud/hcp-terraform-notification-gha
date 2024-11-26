# hcp-terraform-notification-gha

This repository provides an example of integrating HashiCorp Cloud Platform (HCP) Terraform with GitHub Actions for notifications.

## Prerequisites

- HashiCorp Cloud Platform (HCP) Terraform account
- A Github PAT
- A repository, such as this, with  Github Action defined with a [repository dispatch trigger](https://docs.github.com/en/rest/repos/repos?apiVersion=2022-11-28#create-a-repository-dispatch-event) configured.

## Setup

* Clone the Repository

```sh
git clone https://github.com/your-username/hcp-terraform-notification-gha.git
```

* Build the Custom Agent

See [the documentation on custom hooks](https://developer.hashicorp.com/terraform/cloud-docs/agents/hooks) for more details.

```sh
cd agent/
podman build -t hashicorp/tfc-agent:gha .
```

Substitute `podman` for your container build tool of choice.

* Register an agent pool in HCP TF

See [the documentation on agent pools](https://developer.hashicorp.com/terraform/cloud-docs/agents/agent-pools#manage-agent-pools) for more details.

* Run the custom agent

Take care to set the `TFC_AGENT_TOKEN` and the `TFC_AGENT_NAME` correctly

```sh
podman run -e TFC_AGENT_TOKEN -e TFC_AGENT_NAME hashicorp/tfc-agent:gha
```

* Add the required GHA environment variables to your HCP TF workspace

```sh
GITHUB_ACTIONS_PAT_TOKEN=<your github PAT token>
GITHUB_ACTIONS_DISPATCH_ENDPOINT=https://api.github.com/repos/<org>/<repository>/dispatches
```

* Ensure that the workspace is configured to use your custom agent
* Execute a Terraform run on that workspace

Depending on the hook type you've configured, you should see your GHA trigger at the appropriate stage of the TF run.

## A more comprehensive example

This repository is a comprehensive boilerplate customer agent.

https://github.com/straubt1/terraform-agent-mod