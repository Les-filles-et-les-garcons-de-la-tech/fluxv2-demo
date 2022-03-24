# fluxv2-infrastructure-stack-example

This project is inspired by https://github.com/fluxcd/flux2-kustomize-helm-example and some experience with Helm and Kubernetes.

Some usefull documentations : 

Flux V2 : https://toolkit.fluxcd.io/get-started/

Kubernetes : https://kubernetes.io/docs/home/

Helm : https://helm.sh/docs/

Kustomize : https://kubectl.docs.kubernetes.io/installation/kustomize/


Why using FLux2  : 

FLux2 and Helm answer multiple questions you can ask yourself when you work in a company with a kubernetes infrastructure : 
- Where do I store all my "infrastructure" configuration ? 
- How to manage all of those apps on multiple clusters ? 
- How to upgrade those apps easily 
- How can I save my infrastructure with versionning 
- What are the softwares that I should always install and how to do it with multiple environments


Repository structure

```
├── README.md
├── clusters # Cluster configuration 
│   ├── staging # cluster environment config
│   └── production # cluster environment config
├── infrastructure
│   ├── base # Contains apps for infrastructure
│   │   ├── App # Application installed
│   └── staging # contains Custom environment configuration
│       └── values  # Custom values.yaml for staging environment
└── sources # Contains helm repository sources informations
```


# Installation Flux2 : 

Please refer to this documentation :

https://toolkit.fluxcd.io/guides/installation/



## Helm sources : 

We use here all most used or official sources.
Sources are in folder /sources.



--- 

# How to use this project : 

What you should know : 

> - You should not add this repository as a "gitsource" but fork it to your own git repository to avoid production trouble based on modifications on these repo.
> - I recommand you to use this on a fresh install of kubernetes or backup/remove all of your apps you already have before installing this. (you can use existing volumes inside your values files if the related project allows it)
> - Do not use it on a production environment without testing it localy / on a poc cluster first.
> - Use it at your own risks.

## prerequisite : 

- Fork this to your git repo
- Create token to connect Flux to your git repo.
- Whitelist your cluster on your git repo (if needed).
- Read some documentations about flux V2 and at least do the quick install.
- Read some documentations / helm charts of the installed applications.
- Install Flux on your computer (or use it with Docker, this project will not explain how to do it).

## Installation process : 

Example on staging with github : 

```
flux bootstrap github \
    --context=staging \
    --owner=${GITHUB_USER} \
    --repository=${GITHUB_REPO} \
    --branch=main \
    --personal \
    --path=clusters/staging
```

Example on production with gitlab on a folder named infrastructure inside the gitlab root directory : 

```
export GITLAB_TOKEN=<gitlab-token>
flux bootstrap gitlab \
    --context=production \
    --owner=infrastructure \
    --repository=flux-infrastructure-stack \
    --branch=main \
    --path=clusters/production \
    --token-auth --verbose
```