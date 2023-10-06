# ACM Demo

This repo contains different functionalities that can be realized with ACM.

> **_NOTE:_**  The policies are bootstrapped with an ArgoCD application. 

> **_NOTE:_**  Details of bootstrap ArgoCD application can be found in this [article](https://gexperts.com/wp/bootstrapping-openshift-gitops-with-rhacm/). There is a video in [GitOps Guide to Galaxy](https://www.youtube.com/watch?v=WcBis_KErIo) channel.

## Policies

### ACM Policies

- Installation of various products
    - Install Gatekeeper using policies
    - Install ODF using policy generator
    - Enable observability with policy generator
    - Install Kyverno with policy generator
    - Install/Manage ArgoCD

- Customization of observability
    - Define custom alerts (examples: memory > 45% and cpu > 60%)
    - Define custom allow metrics (examples: argocd metrics)
    - Define a grafana dashboard for ArgoCD

- Custom policies showing functionalities such as templating
    - In a given cluster, copy secrets whose names start with `source-` from `bsa-source` namespace to `bsa-user10` 
    - Copy secrets from hub cluster, whose names start with `hub-source-` from `bsa-hub-source` namespace to `bsa-user11` 

#### Placements

Various placements are used to show how it can be used:

- ArgoCD is installed to clusters which have `gitops` and `gitops-repo` claims. These are used by the bootstrap application. As a result, each cluster can be bootstrapped dynamically based on these claims. In the hub cluster, bootstrap refers to the same repository where policies are held which are populated by app of apps.
    - `gitops` refers to the overlay (i.e. in the corresponding repository following directory is expected: `bootstrap/overlays/{gitops}`)
    - `gitops-repo` refers to the git repository

- ODF and observability are using hub placement: i.e they are applied to the hub cluster only
- Gatekeeper is installed to the clusters having the OPA label
- Kyverno is installed to all clusters

### Gatekeeper

There is an example gatekeeper policy showing that the namespaces should have framework label. The policy is in dry run mode: Namescape creation will be allowed but will be reported as violation and showed in ACM.

### Kyverno

Three example policies are: namespaces created by `devops-group` should start with `bsa-`, pods created in namespaces starting with `bsa-` namespace must have `team` label and a mutator policy: namespaces created by the `devops-group` are labeled as `framework`.

## ArgoCD

Various examples are shown to show functionalities that are getting enhanced by ACM.

- ArgoCD enhancement with ACM `PolicyGenerator`
- Distribution of ArgoCD instances with ACM policies (decentralized ArgoCD)
- Defining clusters in ArgoCD with `GitOpsCluster` and `Placement`
- Creating user workloads with ArgoCD (3 options)
    - Bootstrap ArgoCD application distributed to the corresponding managed cluster as a Policy. The application uses app-of-apps pattern to configure the cluster (i.e. group definitions and an example httpd app)
    - `clusterDecisionResource` is used to create applications in all clusters including the one not having any ArgoCD. Various placements are defined for different placement decisions.
    - (TP feature) Pull model example: In this example, application set is defined in hub and distributed to the target cluster with the help of `clusterDecisionResource`. It is the target cluster which applies the resource.

During the demo, a total of 3 clusters are uses. One of them is the hub cluster. Among the two managed clusters, one has ArgoCD installation whereas the other doesn't. Note that ArgoCD is installed in the managed cluster with the help of a policy (which is also managing the bootstrap application).

## References

- OCM Policy Collection repo: https://github.com/open-cluster-management-io/policy-collection.git
- ACM/ArgoCD Bootstrap example repo: https://github.com/gnunn-gitops/acm-hub-bootstrap.git
