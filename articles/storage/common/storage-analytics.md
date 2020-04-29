---
title: Günlük ve ölçüm verilerini toplamak için Azure Storage Analytics 'i kullanma | Microsoft Docs
description: Depolama Analizi, tüm depolama hizmetleri için ölçüm verilerini izlemenizi ve blob, kuyruk ve tablo depolaması için günlükleri toplamanıza olanak sağlar.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/03/2017
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 4ad9f13bcdf36b67400adb62d58ee260ff256bb3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80637138"
---
# <a name="storage-analytics"></a>Depolama Analizi

Azure Depolama Analizi günlüğe kaydetme gerçekleştirir ve depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızla ilgili sorunları tanılayabilirsiniz.

Depolama Analizi kullanmak için, izlemek istediğiniz her hizmet için tek tek etkinleştirmeniz gerekir. [Azure Portal](https://portal.azure.com)etkinleştirebilirsiniz. Ayrıntılar için bkz. [Azure Portal bir depolama hesabını izleme](storage-monitor-storage-account.md). Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi programlı bir şekilde etkinleştirebilirsiniz. [BLOB hizmeti özelliklerini ayarlama](/rest/api/storageservices/set-blob-service-properties), [kuyruk hizmeti özelliklerini ayarlama](/rest/api/storageservices/set-queue-service-properties), [Tablo hizmeti özelliklerini](/rest/api/storageservices/set-table-service-properties)ayarlama ve [dosya hizmeti özellikleri](/rest/api/storageservices/Get-File-Service-Properties) işlemlerini her hizmet için depolama Analizi etkinleştirecek şekilde ayarlama.

Toplanan veriler, blob hizmeti ve tablo hizmeti API 'Leri kullanılarak erişilebilen, iyi bilinen bir blob 'da (günlük için) ve iyi bilinen tablolarda (ölçümler için) depolanır.

Depolama Analizi, depolama hesabınız için toplam sınırdan bağımsız olan depolanan verilerin miktarına göre 20 TB 'lik bir sınıra sahiptir. Depolama hesabı sınırları hakkında daha fazla bilgi için bkz. [Standart depolama hesapları Için ölçeklenebilirlik ve performans hedefleri](scalability-targets-standard-account.md).

Azure depolama ile ilgili sorunları tanımlamak, tanılamak ve sorunlarını gidermek için Depolama Analizi ve diğer araçları kullanma hakkında ayrıntılı bir kılavuz için bkz. [izleme, tanılama ve sorun giderme Microsoft Azure depolama](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="billing-for-storage-analytics"></a>Depolama Analizi için faturalandırma

Tüm ölçüm verileri bir depolama hesabının Hizmetleri tarafından yazılır. Sonuç olarak, Depolama Analizi tarafından gerçekleştirilen her yazma işlemi faturalandırılabilir. Ayrıca, ölçüm verileri tarafından kullanılan depolama miktarı da faturalandırılabilir.

Depolama Analizi tarafından gerçekleştirilen aşağıdaki eylemler faturalandırılabilir:

* Günlüğe kaydetme için blob oluşturma istekleri.
* Ölçümler için tablo varlıkları oluşturma istekleri.

Bir veri saklama ilkesi yapılandırdıysanız, Depolama Analizi eski günlük kaydı ve ölçüm verilerini sildiğinde silme işlemleri için ücretlendirilirsiniz. Ancak, bir istemciden silme işlemleri faturalandırılabilir. Bekletme ilkeleri hakkında daha fazla bilgi için bkz. [depolama Analizi veri bekletme Ilkesi ayarlama](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Faturalandırılabilir istekleri anlama

Bir hesabın depolama hizmetine yapılan her istek faturalandırılabilir veya faturalandırılmamış olur. Depolama Analizi, isteğin nasıl işlendiğini belirten bir durum iletisi de dahil olmak üzere, bir hizmete yapılan her bir isteği günlüğe kaydeder. Benzer şekilde, Depolama Analizi, belirli durum iletilerinin yüzdeleri ve sayısı dahil olmak üzere hem hizmet hem de bu hizmetin API işlemleri için ölçümleri depolar. Bu özellikler birlikte, faturalandırılabilir isteklerinizi çözümlemenize, uygulamanızda geliştirmeler yapmanıza ve hizmetlerinize yönelik isteklerle ilgili sorunları tanılamanıza yardımcı olabilir. Faturalandırma hakkında daha fazla bilgi için bkz. [Azure depolama faturalandırma-bant genişliği, işlemler ve kapasiteyi anlama](https://docs.microsoft.com/archive/blogs/windowsazurestorage/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity).

Depolama Analizi verilere baktığınızda, hangi isteklerin faturalandırılabilir olduğunu belirlemek için [depolama Analizi günlüğe kaydedilen işlemler ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) konusundaki tabloları kullanabilirsiniz. Ardından, belirli bir istek için ücretlendirildiğini görmek üzere günlüklerinizi ve ölçüm verilerinizi durum iletileriyle karşılaştırabilirsiniz. Bir depolama hizmeti veya tek bir API işlemi için kullanılabilirliği araştırmak üzere önceki konudaki tabloları da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure portalında depolama hesabını izleme](storage-monitor-storage-account.md)
* [Depolama Analizi ölçümleri](storage-analytics-metrics.md)
* [Depolama Analizi Günlüğü](storage-analytics-logging.md)
