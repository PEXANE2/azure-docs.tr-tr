---
title: Azure Kubernetes Hizmetinde (AKS) Istio'ya Yükleme
description: Azure Kubernetes Hizmeti (AKS) kümesinde bir hizmet ağı oluşturmak için Istio'yi nasıl yükleyip kullanacağınızı öğrenin
author: paulbouwer
ms.topic: article
ms.date: 02/19/2020
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: f0fe4ab46bfe5c0c0c2ea67aa2e2694321628be5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136372"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Istio'yı Azure Kubernetes Hizmetinde (AKS) yükleyin ve kullanın

[Istio,][istio-github] Bir Kubernetes kümesindeki mikro hizmetler de önemli bir işlevsellik kümesi sağlayan açık kaynak kodlu bir hizmet kafesidir. Bu özellikler arasında trafik yönetimi, hizmet kimliği ve güvenliği, ilke zorlama ve gözlemlenebilirlik yer almaktadır. Istio hakkında daha fazla bilgi için resmi [Istio nedir?][istio-docs-concepts]

Bu makalede, Istio nasıl yüklenir gösterir. Istio `istioctl` istemci ikilisi istemci makinenize yüklenir ve Istio bileşenleri AKS'deki bir Kubernetes kümesine yüklenir.

> [!NOTE]
> Aşağıdaki talimatlar referans Istio sürümü `1.4.0`.
>
> Istio `1.4.x` bültenleri Kubernetes sürümleri `1.13`karşı Istio ekibi `1.14`tarafından `1.15`test edilmiştir , . Sen GitHub ek Istio sürümleri bulabilirsiniz [- Istio Bültenleri,][istio-github-releases] [Istio Haberler][istio-release-notes] ve [Istio Genel SSS][istio-faq]de desteklenen Kubernetes sürümleri her bültenleri hakkında bilgi .

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Istio istioctl istemci ikilisini indirin ve kurun
> * Istio'yı AKS'ye yükleyin
> * Istio yüklemesini doğrulama
> * Eklentilere erişin
> * IStio'u AKS'den kaldırın

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede ayrıntılı adımlar, bir AKS kümesi (Kubernetes `1.13` ve yukarıda, RBAC etkin) oluşturduğunuzve küme ile bir `kubectl` bağlantı kurduk varsayalım. Bu öğelerden herhangi biriyle ilgili yardıma ihtiyacınız varsa, [AKS quickstart'ına][aks-quickstart]bakın.

AKS kümenizde Istio'yu çalıştırmak için ek kaynak gereksinimlerini anlamak için [Istio Performans ve Ölçeklenebilirlik](https://istio.io/docs/concepts/performance-and-scalability/) belgelerini okuduğunuzdan emin olun. Çekirdek ve bellek gereksinimleri, özel iş yükünüze bağlı olarak değişir. Kurulumunuza uygun sayıda düğüm ve VM boyutu seçin.

Bu makalede, Istio yükleme kılavuzunu birkaç ayrı adıma ayırır. Sonuçta resmi Istio kurulum [kılavuzu][istio-install-istioctl]olarak yapı olarak aynıdır.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/istio/install-client-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [MacOS - download and install client binary](includes/servicemesh/istio/install-client-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/istio/install-client-binary-windows.md)]

::: zone-end

## <a name="install-the-istio-components-on-aks"></a>Istio bileşenlerini AKS'ye yükleyin

Istio kurulumuzun bir parçası olarak [Grafana][grafana] ve [Kiali'yi][kiali] kuracağız. Grafana analiz ve izleme panoları sağlar ve Kiali bir hizmet örgü gözlem panosu sağlar. Kurulumumuzda, bu bileşenlerin her biri [Gizli][kubernetes-secrets]olarak sağlanmalıdır kimlik bilgileri gerektirir.

Istio bileşenlerini yüklemeden önce hem Grafana hem de Kiali için sırları yaratmalıyız. Bu sırların Istio `istio-system` tarafından kullanılacak ad alanına yüklenmesi gerekir, bu yüzden de ad alanını oluşturmamız gerekir. Istio yükleyicisi gelecekte bu `kubectl create` nesne üzerinde çalıştırabilirsiniz, `kubectl apply` böylece üzerinden ad alanı oluştururken `--save-config` seçeneği kullanmanız gerekir.

```console
kubectl create namespace istio-system --save-config
```

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - create secrets for Grafana and Kiali](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash check for CRDs](includes/servicemesh/istio/install-create-secrets-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell check for CRDs](includes/servicemesh/istio/install-create-secrets-powershell.md)]

