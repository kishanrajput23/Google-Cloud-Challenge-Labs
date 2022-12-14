
## Task 1 commands:

search kubernetes engine and copy zone from there and paste in below command
```
gcloud container clusters get-credentials production --zone=CHANGE_ZONE
```
```
nano sample.yaml
```
```
  # sandbox-metadata-test.yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: regular-app
    labels:
      app: regular-app
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: regular-app
    template:
      metadata:
        labels:
          app: regular-app
      spec:
        containers:
        - name: regular-app
          image: busybox
          command: ["/bin/sleep","10000"]
```

open editor and edit regular app name to the name given in your lab in task one and then save the file.

```
kubectl create -f sample.yaml
```

## Task 2 command :-
Before running below command change pool name and zone given in your lab under task 2

```
  gcloud container node-pools create POOL_NAME \
  --cluster=production --zone=CHANGE_ZONE --machine-type=n1-standard-2 --image-type=cos_containerd  --sandbox type=gvisor
```
```
kubectl get pod
```

change pod name which you get after running above command

```
kubectl exec -it POD_NAME /bin/sh
```
```
 sudo apt update
 sudo apt install iputils-ping
```

```
ping -c 4 8.8.8.8
```

```
wget "http://metadata.google.internal/computeMetadata/v1/instance/attributes/kube-env" --header "Metadata-Flavor: Google" -O -
```

—---------------—---------------—---------------—---------------—---------------—---------------—------



## Task 3 file:-

change sample.yaml file and now change name according to name given in your lab
```
  # sandbox-metadata-test.yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: unrestricted
    labels:
      app: unrestricted
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: unrestricted
    template:
      metadata:
        labels:
          app: unrestricted
      spec:
        affinity:
          nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: sandbox.gke.io/runtime
                  operator: In
                  values:
                  - gvisor
        tolerations:
          - effect: NoSchedule
            key: sandbox.gke.io/runtime
            operator: Equal
            value: gvisor
        containers:
        - name: unrestricted
          image: busybox
          command: ["/bin/sleep","10000"]
```

Run command :-  

```
kubectl apply -f sample.yaml
```
—---------------—---------------—---------------—---------------—---------------—---------------—---------------



 # Task 4 file:-
 
 change sample.yaml file and now change name according to name given in your lab

```
    # sandbox-metadata-test.yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: unrestricted
    labels:
      app: unrestricted
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: unrestricted
    template:
      metadata:
        labels:
          app: unrestricted
      spec:
        affinity:
          nodeAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
              nodeSelectorTerms:
              - matchExpressions:
                - key: sandbox.gke.io/runtime
                  operator: In
                  values:
                  - gvisor
        tolerations:
          - effect: NoSchedule
            key: sandbox.gke.io/runtime
            operator: Equal
            value: gvisor
        runtimeClassName: gvisor
        containers:
        - name: unrestricted
          image: busybox
          command: ["/bin/sleep","10000"]
```

Run command :-  

```
kubectl apply -f sample.yaml
```

You're done now

