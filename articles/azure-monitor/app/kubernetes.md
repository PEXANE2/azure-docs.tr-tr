---
title: Azure Kubernetes hizmeti (AKS) veya diğer Kubernetes barındırılan uygulamalarını izlemek için Application Insights kullanın-Azure Izleyici | Microsoft Docs
description: Azure Izleyici, Kubernetes barındırılan uygulamalar için uygulama izlemeye olanak sağlamak üzere Kubernetes kümenizdeki hizmet kafes teknolojisini kullanır. Bu, kümenizde çalışan ve bu kaynaklardan gelen ve giden isteklerle ilgili Application Insights telemetri toplamanıza olanak tanır.
ms.topic: conceptual
author: tokaplan
ms.author: alkaplan
ms.date: 04/25/2019
ms.openlocfilehash: c3b2b4e8868b73e22706c684d94371fe37885b9a
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83119254"
---
# <a name="zero-instrumentation-application-monitoring-for-kubernetes-hosted-applications-with-istio---deprecated"></a>Kubernetes barındırılan uygulamalar için, Istio-kullanım DıŞı olmadan sıfır izleme uygulaması izleme

> [!IMPORTANT]
> Bu işlevsellik şu anda kullanım dışıdır ve 1 Ağustos 2020 ' den sonra desteklenecek.
> Şu anda önerilen sürüm yalnızca [tek başına aracı aracılığıyla Java](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent)için geçerlidir.

Azure Izleyici, Kubernetes barındırılan uygulamalar için kullanıma hazır uygulama izlemeyi sağlamak üzere Kubernetes kümenizdeki hizmet ağı teknik ' i 'nden yararlanır. Bağımlılıklarınızı modellemek için [uygulama Haritası](../../azure-monitor/app/app-map.md) gibi varsayılan uygulama Insight özellikleriyle, gerçek zamanlı izleme için [canlı ölçüm akışı](../../azure-monitor/app/live-stream.md) , [varsayılan pano](../../azure-monitor/app/overview-dashboard.md), [Ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md)ve [çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)ile güçlü görselleştirmeler elde edin. Bu özellik, kullanıcıların seçili bir Kubernetes ad alanı içindeki Kubernetes iş yüklerinin tamamında performans sorunlarını ve hata etkin noktalarını belirleyebilmenize yardımcı olur. Azure Izleyici, mevcut hizmet ağı yatırımlarınızı, Istio gibi teknolojilerle büyük harfle sunarak, uygulamanızın kodunda herhangi bir değişiklik yapılmadan otomatik olarak izlenen uygulama izlemeyi sağlar.

