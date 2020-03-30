---
title: Azure Kubernetes Hizmeti'nde (AKS) dahili ağ için giriş denetleyicisi oluşturma
description: Azure Kubernetes Hizmeti (AKS) kümesinde dahili, özel bir ağ için NGINX giriş denetleyicisini nasıl yükleyip yapılandırıştırmayı öğrenin.
services: container-service
ms.topic: article
ms.date: 05/24/2019
ms.openlocfilehash: 8c3eeaf2f9a92f1be9c691091d8e33d09a60b22d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595663"
---
# <a name="create-an-ingress-controller-to-an-internal-virtual-network-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) dahili sanal ağa giriş denetleyicisi oluşturma

Giriş denetleyicisi, Kubernetes hizmetleri için ters proxy, yapılandırılabilir trafik yönlendirmesi ve TLS sonlandırma sağlayan bir yazılım dır. Kubernetes giriş kaynakları, tek tek Kubernetes hizmetleri için giriş kurallarını ve rotalarını yapılandırmak için kullanılır. Giriş denetleyicisi ve giriş kuralları kullanılarak, trafiği Bir Kubernetes kümesindeki birden çok hizmete yönlendirmek için tek bir IP adresi kullanılabilir.

Bu makalede, Bir Azure Kubernetes Hizmeti (AKS) kümesinde [NGINX giriş denetleyicisini][nginx-ingress] nasıl dağıtabileceğinizgösterilmektedir. Giriş denetleyicisi dahili, özel sanal ağ ve IP adresi üzerinde yapılandırılır. Harici erişime izin verilmez. Aks kümesinde her biri tek IP adresi üzerinden erişilebilen iki uygulama çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Dinamik bir genel IP adresiyle][aks-ingress-tls] veya statik bir genel IP [adresiyle][aks-ingress-static-tls] TLS sertifikaları otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGINX giriş denetleyicisini, cert-manager'ı ve örnek bir web uygulamasını yüklemek için Helm kullanır. Helm'in AKS kümenizde başlatılması nı ve Tiller için bir hizmet hesabı kullanmasını ngerekir. Helm'i yapılandırma ve kullanma hakkında daha fazla bilgi için Azure [Kubernetes Hizmetinde (AKS) Helm ile Yükleme uygulamaları'na][use-helm]bakın.

Bu makalede, Azure CLI sürümünü 2.0.64 veya sonraki sürümde çalıştırdığınızda da vardır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Giriş denetleyicisi oluşturma

Varsayılan olarak, dinamik bir genel IP adresi ataması ile bir NGINX giriş denetleyicisi oluşturulur. Ortak bir yapılandırma gereksinimi, dahili, özel ağ ve IP adresi kullanmaktır. Bu yaklaşım, hizmetlerinize erişimi harici erişim olmadan dahili kullanıcılarla sınırlamanızı sağlar.

Aşağıdaki örnek manifest dosyasını kullanarak *internal-ingress.yaml* adlı bir dosya oluşturun. Bu örnek *loadBalancerIP* kaynağına *10.240.0.42* atar. Giriş denetleyicisi ile kullanmak için kendi dahili IP adresinizi sağlayın. Bu IP adresinin sanal ağınız içinde zaten kullanılmadığından emin olun.

```yaml
controller:
  service:
    loadBalancerIP: 10.240.0.42
    annotations:
      service.beta.kubernetes.io/azure-load-balancer-internal: "true"
```

Şimdi Helm ile *nginx-ingress* grafik dağıtmak. Önceki adımda oluşturulan bildirim dosyasını kullanmak `-f internal-ingress.yaml` için parametreyi ekleyin. Daha fazla yedeklilik sağlamak için `--set controller.replicaCount` parametresiyle iki NGINX giriş denetleyicisi çoğaltması dağıtılır. Giriş denetleyicisinin yinelemelerini çalıştıran kopyalardan tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

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
    -f internal-ingress.yaml \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

