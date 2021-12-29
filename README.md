# tazz

Tazz is a VueJs/Vuex frontend with Ruby on Rails backend project. The MVP is currently still in production but 
its aim is to create a service that allows Twitch Streamers to freelance their skills to fans.


## Project requirements
```
brew install node
brew install chruby ruby-install
ruby-install ruby
gem update --system  // update latest version
gem install bundler 
gem install rails --user-install
brew install vue/cli

Install the following packages:
vue add vuetify
npm install @nuxtjs/vuetify -D
npm install vuetify
npm install sass@~1.32 sass-loader deepmerge -D
docker
kubernetes
```

## Project setup (from scratch)
```
// backend
rails new backend --api

// frontend
npm init 
npm install
vue create frontend 
(VUEX, Unit-Test, 3.x, eslint, prettier, lint and fix on commit)

```

### Lints and fixes files
```
npm run lint
```

### Serve webpage locally
```
npm run serve
Then check http://localhost:8080
```

### Docker build & run from container
```
docker build . -t tazz-ct
docker run -it -p 8080:80 -d --name docker-vuejs-nginx tazz-ct

Then check http://localhost:8080

-it  –  This flag sets the container in Interactive mode and allocate a Dedicated TTY id for later SSHing.
-d –  This flag sets the container to run in the background.
-p 8080:80 – Port Forwarding Between Host and the Container. Right to the colon is a container and Left to the colon is Host. 8080 is the Host and 80 is the container Port.
–name – Once started docker daemon assigns a random string name to the container. But i recommend defining a name can be handy way to add meaning to a container.
tazz-ct – The name of the image from which we are going to create a Container.

docker-compose up -d --build  - build image and run container
docker-compose down - tear down container
```

### Docker extra commands
```
docker ps -all
docker builder prune --all
docker rmi <image id>
```


### install eksctl and create a cluster

```
brew tap weaveworks/tap  # connect to third party apps
brew install weaveworks/tap/eksctl # install eksctl


Example:
eksctl create cluster \
--name tazz-cluster \
--region us-east-2 \
--nodegroup-name linux-nodes \
--node-type t2.micro \
--nodes 2

Check if cluster was created:
kubectl get svc

```

### Install the kubernetes Dashboard
```
Install metrics system:
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl get deployment metrics-server -n kube-system
kubectl get --all-namespaces pods
kubectl logs metrics-server-xxxxxxxxx-yyyy -n kube-system

Install Dashboard:
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.5/aio/deploy/recommended.yaml

Create admin file:
https://docs.aws.amazon.com/eks/latest/userguide/dashboard-tutorial.html
kubectl apply -f eks-admin-service-account.yaml

Copy Token for Dashboard
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-admin | awk ‘{print $1}’)

kubectl proxy
```

### Create the deployment file for Kubernetes and reference the ECS image
```
From above ECS Step, get the URL for the image:
784638312003.dkr.ecr.us-east-2.amazonaws.com/tazz-ct

Create the `deployment.yaml` file with the image.

kubectl apply -f deployment.yaml

kubectl get deployments
kubectl rollout status deployment/tazz-demo # check it


Create the `service.yaml` file with the ports and selector identified in deployment

kubectl apply -f service.yaml

kubectl get services

Identify where are the pods of our application are running:
kubectl get pods -o wide

Identify the public address:
kubectl get nodes -o wide


``` 

## Install awscli automatically
```
brew install awscli
aws configure
aws ec2 describe-regions # check if working
```

### Create aws config files manually
```
copy your keys into ~/.aws/credentials

[default]
aws_access_key_id=<key>
aws_secret_access_key=<secret_key
```


### Docker push to amazon ecs
```
aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 784638312003.dkr.ecr.us-east-2.amazonaws.com
docker tag tazz-ct:latest 784638312003.dkr.ecr.us-east-2.amazonaws.com/tazz-ct:latest
docker push 784638312003.dkr.ecr.us-east-2.amazonaws.com/tazz-ct:latest

```

### References
```

https://towardsdatascience.com/kubernetes-application-deployment-with-aws-eks-and-ecr-4600e11b2d3c


Tutorial Video:
https://www.youtube.com/watch?v=p6xDCz00TxU&ab_channel=TechWorldwithNana

https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html

Use EKS 
https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html


### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
```