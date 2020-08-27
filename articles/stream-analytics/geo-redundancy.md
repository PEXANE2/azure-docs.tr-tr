---
title: Azure Stream Analytics işleri için coğrafi yedeklilik elde edin
description: Bu makalede coğrafi Yük devretme yerine Azure Stream Analytics işlerin coğrafi yedekliliği nasıl elde edileceğini açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 08/26/2020
ms.openlocfilehash: 44b0394773485f8054eddc94c7d7e9b91baebc6d
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88951099"
---
# <a name="achieve-geo-redundancy-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri için coğrafi yedeklilik elde edin

Azure Stream Analytics, otomatik coğrafi Yük devretme sağlamaz, ancak aynı Stream Analytics işleri birden fazla Azure bölgesinde dağıtarak coğrafi yedeklilik elde edebilirsiniz. Her iş, yerel bir giriş ve yerel çıkış kaynaklarına bağlanır. Giriş verilerini iki bölgesel girişe göndermek ve iki bölgesel çıkış arasında mutabık kılmak için uygulamanızın sorumluluğundadır. Stream Analytics işleri iki ayrı varlıklardır.

Aşağıdaki diyagramda, Olay Hub 'ı girişi ve Azure veritabanı çıkışı ile örnek coğrafi olarak yedekli Stream Analytics iş dağıtımı gösterilmektedir.

:::image type="content" source="media/geo-redundancy/geo-redundant-jobs.png" alt-text="coğrafi olarak yedekli Stream Analytics işlerinin diyagramı":::

## <a name="primarysecondary-strategy"></a>Birincil/ikincil strateji

Uygulamanızın hangi bölgenin çıkış veritabanının birincil olduğunu ve ikincili olarak kabul edileceğini yönetmesi gerekir. Birincil bölge hatasında, uygulama ikincil veritabanına geçer ve bu veritabanından güncelleştirmeleri okumaya başlar. Yinelenen okumaların en aza indirmesine izin veren gerçek mekanizma uygulamanıza bağlıdır.Çıkışa ek bilgi yazarak bu işlemi basitleştirebilirsiniz. Örneğin, her çıktıya bir zaman damgası veya bir sıra KIMLIĞI ekleyerek yinelenen satırların önemsiz bir işlem atlanmasını sağlayabilirsiniz. Birincil bölge geri yüklendikten sonra, benzer mekanizması kullanarak ikincil veritabanıyla birlikte yakalar.

Farklı giriş ve çıkış türleri farklı coğrafi çoğaltma seçeneklerine izin verse de, her iki Event üreticileri ve olay tüketicileri için esneklik ve denetim sağladığından, coğrafi artıklık elde etmek için bu makalede özetlenen deseninin kullanılması önerilir.

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell ile Azure Stream Analytics işlerini izleme ve yönetme](stream-analytics-monitor-and-manage-jobs-use-powershell.md)
* [Azure Stream Analytics veri odaklı hata ayıklama](stream-analytics-job-diagram-with-metrics.md)