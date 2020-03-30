---
title: Metin verilerini Azure Monitor günlüklerinde ayrıştAma | Microsoft Dokümanlar
description: Azure Monitor kayıtlarında günlük verilerini ayrıştırma için farklı seçenekleri açıklar, veriler yutulduğunda ve sorguda alındığı zaman, her biri için göreli avantajları karşılaştırarak.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/04/2018
ms.openlocfilehash: d7a37d51c411488231205fd036f9a287f5206ce5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672455"
---
# <a name="parse-text-data-in-azure-monitor-logs"></a>Metin verilerini Azure Monitor günlüklerinde ayrıştın
Azure Monitor tarafından toplanan bazı günlük verileri, tek bir özellikte birden çok bilgi parçasını içerir. Bu verileri birden çok özelliğe ayrıştık, sorgularda kullanımı kolaylaştırır. Yaygın bir örnek, tek bir özellik içine birden çok değer içeren tüm günlük girişini toplayan özel bir [günlüktir.](../../log-analytics/log-analytics-data-sources-custom-logs.md) Farklı değerler için ayrı özellikler oluşturarak, her birinde arama yapabilir ve toplayabilirsiniz.

Bu makalede, veriler yutulduğunda ve sorguda alındığı zaman Azure Monitor'da günlük verilerini ayrıştırma için farklı seçenekler açıklanır ve her biri için göreli avantajları karşılaştırılır.


## <a name="parsing-methods"></a>Ayrışma yöntemleri
Verileri, verilerin toplandığı zaman veya sorgu yla verileri çözümlenirken sorgu sırasında ayrıştırabilirsiniz. Her stratejinin aşağıda açıklandığı gibi benzersiz avantajları vardır.

