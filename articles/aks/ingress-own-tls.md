---
title: TLS sertifikalarınızı giriş için kullanın
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmeti (AKS) kümesinde kendi sertifikalarınızı kullanan bir NGINX giriş denetleyicisini nasıl yükleyip yapılandırıştırmayı öğrenin.
services: container-service
ms.topic: article
ms.date: 05/24/2019
ms.openlocfilehash: ec6a398b52424c142b3d7ee82625c10c733456ab
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668469"
---
# <a name="create-an-https-ingress-controller-and-use-your-own-tls-certificates-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Service'te (AKS) HTTPS giriş denetleyicisi oluşturma ve kendi TLS sertifikalarınızı kullanma

Giriş denetleyicisi, Kubernetes hizmetleri için ters proxy, yapılandırılabilir trafik yönlendirmesi ve TLS sonlandırma sağlayan bir yazılım dır. Kubernetes giriş kaynakları, tek tek Kubernetes hizmetleri için giriş kurallarını ve rotalarını yapılandırmak için kullanılır. Giriş denetleyicisi ve giriş kuralları kullanılarak, trafiği Bir Kubernetes kümesindeki birden çok hizmete yönlendirmek için tek bir IP adresi kullanılabilir.

Bu makalede, Bir Azure Kubernetes Hizmeti (AKS) kümesinde [NGINX giriş denetleyicisini][nginx-ingress] nasıl dağıtabileceğinizgösterilmektedir. Kendi sertifikalarınızı oluşturursunuz ve giriş rotasında kullanılmak üzere bir Kubernetes sırrı oluşturursunuz. Son olarak, aks kümesinde her biri tek bir IP adresi üzerinden erişilebilen iki uygulama çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
- [Dinamik bir genel IP adresiyle][aks-ingress-tls] veya statik bir genel IP [adresiyle][aks-ingress-static-tls] TLS sertifikaları otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGINX giriş denetleyicisini ve örnek bir web uygulamasını yüklemek için Helm kullanır. Helm'in AKS kümenizde başlatılması nı ve Tiller için bir hizmet hesabı kullanmasını ngerekir. Helm'in en son sürümünden kullandığınızdan emin olun. Yükseltme yönergeleri için [Helm yükleme dokümanlarına][helm-install]bakın. Helm'i yapılandırma ve kullanma hakkında daha fazla bilgi için Azure [Kubernetes Hizmetinde (AKS) Helm ile Yükleme uygulamaları'na][use-helm]bakın.

Bu makalede, Azure CLI sürümünü 2.0.64 veya sonraki sürümde çalıştırdığınızda da vardır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Giriş denetleyicisi oluşturma

Giriş denetleyicisini oluşturmak `Helm` için *nginx-ingress'i*yüklemek için kullanın. Daha fazla yedeklilik sağlamak için `--set controller.replicaCount` parametresiyle iki NGINX giriş denetleyicisi çoğaltması dağıtılır. Giriş denetleyicisinin yinelemelerini çalıştıran kopyalardan tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

Ayrıca giriş denetleyicisinin bir Linux düğümü üzerinde zamanlanması gerekir. Windows Server düğümleri (şu anda AKS önizlemede) giriş denetleyicisini çalıştırmamalıdır. Kubernetes zamanlayıcısına NGINX giriş denetleyicisini Linux tabanlı bir düğümde çalıştırmasını söylemek için `--set nodeSelector` parametresi kullanılarak bir düğüm seçici belirtilir.

> [!TIP]
> Aşağıdaki örnek, *giriş temel*adlı giriş kaynakları için bir Kubernetes ad alanı oluşturur. Gerektiğinde kendi ortamınız için bir ad alanı belirtin. AKS kümeniz RBAC etkin değilse, Helm komutlarına ekleyin. `--set rbac.create=false`

> [!TIP]
> Kümenizdeki kapsayıcılara gelen istekler için istemci kaynağı IP `--set controller.service.externalTrafficPolicy=Local` [korumasını][client-source-ip] etkinleştirmek istiyorsanız, Helm install komutuna ekleyin. İstemci kaynağı IP, *X-Forwarded-For*altında istek üstbilgisinde depolanır. İstemci kaynağı IP koruma etkin leştirilmiş bir giriş denetleyicisi kullanırken, SSL geçişi çalışmaz.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Yükleme sırasında, giriş denetleyicisi için bir Azure genel IP adresi oluşturulur. Bu genel IP adresi giriş denetleyicisinin ömrü için statiktir. Giriş denetleyicisini silerseniz, ortak IP adresi ataması kaybolur. Daha sonra ek bir giriş denetleyicisi oluşturursanız, yeni bir genel IP adresi atanır. Genel IP adresinin kullanımını korumak istiyorsanız, bunun yerine [statik genel IP adresine sahip bir giriş denetleyicisi oluşturabilirsiniz.][aks-ingress-static-tls]

