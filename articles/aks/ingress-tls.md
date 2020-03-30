---
title: Azure Kubernetes Hizmeti (AKS) kümesi ile HTTPS girişi oluşturma
description: Azure Kubernetes Hizmeti (AKS) kümesinde otomatik TLS sertifika oluşturma için Let's Encrypt kullanan bir NGINX giriş denetleyicisini nasıl yükleyip yapılandırılabildiğini öğrenin.
services: container-service
ms.topic: article
ms.date: 01/29/2020
ms.openlocfilehash: 6f497ee3edd5ee831c091a5a50629df81673acea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78191325"
---
# <a name="create-an-https-ingress-controller-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) https giriş denetleyicisi oluşturma

Giriş denetleyicisi, Kubernetes hizmetleri için ters proxy, yapılandırılabilir trafik yönlendirmesi ve TLS sonlandırma sağlayan bir yazılım dır. Kubernetes giriş kaynakları, tek tek Kubernetes hizmetleri için giriş kurallarını ve rotalarını yapılandırmak için kullanılır. Giriş denetleyicisi ve giriş kuralları kullanılarak, trafiği Bir Kubernetes kümesindeki birden çok hizmete yönlendirmek için tek bir IP adresi kullanılabilir.

Bu makalede, Bir Azure Kubernetes Hizmeti (AKS) kümesinde [NGINX giriş denetleyicisini][nginx-ingress] nasıl dağıtabileceğinizgösterilmektedir. [Sertifika yöneticisi][cert-manager] projesi, [Let's Encrypt][lets-encrypt] sertifikalarını otomatik olarak oluşturmak ve yapılandırmak için kullanılır. Son olarak, aks kümesinde her biri tek bir IP adresi üzerinden erişilebilen iki uygulama çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Statik genel IP adresine sahip TLS sertifikalarını otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Bu makalede, AKS kümenizle aynı kaynak grubunda [DNS Bölgesi][dns-zone] olan [özel bir etki alanınız][custom-domain] olduğunu da varsayar.

Bu makalede, NGINX giriş denetleyicisini, cert-manager'ı ve örnek bir web uygulamasını yüklemek için Helm kullanır. Helm'in en son sürümünden kullandığınızdan emin olun. Yükseltme yönergeleri için [Helm yükleme dokümanlarına][helm-install]bakın. Helm'i yapılandırma ve kullanma hakkında daha fazla bilgi için Azure [Kubernetes Hizmetinde (AKS) Helm ile Yükleme uygulamaları'na][use-helm]bakın.

Bu makalede, Azure CLI sürümünü 2.0.64 veya sonraki sürümde çalıştırdığınızda da vardır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Giriş denetleyicisi oluşturma

Giriş denetleyicisini oluşturmak için `helm` *nginx girişini*yüklemek için komutu kullanın. Daha fazla yedeklilik sağlamak için `--set controller.replicaCount` parametresiyle iki NGINX giriş denetleyicisi çoğaltması dağıtılır. Giriş denetleyicisinin yinelemelerini çalıştıran kopyalardan tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

Ayrıca giriş denetleyicisinin bir Linux düğümü üzerinde zamanlanması gerekir. Windows Server düğümleri (şu anda AKS önizlemede) giriş denetleyicisini çalıştırmamalıdır. Kubernetes zamanlayıcısına NGINX giriş denetleyicisini Linux tabanlı bir düğümde çalıştırmasını söylemek için `--set nodeSelector` parametresi kullanılarak bir düğüm seçici belirtilir.

> [!TIP]
> Aşağıdaki örnek, *giriş temel*adlı giriş kaynakları için bir Kubernetes ad alanı oluşturur. Gerektiğinde kendi ortamınız için bir ad alanı belirtin.

> [!TIP]
> Kümenizdeki kapsayıcılara gelen istekler için istemci kaynağı IP `--set controller.service.externalTrafficPolicy=Local` [korumasını][client-source-ip] etkinleştirmek istiyorsanız, Helm install komutuna ekleyin. İstemci kaynağı IP, *X-Forwarded-For*altında istek üstbilgisinde depolanır. İstemci kaynağı IP koruma etkin leştirilmiş bir giriş denetleyicisi kullanırken, SSL geçişi çalışmaz.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Add the official stable repo
helm repo add stable https://kubernetes-charts.storage.googleapis.com/

