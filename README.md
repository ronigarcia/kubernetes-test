# Kubernetes Manifests Test

This repository contains the necessary YAML files to deploy resources in Kubernetes as specified in the test. The resources include Cluster Autoscaler, Metrics Server, Kube State Metrics, and an HTTPBin application with automatic scaling.

## Requirements

- Git
- kubectl
- A configured and accessible Kubernetes cluster
- Proper permissions to deploy resources to the cluster
- Autoscalling group with the follow tags:
```
k8s.io/cluster-autoscaler/enabled
k8s.io/cluster-autoscaler/MeuClusterName
```
- AWS Role with follow policy
```
{
  "Version": "2012-10-17",
  "Statement": [
   {
        "Effect": "Allow",
        "Action": [
          "autoscaling:DescribeAutoScalingGroups",
          "autoscaling:DescribeAutoScalingInstances",
          "autoscaling:DescribeLaunchConfigurations",
          "autoscaling:DescribeScalingActivities",
          "autoscaling:DescribeTags",
          "ec2:DescribeInstanceTypes",
          "ec2:DescribeLaunchTemplateVersions"
        ],
        "Resource": ["*"]
    }
  ]
} 
```

### For details about resources and requirements:
https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/cloudprovider/aws/README.md
https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html
https://github.com/kubernetes-sigs/metrics-server

## Directory Structure

The repository is organized as follows:

- `kube-system/`: Contains manifests for resources in the kube-system namespace.
- `production/`: Contains manifests for the HTTPBin application in the production namespace.

## Usage

1. Clone the repository:

```bash
$ git clone https://github.com/ronigarcia/kubernetes-test

$ cd kubernetes-test
```
2. Edit the kube-system/cluster-autoscaler/deployment.yaml and kube-system/cluster-autoscaler/serviceaccount.yaml with your environment information.

```bash
$ kubectl apply -f kube-system/cluster-autoscaler/* --context <kubernetes_cluster> --namespace kube-system

$ kubectl apply -f kube-system/metric-server/* --context <kubernetes_cluster> --namespace kube-system

$ kubectl apply -f kube-system/kube-state-metrics/* --context <kubernetes_cluster> --namespace kube-system
```

3. Deploying the HTTPBin Application in the production Namespace

To deploy the HTTPBin application in the production namespace, follow these steps:

```bash
$ kubectl create namespace production

$ kubectl apply -f production/application/* --context <kubernetes_cluster> --namespace production
```

