---
title: Kapsayıcılar için Azure Monitör ile Kubernetes küme durumunu izleyin | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar için Azure Monitor ile AKS ve AKS dışı kümelerinizin durumunu nasıl görüntüleyebilir ve çözümleyebilirsiniz.
ms.topic: conceptual
ms.date: 12/01/2019
ms.openlocfilehash: f50ef13efca78bbb5285b99759b8111dc1915ad0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76843999"
---
# <a name="understand-kubernetes-cluster-health-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitor ile Kubernetes küme durumunu anlayın

Kapsayıcılar için Azure Monitor ile, yönetilen altyapı bileşenlerinin ve kapsayıcılar için Azure Monitor tarafından desteklenen kubernetes kümesinde çalışan tüm düğümlerin sistem durumunu izler ve raporlar. Bu deneyim, kümedeki bir veya daha fazla düğümün kaynak kısıtlı mı yoksa kümedeki bir düğümün veya bölmenin kümedeki çalışan bir uygulamayı küratörlü ölçümlere dayalı olarak etkileyebilecek kullanılamadığını anlayabileceğiniz [çok küme görünümünde](container-insights-analyze.md#multi-cluster-view-from-azure-monitor)hesaplanan ve bildirilen küme durumu durumunun ötesine uzanır.

>[!NOTE]
>Sistem Durumu özelliği şu anda genel önizlemede.
>

Kapsayıcılar için Azure Monitörü'ne nasıl etkinleştirilir hakkında bilgi için [kapsayıcılar için Yerleşik Azure Monitör'e](container-insights-onboard.md)bakın.