::: zone-end

### <a name="install-istio-components"></a>Istio bileşenlerini yükleyin

Artık AKS kümemizde Grafana ve Kiali sırlarını başarıyla yarattığımıza göre, Istio bileşenlerini yükleme nin zamanı geldi. 

Istio için [Helm][helm] kurulum yaklaşımı gelecekte küçümsülecektir. Istio için yeni kurulum `istioctl` yaklaşımı, istemci ikili, [Istio yapılandırma profilleri][istio-configuration-profiles]ve yeni [Istio kontrol düzlemi spec ve api][istio-control-plane]kaldıraçları . Bu yeni yaklaşım, Istio'yı yüklemek için kullanacağımız şey.

> [!NOTE]
> Istio şu anda Linux düğümleri üzerinde çalışacak şekilde zamanlanmış olmalıdır. Kümenizde Windows Server düğümleri varsa, Istio bölmelerinin yalnızca Linux düğümlerinde çalışacak şekilde zamanlandığını sağlamalısınız. Bölmelerin doğru düğümlere zamanlanmış olduğundan emin olmak için [düğüm seçicileri][kubernetes-node-selectors] kullanırız.

> [!CAUTION]
> [SDS (gizli keşif hizmeti)][istio-feature-sds] ve [Istio CNI][istio-feature-cni] Istio özellikleri Alpha şu anda [,][istio-feature-stages]bu yüzden bu etkinleştirmeden önce verilmelidir düşündüm. Buna ek olarak, [Servis Hesabı Belirteç Hacim][kubernetes-feature-sa-projected-volume] ProjeksiyonKubernetes özelliği (SDS için bir gereklilik) geçerli AKS sürümlerinde etkinleştirilmez.
Aşağıdaki içerikle `istio.aks.yaml` birlikte çağrılan bir dosya oluşturun. Bu dosya Istio yapılandırmak için [Istio kontrol düzlemi spec][istio-control-plane] ayrıntıları nı tutar.

```yaml
apiVersion: install.istio.io/v1alpha2
kind: IstioControlPlane
spec:
  # Use the default profile as the base
  # More details at: https://istio.io/docs/setup/additional-setup/config-profiles/
  profile: default
  values:
    global:
      # Ensure that the Istio pods are only scheduled to run on Linux nodes
      defaultNodeSelector:
        beta.kubernetes.io/os: linux
      # Enable mutual TLS for the control plane
      controlPlaneSecurityEnabled: true
      mtls:
        # Require all service to service communication to have mtls
        enabled: false
    grafana:
      # Enable Grafana deployment for analytics and monitoring dashboards
      enabled: true
      security:
        # Enable authentication for Grafana
        enabled: true
    kiali:
      # Enable the Kiali deployment for a service mesh observability dashboard
      enabled: true
    tracing:
      # Enable the Jaeger deployment for tracing
      enabled: true
```

Aşağıdaki gibi komutu `istioctl apply` ve yukarıdaki `istio.aks.yaml` Istio kontrol düzlemi spec dosyasını kullanarak istio yükleyin:

```console
istioctl manifest apply -f istio.aks.yaml --logtostderr --set installPackagePath=./install/kubernetes/operator/charts
```

Yükleyici bir dizi CRD dağıtacak ve ardından Istio'nun bu yapılandırması için tanımlanan ilgili nesnelerin tümünün yüklenmesini sağlamak için [bağımlılıkları][kubernetes-crd] yönetir. Aşağıdaki çıkış parçacığı gibi bir şey görmeniz gerekir.

