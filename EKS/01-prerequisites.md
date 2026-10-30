# prerequisites

Before deploying the Robot Shop application on Amazon EKS, install the following tools on your Ubuntu machine.

## 3. Prerequisites – Install Required Tools

Before deploying applications on Amazon EKS, install the following tools on your Ubuntu machine.

### 3.1 Update Ubuntu and Install Utilities

```bash
sudo apt-get update

sudo apt-get install -y \
  curl wget unzip git tar gzip \
  ca-certificates gnupg jq
```

### 3.2 Install AWS CLI v2

AWS CLI is a command-line tool for managing AWS services, including Amazon EKS.

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" \
  -o "awscliv2.zip"

unzip -q awscliv2.zip

sudo ./aws/install --update

aws --version

rm -rf aws awscliv2.zip
```

### 3.3 Install kubectl

`kubectl` is the command-line tool for interacting with Kubernetes clusters.

```bash
KUBECTL_VERSION=$(curl -fsSL https://dl.k8s.io/release/stable.txt)

curl -LO "https://dl.k8s.io/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

rm kubectl

kubectl version --client
```

### 3.4 Install eksctl

`eksctl` is a command-line tool for creating and managing Amazon EKS clusters.

```bash
ARCH=amd64

PLATFORM="$(uname -s)_${ARCH}"

curl -sLO \
  "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_${PLATFORM}.tar.gz"

tar -xzf "eksctl_${PLATFORM}.tar.gz" -C /tmp

sudo install -m 0755 /tmp/eksctl /usr/local/bin/eksctl

rm -f "eksctl_${PLATFORM}.tar.gz" /tmp/eksctl

eksctl version
```

### 3.5 Install Helm

Helm is a package manager for Kubernetes that simplifies application deployment and management using Helm charts.

```bash
curl -fsSL https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 \
  -o get_helm.sh

chmod 700 get_helm.sh

./get_helm.sh

helm version

rm get_helm.sh
```

### 3.6 Configure AWS CLI

After installing AWS CLI, configure your AWS credentials.

```bash
aws configure
```

Enter your AWS details:

```text
AWS Access Key ID: <YOUR_AWS_ACCESS_KEY>
AWS Secret Access Key: <YOUR_AWS_SECRET_KEY>
Default region name: ap-south-1
Default output format: json
```

**Security:** Never upload AWS access keys or secret keys to GitHub. Use IAM roles or AWS IAM Identity Center where possible.

### 3.7 Verify Installation and AWS Account

Verify all installed tools:

```bash
aws --version
kubectl version --client
eksctl version
helm version
```

Check AWS region:

```bash
aws configure get region
```

Verify AWS account and identity:

```bash
aws sts get-caller-identity
```

### 3.8 Connect kubectl to EKS Cluster

After creating your EKS cluster, configure kubectl:

```bash
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name <YOUR_EKS_CLUSTER_NAME>
```

Verify cluster connectivity:

```bash
kubectl get nodes
kubectl get namespaces
kubectl cluster-info
```

### Official Documentation

- [Installing or Updating kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
- [Installing or Updating eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)
- [Installing AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [Configuring AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config)
- [Installing Helm](https://helm.sh/docs/intro/install/)