# Use Helm to deploy an NGINX ingress controller
helm install nginx stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Yükleme sırasında, giriş denetleyicisi için bir Azure genel IP adresi oluşturulur. Bu genel IP adresi giriş denetleyicisinin ömrü için statiktir. Giriş denetleyicisini silerseniz, ortak IP adresi ataması kaybolur. Daha sonra ek bir giriş denetleyicisi oluşturursanız, yeni bir genel IP adresi atanır. Genel IP adresinin kullanımını korumak istiyorsanız, bunun yerine [statik genel IP adresine sahip bir giriş denetleyicisi oluşturabilirsiniz.][aks-ingress-static-tls]

Genel IP adresini almak için `kubectl get service` komutu kullanın. IP adresinin hizmete atanması birkaç dakika sürer.

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
nginx-ingress-controller                         LoadBalancer   10.0.182.160   MY_EXTERNAL_IP  80:30920/TCP,443:30426/TCP   20m
nginx-ingress-default-backend                    ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Henüz giriş kuralları oluşturulmadı. Herkese açık IP adresine göz atarsanız, NGINX giriş denetleyicisinin varsayılan 404 sayfası görüntülenir.

## <a name="add-an-a-record-to-your-dns-zone"></a>DNS bölgenize A kaydı ekleme

Az *A* [Network dns kaydını kullanarak][az-network-dns-record-set-a-add-record]NGINX hizmetinin harici IP adresiyle DNS bölgenize a kaydı ekleyin.

```console
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name * \
    --ipv4-address MY_EXTERNAL_IP
```

> [!NOTE]
> İsteğe bağlı olarak, özel bir etki alanı yerine giriş denetleyicisi IP adresi için bir FQDN yapılandırabilirsiniz. Bu örneğin bir Bash kabuğu için olduğunu unutmayın.
> 
> ```azurecli-interactive
> # Public IP address of your ingress controller
> IP="MY_EXTERNAL_IP"
> 
> # Name to associate with public IP address
> DNSNAME="demo-aks-ingress"
> 
> # Get the resource-id of the public ip
> PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)
> 
> # Update public ip address with DNS name
> az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
> 
> # Display the FQDN
> az network public-ip show --ids $PUBLICIPID --query "[dnsSettings.fqdn]" --output tsv
> ```

## <a name="install-cert-manager"></a>cert-manager denetleyicisini yükleme

NGINX giriş denetleyicisi, TLS sonlandırmayı destekler. HTTPS için birçok farklı sertifika alma ve yapılandırma yöntemi vardır. Bu makalede, otomatik [Lets Encrypt][lets-encrypt] sertifika oluşturma ve yönetim işlevselliği sağlayan [cert-manager,][cert-manager]kullanarak gösterir.

Sertifika yöneticisi denetleyicisini yüklemek için:

```console
# Install the CustomResourceDefinition resources separately
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.13/deploy/manifests/00-crds.yaml

# Label the ingress-basic namespace to disable resource validation
kubectl label namespace ingress-basic cert-manager.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  cert-manager \
  --namespace ingress-basic \
  --version v0.13.0 \
  jetstack/cert-manager
```

Sertifika yöneticisi yapılandırması hakkında daha fazla bilgi [için, sertifika yöneticisi projesine][cert-manager]bakın.

## <a name="create-a-ca-cluster-issuer"></a>CA küme veren oluşturma

Sertifikalar verilmeden önce, sertifika yöneticisi nin bir [İhraççı][cert-manager-issuer] veya [ClusterIssuer][cert-manager-cluster-issuer] kaynağı na ihtiyacı vardır. Bu Kubernetes kaynakları işlevsellik `Issuer` olarak aynıdır, ancak tek `ClusterIssuer` bir ad alanında çalışır ve tüm ad alanlarında çalışır. Daha fazla bilgi için [sertifika yöneticisi veren][cert-manager-issuer] belgelerine bakın.

Aşağıdaki örnek bildirimi kullanarak `cluster-issuer.yaml`bir küme veren oluşturun. E-posta adresini kuruluşunuzdan geçerli bir adresle güncelleştirin:

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

