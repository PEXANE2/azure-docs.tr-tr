---
title: Azure Veri Gezgini'nde yinelenen verileri işleme
description: Bu konu, Azure Veri Gezgini kullanırken yinelenen verilerle başa çıkmak için çeşitli yaklaşımlar gösterir.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/19/2018
ms.openlocfilehash: 60ec2b86e0205060f907f1fe39d084dca3aac1cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68608236"
---
# <a name="handle-duplicate-data-in-azure-data-explorer"></a>Azure Veri Gezgini'nde yinelenen verileri işleme

Bulut'a veri gönderen aygıtlar verilerin yerel önbelleğini tutar. Veri boyutuna bağlı olarak, yerel önbellek verileri günlerdir veya aylarca saklıyor olabilir. Analitik veritabanlarınızı önbelleğe alınan verileri yeniden gönderen ve analitik veritabanında veri çoğaltmasına neden olan arızalı aygıtlardan korumak istiyorsunuz. Bu konu, bu tür senaryolar için yinelenen verileri işlemek için en iyi uygulamaları özetler.

Veri çoğaltma için en iyi çözüm çoğaltmayı önlemektir. Mümkünse, veri ardışık düzeneği boyunca veri hareketi ile ilişkili maliyetlerden tasarruf sağlayan ve sisteme alınan yinelenen verilerle başa çıkma kaynakları harcamasını önleyen veri ardışık düzende sorunu daha erken giderin. Ancak, kaynak sistemin değiştirilebildiği durumlarda, bu senaryoyla başa çıkmanın çeşitli yolları vardır.

## <a name="understand-the-impact-of-duplicate-data"></a>Yinelenen verilerin etkisini anlama

Yinelenen verilerin yüzdesini izleyin. Yinelenen verilerin yüzdesi keşfedildikten sonra, sorunun kapsamını ve iş etkisinin kapsamını analiz edebilir ve uygun çözümü seçebilirsiniz.

Yinelenen kayıtların yüzdesini belirlemek için örnek sorgu:

```kusto
let _sample = 0.01; // 1% sampling
let _data =
DeviceEventsAll
| where EventDateTime between (datetime('10-01-2018 10:00') .. datetime('10-10-2018 10:00'));
let _totalRecords = toscalar(_data | count);
_data
| where rand()<= _sample
| summarize recordsCount=count() by hash(DeviceId) + hash(EventId) + hash(StationId)  // Use all dimensions that make row unique. Combining hashes can be improved
| summarize duplicateRecords=countif(recordsCount  > 1)
| extend duplicate_percentage = (duplicateRecords / _sample) / _totalRecords  
```

## <a name="solutions-for-handling-duplicate-data"></a>Yinelenen verileri işlemek için çözümler

### <a name="solution-1-dont-remove-duplicate-data"></a>Çözüm #1: Yinelenen verileri kaldırma

İş gereksinimlerinizi ve yinelenen verilerin toleransını anlayın. Bazı veri kümeleri, yinelenen verilerin belirli bir yüzdesi ile yönetebilirsiniz. Yinelenen verilerin büyük bir etkisi yoksa, veri varlığını yok sayabilirsiniz. Yinelenen verileri kaldırmama avantajı, yutma işlemi veya sorgu performansında ek ek ek yükü yoktur.

### <a name="solution-2-handle-duplicate-rows-during-query"></a>Çözüm #2: Sorgu sırasında yinelenen satırları işleme

Başka bir seçenek sorgu sırasında verilerde yinelenen satırları filtrelemektir. Toplanan [`arg_max()`](/azure/kusto/query/arg-max-aggfunction) işlev, yinelenen kayıtları filtrelemek ve zaman damgasını (veya başka bir sütunu) temel alan son kaydı döndürmek için kullanılabilir. Bu yöntemi kullanmanın avantajı, sorgu süresi içinde yineleme oluştuğundan daha hızlı yutulmasıdır. Buna ek olarak, tüm kayıtlar (yinelenenler dahil) denetim ve sorun giderme için kullanılabilir. İşlevkullanmanın `arg_max` dezavantajı, veriler her sorgulanında ek sorgu süresi ve CPU'ya yüklenmesidir. Sorgulanan verilerin miktarına bağlı olarak, bu çözüm işlevsel olmayan veya bellek tüketen olabilir ve diğer seçeneklere geçmeyi gerektirir.

Aşağıdaki örnekte, benzersiz kayıtları belirleyen bir sütun kümesi için kaydedilen son kaydı sorgularız:

```kusto
DeviceEventsAll
| where EventDateTime > ago(90d)
| summarize hint.strategy=shuffle arg_max(EventDateTime, *) by DeviceId, EventId, StationId
```

