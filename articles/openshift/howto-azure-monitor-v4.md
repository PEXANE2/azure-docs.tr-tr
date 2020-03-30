---
title: Azure Red Hat OpenShift 4.3 için Azure Monitör tümleştirmesi
description: Microsoft Azure Red Hat OpenShift kümenizde Azure Monitor'u nasıl etkinleştirizleyeceğinizi öğrenin.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/06/2020
ms.openlocfilehash: a784fc070400995c56d16a3bc264d589bcb1f64e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082855"
---
# <a name="azure-monitor-integration-for-azure-red-hat-openshift-43"></a>Azure Red Hat OpenShift 4.3 için Azure Monitör tümleştirmesi

> [!IMPORTANT] 
> Azure Red Hat OpenShift 4.3'ün şu anda yalnızca Doğu ABD'de özel önizlemede kullanılabildiğini lütfen unutmayın. Özel önizleme kabul sadece davetiye ile. Lütfen bu özelliği etkinleştirmeye çalışmadan önce aboneliğinizi kaydettiğinizden emin olun: [Azure Red Hat OpenShift Özel Önizleme Kaydı](https://aka.ms/aro-preview-register)

> [!NOTE]
> Önizleme özellikleri self servistir ve olduğu gibi ve mevcut olduğu şekilde sağlanır ve hizmet düzeyi sözleşmesi (SLA) ve sınırlı garanti dışında dır. Bu nedenle, özellikler üretim kullanımı için değildir.

Bu makalede, prem'de veya herhangi bir bulut ortamında barındırılan OpenShift 4.3 kümeleri için kapsayıcılar için Azure Monitor'un özel önizlemesinin nasıl etkinleştirilen anlatılmaktadır. Aynı yönergeler, Azure Red Hat OpenShift (ARO) 4.3 kümelerinin izlenmesini etkinleştirmek için de geçerlidir.  

## <a name="prerequisites"></a>Ön koşullar

- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Dümen 3](https://helm.sh/docs/intro/install/)
- Kubernetes kümesinin kubeconfig'ine erişim
- Azure aboneliği erişimi
- Kapsayıcılar Miğferi grafiği için Azure Monitörünü yüklemek için OpenShift 4.3 kümesine erişim
- Azure Aboneliği'nde minimum Katılımcı RBAC rol izni  
- İzleme Aracısı, izleme verilerini Azure Monitor arka ucuna göndermek için aşağıdaki giden bağlantı noktalarını ve etki alanlarını gerektirir (Proxy/güvenlik duvarı tarafından engellendiyse):
  - *.ods.opinsights.azure.com 443
  - *.oms.opinsights.azure.com 443
  - *.blob.core.windows.net 443
  - dc.services.visualstudio.com 443

## <a name="onboarding"></a>Ekleme

> [!TIP]
> Komut dosyası bash 4 özelliklerini kullanır, bu nedenle bash güncel olduğundan emin olun. Geçerli sürümünüzü ' `bash --version`ile kontrol edebilirsiniz.

### <a name="download-the-onboarding-script"></a>Onboarding komut dosyasını indirin

```bash
curl -LO  https://raw.githubusercontent.com/microsoft/OMS-docker/ci_feature/docs/openshiftV4/onboarding_azuremonitor_for_containers.sh
```

Aşağıdaki komut dosyasını azureSubscriptionId, çalışma alanı Bölgesi, clusterName ve Kubernetes kümesinin bağlamı yla yürütün.

```bash
bash onboarding_azuremonitor_for_containers.sh <azureSubscriptionId> <azureRegionforLogAnalyticsWorkspace> <clusterName> <kubeconfigContextNameOftheCluster>
```

Örnek:

```bash
 bash onboarding_azuremonitor_for_containers.sh 27ac26cf-a9f0-4908-b300-9a4e9a0fb205 eastus myocp42 admin 
```

## <a name="configure-agent-data-collection"></a>Aracı veri toplamayı yapılandırma

Varsayılan olarak, İzleme Aracısı kube-sistem hariç tüm ad alanlarında çalışan tüm kapsayıcıların {stdout; stderr} kapsayıcı günlüklerini toplar.  Belirli ad alanına veya ad alanlarına özgü kapsayıcı günlük koleksiyonunu yapılandırmak istiyorsanız, [Kapsayıcı Öngörüleri aracıyapılandırmasına](../azure-monitor/insights/container-insights-agent-config.md)başvurabilirsiniz. Burada, config haritasını kullanarak İzleme aracıyı istenilen veri toplama ayarlarıyla yapılandırabilirsiniz.

## <a name="configure-scraping-of-prometheus-metrics"></a>Prometheus ölçümlerinin yapılandırılması

Kapsayıcılar için Azure Monitörü Prometheus ölçümlerini sıyırır ve Azure Monitor arka ucuna yutun. Prometheus kazıma nasıl yapılandırılabilen talimatlar için [Container Insights Prometheus yapılandırmasına](../azure-monitor/insights/container-insights-prometheus-integration.md) bakın.

Başarılı bir şekilde bindikten [sonra, Karma İzleme'ye](https://aka.ms/azmon-containers-hybrid) gidin ve yeni binilen OpenShift v4 kümenizi görüntülemek için Çevre'yi **"Tümü"** olarak seçin.

## <a name="disable-monitoring"></a>İzlemeyi devre dışı bırakma

İzlemeyi devre dışı bırakmak istiyorsanız, kapsayıcıların arka ucu için izleme verilerini toplamayı ve Azure Monitor'a sindiren leri durdurmak için aşağıdaki komutu kullanarak Kapsayıcılar Için Azure Monitörü Miğfer grafiğini silebilirsiniz.

``` bash
helm del azmon-containers-release-1
```

## <a name="update-monitoring"></a>İzlemeyi güncelleştir

Onboarding bölümünde açıklandığı gibi [onboarding](#onboarding) komut dosyasını, en son Miğfer grafiğine güncelleştirmek için aynı parametreyle yeniden yayın.

## <a name="after-successful-onboarding"></a>Başarılı onboarding sonra

Karma [İzleme'ye](https://aka.ms/azmon-containers-hybrid)gidin ve Yeni etkinleştirilen OpenShift/ARO v4 kümenizi **Izlenen Kümeler** sekmesinde sistem durumu yla görebilirsiniz. Burada, **Küme** sütununa tıklayarak ölçümler, envanter ve günlükler gibi daha derin öngörülere girebilirsiniz.

## <a name="supported-features"></a>Desteklenen özellikler

Desteklenen özellikler ve işlevler hakkında daha fazla bilgi [için, Container Insights genel bakış](../azure-monitor/insights/container-insights-overview.md)bakın.

Geri bildirim askcoin@microsoft.com ve sorularınız için bize ulaşın.

## <a name="next-steps"></a>Sonraki adımlar

İzleme hakkında daha fazla bilgi edinmek için bkz:
- [Konteyner Öngörüleri genel bakış](../azure-monitor/insights/container-insights-overview.md)

- [Günlük Sorgusuna genel bakış](../azure-monitor/log-query/log-query-overview.md)