NginX giriş denetleyicisi için Kubernetes yük dengeleyici hizmeti oluşturulduğunda, aşağıdaki örnek çıktıda gösterildiği gibi dahili IP adresiniz atanır:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                              TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
alternating-coral-nginx-ingress-controller        LoadBalancer   10.0.97.109   10.240.0.42   80:31507/TCP,443:30707/TCP   1m
alternating-coral-nginx-ingress-default-backend   ClusterIP      10.0.134.66   <none>        80/TCP                       1m
```

Henüz giriş kuralları oluşturulmadı, bu nedenle dahili IP adresine göz atarsanız NGINX giriş denetleyicisinin varsayılan 404 sayfası görüntülenir. Giriş kuralları aşağıdaki adımlarda yapılandırılır.

## <a name="run-demo-applications"></a>Demo uygulamaları çalıştırma

Giriş denetleyicisini iş başında görmek için AKS kümenizde iki demo uygulaması çalıştıralım. Bu örnekte, Helm basit bir 'Merhaba dünya' uygulamasının iki örneğini dağıtmak için kullanılır.

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

Her iki uygulama da artık Kubernetes kümenizde çalışıyor. Her uygulamaya trafik yönlendirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, trafiği iki uygulamadan birine yönlendiren kuralları yapılandırır.

Aşağıdaki örnekte, adrese `http://10.240.0.42/` giden trafik adlı `aks-helloworld`hizmete yönlendirilir. Adrese `http://10.240.0.42/hello-world-two` giden trafik `ingress-demo` servise yönlendirilir.

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
    nginx.ingress.kubernetes.io/rewrite-target: /$1
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

`kubectl apply -f hello-world-ingress.yaml` Komutu kullanarak giriş kaynağını oluşturun.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Giriş denetleyicisini test edin

Giriş denetleyicisinin rotalarını test etmek için, bir web istemcisi olan iki uygulamaya göz atın. Gerekirse, AKS kümesindeki bir bölmeden bu dahili işlevselliği hızlı bir şekilde sınayabilirsiniz. Bir test bölmesi oluşturun ve bir terminal oturumu takın:

```console
kubectl run -it --rm aks-ingress-test --image=debian --namespace ingress-basic
```

Pod `curl` kullanarak yükleyin: `apt-get`

```console
apt-get update && apt-get install -y curl
```

Şimdi Kubernetes giriş denetleyicinizin `curl`adresine *http://10.240.0.42*erişin, örneğin . Bu makalenin ilk adımında giriş denetleyicisini dağıttığınızda belirtilen kendi dahili IP adresinizi sağlayın.

```console
curl -L http://10.240.0.42
```

Adresle ek bir yol sağlanmadı, bu nedenle */* giriş denetleyicisi rotaya varsayılan olarak gelir. İlk demo uygulaması, aşağıdaki yoğunlaştırılmış örnek çıktıda gösterildiği gibi döndürülür:

```
$ curl -L 10.240.0.42

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

Şimdi adrese */hello-world-two* yolunu ekleyin, örneğin *http://10.240.0.42/hello-world-two*. Özel başlığı olan ikinci demo uygulaması, aşağıdaki yoğunlaştırılmış örnek çıktıda gösterildiği gibi döndürülür:

```
$ curl -L -k http://10.240.0.42/hello-world-two

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

NAME                REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
kissing-ferret      1           Tue Oct 16 17:13:39 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
intended-lemur      1           Tue Oct 16 17:20:59 2018    DEPLOYED    aks-helloworld-0.1.0                default
pioneering-wombat   1           Tue Oct 16 17:21:05 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

`helm delete` Komutla sürümleri silin. Aşağıdaki örnek NGINX giriş dağıtım ını ve iki örnek AKS merhaba dünya uygulamalarını siler.

```
$ helm delete kissing-ferret intended-lemur pioneering-wombat

release "kissing-ferret" deleted
release "intended-lemur" deleted
release "pioneering-wombat" deleted
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

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dinamik bir genel IP'ye sahip bir giriş denetleyicisi oluşturun ve TLS sertifikalarını otomatik olarak oluşturmak için Şifreleyelim'i yapılandırın][aks-ingress-tls]
- [Statik genel IP adresine sahip bir giriş denetleyicisi oluşturun ve TLS sertifikalarını otomatik olarak oluşturmak için Şifreleyelim'i yapılandırın][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers