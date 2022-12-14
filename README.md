# Kubernetes_CI_CD

# k8s (Kubernetes) CI CD from azure devops to azure cloud


for previous setup and connection instructions- check the terraform pipeline/ project:
https://github.com/johnmogi/weightapp_Terraform_CI_CD

How to use this project
1. create an azure cloud service connection
2. create a k8s enviroment connected to [env] k8s
in this case weightapp acts as staging
head over to enviroment and create a new 'staging'
in your connected terminal hit kubectl to verify connection.
then do:
 kubectl config view --minify
 note down server url such as:
https://weightdns-28afa67b.hcp.eastus.azmk8s.io:443

kubectl get serviceAccounts k8sweight -o=jsonpath={.secrets[*].name}
kubectl get serviceAccounts <service-account-name> -n <namespace> -o=jsonpath={.secrets[*].name}

kubectl get secret -n staging -o json
extract and paste it in the following form into the form connection- 
the followinf is just a sample:

"data": {
                "ca.crt": "LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUU2VENDQXRHZ0F3SUJBZ0lSQUpjZzlISVpoUzgxWldpWlBjOGJaWlV3RFFZSktvWklodmNOQVFFTEJRQXcKRFRFTE1Ba0dBMVVFQXhNQ1kyRXQ3FCOHVmc282cUN3djh2NVo1SG13bDdhKzV3UnI1TjcrRFFUc3g0N2VOWTNNQnZBQ0pqVjAKMHVjVTB1a0xFRDZnbGNtUk5oUzB3NWVjSDFjdTZsMytIK0dPclkvdnA3OTAwRjJiNVJIaXF3bE5RWVc0QVdHawp0SWZlV3JRSVZoWS9GYm5uOWc9PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==",
                "namespace": "c3RhZ2luZw==",
                "token": "ZXlKaGJHY2lPaUpTVXpJMU5pSXNJbXRwWkNJNklrZEpOVkpZVUVsS2FFY3hZMUJtWkVKMU4xOVpUR2xXVWtkQ05FSklVVXhNVjJkclJEQjFRMWx0Y1ZVaWZRLmV5SnBjM01pT2lKcmRXSmxjbTVsZEyZ0NFNE9Ka0xMN3AtYzBkSVhyY3Z2X2ZJM3gxS1dGTUhzM0VGNHloVjJCd20zLXBYUW1heVFWY2tjbUJHemt6eS1aTXphSEdqU3E1WTdDY1MyYnRtU1JpYlgxOHpSa3BlSXNrQmdjTEFUSG9vdHpWWTNiY3NSUUZrMEFpMFd0SEFwa1M1bzYzeXZlWVJYalRqcHRNa09NUkZfVzVPMG9iMTV1dXlpSlJfVFJrTFZJc0pKbXdRVXE4LW5QSUdSbWFON09nSTlXVldiQXBNWE5nMmM="
            }



kubectl get secret <service-account-secret-name> -n <namespace> -o json

make sure youre connected to azure using the cli
az account show
otherwise - do login
! if on the enviroment the service is disconnected- do the following
<pre>
az aks Get-Credentials -n <aks_name> -g <rg_name> --overwrite-existing --admin

az aks Get-Credentials -n k8sweight -g weightapp --overwrite-existing --admin

Merged "k8sweight-admin" as current context in /home/john/.kube/config

create a new namespace and set it as default
k create namespace staging
kubectl config set-context --current --namespace=live

</pre>
! now attempt at reconnect automaticly- the connection should be easier now

- no service account?
let's do this:
https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/

<pre>
kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: k8sweight
EOF
kubectl get secrets
k describe k8sweight-token-2294s 

kubectl get k8sweight -o yaml
kubectl get serviceAccounts k8sweight -n staging -o=jsonpath={.secrets[*].name}
kubectl get secret k8sweight -n staging -o json
kubectl get secret k8sweight-token-2294s -o jsonpath='{.data}'
kubectl get secret k8sweight-token-2294s -o 

k describe secret k8sweight-token-2294s 
kubectl get serviceAccounts <service-account-name> -n <namespace> -o=jsonpath={.secrets[*].name}


kubectl get secret <service-account-secret-name> -n <namespace> -o json
kubectl get secret <service-account-secret-name> -n <namespace> -o json
</pre>

