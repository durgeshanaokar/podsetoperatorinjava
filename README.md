# PodSet Operator in Java Using Fabric8 Kubernetes Client

This is a demo operator which implements a simple operator for a custom resource called PodSet which is somewhat equal to ReplicaSet. Here 
is what this resource looks like:
```
apiVersion: demo.k8s.io/v1alpha1
kind: PodSet
metadata:
  name: example-podset
  spec:
    replicas: 5
```

Each PodSet object would have 'x' number of replicas, so this operator just tries to maintain x number of replicas checking whether that number
of pods are running in cluster or not.

## How to Build
```
   mvn clean install
```

## How to Run
```
   mvn exec:java -Dexec.mainClass=io.fabric8.podset.operator.PodSetOperatorMain
```

Make Sure that PodSet Custom Resource Definition is already applied onto the cluster. If not, just apply it using this command:
```
kubectl apply -f src/main/resources/crd.yaml
```

Once everything is set, you can see that operator creating pods in your cluster for PodSet resource:
![Demo Screenshot](https://i.imgur.com/ECNKBjG.png)

## Deploying onto Kubernetes(minikube) using [Eclipse JKube](https://github.com/eclipse/jkube)
- Make Sure that PodSet Custom Resource Definition is already applied onto the cluster. If not, just apply it using this command:
 ```
 kubectl apply -f src/main/resources/crd.yaml
 ```
- Make sure that you have given correct privileges to `ServiceAccount` that would be used by the `Pod`, it's `default` in our case. Otherwise you might get 403 from Kubernetes API server.
```
kubectl create clusterrolebinding default-pod --clusterrole cluster-admin --serviceaccount=default:default
```
- Build Docker Image
```
mvn k8s:build
```
![JKube Build Docker Image](https://i.imgur.com/IXVlZ8e.png)

- Generate Kubernetes Manifests
```
mvn k8s:resource
```
![JKube Generate Kubernetes Manifests](https://i.imgur.com/slDdq3X.png)
- Apply generated Kubernetes Manifests onto Kubernetes
```
mvn k8s:apply
```
![JKube Apply Generated Kubernetes Manifests](https://i.imgur.com/dgp8lX5.png)
