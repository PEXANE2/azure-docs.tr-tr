---
title: Giriş denetleyicisi oluşturma
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Hizmeti (AKS) kümesinde temel bir NGINX giriş denetleyicisini nasıl yükleyip yapılandırıştırmayı öğrenin.
services: container-service
ms.topic: article
ms.date: 12/20/2019
ms.openlocfilehash: 8b9e4537d6dd771697accc1f9fbdc3b6e2584863
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668530"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) giriş denetleyicisi oluşturma

Giriş denetleyicisi, Kubernetes hizmetleri için ters proxy, yapılandırılabilir trafik yönlendirmesi ve TLS sonlandırma sağlayan bir yazılım dır. Kubernetes giriş kaynakları, tek tek Kubernetes hizmetleri için giriş kurallarını ve rotalarını yapılandırmak için kullanılır. Giriş denetleyicisi ve giriş kuralları kullanılarak, trafiği Bir Kubernetes kümesindeki birden çok hizmete yönlendirmek için tek bir IP adresi kullanılabilir.

Bu makalede, Bir Azure Kubernetes Hizmeti (AKS) kümesinde [NGINX giriş denetleyicisini][nginx-ingress] nasıl dağıtabileceğinizgösterilmektedir. Aks kümesinde her biri tek IP adresi üzerinden erişilebilen iki uygulama çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Dinamik bir genel IP adresiyle][aks-ingress-tls] veya statik bir genel IP [adresiyle][aks-ingress-static-tls] TLS sertifikaları otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGINX giriş denetleyicisini ve örnek bir web uygulamasını yüklemek için Helm kullanır.

Bu makalede, Azure CLI sürümünü 2.0.64 veya sonraki sürümde çalıştırdığınızda da vardır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Giriş denetleyicisi oluşturma

Giriş denetleyicisini oluşturmak için, *nginx-ingress'i*yüklemek için Helm'i kullanın. Daha fazla yedeklilik sağlamak için `--set controller.replicaCount` parametresiyle iki NGINX giriş denetleyicisi çoğaltması dağıtılır. Giriş denetleyicisinin yinelemelerini çalıştıran kopyalardan tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

Ayrıca giriş denetleyicisinin bir Linux düğümü üzerinde zamanlanması gerekir. Windows Server düğümleri (şu anda AKS önizlemede) giriş denetleyicisini çalıştırmamalıdır. Kubernetes zamanlayıcısına NGINX giriş denetleyicisini Linux tabanlı bir düğümde çalıştırmasını söylemek için `--set nodeSelector` parametresi kullanılarak bir düğüm seçici belirtilir.

> [!TIP]
> Aşağıdaki örnek, *giriş temel*adlı giriş kaynakları için bir Kubernetes ad alanı oluşturur. Gerektiğinde kendi ortamınız için bir ad alanı belirtin.

> [!TIP]
> Kümenizdeki kapsayıcılara gelen istekler için istemci kaynağı IP `--set controller.service.externalTrafficPolicy=Local` [korumasını][client-source-ip] etkinleştirmek istiyorsanız, Helm install komutuna ekleyin. İstemci kaynağı IP, *X-Forwarded-For*altında istek üstbilgisinde depolanır. İstemci kaynağı IP koruma etkin leştirilmiş bir giriş denetleyicisi kullanırken, SSL geçişi çalışmaz.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Add the official stable repository
helm repo add stable https://kubernetes-charts.storage.googleapis.com/

# Use Helm to deploy an NGINX ingress controller
helm install nginx-ingress stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