3. create a new pipeline
edploy to azure k8s - service port 80? placed as 8080

4. test simple docker with nginx
--- after success exit to branch --- 

after apply
kubectl rollout status deployment frontend

20.81.52.238
<pre>
kubectl get secrets
kubectl describe secrets/vars
kubectl get secret k8sweight-token-2294s -o jsonpath='{.data}'


helm repo add bitnami https://charts.bitnami.com/bitnami 
helm install weightappdb bitnami/postgresql
kubectl get secret --namespace live weightappdb-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d
POSTGRESQL installation with helm:

helm repo add bitnami https://charts.bitnami.com/bitnami
 helm install weightappdb bitnami/postgresql 

 kubectl get secret --namespace default weightappdb-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d

 helm install weightappdb bitnami/postgresql
 
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/cloud/deploy.yaml
 

 <!-- apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
        - path: /apple
          backend:
            serviceName: apple-service
            servicePort: 5678
        - path: /banana
          backend:
            serviceName: banana-service
            servicePort: 5678 -->

</pre>

   # Continuous Integration Process
  - stage: CI_to_staging
    condition: and(succeeded(), eq(variables['build.sourceBranch'], 'refs/heads/master'))

    NOTE-
    allways control deployments- make sure no empty ones running



az aks show --resource-group weightapp --name k8sweight --query "servicePrincipalProfile.clientId"


issues:

1. ssl not configured correctly on agent- no access to do ssh/ helm install.
2. no CI CD process - only CI, need to extend into production-
seek out the last repo with the process




k run --name measurements -p 5432:5432 -e 'POSTGRES_PASSWORD=09YQiXJ3m2Qe' appuserfordb
kubectl run postgres --image=postgres

kubectl run postgres-client --rm --tty -i --restart='Never' --image postgres:11 --env="PGPASSWORD=09YQiXJ3m2Qe" --command -- psql -h postgresdb -U appuserfordb

install postgres with persistance inside k8s on aks 

dep. process-
add a build stage for db - rejected

easiest option:
manually create a pod for db
inside run the postgres docker

https://severalnines.com/blog/using-kubernetes-deploy-postgresql/

postgres-configmap.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  name: postgres-config
  labels:
    app: postgres
data:
  POSTGRES_DB: postgresdb
  POSTGRES_USER: postgresadmin
  POSTGRES_PASSWORD: admin123

last resort - 
rebuild a managed postgresql inside the same vnet (manually) / or i would need peer connection.

best scenerio:
helm chart install postgres statefull

kubectl run postgresql-postgresql-client --rm --tty -i --restart='Never' --namespace staging --image bitnami/postgresql --env="PGPASSWORD=09YQiXJ3m2Qe" --command -- psql --host 0.0.0.0 -U appuserfordb


not sure:
# Create multiple YAML objects from stdin
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep
spec:
  containers:
  - name: busybox
    image: busybox:1.28
    args:
    - sleep
    - "1000000"
---
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep-less
spec:
  containers:
  - name: busybox
    image: busybox:1.28
    args:
    - sleep
    - "1000"
EOF


kubectl apply -f - <<EOF  > postgres-pvc-pv.yaml
kind: PersistentVolume
apiVersion: v1
metadata:
  name: postgres-pv-volume  # Sets PV's name
  labels:
    type: local  # Sets PV's type to local
    app: postgres
spec:
  storageClassName: manual
  capacity:
    storage: 5Gi # Sets PV Volume
  accessModes:
    - ReadWriteMany
  hostPath:
    path: "/mnt/data"
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: postgres-pv-claim  # Sets name of PVC
  labels:
    app: postgres
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteMany  # Sets read and write access
  resources:
    requests:
      storage: 5Gi  # Sets volume size
EOF





kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: k8sweight
EOF

<!-- K
cat << EOF > ~/postgres.yaml 
apiVersion: v1
kind: Pod
metadata:
  name: postgres
  labels:
    app: postgres
spec:
  containers:
  - image: postgres:alpine3.16
    name: postgres
    ports:
    - containerPort: 5432
EOF -->


https://devopscube.com/create-kubernetes-yaml/

kubectl run mypod --image=nginx:latest \
            --labels type=web \
            --dry-run=client -o yaml > mypod.yaml