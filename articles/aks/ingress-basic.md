---
title: Azure Kubernetes hizmeti 'nde (AKS) temel bir giriş denetleyicisi oluşturma
description: Azure Kubernetes Service (AKS) kümesine temel bir NGıNX giriş denetleyicisi yüklemeyi ve yapılandırmayı öğrenin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: mlearned
ms.openlocfilehash: 7b1fb26adc49067c35745011414ada7b33d7e55e
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74913573"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmeti 'nde (AKS) giriş denetleyicisi oluşturma

Giriş denetleyicisi, Kubernetes Hizmetleri için ters proxy, yapılandırılabilir trafik yönlendirme ve TLS sonlandırma sağlayan bir yazılım parçasıdır. Kubernetes giriş kaynakları, bağımsız bir Kubernetes hizmeti için giriş kurallarını ve rotaları yapılandırmak üzere kullanılır. Bir giriş denetleyicisi ve giriş kuralları kullanarak, bir Kubernetes kümesinde trafiği birden çok hizmete yönlendirmek için tek bir IP adresi kullanılabilir.

Bu makalede bir Azure Kubernetes Service (AKS) kümesinde [NGINX giriş denetleyicisinin][nginx-ingress] nasıl dağıtılacağı gösterilir. Her biri tek IP adresi üzerinden erişilebilen AKS kümesinde iki uygulama çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [HTTP uygulama yönlendirme eklentisini etkinleştirin][aks-http-app-routing]
- [İç, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturun][aks-ingress-internal]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Dinamik bir genel IP adresi][aks-ingress-tls] veya [STATIK bir genel IP adresi][aks-ingress-static-tls] ile otomatik olarak TLS sertifikaları oluşturmak için şifrelemeyi kullanan bir giriş denetleyicisi oluşturun

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGıNX giriş denetleyicisini, CERT-Manager 'ı ve örnek bir Web uygulamasını yüklemek için Held kullanılmaktadır. AKS kümenizde helk başlatılmış ve Tiller için bir hizmet hesabı kullanmanız gerekir. Held 'yi yapılandırma ve kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes hizmeti 'nde (AKS) Held ile uygulama yüklemesi][use-helm].

Bu makalede, Azure CLı sürüm 2.0.64 veya üstünü de çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Giriş denetleyicisi oluşturma

Giriş denetleyicisini oluşturmak için, *NGINX-inge*'yı yüklemek üzere `Helm` kullanın. Ek artıklık için NGıNX giriş denetleyicilerinin iki yinelemesi `--set controller.replicaCount` parametresiyle dağıtılır. Giriş denetleyicisinin Çoğaltmalarından tamamen yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