> [!NOTE]
> Bu, Kubernetes üzerinde uygulama izleme gerçekleştirmenin birçok yöntemlerinden biridir.Ayrıca, bir hizmet ağı gerekmeden [Application Insights SDK 'yı](../../azure-monitor/azure-monitor-app-hub.yml) kullanarak Kubernetes 'te barındırılan herhangi bir uygulamayı da denetleyebilirsiniz. Bir SDK ile uygulamayı işaretlemeden Kubernetes 'i izlemek için aşağıdaki yöntemi kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Bir [Kubernetes kümesi](https://docs.microsoft.com/azure/aks/concepts-clusters-workloads).
- *Kubectl*çalıştırmak için kümeye konsol erişimi.
- Bir [Application Insight kaynağı](create-new-resource.md)
- Hizmet ağı vardır. Kümenizin güncel bir dağıtımı yoksa, [Azure Kubernetes hizmeti 'Nde istio 'yu yüklemeyi ve kullanmayı](https://docs.microsoft.com/azure/aks/istio-install)öğrenebilirsiniz.

## <a name="capabilities"></a>Özellikler

Kubernetes barındırılan uygulamalar için sıfır izleme uygulama izlemeyi kullanarak şunları kullanabilirsiniz:

- [Uygulama Eşlemesi](../../azure-monitor/app/app-map.md)
- [Canlı Akış ölçümleri](../../azure-monitor/app/live-stream.md)
- [Panolar](../../azure-monitor/app/overview-dashboard.md)
- [Ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md)
- [Dağıtılmış izleme](../../azure-monitor/app/distributed-tracing.md)
- [Uçtan uca işlem izleme](../../azure-monitor/learn/tutorial-performance.md#identify-slow-server-operations)

## <a name="installation-steps"></a>Yükleme adımları

Çözümü etkinleştirmek için aşağıdaki adımları gerçekleştiririz:
- Uygulamayı dağıtın (önceden dağıtılmamışsa).
- Uygulamanın hizmet ağı 'nın bir parçası olduğundan emin olun.
- Toplanan Telemetriyi gözlemleyin.

### <a name="configure-your-app-to-work-with-a-service-mesh"></a>Uygulamanızı bir hizmet ağıyla çalışacak şekilde yapılandırma

İstio, [Pod](https://istio.io/docs/setup/kubernetes/additional-setup/sidecar-injection/)'ın iki yolunu destekler.
Çoğu durumda, uygulama içeren Kubernetes ad alanını *istio-ekleme* etiketiyle işaretlemek en kolay yoldur:

```console
kubectl label namespace <my-app-namespace> istio-injection=enabled
```

> [!NOTE]
> Hizmet ağı, verileri tel kapatığında, şifrelenmiş trafiği ele geçirilemez. Kümeden ayrılmadan trafik için şifrelenmemiş bir protokol (örneğin, HTTP) kullanın. Şifrelenmesi gereken dış trafik için giriş denetleyicisinde [TLS sonlandırmayı ayarlamayı](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls) göz önünde bulundurun.

Hizmet ağı dışında çalışan uygulamalar etkilenmez.

### <a name="deploy-your-application"></a>Uygulamanızı dağıtma

- Uygulamanızı *-App-Namespace* ad alanına dağıtın. Uygulama zaten dağıtılmışsa ve yukarıda açıklanan otomatik sepet ekleme yöntemini izlediyseniz, Mao 'nun arabasını yeniden oluşturmasını sağlamak için pod 'yi yeniden oluşturmanız gerekir; bir sıralı güncelleştirme başlatın veya bağımsız bir dizin silin ve yeniden oluşturulması için bekleyin.
- Uygulamanızın [Istio gereksinimleriyle](https://istio.io/docs/setup/kubernetes/prepare/requirements/)uyumlu olduğundan emin olun.

### <a name="deploy-zero-instrumentation-application-monitoring-for-kubernetes-hosted-apps"></a>Kubernetes barındırılan uygulamalar için sıfır izleme uygulaması izlemeyi dağıtma

1. [ *Application Insights bağdaştırıcısı* sürümünü](https://github.com/Microsoft/Application-Insights-Istio-Adapter/releases/)indirip ayıklayın.
2. Yayın klasörü içinde */src/Kubernetes/* adresine gidin.
3. *Uygulama düzenleme-iço-karıştırıcı-bağdaştırıcı-Deployment. YAML*
    - *ISTIO_MIXER_PLUGIN_AI_INSTRUMENTATIONKEY* ortam değişkeninin değerini, telemetrinin bulunduğu Azure Portal Application Insights kaynağının izleme anahtarını içerecek şekilde düzenleyin.
    - Gerekirse, *ISTIO_MIXER_PLUGIN_WATCHLIST_NAMESPACES* ortam değişkeninin değerini, izlemeyi etkinleştirmek istediğiniz ad uzaylarının virgülle ayrılmış bir listesini içerecek şekilde düzenleyin. Tüm ad alanlarını izlemek için boş bırakın.
4. *Src/Kubernetes* altında bulunan *her* YAML dosyasını, aşağıdakileri çalıştırarak uygulayın (yine de */src/Kubernetes/* içinde olmalıdır):

   ```console
   kubectl apply -f .
   ```

### <a name="verify-deployment"></a>Dağıtımı doğrulama

- Application Insights bağdaştırıcının dağıtıldığından emin olun:

  ```console
  kubectl get pods -n istio-system -l "app=application-insights-istio-mixer-adapter"
  ```
> [!NOTE]
> Bazı durumlarda, ince ayar ayarlama gereklidir. Tek bir pod 'ın toplanmasına yönelik Telemetriyi dahil etmek veya hariç tutmak için, bu pod üzerinde *appınsights/Monitoring. Enabled* etiketini kullanın. Bu, ad alanı tabanlı tüm yapılandırma üzerinde önceliğe sahip olacaktır. *Appınsights/Monitoring* 'ı, Pod öğesini içerecek şekilde *true* , hariç tutmak için *false* olarak ayarlayın.

### <a name="view-application-insights-telemetry"></a>Application Insights telemetrisini görüntüleme

- İzlemenin düzgün çalıştığını doğrulamak için uygulamanıza yönelik örnek bir istek oluşturun.
- 3-5 dakika içinde, Azure portal Telemetriyi görmeye başlamanız gerekir. Portalda Application Insights kaynağınızın *uygulama Haritası* bölümünü kullanıma aldığınızdan emin olun.

## <a name="troubleshooting"></a>Sorun giderme

Aşağıda, telemetri Azure portal beklendiği gibi görünmediği zaman, kullanılacak sorun giderme akışı verilmiştir.

1. Uygulamanın yük altında olduğundan ve isteklerin düz HTTP olarak gönderilmesini/alınmasını sağlayın. Telemetriden yükseltilmemiş olduğundan, şifrelenmiş trafik desteklenmez. Gelen veya giden istekler yoksa, hiçbir telemetri olmaz.
2. *Application-Insights-istio-MIXER-Adapter-Deployment. YAML*içindeki *ISTIO_MIXER_PLUGIN_AI_INSTRUMENTATIONKEY* ortam değişkeninde doğru izleme anahtarının sağlandığından emin olun. İzleme anahtarı, Azure portal Application Insights kaynağın *genel bakış* sekmesinde bulunur.
3. *Application-Insights-istio-MIXER-Adapter-Deployment. YAML*içindeki *ISTIO_MIXER_PLUGIN_WATCHLIST_NAMESPACES* ortam değişkeninde doğru Kubernetes ad alanının sağlandığından emin olun. Tüm ad alanlarını izlemek için boş bırakın.
4. Uygulamanızın yer aldığından, Istio tarafından dışarıdan eklenmiş olduğundan emin olun. Her pod üzerinde, Icar 'ın yer aldığı yer olduğunu doğrulayın.

   ```console
   kubectl describe pod -n <my-app-namespace> <my-app-pod-name>
   ```
   Pod üzerinde çalışan bir *IO-proxy* adlı kapsayıcı olduğunu doğrulayın.

5. Application Insights bağdaştırıcısının izlemelerini görüntüleyin.

   ```console
   kubectl get pods -n istio-system -l "app=application-insights-istio-mixer-adapter"
   kubectl logs -n istio-system application-insights-istio-mixer-adapter-<fill in from previous command output>
   ```

   Alınan telemetri öğelerinin sayısı bir dakika sonra güncelleştirilir. Dakikada dakika büyümezse, bir teleo tarafından bağdaştırıcıya bir telemetri gönderilmez.
   Günlükteki hataları arayın.
6. *Kubernetes bağdaştırıcısı Için Application* Insights 'ın beslenmesini denetlüdüyse, bağdaştırıcıya nasıl veri göndermediğini anlamak için Ise 'nin karıştırıcı günlüklerini denetleyin:

   ```console
   kubectl get pods -n istio-system -l "istio=mixer,app=telemetry"
   kubectl logs -n istio-system istio-telemetry-<fill in from previous command output> -c mixer
   ```
   Özellikle *applicationınsi,* ile iletişimlerle ilgili herhangi bir hata olup olmadığına bakın.

## <a name="faq"></a>SSS

Bu projedeki ilerleme durumu hakkında en son bilgiler için, [Istio karıştırıcı projesinin GitHub Application Insights bağdaştırıcısını](https://github.com/Microsoft/Application-Insights-Istio-Adapter/blob/master/SETUP.md#faq)ziyaret edin.

## <a name="uninstall"></a>Kaldır

Ürünü kaldırmak için *src/Kubernetes/* Run altında bulunan *her* bir YAML dosyası için:

```console
kubectl delete -f <filename.yaml>
```


## <a name="next-steps"></a>Sonraki adımlar

Azure Izleyici ve kapsayıcısının birlikte nasıl çalıştığı hakkında daha fazla bilgi edinmek için [bkz. kapsayıcılar Için Azure izleyici genel bakış](../../azure-monitor/insights/container-insights-overview.md)
