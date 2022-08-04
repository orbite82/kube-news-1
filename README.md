# kube-news-2
https://iniciativadevops.com.br/aula2/

#

´´´
~/Iniciativa-Devops/kube-news-2/src
└──> $ docker login
Authenticating with existing credentials...
WARNING! Your password will be stored unencrypted in /home/orbite/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded


~/Iniciativa-Devops/kube-news-2/src
└──> $ docker build -t orbite82/kube-news-2:v1 -f Dockerfile .

~/Iniciativa-Devops/kube-news-2/src
└──> $ docker image ls
REPOSITORY                              TAG            IMAGE ID       CREATED          SIZE
orbite82/kube-news-2                    v1             2e3942c3a2b7   18 seconds ago   937MB
orbite82/conversao-temperatura-3        v1             6751f493a033   23 hours ago     935MB
<none>                                  <none>         5ff2a0fa887a   23 hours ago     935MB
ghcr.io/k3d-io/k3d-proxy                5.4.4          5a963719cb39   3 weeks ago      42.4MB
ghcr.io/k3d-io/k3d-tools                5.4.4          741f01cb5093   3 weeks ago      18.7MB
rancher/k3s                             v1.23.8-k3s1   e01f2f6c0dd7   5 weeks ago      212MB
node                                    16.15.1        b9f398d30e45   5 weeks ago      907MB
gcr.io/k8s-minikube/kicbase             v0.0.30        1312ccd2422d   5 months ago     1.14GB
hashicorp/terraform                     1.0.9          115504c439c9   9 months ago     104MB
quay.io/terraform-docs/terraform-docs   0.16.0         746427358d76   10 months ago    20.9MB

~/Iniciativa-Devops/kube-news-2/src
└──> $ docker push orbite82/kube-news-2:v1

~/Iniciativa-Devops/kube-news-2/src
└──> $ docker tag orbite82/kube-news-2:v1 orbite82/kube-news-2:latest

~/Iniciativa-Devops/kube-news-2/src
└──> $ docker push orbite82/kube-news-2:latest
The push refers to repository [docker.io/orbite82/kube-news-2]
2bb1969ef240: Layer already exists 
cd9f6287060f: Layer already exists 
588576962777: Layer already exists 
7598fed4c00d: Layer already exists 
27c708b9165c: Layer already exists 
ff8e37fde8f2: Layer already exists 
a09864edc19a: Layer already exists 
6373ee9ffddc: Layer already exists 
e6fd4ebbaaab: Layer already exists 
261e5d6450d3: Layer already exists 
65d22717bade: Layer already exists 
3abde9518332: Layer already exists 
0c8724a82628: Layer already exists 
latest: digest: sha256:5095aeed0f24bc904750f7d79923c30b91fdf1d30888b22b2b4665410dedaa49 size: 3052

# Manifestos do K8s

´´´
~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl get all
NAME                            READY   STATUS    RESTARTS   AGE
pod/kubenews-64c48f5c64-kl57l   1/1     Running   0          9s
pod/postgre-d95fbbd5b-7j9zn     1/1     Running   0          2m8s

NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/kube-news    NodePort    10.43.61.233   <none>        80:30000/TCP   2m8s
service/kubernetes   ClusterIP   10.43.0.1      <none>        443/TCP        86m
service/postgre      ClusterIP   10.43.70.167   <none>        5432/TCP       2m8s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kubenews   1/1     1            1           2m8s
deployment.apps/postgre    1/1     1            1           2m8s

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/kubenews-64c48f5c64   1         1         1       9s
replicaset.apps/kubenews-cc56cdfb8    0         0         0       2m8s
replicaset.apps/postgre-d95fbbd5b     1         1         1       2m8s
´´´

´´´
~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl port-forward service/postgre 5432:5432
Forwarding from 127.0.0.1:5432 -> 5432
Forwarding from [::1]:5432 -> 5432
Handling connection for 5432
Handling connection for 5432

´´´

# Alterar a versão do src->views->partial->nav-bar.ejs

´´´
/Iniciativa-Devops/kube-news-2/k8s
└──> $ cd ..

~/Iniciativa-Devops/kube-news-2
└──> $ ls
k8s  LICENSE  README.md  src

~/Iniciativa-Devops/kube-news-2
└──> $ cd src/

~/Iniciativa-Devops/kube-news-2/src
└──> $ docker build -t orbite82/kube-news-2:v2 .

~/Iniciativa-Devops/kube-news-2/src
└──> $ docker push orbite82/kube-news-2:v2 

~/Iniciativa-Devops/kube-news-2/src
└──> $ docker tag orbite82/kube-news-2:v2 orbite82/kube-news-2:latest

