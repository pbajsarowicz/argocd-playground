# Step by Step
Start a Kubernetes Cluster
```commandline
$ minikube start --driver=docker
```

Install Argo CD
```commandline
$ kubectl create namespace argocd
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Install Argo CD CLI
```commandline
$ brew install argocd
```

Access The Argo CD API Server
```commandline
$ kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
$ kubectl get svc argocd-server -n argocd -o=jsonpath='{.status.loadBalancer.ingress[0].ip}'
$ kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Login Using The CLI
```commandline
$ argocd admin initial-password -n argocd
# `--insecure` for dev only 
$ argocd login localhost:8080 --insecure

# Change password
$ argocd account update-password
```

Set The Current Namespace to `argocd`
```commandline
$ kubectl config set-context --current --namespace=argocd
```

Create an Example App
```commandline
$ argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace default
```

Deploy an ApplicationSet
```commandline
$ k apply -f root-application.yaml
```

Expose an App
```commandline
$ kubectl port-forward svc/guestbook-ui -n default 8081:80
```
