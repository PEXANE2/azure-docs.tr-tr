---
title: Azure Veri Gezgini için veri alma özellikleri
description: Azure Veri Gezgini tarafından desteklenen çeşitli veri alma özellikleri hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: 48bce1bf03a0a4c8d81fff7ae54e0b22261b70f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80109582"
---
# <a name="azure-data-explorer-data-ingestion-properties"></a>Azure Veri Gezgini veri alma özellikleri 

Veri alma işlemi, verilerin bir tabloya eklendiği ve Azure Veri Gezgini'nde sorgu için kullanılabilir hale getirildiği işlemdir. Anahtar kelimeden sonra yutma `with` komutuna özellikler eklersiniz.

## <a name="ingestion-properties"></a>Yutma özellikleri

Aşağıdaki tablo, Azure Veri Gezgini tarafından desteklenen özellikleri listeler, açıklar ve örnekler sağlar: 

|Özellik              |Açıklama                                              |Örnek                                             |
|----------------------|---------------------------------------------------------|----------------------------------------------------|
|`ingestionMapping`    |Kaynak dosyadaki verilerin tablodaki gerçek sütunlara nasıl eşleştirilecek olduğunu gösteren bir dize değeri. `format` Değeri ilgili eşleme türüyle tanımlayın. [Bkz. veri eşlemeleri.](/azure/kusto/management/mappings)|`with (format="json", ingestionMapping = "[{\"column\":\"rownumber\", \"Properties\":{\"Path\":\"$.RowNumber\"}}, {\"column\":\"rowguid\", \"Properties\":{\"Path\":\"$.RowGuid\"}}]")`<br>(amortismana uğradı: `avroMapping` `csvMapping`, `jsonMapping`, ) |
|`ingestionMappingReference`|Kaynak dosyadaki verilerin tablodaki gerçek sütunlara nasıl eşlenecek olduğunu gösteren bir dize değeri, adlandırılmış eşleme ilkesi nesnesi kullanarak. `format` Değeri ilgili eşleme türüyle tanımlayın. [Bkz. veri eşlemeleri.](/azure/kusto/management/mappings)|`with (format="csv", ingestionMappingReference = "Mapping1")`<br>(amortismana uğradı: `avroMappingReference` `csvMappingReference`, `jsonMappingReference`, )|
|`creationTime` |Yutulan veri kapsamlarının oluşturulması sırasında kullanılacak tarih saati değeri (ISO8601 dizesi olarak biçimlendirilmiş). Belirtilmemişse, geçerli`now()`değer ( ) kullanılır. Eski verileri sindirirken varsayılanı geçersiz kılmak yararlıdır, böylece bekletme ilkesi doğru şekilde uygulanır.|`with (creationTime="2017-02-13T11:09:36.7992775Z")`|
|`extend_schema`|Boolean değeri, belirtildiği takdirde, komutu tablonun şemasını genişletmesini emreder `false`(varsayılan olarak). Bu seçenek yalnızca `.append` komutlar ve `.set-or-append` komutlar için geçerlidir. Yalnızca izin verilen şema uzantıları, sonunda tabloya ek sütunlar eklenmiştir.|Orijinal tablo şeması ise, `(a:string, b:int)`geçerli bir şema uzantısı `(a:string, b:int, c:datetime, d:string)`olurdu, ancak `(a:string, c:datetime)` geçerli olmaz|
|`folder` |Sorgudan gelen komutları [yutmak](/azure/kusto/management/data-ingestion/ingest-from-query) için, tabloya atayabilmek için klasör. Tablo zaten varsa, bu özellik tablonun klasörünü geçersiz kılar.|`with (folder="Tables/Temporary")`|
|`format` |Veri biçimi (bkz. [desteklenen veri biçimleri).](ingestion-supported-formats.md)|`with (format="csv")`|
|`ingestIfNotExists`|Tabloda zaten aynı değere sahip bir etiketle etiketlenmiş veriler varsa, alımın başarılı olmasını engelleyen bir `ingest-by:` dize değeri. Bu idempotent veri yutulması sağlar. Daha fazla bilgi için [bkz: etiketleri](/azure/kusto/management/extents-overview#ingest-by-extent-tags).|Özellikler, `with (ingestIfNotExists='["Part0001"]', tags='["ingest-by:Part0001"]')` etiketiçeren `ingest-by:Part0001` veriler zaten varsa, geçerli alımIşlemini tamamlamadığını gösterir. Zaten yoksa, bu yeni yutma bu etiket kümesine sahip olmalıdır (gelecekteki bir yutma işlemi aynı verileri yeniden yutmaya çalışırsa.)|
|`ignoreFirstRecord` |Boolean değeri, ayarlanırsa, `true`yutma her dosyanın ilk kaydıyoksaymak gerektiğini gösterir. Dosyadaki ilk kayıt `CSV`sütun adlarıysa, bu özellik dosyalardaki dosyalar ve benzer biçimler için yararlıdır. Varsayılan olarak, `false` kabul edilir.|`with (ignoreFirstRecord=false)`|
|`persistDetails` |Bir Boolean değeri, belirtildiği takdirde, komutu ayrıntılı sonuçları (başarılı olsa bile) böylece [.show işlem ayrıntıları](/azure/kusto/management/operations#show-operation-details) komutu onları alabilir kalıcı gerektiğini gösterir. Varsayılan `false`değer.|`with (persistDetails=true)`|
|`policy_ingestiontime`|Bu komut tarafından oluşturulan bir tabloda [Yutma Zamanı İlkesi](/azure/kusto/management/ingestiontimepolicy) etkinleştirilip etkinleştirmeyeceğini açıklayan boolean değeri. Varsayılan değer: `true`.|`with (policy_ingestiontime=false)`|
|`recreate_schema` |Bir Boolean değeri, belirtildiği takdirde, komut tablonun şema yeniden olup olmadığını açıklar. Bu özellik yalnızca `.set-or-replace` komut için geçerlidir. Her ikisi de ayarlanmışsa, `extend_schema` bu özellik özellikten önce gelir.|`with (recreate_schema=true)`|
|`tags`|JSON dizesi olarak biçimlendirilmiş, yutulan verilerle ilişkilendirilen [etiketlerin](/azure/kusto/management/extents-overview#extent-tagging) listesi |`with (tags="['Tag1', 'Tag2']")`|
|`validationPolicy`|Yutma sırasında hangi doğrulamaların çalıştırılacağı belirtilirse JSON dizesi. Farklı seçeneklerin açıklaması için [Bkz. Veri alımı.](/azure/kusto/management/data-ingestion/)| `with (validationPolicy='{"ValidationOptions":1, "ValidationImplications":1}')`(bu aslında varsayılan ilkedir)|
|`zipPattern`|ZIP arşivi olan depolama dan veri sindirirken bu özelliği kullanın. Bu, ZIP arşivindeki hangi dosyaların yutulmasını seçerken kullanılacak normal ifadeyi gösteren bir dize değeridir.  Arşivdeki diğer tüm dosyalar yoksayılır.|`with (zipPattern="*.csv")`|

## <a name="next-steps"></a>Sonraki adımlar

* [Veri alma](/azure/data-explorer/ingest-data-overview) hakkında daha fazla bilgi edinin
* [Desteklenen veri biçimleri](ingestion-supported-formats.md) hakkında daha fazla bilgi edinin
