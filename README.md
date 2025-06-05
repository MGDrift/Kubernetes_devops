Práctica completa de Kubernetes en Google Cloud

Parte 1: Preparación del entorno
Requisitos previos:
•	Cuenta en Google Cloud
•	Google Cloud SDK instalado
•	Configuración inicial con gcloud init
Parte 2: Crear un cluster de Kubernetes en Google Cloud

Paso 1: Verificar autenticación
Comando: gcloud auth list

Retorna:
Credentialed Accounts

ACTIVE: *
ACCOUNT: manuelguzmannavas@gmail.com

To set the active account, run:
    $ gcloud config set account `ACCOUNT`

Paso 2: Verificar proyecto activo

Comando: gcloud config get-value Project

Retorna:
Your active configuration is: [cloudshell-24161]
terraform-459804

Paso 3: Crear cluster GKE (desde la consola)
•	Ir a Google Cloud Console → Kubernetes Engine
•	Crear Cluster → Configurar según necesidades

Paso 4: Obtener credenciales del cluster

Comando: gcloud container clusters get-credentials devops-cluster-2 --zone us-central1 --project terraform-45980

Objetivo: Configura kubectl para conectarse a tu cluster específico. kubectl es la herramienta para interactuar con Kubernetes.

Retorna: 
Fetching cluster endpoint and auth data.
kubeconfig entry generated for devops-cluster-2.

Paso 5: Verificar conexión

Comando: kubectl cluster-info

Retorna: Kubernetes control plane is running at https://35.224.72.80

Parte 3: Desplegar una aplicación simple

Paso 1: Crear archivo de Deployment

Objetivo: Un Deployment define cómo desplegar y gestionar un conjunto de pods idénticos. Especifica cuántas réplicas quieres (2), qué imagen usar (nginx), y qué puertos exponer.

Paso 2: Aplicar el Deployment

Comando: kubectl apply -f deployment.yaml

Retorna:
deployment.apps/mi-aplicacion created

Paso 3: Crear archivo de Service

Objetivo: Un Service expone tu aplicación a la red. LoadBalancer crea un balanceador de carga externo para acceso desde internet.

Paso 4: Aplicar el Service

Comando: kubectl apply -f service.yaml

Retorna: service/mi-aplicacion-service created

Objetivo: Crea el servicio que permite acceso externo a tus pods.

Paso 5: Verificar el despliegue

Comando: kubectl get pods

Retorna:
NAME                             READY   STATUS    RESTARTS   AGE
mi-aplicacion-6bf654b589-8s774   1/1     Running   0          3m4s
mi-aplicacion-6bf654b589-mlj49   1/1     Running   0          3m3s
Objetivo: Lista todos los pods para ver si están ejecutándose correctamente.
Comando: kubectl get services
Retorna: 
NAME                    TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)        AGE
kubernetes              ClusterIP      34.118.224.1     <none>         443/TCP        21m
mi-aplicacion-service   LoadBalancer   34.118.232.225   35.224.33.78   80:30780/TCP   2m2s

Parte 4: Escalar la aplicación

Paso 1: Escalar el Deployment

Comando: kubectl scale deployment mi-aplicacion --replicas=4

Retorna: deployment.apps/mi-aplicacion scaled

Objetivo: Cambia dinámicamente el número de réplicas de 2 a 4. Kubernetes automáticamente crea 2 pods adicionales.

Paso 2: Verificar el escalado

Comando: kubectl get pods

Retorna: 
NAME                             READY   STATUS    RESTARTS   AGE
mi-aplicacion-6bf654b589-8s774   1/1     Running   0          6m49s
mi-aplicacion-6bf654b589-mlj49   1/1     Running   0          6m48s
mi-aplicacion-6bf654b589-nwptp   1/1     Running   0          63s
mi-aplicacion-6bf654b589-plt4h   1/1     Running   0          63s
Objetivo: Confirma que ahora tienes 4 pods ejecutándose.
Comando: kubectl get deployment mi-aplicación
Retorna:
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
mi-aplicacion   4/4     4            4           7m54s

Objetivo: Muestra el estado del deployment, incluyendo réplicas deseadas vs disponibles.

Parte 5: Limpiar recursos

Paso 1: Eliminar aplicación

Comandos: 
-	kubectl delete -f deployment.yaml 
-	kubectl delete -f service.yaml
  
Retorna:
-	deployment.apps "mi-aplicacion" deleted
-	service "mi-aplicacion-service" deleted
  
Objetivo: Elimina todos los recursos creados por estos archivos YAML (pods, servicios, deployments).


Paso 2: Verificar limpieza

Comandos:
-	kubectl get pods 
-	kubectl get services
  
Retorna:
-	No resources found in default namespace.
-	NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   34.118.224.1   <none>        443/TCP   29m

Objetivo: Confirma que no quedan recursos ejecutándose.

Paso 3: Eliminar el cluster

Comando: gcloud container clusters delete devops-cluster-2 --zone us-central1 --project terraform-459804

Retorna:
Deleting cluster devops-cluster-2...done.                                                                                                               
Deleted [https://container.googleapis.com/v1/projects/terraform-459804/zones/us-central1/clusters/devops-cluster-2].

Objetivo: Elimina completamente el cluster para evitar costos. También elimina todos los nodos y recursos asociados.

¡Práctica completada! 
Quedo hecho el cómo desplegar, escalar y gestionar aplicaciones en Kubernetes usando Google Cloud.
