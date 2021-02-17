---
title: Kapsayıcılar için Azure Izleyici raporları
description: Kapsayıcılar için Azure Izleyici tarafından toplanan verileri analiz etmek için kullanılabilir raporları açıklar.
ms.topic: conceptual
ms.date: 12/07/2020
ms.openlocfilehash: 94709cf4a8b579447c11f91f9bc6863ab4b4fe08
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100582315"
---
# <a name="reports-in-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici raporları
Kapsayıcılar için Azure Izleyici raporlarında, kullanıma hazır [Azure çalışma kitapları](../visualize/workbooks-overview.md)önerilir. Bu makalede, kullanılabilir farklı raporlar ve bunlara nasıl erişebileceğiniz açıklanmıştır.

## <a name="viewing-reports"></a>Raporları görüntüleme
Azure portal **Azure izleyici** menüsünde **kapsayıcılar**' ı seçin. **İzleme** bölümünde **Öngörüler** ' i seçin, belirli bir kümeyi seçin ve ardından **raporlar (Önizleme)** sayfasını seçin. 

[![Raporlar sayfası](media/container-insights-reports/reports-page.png)](media/container-insights-reports/reports-page.png#lightbox)

## <a name="create-a-custom-workbook"></a>Özel çalışma kitabı oluşturma
Bu çalışma kitaplarından herhangi birine dayalı özel bir çalışma kitabı oluşturmak için, **çalışma kitaplarını görüntüle** açılan listesini seçin ve ardından açılan listenin altındaki **aks galerisine gidin** . Çalışma kitapları ve çalışma kitabı şablonlarını kullanma hakkında daha fazla bilgi için bkz. [Azure Izleyici çalışma kitapları](../visualize/workbooks-overview.md)

[![AKS Galerisi](media/container-insights-reports/aks-gallery.png)](media/container-insights-reports/aks-gallery.png#lightbox)

## <a name="node-workbooks"></a>Düğüm çalışma kitapları

- **Disk kapasitesi**: aşağıdaki perspektiflere göre bir kapsayıcı içindeki düğüme sunulan her bir disk için etkileşimli disk kullanımı grafikleri:

    - Tüm diskler için disk yüzdesi kullanımı.
    - Tüm diskler için boş disk alanı.
    - Her bir düğümün diskini, kullanılan alan yüzdesini, kullanılan alan yüzdesi, boş disk alanı (GiB) ve boş disk alanı eğilimi (GiB) gösteren bir kılavuz. Tabloda bir satır seçildiğinde, kullanılan alanın yüzdesi ve boş disk alanı (GiB), satırın altında gösterilir.

- **DISK GÇ**: aşağıdaki perspektiflere göre bir kapsayıcı içindeki düğüme sunulan her bir disk için etkileşimli disk kullanımı grafikleri:

    - Disk g/ç, okunan bayt/sn, bayt/sn yazma ve okuma ve yazma bayt/sn eğilimlerini ile tüm diskler arasında özetlenir.
    - Sekiz performans grafiği disk g/ç performans sorunlarını ölçmenize ve belirlemesine yardımcı olacak ana performans göstergelerini gösterir.

- **GPU**: GPU kullanan her Kubernetes küme düğümü IÇIN etkileşimli GPU kullanım grafikleri.

## <a name="resource-monitoring-workbooks"></a>Kaynak Izleme çalışma kitapları

- **Dağıtımlar**: dağıtımlarınızın durumu (özel hPa 'ler dahil) yatay Pod otomatik Scaler (hPa) &. 
  
- **Iş yükü ayrıntıları**: bir ad alanı için iş yüklerinin performans istatistiklerini gösteren etkileşimli grafikler. Birden çok sekme içerir:

  - POD tarafından CPU ve bellek kullanımına genel bakış.
  - POD yeniden başlatma eğilimi, kapsayıcı yeniden başlatma eğilimi ve PODs için kapsayıcı durumu gösteren POD/Container durumu.
  - Kubernetes, denetleyicinin olaylarının özetini gösteren olaylar.

- **Kubelet**: anahtar düğüm işletim istatistiklerini gösteren iki kılavuz içerir:

    - Node Grid 'e göre genel bakış, her düğüm için yüzde ve eğilim bazında toplam işlem, toplam hata ve başarılı işlemleri özetler.
    - İşlem türüne göre genel bakış toplam işlem, toplam hata ve yüzde ve eğilim bazında başarılı işlemler için her bir işlem için özetler.
## <a name="billing-workbooks"></a>Faturalandırma çalışma kitapları

- **Veri kullanımı**: belgelerimizde paylaşdığımız bir sorgu kitaplığını oluşturmaya gerek kalmadan verilerinizin kaynağını görselleştirmenize yardımcı olur. Bu çalışma kitabında, bu tür perspektiflerden faturalanabilir verileri şu şekilde görüntüleyebilmeniz gereken grafikler vardır:

  - Çözüme göre GB cinsinden alınan toplam faturalanabilir veri
  - Kapsayıcı günlükleri tarafından alınan faturalanabilir veriler (uygulama günlükleri)
  - Faturalanabilir kapsayıcı verileri Kubernetes ad alanı başına alınır
  - Faturalanabilir kapsayıcı, küme adına göre ayrılmış verileri günlüğe kaydeder
  - Günlük kaynak girişi tarafından alınan faturalanabilir kapsayıcı günlük verileri
  - Tanılama ana düğüm günlükleri tarafından alınan faturalandırılabilir Tanılama verileri

## <a name="networking-workbooks"></a>Ağ çalışma kitapları

- **NPM yapılandırması**: Ağ İlkesi Yöneticisi (NPM) Ile yapılandırılan ağ yapılandırmalarınızı izleme.

  - Genel yapılandırma karmaşıklığı hakkında özet bilgiler.
  - İlke, kural ve zaman içinde sayımlar, üç ve bir yapılandırmanın hata ayıklaması için bir zaman boyutu ekleme arasındaki ilişki hakkında öngörüler sağlar.
  - Tüm ıpsets ve her IPSet giriş sayısı.
  - Ağ yapılandırmanıza bileşen eklemek için düğüm başına en kötü ve ortalama durum performansı.

- **Ağ**: her düğümün ağ bağdaştırıcısı için etkileşimli ağ kullanım grafikleri ve bir ızgara, ağ bağdaştırıcılarınızın performansını ölçmenize yardımcı olmak için ana performans göstergelerini sunar.



## <a name="next-steps"></a>Sonraki adımlar

- Azure Izleyici 'deki çalışma kitapları hakkındaki ayrıntılar için bkz. [Azure Izleyici çalışma kitapları](../visualize/workbooks-overview.md) .