```console
Applying manifests for these components:
- Tracing
- EgressGateway
- NodeAgent
- Grafana
- Policy
- Citadel
- CertManager
- IngressGateway
- Injector
- Prometheus
- PrometheusOperator
- Kiali
- Telemetry
- Galley
- Cni
- Pilot
- Base
- CoreDNS
NodeAgent is waiting on a prerequisite...
Telemetry is waiting on a prerequisite...
Galley is waiting on a prerequisite...
Cni is waiting on a prerequisite...
Grafana is waiting on a prerequisite...
Policy is waiting on a prerequisite...
Citadel is waiting on a prerequisite...
EgressGateway is waiting on a prerequisite...
Tracing is waiting on a prerequisite...
Kiali is waiting on a prerequisite...
PrometheusOperator is waiting on a prerequisite...
IngressGateway is waiting on a prerequisite...
Prometheus is waiting on a prerequisite...
CertManager is waiting on a prerequisite...
Injector is waiting on a prerequisite...
Pilot is waiting on a prerequisite...
Applying manifest for component Base
Waiting for CRDs to be applied.
CRDs applied.
Finished applying manifest for component Base
Prerequisite for Tracing has completed, proceeding with install.
Prerequisite for Injector has completed, proceeding with install.
Prerequisite for Telemetry has completed, proceeding with install.
Prerequisite for Policy has completed, proceeding with install.
Prerequisite for PrometheusOperator has completed, proceeding with install.
Prerequisite for NodeAgent has completed, proceeding with install.
Prerequisite for IngressGateway has completed, proceeding with install.
Prerequisite for Kiali has completed, proceeding with install.
Prerequisite for EgressGateway has completed, proceeding with install.
Prerequisite for Galley has completed, proceeding with install.
Prerequisite for Grafana has completed, proceeding with install.
Prerequisite for Cni has completed, proceeding with install.
Prerequisite for Citadel has completed, proceeding with install.
Applying manifest for component Tracing
Prerequisite for Prometheus has completed, proceeding with install.
Prerequisite for Pilot has completed, proceeding with install.
Prerequisite for CertManager has completed, proceeding with install.
Applying manifest for component Kiali
Applying manifest for component Prometheus
Applying manifest for component IngressGateway
Applying manifest for component Policy
Applying manifest for component Telemetry
Applying manifest for component Citadel
Applying manifest for component Galley
Applying manifest for component Pilot
Applying manifest for component Injector
Applying manifest for component Grafana
Finished applying manifest for component Kiali
Finished applying manifest for component Tracing
Finished applying manifest for component Prometheus
Finished applying manifest for component Citadel
Finished applying manifest for component Policy
Finished applying manifest for component IngressGateway
Finished applying manifest for component Injector
Finished applying manifest for component Galley
Finished applying manifest for component Pilot
Finished applying manifest for component Grafana
Finished applying manifest for component Telemetry

Component IngressGateway installed successfully:
================================================

serviceaccount/istio-ingressgateway-service-account created
deployment.apps/istio-ingressgateway created
gateway.networking.istio.io/ingressgateway created
sidecar.networking.istio.io/default created
poddisruptionbudget.policy/ingressgateway created
horizontalpodautoscaler.autoscaling/istio-ingressgateway created
service/istio-ingressgateway created

...
```

