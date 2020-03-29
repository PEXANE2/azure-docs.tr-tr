---
title: Azure Servis Kumaş Etkinlikleri
description: Azure Hizmet Kumaşı kümenizi izlemenize yardımcı olmak için kutudan sağlanan Hizmet Kumaşı etkinlikleri hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 638b650e485ad3e83bd6021639a7e55b540d9cdc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451723"
---
# <a name="service-fabric-events"></a>Service Fabric olayları 

Service Fabric platformu, kümenizde gerçekleşen önemli operasyonel etkinlikler için çeşitli yapılandırılmış olaylar yazar. Bunlar küme yükseltmelerinden yineleme yerleşim kararlarına kadar uzanır. Service Fabric'in kümedeki aşağıdaki varlıklardan birine haritaları maruz ettiği her olay:
* Küme
* Uygulama
* Hizmet
* Bölüm
* Çoğaltma 
* Kapsayıcı

Platform tarafından maruz kalan olayların tam listesini görmek için - [Hizmet Kumaşı etkinlikleri listesi.](service-fabric-diagnostics-event-generation-operational.md)

Aşağıda, kümenizde görülmesi gereken bazı senaryo örnekleri verilmiştir. 
* Düğüm yaşam döngüsü olayları: düğümler yukarı çıktıkça, indikçe, küçülttün/çıktı, yeniden başlatılır ve etkinleştirildikçe/devre dışı bırakıldıkça, bu olaylar size ne olduğunu gösterir ve makinenin kendisinde bir sorun olup olmadığını veya bir düğümün durumunu değiştirmek için SF aracılığıyla çağrılan bir API olup olmadığını belirlemenize yardımcı olur.
* Küme yükseltme: küme yükseltildikçe (SF sürümü veya yapılandırma değişikliği), yükseltmenin başlatıldığını, Yükseltme Etki Alanlarınızın her birinde yuvarlanan ve tamamlanan (veya geri alma) göreceksiniz. 
* Uygulama yükseltmeleri: küme yükseltmeleri gibi, yükseltme rulo olarak olayların kapsamlı bir dizi vardır. Bu olaylar, yükseltmenin ne zaman zaman zamanlandığını, yükseltmenin geçerli durumunu ve olayların genel sırasını anlamak için yararlı olabilir. Bu, hangi yükseltmelerin başarıyla kullanıma sunulduğunun veya geri almanın tetiklenip tetiklenmediğini görmek için geçmişe bakmak için yararlıdır.
* Uygulama/Hizmet dağıtımı / silme: Her uygulama, hizmet ve kapsayıcı için, çoğaltılan veya silinen olaylar vardır ve örneğin çoğaltma sayısını artırırken veya çıkarken kullanışlıdır
* Bölüm hareketleri (yeniden yapılandırma): durumlu bir bölüm yeniden yapılandırmadan (yineleme kümesinde bir değişiklik) geçtiğinde, bir olay günlüğe kaydedilir. Bu, bölüm yineleme kümenizin ne sıklıkta değiştiğini veya üzerinde başarısız olduğunu anlamaya çalışıyorsanız veya birincil yinelemenizi herhangi bir zamanda çalıştıran düğümün izini sürmek için yararlıdır.
* Kaos Olayları: Service Fabric'in [Kaos](service-fabric-controlled-chaos.md) hizmetini kullanırken, hizmet her başlatıldığında veya durdurulduğunda veya sisteme bir hata enjekte ettiğinde olayları görürsünüz.
* Sağlık Olayları: Hizmet Dokusu, bir Uyarı veya Hata sağlık raporu her oluşturulduğunda veya bir varlık tamam sağlık durumuna geri döndüğünde veya sağlık raporunun süresi dolduğunda sistem durumu olaylarını ortaya çıkarır. Bu olaylar, bir varlığın geçmiş sağlık istatistiklerini izlemek için çok yararlıdır. 

## <a name="how-to-access-events"></a>Etkinliklere nasıl erişilir?

Service Fabric etkinliklerine erişilebilen birkaç farklı yol vardır:
* Olaylar ETW/Windows Event günlükleri gibi standart kanallardan günlüğe kaydedilir ve Azure Monitor günlükleri gibi bunları destekleyen herhangi bir izleme aracı tarafından görüntülenebilir. Varsayılan olarak, portalda oluşturulan kümeler tanılamayı açtı ve Windows Azure tanılama aracısının olayları Azure tablo depolama alanına göndermesini sağlar, ancak yine de bunu günlük analiz kaynağınızla tümleştirmeniz gerekir. Daha fazla günlük veya performans sayacı ve [Azure Monitor günlükleri tümleştirmesi](service-fabric-diagnostics-event-analysis-oms.md) almak için kümenizin tanılama yapılandırmasını değiştirmek için [Azure Tanılama aracısını](service-fabric-diagnostics-event-aggregation-wad.md) yapılandırma hakkında daha fazla bilgi edinin
* EventStore hizmetinin kümeyi doğrudan veya Service Fabric Client Library aracılığıyla sorgulamanıza olanak tanıyan Dinlenme API'leri. [Küme olayları için Sorgu EventStore API'leri'ne](service-fabric-diagnostics-eventstore-query.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
* Kümenizi izleme hakkında daha fazla bilgi - [Küme ve platformu izleme](service-fabric-diagnostics-event-generation-infra.md).
* EventStore hizmeti hakkında daha fazla bilgi edinin - [EventStore hizmetine genel bakış](service-fabric-diagnostics-eventstore.md)
