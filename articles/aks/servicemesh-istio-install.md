---
title: Azure Kubernetes Service (AKS) ' de Istio 'yu kurma
description: Azure Kubernetes Service (AKS) kümesinde hizmet ağı oluşturma ve kullanma hakkında bilgi edinin
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: 245ac3b1fd88b8d2430e9ddefef3562efd16e6d1
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2019
ms.locfileid: "73885379"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ' de Istio 'yu yükleyip kullanma

[Istio][istio-github] , bir Kubernetes kümesindeki mikro hizmetlerde anahtar bir işlevsellik kümesi sağlayan açık kaynaklı bir hizmet kafesidir. Bu özellikler trafik yönetimi, hizmet kimliği ve güvenlik, ilke zorlama ve Observability içerir. Istio hakkında daha fazla bilgi için resmi [nedir?][istio-docs-concepts] belgesine bakın.

Bu makalede, Istio 'un nasıl yükleneceği gösterilmektedir. Istio `istioctl` istemci ikilisi istemci makinenize yüklenir ve ıKS bileşenleri AKS 'deki bir Kubernetes kümesine yüklenir.

> [!NOTE]
> Bu yönergeler, Istio sürüm `1.3.2`referans ve en az Helm sürüm `2.14.2`kullanır.
>
> Istio `1.3.x` yayınları, Kubernetes sürümleriyle `1.13`, `1.14`, `1.15`karşı Istio ekibi tarafından test edilmiştir. [GitHub-Istio yayımları][istio-github-releases]üzerinde ek istio sürümleri, [istio haberleri][istio-release-notes] ve desteklenen Kubernetes sürümlerinin her biri hakkında bilgi edinmek IÇIN bkz. [Genel SSS][istio-faq].

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Istio istioctl istemci ikilisini indirme ve yükleme
> * AKS 'e Istio 'yu yükler
> * Istio yüklemesini doğrulama
> * Eklentilere erişin
> * AKS 'ten Istio 'dan kaldırma

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede açıklanan adımlarda, RBAC etkinleştirilmiş bir AKS kümesi (Kubernetes `1.13` ve üzeri) oluşturdunuz ve kümeyle `kubectl` bir bağlantı oluşturmuş olduğunuz varsayılır. Bu öğelerin herhangi biriyle ilgili yardıma ihtiyacınız varsa, [aks hızlı başlangıç][aks-quickstart]bölümüne bakın.

Bu yönergeleri izlemek ve Istio 'yu yüklemek için [Helm][helm] gerekir. En son kararlı sürümü kümenize doğru yüklenip yapılandırılmış olmalıdır. Held 'yi yüklemeyle ilgili yardıma ihtiyacınız varsa bkz. [aks helk Yükleme Kılavuzu][helm-install]. Tüm istio Pod 'ler de Linux düğümlerinde çalışacak şekilde zamanlanmalıdır.

AKS kümenizde Istio çalıştırmaya yönelik ek kaynak gereksinimlerini anlamak için, [Istio performans ve ölçeklenebilirlik](https://istio.io/docs/concepts/performance-and-scalability/) belgelerini okuduğunuzdan emin olun. Çekirdek ve bellek gereksinimleri, belirli iş yükünüze göre farklılık gösterecektir. Kuruluma uygun sayıda düğüm ve VM boyutu seçin.

Bu makale, Istio yükleme kılavuzunu çeşitli ayrı adımlara ayırır. Nihai sonuç, resmi Istio yükleme [kılavuzundaki][istio-install-helm]yapıda aynıdır.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/istio/install-client-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [MacOS - download and install client binary](includes/servicemesh/istio/install-client-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/istio/install-client-binary-windows.md)]

::: zone-end

## <a name="add-the-istio-helm-chart-repository"></a>Istio Helm grafik deposunu ekleme

Istio yayını için Istio Helm grafik deposunu ekleyin. Grafik deposu için yerel bilgilerinizi güncelleştirmek üzere `helm repo update` çalıştırtığınızdan emin olun.

```azurecli
helm repo add istio.io https://storage.googleapis.com/istio-release/releases/$ISTIO_VERSION/charts/
helm repo update
```

## <a name="install-the-istio-crds-on-aks"></a>AKS 'e Istio CRDs 'yi yükler

