# kubernetes-ingress

# Installation
Based on https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/

## 1. Configure RBAC
1. Create a namespace and a service account for the Ingress Controller:

```
$ kubectl apply -f common/ns-and-sa.yaml
```

2. Create a cluster role and cluster role binding for the service account:

```
$ kubectl apply -f rbac/rbac.yaml
```

3. (App Protect only) Create the App Protect role and role binding:

```
$ kubectl apply -f rbac/ap-rbac.yaml
```

4. (App Protect DoS only) Create the App Protect DoS role and role binding:

```
$ kubectl apply -f rbac/apdos-rbac.yaml
```

## 2. Create Common Resources
1. Create a secret with a TLS certificate and a key for the default server in NGINX (below assumes you are in the kubernetes-ingress/deployment directory):

```

```

2. Create a config map for customizing NGINX configuration:

```
kubectl apply -f common/nginx-config.yaml
```

3. Create an IngressClass resource:

```
kubectl apply -f common/ingress-class.yaml
```

## 3. Create Custom Resources
1. Create custom resource definitions for VirtualServer and VirtualServerRoute, TransportServer and Policy resources:

```
kubectl apply -f crds/k8s.nginx.org_virtualservers.yaml
kubectl apply -f crds/k8s.nginx.org_virtualserverroutes.yaml
kubectl apply -f crds/k8s.nginx.org_transportservers.yaml
kubectl apply -f crds/k8s.nginx.org_policies.yaml
```

2. If you would like to use the TCP and UDP load balancing features of the Ingress Controller, create the following additional resources:

Create a custom resource definition for GlobalConfiguration resource:

```
kubectl apply -f crds/k8s.nginx.org_globalconfigurations.yaml
```


3. If you would like to use the App Protect WAF module, create the following additional resources:

Create a custom resource definition for APPolicy, APLogConf and APUserSig:

```
kubectl apply -f crds/appprotect.f5.com_aplogconfs.yaml
kubectl apply -f crds/appprotect.f5.com_appolicies.yaml
kubectl apply -f crds/appprotect.f5.com_apusersigs.yaml
```

4. If you would like to use the App Protect DoS module, create the following additional resources:

Create a custom resource definition for APDosPolicy, APDosLogConf and DosProtectedResource:

```
kubectl apply -f crds/appprotectdos.f5.com_apdoslogconfs.yaml
kubectl apply -f crds/appprotectdos.f5.com_apdospolicy.yaml
kubectl apply -f crds/appprotectdos.f5.com_dosprotectedresources.yaml
```

## 4. Deploy the Ingress Controller
We include two options for deploying the Ingress Controller:

- Deployment. Use a Deployment if you plan to dynamically change the number of Ingress Controller replicas.
- DaemonSet. Use a DaemonSet for deploying the Ingress Controller on every node or a subset of nodes.

1. Run the Ingress Controller
- Use a Deployment. When you run the Ingress Controller by using a Deployment, by default, Kubernetes will create one Ingress Controller pod.

For NGINX, run:

```
kubectl apply -f deployment/nginx-ingress.yaml
```

- Use a DaemonSet: When you run the Ingress Controller by using a DaemonSet, Kubernetes will create an Ingress Controller pod on every node of the cluster.

See also: See the Kubernetes DaemonSet docs to learn how to run the Ingress Controller on a subset of nodes instead of on every node of the cluster.

For NGINX, run:

```
kubectl apply -f daemon-set/nginx-ingress.yaml
```

## 5. Get Access to the Ingress Controller
1. Create a Service for the Ingress Controller Pods
- Use a LoadBalancer service:

Create a service using a manifest for your cloud provider:

```
kubectl apply -f service/loadbalancer.yaml
```