İhraççıyı oluşturmak `kubectl apply` için komutu kullanın.

```console
kubectl apply -f cluster-issuer.yaml --namespace ingress-basic
```

## <a name="run-demo-applications"></a>Demo uygulamaları çalıştırma

Giriş denetleyicisi ve sertifika yönetimi çözümü yapılandırıldı. Şimdi AKS kümenizde iki demo uygulaması çalıştıralım. Bu örnekte, Helm basit bir *Merhaba dünya* uygulamasının iki örneğini dağıtmak için kullanılır.

Örnek Miğfer grafiklerini yüklemeden önce, Miğfer ortamınıza Azure örnekleri deposunu ekleyin.

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

*Azure-samples/aks-helloworld* Helm grafiğini kullanarak *aks-helloworld* adlı bir demo uygulaması oluşturun.

```console
helm install aks-helloworld azure-samples/aks-helloworld --namespace ingress-basic
```

*Aks-helloworld-two*adlı demo uygulamasının ikinci bir örneğini oluşturun. İki uygulamanın görsel olarak farklı olması için yeni bir başlık ve benzersiz hizmet adı *belirtin .*

```console
helm install aks-helloworld-two azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="aks-helloworld-two"
```

## <a name="create-an-ingress-route"></a>Giriş rotası oluşturma

Her iki uygulama da artık Kubernetes kümenizde çalışıyor. Ancak, tür `ClusterIP` bir hizmet ile yapılandırılır ve internetten erişilebilir değildir. Bunları herkese açık hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, trafiği iki uygulamadan birine yönlendiren kuralları yapılandırır.

Aşağıdaki örnekte, adrese *trafik merhaba-dünya-ingress. MY_CUSTOM_DOMAIN* *aks-helloworld* servisine yönlendirilir. Adrese trafik *merhaba-dünya girişi. MY_CUSTOM_DOMAIN/hello-world-two* *aks-helloworld-two* servisine yönlendirilir. Trafik *merhaba-dünya-ingress için. MY_CUSTOM_DOMAIN/statik* statik varlıklar için *aks-helloworld* adlı hizmete yönlendirilir.

Aşağıdaki örnek `hello-world-ingress.yaml` YAML kullanarak adlı bir dosya oluşturun. Ana *bilgisayarları* ve *ana bilgisayarları* önceki adımda oluşturduğunuz DNS adına güncelleştirin.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /$2
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
    - hello-world-ingress.MY_CUSTOM_DOMAIN
    secretName: tls-secret
  rules:
  - host: hello-world-ingress.MY_CUSTOM_DOMAIN
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: aks-helloworld-two
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress-static
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /static/$2
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
    - hello-world-ingress.MY_CUSTOM_DOMAIN
    secretName: tls-secret
  rules:
  - host: hello-world-ingress.MY_CUSTOM_DOMAIN
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /static(/|$)(.*)
```

`kubectl apply` Komutu kullanarak giriş kaynağını oluşturun.

```console
kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic
```

## <a name="verify-a-certificate-object-has-been-created"></a>Sertifika nesnesi oluşturulduğunu doğrulama

Ardından, bir sertifika kaynağı oluşturulmalıdır. Sertifika kaynağı istenen X.509 sertifikasını tanımlar. Daha fazla bilgi için [sertifika yöneticisi sertifikalarına][cert-manager-certificates]bakın. Cert-manager, v0.2.2'den beri cert-manager ile otomatik olarak dağıtılan giriş-şim kullanarak sizin için otomatik olarak bir sertifika nesnesi oluşturmuştur. Daha fazla bilgi için [giriş-şim belgelerine][ingress-shim]bakın.

Sertifikanın başarıyla oluşturulduğunu doğrulamak için `kubectl get certificate --namespace ingress-basic` komutu kullanın ve *READY* is *True*doğruluğunu doğrulayın , bu birkaç dakika sürebilir.

```
$ kubectl get certificate --namespace ingress-basic