İstio, çalışma zamanı yapılandırmasını yönetmek için [özel kaynak tanımları (CRDs)][kubernetes-crd] kullanır. IBU bileşenleri üzerinde bir bağımlılığı olduğundan, önce Istio CRDs 'yi yüklememiz gerekir. AKS kümenizdeki `istio-system` ad alanına Istio CRDs 'yi yüklemek için Helm ve `istio-init` grafiğini kullanın:

```azurecli
helm install istio.io/istio-init --name istio-init --namespace istio-system
```

[İşler][kubernetes-jobs] , crds 'yi yüklemek Için `istio-init` hele grafiğinin bir parçası olarak dağıtılır. Bu işlerin tamamlanması, küme ortamınıza bağlı olarak 20 ' den az sürer. İşlerin şu şekilde başarıyla tamamlandığını doğrulayabilirsiniz:

```azurecli
kubectl get jobs -n istio-system
```

Aşağıdaki örnek çıktıda başarıyla tamamlanan işler gösterilmektedir.

```console
NAME                      COMPLETIONS   DURATION   AGE
istio-init-crd-10-1.3.2   1/1           14s        14s
istio-init-crd-11-1.3.2   1/1           12s        14s
istio-init-crd-12-1.3.2   1/1           14s        14s
```

İşlerin başarılı bir şekilde tamamlandığını Onayladığımıza göre, doğru sayıda Istio CRDs yüklü olduğunu doğrulayalım. Aşağıdaki komutu çalıştırarak tüm 23 Istio CRDs 'nin yüklendiğini doğrulayabilirsiniz. Komut `23`sayı döndürmelidir.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - check CRD count](includes/servicemesh/istio/install-check-crd-count-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - check CRD count](includes/servicemesh/istio/install-check-crd-count-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - check CRD count](includes/servicemesh/istio/install-check-crd-count-powershell.md)]

::: zone-end

Bu noktaya sahipseniz, bu durumda Istio CRDs 'yi başarıyla yüklemiş olursunuz. Şimdi de bir sonraki bölüme geçin. bu [bileşenleri AKS 'e yükleyebilirsiniz](#install-the-istio-components-on-aks).

## <a name="install-the-istio-components-on-aks"></a>AKS 'e Istio bileşenlerini yükler

[Grafana][grafana] ve [kiali][kiali] 'nin bir parçası olarak ilerliyoruz. Grafana, analiz ve izleme panoları sağlar ve Kiali bir hizmet ağı Observability panosu sağlar. Kurulumumuzda, bu bileşenlerin her biri [gizli][kubernetes-secrets]olarak sağlanması gereken kimlik bilgilerini gerektirir.

Istio bileşenlerini yükleyebilmemiz için, hem Grafana hem de Kiali için gizli dizileri oluşturuyoruz. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - create secrets for Grafana and Kiali](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash check for CRDs](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell check for CRDs](includes/servicemesh/istio/install-create-secrets-powershell.md)]

::: zone-end

### <a name="install-istio-components"></a>Istio bileşenlerini yükler

AKS kümenizdeki Grafana ve Kiali gizli dizilerini başarıyla oluşturduğumuzdan, bu durumda Istio bileşenlerinin yüklenmesi zaman alabilir. AKS kümenizdeki `istio-system` ad alanına Istio bileşenlerini yüklemek için Helm ve `istio` grafiğini kullanın. 

> [!NOTE]
> **Yükleme seçenekleri**
> 
> Yüklememizin kapsamında aşağıdaki seçenekleri kullanıyoruz:
> - `global.controlPlaneSecurityEnabled=true`-denetim düzlemi için etkin ve karşılıklı TLS
> - `global.mtls.enabled=true`-tüm hizmetin hizmet ile iletişim kurmak için MTLS olması gerekir
> - `grafana.enabled=true`-analiz ve izleme panoları için Grafana dağıtımını etkinleştirme
> - `grafana.security.enabled=true`-Grafana için kimlik doğrulamasını etkinleştirme
> - `tracing.enabled=true`-izleme için Jaeger dağıtımını etkinleştirme
> - `kiali.enabled=true`-bir hizmet ağı Observability panosu için Kiali dağıtımını etkinleştirme
>
> **Düğüm seçicileri**
>
> IBU Şu anda Linux düğümlerinde çalıştırılmak üzere zamanlanmalıdır. Kümenizde Windows Server düğümleriniz varsa, Istio 'ların yalnızca Linux düğümlerinde çalışacak şekilde zamanlandığından emin olmanız gerekir. Düğümlerin doğru düğümlere zamanlandığından emin olmak için [düğüm seçicileri][kubernetes-node-selectors] kullanacağız.

