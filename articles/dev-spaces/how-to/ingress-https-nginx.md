---
title: Özel NGıNX giriş denetleyicisi kullanma ve HTTPS 'yi yapılandırma
services: azure-dev-spaces
ms.date: 12/10/2019
ms.topic: conceptual
description: Azure Dev Spaces özel bir NGıNX giriş denetleyicisi kullanmak üzere yapılandırmayı ve bu giriş denetleyicisini kullanarak HTTPS 'yi yapılandırmayı öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
ms.openlocfilehash: 2debe6daf409200059f28aef52202223193e8f0c
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78270043"
---
# <a name="use-a-custom-nginx-ingress-controller-and-configure-https"></a>Özel NGıNX giriş denetleyicisi kullanma ve HTTPS 'yi yapılandırma

Bu makalede, Azure Dev Spaces özel bir NGıNX giriş denetleyicisi kullanmak üzere nasıl yapılandırılacağı gösterilmektedir. Bu makalede ayrıca, bu özel giriş denetleyicisinin HTTPS kullanmak üzere nasıl yapılandırılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Hesabınız yoksa [ücretsiz bir hesap][azure-account-create] oluşturabilirsiniz.
* [Yüklü Azure CLI][az-cli].
* [Azure dev Spaces etkinleştirilmiş Azure Kubernetes hizmeti (AKS) kümesi][qs-cli].
* [kubectl][kubectl] yüklendi.
* [Held 3 yüklendi][helm-installed].
* [DNS bölgesi][dns-zone]olan [özel bir etki alanı][custom-domain] .  Bu makalede, özel etki alanı ve DNS bölgesinin AKS kümeniz ile aynı kaynak grubunda olduğu varsayılır, ancak farklı bir kaynak grubunda özel bir etki alanı ve DNS bölgesi kullanmak mümkündür.

## <a name="configure-a-custom-nginx-ingress-controller"></a>Özel NGıNX giriş denetleyicisi yapılandırma

Kubernetes komut satırı istemcisi olan [kubectl][kubectl]kullanarak kümenize bağlanın. Kubernetes kümenize bağlanmak üzere `kubectl` yapılandırmak için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKS
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
$ kubectl get nodes
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
```

NGıNX giriş denetleyicisi HELI grafiğini içeren [resmi kararlı Held deposunu][helm-stable-repo]ekleyin.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

NGıNX giriş denetleyicisi için bir Kubernetes ad alanı oluşturun ve `helm`kullanarak bu adı yüklersiniz.

```console
kubectl create ns nginx
helm install nginx stable/nginx-ingress --namespace nginx --version 1.27.0
```

> [!NOTE]
> Yukarıdaki örnek, giriş denetleyiciniz için genel bir uç nokta oluşturur. Bunun yerine giriş denetleyicinizde özel bir uç nokta kullanmanız gerekiyorsa, *--set Controller. Service. açıklamalarını ekleyin. " Service\\. Beta\\. Kubernetes\\. IO/Azure-Load-dengeleyici-internal "= true* parametresi *helk install* komutuna doğru. Örnek:
> ```console
> helm install nginx stable/nginx-ingress --namespace nginx --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-internal"=true --version 1.27.0
> ```
> Bu özel uç nokta, AKS kümesinin dağıtıldığı sanal ağ içinde kullanıma sunulur.

[Kubectl Get][kubectl-get]kullanarak NGINX giriş DENETLEYICISI hizmetinin IP adresini alın.

```console
kubectl get svc -n nginx --watch
```

Örnek çıktı, *NGINX* ad alanındaki tüm hizmetlerin IP adreslerini gösterir.

```console
NAME                                  TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE
nginx-nginx-ingress-controller        LoadBalancer   10.0.19.39     <pending>        80:31314/TCP,443:30521/TCP   10s
nginx-nginx-ingress-default-backend   ClusterIP      10.0.210.231   <none>           80/TCP                       10s
...
nginx-nginx-ingress-controller        LoadBalancer   10.0.19.39     MY_EXTERNAL_IP   80:31314/TCP,443:30521/TCP   26s
```

[Az Network DNS Record-set A Add-Record][az-network-dns-record-set-a-add-record]kullanılarak, NGINX HIZMETININ dış IP adresiyle DNS bölgenize *bir* kayıt ekleyin.

```console
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name *.nginx \
    --ipv4-address MY_EXTERNAL_IP
