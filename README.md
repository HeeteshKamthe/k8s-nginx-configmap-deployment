# k8s-nginx-configmap-deployment
This project demonstrates how to deploy an Nginx web server on Kubernetes using a Deployment, Service, and ConfigMap to manage the Nginx configuration dynamically.

>  Works on Kubernetes clusters created with `kubeadm`.

---

##  Project Structure

k8s-nginx-configmap-deployment/</br>
├── configmap.yml    # Creates a ConfigMap with custom nginx.conf</br>
├── deployment.yml   # Deploys NGINX Pod using the ConfigMap</br>
├── service.yml      # Exposes NGINX using a NodePort service</br>
├── nginx.conf        # Custom NGINX configuration file</br>

---

##  Features

- NGINX container deployment via Deployment
- Custom `nginx.conf` injected using `ConfigMap`
- NGINX exposed using a `NodePort` service
- Access service via browser or curl

---

##  Prerequisites

- A Kubernetes cluster created with `kubeadm`
- `kubectl` configured and connected to the cluster
- At least one **worker node**
- `nginx.conf` already present in the repo

---

##  How to Deploy

### 1️. Clone the Repository

```bash
git clone https://github.com/HeeteshKamthe/k8s-nginx-configmap-deployment.git
cd k8s-nginx-configmap-deployment
```


### 2️. Create the ConfigMap

```bash
kubectl apply -f configmap.yml
```
This creates a ConfigMap named nginx-config, containing the custom nginx.conf.

### 3️. Deploy the NGINX Pod

```bash
kubectl apply -f deployment.yml
```

This will:

- Deploy an NGINX container</br>
- Mount the custom config from the ConfigMap</br>
- Use port 80 in the container

### 4️. Expose the Pod using a NodePort Service

```bash
kubectl apply -f service.yml
```

This exposes the NGINX container using a NodePort (e.g., 30008).

To check which port is assigned:

```bash
kubectl get svc nginx-service
```

### 5️. Access the Application
#### Option A: From Browser (External)

```bash
http://<worker-node-public-ip>:<nodePort>
```


#### Option B: From the Node (Internal)

```bash
curl http://localhost:<nodePort>
```

Example:

```bash
curl http://localhost:30008
```

---

##  Troubleshooting
### ✘ curl localhost:<nodePort> fails:

- Check if the pod is scheduled on a different node

- Check port is open in firewall/security group

- Run: kubectl get pods -o wide to find pod location

### ✘ Service shows <none> as External IP:

- That’s expected for NodePort. Use the node's IP.

### ✘ Port not responding:

- Check if NGINX crashed: kubectl logs <pod-name>