Bu sorgu, doğrudan tabloyu sorgulamak yerine bir işlevin içine de yerleştirilebilir:

```kusto
.create function DeviceEventsView
{
DeviceEventsAll
| where EventDateTime > ago(90d)
| summarize arg_max(EventDateTime, *) by DeviceId, EventId, StationId
}
```

### <a name="solution-3-filter-duplicates-during-the-ingestion-process"></a>Çözüm #3: Yutma işlemi sırasında filtre yinelemeleri

Başka bir çözüm, yutma işlemi sırasında yinelenenleri filtrelemektir. Sistem, Kusto tablolarına giriş sırasında yinelenen verileri yok sayar. Veriler bir hazırlama tablosuna yutularak yinelenen satırları kaldırdıktan sonra başka bir tabloya kopyalanır. Bu çözümün avantajı, sorgu performansının önceki çözüme göre önemli ölçüde iyileşmesidir. Dezavantajları artan yutma süresi ve ek veri depolama maliyetleri içerir. Ayrıca, bu çözüm yalnızca yinelemeler aynı anda yutulmazsa çalışır. Yinelenen kayıtları içeren birden çok eşzamanlı yutma varsa, çoğaltma işlemi tabloda varolan eşleşen kayıtları bulamayacaktır beri tüm yutulabilir.    

Aşağıdaki örnekte bu yöntem gösteriş verilmiştir:

1. Aynı şema içeren başka bir tablo oluşturun:

    ```kusto
    .create table DeviceEventsUnique (EventDateTime: datetime, DeviceId: int, EventId: int, StationId: int)
    ```

1. Yeni kayıtları daha önce yutulanlarla birleştirmeyi anti-alarak yinelenen kayıtları filtrelemek için bir işlev oluşturun.

    ```kusto
    .create function RemoveDuplicateDeviceEvents()
    {
    DeviceEventsAll
    | join hint.strategy=broadcast kind = anti
        (
        DeviceEventsUnique
        | where EventDateTime > ago(7d)   // filter the data for certain time frame
        | limit 1000000   //set some limitations (few million records) to avoid choking-up the system during outage recovery

        ) on DeviceId, EventId, StationId
    }
    ```

    > [!NOTE]
    > Birleştirmeler CPU'ya bağlı işlemlerdir ve sisteme ek bir yük ekleyin.

1. [Güncelleştirme İlkesi'ni](/azure/kusto/management/update-policy) tabloda `DeviceEventsUnique` ayarlayın. Yeni veriler tabloya `DeviceEventsAll` girdiğinde güncelleştirme ilkesi etkinleştirilir. Kusto motoru, yeni [boyutlar](/azure/kusto/management/extents-overview) oluşturuldukça işlevi otomatik olarak çalıştıracaktır. İşleme, yeni oluşturulan verilere göre kapsamdadır. Aşağıdaki komut, güncelleştirme ilkesini`DeviceEventsAll`oluşturmak için`DeviceEventsUnique`kaynak tabloyu ( ), hedef tabloyu ( ) ve işlevi `RemoveDuplicatesDeviceEvents` bir araya getirir.

    ```kusto
    .alter table DeviceEventsUnique policy update
    @'[{"IsEnabled": true, "Source": "DeviceEventsAll", "Query": "RemoveDuplicateDeviceEvents()", "IsTransactional": true, "PropagateIngestionProperties": true}]'
    ```

    > [!NOTE]
    > Güncelleştirme ilkesi, veriler yutma sırasında filtrelendikten sonra iki kez `DeviceEventsAll` (tabloya ve `DeviceEventsUnique` tabloya) sindirildiğinden, alım süresini uzatır.

1. (İsteğe bağlı) Verilerin kopyalarını depolamaktan `DeviceEventsAll` kaçınmak için tabloda daha düşük bir veri saklama ayarlayın. Veri hacmine ve sorun giderme için verileri tutmak istediğiniz süreye bağlı olarak gün sayısını seçin. Veriler depolamaya `0d` yüklenmediği için COGS'i kaydetmek ve performansı artırmak için gün bekletme olarak ayarlayabilirsiniz.

    ```kusto
    .alter-merge table DeviceEventsAll policy retention softdelete = 1d
    ```

## <a name="summary"></a>Özet

Veri yinelemesi birden çok şekilde işlenebilir. İşletmeniz için doğru yöntemi belirlemek için fiyat ve performansı göz önünde bulundurarak seçenekleri dikkatle değerlendirin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Veri Gezgini için sorgu yazma](write-queries.md)
