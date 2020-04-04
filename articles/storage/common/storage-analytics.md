---
title: Günlükler ve ölçümler verileri toplamak için Azure Depolama analitiğini kullanın | Microsoft Dokümanlar
description: Depolama Analitiği, tüm depolama hizmetleri için metrik verilerini izlemenize ve Blob, Queue ve Tablo depolama için günlükler toplamanıza olanak tanır.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/03/2017
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 4ad9f13bcdf36b67400adb62d58ee260ff256bb3
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80637138"
---
# <a name="storage-analytics"></a>Depolama Analizi

Azure Depolama Analizi günlüğe kaydetme yapar ve bir depolama hesabı için metrik verileri sağlar. Bu verileri istekleri izlemek, kullanım eğilimlerini çözümlemek ve depolama hesabınızla ilgili sorunları tanılamak için kullanabilirsiniz.

Depolama Analizi'ni kullanmak için, izlemek istediğiniz her hizmet için bunu ayrı ayrı etkinleştirmeniz gerekir. [Azure portalından](https://portal.azure.com)etkinleştirebilirsiniz. Ayrıntılar için Azure [portalındaki bir depolama hesabını izleyin'e](storage-monitor-storage-account.md)bakın. Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi'ni programlı olarak etkinleştirebilirsiniz. Her hizmet için Depolama Analizi'ni etkinleştirmek için [Blob Hizmet Özelliklerini Ayarla,](/rest/api/storageservices/set-blob-service-properties) [Sıra Hizmet Özelliklerini Ayarla](/rest/api/storageservices/set-queue-service-properties)ve Dosya [Hizmeti](/rest/api/storageservices/set-table-service-properties) [Özelliklerini Ayarla](/rest/api/storageservices/Get-File-Service-Properties) işlemlerini ayarlayın.

Toplanan veriler, Blob hizmeti ve Tablo hizmeti API'leri kullanılarak erişilebilen tanınmış bir blob (günlüğe kaydetme için) ve tanınmış tablolarda (ölçümler için) depolanır.

Depolama Analitiği, depolama hesabınız için toplam sınırdan bağımsız olarak depolanan veri miktarı üzerinde 20 TB sınırına sahiptir. Depolama hesabı sınırları hakkında daha fazla bilgi [için, standart depolama hesapları için Ölçeklenebilirlik ve performans hedeflerine](scalability-targets-standard-account.md)bakın.

Azure Depolama ile ilgili sorunları tanımlamak, tanılamak ve sorun gidermek için Depolama Analizi'ni ve diğer araçları kullanma yla ilgili ayrıntılı bir kılavuz için Microsoft [Azure Depolama Depolamasını İzle, tanılama ve sorun giderme](storage-monitoring-diagnosing-troubleshooting.md)konusuna bakın.

## <a name="billing-for-storage-analytics"></a>Depolama Analitiği için Faturalandırma

Tüm ölçümler verileri bir depolama hesabının hizmetleri tarafından yazılır. Sonuç olarak, Depolama Analytics tarafından gerçekleştirilen her yazma işlemi faturalandırılabilir. Ayrıca, ölçüm verileri tarafından kullanılan depolama miktarı da faturalandırılabilir.

Depolama Analizi tarafından gerçekleştirilen aşağıdaki eylemler faturalandırılabilir:

* Günlüğe kaydetme için blobs oluşturma istekleri.
* Ölçümler için tablo varlıkları oluşturma istekleri.

Bir veri saklama ilkesi yapılandırmışsanız, Depolama Analitiği eski günlük ve metrik verilerini sildiğinde hareketleri silmeniz için ücretlendirilmezsiniz. Ancak, bir istemciden silme hareketleri faturalandırılabilir. Bekletme ilkeleri hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/azure/hh343263.aspx)

### <a name="understanding-billable-requests"></a>Faturalandırılabilir istekleri anlama

Bir hesabın depolama hizmetine yapılan her istek faturalandırılabilir veya faturalandırılamaz. Depolama Analizi, isteğin nasıl işleneceğini gösteren bir durum iletisi de dahil olmak üzere bir hizmete yapılan her bir isteği kaydeder. Benzer şekilde, Depolama Analitiği, belirli durum iletilerinin yüzdeleri ve sayısı da dahil olmak üzere, hem bir hizmet hem de bu hizmetin API işlemleri için ölçümler depolar. Bu özellikler birlikte, faturalandırılabilir isteklerinizi çözümlemenize, uygulamanızda iyileştirmeler yapmanıza ve hizmetlerinizdeki isteklerle ilgili sorunları tanılamanıza yardımcı olabilir. Faturalandırma hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/archive/blogs/windowsazurestorage/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity)

Depolama Analitiği verilerine bakarken, hangi isteklerin faturalandırılabilir olduğunu belirlemek için [Depolama Analitiği Günlüğe Kaydedilmiş İşlemler ve Durum İletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) konusundaki tabloları kullanabilirsiniz. Ardından, belirli bir istek için ücretlendirilip ücretlendirilmediğinizi görmek için günlüklerinizi ve ölçüm verilerinizi durum iletileri ile karşılaştırabilirsiniz. Bir depolama hizmeti nin veya tek tek API işleminin kullanılabilirliğini araştırmak için önceki konudaki tabloları da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure portalında depolama hesabını izleme](storage-monitor-storage-account.md)
* [Depolama Analizi Ölçümleri](storage-analytics-metrics.md)
* [Depolama Analizi Günlüğü](storage-analytics-logging.md)
