# Environment Setup

## Pre-requisites

- OpenShift Cluster
- Linux or Mac Workstation
- [3scale SaaS Tenant](https://www.3scale.net/signup)

## 3scale SaaS Environment

- Go to your 3scale SaaS Admin console
- [Generate a new Access Token](https://access.redhat.com/documentation/en-us/red_hat_3scale/2-saas/html/accounts/tokens) that has **write access** to the **Account Management API**
- Save the generated access token for later use:

```sh
export SAAS_ACCESS_TOKEN=123...456
```

- Save the name of your 3scale tenant (the string before `-admin.3scale.net` in your Admin Console) for later use

```sh
export SAAS_TENANT=nmasse-redhat
```

- Navigate to **Audience** > **Accounts** > **Listing**
- Click on **Developer**
- Saver the **Developer** Account ID that is the last part of the URL (after **/buyers/accounts/**)

```sh
export SAAS_DEVELOPER_ACCOUNT_ID=2445582535751
```

## Install Tekton

Create an OpenShift project to hold all your artefacts:

```sh
oc project api-lifecycle
```

Save the name of the project for later use:

```sh
export TEKTON_NAMESPACE=api-lifecycle
```

Install Tekton:

```sh
oc new-project tekton-pipelines
oc adm policy add-scc-to-user anyuid -z tekton-pipelines-controller
oc apply --filename https://storage.googleapis.com/tekton-releases/latest/release.yaml
```

## Generate the 3scale toolbox secret

- First, [install the 3scale toolbox locally](https://github.com/3scale/3scale_toolbox#installation).
- Then, create a secret that contains all your [3scale remotes](https://github.com/3scale/3scale_toolbox/blob/master/docs/remotes.md):

```sh
3scale remote add 3scale-saas "https://$SAAS_ACCESS_TOKEN@$SAAS_TENANT-admin.3scale.net/"
oc create secret generic 3scale-toolbox -n "$TEKTON_NAMESPACE" --from-file="$HOME/.3scalerc.yaml"
```
