---
title: Azure Red Hat OpenShift 4,3 için Azure Izleyici tümleştirmesi
description: Microsoft Azure Red Hat OpenShift kümenizde Azure Izleyicisini nasıl etkinleştireceğinizi öğrenin.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
ms.openlocfilehash: 1a372ef12ef7b0afb06ed3c540daaee5e38c3374
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78899171"
---
# <a name="azure-monitor-integration-for-azure-red-hat-openshift-43"></a>Azure Red Hat OpenShift 4,3 için Azure Izleyici tümleştirmesi

> [!IMPORTANT]
> Azure Red Hat OpenShift 4,3, önizlemede sunulmaktadır. Önizleme özellikleri Self-Service ' dir ve olduğu gibi sağlanır ve hizmet düzeyi sözleşmesi (SLA) ve sınırlı garantiden çıkarılır. Bu nedenle, Özellikler üretim kullanımı için tasarlanmamıştır. 

Bu makalede, şirket içi veya herhangi bir bulut ortamında barındırılan OpenShift 4,3 kümelerine yönelik kapsayıcılar için Azure Izleyici 'nin özel önizlemesine nasıl etkinleştirileceği açıklanır. Ayrıca, Azure Red Hat OpenShift (ARO) 4,3 kümelerine yönelik izlemeyi etkinleştirmek için de aynı yönergeler geçerlidir.  

## <a name="prerequisites"></a>Önkoşullar

- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Helb 3](https://helm.sh/docs/intro/install/)
- Kubernetes kümesinin kubeconfig 'e erişim
- Bir Azure aboneliğine erişim
- Openshıft 4,3 kümesine erişim-kapsayıcılar için Azure Monitor Helm grafiğini yüklemek için
- Azure aboneliğinde en düşük katkıda bulunan RBAC rolü izni  
- İzleme Aracısı, izleme verilerini Azure Izleyici arka ucuna göndermek için aşağıdaki giden bağlantı noktalarını ve etki alanlarını gerektirir (proxy/güvenlik duvarı tarafından engellenmişse):
  - *. ods.opinsights.azure.com 443
  - *. oms.opinsights.azure.com 443
  - *. blob.core.windows.net 443
  - dc.services.visualstudio.com 443

## <a name="onboarding"></a>Ekleme

> [!TIP]
> Betik Bash 4 özelliklerini kullanır, bu nedenle Bash 'nizin güncel olduğundan emin olun. Güncel sürümünüzü `bash --version`kontrol edebilirsiniz.

### <a name="download-the-onboarding-script"></a>Ekleme betiğini indirin

```bash
curl -LO  https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/openshiftV4/onboarding_azuremonitor_for_containers.sh
```

Aşağıdaki betiği Azuyeniden gönderme Scriptionıd, çalışma alanı bölgesi, clusterName ve Kubernetes kümesinin bağlamıyla yürütün.

```bash
bash onboarding_azuremonitor_for_containers.sh <azureSubscriptionId> <azureRegionforLogAnalyticsWorkspace> <clusterName> <kubeconfigContextNameOftheCluster>
```

Örnek:

```bash
 bash onboarding_azuremonitor_for_containers.sh 27ac26cf-a9f0-4908-b300-9a4e9a0fb205 eastus myocp42 admin 
```

## <a name="configure-agent-data-collection"></a>Aracı veri toplamayı yapılandır

Varsayılan olarak, Izleme Aracısı, Kuto-System hariç tüm ad alanlarında çalışan tüm kapsayıcıların {stdout; stderr} kapsayıcı günlüklerini toplar.  Kapsayıcı günlük koleksiyonunu belirli bir ad alanına veya ad alanlarına özgü olarak yapılandırmak istiyorsanız, [kapsayıcı öngörüleri aracı yapılandırmasına](../azure-monitor/insights/container-insights-agent-config.md)başvurabilirsiniz. Burada, yapılandırma haritasını kullanarak, istenen veri toplama ayarlarıyla Izleme aracısını yapılandırabilirsiniz.

## <a name="configure-scraping-of-prometheus-metrics"></a>Prometheus ölçümlerinin korumasını yapılandırın

Kapsayıcılar için Azure Izleyici, Prometheus ölçümlerini ve Azure Izleyici arka ucunu alma. Prometheus scraping yapılandırma yönergeleri için [Container Insights Prometheus yapılandırması](../azure-monitor/insights/container-insights-prometheus-integration.md) ' na bakın.

Başarılı olduktan sonra, [karma izlemeye](https://aka.ms/azmon-containers-hybrid) gidin ve yeni eklendi openshıft v4 kümenizi görüntülemek için ortamı **"tümü"** olarak seçin.

## <a name="disable-monitoring"></a>İzlemeyi devre dışı bırakma

İzlemeyi devre dışı bırakmak istiyorsanız, kapsayıcılar arka ucu için Azure Izleyici 'ye toplamayı ve izleme verilerini toplamayı durdurmak üzere aşağıdaki komutu kullanarak kapsayıcılar için Azure Izleyicisi Helm grafiğini silebilirsiniz.

``` bash
helm del azmon-containers-release-1
```

## <a name="update-monitoring"></a>İzlemeyi Güncelleştir

Hazırlama bölümünde açıklandığı gibi ekleme [betiğini, en](#onboarding) son hele grafiğine güncelleştirmek için aynı parametreyi kullanarak yeniden çalıştırın.

## <a name="after-successful-onboarding"></a>Başarılı ekleme sonrasında

[Karma izlemeye](https://aka.ms/azmon-containers-hybrid)gidin ve yeni etkinleştirilen OpenShift/Aro v4 kümenizi **izlenen kümeler** sekmesinde sistem durumu ile birlikte görebilirsiniz. Burada, **küme** sütununa tıklayarak ölçümler, envanter ve Günlükler gibi daha derin Öngörüler elde edebilirsiniz.

## <a name="supported-features"></a>Desteklenen özellikler

Desteklenen özellikler ve işlevler hakkında daha fazla bilgi için bkz. [Container Insights 'a genel bakış](../azure-monitor/insights/container-insights-overview.md).

Geri bildirim ve soru için askcoin@microsoft.com aracılığıyla bizimle iletişim kurun.

## <a name="next-steps"></a>Sonraki adımlar

İzleme hakkında daha fazla bilgi için bkz.
- [Kapsayıcı öngörülerine genel bakış](../azure-monitor/insights/container-insights-overview.md)

- [Günlük sorgusuna genel bakış](../azure-monitor/log-query/log-query-overview.md)
