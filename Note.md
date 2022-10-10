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

                                                                      