~/Iniciativa-Devops/kube-news-2/src
└──> $ docker push orbite82/kube-news-2:latest 

/Iniciativa-Devops/kube-news-2/src
└──> $ cd ..

~/Iniciativa-Devops/kube-news-2
└──> $ ls
k8s  LICENSE  README.md  src

:~/Iniciativa-Devops/kube-news-2
└──> $ cd k8s/

~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl apply -f deployment.yaml
deployment.apps/postgre unchanged
service/postgre unchanged
deployment.apps/kubenews configured
service/kube-news unchanged

´´´
# test

localhost:30000/

# Rollback

´´´
~/Iniciativa-Devops/kube-news-2
└──> $ kubectl rollout undo deployment kubenews
deployment.apps/kubenews rolled back
´´´

# Test

localhost:30000/

# Kube Config da Digital Ocean

´´´
~/Iniciativa-Devops/kube-news-2
└──> $ cp -r /home/orbite/Downloads/k8s-iniciativa-kubeconfig.yaml ~/.kube/config 

~/Iniciativa-Devops/kube-news-2
└──> $ kubectl get nodes
NAME            STATUS   ROLES    AGE   VERSION
default-7nuv2   Ready    <none>   68s   v1.23.9
default-7nuvl   Ready    <none>   64s   v1.23.9
default-7nuvp   Ready    <none>   41s   v1.23.9

~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl apply -f deployment.yaml
deployment.apps/postgre created
service/postgre created
deployment.apps/kubenews created
service/kube-news created

~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl get pods
NAME                       READY   STATUS              RESTARTS   AGE
kubenews-bd76546ff-44ll4   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-5fvmd   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-5h8zn   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-77blx   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-8q6jj   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-bj272   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-cmmvd   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-f47l7   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-fd8kz   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-gqwpl   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-jxpxr   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-k95g8   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-k9t7r   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-lltq5   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-qtjvf   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-vlfgt   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-wtfbv   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-wthd9   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-wzblw   0/1     ContainerCreating   0          23s
kubenews-bd76546ff-xwwzw   0/1     ContainerCreating   0          23s
postgre-d95fbbd5b-5pkqz    0/1     ContainerCreating   0          24s

´´´
#  Alterar deployment

type: LoadBalancer

´´´
~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl apply -f deployment.yaml

~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl get all
NAME                           READY   STATUS    RESTARTS   AGE
pod/kubenews-bd76546ff-44ll4   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-5fvmd   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-5h8zn   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-77blx   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-8q6jj   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-bj272   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-cmmvd   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-f47l7   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-fd8kz   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-gqwpl   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-jxpxr   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-k95g8   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-k9t7r   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-lltq5   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-qtjvf   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-vlfgt   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-wtfbv   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-wthd9   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-wzblw   1/1     Running   0          4m21s
pod/kubenews-bd76546ff-xwwzw   1/1     Running   0          4m21s
pod/postgre-d95fbbd5b-5pkqz    1/1     Running   0          4m22s

NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/kube-news    LoadBalancer   10.245.70.116    <pending>     80:30000/TCP   4m21s
service/kubernetes   ClusterIP      10.245.0.1       <none>        443/TCP        10m
service/postgre      ClusterIP      10.245.181.195   <none>        5432/TCP       4m21s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kubenews   20/20   20           20          4m22s
deployment.apps/postgre    1/1     1            1           4m23s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/kubenews-bd76546ff   20        20        20      4m22s
replicaset.apps/postgre-d95fbbd5b    1         1         1       4m23s

´´´
# ok

´´´
NAME                           READY   STATUS    RESTARTS   AGE
pod/kubenews-bd76546ff-44ll4   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-5fvmd   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-5h8zn   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-77blx   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-8q6jj   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-bj272   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-cmmvd   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-f47l7   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-fd8kz   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-gqwpl   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-jxpxr   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-k95g8   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-k9t7r   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-lltq5   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-qtjvf   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-vlfgt   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-wtfbv   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-wthd9   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-wzblw   1/1     Running   0          7m22s
pod/kubenews-bd76546ff-xwwzw   1/1     Running   0          7m22s
pod/postgre-d95fbbd5b-5pkqz    1/1     Running   0          7m23s

NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP       PORT(S)        AGE
service/kube-news    LoadBalancer   10.245.70.116    138.197.228.187   80:30000/TCP   7m22s
service/kubernetes   ClusterIP      10.245.0.1       <none>            443/TCP        13m
service/postgre      ClusterIP      10.245.181.195   <none>            5432/TCP       7m22s

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kubenews   20/20   20           20          7m23s
deployment.apps/postgre    1/1     1            1           7m24s

