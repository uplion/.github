# Introduction

UPLION, which stands for Unified Platform for Leveraging Intelligent Operations in AI Networks, aims to revolutionize the deployment and management of AI models. Here’s a breakdown of its components:

**Unified Platform for AI**: UPLION serves as a comprehensive backbone for all AI operations, simplifying the complex AI landscape into an integrated environment.

**Cloud-Native Design**: Built to thrive in the cloud, UPLION ensures scalability, resilience, and seamless updates, meeting the demands of modern AI systems.

**Intelligent Task Distribution**: By employing smart algorithms, UPLION optimizes resource utilization and enhances performance and reliability.

**Resource Optimization**: Dynamic allocation of resources based on demand ensures efficient use of computational power and guarantees availability.

**Kubernetes Integration**: UPLION utilizes Kubernetes for efficient management of worker nodes, introducing a Custom Resource Definition (CRD) and a corresponding operator to implement bespoke logic.

UPLION's architecture is centered around several key elements: the main API service, a message queue, worker nodes, and an AI model operator. The AI model operator is crucial in managing worker nodes. When a client sends an API request, the main API service processes it, places it into the message queue, and a suitable worker node retrieves and processes the task. This design ensures high concurrency and low latency.

Additional services such as authentication and rate limiting, frontend, admin dashboard, account services, and billing service augment UPLION’s functionality. For primary storage, a PostgreSQL database cluster is used, with read replicas and a Redis cluster for caching to enhance performance.

By leveraging Kubernetes' event system, UPLION implements robust error-handling mechanisms, making it a truly Kubernetes-native solution.

In summary, UPLION represents a forward-thinking approach to AI system design, addressing current challenges while preparing for future advancements. It offers a glimpse into the future of resilient, scalable, and efficient AI infrastructures.

# Installation

## How to use

### Prerequisites

You must have the following tools installed and available in your `PATH`:

- [terraform](https://developer.hashicorp.com/terraform/install)
  - could be installed by `winget` or `choco`
- [aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) - `aws` command
- [kubectl](https://kubernetes.io/docs/tasks/tools/) - If you want to interact with the cluster
- [jq](https://jqlang.github.io/jq/) - If you are using the AWS Academy Learner Lab & get the `role_arn` by command line
  - could be installed by `winget` or `choco`
- [helm](https://helm.sh/zh/docs/intro/quickstart/)
  - could be installed by `winget` or `choco`

### Configure AWS credentials

```bash
aws configure
```

or paste credentials in `~/.aws/credentials` file.

### Get `role_arn`

#### If you are using the AWS Academy Learner Lab

If you are using the AWS Academy Learner Lab, I assume you have a role called `LabRole` that has the required permissions. So you can save the role ARN in `terraform.tfvars` with the following command:

Bash:

```bash
echo "role_arn = \"`aws iam get-role --role-name LabRole | jq -r .Role.Arn`\"" | tee terraform.tfvars
```

Powershell:

```powershell
echo "role_arn = `"$(aws iam get-role --role-name LabRole | jq -r .Role.Arn)`"" | tee terraform.tfvars
```

or you can get the role ARN from the AWS console (search IAM -> Roles -> LabRole -> Copy ARN) and paste it in the `terraform.tfvars` file.

Then the `terraform.tfvars` file should look like this:

```
role_arn = "arn:aws:iam::123456789012:role/LabRole"
```

#### If you are using a different account

Otherwise, you need to create a role with `AmazonEC2ContainerRegistryReadOnly`, `AmazonEKSClusterPolicy`, `AmazonEKSWorkerNodePolicy` and `AmazonSSMManagedInstanceCore`. Then run the above command with the role name you created.

### Deploy

```bash
terraform init
terraform apply -auto-approve
```

### Destroy

Destroy the resources to avoid unnecessary charges.

```bash
terraform destroy -auto-approve
```
