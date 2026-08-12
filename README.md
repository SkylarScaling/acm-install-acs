# acm-install-acs
Deploy ACS to ACM hub and managed clusters using ACM PolicyGenerator and OpenShift GitOps.

## Overview

This repo contains ACM policies that install and configure Red Hat Advanced Cluster Security (ACS) across an ACM hub and its managed clusters. Policies are delivered to the hub via an ArgoCD Application using the ACM PolicyGenerator kustomize plugin.

## Prerequisites

- Red Hat Advanced Cluster Management 2.6+
- OpenShift GitOps (ArgoCD) with the `policy-generator` Config Management Plugin configured
- The `global` ManagedClusterSet bound to the `acm-install-policies` namespace (included in `placements/managed-cluster-set-binding.yaml`)

## Deployment

This repo is deployed by the ArgoCD Application defined in `gitops-install-acs`. Apply that repo to the hub cluster:

```bash
oc apply -f gitops-install-acs/acs/01-acs-policy-application.yaml
```

ArgoCD will use the `policy-generator` CMP to run `kustomize build --enable-alpha-plugins` and apply the resulting ACM policies to the hub.

## Policy Sets

### acs-central (hub cluster)
Installs ACS Central components on the ACM hub:
1. `policy-acs-operator-central` — installs the RHACS operator and Central CR
2. `policy-acs-central-status` — verifies Central deployments are healthy
3. `policy-acs-central-ca-bundle` — creates init-bundle RBAC and Job
4. `policy-acs-sync-resources` — syncs TLS secrets to managed clusters
5. `policy-acs-monitor-certs` — monitors init-bundle certificate expiry
6. `policy-acs-central-ca-bundle-expired` — rotates expired certificates
7. `policy-advanced-managed-cluster-status` — verifies sensor/collector on hub
8. `policy-compliance-operator-install` — installs the Compliance Operator

### acs-secured (all managed clusters)
Installs ACS Secured Cluster components on each managed cluster:
1. `policy-advanced-managed-cluster-security` — installs RHACS operator and SecuredCluster CR
2. `policy-advanced-managed-cluster-status` — verifies sensor/collector are healthy
3. `policy-compliance-operator-install` — installs the Compliance Operator
