####  How to Install Helm chart?

```sh
helm repo add stable https://charts.helm.sh/stable
helm install my-app stable/app
```

####  Specify values per env?

```sh
helm install my-app -f values-dev.yaml
```