```

Yukarıdaki örnek *MY_CUSTOM_DOMAIN* DNS bölgesine *bir* kayıt ekler.

Bu makalede, Azure Dev Spaces kullanmayı göstermek için [Azure dev Spaces bisiklet paylaşımı örnek uygulamasını](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) kullanacaksınız. Uygulamayı GitHub 'dan kopyalayıp dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/charts
```

[Values. YAML][values-yaml] dosyasını açın ve aşağıdaki güncelleştirmeleri yapın:
* *< REPLACE_ME_WITH_HOST_SUFFIX >* tüm örneklerini *NGINX ile değiştirin. MY_CUSTOM_DOMAIN* *MY_CUSTOM_DOMAIN*için etki alanınızı kullanma. 
* *Kubernetes.io/ingress.class: traefik-azds # dev Spaces-* *Kubernetes.io/ingress.class: NGINX # Custom*girişi ile değiştirin. 

Güncelleştirilmiş `values.yaml` dosyasına bir örnek aşağıda verilmiştir:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
    hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
    hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
```

Değişikliklerinizi kaydedin ve dosyayı kapatın.

`azds space select`kullanarak örnek uygulamanızla *dev* alanını oluşturun.

```console
azds space select -n dev -y
```

Örnek uygulamayı `helm install`kullanarak dağıtın.

```console
helm install bikesharingsampleapp . --dependency-update --namespace dev --atomic
```

Yukarıdaki örnek, örnek uygulamayı *dev* ad alanına dağıtır.

`azds list-uris`kullanarak örnek uygulamaya erişmek için URL 'Leri görüntüleyin.

```console
azds list-uris
```

Aşağıdaki çıktıda `azds list-uris`örnek URL 'Ler gösterilmektedir.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/  Available
http://dev.gateway.nginx.MY_CUSTOM_DOMAIN/         Available
```

`azds list-uris` komutundan ortak URL 'yi açarak *bıkesharingweb* hizmetine gidin. Yukarıdaki örnekte, *bıkesharingweb* hizmeti IÇIN genel URL `http://dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/`.

> [!NOTE]
> *Bıkesharingweb* hizmeti yerine bir hata sayfası görürseniz, *values. yaml* dosyasındaki *Kubernetes.io/ingress.Class* ek açıklamasını ve **konağını güncelleştirdiğinizi** doğrulayın.

*Geliştirici* altında bir alt alan oluşturmak ve alt geliştirme alanına erişmek Için URL 'leri listelemek üzere `azds space select` komutunu kullanın.

```console
azds space select -n dev/azureuser1 -y
azds list-uris
```

Aşağıdaki çıktı, *azureuser1* alt geliştirme alanındaki örnek uygulamaya erişmek için `azds list-uris` ' den örnek URL 'leri gösterir.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/  Available
http://azureuser1.s.dev.gateway.nginx.MY_CUSTOM_DOMAIN/         Available
```

`azds list-uris` komutundan ortak URL 'yi açarak *azureuser1* alt geliştirme alanında *bıkesharingweb* hizmetine gidin. Yukarıdaki örnekte, *azureuser1* alt dev Space 'teki *bıkesharingweb* hizmeti için genel URL `http://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/`.

## <a name="configure-the-nginx-ingress-controller-to-use-https"></a>NGıNX giriş denetleyicisini HTTPS kullanacak şekilde yapılandırma

NGıNX giriş denetleyicinizi HTTPS kullanacak şekilde yapılandırırken TLS sertifikasının yönetimini otomatikleştirmek için [CERT-Manager][cert-manager] kullanın. *Certmanager* grafiğini yüklemek için `helm` kullanın.

```console
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.12/deploy/manifests/00-crds.yaml --namespace nginx
kubectl label namespace nginx certmanager.k8s.io/disable-validation=true
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager --namespace nginx --version v0.12.0 jetstack/cert-manager --set ingressShim.defaultIssuerName=letsencrypt --set ingressShim.defaultIssuerKind=ClusterIssuer
```

`letsencrypt-clusterissuer.yaml` bir dosya oluşturun ve e-posta alanını e-posta adresiniz ile güncelleştirin.

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: MY_EMAIL_ADDRESS
    privateKeySecretRef:
      name: letsencrypt
    solvers:
      - http01:
          ingress:
            class: nginx