> [!CAUTION]
> [SDS (gizli bulma hizmeti)][istio-feature-sds] ve [istio CNI][istio-feature-cni] istio özellikleri şu anda [Alpha][istio-feature-stages]içinde olduğundan, bunlar etkinleştirilmeden önce düşünülmelidir. Ayrıca, [Service Account Token Volume Projection][kubernetes-feature-sa-projected-volume] Kubernetes ÖZELLIĞI (SDS için bir gereksinim) geçerli aks sürümlerinde etkin değildir.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/istio/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - install Istio components](includes/servicemesh/istio/install-components-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - install Istio components](includes/servicemesh/istio/install-components-powershell.md)]

::: zone-end

`istio` Held grafiği çok sayıda nesne dağıtır. Listeyi yukarıdaki `helm install` komutunun çıktısından görebilirsiniz. Çalışan bileşenlerinin dağıtımı, küme ortamınıza bağlı olarak 2 dakika sürer.

Bu noktada, AKS kümenize Istio 'u dağıttık. Başarılı bir Istio dağıtımına sahip olduğunuzdan emin olmak için, bir sonraki bölüme ilerleyip bu [yüklemeyi doğrulayalım](#validate-the-istio-installation).

## <a name="validate-the-istio-installation"></a>Istio yüklemesini doğrulama

Önce beklenen hizmetlerin oluşturulduğunu onaylayın. Çalışan hizmetleri görüntülemek için [kubectl Get svc][kubectl-get] komutunu kullanın. `istio-system` ad alanını sorgulayın, burada, Istio ve eklenti bileşenlerinin `istio` Helm grafiği tarafından yüklendiği yer:

```console
kubectl get svc --namespace istio-system --output wide
```

Aşağıdaki örnek çıktıda Şu anda çalışıyor olması gereken hizmetler gösterilmektedir:

- `istio-*` Hizmetleri
- `jaeger-*`, `tracing`ve `zipkin` eklenti izleme hizmetleri
- `prometheus` eklentisi ölçüm hizmeti
- `grafana` eklentisi Analizi ve izleme panosu hizmeti
- `kiali` eklentisi hizmet ağı Pano hizmeti

