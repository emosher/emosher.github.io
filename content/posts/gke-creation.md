---
title: "GKE Cluster Creation"
date: 2022-11-04T13:49:03-04:00
draft: false
---

### Background

Google Kubernetes Engine (GKE) has become one of my favorite ways to easily spin up a Kubernetes cluster.  I find Google Cloud overall to be very easy to use and get started with, and so I've taken to using GKE often.  I use other providers too, but GKE is pretty my go to.  Most often, I'm using a cluster to deploy [Tanzu Application Platform](https://tanzu.vmware.com/application-platform) on top of it.  

### Problem 

I believe that clusters can and should be disposable.  Thus, I find myself recreating clusters often.  While the web UI is a great option, I've also find the need to tweak certain knobs from time to time.  Since I use my clusters with Tanzu Application Platform, I also need certain features for that to work.  I needed to find a way to more repeatably    

### Solution

This solution assumes you have installed and [set up](https://cloud.google.com/sdk/docs/initializing) the `gcloud` CLI, or are running this in the Google Cloud Shell.

```bash
CLUSTER=tap-demo
GKE_VERSION=1.24.5-gke.600
NODE_TYPE=e2-standard-2
NUM_NODES=4
PROJ=my-project

gcloud beta container --project $PROJ clusters create $CLUSTER \
 --region "us-east4" --no-enable-basic-auth \
 --cluster-version $GKE_VERSION --release-channel "regular" \
 --machine-type $NODE_TYPE --image-type "COS_CONTAINERD" \
 --disk-type "pd-balanced" --disk-size "100" --metadata disable-legacy-endpoints=true \
 --scopes "https://www.googleapis.com/auth/devstorage.read_only",\
"https://www.googleapis.com/auth/logging.write",\
"https://www.googleapis.com/auth/monitoring",\
"https://www.googleapis.com/auth/servicecontrol",\
"https://www.googleapis.com/auth/service.management.readonly",\
"https://www.googleapis.com/auth/trace.append",\
"https://www.googleapis.com/auth/ndev.clouddns.readwrite" \
 --max-pods-per-node "110" --num-nodes $NUM_NODES --logging=SYSTEM,WORKLOAD \
 --monitoring=SYSTEM --enable-ip-alias \
 --network "projects/mosher-workspace/global/networks/default" \
 --subnetwork "projects/mosher-workspace/regions/us-east4/subnetworks/default" \
 --no-enable-intra-node-visibility --default-max-pods-per-node "110" \
 --no-enable-master-authorized-networks \
 --addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver --enable-autoupgrade \
 --enable-autorepair --max-surge-upgrade 1 \
 --max-unavailable-upgrade 0 --enable-shielded-nodes
```

### Further Details


Of note for my use case is that I need `"https://www.googleapis.com/auth/ndev.clouddns.readwrite"` in order for [External DNS](https://github.com/kubernetes-sigs/external-dns/blob/master/docs/tutorials/gke.md) to work.  I moved the common things I need to change to variables, but you could certainly move other things there if needed.  For example, you may want a different region but I almost always use `us-east4`.  

Last, I think it's important to point out how I got here - go to the GKE cluster creation screen in the Web UI, and you can select options in the GUI and then select the equivalent "command line" link at the bottom of the page.  From there, you can customize the command to your liking or add it to your own scripting. 

![Command Line](/gke-gui-cli.png "GKE GUI CLI Link")