>[!NOTE]
>AKS Engine kümelerini desteklemek için aşağıdakileri karşıladığını doğrulayın:
>- [BU HELM istemcisinin](https://helm.sh/docs/using_helm/)en son sürümünü kullanıyor.
>- Containerized agent sürümü *microsoft/oms:ciprod11012019*. Aracıyı yükseltmek için [Kubernetes kümesindeki yükseltme aracısına](container-insights-manage-agent.md#upgrade-agent-on-monitored-kubernetes-cluster)bakın.
>

## <a name="overview"></a>Genel Bakış

Kapsayıcılar için Azure Monitor'da, Sistem Durumu (önizleme) özelliği, sorunları belirlemenize ve tanılamanıza yardımcı olmak için Kubernetes kümenizin proaktif sistem durumu izlemesini sağlar. Algılanan önemli sorunları görüntüleme olanağı sağlar. Kümenizin durumunu değerlendiren monitörler kümenizdeki kapsayıcı aracıüzerinde çalışır ve sistem durumu verileri Log Analytics çalışma alanınızdaki **KubeHealth** tablosuna yazılır. 

Kubernetes küme durumu, aşağıdaki Kubernetes nesneleri ve soyutlamaları tarafından düzenlenen bir dizi izleme senaryosuna dayanır:

- Kubernetes altyapısı - CPU ve bellek kullanımını değerlendirerek kümenizde dağıtılan düğümler üzerinde çalışan Kubernetes API sunucusu, Çoğaltma Setleri ve DaemonSets'in bir toplamasını sağlar ve Pods kullanılabilirliğini

    ![Kubernetes altyapı sağlık toplama görünümü](./media/container-insights-health/health-view-kube-infra-01.png)

- Düğümler - CPU ve bellek kullanımını değerlendirerek, her havuzda düğüm havuzları ve tek tek Düğümlerin durumunu niçin bir toplama sağlar ve Kubernetes tarafından bildirilen bir Düğümün durumunu.

    ![Düğümler sistem durumu toplama görünümü](./media/container-insights-health/health-view-nodes-01.png)

Şu anda, sadece sanal bir kubelet durumu desteklenir. CPU ve sanal kublet düğümlerinin bellek kullanımı için sistem durumu **Bilinmiyor**olarak bildirilir , bir sinyal onlardan alınmadığından beri.

Tüm monitörler, Kubernetes nesnesini veya soyutlamasını (diğer bir deyişle, Kubernetes altyapısı veya Düğümleri) temsil eden bir toplu monitörün tüm bunların birleşik durumunu yansıtan en üst düzey monitör olduğu Sağlık Hiyerarşisi bölmesinde hiyerarşik bir düzende gösterilir. bağımlı alt monitörler. Sistem durumu türetmek için kullanılan temel izleme senaryoları şunlardır:

* Düğüm ve kapsayıcıdan CPU kullanımını değerlendirin.
* Düğüm ve kapsayıcıdan bellek kullanımını değerlendirin.
* Kubernetes tarafından bildirilen hazır durumlarının hesaplanmasına dayalı bakla ve düğümlerin durumu.

Durumu göstermek için kullanılan simgeler şunlardır:

|Simge|Anlamı|  
|--------|-----------|  
|![Yeşil onay simgesi sağlam durumunu gösterir](./media/container-insights-health/healthyicon.png)|Başarılı, sistem durumu hazır (yeşil)|  
|![Sarı üçgen ve ünlem işareti uyarı simgesidir](./media/container-insights-health/warningicon.png)|Uyarı (sarı)|  
|![Üzerinde beyaz X bulunan kırmızı düğme kritik durumu gösterir](./media/container-insights-health/criticalicon.png)|Kritik (kırmızı)|  
|![Gri renk simgesi](./media/container-insights-health/grayicon.png)|Bilinmiyor (gri)|  

## <a name="monitor-configuration"></a>Yapılandırmayı izleyin

Kapsayıcılar Durumu özelliği için Azure Monitor'u destekleyen her monitörün davranışını ve yapılandırmasını anlamak için [Sistem Durumu denetim yapılandırma kılavuzuna](container-insights-health-monitors-config.md)bakın.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com)oturum açın. 

## <a name="view-health-of-an-aks-or-non-aks-cluster"></a>AKS veya AKS olmayan bir kümenin sistem durumunu görüntüleme

Kapsayıcılar için Azure Monitörü'ne erişim Sağlık (önizleme) özelliği, Azure portalındaki sol bölmeden **Öngörüler'i** seçerek doğrudan bir AKS kümesinden kullanılabilir. İstatistikler bölümünde **Kapsayıcılar'ı** seçin. **Containers** 

Aks olmayan bir kümeden sistem durumunu görüntülemek için, şirket içinde veya Azure Yığını'nda barındırılan bir AKS Engine kümesi, Azure portalındaki sol bölmeden **Azure Monitör'ü** seçin. İstatistikler bölümünde **Kapsayıcılar'ı** seçin. **Containers**  Çok kümeli sayfada, listeden AKS olmayan kümeyi seçin.

Kapsayıcılar için Azure **Monitör'de, Küme** sayfasından **Sistem Durumu'nu**seçin.

![Küme durumu panosu örneği](./media/container-insights-health/container-insights-health-page.png)

## <a name="review-cluster-health"></a>İnceleme küme durumu

Sistem Durumu sayfası açıldığında, varsayılan olarak **Kubernetes Altyapısı** **Sağlık Yönü** tablosunda seçilir.  Izgara, Kubernetes altyapısının ve küme düğümlerinin mevcut sistem toplama durumunu özetler. Sistem durumu yönü seçili olarak, Sistem Durumu Hiyerarşisi bölmesindeki (yani orta bölmedeki) sonuçları güncelleştirir ve tüm alt monitörleri geçerli sistem durumu durumunu gösteren hiyerarşik bir düzende gösterir. Bağımlı monitör hakkında daha fazla bilgi görüntülemek için, sayfanın sağ tarafında otomatik olarak bir tane seçebilir ve özellik bölmesi görüntülenir. 

![Küme durumu özelliği bölmesi](./media/container-insights-health/health-view-property-pane.png)

Özellik bölmesinde aşağıdakileri öğrenirsiniz:

- Genel **Bakış** sekmesinde, seçilen monitörün geçerli durumunu, monitörün en son ne zaman hesaplandığını ve son durum değişikliğinin ne zaman gerçekleştiğini gösterir. Hiyerarşide seçilen monitörtürüne bağlı olarak ek bilgiler gösterilir.

    Sistem Durumu Hiyerarşisi bölmesinde bir toplu monitör seçerseniz, özellik bölmesindeki **Genel Bakış** sekmesinin altında hiyerarşideki toplam alt monitör sayısının bir toplaması ve kritik, uyarı ve sağlıklı durumda kaç toplu monitör ün olduğu gösterir. 

    ![Toplu izleme için sistem durumu bölmesi Genel Bakış sekmesi](./media/container-insights-health/health-overview-aggregate-monitor.png)

    Sistem Durumu Hiyerarşisi bölmesinde bir birim izleme birimi seçerseniz, son **durum değişikliği** altında, son dört saat içinde kapsayıcı aracı tarafından hesaplanan ve bildirilen önceki örnekleri de gösterir. Bu, durumunu belirlemek için birkaç ardışık değeri karşılaştırmak için birim monitörler hesaplamasına dayanır. Örneğin, Pod hazır *durum* birimi monitörünü seçtiyseniz, *Artarda ÖrnekleDevlet Geçişi*parametresi tarafından denetlenir son iki örneği gösterir. Daha fazla bilgi için [birim monitörlerin](container-insights-health-monitors-config.md#unit-monitors)ayrıntılı açıklamasına bakın.
    
    ![Sistem durumu bölmesi Genel Bakış sekmesi](./media/container-insights-health/health-overview-unit-monitor.png)

    **Son durum değişikliği** tarafından bildirilen süre bir gün veya daha eskiyse, bu, monitör için durum değişikliğinin sonucudur. Ancak, bir birim izleme için alınan son örnek dört saatten eskiyse, bu büyük olasılıkla kapsayıcı aracının veri göndermediğini gösterir. Aracı belirli bir kaynağın (örneğin bir Düğüm) var olduğunu biliyorsa, ancak Düğümün CPU'sundan veya bellek kullanım monitörlerinden (örnek olarak) veri almamışsa, monitörün sistem durumu **Bilinmiyor**olarak ayarlanır.  

- **Config** sekmesinde, varsayılan yapılandırma parametre ayarlarını (yalnızca birim monitörler için değil, toplam monitörler için) ve değerlerini gösterir.
- **Bilgi** sekmesinde, monitörün davranışını ve sağlıksız durumu nasıl değerlendirdiği hakkında bilgi içerir.

Bu sayfadaki verileri izlemek otomatik olarak yenilenmez ve kümeden alınan en son sistem durumu durumunu görmek için sayfanın üst kısmındaki **Yenile'yi** seçmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Kümelerinizi uyarmak, görselleştirmek veya analiz etmek için önceden tanımlanmış sorguları ve örnekleri görmek veya değerlendirmek için [günlük sorgu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin.
