[Ondrej Sika (sika.io)](https://sika.io) | <ondrej@sika.io> | [go to course ->](#course) | [join slack](https://sika.link/slack-sikapublic), [open slack](https://sikapublic.slack.com) | [join.sika.io](https://join.sika.io)

# ArgoCD Training

## Course

## Setup

acd alias

```
alias acd="argocd "
```

## Core Concepts

<https://argo-cd.readthedocs.io/en/stable/core_concepts/>

- **Application** A group of Kubernetes resources as defined by a manifest. This is a Custom Resource Definition (CRD).
- **Application source type** Which **Tool** is used to build the application.
- **Target state** The desired state of an application, as represented by files in a Git repository.
- **Live state** The live state of that application. What pods etc are deployed.
- **Sync status** Whether or not the live state matches the target state. Is the deployed application the same as Git says it should be?
- **Sync** The process of making an application move to its target state. E.g. by applying changes to a Kubernetes cluster.
- **Sync operation status** Whether or not a sync succeeded.
- **Refresh** Compare the latest code in Git with the live state. Figure out what is different.
- **Health** The health of the application, is it running correctly? Can it serve requests?
- **Tool** A tool to create manifests from a directory of files. E.g. Kustomize or Ksonnet. See **Application Source Type**.
- **Configuration management tool** See **Tool**.
- **Configuration management plugin** A custom tool.

## Install ArgoCD

Install cluster essentials (ingress-nginx, cert-manager, letsencrypt issuer)

```
make install-essentials
```

Install ArgoCD

```
make install-argocd
make install-essentials
```

Get `admin` initial password

```
slu argocd initial-password
```

Copy to pasteboard (Mac)

```
slu argocd initial-password | pbcopy
```

See

<https://argocd.k8s.sikademo.com>


## Create basic ArgoCD App

Using CLI

```
argocd app create example-1 --repo https://github.com/argocd-training/example-1.git --path . --dest-server https://kubernetes.default.svc --dest-namespace default
```

Declarative

```
kubectl apply -f argocd-app-example-1.yml
```

## Create ArgoCD App from own repository

Fork [argocd-training/example-1](https://github.com/argocd-training/example-1) to Gitlab.

Create ArgoCD App using CLI

```
argocd app create example-1-own --repo https://gitlab.sikademo.com/ondrejsika/example-1.git --path . --dest-server https://kubernetes.default.svc --dest-namespace example-1-own
```

See YAML:

```
kubectl -n argocd get app example-1-own -o yaml
```

## Manual Sync

Make changes to your repository ([gitlab.sikademo.com/ondrejsika/example-1](https://gitlab.sikademo.com/ondrejsika/example-1)) and see if ArgoCD propagate those changes to cluster.

You can click **refresh** to fetch new repo version.

Nothing is updated, you can see diff of current state and desired state.

If you want to apply those new changes, you have to click **sync**.

## CLI

### argocd CLI

Login

```bash
acd login <argocd_domain_port>
```

Login "without" password

```bash
acd login $(slu acd domain) --username admin --password $(slu acd initial-password)
```

### slu CLI

Get Initial `admin` password

```
slu argocd initial-password
```

```
slu acd ip
```

```
slu acd ip | pbcopy
```

Get / Open ArgoCD URL

```
slu argocd get
```

```
slu argocd open
```

Reset `admin` password

```
slu argocd password-reset
```

```
slu acd pr
```

## Thank you! & Questions?

That's it. Do you have any questions? **Let's go for a beer!**

### Ondrej Sika

- email: <ondrej@sika.io>
- web: <https://sika.io>
- twitter: [@ondrejsika](https://twitter.com/ondrejsika)
- linkedin: [/in/ondrejsika/](https://linkedin.com/in/ondrejsika/)
- Newsletter, Slack, Facebook & Linkedin Groups: <https://join.sika.io>

_Do you like the course? Write me recommendation on Twitter (with handle `@ondrejsika`) and LinkedIn (add me [/in/ondrejsika](https://www.linkedin.com/in/ondrejsika/) and I'll send you request for recommendation). **Thanks**._

Wanna to go for a beer or do some work together? Just [book me](https://book-me.sika.io) :)
