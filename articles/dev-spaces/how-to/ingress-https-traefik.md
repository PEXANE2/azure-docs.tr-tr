---
title: Özel bir traefik giriş denetleyicisi kullanın ve HTTPS'yi yapılandırın
services: azure-dev-spaces
ms.date: 12/10/2019
ms.topic: conceptual
description: Azure Dev Spaces'i özel bir traefik giriş denetleyicisi kullanacak şekilde nasıl yapılandırılabildiğini ve bu giriş denetleyicisini kullanarak HTTPS'yi yapılandırmayı öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s
ms.openlocfilehash: fd11b3bbd3f90b75203084ff0753c1485d57a35b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80155438"
---
# <a name="use-a-custom-traefik-ingress-controller-and-configure-https"></a>Özel bir traefik giriş denetleyicisi kullanın ve HTTPS'yi yapılandırın

Bu makalede, azure dev spaces'i özel bir traefik giriş denetleyicisi kullanmak üzere nasıl yapılandırdığınız gösterilmektedir. Bu makalede, https kullanmak için bu özel giriş denetleyicisi nasıl yapılandırılabildiğinizi de gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Hesabınız yoksa [ücretsiz bir hesap][azure-account-create] oluşturabilirsiniz.
* [Yüklü Azure CLI][az-cli].
* [Azure Geliştirme Alanları ile Azure Kubernetes Hizmeti (AKS) kümesi etkinleştirildi.][qs-cli]
* [kubectl][kubectl] yüklü.
* [Helm 3 yüklü][helm-installed].
* [DNS Bölgesi][dns-zone]olan özel bir [etki alanı.][custom-domain] Bu makalede, özel etki alanı ve DNS Bölgesi AKS kümenizle aynı kaynak grubunda olduğunu varsayar, ancak farklı bir kaynak grubunda özel bir etki alanı ve DNS Bölgesi kullanmak mümkündür.

## <a name="configure-a-custom-traefik-ingress-controller"></a>Özel bir traefik giriş denetleyicisi yapılandırma

[Kubectl][kubectl], Kubernetes komut satırı istemcisi kullanarak kümenize bağlanın. `kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut kimlik bilgilerini karşıdan yükler ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKS
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
```

Traefik ingress controller Helm grafiğini içeren [resmi kararlı Miğfer deposunu][helm-stable-repo]ekleyin.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

Traefik giriş denetleyicisi için bir Kubernetes `helm`ad alanı oluşturun ve kullanarak yükleyin.

> [!NOTE]
> AKS kümenizde RBAC etkin değilse, *--set rbac.enabled=true* parametresini kaldırın.

```console
kubectl create ns traefik
helm install traefik stable/traefik --namespace traefik --set kubernetes.ingressClass=traefik --set rbac.enabled=true --set fullnameOverride=customtraefik --set kubernetes.ingressEndpoint.useDefaultPublishedService=true --version 1.85.0
```

> [!NOTE]
> Yukarıdaki örnek, giriş denetleyiciniz için genel bir bitiş noktası oluşturur. Giriş denetleyiciniz için özel bir bitiş noktası kullanmanız gerekiyorsa, *--set service.ek açıklamaları ekleyin." service\\\\.beta\\.kubernetes .io/azure-load-balancer-internal"=true* parametre to the helm *install* komutu.
> ```console
> helm install traefik stable/traefik --namespace traefik --set kubernetes.ingressClass=traefik --set rbac.enabled=true --set fullnameOverride=customtraefik --set kubernetes.ingressEndpoint.useDefaultPublishedService=true --set service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-internal"=true --version 1.85.0
> ```
> Bu özel bitiş noktası, AKS kümenizin dağıtıldığı sanal ağ içinde ortaya çıkarır.

[Kubectl get][kubectl-get]kullanarak traefik ingress denetleyici hizmetinin IP adresini alın.

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

Az *A* [Network dns kaydını][az-network-dns-record-set-a-add-record]kullanarak traefik hizmetinin harici IP adresiyle DNS bölgenize bir kayıt ekleyin.

```azurecli
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name *.traefik \
    --ipv4-address MY_EXTERNAL_IP
```

Yukarıdaki örnek, *MY_CUSTOM_DOMAIN* DNS bölgesine *bir A* kaydı ekler.

