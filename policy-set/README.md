Reference: https://github.com/stolostron/policy-collection/tree/main/policygenerator/policy-sets/community/openshift-plus

# ACS PolicySet
Dev Notes:

- Need to be added as Subscription Admin before the PolicyGenerator subscription can be applied:
https://github.com/stolostron/policy-collection/blob/main/README.md#subscription-administrator
- Create repo for each item you want to add as a Channel

## Prerequisites
 To install ACS using this PolicySet, you must first have:
 - A supported Red Hat OpenShift Container Platform version
 - A supported Red Hat Advanced Cluster Management for Kubernetes version  

## Installation

The ACS Central component is deployed to the hub cluster.  The ACS Secured Cluster Services and the Compliance Operator are deployed onto all OpenShift managed clusters.

<TODO>Automate This:

Prior to applying the `PolicySet`, perform these steps:

1. Create the namespace `policies`: `oc create ns policies`
2. Prepare for Red Hat OpenShift Data Foundation by adding worker nodes for storage described [here](https://red-hat-storage.github.io/ocs-training/training/ocs4/ocs.html#_scale_ocp_cluster_and_add_new_worker_nodes)
3. Create the namespace `openshift-storage`: `oc create ns openshift-storage`
4. Label the storage namespace: `oc label namespace openshift-storage "openshift.io/cluster-monitoring=true"`
5. To allow for subscriptions to be applied below you must apply and set to enforce the policy [policy-configure-subscription-admin-hub.yaml](https://github.com/stolostron/policy-collection/blob/main/community/CM-Configuration-Management/policy-configure-subscription-admin-hub.yaml) in the policies namespace.
6. Policies are installed to the `policies` namespace.  Make sure the placement bindings match this namespace for the hub and other managed clusters.
   Example yaml to apply a ManagedClusterSetBinding for the policies namespace.
    ```apiVersion: cluster.open-cluster-management.io/v1beta1
    kind: ManagedClusterSetBinding
    metadata:
        name: default
        namespace: policies
    spec:
        clusterSet: default
    ```

Apply the policies using the kustomize command or subscribing to a fork of the repository and pointing to this directory.  See 
the details for using the Policy Generator for more information.  The command to run is `kustomize build --enable-alpha-plugins  | oc apply -f -`