Giriş denetleyicisinin da bir Linux düğümünde zamanlanması gerekir. Windows Server düğümleri (Şu anda AKS 'deki önizlemede) giriş denetleyicisini çalıştırmamalıdır. Kubernetes Scheduler ' ın Linux tabanlı bir düğümde NGıNX giriş denetleyicisini çalıştırmasını bildirmek için `--set nodeSelector` parametresi kullanılarak bir düğüm seçici belirtilir.

> [!TIP]
> Aşağıdaki örnek, *Giriş-Basic*adlı giriş kaynakları için bir Kubernetes ad alanı oluşturur. Gerektiğinde kendi ortamınız için bir ad alanı belirtin. AKS kümeniz RBAC etkinleştirilmemişse, helk komutlarına `--set rbac.create=false` ekleyin.

> [!TIP]
> Kümenizdeki kapsayıcılara yönelik [istemci kaynak IP korumasını][client-source-ip] etkinleştirmek Istiyorsanız, Helm install komutuna `--set controller.service.externalTrafficPolicy=Local` ekleyin. İstemci kaynak IP 'si, *Için X-iletilen-için*istek üstbilgisinde depolanır. İstemci kaynak IP koruması etkinken bir giriş denetleyicisi kullanılırken, SSL geçişi çalışmaz.

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

Kubernetes yük dengeleyici hizmeti NGıNX giriş denetleyicisi için oluşturulduğunda, aşağıdaki örnek çıktıda gösterildiği gibi dinamik bir genel IP adresi atanır:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                                 TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
aspiring-labradoodle-nginx-ingress-controller        LoadBalancer   10.0.61.144    40.117.74.8   80:30386/TCP,443:32276/TCP   6m2s
aspiring-labradoodle-nginx-ingress-default-backend   ClusterIP      10.0.192.145   <none>        80/TCP                       6m2s
```

Henüz giriş kuralı oluşturulmadı, bu nedenle iç IP adresine gözattığınızda NGıNX giriş denetleyicisinin varsayılan 404 sayfası görüntülenir. Giriş kuralları aşağıdaki adımlarda yapılandırılır.

## <a name="run-demo-applications"></a>Demo uygulamalarını çalıştırma

Giriş denetleyicisini çalışır durumda görmek için AKS kümenizde iki tanıtım uygulaması çalıştıralım. Bu örnekte, HELI basit bir ' Hello World ' uygulamasının iki örneğini dağıtmak için kullanılır.

Örnek HELI grafiklerini yükleyebilmeniz için önce aşağıdaki gibi, Azure Samples Repository 'yi Helu ortamınıza ekleyin:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Aşağıdaki komutla bir Held grafiğinden ilk demo uygulamayı oluşturun:

```console
helm install azure-samples/aks-helloworld --namespace ingress-basic
```

Şimdi tanıtım uygulamasının ikinci bir örneğini yükleyin. İkinci örnek için, iki uygulamanın görsel olarak benzersiz olması için yeni bir başlık belirtirsiniz. Ayrıca, benzersiz bir hizmet adı da belirtirsiniz:

```console
helm install azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Giriş yolu oluşturma

Her iki uygulama da artık Kubernetes kümenizde çalışıyor. Trafiği her bir uygulamaya yönlendirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, trafiği iki uygulamadan birine yönlendiren kuralları yapılandırır.

Aşağıdaki örnekte, Address `http://40.117.74.8/` trafiği `aks-helloworld`adlı hizmete yönlendirilir. Adres `http://40.117.74.8/hello-world-two` trafik `ingress-demo` hizmetine yönlendirilir.

`hello-world-ingress.yaml` adlı bir dosya oluşturun ve aşağıdaki örnekte bulunan YAML 'yi kopyalayın.

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
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

`kubectl apply -f hello-world-ingress.yaml` komutunu kullanarak giriş kaynağını oluşturun.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Giriş denetleyicisini test etme

Giriş denetleyicisinin yollarını test etmek için iki uygulamaya gidin. *http://40.117.74.8* gibi NGINX GIRIŞ denetleyicinizin IP adresine bir Web tarayıcısı açın. İlk demo uygulaması, aşağıdaki örnekte gösterildiği gibi Web tarayıcısında görüntülenir:

![Giriş denetleyicisinin arkasında çalışan ilk uygulama](media/ingress-basic/app-one.png)

Şimdi, *http://40.117.74.8/hello-world-two* gibi IP adresine */Hello-World-Two* yolunu ekleyin. Özel başlığa sahip ikinci demo uygulaması görüntülenir:

![Giriş denetleyicisinin arkasında çalışan ikinci uygulama](media/ingress-basic/app-two.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede giriş bileşenlerini ve örnek uygulamaları yüklemek için Held kullanılmaktadır. Bir helk grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar, pods, dağıtımlar ve hizmetler içerir. Bu kaynakları temizlemek için, tüm örnek ad alanını veya ayrı kaynakları silebilirsiniz.

### <a name="delete-the-sample-namespace-and-all-resources"></a>Örnek ad alanını ve tüm kaynakları Sil

Tüm örnek ad alanını silmek için `kubectl delete` komutunu kullanın ve ad alanı adınızı belirtin. Ad alanındaki tüm kaynaklar silinir.

```console
kubectl delete namespace ingress-basic
```

Ardından, AKS Hello World uygulamasının Held deposunu kaldırın:

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Kaynakları tek tek Sil

Alternatif olarak, oluşturulan kaynakları tek tek silmek daha ayrıntılı bir yaklaşımdır. `helm list` komutuyla Held sürümlerini listeleyin. Aşağıdaki örnek çıktıda gösterildiği gibi *NGINX-ingress* ve *aks-HelloWorld*adlı grafikleri arayın:

```
$ helm list

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
aspiring-labradoodle    1           Wed Mar 27 19:55:37 2019    DEPLOYED    nginx-ingress-1.3.1     0.22.0      ingress-basic
esteemed-koala          1           Wed Mar 27 19:59:18 2019    DEPLOYED    aks-helloworld-0.1.0                ingress-basic
wonderful-puma          1           Wed Mar 27 19:59:07 2019    DEPLOYED    aks-helloworld-0.1.0                ingress-basic
```

`helm delete` komutuyla yayınları silin. Aşağıdaki örnekte NGıNX giriş dağıtımı ve iki örnek AKS Hello World Apps de silinir.

```
$ helm delete aspiring-labradoodle esteemed-koala wonderful-puma

release "aspiring-labradoodle" deleted
release "esteemed-koala" deleted
release "wonderful-puma" deleted
```

Ardından, AKS Hello World uygulamasının Held deposunu kaldırın:

```console
helm repo remove azure-samples
```

Trafiği örnek uygulamalara yönlendiren giriş yolunu kaldırın:

```console
kubectl delete -f hello-world-ingress.yaml
```

Son olarak, kendi ad alanını silebilirsiniz. `kubectl delete` komutunu kullanın ve ad alanı adınızı belirtin:

```console
kubectl delete namespace ingress-basic
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bazı dış bileşenler AKS 'ye eklenmiştir. Bu bileşenler hakkında daha fazla bilgi edinmek için aşağıdaki proje sayfalarına bakın:

- [Held CLı][helm-cli]
- [NGıNX giriş denetleyicisi][nginx-ingress]

Aşağıdakileri de yapabilirsiniz:

- [HTTP uygulama yönlendirme eklentisini etkinleştirin][aks-http-app-routing]
- [İç, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturun][aks-ingress-internal]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Dinamik bir genel IP adresi][aks-ingress-tls] veya [STATIK bir genel IP adresi][aks-ingress-static-tls] ile otomatik olarak TLS sertifikaları oluşturmak için şifrelemeyi kullanan bir giriş denetleyicisi oluşturun

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
