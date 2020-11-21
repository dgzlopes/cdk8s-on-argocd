# Run cdk8s on Argo CD

**Wait, what?**

[Argo CD](https://argoproj.github.io/argo-cd/) is a GitOps continuous delivery tool for Kubernetes, that automates the deployment of the desired state of your applications in the specified target environments.

On the other hand, [cdk8s](https://cdk8s.io/) is an open-source software development framework for defining Kubernetes applications and reusable abstractions using familiar programming languages and rich object-oriented APIs.

This repo explains how to use both together.

## Configure Argo CD

Argo CD allows integrating more config management tools using config management plugins. Following changes are required to configure CDK8s:
- Replace `argoproj/argocd` images with our custom images that contain all the things needed to run cdk8s.
- Register a new cdk8s plugin in `argocd-m` ConfigMap:

```yaml
data:
  configManagementPlugins: |
    - name: cdk8s
      init:                          
        command: ["bash"]
        args: ["-c","<commands to synth your code>"]
      generate:                      
        command: ["bash"]
        args: ["-c", "cat dist/*"]
```

Now you can use the plugin on your apps. Here is an example:
```YAML
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: hello
spec:
  destination:
    namespace: default
    server: 'https://kubernetes.default.svc'
  source:
    path: 'k8s'
    repoURL: 'https://github.com/dgzlopes/my-secret-project.git'
    targetRevision: HEAD
    plugin:
      env:
        - name: CHART
          value: hello
        - name: ENVIRONMENT
          value: dev
      name: cdk8s
  project: default
```
## Docker images

### Typescript

TBD

## Acknowledgements

- To Max Brenner. Your post [Integrating cdk8s with Argo CD](https://brennerm.github.io/posts/integrating-cdk8s-with-argocd.html) was a big inspiration for this repo! Thanks :)