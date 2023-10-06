# ACM Demo

This repo contains different functionalities that can be realized with ACM.

> **_NOTE:_**  The policies are bootstrapped with an ArgoCD application. 

> **_NOTE:_**  Details of bootstrap ArgoCD application can be found in this [article](https://gexperts.com/wp/bootstrapping-openshift-gitops-with-rhacm/). There is a video in [GitOps Guide to Galaxy](https://www.youtube.com/watch?v=WcBis_KErIo) channel.

## ACM Policies

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

### Placements

Various placements are used to show how it can be used:

- ArgoCD is installed to clusters which have `gitops` and `gitops-repo` claims. These are used by the bootstrap application. As a result, each cluster can be bootstrapped dynamically based on these claims. In the hub cluster, bootstrap refers to the same repository where policies are held which are populated by app of apps.
    - `gitops` refers to the overlay (i.e. in the corresponding repository following directory is expected: `bootstrap/overlays/{gitops}`)
    - `gitops-repo` refers to the git repository

- ODF and observability are using hub placement: i.e they are applied to the hub cluster only
- Gatekeeper is installed to the clusters having the OPA label
- Kyverno is installed to all clusters

## Gatekeeper

There is an example gatekeeper policy showing that the namespaces should have framework label. The policy is in dry run mode: Namescape creation will be allowed but will be reported as violation and showed in ACM.

## Kyverno

Three example policies are: namespaces created by `devops-group` should start with `bsa-`, pods created in namespaces starting with `bsa-` namespace must have `team` label and a mutator policy: namespaces created by the `devops-group` are labeled as `framework`, pods are labeled with team.

## References

- OCM Policy Collectio repo: https://github.com/open-cluster-management-io/policy-collection.git
- ACM/ArgoCD Bootstrap example repo: https://github.com/gnunn-gitops/acm-hub-bootstrap.git