`istio-ingressgateway` `<pending>`dış IP 'si gösteriyorsa, Azure ağ tarafından bir IP adresi atanmadan birkaç dakika bekleyin.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                                      AGE   SELECTOR
grafana                  ClusterIP      10.0.164.244   <none>           3000/TCP                                                                                                                                     53s   app=grafana
istio-citadel            ClusterIP      10.0.49.16     <none>           8060/TCP,15014/TCP                                                                                                                           53s   istio=citadel
istio-galley             ClusterIP      10.0.175.173   <none>           443/TCP,15014/TCP,9901/TCP                                                                                                                   53s   istio=galley
istio-ingressgateway     LoadBalancer   10.0.226.151   20.188.221.111   15020:31128/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:30817/TCP,15030:30436/TCP,15031:32485/TCP,15032:30980/TCP,15443:30124/TCP   53s   app=istio-ingressgateway,istio=ingressgateway,release=istio
istio-pilot              ClusterIP      10.0.102.158   <none>           15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       53s   istio=pilot
istio-policy             ClusterIP      10.0.234.53    <none>           9091/TCP,15004/TCP,15014/TCP                                                                                                                 53s   istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.216.8     <none>           443/TCP,15014/TCP                                                                                                                            53s   istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.154.215   <none>           9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       53s   istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                                   52s   app=jaeger
jaeger-collector         ClusterIP      10.0.26.109    <none>           14267/TCP,14268/TCP                                                                                                                          52s   app=jaeger
jaeger-query             ClusterIP      10.0.70.55     <none>           16686/TCP                                                                                                                                    52s   app=jaeger
kiali                    ClusterIP      10.0.36.206    <none>           20001/TCP                                                                                                                                    53s   app=kiali
prometheus               ClusterIP      10.0.236.99    <none>           9090/TCP                                                                                                                                     53s   app=prometheus
tracing                  ClusterIP      10.0.83.152    <none>           80/TCP                                                                                                                                       52s   app=jaeger
zipkin                   ClusterIP      10.0.25.86     <none>           9411/TCP                                                                                                                                     52s   app=jaeger
```

Ardından, gerekli yığınların oluşturulduğunu onaylayın. [Kubectl Get Pod][kubectl-get] komutunu kullanın ve `istio-system` ad alanını sorgulayın:

```console
kubectl get pods --namespace istio-system
```

Aşağıdaki örnek çıktı, çalıştıran Pod 'leri göstermektedir:

- `istio-*` Pod
- `prometheus-*` eklenti ölçümleri Pod
- `grafana-*` eklentisi Analizi ve izleme panosu Pod
- `kiali` eklentisi hizmet ağı panosu Pod

```console
NAME                                     READY   STATUS      RESTARTS   AGE
grafana-7c48555456-msl7b                 1/1     Running     0          88s
istio-citadel-566fc66db7-m8wgl           1/1     Running     0          87s
istio-galley-5746db8d56-pl5gg            1/1     Running     0          88s
istio-ingressgateway-6c94f7c9bf-f5lt5    1/1     Running     0          88s
istio-init-crd-10-1.3.2-xw9g2            0/1     Completed   0          92m
istio-init-crd-11-1.3.2-54rz8            0/1     Completed   0          92m
istio-init-crd-12-1.3.2-789qj            0/1     Completed   0          92m
istio-pilot-6748968b6d-rvdfx             2/2     Running     0          87s
istio-policy-7576bbbcf7-2stft            2/2     Running     0          87s
istio-sidecar-injector-76d79d494-7jk9n   1/1     Running     0          87s
istio-telemetry-74b7bf676d-tfrcl         2/2     Running     0          88s
istio-tracing-655d9588bc-d2htg           1/1     Running     0          86s
kiali-65d55bcfb8-tqrfk                   1/1     Running     0          88s
prometheus-846f9849bd-br8kp              1/1     Running     0          87s
```

`Completed` durumu olan üç `istio-init-crd-*` Pod olmalıdır. Bu FID 'ler, daha önceki bir adımda CRDs 'yi oluşturan işleri çalıştırmaktan sorumludur. Diğer tüm yığınların `Running`durumunu göstermesi gerekir. Ayırımlarınızın bu durumları yoksa, tamamlanana kadar bir dakika veya iki tane bekleyin. Herhangi bir pod bir sorun bildirirse, çıktısını ve durumlarını gözden geçirmek için [kubectl 'yi bir pod betimleyen][kubectl-describe] komutunu kullanın.

## <a name="accessing-the-add-ons"></a>Eklentilere erişme

Yukarıdaki kurulumla, daha fazla işlevsellik sağlayan bir dizi eklenti yüklenmiş. Eklentiler için Web uygulamaları, bir dış IP adresi aracılığıyla **herkese açık bir şekilde gösterilmez** . 

Eklenti kullanıcı arabirimlerine erişmek için `istioctl dashboard` komutunu kullanın. Bu komut, [kubectl bağlantı noktasından ileri][kubectl-port-forward] ve rastgele bir bağlantı noktasından yararlanarak istemci makineniz ile aks kümenizdeki ilgili Pod arasında güvenli bir bağlantı oluşturur. Daha sonra, eklenti Web uygulamasını varsayılan tarayıcınızda otomatik olarak açar.

Bu makalenin önceki kısımlarında yer aldığı için kimlik bilgilerini belirterek Grafana ve Kiali için ek bir güvenlik katmanı ekledik.

### <a name="grafana"></a>Grafana

Istio için analiz ve izleme panoları [Grafana][grafana]tarafından sağlanır. İstendiğinde, Grafana gizli anahtarı aracılığıyla oluşturduğunuz kimlik bilgilerini kullanmayı unutmayın. Grafana panosunu güvenli şekilde açmak için aşağıdaki adımları izleyin:

```console
istioctl dashboard grafana
```

### <a name="prometheus"></a>Prometheus

[Ceso ölçümleri Prometheus][prometheus]tarafından sağlanmaktadır. Prometheus panosunu şu şekilde güvenli şekilde açın:

```console
istioctl dashboard prometheus
```

### <a name="jaeger"></a>Jaeger

Istio 'da izleme, [Jaeger][jaeger]tarafından sağlanır. Şu şekilde Jaeger panosunu güvenli şekilde açın:

```console
istioctl dashboard jaeger
```

### <a name="kiali"></a>Kiali

Bir hizmet ağı Observability panosu [Kiali][kiali]tarafından sağlanır. İstendiğinde Kiali gizli dizisi aracılığıyla oluşturduğunuz kimlik bilgilerini kullanmayı unutmayın. Kiali panosunu güvenli şekilde aşağıdaki gibi açın:

```console
istioctl dashboard kiali
```

### <a name="envoy"></a>Envoy

[Envoy][envoy] proxy 'lerine yönelik basit bir arabirim mevcuttur. Belirtilen Pod 'da çalışan bir Envoy proxy 'si için yapılandırma bilgileri ve ölçümler sağlar. Aşağıdaki gibi, Envoy arabirimini güvenli şekilde açın:

```console
istioctl dashboard envoy <pod-name>.<namespace>
```

## <a name="uninstall-istio-from-aks"></a>AKS 'ten Istio 'dan kaldırma

> [!WARNING]
> Çalışan bir sistemden Istio 'un silinmesi, hizmetleriniz arasında trafik ile ilgili sorunlar oluşmasına neden olabilir. Devam etmeden önce sisteminizin ilerlemeden düzgün şekilde çalışmaya yönelik hükümler oluşturduğunuzdan emin olun.

### <a name="remove-istio-components-and-namespace"></a>Istio bileşenlerini ve ad alanını kaldır

AKS kümenizdeki ICU 'yi kaldırmak için aşağıdaki komutları kullanın. `helm delete` komutları `istio` ve `istio-init` grafiklerini kaldırır ve `kubectl delete namespace` komutu `istio-system` ad alanını kaldırır.

```azurecli
helm delete --purge istio
helm delete --purge istio-init
kubectl delete namespace istio-system
```

### <a name="remove-istio-crds-and-secrets"></a>Istio CRDs ve gizli dizileri kaldır

Yukarıdaki komutlar tüm Istio bileşenlerini ve ad alanını siler, ancak yine de ilerme CRU ve gizli dizileri ile hala çalışıyoruz. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - remove Istio CRDs and secrets](includes/servicemesh/istio/uninstall-powershell.md)]

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgelerde, bir kanarya yayını kullanıma sunmak üzere akıllı yönlendirme sağlamak için istio 'yu nasıl kullanabileceğiniz açıklanmaktadır:

> [!div class="nextstepaction"]
> [AKS Istio akıllı yönlendirme senaryosu][istio-scenario-routing]

Istio 'ın daha fazla yükleme ve yapılandırma seçeneklerini araştırmak için aşağıdaki resmi Istio makalelerine bakın:

- [İstio-Helm Yükleme Kılavuzu][istio-install-helm]
- [İstio-Helm yükleme seçenekleri][istio-install-helm-options]

Ayrıca şunları kullanarak ek senaryolar izleyebilirsiniz:

- [İstio BookInfo uygulama örneği][istio-bookinfo-example]

Application Insights ve Istio kullanarak AKS uygulamanızı nasıl izleyeceğinizi öğrenmek için aşağıdaki Azure Izleyici belgelerine bakın:

- [Kubernetes barındırılan uygulamalar için sıfır izleme uygulaması izleme][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-faq]: https://istio.io/faq/general/
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/news/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-helm]: https://istio.io/docs/setup/install/helm/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/

[istio-feature-stages]: https://istio.io/about/feature-stages/
[istio-feature-sds]: https://istio.io/docs/tasks/security/auth-sds/
[istio-feature-cni]: https://istio.io/docs/setup/additional-setup/cni/

[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-feature-sa-projected-volume]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection
[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-jobs]: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-node-selectors]: https://docs.microsoft.com/azure/aks/concepts-clusters-workloads#node-selectors
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[envoy]: https://www.envoyproxy.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./servicemesh-istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
