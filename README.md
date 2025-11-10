# Introducción

Este repositorio contiene el desarrollo completo del **Laboratorio de Kubernetes (Práctica 3)** correspondiente a la asignatura *Sistemas y Servicios en la Nube*.
El propósito del laboratorio es comprender y aplicar los conceptos fundamentales del ecosistema Kubernetes mediante la instalación de **Minikube**, la creación de contenedores personalizados, la gestión básica de despliegues, escalado, servicios, y finalmente la implementación de una aplicación multi-servicio: **WordPress + MySQL** con volúmenes persistentes.

---

## Estructura del laboratorio

La práctica se divide en cuatro partes:

### 1️⃣ Instalación de Minikube y kubectl

* Instalación y verificación del entorno local de Kubernetes usando **Minikube** sobre Docker.
* Validación de la configuración con:

  ```bash
  minikube status
  kubectl version
  ```

---

### 2️⃣ Gestión básica de Kubernetes

* Creación de una imagen personalizada basada en **NGINX** incluyendo un `index.html` con mensaje dinámico.
* Construcción de la imagen tanto en Docker local como en el daemon interno de Minikube.
* Despliegue de un **Pod** manual (`ioepod.yaml`) usando la imagen personalizada.
* Acceso al contenido mediante `kubectl proxy`:

  ```
  http://127.0.0.1:8001/api/v1/namespaces/default/pods/ioepod/proxy/
  ```
* Pruebas desde navegador y terminal.

---

### 3️⃣ Escalado y actualización de servicios

* Creación de un **Deployment** con 3 réplicas.
* Configuración de un **Service NodePort** para exponer el servicio.
* Acceso a través de:

  ```bash
  minikube service ioesvc
  ```
* Observación de la relación Deployment → ReplicaSet → Pods.
* Escalado horizontal:

  ```bash
  kubectl scale deployment ioedeploy --replicas=5
  ```
* Pruebas de comportamiento tras eliminar Pods y ReplicaSets.
* Revisión del historial de despliegues:

  ```bash
  kubectl rollout history deployment ioedeploy
  ```
* Ejecución de rollback:

  ```bash
  kubectl rollout undo deployment ioedeploy --to-revision=1
  ```

---

### 4️⃣ WordPress + MySQL con volúmenes persistentes

* Creación de un **Secret** para contraseña de MySQL:

  ```bash
  kubectl create secret generic mysql-pass --from-literal=password=<PASSWORD>
  ```
* Creación de dos PVCs de **50Mi** para MySQL y WordPress.
* Despliegue de:

  * `wordpress-mysql` (Deployment + Service tipo ClusterIP)
  * `wordpress` (Deployment + Service tipo LoadBalancer)
* Uso de volúmenes persistentes para garantizar integridad de datos.
* Acceso a WordPress mediante:

  ```bash
  minikube service wordpress
  ```

  o vía `minikube tunnel` para obtener IP externa.
* Verificación de servicios:

  ```
  kubectl get services wordpress
  ```

---

## 📂 Estructura del repositorio

```
Lab3/
│
├── ioe-nginx/
│   ├── Dockerfile
│   ├── index.html
│   ├── conf/
│   │   └── default.conf
│   └── ioepod.yaml
│
├── ioedeploy.yaml
├── ioesvc.yaml
│
├── wordpress/
│   ├── mysql-pvc.yaml
│   ├── wordpress-pvc.yaml
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── wordpress-deployment.yaml
│   └── wordpress-service.yaml
│
└── README.md
```

Si quieres, puedo agregar un apartado final con **capturas de pantalla referenciadas** o un **diagrama de arquitectura** hecho en ASCII para el repo.