NAME                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/kubenews-bd76546ff   20        20        20      7m23s
replicaset.apps/postgre-d95fbbd5b    1         1         1       7m24s
´´´
# Trocando a versão do deployment

´´´

NAME                            READY   STATUS    RESTARTS   AGE
pod/kubenews-64c48f5c64-2khm4   1/1     Running   0          44s
pod/kubenews-64c48f5c64-52r7k   1/1     Running   0          51s
pod/kubenews-64c48f5c64-5scfm   1/1     Running   0          43s
pod/kubenews-64c48f5c64-89q2j   1/1     Running   0          44s
pod/kubenews-64c48f5c64-8gz6z   1/1     Running   0          51s
pod/kubenews-64c48f5c64-8w5lp   1/1     Running   0          51s
pod/kubenews-64c48f5c64-9hjxj   1/1     Running   0          51s
pod/kubenews-64c48f5c64-bfhjd   1/1     Running   0          45s
pod/kubenews-64c48f5c64-cqwcp   1/1     Running   0          45s
pod/kubenews-64c48f5c64-fgl52   1/1     Running   0          51s
pod/kubenews-64c48f5c64-jlvn4   1/1     Running   0          42s
pod/kubenews-64c48f5c64-k8lwg   1/1     Running   0          51s
pod/kubenews-64c48f5c64-kgtk8   1/1     Running   0          45s
pod/kubenews-64c48f5c64-lsfv2   1/1     Running   0          51s
pod/kubenews-64c48f5c64-rmbp2   1/1     Running   0          51s
pod/kubenews-64c48f5c64-thkdp   1/1     Running   0          42s
pod/kubenews-64c48f5c64-tpgpz   1/1     Running   0          51s
pod/kubenews-64c48f5c64-vqbx6   1/1     Running   0          51s
pod/kubenews-64c48f5c64-x7t7t   1/1     Running   0          42s
pod/kubenews-64c48f5c64-xqd95   1/1     Running   0          43s
pod/postgre-d95fbbd5b-5pkqz     1/1     Running   0          35m

NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP       PORT(S)        AGE
service/kube-news    LoadBalancer   10.245.70.116    138.197.228.187   80:30000/TCP   35m
service/kubernetes   ClusterIP      10.245.0.1       <none>            443/TCP        42m
service/postgre      ClusterIP      10.245.181.195   <none>            5432/TCP       35m

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/kubenews   20/20   20           20          35m
deployment.apps/postgre    1/1     1            1           35m

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/kubenews-64c48f5c64   20        20        20      52s
replicaset.apps/kubenews-bd76546ff    0         0         0       35m
replicaset.apps/postgre-d95fbbd5b     1         1         1       35m
´´´
´´´
~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl delete -f deployment.yaml
deployment.apps "postgre" deleted
service "postgre" deleted
deployment.apps "kubenews" deleted
service "kube-news" deleted
´´´

# Terraform

https://registry.terraform.io/providers/digitalocean/digitalocean/latest/docs

´´´

~/Iniciativa-Devops/kube-news-2/iac
└──> $ terraform apply --auto-approve
digitalocean_kubernetes_cluster.orbite: Refreshing state... [id=062780a0-a2ce-451b-b9ad-3429edcdf988]
digitalocean_kubernetes_node_pool.bar: Refreshing state... [id=49949b37-e57d-4cc0-b13c-6fff6efd41e9]

Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

kube_endpoint = "https://062780a0-a2ce-451b-b9ad-3429edcdf988.k8s.ondigitalocean.com"



:~/Iniciativa-Devops/kube-news-2/iac
└──> $ terraform apply --auto-approve
╷
│ Error: Could not load plugin
│ 
│ 
│ Plugin reinitialization required. Please run "terraform init".
│ 
│ Plugins are external binaries that Terraform uses to access and manipulate
│ resources. The configuration provided requires plugins which can't be located,
│ don't satisfy the version constraints, or are otherwise incompatible.
│ 
│ Terraform automatically discovers provider requirements from your
│ configuration, including providers used in child modules. To see the
│ requirements and constraints, run "terraform providers".
│ 
│ failed to instantiate provider "registry.terraform.io/hashicorp/local" to obtain schema: unknown provider
│ "registry.terraform.io/hashicorp/local"
│ 
╵

~/Iniciativa-Devops/kube-news-2/iac
└──> $ terraform init

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of digitalocean/digitalocean from the dependency lock file
- Finding latest version of hashicorp/local...
- Using previously-installed digitalocean/digitalocean v2.21.0
- Installing hashicorp/local v2.2.3...
- Installed hashicorp/local v2.2.3 (self-signed, key ID 34365D9472D7468F)

Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://www.terraform.io/docs/cli/plugins/signing.html