Bu makalede, Azure [Dev](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) Spaces kullanarak göstermek için Azure Dev Spaces Bike Paylaşımı örnek uygulamasını kullanırsınız. Uygulamayı GitHub'dan klonla ve dizinine gidin:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/charts
```

[Values.yaml'ı][values-yaml] açın ve aşağıdaki güncelleştirmeleri yapın:
* *<REPLACE_ME_WITH_HOST_SUFFIX>* tüm örneklerini *traefik ile değiştirin. *etki alanınızı *MY_CUSTOM_DOMAIN*için kullanmaya MY_CUSTOM_DOMAIN. 
* *kubernetes.io/ingress.class değiştirin: traefik-azds # Dev Spaces-kubernetes.io/ingress.class* ile *özel: traefik # Özel Ingress*. 

Aşağıda güncelleştirilmiş `values.yaml` bir dosya örneği verilmiştir:

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

Örnek uygulamanızla *dev* alanını `azds space select`oluşturun.

```console
azds space select -n dev -y
```

Örnek uygulamayı kullanarak `helm install`dağıtın.

```console
helm install bikesharingsampleapp . --dependency-update --namespace dev --atomic
```

Yukarıdaki örnek, örnek uygulamayı *dev* ad alanına dağıtır.

Örnek uygulamaya erişmek için URL'leri `azds list-uris`görüntüleyin.

```console
azds list-uris
```

Aşağıdaki çıktı, url'leri `azds list-uris`.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/  Available
http://dev.gateway.traefik.MY_CUSTOM_DOMAIN/         Available
```

Genel URL'yi komuttan açarak *bikesharingweb* hizmetine `azds list-uris` gidin. Yukarıdaki örnekte, *bikesharingweb* hizmetinin genel `http://dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/`URL'si .

> [!NOTE]
> *Bikesharingweb* hizmeti yerine bir hata sayfası görürseniz, *values.yaml* dosyasındaki *hem kubernetes.io/ingress.class* ek açıklamasını **hem de** ana bilgisayarı güncelleştirdiğinizi doğrulayın.

Dev `azds space select` altında bir alt boşluk *dev* oluşturmak ve çocuk dev alanına erişmek için URL'leri listelemek için komutu kullanın.

```console
azds space select -n dev/azureuser1 -y
azds list-uris
```

Aşağıdaki çıktı, *azureuser1* alt `azds list-uris` geliştirme alanında örnek uygulamaya erişmek için örnek URL'leri gösterir.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/  Available
http://azureuser1.s.dev.gateway.traefik.MY_CUSTOM_DOMAIN/         Available
```

Komutun ortak URL'sini açarak *azureuser1* alt geliştirme alanında `azds list-uris` *bikesharingweb* hizmetine gidin. Yukarıdaki örnekte, *azureuser1* alt dev alanında *bikesharingweb* hizmeti için `http://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/`ortak URL' dir.

## <a name="configure-the-traefik-ingress-controller-to-use-https"></a>HTTPS'yi kullanacak şekilde traefik giriş denetleyicisini yapılandırın

Https kullanmak için traefik giriş denetleyicinizi yapılandırırken TLS sertifikasının yönetimini otomatikleştirmek için [sertifika yöneticisini][cert-manager] kullanın. Sertifika `helm` *grafiğini* yüklemek için kullanın.

```console
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.12/deploy/manifests/00-crds.yaml --namespace traefik
kubectl label namespace traefik certmanager.k8s.io/disable-validation=true
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager --namespace traefik --version v0.12.0 jetstack/cert-manager --set ingressShim.defaultIssuerName=letsencrypt --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Bir `letsencrypt-clusterissuer.yaml` dosya oluşturun ve e-posta adresinizle e-posta alanını güncelleştirin.

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
> Test etmek için *ClusterIssuer'ınız*için kullanabileceğiniz bir [evreleme sunucusu][letsencrypt-staging-issuer] da vardır.

Uygulamak `kubectl` `letsencrypt-clusterissuer.yaml`için kullanın.

```console
kubectl apply -f letsencrypt-clusterissuer.yaml --namespace traefik
```

Önceki *traefik* *ClusterRole* ve *ClusterRoleBinding*kaldırın, sonra kullanarak `helm`HTTPS kullanmak için traefik yükseltin.

> [!NOTE]
> AKS kümenizde RBAC etkin değilse, *--set rbac.enabled=true* parametresini kaldırın.

```console
kubectl delete ClusterRole traefik
kubectl delete ClusterRoleBinding traefik
helm upgrade traefik stable/traefik --namespace traefik --set kubernetes.ingressClass=traefik --set rbac.enabled=true --set kubernetes.ingressEndpoint.useDefaultPublishedService=true --version 1.85.0 --set ssl.enabled=true --set ssl.enforced=true --set ssl.permanentRedirect=true
```

[Kubectl get][kubectl-get]kullanarak traefik ingress denetleyici hizmetinin güncellenmiş IP adresini alın.

```console
kubectl get svc -n traefik --watch
```

Örnek çıktı, *traefik* ad alanındaki tüm hizmetlerin IP adreslerini gösterir.

```console
NAME      TYPE           CLUSTER-IP    EXTERNAL-IP          PORT(S)                      AGE
traefik   LoadBalancer   10.0.205.78   <pending>            80:32484/TCP,443:30620/TCP   20s
...
traefik   LoadBalancer   10.0.205.78   MY_NEW_EXTERNAL_IP   80:32484/TCP,443:30620/TCP   60s
```

Az network dns kullanarak traefik hizmetinin yeni harici IP adresi ile DNS bölgenize bir *kayıt* ekleyin [dns kaydet-bir eklenti kaydı ayarlayın][az-network-dns-record-set-a-add-record] ve az network [dns kaydını][az-network-dns-record-set-a-remove-record]kullanarak önceki *A* kaydını kaldırın.

```azurecli
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name *.traefik \
    --ipv4-address MY_NEW_EXTERNAL_IP

