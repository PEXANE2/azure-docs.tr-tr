---
title: Özel bir traefik giriş denetleyicisi kullanma ve https 'yi yapılandırma
services: azure-dev-spaces
ms.date: 12/10/2019
ms.topic: conceptual
description: Azure Dev Spaces özel bir traefik ingınress denetleyicisi kullanmak ve bu giriş denetleyicisini kullanarak HTTPS 'yi yapılandırmak için nasıl yapılandırılacağını öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s
ms.openlocfilehash: 125e055ba2b2c9ccbd7b9e53d02850aa6a0e6350
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79366045"
---
# <a name="use-a-custom-traefik-ingress-controller-and-configure-https"></a>Özel bir traefik giriş denetleyicisi kullanma ve https 'yi yapılandırma

Bu makalede, Azure Dev Spaces özel bir traefik ingınress denetleyicisi kullanmak üzere nasıl yapılandırılacağı gösterilmektedir. Bu makalede ayrıca, bu özel giriş denetleyicisinin HTTPS kullanmak üzere nasıl yapılandırılacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Hesabınız yoksa [ücretsiz bir hesap][azure-account-create] oluşturabilirsiniz.
* [Yüklü Azure CLI][az-cli].
* [Azure dev Spaces etkinleştirilmiş Azure Kubernetes hizmeti (AKS) kümesi][qs-cli].
* [kubectl][kubectl] yüklendi.
* [Held 3 yüklendi][helm-installed].
* [DNS bölgesi][dns-zone]olan [özel bir etki alanı][custom-domain] . Bu makalede, özel etki alanı ve DNS bölgesinin AKS kümeniz ile aynı kaynak grubunda olduğu varsayılır, ancak farklı bir kaynak grubunda özel bir etki alanı ve DNS bölgesi kullanmak mümkündür.

## <a name="configure-a-custom-traefik-ingress-controller"></a>Özel bir traefik giriş denetleyicisi yapılandırma

Kubernetes komut satırı istemcisi olan [kubectl][kubectl]kullanarak kümenize bağlanın. Kubernetes kümenize bağlanmak üzere `kubectl` yapılandırmak için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKS
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
```

Traefik giriş denetleyicisi HELI grafiğini içeren [resmi kararlı Held deposunu][helm-stable-repo]ekleyin.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

Traefik giriş denetleyicisi için bir Kubernetes ad alanı oluşturun ve `helm`kullanarak yüklemeyi yapın.

> [!NOTE]
> AKS ' de RBAC etkinleştirilmemişse, *--RBAC. Enabled = true* parametresini kaldırın.

```console
kubectl create ns traefik
helm install traefik stable/traefik --namespace traefik --set kubernetes.ingressClass=traefik --set rbac.enabled=true --set fullnameOverride=customtraefik --set kubernetes.ingressEndpoint.useDefaultPublishedService=true --version 1.85.0
```

> [!NOTE]
> Yukarıdaki örnek, giriş denetleyiciniz için genel bir uç nokta oluşturur. Bunun yerine giriş denetleyicinizde özel bir uç nokta kullanmanız gerekiyorsa, *--Set Service. açıklamalarını ekleyin. " Service\\. Beta\\. Kubernetes\\. IO/Azure-Load-dengeleyici-internal "= true* parametresi *helk install* komutuna doğru.
> ```console
> helm install traefik stable/traefik --namespace traefik --set kubernetes.ingressClass=traefik --set rbac.enabled=true --set fullnameOverride=customtraefik --set kubernetes.ingressEndpoint.useDefaultPublishedService=true --set service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-internal"=true --version 1.85.0
> ```
> Bu özel uç nokta, AKS kümesinin dağıtıldığı sanal ağ içinde kullanıma sunulur.

[Kubectl Get][kubectl-get]kullanarak traefik giriş denetleyici hizmetinin IP adresini alın.

```console
kubectl get svc -n traefik --watch
```

Örnek çıktı, *traefik* ad alanındaki tüm hizmetlerin IP adreslerini gösterir.

```console
NAME      TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
traefik   LoadBalancer   10.0.205.78   <pending>     80:32484/TCP,443:30620/TCP   20s
...
traefik   LoadBalancer   10.0.205.78   MY_EXTERNAL_IP   80:32484/TCP,443:30620/TCP   60s
```

[Az Network DNS Record-set A Add-Record][az-network-dns-record-set-a-add-record]kullanılarak DNS bölgenize, traefik HIZMETININ dış IP adresini kullanarak *bir* kayıt ekleyin.

```azurecli
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name *.traefik \
    --ipv4-address MY_EXTERNAL_IP
```

Yukarıdaki örnek *MY_CUSTOM_DOMAIN* DNS bölgesine *bir* kayıt ekler.

Bu makalede, Azure Dev Spaces kullanmayı göstermek için [Azure dev Spaces bisiklet paylaşımı örnek uygulamasını](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) kullanacaksınız. Uygulamayı GitHub 'dan kopyalayıp dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/charts
```