Genel IP adresini almak için `kubectl get service` komutu kullanın. IP adresinin hizmete atanması birkaç dakika sürer.

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                          TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
virulent-seal-nginx-ingress-controller        LoadBalancer   10.0.48.240   40.87.46.190   80:31159/TCP,443:30657/TCP   7m
virulent-seal-nginx-ingress-default-backend   ClusterIP      10.0.50.5     <none>         80/TCP                       7m
```

Dağıtımı test etmek için son adımda kullanıldığından, bu genel IP adresini not alın.

Henüz giriş kuralları oluşturulmadı. Herkese açık IP adresine göz atarsanız, NGINX giriş denetleyicisinin varsayılan 404 sayfası görüntülenir.

## <a name="generate-tls-certificates"></a>TLS sertifikaları oluşturma

Bu makale için, `openssl`'' ile kendi imzalı bir sertifika oluşturalım. Üretim kullanımı için, bir sağlayıcı veya kendi sertifika yetkilisi (CA) aracılığıyla güvenilir, imzalı bir sertifika talep etmelisiniz. Bir sonraki adımda, TLS sertifikasını ve OpenSSL tarafından oluşturulan özel anahtarı kullanarak bir Kubernetes *Secret* oluşturursunuz.

Aşağıdaki örnek, *aks-ingress-tls.crt*adlı 365 gün için geçerli 2048 bit RSA X509 sertifikası oluşturur. Özel anahtar dosyası *aks-ingress-tls.key*olarak adlandırılır. Bir Kubernetes TLS sırrı bu dosyaların her ikisini de gerektirir.

Bu makale, *demo.azure.com* konusu ortak adı ile çalışır ve değiştirilmesi gerekmez. Üretim kullanımı için parametre için `-subj` kendi kuruluş değerlerinizi belirtin:

```console
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -out aks-ingress-tls.crt \
    -keyout aks-ingress-tls.key \
    -subj "/CN=demo.azure.com/O=aks-ingress-tls"
```

## <a name="create-kubernetes-secret-for-the-tls-certificate"></a>TLS sertifikası için Kubernetes gizli dizisi oluşturma

Kubernetes'in giriş denetleyicisi için TLS sertifikasını ve özel anahtarı kullanmasına izin vermek için bir Gizli oluşturur ve kullanırsınız. Gizli bir kez tanımlanır ve önceki adımda oluşturulan sertifika ve anahtar dosyasını kullanır. Giriş yollarını tanımlarken bu gizliye başvurursunuz.

Aşağıdaki örnekte gizli bir ad *oluşturur aks-ingress-tls:*

```console
kubectl create secret tls aks-ingress-tls \
    --namespace ingress-basic \
    --key aks-ingress-tls.key \
    --cert aks-ingress-tls.crt
```

## <a name="run-demo-applications"></a>Demo uygulamaları çalıştırma

Sertifikanızın içeren bir giriş denetleyicisi ve Gizli yapılandırıldı. Şimdi AKS kümenizde iki demo uygulaması çalıştıralım. Bu örnekte, Helm basit bir 'Merhaba dünya' uygulamasının iki örneğini dağıtmak için kullanılır.

Örnek Miğfer grafiklerini yüklemeden önce, Azure örnek lerini Miğfer ortamınıza aşağıdaki gibi ekleyin:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Bir Miğfer grafiğinden aşağıdaki komutla ilk demo uygulamasını oluşturun:

```console
helm install azure-samples/aks-helloworld --namespace ingress-basic
```

Şimdi demo uygulamasının ikinci bir örneğini yükleyin. İkinci örnek için, iki uygulamanın görsel olarak farklı olması için yeni bir başlık belirtirsiniz. Ayrıca benzersiz bir hizmet adı belirtirsiniz:

```console
helm install azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Giriş rotası oluşturma

Her iki uygulama şimdi Kubernetes kümeüzerinde çalışıyor, ancak tür `ClusterIP`bir hizmet ile yapılandırılır. Bu nedenle, uygulamalara internetten erişilemez. Bunları herkese açık hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, trafiği iki uygulamadan birine yönlendiren kuralları yapılandırır.

Aşağıdaki örnekte, adrese `https://demo.azure.com/` giden trafik adlı `aks-helloworld`hizmete yönlendirilir. Adrese `https://demo.azure.com/hello-world-two` giden trafik `ingress-demo` servise yönlendirilir. Bu makale için, bu demo ana bilgisayar adlarını değiştirmeniz gerekmez. Üretim kullanımı için, sertifika isteği ve oluşturma sürecinin bir parçası olarak belirtilen adları sağlayın.

> [!TIP]
> Sertifika istek işlemi sırasında belirtilen ana bilgisayar adı, CN adı, giriş rotanızda tanımlanan ana bilgisayarla eşleşmiyorsa, giriş denetleyicisi bir *Kubernetes Ingress Controller Sahte Sertifika* uyarısı görüntüler. Sertifikanızın ve giriş rotanızın ana bilgisayar adlarının eşleştirdiğinden emin olun.

*Tls* *bölümü,* giriş demo.azure.com için *aks-ingress-tls* adlı Secret'ı kullanmak için giriş yolunu söyler. Yine, üretim kullanımı için kendi ana bilgisayar adresinizi belirtin.

