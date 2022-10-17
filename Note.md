## Step:
### 0. Docs
https://viblo.asia/p/kubernetes-practice-trien-khai-nodejs-microservice-tren-kubernetes-phan-2-automatic-update-config-with-argocd-Qbq5QBMJKD8
https://github.com/oktadev/okta-jhipster-k8s-eks-microservices-example

### 1. Install okta, ubuntu or window
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
    choco install okta
    okta register
dien doan diendoanq@gmail.com/Congacon@123
domain: https://dev-32565941.okta.com

okta.env
    export SPRING_SECURITY_OAUTH2_CLIENT_PROVIDER_OIDC_ISSUER_URI="https://dev-32565941.okta.com/oauth2/default"
    export SPRING_SECURITY_OAUTH2_CLIENT_REGISTRATION_OIDC_CLIENT_ID="0oa6sys5khYgWFtXB5d7"
    export SPRING_SECURITY_OAUTH2_CLIENT_REGISTRATION_OIDC_CLIENT_SECRET="0bG5Trhyx8Aq7UsIEkoAIsWpF3TlswwYFZ3E7_Lg"


### ArgoCD
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    #CLI 
    curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
    chmod +x /usr/local/bin/argocd

Test

    kubectl port-forward svc/argocd-server -n argocd 8080:443

Password

    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    Output: uclB83E1TTGz4OIO

                                                                      

### Install Nginx Controller
https://devopscube.com/setup-ingress-kubernetes-nginx-controller/
https://arnavtripathy98.medium.com/solution-how-to-deploy-argo-cd-dashboard-over-nginx-ingress-controller-926d8a540844

We need to deploy the following objects to have a working Nginx controller.

    ingress-nginx namespace
    Service account/Roles/ClusterRoles for Nginx admission controller
    Validating webhook Configuration
    Jobs to create/update Webhook CA bundles
    Service account/Roles/ClusterRoles of Nginx controller deployment
    Nginx controller configmap
    Services for nginx controller & admission controller
    Ingress controller deployment

- Cai dat elk,
https://viblo.asia/p/k8s-phan-13-logging-tren-k8s-su-dung-elk-section2-63vKjgzMZ2R

cai dat zookeeper,kafka

    cd zookeeper-k8s
    helm -n jhipster install zookeeper -f values.yaml zookeeper
    kubectl port-forward --namespace jhipster svc/zookeeper 2181:2181 &
    zkCli.sh 127.0.0.1:218

    cd kafka-k8s
    helm -n jhipster install kafka -f value-kafka.yaml kafka

Test

    kubectl run kafka-client --restart='Never' --image docker.io/bitnami/kafka:2.8.0-debian-10-r0 --namespace jhipster --command -- sleep infinity    
    kafka-console-consumer.sh --bootstrap-server kafka.jhipster.svc.cluster.local:9092 --topic test --from-beginning

Helm install elasticsearch
    cd elk/elastic-elasticsearch
    helm -n jhipster install elasticsearch -f value-elasticsearch.yaml elasticsearch
    cd elk/elastic-kibana
    helm -n jhipster install kibana -f value-kibana.yaml kibana
    cd elk/elastic-logstash
    helm -n jhipster install logstash -f value-logstash.yaml logstash
    cd elk/elastic-filebeat
    helm -n jhipster install filebeat -f value-filebeat.yaml filebeat

Reduce size of elasticsearch : https://staffordwilliams.com/blog/2021/02/01/minimal-elasticsearch-resources-in-kubernetes/
Singple pod elastic search will need to have https://stackoverflow.com/questions/64533991/k8s-elasticsearch-with-filebeat-is-keeping-not-ready-after-rebooting