[Values. YAML][values-yaml] dosyasını açın ve aşağıdaki güncelleştirmeleri yapın:
* *< REPLACE_ME_WITH_HOST_SUFFIX >* tüm örneklerini traefik ile değiştirin *. MY_CUSTOM_DOMAIN* *MY_CUSTOM_DOMAIN*için etki alanınızı kullanma. 
* *Kubernetes.io/ingress.class: traefik # özel*giriş ile *Kubernetes.io/ingress.class: traefik-azds # dev Spaces* ile değiştirin. 

Güncelleştirilmiş `values.yaml` dosyasına bir örnek aşağıda verilmiştir:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: traefik  # Custom Ingress
    hosts:
      - dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: traefik  # Custom Ingress
    hosts:
      - dev.gateway.traefik.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
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
http://dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/  Available
http://dev.gateway.traefik.MY_CUSTOM_DOMAIN/         Available
```

`azds list-uris` komutundan ortak URL 'yi açarak *bıkesharingweb* hizmetine gidin. Yukarıdaki örnekte, *bıkesharingweb* hizmeti IÇIN genel URL `http://dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/`.

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
http://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/  Available
http://azureuser1.s.dev.gateway.traefik.MY_CUSTOM_DOMAIN/         Available
```

`azds list-uris` komutundan ortak URL 'yi açarak *azureuser1* alt geliştirme alanında *bıkesharingweb* hizmetine gidin. Yukarıdaki örnekte, *azureuser1* alt dev Space 'teki *bıkesharingweb* hizmeti için genel URL `http://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/`.

## <a name="configure-the-traefik-ingress-controller-to-use-https"></a>Traefik giriş denetleyicisini HTTPS kullanacak şekilde yapılandırma

Traefik ınress denetleyicinizi HTTPS kullanacak şekilde yapılandırırken TLS sertifikasının yönetimini otomatikleştirmek için [CERT-Manager][cert-manager] kullanın. *Certmanager* grafiğini yüklemek için `helm` kullanın.

```console
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.12/deploy/manifests/00-crds.yaml --namespace traefik
kubectl label namespace traefik certmanager.k8s.io/disable-validation=true
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager --namespace traefik --version v0.12.0 jetstack/cert-manager --set ingressShim.defaultIssuerName=letsencrypt --set ingressShim.defaultIssuerKind=ClusterIssuer
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
            class: traefik
```

> [!NOTE]
> Test için, *Kümevereninizi*için kullanabileceğiniz bir [hazırlama sunucusu][letsencrypt-staging-issuer] da vardır.

`letsencrypt-clusterissuer.yaml`uygulamak için `kubectl` kullanın.

```console
kubectl apply -f letsencrypt-clusterissuer.yaml --namespace traefik
```

`helm`kullanarak HTTPS kullanmak için traefik yükseltin.

```console
helm upgrade traefik stable/traefik --namespace traefik --set kubernetes.ingressClass=traefik --set kubernetes.ingressEndpoint.useDefaultPublishedService=true --version 1.85.0 --set ssl.enabled=true --set ssl.enforced=true --set ssl.permanentRedirect=true
```

, *CERT-Manager* ve https kullanımına ilişkin ayrıntıları dahil etmek için [values. YAML][values-yaml] 'yi güncelleştirin. Güncelleştirilmiş `values.yaml` dosyasına bir örnek aşağıda verilmiştir:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: traefik  # Custom Ingress
      cert-manager.io/cluster-issuer: letsencrypt
    hosts:
      - dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
    tls:
    - hosts:
      - dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN
      secretName: dev-bikesharingweb-secret

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: traefik  # Custom Ingress
      cert-manager.io/cluster-issuer: letsencrypt
    hosts:
      - dev.gateway.traefik.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
    tls:
    - hosts:
      - dev.gateway.traefik.MY_CUSTOM_DOMAIN
      secretName: dev-gateway-secret
```

`helm`kullanarak örnek uygulamayı yükseltin:

```console
helm upgrade bikesharing . --namespace dev --atomic
```

*Geliştirme/azureuser1* alt alanındaki örnek uygulamaya gıdın ve HTTPS kullanmak üzere yönlendirildiğini unutmayın. Ayrıca sayfanın yüklendiğine, ancak tarayıcıda bazı hataların gösterildiğine dikkat edin. Tarayıcı konsolu 'nu açmak, HTTP kaynaklarını yüklemeye çalışan bir HTTPS sayfasıyla ilgili hatayı gösterir. Örnek:

```console
Mixed Content: The page at 'https://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/devsignin' was loaded over HTTPS, but requested an insecure resource 'http://azureuser1.s.dev.gateway.traefik.MY_CUSTOM_DOMAIN/api/user/allUsers'. This request has been blocked; the content must be served over HTTPS.
```

Bu hatayı onarmak için [Bıkesharingweb/azds. YAML][azds-yaml] 'yi, *Kubernetes.io/ingress.Class* için *Traefik* ve *$ (hostsuffix)* özel etki alanınızı kullanacak şekilde güncelleştirin. Örnek:

```yaml
...
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik
      hosts:
      # This expands to [space.s.][rootSpace.]bikesharingweb.<random suffix>.<region>.azds.io
      - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.traefik.MY_CUSTOM_DOMAIN
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

`BikeSharingWeb` dizinine gidin ve güncelleştirilmiş BikeSharingWeb hizmetinizi çalıştırmak için `azds up` kullanın.

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