Terraform has made some changes to the provider dependency selections recorded
in the .terraform.lock.hcl file. Review those changes and commit them to your
version control system if they represent changes you intended to make.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.


/Iniciativa-Devops/kube-news-2/iac
└──> $ terraform apply --auto-approve
digitalocean_kubernetes_cluster.orbite: Refreshing state... [id=062780a0-a2ce-451b-b9ad-3429edcdf988]
digitalocean_kubernetes_node_pool.bar: Refreshing state... [id=49949b37-e57d-4cc0-b13c-6fff6efd41e9]
local_file.kube_config: Creating...
local_file.kube_config: Creation complete after 0s [id=495a4e839407825d83265120c1c21d8245122ca9]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:

kube_endpoint = "https://062780a0-a2ce-451b-b9ad-3429edcdf988.k8s.ondigitalocean.com"


~/Iniciativa-Devops/kube-news-2/iac
└──> $ ls
kube_config.yaml  main.tf  terraform.tfstate  terraform.tfstate.backup  terraform.tfvars


~/Iniciativa-Devops/kube-news-2/iac
└──> $ kubectl get nodes
Unable to connect to the server: dial tcp: lookup fa294516-fc5d-477f-93ce-7bf92964229d.k8s.ondigitalocean.com on 127.0.0.53:53: no such host


~/Iniciativa-Devops/kube-news-2/iac
└──> $ kubectl get nodes --kubeconfig kube_config.yaml
NAME                STATUS   ROLES    AGE   VERSION
premium-7nmrf       Ready    <none>   34m   v1.23.9
worker-pool-7nmm8   Ready    <none>   61m   v1.23.9
worker-pool-7nmmu   Ready    <none>   61m   v1.23.9


~/Iniciativa-Devops/kube-news-2/iac
└──> $ cp kube_config.yaml ~/.kube/config 

~/Iniciativa-Devops/kube-news-2/iac
└──> $ kubectl get nodes
NAME                STATUS   ROLES    AGE   VERSION
premium-7nmrf       Ready    <none>   35m   v1.23.9
worker-pool-7nmm8   Ready    <none>   61m   v1.23.9
worker-pool-7nmmu   Ready    <none>   62m   v1.23.9

~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl apply -f deployment.yaml
deployment.apps/postgre created
service/postgre created
deployment.apps/kubenews created
service/kube-news created

~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl get pods
NAME                        READY   STATUS              RESTARTS   AGE
kubenews-64c48f5c64-5kvf5   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-5nrnv   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-8hbxf   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-b2nzk   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-ctbd2   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-d4mm7   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-ftvhl   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-gxscd   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-hr7kz   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-jfvj8   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-l4x2m   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-lqrtx   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-m92rc   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-q576x   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-qc6p9   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-t76xg   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-wgttz   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-wq9mv   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-z7gvp   0/1     ContainerCreating   0          21s
kubenews-64c48f5c64-zjsgb   0/1     ContainerCreating   0          21s
postgre-d95fbbd5b-khtr7     0/1     ContainerCreating   0          22s


~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl get services
NAME         TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kube-news    LoadBalancer   10.245.255.110   <pending>     80:30000/TCP   52s
kubernetes   ClusterIP      10.245.0.1       <none>        443/TCP        69m
postgre      ClusterIP      10.245.223.122   <none>        5432/TCP       52s

~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl get replicaset
NAME                  DESIRED   CURRENT   READY   AGE
kubenews-64c48f5c64   20        20        14      74s
postgre-d95fbbd5b     1         1         1       75s


´´´

# Teste

´´´
~/Iniciativa-Devops/kube-news-2/k8s
└──> $ kubectl get services

Every 2,0s: kubectl get services                                                                           afya: Wed Aug  3 17:34:41 2022

NAME         TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)        AGE
kube-news    LoadBalancer   10.245.255.110   206.189.252.34   80:30000/TCP   4m53s
kubernetes   ClusterIP      10.245.0.1       <none>           443/TCP        73m
postgre      ClusterIP      10.245.223.122   <none>           5432/TCP       4m53s

´´´

# Desafio 4

* Criar PipeLine GitHub Actions, Configurar Secrets no GitHub, Terraform apply para subir o cluster na Digital Ocean

´´´
~/Iniciativa-Devops/kube-news-2/iac
└──> $ terraform init

~/Iniciativa-Devops/kube-news-2/iac
└──> $ terraform apply --auto-approve

~/Iniciativa-Devops/kube-news-2/iac
└──> $ cp kube_config.yaml ~/.kube/config

~/Iniciativa-Devops/kube-news-2/iac
└──> $ git pull
´´´