### <a name="parse-data-at-collection-time"></a>Toplama zamanında verileri ayrıştın
Verileri toplama zamanında ayrıştırdığınızda, tabloda yeni özellikler oluşturan [Özel Alanlar'ı](../../log-analytics/log-analytics-custom-fields.md) yapılandırın. Sorguların herhangi bir ayrıştırma mantığı içermesi ve bu özellikleri tablodaki başka bir alan olarak kullanması gerekir.

Bu yöntemin avantajları şunlardır:

- Sorguya ayrıştirma komutları eklemeniz gerekmediğinden toplanan verileri sorgulamak daha kolaydır.
- Sorgu ayrışma gerçekleştirmek gerekmez beri daha iyi sorgu performansı.
 
Bu yöntemin dezavantajları şunlardır:

- Önceden tanımlanmalıdır. Zaten toplanmış verileri içeremez.
- Ayrıştırma mantığını değiştirirseniz, yalnızca yeni veriler için geçerli olur.
- Sorgularda kullanılabilir olandan daha az ayrışma seçeneği.
- Veri toplamak için gecikme süresini artırır.
- Hataları işlemek zor olabilir.


### <a name="parse-data-at-query-time"></a>Sorgu zamanında verileri ayrıştın
Sorgu zamanında verileri ayrıştırken, verileri birden çok alana ayrıştmak için sorgunuza mantık eklersiniz. Gerçek tablonun kendisi değiştirilmemiş.

Bu yöntemin avantajları şunlardır:

- Zaten toplanmış veriler de dahil olmak üzere tüm veriler için geçerlidir.
- Mantık değişiklikleri hemen tüm verilere uygulanabilir.
- Belirli veri yapıları için önceden tanımlanmış mantık da dahil olmak üzere esnek ayrıştırma seçenekleri.
 
Bu yöntemin dezavantajları şunlardır:

- Daha karmaşık sorgular gerektirir. Bu, [bir tabloyu simüle etmek için işlevler](#use-function-to-simulate-a-table)kullanılarak azaltılabilir.
- Birden çok sorguda ayrışma mantığını çoğaltmak gerekir. Fonksiyonlar aracılığıyla bazı mantık paylaşabilirsiniz.
- Çok büyük kayıt kümelerine (milyarlarca kayıt) karşı karmaşık mantık çalıştırırken genel ek yükü oluşturabilir.

## <a name="parse-data-as-its-collected"></a>Toplanan verileri ayrıştın
Veri ayrıştırma hakkında ayrıntılar için [Azure Monitor'da özel alanlar oluşturun'a](../platform/custom-fields.md) bakın. Bu, tabloda diğer özellikler gibi sorgular tarafından kullanılabilecek özel özellikler oluşturur.

## <a name="parse-data-in-query-using-patterns"></a>Desenleri kullanarak sorgudaki verileri ayrıştın
Ayrıştırmak istediğiniz veriler kayıtlar arasında yinelenen bir desenle tanımlanabiliyorsa, belirli veri parçasını bir veya daha fazla yeni özelliğe ayıklamak için [Kusto sorgu dilindeki](/azure/kusto/query/) farklı işleçleri kullanabilirsiniz.

### <a name="simple-text-patterns"></a>Basit metin desenleri

Bir dize ifadesinden çıkarılabilen bir veya daha fazla özel özellik oluşturmak için sorgunuzdaki [ayrıştırıcı](/azure/kusto/query/parseoperator) işleci kullanın. Tanımlanacak deseni ve oluşturulacak özelliklerin adlarını belirtirsiniz. Bu özellikle _key=value'a_benzer bir forma sahip anahtar değeri dizelerine sahip veriler için yararlıdır.

Aşağıdaki biçimde veri içeren özel bir günlük düşünün.

```
Time=2018-03-10 01:34:36 Event Code=207 Status=Success Message=Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
Time=2018-03-10 01:33:33 Event Code=208 Status=Warning Message=Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
Time=2018-03-10 01:35:44 Event Code=209 Status=Success Message=Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
Time=2018-03-10 01:38:22 Event Code=302 Status=Error Message=Application could not connect to database
Time=2018-03-10 01:31:34 Event Code=303 Status=Error Message=Application lost connection to database
```

Aşağıdaki sorgu, bu verileri tek tek özelliklere ayrışdırırdı. _Proje_ ile satır yalnızca hesaplanan özellikleri değil, _RawData_, hangi özel günlük tüm giriş tutan tek özellik döndürmek için eklenir.

```Kusto
MyCustomLog_CL
| parse RawData with * "Time=" EventTime " Event Code=" Code " Status=" Status " Message=" Message
| project EventTime, Code, Status, Message
```

Aşağıda, _Azure Etkinliği_ tablosunda bir UPN'nin kullanıcı adını ortaya çıkan başka bir örnek verilmiştir.

```Kusto
AzureActivity
| parse  Caller with UPNUserPart "@" * 
| where UPNUserPart != "" //Remove non UPN callers (apps, SPNs, etc)
| distinct UPNUserPart, Caller
```


### <a name="regular-expressions"></a>Normal ifadeler
Verileriniz normal bir ifadeyle tanımlanabiliyorsa, tek tek değerleri ayıklamak için [normal ifadeleri kullanan işlevleri](/azure/kusto/query/re2) kullanabilirsiniz. Aşağıdaki örnekte, _AzureActivity_ kayıtlarından _UPN_ alanını ayırmak ve ardından farklı kullanıcıları döndürmek için [ekstre](/azure/kusto/query/extractfunction) kullanır.

```Kusto
AzureActivity
| extend UPNUserPart = extract("([a-z.]*)@", 1, Caller) 
| distinct UPNUserPart, Caller
```

Azure Monitor, büyük ölçekte verimli ayrıştırmayı etkinleştirmek için, diğer normal ifade türevleri ile benzer ancak aynı olmayan Normal İfadeler'in re2 sürümünü kullanır. Ayrıntılar için [re2 ifade sözdizimine](https://aka.ms/kql_re2syntax) bakın.


## <a name="parse-delimited-data-in-a-query"></a>Sorgudaki verileri ayrıştama
Sınırlı veri, CSV dosyasındaki virgül gibi ortak bir karaktere sahip alanları ayırır. Belirttiğiniz bir sınırlayıcı kullanarak sınırlı olmayan verileri ayrıştamak için [bölme](/azure/kusto/query/splitfunction) işlevini kullanın. Bunu, verilerdeki tüm alanları döndürmek veya çıktıya eklenecek tek tek alanları belirtmek için [genişlet](/azure/kusto/query/extendoperator) işleciyle kullanabilirsiniz.

> [!NOTE]
> Bölme dinamik bir nesne döndürür, sonuçların işleçlerde ve filtrelerde kullanılmak üzere dize gibi veri türlerine açıkça aktarılması gerekebilir.

Aşağıdaki CSV biçiminde veri içeren özel bir günlük düşünün.

```
2018-03-10 01:34:36, 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
2018-03-10 01:33:33, 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
2018-03-10 01:35:44, 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
2018-03-10 01:38:22, 302,Error,Application could not connect to database
2018-03-10 01:31:34, 303,Error,Application lost connection to database
```

Aşağıdaki sorgu bu verileri ayrışdıracak ve hesaplanan özelliklerden ikisiyle özetler. İlk satır _RawData_ özelliğini bir dize dizisine böler. Sonraki satırların her biri tek tek özelliklere bir ad verir ve bunları uygun veri türüne dönüştürmek için işlevleri kullanarak çıktıya ekler.

```Kusto
MyCustomCSVLog_CL
| extend CSVFields  = split(RawData, ',')
| extend EventTime  = todatetime(CSVFields[0])
| extend Code       = toint(CSVFields[1]) 
| extend Status     = tostring(CSVFields[2]) 
| extend Message    = tostring(CSVFields[3]) 
| where getyear(EventTime) == 2018
| summarize count() by Status,Code
```

## <a name="parse-predefined-structures-in-a-query"></a>Sorguda önceden tanımlanmış yapıları ayrıştır
Verileriniz bilinen bir yapıda biçimlendirilmişse, önceden tanımlanmış yapıları ayrıştırmak için [Kusto sorgu dilindeki](/azure/kusto/query/) işlevlerden birini kullanabilirsiniz:

- [Json](/azure/kusto/query/parsejsonfunction)
- [XML](/azure/kusto/query/parse-xmlfunction)
- [IPv4](/azure/kusto/query/parse-ipv4function)
- [URL](/azure/kusto/query/parseurlfunction)
- [URL sorgusu](/azure/kusto/query/parseurlqueryfunction)
- [Dosya yolu](/azure/kusto/query/parsepathfunction)
- [Kullanıcı aracısı](/azure/kusto/query/parse-useragentfunction)
- [Sürüm dizesi](/azure/kusto/query/parse-versionfunction)

Aşağıdaki örnek sorgu, JSON'da yapılandırılan _Azure Etkinliği_ tablosunun _Özellikler_ alanını ayrıştırır. Sonuçları, JSON'da adlandırılmış değeri içeren _parsedProp_adı verilen dinamik bir özelliğe kaydeder. Bu değerler, sorgu sonuçlarını filtrelemek ve özetlemek için kullanılır.

```Kusto
AzureActivity
| extend parsedProp = parse_json(Properties) 
| where parsedProp.isComplianceCheck == "True" 
| summarize count() by ResourceGroup, tostring(parsedProp.tags.businessowner)
```

Bu ayrıştma işlevleri işlemci yoğun olabilir, bu nedenle yalnızca sorgunuz biçimlendirilmiş verilerden birden çok özellik kullandığında kullanılmalıdır. Aksi takdirde, basit desen eşleştirme işleme daha hızlı olacaktır.

Aşağıdaki örnek, etki alanı denetleyicisi TGT Preauth türü dökümü gösterir. Tür yalnızca XML dizesi olan EventData alanında bulunur, ancak bu alandan başka veri gerekmez. Bu durumda, [ayrıştırmak](/azure/kusto/query/parseoperator) gerekli veri parçasını seçmek için kullanılır.

```Kusto
SecurityEvent
| where EventID == 4768
| parse EventData with * 'PreAuthType">' PreAuthType '</Data>' * 
| summarize count() by PreAuthType
```

## <a name="use-function-to-simulate-a-table"></a>Tabloyu simüle etmek için işlevi kullanma
Belirli bir tablonun aynı ayrıştırma gerçekleştirmek birden çok sorguolabilir. Bu durumda, her sorguda ayrıştırma mantığını çoğaltmak yerine ayrıştırılmış verileri döndüren [bir işlev oluşturun.](functions.md) Daha sonra işlev diğer adını diğer sorgularda özgün tablonun yerine kullanabilirsiniz.

Yukarıdaki virgül-sınırlı özel günlük örneğini göz önünde bulundurun. Ayrıştırılmış verileri birden çok sorguda kullanmak için, aşağıdaki sorguyu kullanarak bir işlev oluşturun ve _MyCustomCSVLog_takma adıyla kaydedin.

```Kusto
MyCustomCSVLog_CL
| extend CSVFields = split(RawData, ',')
| extend DateTime  = tostring(CSVFields[0])
| extend Code      = toint(CSVFields[1]) 
| extend Status    = tostring(CSVFields[2]) 
| extend Message   = tostring(CSVFields[3]) 
```

Şimdi aşağıdaki gibi sorgularda gerçek tablo adı yerine _MyCustomCSVLog_ takma kullanabilirsiniz.

```Kusto
MyCustomCSVLog
| summarize count() by Status,Code
```


## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](log-query-overview.md) hakkında bilgi edinin.
