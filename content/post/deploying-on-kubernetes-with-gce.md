+++
bigimg = ""
date = "2017-01-06T22:37:18+01:00"
subtitle = ""
title = "Deploying leanmanager on kubernetes with GCE"

+++

Writing a new post after six months and in Christmas... New year, new promises, old projects. I've been quite busy the second half of 2016, but also very happy and satisfied with some personal and professional projects. No more excuses and let's focus in this post.

# Introduction

I want to deploy my [leanmanager Docker image](https://hub.docker.com/r/antonmry/leanmanager) so the bot is available all the time for team but you can choose any Docker image you want to use. I want to use [Google Container Engine](https://cloud.google.com/container-engine/docs/quickstart) and do it everything as much automatic as possible.

## GCE installation

First step, make sure you've created previously a project in the Google Cloud console. If you don't have the Cloud SDK, you are going to need it. It's quite easy to install following [the Google instructions](https://cloud.google.com/sdk/docs/quickstart-linux):

```
cd ~/Software
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-138.0.0-linux-x86_64.tar.gz
tar -zxvf google-cloud-sdk-138.0.0-linux-x86_64.tar.gz
rm google-cloud-sdk-138.0.0-linux-x86_64.tar.gz
./google-cloud-sdk/install.sh
```

**Note**: be careful, last command modifies your .bashrc and it may cause problems.

Now, it's time to log in:

```
gcloud init
```

And now, install kubectl, the client to manage kubernetes:

```
gcloud components install kubectl
```

## Launching the service

The first step is to create the cluster. It may take some time.

```
gcloud container clusters create leanmanager-cluster

```

Ensure kubectl can access to the service:

```
gcloud auth application-default login
```

And now, it's time to launch the leanmanager image:

```
kubectl run leanmanager-node --image=antonmry/leanmanager:latest --env="LEANMANAGER_TOKEN=$LEANMANAGER_TOKEN"
```

**Note:** I have an environment variable `LEANMANAGER_TOKEN` with the token to authenticate to Slack. The bot automatically connects using Websocket but if you want to expose any service, add `--port=8080" to allow access to it. You will need also to create a Load Balancer, the steps are explained [here](https://cloud.google.com/container-engine/docs/quickstart).


## Cleaning the service

To stop the service and delete the cluster:

```
gcloud container clusters delete leanmanager-cluster
```

# Next step: automate it!

Our next step it's going to be to automate all the process. To do it, we'll use [Terraform](https://www.terraform.io).



