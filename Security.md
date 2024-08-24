# Guidelines for Securing Kubernetes on Azure
This is intended as a guide on how to secure a production ready Kubernetes environment running on Azure.
## What is this
When it comes to securing your Kubernetes production environment in an enterprise, there are many tutorials and opinions that exist in the community.
We strive to collect well founded concepts and help the reader to evaluate options and balance tradeoffs.
Feedback welcome!
    > To the editors: See this as a base line - comment on todos, feel free to add clearification on the structure and collect good source references for guidlines. 
    > We should focus on parts that are azure specific on which we will contribute content and reference good content for Kubernetes specific topics.
    > We will start the actual content once we have reviewed this and split the contribution of content.
The severity or importance of each topic is indicated by an emoji in the topic name.
* :boom: Critical
* :fire: High
* :cloud: Medium
* :partly_sunny: Low

## First Principles:
## Security design principles:

* Minimise attack surface
* Defense in depth
* Establish secure defaults
* Apply least privileged access
* Segregation of responsibility
* Integrate security into DevOps
* Trust your sources
* Minimise attack surfaxce
* Apply security in a layered approach
* Integrate security in every process
* Keep security simple

Table of Contents
=================

  * [Setting up environments](./Security_setting_up_environments.md) -> [BulletPoints](#setting-up-environments)
     * Separating environments
     * Setting up/ Validating virtual network
     * Provisioning clusters 
  * [Securing a cluster](./Security_securing_a_cluster.md) -> [BulletPoints](#securing-a-cluster)
     * Securing endpoints for api server and cluster nodes
     * Ensuring authentication and authorization
     * Setting up & keeping least privileged access for common tasks
     * Create administrative boundaries(namespaces) between resources as sample
     * Securing communication paths between namespaces (and nodes)   
     * Continous Monitoring and Auditing of security relevant events
     * Running benchmarks and tests to validate cluster setup
     * Regular maintenance, security and cleanup tasks
     * Configuration best practices        
  *  [Securing workloads](./Security_securing_workloads.md) -> [BulletPoints](#securing-workloads) `Dennis`
     * DenyEscalatingExec, Pod identities, security contexts and pod security policies
     * Securing serviceAccounts and secrets
     * Network segmentation (Ingress/ Egress)
     * Secure images and admission controller
     * Container sandboxes
     * Managing secrets and privileged information
  * Links
* [Setting up environments](./Security_setting_up_environments.md)
    * Separating environments
    * Setting up/ Validating virtual network
    * Provisioning clusters 
* [Securing a cluster](./Security_securing_a_cluster.md)
    * Securing endpoints for api server and cluster nodes
    * Ensuring authentication and authorization
    * Setting up & keeping least privileged access for common tasks
    * Create administrative boundaries(namespaces) between resources as sample
    * Securing communication paths between namespaces (and nodes)   
    * Continous Monitoring and Auditing of security relevant events
    * Running benchmarks and tests to validate cluster setup
    * Regular maintenance, security and cleanup tasks
    * Configuration best practices        
*  [Securing workloads](./Security_securing_workloads.md) `Dennis`
    * DenyEscalatingExec, Pod identities, security contexts and pod security policies
    * Securing serviceAccounts and secrets
    * Network segmentation (Ingress/ Egress)
    * Secure images and admission controller
    * Container sandboxes
    * Managing secrets and privileged information
* Links


Setting up environments
  33 changes: 33 additions & 0 deletions 33  
Security_securing_a_cluster.md
 
Original file line number	Diff line number	Diff line change
@@ -0,0 +1,33 @@
# Securing a cluster

    > Understanding the cluster attack surface
    > Concepts that can be applied to configure and bootstrap authentication in azure
    > Minimizing the blast radius by applying least priviliges inside and outside the cluster
    > Securing and maintaining host vms
    > Monitoring and securing security events and logs

Table of Contents
=================

* [Securing a cluster](./Security_securing_a_cluster.md)
    * Securing endpoints for api server and cluster nodes
    * Ensuring authentication and authorization
    * Setting up & keeping least privileged access for common tasks
    * Create administrative boundaries(namespaces) between resources as sample
    * Securing communication paths between namespaces (and nodes)   
    * Continous Monitoring and Auditing of security relevant events
    * Running benchmarks and tests to validate cluster setup
    * Regular maintenance, security and cleanup tasks
    * Configuration best practices

- [ ] :boom: Master Endpoint security in AKS / ACS-Engine
- [ ] :boom: Securing access to host vms
- [ ] :boom: Configure RBAC
- [ ] :boom: Continous security using tools like Aqua, NeuVektor, Twistlock, SysDig
- [ ] :boom: Configure "dev" namespaces with permissions, rolebindings resource quotas and users
- [ ] :boom: Upgrading and mainting hosts, apparmor, linux capabilities filter, os security patching
- [ ] :fire: Evaluation of security benchbmarks like KubeBench / CSI
- [ ] :cloud: Maintenance of certificate and key rotation, cleanup of docker registry
- [ ] :cloud: Security Impact of activating addons and dashboard
- [ ] :cloud: Encrypted service to service communication across nodes
- [ ] :cloud: Service Endpoints for PaaS Service lockdown
   46 changes: 38 additions & 8 deletions 46 
Security_securing_workloads.md
 
Original file line number	Diff line number	Diff line change
@@ -8,16 +8,46 @@
Table of Contents
=================

  *  [Securing workloads](./Security_securing_workloads.md)
     * DenyEscalatingExec, Pod identities, security contexts and pod security policies
     * Securing serviceAccounts and secrets
     * Network segmentation (Ingress/ Egress)
     * Secure images and admission controller
     * Container sandboxes
     * Managing secrets and privileged information
*  [Securing workloads](./Security_securing_workloads.md)
    * DenyEscalatingExec, Pod identities, security contexts and pod security policies
    * Securing serviceAccounts and secrets
    * Network segmentation (Ingress/ Egress)
    * Secure images and admission controller
    * Container sandboxes
    * Managing secrets and privileged information

- [ ] :fire: Image scanning in azure container registry, ValidatingAdmissionWebhook and third party products like Twistlock, Neuvektor and Aqua
- [ ] :cloud: Ensuring DenyEscalatingExec adimission controllers/ pod security policies, privileged pods, runasroot, volumes, fsGroups, hostports on AKS / ACS-Engine
- [ ] :cloud: Capabilities of filtering network traffic with policies, azure firewall or network appliances
- [ ] :cloud: Container sandboxes, gVisor, kataContainers
- [ ] :fire: Maintaining secrets in HashiCorpVaul, Azure KeyVault, Azure KMS Plugin
- [ ] :fire: Maintaining secrets in HashiCorpVaul, Azure KeyVault, Azure KMS Plugin

## Image protection


For multitentant clusters it is useful to enforce the image pull policy to Always - see AlwaysPullImages admission controller

## Admission controlers

An admission controller is a piece of code that intercepts requests to the Kubernetes API server prior to persistence of the object, but after the request is authenticated and authorized. The controllers consist of the list below, are compiled into the kube-apiserver binary, and may only be configured by the cluster administrator. 

AKS supports the following admission controllers: https://docs.microsoft.com/en-us/azure/aks/faq#what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed

### DenyEscalatingExec 


### Pod security policies

A `Pod Security Policy` is a cluster-level resource that can enforce rules on security aspects of a pod specification and affect the `SecurityContext`that will be applied to a pod and and container. It will be stored as an configuration object that defines conditions that a pod must run with in order to be accepted into the cluster.

> AKS does not support Pod Security Policies today.
## Maintaining secrets

KeyVault Flex Volume: https://github.com/Azure/kubernetes-keyvault-flexvol
Kubernetes KMS plugin: https://github.com/Azure/kubernetes-kms 
Key Vault Agent: https://github.com/Hexadite/acs-keyvault-agent 

## Network policies

Good sample network policies recipes: https://github.com/ahmetb/kubernetes-network-policy-recipes 
  18 changes: 18 additions & 0 deletions 18 
Security_setting_up_environments.md
 
Original file line number	Diff line number	Diff line change
@@ -0,0 +1,18 @@
# Setting up environments

    > Concepts that can be applied to ensure security isolation for different workloads
    > Separating Subscriptions, Resource Groups, Azure RBAC, Service Accounts and Secret

Table of Contents
=================

* [Setting up environments](./Security_setting_up_environments.md)
    * Separating environments
    * Setting up/ Validating virtual network
    * Provisioning clusters 

- [ ] :fire: Cluster vs Nodes vs Namespace isolation
- [ ] :fire: Azure service principals and MSI
- [ ] :cloud: Dedicated nodes / hyper-v isolation on Nodes
- [ ] :fire: Inbound/ Outbound traffic (Forced Tunneling)
- [ ] :fire: Setting up RBAC
  26 changes: 26 additions & 0 deletions 26 
securing_a_cluster/role_deployment_manager.yaml
 
Original file line number	Diff line number	Diff line change
@@ -0,0 +1,26 @@
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: dev
  name: deployment-manager
rules:
- apiGroups: ["extensions", "apps"]
  resources: ["deployments"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: dev
  name: dev-deployment-manager
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: deployment-manager
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: "$DEPLOYMENT_USER_ID"
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: "$DEPLOYMENT_GROUP_ID" 
  23 changes: 23 additions & 0 deletions 23  
securing_a_cluster/role_log_reader.yaml
 
Original file line number	Diff line number	Diff line change
@@ -0,0 +1,23 @@
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: small
  name: pod-and-pod-logs-reader
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "watch", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: small
  name: small-pod-reader
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-and-pod-logs-reader
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: "$READER_USER_ID" 
  23 changes: 23 additions & 0 deletions 23 
securing_workloads/networkpolicy_securing_egress.yaml
 
Original file line number	Diff line number	Diff line change
@@ -0,0 +1,23 @@
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: limit-deny-external-egress
spec:
  podSelector:
    matchLabels:
      run: psqldemo
  policyTypes:
  - Egress
  egress:
  - ports:
    - port: 53
      protocol: UDP
    - port: 53
      protocol: TCP
    - port: 5432
      protocol: TCP
    to:
    - ipBlock:
        cidr: 168.63.129.0/24
    - ipBlock:
        cidr: 191.237.232.0/22 
  24 changes: 24 additions & 0 deletions 24 
securing_workloads/podsecuritypolicy_privileged.yaml
 
Original file line number	Diff line number	Diff line change
@@ -0,0 +1,24 @@
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: privileged
spec:
  fsGroup:
    rule: RunAsAny
  privileged: true
  runAsUser:
    rule: RunAsAny
  seLinux:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  volumes:
  - '*'
  allowedCapabilities:
  - '*'
  hostPID: true
  hostIPC: true
  hostNetwork: true
  hostPorts:
  - min: 1
    max: 65536 
  48 changes: 48 additions & 0 deletions 48 
securing_workloads/podsecuritypolicy_restricted.yaml
 
Original file line number	Diff line number	Diff line change
@@ -0,0 +1,48 @@
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restrictive-policy
  annotations:
    seccomp.security.alpha.kubernetes.io/allowedProfileNames: 'docker/default'
    apparmor.security.beta.kubernetes.io/allowedProfileNames: 'runtime/default'
    seccomp.security.alpha.kubernetes.io/defaultProfileName:  'docker/default'
    apparmor.security.beta.kubernetes.io/defaultProfileName:  'runtime/default'
spec:
  privileged: false
  # Required to prevent escalations to root.
  allowPrivilegeEscalation: false
  # This is redundant with non-root + disallow privilege escalation,
  # but we can provide it for defense in depth.
  requiredDropCapabilities:
    - ALL
  # Allow core volume types.
  volumes:
    - 'configMap'
    - 'emptyDir'
    - 'projected'
    - 'secret'
    - 'downwardAPI'
    # Assume that persistentVolumes set up by the cluster admin are safe to use.
    - 'persistentVolumeClaim'
  hostNetwork: false
  hostIPC: false
  hostPID: false
  runAsUser:
    # Require the container to run without root privileges.
    rule: 'MustRunAsNonRoot'
  seLinux:
    # This policy assumes the nodes are using AppArmor rather than SELinux.
    rule: 'RunAsAny'
  supplementalGroups:
    rule: 'MustRunAs'
    ranges:
      # Forbid adding the root group.
      - min: 1
        max: 65535
  fsGroup:
    rule: 'MustRunAs'
    ranges:
      # Forbid adding the root group.
      - min: 1
        max: 65535
  readOnlyRootFilesystem: false
