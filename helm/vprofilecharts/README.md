# Helm Deployment Guide – Application

This document explains how to package and deploy  application to Kubernetes using Helm.

---

# 1. Install Helm

First, install Helm on your system.

Example for Linux:

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

Verify installation:

```
helm version
```

---

# 2. Clone the Source Code

Clone the project repository:

```
git clone https://github.com/esraaghazal/CI-CD-end-to-end.git
cd CI-CD-end-to-end
```

---

# 3. Create Helm Chart

Create a new Helm chart directory.

```
mkdir helm
cd helm
helm create vprofilecharts
```

Helm will generate the following structure:

```
vprofilecharts/
│
├── charts/
├── templates/
├── Chart.yaml
└── values.yaml
```

---

# 4. Move Kubernetes YAML Files

Move the Kubernetes deployment and service YAML files into the Helm templates directory.

Example:

```
templates/
   appdeploy.yaml
   service.yaml
   ingress.yaml
```

---

# 5. Update Deployment Template

Modify the image value in **appdeploy.yaml** so that it reads from the Helm values file.

Change this:

```
image: imranvisualpath/vproappdock:latest
```

To:

```
image: {{ .Values.appimage }}
```

---

# 6. Test the Helm Chart

Before deploying to production, test the Helm chart in a test namespace.

Create the namespace:

```
kubectl create namespace test
```

Deploy the application using Helm:

```
helm install --namespace test vprofile-stack /root/CI-CD-end-to-end/helm/vprofilecharts \
--set appimage=imranvisualpath/vproappdock:9
```

Verify deployment:

```
kubectl get pods -n test
kubectl get svc -n test
```

---

# 7. Deploy to Production

After successful testing, create the production namespace:

```
kubectl create namespace prod
```

Then deploy the application to production:

```
helm install --namespace prod vprofile-stack /root/CI-CD-end-to-end/helm/vprofilecharts \
--set appimage=imranvisualpath/vproappdock:9
```

---

# 8. Upgrade Application

To upgrade the application with a new Docker image:

```
helm upgrade vprofile-stack helm/vprofilecharts \
--set appimage=<new-docker-image> \
--namespace prod
```

---

# 9. Verify Deployment

Check running resources:

```
kubectl get all -n prod
```

---

# Summary

This Helm chart helps automate Kubernetes deployments by:

* Packaging Kubernetes manifests
* Managing application versions
* Simplifying upgrades and rollbacks
* Supporting different environments (test / production)

Helm makes Kubernetes deployments more consistent and easier to manage.