NginX giriş denetleyicisi için Kubernetes yük dengeleyici hizmeti oluşturulduğunda, aşağıdaki örnek çıktıda gösterildiği gibi dinamik bir genel IP adresi atanır:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                             TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
nginx-ingress-controller         LoadBalancer   10.0.61.144    EXTERNAL_IP   80:30386/TCP,443:32276/TCP   6m2s
nginx-ingress-default-backend    ClusterIP      10.0.192.145   <none>        80/TCP                       6m2s
```

Henüz giriş kuralları oluşturulmadı, bu nedenle dahili IP adresine göz atarsanız NGINX giriş denetleyicisinin varsayılan 404 sayfası görüntülenir. Giriş kuralları aşağıdaki adımlarda yapılandırılır.

## <a name="run-demo-applications"></a>Demo uygulamaları çalıştırma

Giriş denetleyicisini iş başında görmek için AKS kümenizde iki demo uygulaması çalıştıralım. Bu örnekte, Helm basit bir *Merhaba dünya* uygulamasının iki örneğini dağıtmak için kullanılır.

Örnek Miğfer grafiklerini yüklemeden önce, Azure örnek lerini Miğfer ortamınıza aşağıdaki gibi ekleyin:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Bir Miğfer grafiğinden aşağıdaki komutla ilk demo uygulamasını oluşturun:

```console
helm install aks-helloworld azure-samples/aks-helloworld --namespace ingress-basic
```

Şimdi demo uygulamasının ikinci bir örneğini yükleyin. İkinci örnek için, iki uygulamanın görsel olarak farklı olması için yeni bir başlık belirtirsiniz. Ayrıca benzersiz bir hizmet adı belirtirsiniz:

```console
helm install aks-helloworld-two azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="aks-helloworld-two"
```

## <a name="create-an-ingress-route"></a>Giriş rotası oluşturma

Her iki uygulama da artık Kubernetes kümenizde çalışıyor. Her uygulamaya trafik yönlendirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, trafiği iki uygulamadan birine yönlendiren kuralları yapılandırır.

Aşağıdaki örnekte, *EXTERNAL_IP* giden trafik adlı `aks-helloworld`hizmete yönlendirilir. *EXTERNAL_IP/hello-world-two'ya* giden trafik `aks-helloworld-two` servise yönlendirilir. *EXTERNAL_IP/statik* trafik statik kıymetler için `aks-helloworld` adlandırılmış hizmete yönlendirilir.

Adlandırılmış `hello-world-ingress.yaml` bir dosya oluşturun ve aşağıdaki örnekYAML'de kopyalayın.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  rules:
  - http:
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
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /static/$2
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /static(/|$)(.*)
```

`kubectl apply -f hello-world-ingress.yaml` Komutu kullanarak giriş kaynağını oluşturun.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
ingress.extensions/hello-world-ingress-static created
```

## <a name="test-the-ingress-controller"></a>Giriş denetleyicisini test edin

Giriş denetleyicisinin rotalarını test etmek için iki uygulamaya göz atın. NGINX giriş denetleyicinizin IP adresine bir web tarayıcısı *EXTERNAL_IP.* İlk demo uygulaması aşağıdaki örnekte gösterildiği gibi web tarayıcısında görüntülenir:

![Giriş denetleyicisinin arkasında çalışan ilk uygulama](media/ingress-basic/app-one.png)

Şimdi IP adresine */hello-world-two* yolunu ekleyin, *örneğin EXTERNAL_IP/hello-world-two.* Özel başlığı ile ikinci demo uygulaması görüntülenir:

![Giriş denetleyicisinin arkasında çalışan ikinci uygulama](media/ingress-basic/app-two.png)

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
$ helm list --namespace ingress-basic

NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
aks-helloworld          ingress-basic   1               2020-01-06 19:57:00.131576 -0600 CST    deployed        aks-helloworld-0.1.0               
aks-helloworld-two      ingress-basic   1               2020-01-06 19:57:10.971365 -0600 CST    deployed        aks-helloworld-0.1.0               
nginx-ingress           ingress-basic   1               2020-01-06 19:55:46.358275 -0600 CST    deployed        nginx-ingress-1.27.1    0.26.1  
```

`helm delete` Komutla sürümleri silin. Aşağıdaki örnek NGINX giriş dağıtım ını ve iki örnek AKS merhaba dünya uygulamalarını siler.

```
$ helm delete aks-helloworld aks-helloworld-two nginx-ingress --namespace ingress-basic

release "aks-helloworld" uninstalled
release "aks-helloworld-two" uninstalled
release "nginx-ingress" uninstalled
```

Ardından, AKS merhaba dünya uygulaması için Helm repo kaldırın:

```console
helm repo remove azure-samples
```

Trafiği örnek uygulamalara yönlendiren giriş rotasını kaldırın:

```console
kubectl delete -f hello-world-ingress.yaml
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

- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Dinamik bir genel IP adresiyle][aks-ingress-tls] veya statik bir genel IP [adresiyle][aks-ingress-static-tls] TLS sertifikaları otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers
