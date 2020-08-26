---
title: Azure Stream Analytics çıkış çıkışları
description: Bu makalede Azure Stream Analytics için kullanılabilen veri çıkışı seçenekleri açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 92cc742fce101b67bfb359381b4eabf9cf2a404e
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88869584"
---
# <a name="outputs-from-azure-stream-analytics"></a>Azure Stream Analytics çıkış çıkışları

Azure Stream Analytics iş bir giriş, sorgu ve bir çıktıdan oluşur. Dönüştürülmüş verileri gönderebilmeniz için birkaç çıktı türü vardır. Bu makalede desteklenen Stream Analytics çıkışları listelenmektedir. Stream Analytics sorgunuzu tasarlarken, [from yan tümcesini](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics)kullanarak çıkışın adına bakın. Sorguya birden çok yan tümce ekleyerek, iş başına tek bir çıktı veya akış işi başına birden fazla çıkış (ihtiyacınız varsa) kullanabilirsiniz.

Stream Analytics iş çıktıları oluşturmak, düzenlemek ve test etmek için [Azure Portal](stream-analytics-quick-create-portal.md#configure-job-output), [Azure POWERSHELL](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output)ve [Visual Studio 'yu](stream-analytics-quick-create-vs.md)kullanabilirsiniz.

Bazı çıkış türleri [bölümleme](#partitioning)destekler ve [Çıkış toplu işlem boyutları](#output-batch-size) , üretilen işi iyileştirmek için farklılık gösterir. Aşağıdaki tabloda her çıkış türü için desteklenen özellikler gösterilmektedir:

| Çıkış türü | Bölümleme | Güvenlik | 
|-------------|--------------|----------|
|[Azure Data Lake Storage Gen 1](azure-data-lake-storage-gen1-output.md)|Azure Active Directory Kullanıcı </br> MSI|
|[Azure SQL Veritabanı](sql-database-output.md)|Evet, etkin olmalıdır.|SQL kullanıcı kimlik doğrulaması </br> MSI (Önizleme)|
|[Azure SYNAPSE Analytics (Önizleme)](azure-synapse-analytics-output.md)|Hayır|SQL kullanıcı kimlik doğrulaması|
|[BLOB depolama ve Azure Data Lake Gen 2](blob-storage-azure-data-lake-gen2-output.md)|Yes|MSI </br> Erişim anahtarı|
|[Azure Event Hubs](event-hubs-output.md)|Yes|Erişim anahtarı|
|[Power BI](power-bi-output.md)|Hayır|Azure Active Directory Kullanıcı </br> MSI|
|[Azure Tablo Depolama](table-storage-output.md)|Yes|Hesap anahtarı|
|[Azure Service Bus kuyrukları](service-bus-queues-output.md)|Yes|Erişim anahtarı|
|[Azure Service Bus konuları](service-bus-topics-output.md)|Yes|Erişim anahtarı|
|[Azure Cosmos DB](azure-cosmos-db-output.md)|Yes|Erişim anahtarı|
|[Azure İşlevleri](azure-functions-output.md)|Yes|Erişim anahtarı|

## <a name="partitioning"></a>Bölümleme

Stream Analytics, Power BI hariç tüm çıkışlara yönelik bölümleri destekler. Bölüm anahtarları ve çıkış yazıcısı sayısı hakkında daha fazla bilgi için ilgilendiğiniz belirli çıktı türü için makaleye bakın. Tüm çıkış makaleleri önceki bölüme bağlanır.  

Çıktı yazıcılarının sayısı `INTO <partition count>` , sorgunuzda bir (bkz.) yan tümcesi [INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)kullanılarak denetlenebilir ve bu, istenen iş topolojisini elde etmek için yararlı olabilir. Çıkış bağdaştırıcınız bölümlendirilmemişse, bir giriş bölümünde verilerin bulunmaması, geç varış süresinin sonuna kadar gecikmeye neden olur. Bu gibi durumlarda, çıkış tek bir yazıcı ile birleştirilir ve bu da işlem hattınızda performans sorunlarına neden olabilir. Geç alma ilkesi hakkında daha fazla bilgi için bkz. [Azure Stream Analytics olay sırası konuları](stream-analytics-out-of-order-and-late-events.md).

## <a name="output-batch-size"></a>Çıkış toplu iş boyutu

Tüm çıktılar toplu işlemeyi destekler, ancak yalnızca bazı toplu iş boyutunu açıkça destekler. Azure Stream Analytics, olayları işlemek ve çıkışlara yazmak için değişken boyutlu toplu işler kullanır. Genellikle Stream Analytics altyapısı tek seferde bir ileti yazmaz ve verimlilik için toplu işler kullanır. Hem gelen hem de giden olayların oranı yüksek olduğunda Stream Analytics daha büyük toplu işler kullanır. Çıkış hızı düşük olduğunda, gecikme süresini düşük tutmak için daha küçük toplu işler kullanır.

## <a name="parquet-output-batching-window-properties"></a>Parquet çıkış toplu işleme penceresi özellikleri

Azure Resource Manager şablon dağıtımı veya REST API kullanılırken, iki toplu işlem penceresi özelliği şunlardır:

1. *timeWindow*

   Toplu iş başına en fazla bekleme süresi. Değer bir TimeSpan dizesi olmalıdır. Örneğin, iki dakika için "00:02:00". Bu süreden sonra, en düşük satır gereksinimi karşılanmasa bile toplu iş çıktıya yazılır. Varsayılan değer 1 dakikadır ve izin verilen en fazla 2 saattir. Blob çıktınızdan yol deseninin sıklığı varsa, bekleme süresi bölüm saat aralığından daha yüksek olamaz.

2. *Boyut penceresi*

   Toplu iş başına en az satır sayısı. Parquet için her Batch yeni bir dosya oluşturur. Geçerli varsayılan değer 2.000 satırdır ve izin verilen en fazla 10.000 satır olur.

Bu toplu işlem penceresi özellikleri yalnızca API sürüm **2017-04-01-önizleme**tarafından desteklenir. REST API çağrısının JSON yükünün bir örneği aşağıda verilmiştir:

```json
"type": "stream",
      "serialization": {
        "type": "Parquet",
        "properties": {}
      },
      "timeWindow": "00:02:00",
      "sizeWindow": "2000",
      "datasource": {
        "type": "Microsoft.Storage/Blob",
        "properties": {
          "storageAccounts" : [
          {
            "accountName": "{accountName}",
            "accountKey": "{accountKey}",
          }
          ],
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>
> [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
