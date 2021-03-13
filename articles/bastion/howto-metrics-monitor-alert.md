---
title: Azure Izleyici 'yi kullanarak izlemeyi ve ölçümleri yapılandırma
titleSuffix: Azure Bastion
description: Azure Izleyici 'yi kullanarak Azure Izleme ve ölçümler hakkında bilgi edinin, Azure 'da ölçümler, uyarı, tanılama günlükleri çözümü.
services: bastion
author: mialdrid
ms.service: bastion
ms.topic: how-to
ms.date: 03/12/2021
ms.author: mialdrid
ms.openlocfilehash: 3a5beba3938b5a845a378ede155f2f64e6baac7a
ms.sourcegitcommit: df1930c9fa3d8f6592f812c42ec611043e817b3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103417952"
---
# <a name="how-to-configure-monitoring-and-metrics-for-azure-bastion-using-azure-monitor"></a>Azure Izleyici kullanarak Azure savunma için izleme ve ölçümleri yapılandırma

Bu makale, Azure Izleyici 'yi kullanarak Azure için izleme ve ölçülerle çalışmanıza yardımcı olur.

>[!NOTE]
>**Klasik ölçümlerin** kullanılması önerilmez.
>

## <a name="about-metrics"></a>Ölçümler hakkında

Azure savunma 'nın kullanılabilir çeşitli ölçümleri vardır. Aşağıdaki tabloda, kullanılabilir her ölçüm için kategori ve boyutlar gösterilmektedir.

|**Ölçüm**|**Kategori**|**Boyut (ler)**|
| --- | --- | --- |
|Savunma iletişim durumu * *|[Kullanılabilirlik](#availability)|Yok|
|Toplam bellek|[Kullanılabilirlik](#availability)|Örnek|
|Kullanılan CPU|[Trafik](#traffic)|Örnek
|Kullanılan bellek|[Trafik](#traffic)|Örnek
|Oturum sayısı|[Performans](#performance)|Örnek|

* * Savunma iletişim durumu yalnızca Kasım 2020 ' den sonra dağıtılan savunma konakları için geçerlidir.

### <a name="availability-metrics"></a><a name="availability"></a>Kullanılabilirlik ölçümleri

#### <a name="bastion-communication-status"></a><a name="communication-status"></a>Savunma iletişim durumu

Savunma ana bilgisayarıyla oluşan tüm örneklerde toplanan Azure savunma iletişim durumunu görüntüleyebilirsiniz.

* **1** değeri, savunma için kullanılabilir olduğunu gösterir.
* **0** değeri, savunma hizmetinin kullanılamadığını belirtir.

:::image type="content" source="./media/metrics-monitor-alert/communication-status.png" alt-text="İletişim durumunu gösteren ekran görüntüsü.":::

#### <a name="total-memory"></a><a name="total-memory"></a>Toplam bellek

Azure savunma 'nın toplam bellek miktarını görüntüleyerek her bir savunma örneğine bölebilirsiniz.

:::image type="content" source="./media/metrics-monitor-alert/total-memory.png" alt-text="Toplam belleği gösteren ekran görüntüsü.":::

### <a name="traffic-metrics"></a><a name="traffic"></a>Trafik ölçümleri

#### <a name="used-cpu"></a><a name="used-cpu"></a>Kullanılan CPU

Azure savunma 'nın CPU kullanımını, her bir savunma örneğine göre bölüşrmek üzere görüntüleyebilirsiniz. Bu ölçümü izlemek, Azure savunma oluşturan örneklerin kullanılabilirliğini ve kapasitesini ölçmeye yardımcı olur.

:::image type="content" source="./media/metrics-monitor-alert/used-cpu.png" alt-text="Kullanılan CPU 'YU gösteren ekran görüntüsü.":::

#### <a name="used-memory"></a><a name="used-memory"></a>Kullanılan bellek

Her bir savunma örneğine göre bölünen her bir savunma örneği üzerinde bellek kullanımını görüntüleyebilirsiniz. Bu ölçümü izlemek, Azure savunma oluşturan örneklerin kullanılabilirliğini ve kapasitesini ölçmeye yardımcı olur.

:::image type="content" source="./media/metrics-monitor-alert/used-memory.png" alt-text="Kullanılan belleği gösteren ekran görüntüsü.":::

### <a name="performance-metrics"></a><a name="performance"></a>Performans ölçümleri

#### <a name="session-count"></a>Oturum sayısı

Her bir oturum türü (RDP ve SSH) arasında toplanan etkin oturum başına etkin oturum sayısını görüntüleyebilirsiniz. Her bir Azure savunma, etkin RDP ve SSH oturumlarının bir aralığını destekleyebilir. Bu ölçümü izlemek, savunma hizmetini çalıştıran örnek sayısını ayarlamanız gerektiğini anlamanıza yardımcı olur. Azure savunma 'nın destekleyeabileceği oturum sayısı hakkında daha fazla bilgi için [Azure savunma hakkında SSS](bastion-faq.md)bölümüne bakın.

:::image type="content" source="./media/metrics-monitor-alert/session-count.png" alt-text="Oturum sayısını gösteren ekran görüntüsü.":::

## <a name="how-to-view-metrics"></a><a name="metrics"></a>Ölçümleri görüntüleme

1. Ölçümleri görüntülemek için savunma ana bilgisayarınıza gidin.
1. **İzleme** listesinden **ölçümler**' i seçin.
1. Parametreleri seçin. Hiçbir ölçüm ayarlanmamışsa, **ölçüm Ekle**' ye tıklayın ve ardından parametreleri seçin.

   * **Kapsam:** Varsayılan olarak, kapsam, savunma ana bilgisayarına ayarlanır.
   * **Ölçüm ad alanı:** Standart ölçümler.
   * **Ölçüm:** Görüntülemek istediğiniz ölçümü seçin.

1. Ölçüm seçildikten sonra, varsayılan toplama uygulanır. İsteğe bağlı olarak, ölçüyü farklı boyutlara sahip olacak şekilde bölerek bölmeyi uygulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md)makalesini okuyun.
  
