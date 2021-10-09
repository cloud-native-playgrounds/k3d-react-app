# k3d-react-app

![image](https://user-images.githubusercontent.com/35857179/136643657-56c8a9c1-34ce-46e9-9f52-dfcdf09ea74b.png)

## Getting started

```
npm i
```

```
npm run start
```

## Docker

```
docker build -t k3d-react-app .
```

```
docker run -it -p 8080:80 k3d-react-app
```

## k3d Setup

### Install k3d (this repository is based on v5.0.0)

```
brew install k3d
```

Create a cluster 

> Without opening a port during the creation of the k3d cluster, a nodeport service will not expose your app

```
k3d cluster create k3d-react-app -p "80:30080@agent[0]" --agents 2
```

Run the following command to verify you've two agents

```
kubectl get node -o wide
```

Export ``kubeconfig``

```
export KUBECONFIG="$(k3d kubeconfig write k3d-react-app)"
```

## Applying manifests

Create a secret for pulling images from a private ECR repository

```
kubectl create secret docker-registry aws-ecr-credential \
  --docker-server=<aws-account-id>.dkr.ecr.<aws-region>.amazonaws.com \
  --docker-username=AWS \
  --docker-password=$(aws ecr get-login-password)
```

Apply ``Deployment``

```
kubectl apply -f deployment.yml
```

Apply ``Service``

```
kubectl apply -f service.yml
```

## Verifying the result  

Go to ``http://localhost/``

## Clean up 

```
kubectl delete service,deployment k3d-react-app
```