Bu noktada, Istio'yu AKS kümenize dağıttınız. Istio'nun başarılı bir şekilde konuşlandırıldığından emin olmak için, [Istio yüklemesini doğrulamak](#validate-the-istio-installation)için bir sonraki bölüme geçelim.

## <a name="validate-the-istio-installation"></a>Istio yüklemesini doğrulama

İlk olarak beklenen hizmetlerin oluşturulduğunu doğrulayın. Çalışan hizmetleri görüntülemek için svc komutu [almak kubectl][kubectl-get] kullanın. Istio ve eklenti bileşenlerinin `istio-system` `istio` Miğfer grafiği tarafından yüklendiği ad alanını sorgulayın:

```console
kubectl get svc --namespace istio-system --output wide
```

Aşağıdaki örnek çıktı, şimdi çalışıyor olması gereken hizmetleri gösterir:

- `istio-*`Hizmetleri
- `jaeger-*`, `tracing`ve `zipkin` eklenti izleme hizmetleri
- `prometheus`eklenti ölçümleri hizmeti
- `grafana`eklenti analizi ve izleme panosu hizmeti
- `kiali`eklenti hizmeti örgü panosu hizmeti

Harici `istio-ingressgateway` bir ip, `<pending>`bir IP adresi Azure ağ tarafından atanana kadar birkaç dakika bekleyin gösterirse.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                      AGE   SELECTOR
grafana                  ClusterIP      10.0.116.147   <none>           3000/TCP                                                                                                                     92s   app=grafana
istio-citadel            ClusterIP      10.0.248.152   <none>           8060/TCP,15014/TCP                                                                                                           94s   app=citadel
istio-galley             ClusterIP      10.0.50.100    <none>           443/TCP,15014/TCP,9901/TCP,15019/TCP                                                                                         93s   istio=galley
istio-ingressgateway     LoadBalancer   10.0.36.213    20.188.221.111   15020:30369/TCP,80:31368/TCP,443:30045/TCP,15029:32011/TCP,15030:31212/TCP,15031:32411/TCP,15032:30009/TCP,15443:30010/TCP   93s   app=istio-ingressgateway
istio-pilot              ClusterIP      10.0.23.222    <none>           15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                       93s   istio=pilot
istio-policy             ClusterIP      10.0.59.250    <none>           9091/TCP,15004/TCP,15014/TCP                                                                                                 93s   istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.123.219   <none>           443/TCP                                                                                                                      93s   istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.216.9     <none>           9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                       89s   istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                   96s   app=jaeger
jaeger-collector         ClusterIP      10.0.221.24    <none>           14267/TCP,14268/TCP,14250/TCP                                                                                                95s   app=jaeger
jaeger-query             ClusterIP      10.0.46.154    <none>           16686/TCP                                                                                                                    95s   app=jaeger
kiali                    ClusterIP      10.0.174.97    <none>           20001/TCP                                                                                                                    94s   app=kiali
prometheus               ClusterIP      10.0.245.226   <none>           9090/TCP                                                                                                                     94s   app=prometheus
tracing                  ClusterIP      10.0.249.95    <none>           9411/TCP                                                                                                                     95s   app=jaeger
zipkin                   ClusterIP      10.0.154.89    <none>           9411/TCP                                                                                                                     94s   app=jaeger
```

Ardından, gerekli bölmelerin oluşturulduğunu doğrulayın. [Kubectl get pods][kubectl-get] komutunu kullanın `istio-system` ve yine ad alanını sorgula:

```console
kubectl get pods --namespace istio-system
```

Aşağıdaki örnek çıktı, çalışan bölmeleri gösterir:

- `istio-*` bakla
- `prometheus-*` eklenti ölçümleri bölmesi
- `grafana-*` eklenti analizi ve izleme panosu bölmesi
- `kiali` eklenti hizmeti örgü pano bölmesi

```console
NAME                                          READY   STATUS    RESTARTS   AGE
grafana-6bc97ff99-k9sk4                       1/1     Running   0          92s
istio-citadel-6b5c754454-tb8nf                1/1     Running   0          94s
istio-galley-7d6d78d7c5-zshsd                 2/2     Running   0          94s
istio-ingressgateway-85869c5cc7-x5d76         1/1     Running   0          95s
istio-pilot-787d6995b5-n5vrj                  2/2     Running   0          94s
istio-policy-6cf4fbc8dc-sdsg5                 2/2     Running   2          94s
istio-sidecar-injector-5d5b978668-wrz2s       1/1     Running   0          94s
istio-telemetry-5498db684-6kdnw               2/2     Running   1          94s
istio-tracing-78548677bc-74tx6                1/1     Running   0          96s
kiali-59b7fd7f68-92zrh                        1/1     Running   0          95s
prometheus-7c7cf9dbd6-rjxcv                   1/1     Running   0          94s
```

Tüm bölmeler bir durum `Running`göstermelidir. Kapsüllerinizde bu durum yoksa, bunu öğrenene kadar bir iki dakika bekleyin. Herhangi bir bölme bir sorun bildiriyorsa, çıktılarını ve durumlarını gözden geçirmek için [kubectl describe pod][kubectl-describe] komutunu kullanın.

## <a name="accessing-the-add-ons"></a>Eklentilere erişim

Istio tarafından yukarıdaki kurulumumuza ek işlevsellik sağlayan bir dizi eklenti yüklendi. Eklentiler için web uygulamaları harici bir ip adresi üzerinden herkese açık **değildir.** 

Eklenti kullanıcı arabirimlerine erişmek için `istioctl dashboard` komutu kullanın. Bu komut, istemci makineniz ile AKS kümenizdeki ilgili bölme arasında güvenli bir bağlantı oluşturmak için [kubectl port ileri][kubectl-port-forward] ve rasgele bir bağlantı noktası ndan yararlanır. Daha sonra varsayılan tarayıcınızda eklenti web uygulamasını otomatik olarak açar.

Bu makalede daha önce onlar için kimlik bilgilerini belirterek Grafana ve Kiali için ek bir güvenlik katmanı ekledik.

### <a name="grafana"></a>Grafana

Istio için analiz ve izleme panoları [Grafana][grafana]tarafından sağlanmaktadır. Grafana sırrı aracılığıyla oluşturduğunuz kimlik bilgilerini istendiğinde daha önce kullanmayı unutmayın. Grafana panosunu aşağıdaki gibi güvenli bir şekilde açın:

```console
istioctl dashboard grafana
```

### <a name="prometheus"></a>Prometheus

Istio için ölçümler [Prometheus][prometheus]tarafından sağlanmaktadır. Prometheus panosunu aşağıdaki gibi güvenli bir şekilde açın:

```console
istioctl dashboard prometheus
```

### <a name="jaeger"></a>Jaeger

Istio içinde Izleme [Jaeger][jaeger]tarafından sağlanmaktadır. Jaeger panosunu aşağıdaki gibi güvenli bir şekilde açın:

```console
istioctl dashboard jaeger
```

### <a name="kiali"></a>Kiali

Bir hizmet örgü gözlem panosu [Kiali][kiali]tarafından sağlanmaktadır. İstendiğinde Kiali sırrı aracılığıyla oluşturduğunuz kimlik bilgilerini kullanmayı unutmayın. Kiali panosunu aşağıdaki gibi güvenli bir şekilde açın:

```console
istioctl dashboard kiali
```

### <a name="envoy"></a>Envoy

[Elçi][envoy] vekilleri için basit bir arayüz mevcuttur. Belirli bir bölmede çalışan bir Elçi proxy'si için yapılandırma bilgileri ve ölçümleri sağlar. Elçi arabirimini aşağıdaki gibi güvenli bir şekilde açın:

```console
istioctl dashboard envoy <pod-name>.<namespace>
```

## <a name="uninstall-istio-from-aks"></a>IStio'u AKS'den kaldırın

> [!WARNING]
> Istio'nun çalışan bir sistemden silmesi, hizmetleriniz arasında trafikle ilgili sorunlara neden olabilir. Devam etmeden önce Istio olmadan sisteminizin hala doğru çalışması için hükümler yaptığınızdan emin olun.

### <a name="remove-istio-components-and-namespace"></a>Istio bileşenlerini ve ad alanını kaldırma

ISTio'yu AKS kümenizden kaldırmak `istioctl manifest generate` için `istio.aks.yaml` Istio kontrol düzlemi spec dosyasının komutunu kullanın. Bu, yüklenen tüm bileşenleri ve `kubectl delete` `istio-system` ad alanını kaldırmak için boruyla çalışacağımız dağıtılmış bildirimi oluşturur.

```console
istioctl manifest generate -f istio.aks.yaml -o istio-components-aks --logtostderr --set installPackagePath=./install/kubernetes/operator/charts 

kubectl delete -f istio-components-aks -R
```

### <a name="remove-istio-crds-and-secrets"></a>Istio CRD'leri ve Sırları Kaldırın

Yukarıdaki komutlar tüm Istio bileşenleri ve ad alanı silmek, ama biz hala oluşturulan Istio sırları ile bırakılır. 

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - remove Istio secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - remove Istio secrets](includes/servicemesh/istio/uninstall-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - remove Istio secrets](includes/servicemesh/istio/uninstall-powershell.md)]

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgeler, bir kanarya sürümü dışarı rulo akıllı yönlendirme sağlamak için Istio nasıl kullanabileceğinizi açıklar:

> [!div class="nextstepaction"]
> [AKS Istio akıllı yönlendirme senaryosu][istio-scenario-routing]

Istio için daha fazla kurulum ve yapılandırma seçeneğini keşfetmek için aşağıdaki resmi Istio kılavuzuna bakın:

- [Istio - kurulum kılavuzları][istio-installation-guides]

Ayrıca aşağıdakileri kullanarak ek senaryolar da izleyebilirsiniz:

- [Istio Bookinfo Uygulama örneği][istio-bookinfo-example]

Uygulama Öngörüleri ve Istio'yu kullanarak AKS uygulamanızı nasıl izleyeceğinizi öğrenmek için aşağıdaki Azure Monitör belgelerine bakın:

- [Kubernetes için sıfır enstrümantasyon uygulama izleme uygulamaları barındırılan][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-faq]: https://istio.io/faq/general/
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/news/
[istio-installation-guides]: https://istio.io/docs/setup/install/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-istioctl]: https://istio.io/docs/setup/install/istioctl/
[istio-configuration-profiles]: https://istio.io/docs/setup/additional-setup/config-profiles/
[istio-control-plane]: https://istio.io/docs/reference/config/istio.operator.v1alpha1/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/

[istio-feature-stages]: https://istio.io/about/feature-stages/
[istio-feature-sds]: https://istio.io/docs/tasks/traffic-management/ingress/secure-ingress-sds/
[istio-feature-cni]: https://istio.io/docs/setup/additional-setup/cni/

[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-feature-sa-projected-volume]: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection
[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
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