NAME         READY   SECRET       AGE
tls-secret   True    tls-secret   11m
```

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmasını test edin

Hello-world girişi için bir web *tarayıcısı açın. MY_CUSTOM_DOMAIN* Kubernetes giriş kontrol cihazınızın. HTTPS'yi kullanmak için yönlendirilediğinize ve sertifikaya güvenilen ve demo uygulamasının web tarayıcısında gösterildiğine dikkat edin. */hello-world-two* yolunu ekleyin ve özel başlığı gösterilen ikinci demo uygulamasını fark edin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, giriş bileşenleri, sertifikalar ve örnek uygulamalar yüklemek için Helm kullanılmıştır. Bir Miğfer grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar bölmeleri, dağıtımları ve hizmetleri içerir. Bu kaynakları temizlemek için, örnek ad alanının tamamını veya tek tek kaynakları silebilirsiniz.

### <a name="delete-the-sample-namespace-and-all-resources"></a>Örnek ad alanını ve tüm kaynakları silme

Örnek ad alanının tamamını silmek için komutu `kubectl delete` kullanın ve ad alanı adınızı belirtin. Ad alanındaki tüm kaynaklar silinir.

```console
kubectl delete namespace ingress-basic
```

Daha sonra, AKS merhaba dünya uygulaması için Helm repo kaldırın:

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Kaynakları tek tek silme

Alternatif olarak, daha ayrıntılı bir yaklaşım oluşturulan tek tek kaynakları silmektir. İlk olarak, küme veren kaynakları kaldırın:

```console
kubectl delete -f cluster-issuer.yaml --namespace ingress-basic
```

Helm bültenlerini komutla listele. `helm list` Aşağıdaki örnek çıktıda gösterildiği gibi *nginx-ingress* ve *aks-helloworld*adlı grafiklere bakın:

```
$ helm list --namespace ingress-basic

NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
aks-helloworld          ingress-basic   1               2020-01-15 10:24:32.054871 -0600 CST    deployed        aks-helloworld-0.1.0               
aks-helloworld-two      ingress-basic   1               2020-01-15 10:24:37.671667 -0600 CST    deployed        aks-helloworld-0.1.0               
cert-manager            ingress-basic   1               2020-01-15 10:23:36.515514 -0600 CST    deployed        cert-manager-v0.13.0    v0.13.0    
nginx                   ingress-basic   1               2020-01-15 10:09:45.982693 -0600 CST    deployed        nginx-ingress-1.29.1    0.27.0  
```

`helm delete` Komutla sürümleri silin. Aşağıdaki örnek NGINX giriş dağıtım ını ve iki örnek AKS merhaba dünya uygulamalarını siler.

```
$ helm uninstall aks-helloworld aks-helloworld-two cert-manager nginx --namespace ingress-basic

release "aks-helloworld" uninstalled
release "aks-helloworld-two" uninstalled
release "cert-manager" uninstalled
release "nginx" uninstalled
```

Ardından, AKS merhaba dünya uygulaması için Helm repo kaldırın:

```console
helm repo remove azure-samples
```

Trafiği örnek uygulamalara yönlendiren giriş rotasını kaldırın:

```console
kubectl delete -f hello-world-ingress.yaml --namespace ingress-basic
```

Son olarak, kendi ad alanını silebilirsiniz. Komutu `kubectl delete` kullanın ve ad alanı adınızı belirtin:

```console
kubectl delete namespace ingress-basic
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede AKS bazı dış bileşenleri dahil. Bu bileşenler hakkında daha fazla bilgi edinmek için aşağıdaki proje sayfalarına bakın:

- [Dümen CLI][helm-cli]
- [NGINX giriş denetleyicisi][nginx-ingress]
- [sertifika yöneticisi][cert-manager]

Aşağıdakileri de yapabilirsiniz:

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Statik genel IP adresine sahip TLS sertifikalarını otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun][aks-ingress-static-tls]

<!-- LINKS - external -->
[az-network-dns-record-set-a-add-record]: /cli/azure/network/dns/record-set/a?view=azure-cli-latest#az-network-dns-record-set-a-add-record
[custom-domain]: ../app-service/manage-custom-dns-buy-domain.md#buy-the-domain
[dns-zone]: ../dns/dns-getstarted-cli.md
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[ingress-shim]: https://docs.cert-manager.io/en/latest/tasks/issuing-certificates/ingress-shim.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[client-source-ip]: concepts-network.md#ingress-controllers
[install-azure-cli]: /cli/azure/install-azure-cli
