---
title: Azure kuyruk depolama izleme verileri başvurusu
description: Azure kuyruğu depolamadaki verileri izlemek için günlük ve ölçüm başvurusu.
author: normesta
services: azure-monitor
ms.author: normesta
ms.date: 10/02/2020
ms.topic: reference
ms.service: azure-monitor
ms.subservice: logs
ms.custom: monitoring
ms.openlocfilehash: 95f20737b044140fe12ea939e71cd2397cb4826d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100576685"
---
# <a name="azure-queue-storage-monitoring-data-reference"></a>Azure kuyruk depolama izleme verileri başvurusu

Azure depolama için izleme verilerini toplama ve çözümleme hakkında ayrıntılı bilgi için bkz. [Azure Storage 'ı izleme](monitor-queue-storage.md) .

## <a name="metrics"></a>Ölçümler

Aşağıdaki tablolarda, Azure depolama için toplanan platform ölçümleri listelenmektedir.

### <a name="capacity-metrics"></a>Kapasite ölçümleri

Kapasite ölçümleri değerleri günlük olarak (24 saate kadar) yenilenir. Zaman dilimi, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm kapasite ölçümleri için desteklenen zaman dilimi bir saattir (PT1H).

Azure depolama, Azure Izleyici 'de aşağıdaki kapasite ölçümlerini sağlar.

#### <a name="account-level-capacity-metrics"></a>Hesap düzeyi kapasite ölçümleri

[!INCLUDE [Account-level capacity metrics](../../../includes/azure-storage-account-capacity-metrics.md)]

#### <a name="queue-storage-metrics"></a>Kuyruk depolama ölçümleri

Bu tabloda [kuyruk depolama ölçümleri](../../azure-monitor/essentials/metrics-supported.md#microsoftstoragestorageaccountsqueueservices)gösterilmektedir.

| Metric | Açıklama |
| ------------------- | ----------------- |
| **QueueCapacity** | Depolama hesabı tarafından kullanılan kuyruk depolama miktarı. <br><br> Birim `Bytes` <br> Toplama türü: `Average` <br> Değer örneği: `1024` |
| **QueueCount** | Depolama hesabındaki sıraların sayısı. <br><br> Birim `Count` <br> Toplama türü: `Average` <br> Değer örneği: `1024` |
| **QueueMessageCount** | Depolama hesabındaki sıra iletilerinin yaklaşık sayısı. <br><br> Birim `Count` <br> Toplama türü: `Average` <br> Değer örneği: `1024` |

### <a name="transaction-metrics"></a>İşlem ölçümleri

İşlem ölçümleri, Azure depolama 'daki bir depolama hesabına yönelik her istek için Azure Izleyici 'ye dağıtılır. Depolama hesabınızda hiçbir etkinlik olmaması durumunda, dönem içinde işlem ölçümlerinde hiç veri olmayacaktır. Tüm işlem ölçümleri hem hesap hem de kuyruk depolama hizmeti düzeyinde kullanılabilir. Zaman dilimi, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm işlem ölçümleri için desteklenen saat graıns PT1H ve PT1M.

[!INCLUDE [Transaction metrics](../../../includes/azure-storage-account-transaction-metrics.md)]

<a id="metrics-dimensions"></a>

## <a name="metrics-dimensions"></a>Ölçüm boyutları

Azure depolama, Azure Izleyici 'de ölçümler için aşağıdaki boyutları destekler.

[!INCLUDE [Metrics dimensions](../../../includes/azure-storage-account-metrics-dimensions.md)]

## <a name="resource-logs-preview"></a>Kaynak günlükleri (Önizleme)

> [!NOTE]
> Azure Izleyici 'de Azure depolama günlükleri genel önizlemede bulunur ve tüm genel bulut bölgelerinde önizleme testi için kullanılabilir. Bu önizleme, genel amaçlı v1 ve genel amaçlı v2 depolama hesaplarında blob 'lar (Azure Data Lake Storage 2.), dosyalar, kuyruklar, tablolar, Premium Depolama hesapları için Günlükler sunar. Klasik depolama hesapları desteklenmez.

Aşağıdaki tabloda Azure Izleyici günlüklerinde veya Azure Storage 'da toplandıklarında Azure depolama kaynak günlüklerinin özellikleri listelenmektedir. Özellikler işlemi, hizmeti ve işlemi gerçekleştirmek için kullanılan yetkilendirme türünü anlatmaktadır.

### <a name="fields-that-describe-the-operation"></a>İşlemi tanımlayan alanlar

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-operation.md)]

### <a name="fields-that-describe-how-the-operation-was-authenticated"></a>İşlemin nasıl doğrulandığını tanımlayan alanlar

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-authentication.md)]

### <a name="fields-that-describe-the-service"></a>Hizmeti tanımlayan alanlar

[!INCLUDE [Account level capacity metrics](../../../includes/azure-storage-logs-properties-service.md)]

## <a name="see-also"></a>Ayrıca bkz.

- Bkz. Azure kuyruk depolamayı izleme açıklaması için bkz. [Azure kuyruk depolamayı izleme](monitor-queue-storage.md) .
- Azure kaynaklarını izleme hakkında ayrıntılı bilgi için bkz. Azure [izleyici ile Azure kaynaklarını izleme](../../azure-monitor/essentials/monitor-azure-resource.md) .