```

> [!NOTE]
> Test için, *Kümevereninizi*için kullanabileceğiniz bir [hazırlama sunucusu][letsencrypt-staging-issuer] da vardır.

`letsencrypt-clusterissuer.yaml`uygulamak için `kubectl` kullanın.

```console
kubectl apply -f letsencrypt-clusterissuer.yaml --namespace nginx
```

, *CERT-Manager* ve https kullanımına ilişkin ayrıntıları dahil etmek için [values. YAML][values-yaml] 'yi güncelleştirin. Güncelleştirilmiş `values.yaml` dosyasına bir örnek aşağıda verilmiştir:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
      cert-manager.io/cluster-issuer: letsencrypt
    hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
    tls:
    - hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN
      secretName: dev-bikesharingweb-secret

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
      cert-manager.io/cluster-issuer: letsencrypt
    hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
    tls:
    - hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN
      secretName: dev-gateway-secret
```

`helm`kullanarak örnek uygulamayı yükseltin:

```console
helm upgrade bikesharing . --namespace dev --atomic
```

*Geliştirme/azureuser1* alt alanındaki örnek uygulamaya gıdın ve HTTPS kullanmak üzere yönlendirildiğini unutmayın. Ayrıca sayfanın yüklendiğine, ancak tarayıcıda bazı hataların gösterildiğine dikkat edin. Tarayıcı konsolu 'nu açmak, HTTP kaynaklarını yüklemeye çalışan bir HTTPS sayfasıyla ilgili hatayı gösterir. Örnek:

```console
Mixed Content: The page at 'https://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/devsignin' was loaded over HTTPS, but requested an insecure resource 'http://azureuser1.s.dev.gateway.nginx.MY_CUSTOM_DOMAIN/api/user/allUsers'. This request has been blocked; the content must be served over HTTPS.
```

Bu hatayı onarmak için aşağıdaki şekilde [Bıkesharingweb/azds. YAML][azds-yaml] 'yi güncelleştirin:

```yaml
...
    ingress:
      annotations:
        kubernetes.io/ingress.class: nginx
        cert-manager.io/cluster-issuer: letsencrypt
      hosts:
      # This expands to [space.s.][rootSpace.]bikesharingweb.<random suffix>.<region>.azds.io
      - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.nginx.MY_CUSTOM_DOMAIN
      tls:
      - hosts:
        - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.nginx.MY_CUSTOM_DOMAIN
        secretName: dev-bikesharingweb-secret
...
```

[Bıkesharingweb/Package. JSON][package-json] ' i, *URL* paketi için bir bağımlılık ile güncelleştirin.

```json
{
...
    "react-responsive": "^6.0.1",
    "universal-cookie": "^3.0.7",
    "url": "0.11.0"
  },
...
```

[Bıkesharingweb/Pages/yardımcılar. js][helpers-js] ' de bulunan *Getapihostasync* yöntemini HTTPS kullanacak şekilde güncelleştirin:

```javascript
...
    getApiHostAsync: async function() {
        const apiRequest = await fetch('/api/host');
        const data = await apiRequest.json();
        
        var urlapi = require('url');
        var url = urlapi.parse(data.apiHost);

        console.log('apiHost: ' + "https://"+url.host);
        return "https://"+url.host;
    },
...
```

`BikeSharingWeb` dizinine gidin ve güncelleştirilmiş *Bikesharingweb* hizmetinizi çalıştırmak için `azds up` kullanın.

```console
cd ../BikeSharingWeb/
azds up
```

*Geliştirme/azureuser1* alt alanındaki örnek uygulamaya gidin ve herhangi bir hata olmadan https kullanmaya yönlendirildiğini görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces birden çok kapsayıcı genelinde daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleriyle veya dallarıyla çalışarak işbirliğine dayalı geliştirmeyi nasıl kolaylaştırabileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Dev Spaces 'de takım geliştirme][team-development-qs]


[az-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-network-dns-record-set-a-add-record]: /cli/azure/network/dns/record-set/a?view=azure-cli-latest#az-network-dns-record-set-a-add-record
[custom-domain]: ../../app-service/manage-custom-dns-buy-domain.md#buy-the-domain
[dns-zone]: ../../dns/dns-getstarted-cli.md
[qs-cli]: ../quickstart-cli.md
[team-development-qs]: ../quickstart-team-development.md

[azds-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/azds.yaml
[azure-account-create]: https://azure.microsoft.com/free
[cert-manager]: https://cert-manager.io/
[helm-installed]: https://helm.sh/docs/intro/install/
[helm-stable-repo]: https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository
[helpers-js]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/pages/helpers.js#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[letsencrypt-staging-issuer]: https://cert-manager.io/docs/configuration/acme/#creating-a-basic-acme-issuer
[package-json]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/package.json
[values-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml