---
title: Yapılandırma hata kodları-Azure Stream Analytics
description: Yapılandırma hata kodlarıyla ilgili Azure Stream Analytics sorunları giderin.
ms.author: mamccrea
author: mamccrea
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: de8eefd099f3691ae5e5eb5234ae8f76015dd68f
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86041124"
---
# <a name="azure-stream-analytics-configuration-error-codes"></a>Azure Stream Analytics yapılandırma hata kodları

Azure Stream Analytics işinizin beklenmedik davranışlarından hata ayıklamaya yardımcı olması için etkinlik günlüklerini ve kaynak günlüklerini kullanabilirsiniz. Bu makalede, her yapılandırma hata kodu için açıklama listelenir. Yapılandırma hataları, iş yapılandırmanızla veya giriş ve çıkış konfigürasyonlarıyla ilgilidir.

## <a name="eventhubunauthorizedaccess"></a>EventHubUnauthorizedAccess

* **Neden**: Olay Hub 'ı *yetkisiz erişim* hatası oluşturdu.

## <a name="eventhubreceiverepochconflict"></a>EventHubReceiverEpochConflict

* **Neden**: farklı dönem değerleri olan birden fazla olay hub alıcısı var.
* **Öneri**: Stream Analytics işiniz çalışırken *Service Bus Gezgini* 'Nin veya bir *eventprocessorhost* uygulamasının bağlı olmadığından emin olun.

## <a name="eventhubreceiverquotaexceeded"></a>EventHubReceiverQuotaExceeded

* **Neden**: bir tüketici grubunda bölüm başına izin verilen en fazla alıcı sayısına ulaşıldığından Stream Analytics bölüme bağlanamaz.
* **Öneri**: diğer Stream Analytics işlerinin veya Service Bus gezgin 'in aynı tüketici grubunu kullandığından emin olun.

## <a name="eventhuboutputthrottled"></a>Eventhui Putkısıtlanıyor

* **Neden**: azaltma nedeniyle verileri olay hub 'ına yazarken bir hata oluştu.
* **Öneri**: Bu işlem düzenli olarak oluşursa, üretilen işi yükseltin.

## <a name="eventhuboutputinvalidconnectionconfig"></a>Eventhui Putınvalidconnectionconfig

* **Neden**: belirtilen bağlantı yapılandırması yanlış.
* **Öneri**: yapılandırmayı düzeltin ve işi yeniden başlatın.

## <a name="eventhuboutputinvalidhostname"></a>Eventhui Putınvalidhostname

* **Neden**: Olay Hub 'ı ana bilgisayarına ulaşılamıyor.
* **Öneri**: sağlanan ana bilgisayar adının doğru olduğundan emin olun.

## <a name="eventhuboutputunexpectedpartitioncount"></a>EventHubOutputUnexpectedPartitionCount

* **Neden**: eventhub gönderici beklenmeyen bir EventHub bölüm sayısıyla karşılaştı.
* **Öneri**: EventHub bölüm sayısı değiştiyse Stream Analytics işinizi yeniden başlatın.

## <a name="cosmosdbpartitionkeynotfound"></a>CosmosDBPartitionKeyNotFound

* **Neden**: Stream Analytics veritabanında belirli bir Cosmos DB koleksiyonunun bölüm anahtarını bulamadı.
* **Öneri**: Cosmos DB koleksiyon için belirtilen geçerli bir bölüm anahtarı olduğundan emin olun.

## <a name="cosmosdbinvalidpartitionkeycolumn"></a>CosmosDBInvalidPartitionKeyColumn

* **Neden**: bir bölüm anahtarı bir yaprak düğüm veya en üst düzey olmadığında oluşturulur.

## <a name="cosmosdbinvalididcolumn"></a>CosmosDBInvalidIdColumn

* **Neden**: \[ birincil anahtar özelliği olarak farklı bir sütun seçilirse, sorgu çıktısı sütun kimliğini içeremez].

## <a name="cosmosdbdatabasenotfound"></a>CosmosDBDatabaseNotFound

* **Neden**: Stream Analytics cosmosdb veritabanını bulamıyor.

## <a name="cosmosdbcollectionnotfound"></a>CosmosDBCollectionNotFound

* **Neden**: Stream Analytics bir veritabanında belirli bir Cosmos DB koleksiyonu bulamıyor.

## <a name="cosmosdboutputwritethrottling"></a>Cosmosdi Putwriteazaltma

* **Neden**: Cosmos DB tarafından azaltma nedeniyle veri yazılırken bir hata oluştu.
* **Öneri**: koleksiyon performans katmanını yükseltin ve veritabanınızın performansını ayarlayın.

## <a name="sqldatabaseconnectionstringerror"></a>SQLDatabaseConnectionStringError

* **Neden**: Stream Analytics işi bir kimlik doğrulama hatasıyla karşılaştı.
* **Öneri**: SQL veritabanı bağlantı dizesinin doğru olduğundan emin olun.

## <a name="sqldatabasemanagedidentityauthenticationerror"></a>Sqldatabasemanagedıdentityauthenticationerror

* **Neden**: Stream Analytics işi bir kimlik doğrulama hatasıyla karşılaştı. 
* **Öneri**: hesap adının düzgün yapılandırıldığından ve Iş tarafından YÖNETILEN kimliğin SQL veritabanına erişimi olduğundan emin olun.

## <a name="sqldatabaseoutputnotableerror"></a>SQLDatabaseOutputNoTableError

* **Neden**: Stream Analytics belirli bir tablo için şema bilgilerini bulamıyor.

## <a name="sqldwoutputinvalidserviceedition"></a>Sqldwoutputınvalidserviceedition

* **Neden**: SQL veritabanı desteklenmiyor.
* **Öneri**: SYNAPSE SQL havuzunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Giriş bağlantısı sorunlarını giderme](stream-analytics-troubleshoot-input.md)
* [Azure Stream Analytics çıkışları sorunlarını giderme](stream-analytics-troubleshoot-output.md)
* [Azure Stream Analytics sorgularının sorunlarını giderme](stream-analytics-troubleshoot-query.md)
* [Kaynak günlüklerini kullanarak Azure Stream Analytics sorunlarını giderme](stream-analytics-job-diagnostic-logs.md)
* [Azure Stream Analytics veri hataları](data-errors.md)