Adlandırılmış `hello-world-ingress.yaml` bir dosya oluşturun ve aşağıdaki örnekYAML'de kopyalayın.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - demo.azure.com
    secretName: aks-ingress-tls
  rules:
  - host: demo.azure.com
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

`kubectl apply -f hello-world-ingress.yaml` Komutu kullanarak giriş kaynağını oluşturun.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmasını test edin

Sertifikaları sahte *demo.azure.com* ana bilgisayarımızla `curl` test etmek için *--çözüm* parametresini kullanın ve belirtin. Bu parametre, giriş denetleyicinizin genel IP adresiyle *demo.azure.com* adını eşlemenizi sağlar. Aşağıdaki örnekte gösterildiği gibi, kendi giriş denetleyicinizin ortak IP adresini belirtin:

```
curl -v -k --resolve demo.azure.com:443:40.87.46.190 https://demo.azure.com
```

Adresle ek bir yol sağlanmadı, bu nedenle */* giriş denetleyicisi rotaya varsayılan olarak gelir. İlk demo uygulaması, aşağıdaki yoğunlaştırılmış örnek çıktıda gösterildiği gibi döndürülür:

```
$ curl -v -k --resolve demo.azure.com:443:40.87.46.190 https://demo.azure.com

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

Komutumuzdaki `curl` *-v* parametresi alınan TLS sertifikası da dahil olmak üzere ayrıntılı bilgi çıkarmaktadır. Kıvırma çıktınızın yarısında, kendi TLS sertifikanızın kullanıldığını doğrulayabilirsiniz. *-k* parametresi, kendi imzalı bir sertifika kullanıyor olmamıza rağmen sayfayı yüklemeye devam ediyor. Aşağıdaki örnek, *verenin: CN=demo.azure.com; O=aks-ingress-tls* sertifikası kullanılmıştır:

```
[...]
* Server certificate:
*  subject: CN=demo.azure.com; O=aks-ingress-tls
*  start date: Oct 22 22:13:54 2018 GMT
*  expire date: Oct 22 22:13:54 2019 GMT
*  issuer: CN=demo.azure.com; O=aks-ingress-tls
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
[...]
```

Şimdi adrese */hello-world-two* yolunu ekleyin, örneğin `https://demo.azure.com/hello-world-two`. Özel başlığı olan ikinci demo uygulaması, aşağıdaki yoğunlaştırılmış örnek çıktıda gösterildiği gibi döndürülür:

```
$ curl -v -k --resolve demo.azure.com:443:137.117.36.18 https://demo.azure.com/hello-world-two

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>AKS Ingress Demo</title>
[...]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, giriş bileşenleri ve örnek uygulamalar yüklemek için Helm kullanılır. Bir Miğfer grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar bölmeleri, dağıtımları ve hizmetleri içerir. Bu kaynakları temizlemek için, örnek ad alanının tamamını veya tek tek kaynakları silebilirsiniz.

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

Alternatif olarak, daha ayrıntılı bir yaklaşım oluşturulan tek tek kaynakları silmektir. Helm bültenlerini komutla listele. `helm list` Aşağıdaki örnek çıktıda gösterildiği gibi *nginx-ingress* ve *aks-helloworld*adlı grafiklere bakın:

```
$ helm list

NAME            REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
virulent-seal   1           Tue Oct 23 16:37:24 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
billowing-guppy 1           Tue Oct 23 16:41:38 2018    DEPLOYED    aks-helloworld-0.1.0                default
listless-quokka 1           Tue Oct 23 16:41:30 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

`helm delete` Komutla sürümleri silin. Aşağıdaki örnek, NGINX giriş dağıtımını ve iki örnek AKS merhaba dünya uygulamalarını siler.

```
$ helm delete virulent-seal billowing-guppy listless-quokka

release "virulent-seal" deleted
release "billowing-guppy" deleted
release "listless-quokka" deleted
```

Ardından, AKS merhaba dünya uygulaması için Helm repo kaldırın:

```console
helm repo remove azure-samples
```

Trafiği örnek uygulamalara yönlendiren giriş rotasını kaldırın:

```console
kubectl delete -f hello-world-ingress.yaml
```

Gizli sertifikayı silin:

```console
kubectl delete secret aks-ingress-tls
```

Son olarak, kendi ad alanını silebilirsiniz. Komutu `kubectl delete` kullanın ve ad alanı adınızı belirtin:

```console
kubectl delete namespace ingress-basic
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede AKS bazı dış bileşenleri dahil. Bu bileşenler hakkında daha fazla bilgi edinmek için aşağıdaki proje sayfalarına bakın:

- [Dümen CLI][helm-cli]
- [NGINX giriş denetleyicisi][nginx-ingress]

Aşağıdakileri de yapabilirsiniz:

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
- [Dinamik bir genel IP adresiyle][aks-ingress-tls] veya statik bir genel IP [adresiyle][aks-ingress-static-tls] TLS sertifikaları otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
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
[aks-ingress-tls]: ingress-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers
