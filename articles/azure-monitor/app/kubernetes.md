---
title: Azure Kubernetes Hizmetinizi (AKS) veya diğer Kubernetes barındırılan uygulamalarınızı izlemek için Uygulama Öngörülerini kullanın - Azure Monitör | Microsoft Dokümanlar
description: Azure Monitor, Kubernetes kümenizde hizmet kafesi teknolojisini kullanır, Istio, kubernetes barındırılan herhangi bir Kubernetes uygulaması için uygulama izleme sağlamak için. Bu, kümenizde çalışan bölmelere gelen ve giden isteklerle ilgili Uygulama Öngörüleri telemetrisini toplamanızı sağlar.
ms.topic: conceptual
author: tokaplan
ms.author: alkaplan
ms.date: 04/25/2019
ms.openlocfilehash: 56a0cb66f5b54c817067970ab369d7ca471a1696
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132356"
---
# <a name="zero-instrumentation-application-monitoring-for-kubernetes-hosted-applications"></a>Kubernetes için sıfır enstrümantasyon uygulama izleme uygulamaları barındırılan

> [!IMPORTANT]
> Bu işlevsellik şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure Monitor artık Kubernetes kümenizde hizmet örgü teknolojisinden yararlanarak, Kubernetes tarafından barındırılan herhangi bir Uygulama için kutu uygulama izleme sini sağlıyor. Bağımlılıklarınızı modellemek için [Uygulama Haritası,](../../azure-monitor/app/app-map.md) gerçek zamanlı izleme için [Canlı Ölçümler Akışı,](../../azure-monitor/app/live-stream.md) [varsayılan pano,](../../azure-monitor/app/overview-dashboard.md) [Metrik Gezgin](../../azure-monitor/platform/metrics-getting-started.md)ve [Çalışma Kitapları](../../azure-monitor/app/usage-workbooks.md)ile güçlü görselleştirmeler gibi varsayılan Uygulama Öngörüsü özellikleriyle. Bu özellik, kullanıcıların seçili Bir Kubernetes ad alanı içinde tüm Kubernetes iş yüklerinde performans darboğazlarını ve başarısız etkin noktalarını tespit etmelerine yardımcı olur. Azure Monitor, Mevcut hizmet örgü yatırımlarınızdan Istio gibi teknolojilerle yararlanarak, uygulamanızın kodunda herhangi bir değişiklik yapmadan otomatik araçla etkinleştirilen uygulama izleme olanağı sağlar.

> [!NOTE]
> Bu Kubernetes uygulama izleme gerçekleştirmek için birçok yollarından biridir.Ayrıca, Hizmet örgüsüne gerek kalmadan [Application Insights SDK'yı](../../azure-monitor/azure-monitor-app-hub.yml) kullanarak Kubernetes'te barındırılan herhangi bir uygulamaya da araç gerebilirsiniz. Uygulamayı SDK ile uygulama yapmadan Kubernetes'i izlemek için aşağıdaki yöntemi kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Bir [Kubernetes kümesi.](https://docs.microsoft.com/azure/aks/concepts-clusters-workloads)
- *Kubectl*çalıştırmak için kümeye konsol erişimi .
- [Uygulama Öngörüsü kaynağı](create-new-resource.md)
- Bir servis örgü var. Kümenizde Istio dağıtılmıyorsa, [Azure Kubernetes Hizmeti'nde Istio'nun nasıl yüklenilip kullanılacağını](https://docs.microsoft.com/azure/aks/istio-install)öğrenebilirsiniz.

## <a name="capabilities"></a>Özellikler

Kubernetes barındırılan uygulamalar için sıfır enstrümantasyon uygulaması izleme kullanarak, kullanmak mümkün olacak:

- [Uygulama Eşlemesi](../../azure-monitor/app/app-map.md)
- [Canlı Akış Ölçümleri](../../azure-monitor/app/live-stream.md)
- [Panolar](../../azure-monitor/app/overview-dashboard.md)
- [Ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md)
- [Dağıtılmış izleme](../../azure-monitor/app/distributed-tracing.md)
- [Uçuca hareket izleme](../../azure-monitor/learn/tutorial-performance.md#identify-slow-server-operations)

## <a name="installation-steps"></a>Yükleme adımları

Çözümü etkinleştirmek için aşağıdaki adımları gerçekleştireceğiz:
- Uygulamayı dağıtın (zaten dağıtılmıyorsa).
- Uygulamanın servis kafesinin bir parçası olduğundan emin olun.
- Toplanan telemetriyi gözlemleyin.

### <a name="configure-your-app-to-work-with-a-service-mesh"></a>Uygulamanızı bir hizmet ağıyla çalışacak şekilde yapılandırın

Istio [bir pod enstrümanting](https://istio.io/docs/setup/kubernetes/additional-setup/sidecar-injection/)iki yolu destekler.
Çoğu durumda, uygulamanızı içeren Kubernetes ad alanını *istio-enjeksiyon* etiketiyle işaretlemek en kolayıdır:

```console
kubectl label namespace <my-app-namespace> istio-injection=enabled
```

> [!NOTE]
> Servis kafesi verileri kablodan kaldırdığından, şifreli trafiği durduramayız. Kümeden çıkmayan trafik için şifrelenmemiş bir iletişim kuralı (örneğin, HTTP) kullanın. Şifrelenmiş olması gereken dış trafik için giriş denetleyicisi'nde [TLS sonlandırma ayarlamayı](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls) düşünün.

Hizmet kafesi dışında çalışan uygulamalar etkilenmez.

### <a name="deploy-your-application"></a>Uygulamanızı dağıtma

- Uygulamanızı *uygulama mama-namespace ad* alanına dağıtın. Uygulama zaten dağıtıldıysa ve yukarıda açıklanan otomatik yan araba enjeksiyon yöntemini izlediyseniz, Istio'nun yan arabasına enjekte ettiğinden emin olmak için bölmeleri yeniden oluşturmanız gerekir; yuvarlanma güncelleştirmesi başlatın veya tek tek bölmeleri silin ve bunların yeniden oluşturulmasını bekleyin.
- Uygulamanızın [Istio gerekliliklerine](https://istio.io/docs/setup/kubernetes/prepare/requirements/)uygun olduğundan emin olun.

### <a name="deploy-zero-instrumentation-application-monitoring-for-kubernetes-hosted-apps"></a>Kubernetes barındırılan uygulamalar için sıfır enstrümantasyon uygulama izleme dağıtma

1. Uygulama [ *Öngörüleri bağdaştırıcısı* sürümü](https://github.com/Microsoft/Application-Insights-Istio-Adapter/releases/)indirin ve ayıklayın.
2. Sürüm klasöründe */src/kubernetes/* 'a gidin.
3. *Uygulama-öngörüler-istio-mikser-adaptör-deployment.yaml* düzenleme
    - telemetriyi içerecek şekilde Azure portalındaki Application Insights kaynağının enstrümantasyon anahtarını içerecek şekilde *ISTIO_MIXER_PLUGIN_AI_INSTRUMENTATIONKEY* ortam değişkeninin değerini edin.
    - Gerekirse, izlemeyi etkinleştirmek istediğiniz virgülden ayrılmış ad alanları listesini içerecek şekilde *ISTIO_MIXER_PLUGIN_WATCHLIST_NAMESPACES* ortam değişkeninin değerini edin. Tüm ad alanlarını izlemek için boş bırakın.
4. *Src/kubernetes* altında bulunan *her* YAML dosyasını uygulayın/ aşağıdakileri çalıştırarak (hala içinde */src/kubernetes/* olmalıdır):

   ```console
   kubectl apply -f .
   ```

### <a name="verify-deployment"></a>Dağıtımı doğrulama

- Uygulama Öngörüleri bağdaştırıcının dağıtıldığından emin olun:

  ```console
  kubectl get pods -n istio-system -l "app=application-insights-istio-mixer-adapter"
  ```
> [!NOTE]
> Bazı durumlarda, ince ayar ayar gereklidir. Tek bir bölmenin toplanmasını engellemek için telemetriyi eklemek veya hariç tutmak için, o bölmede *appinsights/monitoring.enabled* etiketini kullanın. Bu, tüm ad alanı tabanlı yapılandırma üzerinde önceliğe sahip olacaktır. *Uygulama öngörülerini/izlemeyi ayarlayın.bölmeyi* içerecek *şekilde doğru* ve dışlamak için *yanlış* olmasını etkinleştirin.

### <a name="view-application-insights-telemetry"></a>Uygulama Öngörülerini telemetriyi görüntüle

- İzlemenin düzgün çalıştığını doğrulamak için uygulamanız aleyhine örnek bir istek oluşturun.
- 3-5 dakika içinde Azure portalında telemetrinin göründüğünü görmeye başlamalısınız. Portaldaki Uygulama Öngörüleri kaynağınızın *Uygulama Haritası* bölümüne göz atettiğinizden emin olun.

## <a name="troubleshooting"></a>Sorun giderme

Aşağıda, telemetri beklendiği gibi Azure portalında görünmediğinde kullanılacak sorun giderme akışı verilmiştir.

1. Uygulamanın yük altında olduğundan ve istekleri düz HTTP'de gönderdiğinden/aldığından emin olun. Telemetri telden kaldırıldığıiçin şifreli trafik desteklenmez. Gelen veya giden istek yoksa, telemetri de olmayacaktır.
2. Doğru enstrümantasyon anahtarının *uygulama-öngörüler-istio-mikser-adaptör-deployment.yaml'de* *ISTIO_MIXER_PLUGIN_AI_INSTRUMENTATIONKEY* ortam değişkeninde sağlandığından emin olun. Enstrümantasyon anahtarı, Azure portalındaki Uygulama Öngörüleri kaynağının *Genel Bakış* sekmesinde bulunur.
3. Doğru Kubernetes ad alanı *uygulama-insights-istio-mikser-adaptör-deployment.yaml* *ISTIO_MIXER_PLUGIN_WATCHLIST_NAMESPACES* ortam değişkeninde sağlanmıştır emin olun. Tüm ad alanlarını izlemek için boş bırakın.
4. Uygulamanızın kapsüllerinin Istio tarafından yan araba enjekte edildiğinden emin olun. Istio'nun arabasının her bölmede olduğunu doğrulayın.

   ```console
   kubectl describe pod -n <my-app-namespace> <my-app-pod-name>
   ```
   Bölmede *istio proxy* adlı bir kapsayıcı olduğunu doğrulayın.

5. Application Insights bağdaştırıcısının izlerini görüntüleyin.

   ```console
   kubectl get pods -n istio-system -l "app=application-insights-istio-mixer-adapter"
   kubectl logs -n istio-system application-insights-istio-mixer-adapter-<fill in from previous command output>
   ```

   Alınan telemetri öğelerinin sayısı dakikada bir güncelleştirilir. Dakika içinde büyümezse - hiçbir telemetri Istio tarafından adaptör gönderiliyor.
   Günlükte herhangi bir hata arayın.
6. *Kubernetes* bağdaştırıcısı için Uygulama Öngörüsü'nün telemetriyle beslenmediği tespit edildiyse, bağdaştırıcıya neden veri göndermediğini anlamak için Istio'nun Mikser günlüklerini kontrol edin:

   ```console
   kubectl get pods -n istio-system -l "istio=mixer,app=telemetry"
   kubectl logs -n istio-system istio-telemetry-<fill in from previous command output> -c mixer
   ```
   Özellikle *applicationinsightsadapter adaptörü* ile iletişim ile ilgili herhangi bir hata arayın.

## <a name="faq"></a>SSS

Bu projedeki ilerleme yle ilgili en son bilgiler [için, Istio Mixer projesinin GitHub'ı için Application Insights adaptörlerini](https://github.com/Microsoft/Application-Insights-Istio-Adapter/blob/master/SETUP.md#faq)ziyaret edin.

## <a name="uninstall"></a>Kaldırma

Ürünü kaldırmak için, *src/kubernetes/* run altında bulunan *her* YAML dosyası için:

```console
kubectl delete -f <filename.yaml>
```


## <a name="next-steps"></a>Sonraki adımlar

Azure Monitör ve kapsayıcıların birlikte nasıl çalıştığı hakkında daha fazla bilgi edinmek [için kapsayıcılara genel bakış için Azure Monitor'u](../../azure-monitor/insights/container-insights-overview.md) ziyaret edin