az network dns record-set a remove-record \
    --resource-group myResourceGroup \
    --zone-name  MY_CUSTOM_DOMAIN \
    --record-set-name *.traefik \
    --ipv4-address PREVIOUS_EXTERNAL_IP
```

Yukarıdaki örnek, MY_CUSTOM_DOMAIN *DNS* bölgesindeki *A* kaydını *PREVIOUS_EXTERNAL_IP*kullanmak üzere güncelleştirir.

*Sertifika yöneticisi* ve HTTPS'yi kullanmak için ayrıntıları içerecek şekilde [values.yaml'yi][values-yaml] güncelleştirin. Aşağıda güncelleştirilmiş `values.yaml` bir dosya örneği verilmiştir:

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

Örnek uygulamayı kullanarak `helm`yükseltin:

```console
helm upgrade bikesharingsampleapp . --namespace dev --atomic
```

*Dev/azureuser1* alt alanında örnek uygulamaya gidin ve HTTPS'yi kullanmak için yönlendirilenlerinizi fark edin.

> [!IMPORTANT]
> DNS değişikliklerinin tamamlanması ve örnek uygulamanızın erişilebilir olması 30 dakika veya daha uzun sürebilir.

Ayrıca, sayfanın yükyük olduğunu, ancak tarayıcının bazı hatalar gösterdiğini de unutmayın. Tarayıcı konsolununaçılması, hatanın HTTP kaynaklarını yüklemeye çalışan bir HTTPS sayfasıyla ilgili olduğunu gösterir. Örnek:

```console
Mixed Content: The page at 'https://azureuser1.s.dev.bikesharingweb.traefik.MY_CUSTOM_DOMAIN/devsignin' was loaded over HTTPS, but requested an insecure resource 'http://azureuser1.s.dev.gateway.traefik.MY_CUSTOM_DOMAIN/api/user/allUsers'. This request has been blocked; the content must be served over HTTPS.
```

Bu hatayı düzeltmek için [BikeSharingWeb/azds.yaml'yi][azds-yaml] *güncelleştirin* ve kubernetes.io/ingress.class için *traefik* ve *$(hostSuffix)* için özel etki alanınızı kullanmak için. Örnek:

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

[BikeSharingWeb/package.json'ı][package-json] *url* paketi için bağımlılıkla güncelleştirin.

```json
{
...
    "react-responsive": "^6.0.1",
    "universal-cookie": "^3.0.7",
    "url": "0.11.0"
  },
...
```

HTTPS'yi kullanmak için [BikeSharingWeb/lib/helpers.js'deki][helpers-js] *getApiHostAsync* yöntemini güncelleştirin:

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

Dizin `BikeSharingWeb` gidin ve `azds up` güncelleştirilmiş BikeSharingWeb hizmeti çalıştırmak için kullanın.

```console
cd ../BikeSharingWeb/
azds up
```

*Dev/azureuser1* alt alanında örnek uygulamaya gidin ve herhangi bir hata olmadan HTTPS'yi kullanmak için yönlendirilenifark edin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Geliştirme Alanlarında ekip geliştirme][team-development-qs]


[az-cli]: /cli/azure/install-azure-cli?view=azure-cli-latest
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-network-dns-record-set-a-add-record]: /cli/azure/network/dns/record-set/a?view=azure-cli-latest#az-network-dns-record-set-a-add-record
[az-network-dns-record-set-a-remove-record]: /cli/azure/network/dns/record-set/a?view=azure-cli-latest#az-network-dns-record-set-a-remove-record
[custom-domain]: ../../app-service/manage-custom-dns-buy-domain.md#buy-the-domain
[dns-zone]: ../../dns/dns-getstarted-cli.md
[qs-cli]: ../quickstart-cli.md
[team-development-qs]: ../quickstart-team-development.md

[azds-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/azds.yaml
[azure-account-create]: https://azure.microsoft.com/free
[cert-manager]: https://cert-manager.io/
[helm-installed]: https://helm.sh/docs/intro/install/
[helm-stable-repo]: https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository
[helpers-js]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/lib/helpers.js#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[letsencrypt-staging-issuer]: https://cert-manager.io/docs/configuration/acme/#creating-a-basic-acme-issuer
[package-json]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/package.json
[values-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml