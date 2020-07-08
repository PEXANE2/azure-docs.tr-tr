---
title: Video kaydı-Azure
description: CCTV kameralarının bir video yönetim sistemi bağlamında video kaydı, kameralardan video yakalama ve mobil ve tarayıcı uygulamaları aracılığıyla görüntülenmek üzere kaydetme sürecini ifade eder. Video kaydı, sürekli video kaydı ve olay tabanlı video kaydı olarak kategorilere ayrılmıştır.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: c03472f577e67d5b2705a2ebe3f53e85e0849819
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84260949"
---
# <a name="video-recording"></a>Video kaydı

CCTV kameralarının bir video yönetim sistemi bağlamında video kaydı, kameralardan video yakalama ve mobil ve tarayıcı uygulamaları aracılığıyla görüntülenmek üzere kaydetme sürecini ifade eder. Video kaydı, sürekli video kaydı ve olay tabanlı video kaydı olarak kategorilere ayrılmıştır. 

## <a name="continuous-video-recording"></a>Sürekli video kaydı  

Sürekli video kaydı (CVR), bir video kaynağından yakalanan tüm videonun sürekli olarak kaydedilme sürecini ifade eder. CVR, daha sonraki bir zamanda çözümlemek ve/veya denetlemek için tüm videonun ( [kayıt ilkesi](#recording-policy)tarafından dikte edilen) kullanılabilmesini sağlar.

## <a name="event-based-video-recording"></a>Olay tabanlı video kaydı  

Olay tabanlı video kaydı (EVR), bir olay tarafından tetiklenen videoyu kaydetme sürecini ifade eder. Söz konusu olay, video sinyalinin kendisinin işlenmesi veya bağımsız bir kaynaktan (örneğin, bir kapı sensörden) kaynaklanabilir. Olay tabanlı video kaydı (EVR), depolama tasarruflarına yol açabilir, ancak olayları oluşturan ve video kaydını tetikleyen ek bileşenler gerektirir (önceden tanımlanmış bir ilke için). Diğer bir deyişle, EVR, video kaydını tetiklemesi gereken olaylarla ilgili ek kararlar vermenizi ve video kaydıyla ilişkili ilkeyi (kayıt ilkesi olarak da bilinir) gerektirir. Bir ilkeye örnek olarak şunlar olabilir: videoyu olay zamanından önce 30 saniyeden başlayarak 2 dakika boyunca kaydedin. Söz konusu olaylar, kameranın kendisindeki video işleme nedeniyle kaynaklı olabilir. Örnek olarak, kameranın görünüm penceresinin önceden yapılandırılmış bir bölgesi içinde hareket algılandığında bir hareket algılama sinyal olayı oluşturulmasını destekler. Olaylar ayrıca, kameradan video yakalayan başka bir cihazdaki videoyu işleyerek ve basit görüntü işleme teknikleri veya gelişmiş makine öğrenimi modelleri kullanılarak analiz edilebilir. 

## <a name="choosing-recording-modes"></a>Kayıt modlarını seçme  

CVR veya EVR kullanmanın seçimi, iş hedeflerine bağlıdır. IoT Edge canlı video analizi hem CVR hem de EVR için platform özellikleri sağlar. [CVR](continuous-video-recording-concept.md) ve [EVR](event-based-video-recording-concept.md) senaryo makalelerinde bu konuda daha fazla bilgi edinebilirsiniz.

## <a name="recording-policy"></a>Kayıt ilkesi  

Kayıt ilkesi, kayıt başlatma/durdurma zamanını (EVR durumunda), kaydetme süresini ve kaydı silme işlemini başlatan ilkelere başvurur. Kayıt ilkeleri, depolama maliyetini iş gereksinimleriyle dengelemenize olanak tanır. Kayıt ilkeleri CVR ve EVR arasında farklılık gösterir. CVR için kayıt ilkesi, kaç gün videonun depolanması gerektiğini tanımlar (örneğin, son 7 gün). EVR için kayıt ilkesi, kaydın ne zaman başlayıp bitdiğinin, videonun süresiyle birlikte tanımlar. [CVR](continuous-video-recording-concept.md) ve [EVR](event-based-video-recording-concept.md) senaryo makalelerinde bu konuda daha fazla bilgi edinebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Media Services’ta hareket algılama, video klipleri kaydetme](detect-motion-record-video-clips-media-services-quickstart.md)
* [Hareket algılama, video kliplerini uç cihazlara kaydetme](detect-motion-record-video-clips-edge-devices-quickstart.md)

