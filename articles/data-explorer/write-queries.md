---
title: Azure Veri Gezgini için sorgu yazma
description: Bu nasıl yapılsa da, Azure Veri Gezgini için temel ve daha gelişmiş sorguları nasıl gerçekleştireceklerini öğrenirsiniz.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/07/2019
ms.openlocfilehash: 80d3eaaf7e588766d62f5e5885d75e61c590970e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68881191"
---
# <a name="write-queries-for-azure-data-explorer"></a>Azure Veri Gezgini için sorgu yazma

Bu makalede, en yaygın işleçlerle temel sorguları gerçekleştirmek için Azure Veri Gezgini'ndeki sorgu dilini nasıl kullanacağınızı öğrenirsiniz. Ayrıca dilin daha gelişmiş özelliklerinden bazılarına da maruz kalırsın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki sorguları iki şekilde çalıştırabilirsiniz:

- Azure Veri Gezgini'nde, öğrenmeye yardımcı olmak için kurduğumuz *kümeleme* de yardımcı olur.
    Azure Etkin dizininin üyesi olan bir kuruluş e-posta hesabıyla [kümede oturum açın.](https://dataexplorer.azure.com/clusters/help/databases/samples)

- StormEvents örnek verilerini içeren kendi kümenizde. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure Veri Gezgini kümesi ve veritabanı oluşturun](create-cluster-database-portal.md) ve örnek verileri Azure Veri [Gezgini'ne alın.](ingest-sample-data.md)

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="overview-of-the-query-language"></a>Sorgu diline genel bakış

Azure Veri Gezgini'ndeki sorgu dili, verileri işlemek ve sonuçları döndürmek için salt okunur bir istektir. İstek, sözdizimini kolay okunmasını, yazmasını ve otomatikleştirmesini sağlamak için tasarlanmış bir veri akışı modeli kullanılarak düz metinde belirtilir. Sorgu, SQL'e benzer bir hiyerarşide düzenlenen şema varlıklarını kullanır: veritabanları, tablolar ve sütunlar.

Sorgu, bir yarı sütunla sınırlandırılmış bir sorgu`;`deyimi dizisinden oluşur ve en az bir deyim tablo ifade deyimidir ve bu da tablo benzeri bir sütun ve satır örgüsünde düzenlenmiş verileri üreten bir deyimdir. Sorgunun tabular ifade ifadeleri sorgusonuçlarını üretir.

Tabloifade deyiminin sözdizimi, veri kaynağıyla (örneğin, bir veritabanındaki tablo veya veri üreten bir işleç) başlayarak ve daha sonra boru (`|`) delimiter kullanarak birbirine bağlanan bir veri dönüştürme işleci kümesinden akan bir tablo sorgu işlecinden diğerine tabloveri akışına sahiptir.

Örneğin, aşağıdaki sorguda bir tablo ifade deyimi olan tek bir ifade vardır. İfade, adı verilen `StormEvents` bir tabloya yapılan bir başvuruyla başlar (bu tabloyu barındıran veritabanı burada örtülüdür ve bağlantı bilgilerinin bir parçasıdır). Bu tabloya ait veriler (satırlar) `StartTime` sütunun değerine göre filtrelenir ve `State` sütunun değerine göre filtrelenir. Sorgu daha sonra "hayatta kalan" satır sayısını döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVWws1VISSxJLQGyNYwMDMx1DQ11DQw1FRLzUpBU2aArMgIpQjGvJFXB1lZByc3HP8jTxVFJQQEkm5xfmlcCAHoR9euCAAAA)**\]**

```Kusto
StormEvents
| where StartTime >= datetime(2007-11-01) and StartTime < datetime(2007-12-01)
| where State == "FLORIDA"  
| count
```

Bu durumda, sonuç:

|Sayı|
|-----|
|   23|
| |

Daha fazla bilgi için [Sorgu dili başvurusuna](https://aka.ms/kustolangref)bakın.

## <a name="most-common-operators"></a>En yaygın operatörler

Bu bölümde kapsanan işleçler, Azure Veri Gezgini'ndeki sorguları anlamak için yapı taşlarıdır. Yazdığınız sorguların çoğu bu işleçlerden birkaçını içerir.

Yardım kümesinde sorguları çalıştırmak için: Her sorgunun üstünde **sorguyu çalıştırmak için Tıklat'ı** tıklatın'ı seçin.

Kendi kümenizde sorguları çalıştırmak için:

1. Her sorguyu web tabanlı sorgu uygulamasına kopyalayın ve ardından sorguyu seçin veya imlecinizi sorguya yerleştirin.

1. Uygulamanın üst kısmında **Çalıştır'ı**seçin.

### <a name="count"></a>count

[**count**](https://docs.microsoft.com/azure/kusto/query/countoperator): Tablodaki satır sayısını verir.

Aşağıdaki sorgu, StormEvents tablosundaki satır sayısını döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRSM4vzSsBALU2eHsTAAAA)**\]**

```Kusto
StormEvents | count
```

### <a name="take"></a>almak

[**take**](https://docs.microsoft.com/azure/kusto/query/takeoperator): Belirtilen veri satırı sayısına kadar döner.

Aşağıdaki sorgu StormEvents tablosundan beş satır döndürür. Anahtar kelime *sınırı* almak için bir takma *addır.*

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRKEnMTlUwBQDEz2b8FAAAAA%3d%3d)**\]**

```Kusto
StormEvents | take 5
```

> [!TIP]
> Kaynak veriler sıralı olmadığı sürece hangi kayıtların döndürüldürün garantisi yoktur.

### <a name="project"></a>proje

[**project**](https://docs.microsoft.com/azure/kusto/query/projectoperator): Sütun alt kümesini seçer.

Aşağıdaki sorgu belirli bir sütun kümesini döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUShJzE5VMAWxCorys1KTSxSCSxKLSkIyc1N1FFzzUiAMoFgJiA%2fSFlJZAGS6JOYmpqcGFOUXpBaVVAKlCjKL81NS%2fRKLihJLMstSAY%2buIINnAAAA)**\]**

```Kusto
StormEvents
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="where"></a>where

[**where**](https://docs.microsoft.com/azure/kusto/query/whereoperator): Tabloyu bir yüklemi karşılayan satır alt kümesine filtreler.

Aşağıdaki sorgu verileri `EventType` ve . `State`

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAEWMPQvCMBCGd8H%2fcFuWro4dBOvHkgoJOB%2fm0KjJhetRKfjjNe3g9n49r1OW1I2UdVivPvC%2bkxDM3k%2bFoG3B7F%2fMwQDmAE5Rl%2fCydceTPfjemsopPgk2VRXhB121TkV9TNRAl8MiZrz53zeww4Q3OgsXEp1%2bVYkDB7IoghpH%2bgI9OH8WnwAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="sort"></a>sort

[**sıralama**](https://docs.microsoft.com/azure/kusto/query/sortoperator): Giriş tablosunun satırlarını bir veya daha fazla sütuna göre sırayla sıralayın.

Aşağıdaki sorgu, verileri azalan sırada `DamageProperty`.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2NPQvCMBCGd8H%2fcFuXrI4dBOvHEoUGnM%2fm0KjphctRKfjjNe0guL0fvM%2fbKktsBuo1LxdveN1ICCbvxkRQ11Btn8y%2bAuw9tIo6h%2bd1uz%2fYnTvaquwyi8JlhA1GvNJJOJHoCJ5yV2rFB8GqqCR8p04LSdSFSAaa3s9iopvfu%2fnDfasUMnuyKIIaBvoAtvGMsb4AAAA%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| sort by DamageProperty desc
| take 5
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

> [!NOTE]
> Operasyon sırası önemlidir. Önce `sort by` `take 5` koyarak deneyin. Farklı sonuçlar alır mısın?

### <a name="top"></a>üst

[**top**](https://docs.microsoft.com/azure/kusto/query/topoperator): Belirtilen sütunlara göre sıralanmış ilk *N* kayıtlarını verir.

Aşağıdaki sorgu, bir daha az işleç ile yukarıdaki gibi aynı sonuçları döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2NOwvCMBSFd8H%2fcLcsWR07CNbHkgoJOMfmohGTG24vlYA%2fXtsOgtt5cL5jhTi1I2YZ1qs3vO7ICLN3tSA0Daj9kygo8DmAFS9LeNna48kcXGfUtBMqsIFrhZ1P%2foZnpoIsFQIO%2fdQXpgf2MgFYXEyooc1hETNU%2f071H%2bRblThQQOOZvcQRP1rSng21AAAA)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| top 5 by DamageProperty desc
| project StartTime, EndTime, State, EventType, DamageProperty, EpisodeNarrative
```

### <a name="extend"></a>Genişlet -mek

[**extend**](https://docs.microsoft.com/azure/kusto/query/extendoperator): Türemiş sütunları hesaplar.

Aşağıdaki sorgu, her satırda bir değer hesaplayarak yeni bir sütun oluşturur.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2OvQ7CMAyEdyTewVuWMDJ2QGr5WQJSKzGHxoIiEkeuKVTi4WmooBKbfXeffaUQ%2b6LDIO189oLHBRnhs1d9RMgyUOsbkVNgg4NSrIzicVVud2ZT7Y1KnFCEJZx6yK23ZzwwRWTpwWFbJx%2bfggOf39lKQwEyKIKrGo%2bwSEdZ0pyCkemKtUyi%2fib1j9ZjDz311H9%2fBys2LTk0lhPT4RvwA3pn6AAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where EventType == 'Flood' and State == 'WASHINGTON'
| top 5 by DamageProperty desc
| extend Duration = EndTime - StartTime
| project StartTime, EndTime, Duration, State, EventType, DamageProperty, EpisodeNarrative
```

İfadeler tüm olağan işleçleri (+, -, *, /, %)ve çağırabileceğiniz bir dizi yararlı işlev vardır.

### <a name="summarize"></a>Özetle

[**özet :**](https://docs.microsoft.com/azure/kusto/query/summarizeoperator)Satır gruplarını toplar.

Aşağıdaki sorgu da olayların sayısını `State`döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIBYnFJ%2beX5pUo2CqAaQ1NhaRKheCSxJJUAB%2fedDI3AAAA)**\]**

```Kusto
StormEvents
| summarize event_count = count() by State
```

**Özetle** işleç, **yan** tümcede aynı değerlere sahip satırları bir araya getirir ve sonra her grubu tek bir satırda birleştirmek için toplama işlevini **(sayım**gibi) kullanır. Yani, bu durumda, her durum için bir satır ve bu durumdaki satır sayısı için bir sütun vardır.

Bir dizi toplama işlevi vardır ve birkaç tane hesaplanmış sütun oluşturmak için birkaç ını bir **özet** işleçte kullanabilirsiniz. Örneğin, her eyaletteki fırtına sayısını ve eyalet başına benzersiz sayıda fırtına yı alabilir, ardından fırtınadan en çok etkilenen durumları elde etmek için **en üstte** kullanabilirsiniz.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlUIBkk455fmlSjYKiSDaA1NHYWQyoJU%2fzSwXDFQPAUiAdYPktJUSKoE6kwsSQUZVpJfoGAKEYGblZJanAwAgbFb73QAAAA%3d)**\]**

```Kusto
StormEvents
| summarize StormCount = count(), TypeOfStorms = dcount(EventType) by State
| top 5 by StormCount desc
```

**Özet işlemin** sonucu:

- Tarafından adlandırılmış **by** her sütun

- Her hesaplanmış ifade için bir sütun

- Değerlere göre her birleşimi için bir satır

### <a name="render"></a>Render

[**render**](https://docs.microsoft.com/azure/kusto/query/renderoperator): Sonuçları grafik çıktı olarak işler.

Aşağıdaki sorgu bir sütun grafiği görüntüler.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWMsQ7CQAxDdyT%2bIWMrdSgbSxmQ2Nj6Aei4Ru0hkqA0VwTi49uUBRZL9rPdmiidJmQbt5sPjJkoaHojoGeXKJmtWbUoK6DUQQNh6osj9onPwUq4vqC1YLjORc2Dpef2OaD%2bPcEBdvu6dvZQuWG077b6LTlV5A4VotwzcRyC2gxU6ktSqQAAAA%3d%3d)**\]**

```Kusto
StormEvents
| summarize event_count=count(), mid = avg(BeginLat) by State
| sort by mid
| where event_count > 1800
| project State, event_count
| render columnchart
```

Aşağıdaki sorgu basit bir zaman grafiği görüntüler.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIBYnFJ%2beX5pXYgkkNTYWkSoWkzDyN4JLEopKQzNxUHQXDFE2QtqLUvJTUIoUSoFhyBlASAAyXWQJWAAAA)**\]**

```Kusto
StormEvents
| summarize event_count=count() by bin(StartTime, 1d)
| render timechart
```

Aşağıdaki sorgu, olayları modulo'nun bir gün içinde olduğu zamana göre sayar, saate binned ve bir zaman grafiği görüntüler.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEADWNQQqDMBRE90LvMBtBwY0HcNkT2L2k8UuEJh9%2bfqSWHt4k4GZghpk3s7L450FB46P5g75KYYXjJJiwfZilm9WIvnZPaDGuGDC6vnRj8t7I%2fiNQ2S%2bWU9CpatfjfVZKLbLo7WGiLZnkGxJoxlqX%2bRf81ZbyiAAAAA%3d%3d)**\]**

```Kusto
StormEvents
| extend hour = floor(StartTime % 1d , 1h)
| summarize event_count=count() by hour
| sort by hour asc
| render timechart
```

Aşağıdaki sorgu, bir zaman grafiğindeki birden çok günlük seriyi karşılaştırır.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEACWPSwvCMBCE74L%2fYSgIFXrpD%2bihaKzxkUBTXyeputKCbSCmvvDHm9TL7gwzsN8qq03DHtTa%2b3DwBb0stRdUujMJrjetTQhlS2OLuiGMEF8QIa7GvvusyJBPLaFuEQbZZjWDnGHN9nwigyhYp1wwt7c8z7jgqZM7riZSKC6cFjIv5pimS1n4SLAdFixX7OCMzFkmRdAfundNU5r6QyAPejzrrrVJP8MxTu8eN%2fqT%2bL5xL5CBdcjnyrH%2fALPTSKnkAAAA)**\]**

```Kusto
StormEvents
| extend hour= floor( StartTime % 1d , 1h)
| where State in ("GULF OF MEXICO","MAINE","VIRGINIA","WISCONSIN","NORTH DAKOTA","NEW JERSEY","OREGON")
| summarize event_count=count() by hour, State
| render timechart
```

> [!NOTE]
> **Render** işleci, motorun bir parçası değil, istemci tarafı bir özelliktir. Kullanım kolaylığı için dile entegre edilmiştir. Web uygulaması aşağıdaki seçenekleri destekler: barchart, columnchart, piechart, timechart ve linechart. 

## <a name="scalar-operators"></a>Skaler operatörleri

Bu bölüm en önemli skaler işleçlerinden bazılarını kapsamaktadır.

### <a name="bin"></a>bin()

[**bin()**](https://docs.microsoft.com/azure/kusto/query/binfunction): Değerleri belirli bir depo gözü boyutunun tamsayı katLarına yuvarlar.

Aşağıdaki sorgu, bir gün kova boyutuyla sayımı hesaplar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVWwU0hJLEktATI1jAwMzHUNjHQNTTQVEvNSkBTZYCoyMtQEGVdcmpubWJRZlaqQCrIiPjm%2fNK9EwVYBTGtoKiRVKiRl5mnAjdJRMEzRBABIhjnmkwAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime > datetime(2007-02-14) and StartTime < datetime(2007-02-21)
| summarize event_count = count() by bin(StartTime, 1d)
```

### <a name="case"></a>durum()

[**case()**](https://docs.microsoft.com/azure/kusto/query/casefunction): Yüklemlerin listesini değerlendirir ve yüklemi tatmin olan ilk sonuç ifadesini veya son **başka** ifadeyi döndürür. Verileri kategorilere ayırmak veya gruplandırmak için bu işleci kullanabilirsiniz:

Aşağıdaki sorgu yeni bir `deaths_bucket` sütun döndürür ve ölümleri sayıya göre gruplanır.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAGWOwQrCQAxE74X%2bQ9hTCwX14FFBaK9e%2bgGS7gZdbFrYZEXFj7dbqgfNbfJmhml1DNzcaFDJsxdIZMbgnwSOUC8Cu%2fQq6lnUPpDVEroHtIpKKUB3pcEt7lMX7ZV0ClkUgiLPYLqlaQ%2fbdQWmx3AmU%2f2gTUJMzkf%2bYwkJY99%2fiDmuDqac545Bv3MAxb4Bic1Oy88AAAA%3d)**\]**

```Kusto
StormEvents
| summarize deaths = sum(DeathsDirect) by State
| extend deaths_bucket = case (
    deaths > 50, "large",
    deaths > 10, "medium",
    deaths > 0, "small",
    "N/A")
| sort by State asc
```

### <a name="extract"></a>özü()

[**extract()**](https://docs.microsoft.com/azure/kusto/query/extractfunction): Metin dizesinden normal bir ifade için eşleşme alır.

Aşağıdaki sorgu, bir izlemeden belirli öznitelik değerlerini ayıklar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAE2OwQrCMBBE74X%2bw9BTojHYagSVHJRevXkrHqJdpVBbSVew4McbFYungeXtvKmJsetzxw4WZQh2x5og9t6daIWOfdVcJIpkY1OFrc0U8rt3XLWNTbOZnhultU4UfoD5A4zRmVkovInDOo6%2bojh6gh5MTTmQwR0uQckiGb5FMZ0s9WEsQ3uo%2fixSccT9jdqz8ORqKTECV1cSaSdfq2k6L8oAAAA%3d)**\]**

```Kusto
let MyData = datatable (Trace: string) ["A=1, B=2, Duration=123.45,...", "A=1, B=5, Duration=55.256, ..."];
MyData
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s)
```

Bu sorgu, **let** bir adı (bu durumda) `MyData`bir ifadeye bağlayan bir let deyimi kullanır. **Let** deyiminin göründüğü kapsamın geri kalanı için (genel kapsam veya işlev gövdesi kapsamında), ad bağlı değerine başvurmak için kullanılabilir.

### <a name="parse_json"></a>parse_json()

[**parse_json()**](https://docs.microsoft.com/azure/kusto/query/parsejsonfunction): Bir dizeyi JSON değeri olarak yorumlar ve değeri dinamik olarak döndürür. Bir bileşik JSON nesnesinin birden fazla öğeayıklamak gerektiğinde **extractjson()** işlevini kullanarak üstündür.

Aşağıdaki sorgu, JSON öğelerini bir diziden ayıklar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAHWPQQuCQBCF74L%2fYdmLBSJ6EGKjU17r1E0kJh1C2XZlHc0w%2f3ur1s1O896bB%2fONRGKnVwIE7MAKOwhuEtnmYiBHwRoypbpvXSf1Bl60BqjUiot04B3IFrmIol0Q%2bpPLdauIi3iyj9KWojCcNfRWx7NuqEiw48KaMRu9bO86y3HXeTPsCVXBzvg8amlpajANXqtGq4VmO5VqoyvM6dsKfkhpmAUzkf9nM9OtLi3reg79ar788AEVX8GkOAEAAA%3d%3d)**\]**

```Kusto
let MyData = datatable (Trace: string)
['{"duration":[{"value":118.0,"valcount":5.0,"min":100.0,"max":150.0,"stdDev":0.0}]}'];
MyData
| extend NewCol = parse_json(Trace)
| project NewCol.duration[0].value, NewCol.duration[0].valcount, NewCol.duration[0].min, NewCol.duration[0].max, NewCol.duration[0].stdDev
```

Aşağıdaki sorgu JSON öğelerini ayıklar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAE2OwQqCQBCG74LvsOzFBBE9CLHRKa916hYRkw6RbLuyO5pRvXvrGtZpvn9m4P8kEts%2bSiBga1a7QXCWyBZ7AxUKZslc1SVmh%2bjJe5AdcpHnyzRLxlTpThEXxRhvV%2bVOWeYZBseFZ0t1iT0XLryj4yoMprIweDEcCFXNdnjfaOnaWzAWT43VamqPx6fW6AYr%2bn6l3iH5S95hXjiLH8Mw82TxAQvJEB%2fsAAAA)**\]**

```Kusto
let MyData = datatable (Trace: string) ['{"value":118.0,"valcount":5.0,"min":100.0,"max":150.0,"stdDev":0.0}'];
MyData
| extend NewCol = parse_json(Trace)
| project NewCol.value, NewCol.valcount, NewCol.min, NewCol.max, NewCol.stdDev
```

Aşağıdaki sorgu dinamik bir veri türü ile JSON öğeleri ayıklar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAD2NMQvCMBBG90D%2bw5GphVLSoSARt65ubuJwJjdU0lZiWlrU%2f25MotO9x8H7LHk4bh16hAOYcDxeLUFxcqhJgdlGHHpdcnbOWDzFgnYmoZpmV8tK6GkePTmh2q8N%2fRg%2bUkbGNXAb%2beFNR4tQQd7lZc9ZGuXsBXc33Uh7iJN1jFdZcvunIf5HXCvOEqf2BwXmDCnKAAAA)**\]**

```Kusto
let MyData = datatable (Trace: dynamic)
[dynamic({"value":118.0,"counter":5.0,"min":100.0,"max":150.0,"stdDev":0.0})];
MyData
| project Trace.value, Trace.counter, Trace.min, Trace.max, Trace.stdDev
```

### <a name="ago"></a>önce()

[**ago()**](https://docs.microsoft.com/azure/kusto/query/agofunction): Verilen zaman dilimini geçerli UTC saat saatinden çıkarır.

Aşağıdaki sorgu son 12 saatiçin veri döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA1WOQQ6CQAxF9yTc4S8hQcmQuNSNR4ALTKQyJDAlnSIuPLwzJGrctM3v+7+t684R7qMEhW6MafQUMJAnsUoIdl4mQm/VVrC+h0Z6shFOINZAIc/qOql24KIEL8nIAuWYohC6sfQB9yjtPtPA8SrhmGeLjF7RjTO1Gu+cIdYPVHjeisOpLyukKTbjYml5piuvXknwIU1lGlPm2Qvzg55L+u+b9udIyOZI6LfHZf/YNK58Ay2HrbAEAQAA)**\]**

```Kusto
//The first two lines generate sample data, and the last line uses
//the ago() operator to get records for last 12 hours.
print TimeStamp= range(now(-5d), now(), 1h), SomeCounter = range(1,121)
| mv-expand TimeStamp, SomeCounter
| where TimeStamp > ago(12h)
```

### <a name="startofweek"></a>hafta başlangıcı()

[**startofweek()**](https://docs.microsoft.com/azure/kusto/query/startofweekfunction): Sağlanan bir mahsup ile kaydırılan tarihi içeren haftanın başlangıcını verir

Aşağıdaki sorgu, haftanın başlangıcını farklı uzaklıklarla döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEACtKzEtPVchPSytOLVFIK8rPVdA1VCjJVzBUKC5JLVAw5OWqUSgoys9KTS5RKE9NzQ4uSSwqUbAFygLp%2fDSQkEZefrmGpg7UEE0dCA0AdE3lv1kAAAA%3d)**\]**

```Kusto
range offset from -1 to 1 step 1
| project weekStart = startofweek(now(), offset),offset
```

Bu sorgu, tek sütunlu değerler tablosu oluşturan **aralık** işleci kullanır. Ayrıca bakınız: [**startofday()**](https://docs.microsoft.com/azure/kusto/query/startofdayfunction), [**startofweek()**](https://docs.microsoft.com/azure/kusto/query/startofweekfunction), [**startofyear()**](https://docs.microsoft.com/azure/kusto/query/startofyearfunction), [**endofday()**](https://docs.microsoft.com/azure/kusto/query/startofmonthfunction), [**endofweek()**](https://docs.microsoft.com/azure/kusto/query/endofdayfunction), [**endofmonth()**](https://docs.microsoft.com/azure/kusto/query/endofmonthfunction), ve [**endofyear()**](https://docs.microsoft.com/azure/kusto/query/endofyearfunction). [**endofweek()**](https://docs.microsoft.com/azure/kusto/query/endofweekfunction)

### <a name="between"></a>arasında()

[**arasında()**](https://docs.microsoft.com/azure/kusto/query/betweenoperator): Kapsayıcı aralıktaki girdiyle eşleşir.

Aşağıdaki sorgu, verileri belirli bir tarih aralığına göre filtreler.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrApGRuaaCnp4ChrixgaYmyKTk%2fNK8EgBluyagXgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime between (datetime(2007-07-27) .. datetime(2007-07-30))
| count
```

Aşağıdaki sorgu, başlangıç tarihinden itibaren üç günlük hafif`3d`bir varyasyonla verileri belirli bir tarih aralığına göre filtreler.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQguSSwqCcnMTVVISi0pT03NU9BISSxJLQGKaBgZGJjrApGRuaaCnp6CcYomSF9yfmleCQCGAqjRTAAAAA%3d%3d)**\]**

```Kusto
StormEvents
| where StartTime between (datetime(2007-07-27) .. 3d)
| count
```

## <a name="tabular-operators"></a>Tabular işleçleri

Kusto'nun, bazıları bu makalenin diğer bölümlerinde yer alan birçok tabular işleçleri vardır. Burada **ayrışdırıya**odaklanacağız. 

### <a name="parse"></a>parse

[**ayrışmama**](https://docs.microsoft.com/azure/kusto/query/parseoperator): Dize ifadesini değerlendirir ve değerini bir veya daha fazla hesaplanan sütuna ayrıştur. Ayrışdırmanın üç yolu vardır: basit (varsayılan), regex ve rahat.

Aşağıdaki sorgu, basit ayrıştırma varsayılanını kullanarak izlemeyi ayrıştırır ve ilgili değerleri ayıklar. İfade (StringConstant olarak anılacaktır) normal bir dize değeridir ve eşleşme katıdır: genişletilmiş sütunlar gerekli türlerle eşleşmelidir.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2UTU%2fDMAyG75X6H6xcxlCkpRlsUNQjN6gQ2wnEoevMFsiaKk2HJvHjabqvlI91l11QLrH12vETW5Zo4H411kmKEME0MdWZSISz2yVmpvaHhdEim3V979n3OrU%2fhFgZ8boaSZHiI0pMiipEY6FKnWKcLDB6EDlKkeEoneO0lKgpGGUSWYcUER9SKOw1LhcT1BHvU5AqfR%2bLKpbxXjDscRYMgF2FFyxkwRMFvX7ngCLXuBSqLO5%2bT9S%2ftrJuh54OI7g8iMFaMdhxGOy0GJz9i25w%2fjdG0IoRHNWNNe1ph2pwEKNlqI7HsEPley83vrfZCL73CXmiq%2fr32wA%2bhJnDOZAGEQHXBNIEIq4VSpXNbAIXkbjAO8UOmuz4bWoXlrhWWO0vqyA2%2bAcw2f7B1rORd60calat3jA1TRbq1A6NxsC%2bLdCoCuj3p74AKTs4pmcFAAA%3d)**\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse EventTrace with * "resourceName=" resourceName ", totalSlices=" totalSlices:long * "sliceNumber=" sliceNumber:long * "lockTime=" lockTime ", releaseTime=" releaseTime:date "," * "previousLockTime=" previouLockTime:date ")" *  
| project resourceName ,totalSlices , sliceNumber , lockTime , releaseTime , previouLockTime
```

Aşağıdaki sorgu bir izlemeyi ayrıştırır ve `kind = regex`'yi kullanarak ilgili değerleri ayıklar. StringConstant normal bir ifade olabilir.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2UQU%2fCQBCF7036HyZ7gWKRbVHQmgY9eNPGCCcoh9KOsLK0ZLtFMf54l6LQBgUuXEyTTbP7pt3vvclwlPC47IkgRHAhCqR6Rhyher%2fAWOb7TioFi8eGrg10rZLvO%2bAlkr0su5yF%2bIwcg1SVCEyTTIToBTN0n9gcOYuxG04wyjgKE2QiA56XpK7dNiFdvXrZbITCtZsm8CSc9piqpXbDajdsarWAXjkX1KFW3wSx%2fs8exVzggiVZ%2bvD7h5rXK5lRMU%2bHYV3uxaAHMehxGPS0GDb9F2nY9t8Y1kEM66g01rSnbarWXowDTXU8xqqpdG14o2vfE0HXPmEeCHX%2fKYsjNR8EjvEdtqMB3picAKme1zrGIKh%2f3NX7w5pLoEgLt6SM56c1PzpTq6oqYpIitMOTeAxAlKb6c3Wjs3GBbAzJJUV8UjQjP91BJztuOGryKbHvGwQgxxbJK4ayTFKKBbahQCkA2DX7C29veJJmBQAA)**\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse kind = regex EventTrace with "(.*?)[a-zA-Z]*=" resourceName @", totalSlices=\s*\d+\s*.*?sliceNumber=" sliceNumber:long  ".*?(previous)?lockTime=" lockTime ".*?releaseTime=" releaseTime ".*?previousLockTime=" previousLockTime:date "\\)"  
| project resourceName , sliceNumber , lockTime , releaseTime , previousLockTime
```

Aşağıdaki sorgu bir izlemeyi ayrıştırır ve `kind = relaxed`'yi kullanarak ilgili değerleri ayıklar. StringConstant normal bir dize değeridir ve eşleşme rahattır: genişletilmiş sütunlar gerekli türleri kısmen eşleşebilir.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAN2US0%2fCQBDH7036HSZ7wZpN2BYFrenRGzZG4KLxUNoRVpYu2W5REj%2b83fKw9QE1kYvppTOZx%2f%2b3MxmBGm5WQxXFCAEkkS6%2bsUA4uV5iqku%2fn2nF04ljWw%2b21Sr9PoRS86fVQPAY71BglBUpCjOZqxjDaI7BLV%2bg4CkO4ikmuUBFQUsdiTIlC7wehcz8hvl8jCrwOhSEjGdDXuQyr%2b322h5zu8Au%2fDPmM%2feeglr32ROxULjkMs%2f63xfqXJowp0WPh%2bGe78VgBzFYMwx2XAyP%2fYtpeN7PGO5BDLfRNNa0x12q7l6MA0vVHMMslW09XtnW5iLY1hssIlXon%2fE0CYom0SsmQP6IMxz1%2b7%2b7AnXQdX6TNXMIvHA9hVMgNYEEqiaQuj5StXwh04kpUNVLqup3ETsCsoMxpavSSdXyi7NrIohJ%2foJDtoRbzybcMeFQjkjJZ4x1nYVWtEPtleHjjaGmCujnVu%2fWU75tHgYAAA%3d%3d)**\]**

```Kusto
let MyTrace = datatable (EventTrace:string)
[
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=23, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=15, lockTime=02/17/2016 08:40:00Z, releaseTime=02/17/2016 08:40:00Z, previousLockTime=02/17/2016 08:39:00Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=20, lockTime=02/17/2016 08:40:01Z, releaseTime=02/17/2016 08:40:01Z, previousLockTime=02/17/2016 08:39:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=22, lockTime=02/17/2016 08:41:01Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:01Z)',
'Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41:00Z, releaseTime=02/17/2016 08:41:00Z, previousLockTime=02/17/2016 08:40:00Z)'
];
MyTrace
| parse kind=relaxed "Event: NotifySliceRelease (resourceName=PipelineScheduler, totalSlices=NULL, sliceNumber=23, lockTime=02/17/2016 08:40:01, releaseTime=NULL, previousLockTime=02/17/2016 08:39:01)" with * "resourceName=" resourceName ", totalSlices=" totalSlices:long * "sliceNumber=" sliceNumber:long * "lockTime=" lockTime ", releaseTime=" releaseTime:date "," * "previousLockTime=" previousLockTime:date ")" *  
| project resourceName ,totalSlices , sliceNumber , lockTime , releaseTime , previousLockTime
```

## <a name="time-series-analysis"></a>Zaman serisi analizi

### <a name="make-series"></a>make serisi

[**make-serisi**](https://docs.microsoft.com/azure/kusto/query/make-seriesoperator): [özet gibi](https://docs.microsoft.com/azure/kusto/query/summarizeoperator)satır gruplarını bir araya toplar, ancak değerlere göre her bir kombinasyon için bir (zaman) serisi vektör oluşturur.

Aşağıdaki sorgu, günlük fırtına olaylarının sayısı için bir dizi zaman serisini döndürür. Sorgu, her durum için üç aylık bir dönemi kapsar ve eksik kutuları sabit 0 ile doldurur:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUchNzE7VLU4tykwtVsizTc4vzSvR0FRISU1LLM0psTVQyM9TCC5JLCoJycxNVcjMUyhKzEtP1UhJLEktAYpoGBkYmOsaGAKRpo4CmqixrjFI1DBFUyGpEmRKSSoAazsM0n0AAAA%3d)**\]**

```Kusto
StormEvents
| make-series n=count() default=0 on StartTime in range(datetime(2007-01-01), datetime(2007-03-31), 1d) by State
```

Bir dizi (zaman) serisi oluşturduktan sonra, anormal şekilleri, mevsimsel desenleri ve çok daha fazlasını algılamak için seri işlevleri uygulayabilirsiniz.

Aşağıdaki sorgu, belirli bir gün içinde en çok olay olan ilk üç durumları ayıklar:

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAF2OsQoCMRBEe8F%2f2DIBAzmvsLrSLzj7EC%2brBs3mSPbkBD%2feLDYibPVmZmdGziUdn0hct5s3JH9HU7FErEDDlBdipSHgxS8PHixkgpF94VNMCJGgeLqiCp6RG1F7aw%2fGdu30Dv5ob3qhXdBwfskXRmnElZECfDtdbbgq0qJwnqEX76%2fmyCW%2ftkV1Ek9pWSwgNdOt7foAJIuybs8AAAA%3d)**\]**

```Kusto
StormEvents
| make-series n=count() default=0 on StartTime in range(datetime(2007-01-01), datetime(2007-03-31), 1d) by State
| extend series_stats(n)
| top 3 by series_stats_n_max desc
| render timechart
```

Daha fazla bilgi [için, seri işlevlerinin](https://docs.microsoft.com/azure/kusto/query/scalarfunctions#series-processing-functions)tam listesini gözden geçirin.

## <a name="advanced-aggregations"></a>Gelişmiş toplamalar

Bu makalenin **başlarında, sayım** ve **özetle gibi**temel toplamaları ele aldık. Bu bölümde daha gelişmiş seçenekler tanıtıyor.

### <a name="top-nested"></a>üst iç içe

[**üst iç içe**](https://docs.microsoft.com/azure/kusto/query/topnestedoperator): Hiyerarşik üst sonuçlar üretir, her düzey önceki düzey değerlerine dayalı bir dekler aşağı.

Bu işleç pano görselleştirme senaryoları için yararlıdır veya aşağıdaki gibi bir soruyu yanıtlamak için gerekli olduğunda: "K1'in en üst-N değerlerini bulun (bazı toplama kullanarak); her biri için, K2 üst-M değerleri (başka bir toplama kullanarak) ne olduğunu bulmak; ..."

Aşağıdaki sorgu, en üst `State` düzeyde hiyerarşik bir `Sources`tablo döndürür ve ardından .

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjJL9DNSy0uSU1RMFLIT1MILkksSVVIqlQoLs3VcEpNz8zzSSzR1OHlQlJoDFaYX1qUTEilIUila16KT35yYklmfh6GcgDrXwk5jgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| top-nested 2 of State by sum(BeginLat),
top-nested 3 of Source by sum(BeginLat),
top-nested 1 of EndLocation by sum(BeginLat)
```

### <a name="pivot-plugin"></a>pivot() eklentisi

[**pivot() eklentisi**](https://docs.microsoft.com/azure/kusto/query/pivotplugin): Giriş tablosundaki tek ildeki benzersiz değerleri çıktı tablosunda birden çok sütuna dönüştürerek tabloyu döndürür. İşletici, son çıktıda kalan sütun değerlerinde gerekli olan toplamaları gerçekleştirir.

Aşağıdaki sorgu bir filtre uygular ve satırları sütunlara döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSgoys9KTS5RCC5JLEnVUQBLhFQWpILkyjNSi1IhMgrFJYlFJcXlmSUZCkqOPkoIabgOhYzEYgWl8My8FLBsalliTilIZ0FmWX6JBtgUTQDlv21NfQAAAA%3d%3d)**\]**

```Kusto
StormEvents
| project State, EventType
| where State startswith "AL"
| where EventType has "Wind"
| evaluate pivot(State)
```

### <a name="dcount"></a>dcount()

[**dcount()**](https://docs.microsoft.com/azure/kusto/query/dcount-aggfunction): Gruptaki bir ifadenin farklı değerlerinin sayısını tahmin eder. Tüm değerleri saymak için [**count()**](https://docs.microsoft.com/azure/kusto/query/countoperator) kullanın.

Aşağıdaki sorgu ' `Source` `State`ya göre ayrı sayar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlUIzi8tSk4tVrBVSEnOL80r0YAIaCokVSoElySWpAIAFKgSBDoAAAA%3d)**\]**

```Kusto
StormEvents
| summarize Sources = dcount(Source) by State
```

### <a name="dcountif"></a>dcountif()

[**dcountif()**](https://docs.microsoft.com/azure/kusto/query/dcountif-aggfunction): Yüklemin doğru olarak değerlendirdiği satırlar için ifadenin farklı değerlerinin sayısını tahmin eder.

Aşağıdaki sorgu, where'in `Source` `DamageProperty < 5000`farklı değerlerini sayar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSspVqhRKEnMTlUwNDDg5apRKC7NzU0syqxKVQjOLy1KTi1WsFVISc4vzSvJTNOACOkouCTmJqanBhTlF6QWlVQq2CiYGhgYaCokVSoElySWpAIAuk%2fTX14AAAA%3d)**\]**

```Kusto
StormEvents 
| take 100
| summarize Sources = dcountif(Source, DamageProperty < 5000) by State
```

### <a name="dcount_hll"></a>dcount_hll()

[**dcount_hll()**](https://docs.microsoft.com/azure/kusto/query/dcount-hllfunction): HyperLogLog sonuçlarından [**(hll**](https://docs.microsoft.com/azure/kusto/query/hll-aggfunction) veya [**hll_merge**](https://docs.microsoft.com/azure/kusto/query/hll-merge-aggfunction)tarafından **oluşturulan) dcount** hesaplar.

Aşağıdaki sorgu, sayımı oluşturmak için HLL algoritmasını kullanır.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlXIyMkJSi1WsAUxNFwScxPTUwOK8gtSi0oqNRWSKhWSMvM0gksSi0pCMnNTdQwNcjUx9PumFqWnpkCMiM8FcTQgpoKVFhTlZ6UmlyikJOeX5pXEg6yB69EEAKm9wyCXAAAA)**\]**

```Kusto
StormEvents
| summarize hllRes = hll(DamageProperty) by bin(StartTime,10m)
| summarize hllMerged = hll_merge(hllRes)
| project dcount_hll(hllMerged)
```

### <a name="arg_max"></a>arg_max()

[**arg_max()**](https://docs.microsoft.com/azure/kusto/query/arg-max-aggfunction): Grupta bir ifadeyi en üst düzeye çıkaran bir satır bulur ve başka bir ifadenin değerini (veya tüm satırı döndürmek için *) döndürür.

Aşağıdaki sorgu, her durumda son sel raporunun saatini döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSjPSC1KVQDzQyoLUhVsbRWU3HLy81OUQLLFpbm5iUWZVakKiUXp8bmJFRrBJYlFJSGZuak6ClqaCkmVCkCBklSQ2oKi%2fKzU5BKIgI4CkkLXvBQoA2YNAHO1S0OFAAAA)**\]**

```Kusto
StormEvents
| where EventType == "Flood"
| summarize arg_max(StartTime, *) by State
| project State, StartTime, EndTime, EventType
```

### <a name="makeset"></a>makeset()

[**makeset()**](https://docs.microsoft.com/azure/kusto/query/makeset-aggfunction): Bir ifadenin grupta aldığı farklı değerler kümesinin dinamik (JSON) dizisini verir.

Aşağıdaki sorgu, her durum tarafından bir sel raporlandığında ve farklı değerler kümesinden bir dizi oluşturduğu tüm zamanları döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWLQQ6CQBAE7yb8ocNJE76wR3mA8IEFOxF1mM3siIHweAVPHqsq1bianCeOnovDiveNRuzczokIAWX9VL2WW80vkWjDQuzuwqTmGQESH8z0Y%2bPRvB2EJ3QzvuTcvmR6Z%2b8%2fUf3NH6ZkMFeAAAAA)**\]**

```Kusto
StormEvents
| where EventType == "Flood"
| summarize FloodReports = makeset(StartTime) by State
| project State, FloodReports
```

### <a name="mv-expand"></a>mv-genişletmek

[**mv-expand**](https://docs.microsoft.com/azure/kusto/query/mvexpandoperator): Çok değerli toplama(lar)ı dinamik bir sütundan genişletir, böylece koleksiyondaki her değer ayrı bir satır alır. Genişletilmiş bir satırdaki diğer tüm sütunlar çoğaltılır. Makelist'in tam tersi.

Aşağıdaki sorgu, bir küme oluşturup **mv genişletme** yeteneklerini göstermek için kullanarak örnek veri oluşturur.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWOQQ6CQAxF9yTcoWGliTcws1MPIFygyk9EKTPpVBTj4Z2BjSz%2f738v7WF06r1vD2xcp%2bCoNq9yHDFYLIsvvW5Q0JybKYCco2omqnyNTxHW7oPFckbwajFZhB%2bIsE1trNZ0gi1dpuRmQ%2baC%2bjuuthS7Fbwvi%2f%2bP8lpGvAMP7Wr3A6BceSu7AAAA)**\]**

```Kusto
let FloodDataSet = StormEvents
| where EventType == "Flood"
| summarize FloodReports = makeset(StartTime) by State
| project State, FloodReports;
FloodDataSet
| mv-expand FloodReports
```

### <a name="percentiles"></a>yüzdelik()

[**yüzdelik()**](https://docs.microsoft.com/azure/kusto/query/percentiles-aggfunction): Bir ifadeyle tanımlanan popülasyonun belirtilen [**en yakın yüzdelik**](https://docs.microsoft.com/azure/kusto/query/percentiles-aggfunction) yüzdesi için bir tahmin verir. Doğruluk yüzdelik bölgede nüfusun yoğunluğuna bağlıdır. Özetiçinde toplama bağlamında sadece [**kullanılabilir.**](https://docs.microsoft.com/azure/kusto/query/summarizeoperator)

Aşağıdaki sorgu, fırtına süresi için yüzdelik dilimleri hesaplar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUUitKEnNS1FIKS1KLMnMz1OwVXDNSwnJzE1V0FUILkksKgGxQQrLM1KLUhHq7BQMirEI2ygYZ4CEi0tzcxOLMqtSFQpSi5KBlmXmpBZrwJTpKJjqKBgZACkgtgBiS1NNAEC7XiaYAAAA)**\]**

```Kusto
StormEvents
| extend duration = EndTime - StartTime
| where duration > 0s
| where duration < 3h
| summarize percentiles(duration, 5, 20, 50, 80, 95)
```

Aşağıdaki sorgu, fırtına süresi için yüzdelik dilimleri duruma göre hesaplar ve verileri`5m`beş dakikalık kutulara göre normalleştirir ( ).

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1NSwrCMBTcC95hli1EKEpBQd31BHUvafOgAZNI8uIPD28SEBVcDDMM8%2bnZedNdyHKYz56gG5NVUNFL1s5ih86qgzaEBXqWnrPOwetEnj65PZrwx95iNWU7RGOk1w8C5avj6KLlNF64qjHcMWhbvXsCralFPmT6rZ%2fJj2lAnyh8pwWWTaKEdcKmLYul%2fgLODFs%2b4AAAAA%3d%3d)**\]**

```Kusto
StormEvents
| extend duration = EndTime - StartTime
| where duration > 0s
| where duration < 3h
| summarize event_count = count() by bin(duration, 5m), State
| summarize percentiles(duration, 5, 20, 50, 80, 95) by State
```

### <a name="cross-dataset"></a>Çapraz Veri Kümesi

Bu bölüm, daha karmaşık sorgular oluşturmanıza, tablolar arasında veri birleştirmenizi ve veritabanları ve kümeler arasında sorgu yapmanızı sağlayan öğeleri kapsar.

### <a name="let"></a>Izin

[**let**](https://docs.microsoft.com/azure/kusto/query/letstatement): Modülerliği ve yeniden kullanımı artırır. **Let** deyimi, karmaşık olabilecek bir ifadeyi her biri bir ada bağlı olmak üzere birden çok parçaya ayırmanızı ve bu parçaları bir araya getirmenizi sağlar. **İzin** deyimi, kullanıcı tanımlı işlevler ve görünümler (sonuçları yeni bir tablo gibi görünen tablolar üzerindeki ifadeler) oluşturmak için de kullanılabilir. **Let** deyimiyle bağlanan ifadeler skaler tipte, tabular tipte veya kullanıcı tanımlı işlevde (lambdas) olabilir.

Aşağıdaki örnekte bir tabular tür değişkenoluşturur ve sonraki bir ifadede kullanır.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAMtJLVHwyUzPKMnLzEsPLskvyi1WsOXlArNcy1LzSop5uWoUyjNSi1IVwPyQyoJUBVtbBSW4LiVrXq4coDGOZYk5iXnJGakkGQPXBTIGzSUgPVn5mXkKGmhmayrk5ykElySWpIKUpGQWl2TmJZdARACul3kY0gAAAA%3d%3d)**\]**

```Kusto
let LightningStorms =
StormEvents
| where EventType == "Lightning";
let AvalancheStorms =
StormEvents
| where EventType == "Avalanche";
LightningStorms
| join (AvalancheStorms) on State
| distinct State
```

### <a name="join"></a>join

[**join**](https://docs.microsoft.com/azure/kusto/query/joinoperator): Her tablodan belirtilen sütun(lar) değerlerini eşleştirerek yeni bir tablo oluşturmak için iki tablonun satırlarını birleştirin. Kusto birleştirme türleri tam bir dizi destekler: **fullouter**, **iç**, **innerunique**, **leftanti**, **leftantisemi**, **leftouter**, **leftsemi**, **rightanti**, **rightantisemi**, **rightouter**, **rightsemi**.

Aşağıdaki örnek, iç birleştirme ile iki tablo birleştirir.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA8tJLVGIULBVSEksAcKknFQN79RKq+KSosy8dB2FsMSc0lRDq5z8vHRNXq5oXi4FIFBPVNcx1IGyk9R1jJDYxjB2srqOCS9XrDUvVw7Qhkj8Nhih2wA0ydAAySgjZI4xnJMCtMQAYkuEQo1CVn5mnkJ2Zl6KbWZeXmoR0Nb8PAWgZQAFPLdO5AAAAA==)**\]**

```Kusto
let X = datatable(Key:string, Value1:long)
[
    'a',1,
    'b',2,
    'b',3,
    'c',4
];
let Y = datatable(Key:string, Value2:long)
[
    'b',10,
    'c',20,
    'c',30,
    'd',40
];
X 
| join kind=inner Y on Key
```

> [!TIP]
> Birleştirmeden önce giriş tablolarındaki satır ve sütun sayısını azaltmak için **nerede** ve **proje** işleçlerini kullanın. Bir tablo her zaman diğerinden daha küçükse, birleştirmenin sol (borulu) tarafı olarak kullanın. Birleştirme eşleşmesi sütunları aynı ada sahip olmalıdır. Tablolardan birinde bir sütunu yeniden adlandırmak için gerekirse **proje** işlecikullanın.

### <a name="serialize"></a>seri hale

[**serialize**](https://docs.microsoft.com/azure/kusto/query/serializeoperator): Satır kümesini serihale getirir, böylece **row_number()** gibi serileştirilmiş veri gerektiren işlevleri kullanabilirsiniz.

Veriler seri hale getirilen için aşağıdaki sorgu başarılı oldu.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIzi%2fNK9HQVEiqVAguSSxJBcumFmUm5gBlQZzUipLUvBSFovzy%2bLzS3KTUIgVbJI6GJgB4pV4NWgAAAA%3d%3d)**\]**

```Kusto
StormEvents
| summarize count() by State
| serialize
| extend row_number = row_number()
```

Satır kümesi, bir sonucu: **sıralama,** **üst,** veya **aralık** işleçleri, isteğe bağlı **olarak proje**tarafından takip serileştirilmiş olarak kabul edilir , **proje-away**, **genişletmek**, **nerede**, **ayrıştırmak**, **mv-genişletmek,** veya işleçleri **almak.**

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAAsuyS%2fKdS1LzSsp5uWqUSguzc1NLMqsSlVIzi%2fNK9HQVEiqVAguSSxJBcvmF5XABRQSi5NBgqkVJal5KQpF%2beXxeaW5SalFCrZIHA1NAEGimf5iAAAA)**\]**

```Kusto
StormEvents
| summarize count() by State
| sort by State asc
| extend row_number = row_number()
```

### <a name="cross-database-and-cross-cluster-queries"></a>Veritabanı ve çapraz küme sorguları

[Veritabanı ve çapraz küme sorguları](https://docs.microsoft.com/azure/kusto/query/cross-cluster-or-database-queries): Bir veritabanını aynı kümeüzerinde `database("MyDatabase").MyTable`. Uzak bir kümedeki `cluster("MyCluster").database("MyDatabase").MyTable`bir veritabanını .

Aşağıdaki sorgu bir kümeden çağrılır `MyCluster` ve kümeden verileri sorgular. Bu sorguyacak şekilde kendi küme adınızı ve veritabanı adınızı kullanın.

```Kusto
cluster("MyCluster").database("Wiki").PageViews
| where Views < 2000
| take 1000;
```

### <a name="user-analytics"></a>Kullanıcı Analizi

Bu bölümde, Kusto'da kullanıcı davranışlarının çözümlemesi gerçekleştirmenin ne kadar kolay olduğunu gösteren öğeler ve sorgular yer almaktadır.

### <a name="activity_counts_metrics-plugin"></a>activity_counts_metrics eklentisi

[**activity_counts_metrics eklentisi**](https://docs.microsoft.com/azure/kusto/query/activity-counts-metrics-plugin): Yararlı etkinlik ölçümlerini (toplam sayım değerleri, farklı sayım değerleri, farklı değer sayısı ve toplanan farklı sayı) hesaplar. Ölçümler her zaman penceresi için hesaplanır, daha sonra karşılaştırılır ve önceki tüm zaman pencereleriyle toplanır.

Aşağıdaki sorgu, günlük etkinlik sayılarını hesaplayarak kullanıcı benimsenmesini analiz eder.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAJXSPQvCMBAG4L3Q%2f5CtFlLoFyiVDn4M6mqdREpsggTaKs1VEfzxXm0LDiEimcJz3CW8VwogClgDKWcgQFZiEvrB1PNnnh%2b4c9sqsUDUXMPxyA9Z8%2bsjDfhwz0hKsBzPuRSTgxLNlicKGllfKMmwBw6sbsnY0bWto205C4cS3Rso2tpgO4MtDbbSWvixzGD6eb1ttBYZev42%2fbzI8L%2fe9n9b3NkJQ8xs60XEnZUt1hBWgLxLeObFta1B5ZXAKAs1BPuVKO03iXb7gp36tXDfExVB%2f2ICAAA%3d)**\]**

```Kusto
let start=datetime(2017-08-01);
let end=datetime(2017-08-04);
let window=1d;
let T = datatable(UserId:string, Timestamp:datetime)
[
'A', datetime(2017-08-01),
'D', datetime(2017-08-01),
'J', datetime(2017-08-01),
'B', datetime(2017-08-01),
'C', datetime(2017-08-02),
'T', datetime(2017-08-02),
'J', datetime(2017-08-02),
'H', datetime(2017-08-03),
'T', datetime(2017-08-03),
'T', datetime(2017-08-03),
'J', datetime(2017-08-03),
'B', datetime(2017-08-03),
'S', datetime(2017-08-03),
'S', datetime(2017-08-04),
];
T
| evaluate activity_counts_metrics(UserId, Timestamp, start, end,
window)
```

### <a name="activity_engagement-plugin"></a>activity_engagement eklentisi

[**activity_engagement eklentisi**](https://docs.microsoft.com/azure/kusto/query/activity-engagement-plugin): Kayan zaman çizelgesi penceresi üzerindeki ID sütununa göre etkinlik etkileşim oranını hesaplar. **activity_engagement eklentisi** DAU, WAU ve MAU (günlük, haftalık ve aylık aktif kullanıcılar) hesaplamak için kullanılabilir.

Aşağıdaki sorgu, hareketli yedi günlük bir pencerede, haftalık uygulamayı kullanan toplam farklı kullanıcılarla karşılaştırıldığında günlük bir uygulamayı kullanan toplam farklı kullanıcıların oranını döndürür.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1RQWrDMBC8G%2fyHvUVOHGy1lByKD6GBviDkUIoR1tpVsS0jr0MCeXxXiigpVAiBVjOzM6uigHcc0SlCcGrUdgCtSIFtYZnRgWrInA0ZnNOkR4J6JuUIKo9CMgOKp1LutqXknb1GDI76P8RzQHCXDqHW6gqt43ZRkeydNxNOIHWa3AAv5Ctei2xvx06IQNtGTlZInT0AHQN9BpFt5EO59kHmKvQVUUivX8q1y3L4c9%2fIks%2bt5LoMwsMZLxMrgtHVXcb7pOuEthWemEFvBkPARL%2fSpCjgTfXN0vuBHvbH4rQ%2fsikyNjg6q37xL3GsV47cqQ4HHEl8rIxefeZhNHmMmIehsB2dp8nunnZy9hsbiriDWuqTWqpfxdBsLb2ZGzhm8y%2f6b2i%2bWO8HLFcMGe8BAAA%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-01-01);
let _end = datetime(2017-01-31);
range _day from _start to _end step 1d
| extend d = tolong((_day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+100*r-1), 1)
| mv-expand id=_users to typeof(long) limit 1000000
// Calculate DAU/WAU ratio
| evaluate activity_engagement(['id'], _day, _start, _end, 1d, 7d)
| project _day, Dau_Wau=activity_ratio*100
| render timechart
```

> [!TIP]
> DAU/MAU hesaplanırken, son verileri ve hareketli pencere dönemini (OuterActivityWindow) değiştirin.

### <a name="activity_metrics-plugin"></a>activity_metrics eklentisi

[**activity_metrics eklentisi**](https://docs.microsoft.com/azure/kusto/query/activity-metrics-plugin): Geçerli dönem penceresine ve önceki dönem penceresine göre yararlı etkinlik ölçümlerini (farklı sayım değerleri, farklı değer sayısı, farklı değer sayısı, bekletme oranı ve karmaşa hızı) hesaplar.

Aşağıdaki sorgu, belirli bir veri kümesi için karmaşa ve bekletme oranını hesaplar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG2SwW7CMAyG70i8g2%2bk0KoNE%2bIwscsOe4hpqqLGQFjaVKkLVNrDLw7RxjRyqBTr%2fz%2f3t1OW8IYdekUIXnXataAVKXB7GAf0oBoyZ0MGh%2fnMIkE9kPIEO1YhmRbFupLbopJFtc6ekwY7%2fV%2bxKZ4kK0KXA0Kt1QR7H9olIrmbbyDsQer57AvwSlxhFjnruoMQ0VYkT1ZKnd0JfRByBpGt5F255iDDLvYVCaSXm2rpsxz%2b3FfrKnwLGeoygtszXvtABKN3Nwz%2fJ009ur1gYwbWtIZAVvGw53JEn%2fK9PJwSi3rvTthQlOWBPp%2bVJbwq24yWN3FB%2fLQTeAwByLgOeD8x0lnZkRVpL1PdInnTDOJ9YfTiI0%2fE24DyONIctvpB0x94zfBlSJBDcxz97509PgDCM%2bAMzTEgvwEO44wSMAIAAA%3d%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-01-02);
let _end = datetime(2017-05-31);
range _day from _start to _end step 1d
| extend d = tolong((_day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+200*r-1), 1)
| mv-expand id=_users to typeof(long) limit 1000000
| where _day > datetime(2017-01-02)
| project _day, id
// Calculate weekly retention rate
| evaluate activity_metrics(['id'], _day, _start, _end, 7d)
| project _day, retention_rate*100, churn_rate*100
| render timechart
```

### <a name="new_activity_metrics-plugin"></a>new_activity_metrics eklentisi

[**new_activity_metrics eklentisi**](https://docs.microsoft.com/azure/kusto/query/new-activity-metrics-plugin): Yeni kullanıcıların kohortu için yararlı etkinlik ölçümlerini (farklı sayım değerleri, farklı değer sayısı, farklı değer sayısı, bekletme oranı ve çalkalama oranı) hesaplar. Bu eklenti kavramı [**activity_metrics eklentisi**](https://docs.microsoft.com/azure/kusto/query/activity-metrics-plugin)benzer , ama yeni kullanıcılar üzerinde duruluyor.

Aşağıdaki sorgu, yeni kullanıcı kohort (ilk hafta gelen kullanıcılar) için bir hafta boyunca bir hafta boyunca pencere ile bir bekletme ve çalkalama oranı hesaplar.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAG1Ry27DIBC8W%2fI%2f7C04wbJJFeVQ5VapP9BbVVnIrGMaGyy8eVjqxxcwh1QqBx7LzCwzVBW8o0EnCcFJo%2bwISpIE28F1RgeyJX3TpHHOswEJmpmkIzgFFJIeke1rcSzrQ1mL4jVh0Kj%2fEC8R4bucEd7kAp3z3ZIg2ZU2E04gVJ79AD4oVIIU2cGaM2OBVSZKUQlVPOGcxwUHrNiJp3ITbMyn2JUlHbU91FtXcPhz3u1rP5fC10UUHm%2f4mLwiaHVaZcIzaZnQdiwQCxj0qAlEHUeeVRV8yAuCNcMC1CN02s0Ed8QLtLa33igbpK9M0skRCd3q4CaHa%2fgBg%2fcmJb40%2ft7pdmafG602XzxExpN3HsPicFQ8z1IcQWhy9htbisk2EU92XZ1vZkhb04Sv5tD2V7fufwFYtolnAgIAAA%3d%3d)**\]**

```Kusto
// Generate random data of user activities
let _start = datetime(2017-05-01);
let _end = datetime(2017-05-31);
range Day from _start to _end step 1d
| extend d = tolong((Day - _start)/1d)
| extend r = rand()+1
| extend _users=range(tolong(d*50*r), tolong(d*50*r+200*r-1), 1)
| mv-expand id=_users to typeof(long) limit 1000000
// Take only the first week cohort (last parameter)
| evaluate new_activity_metrics(['id'], Day, _start, _end, 7d, _start)
| project from_Day, to_Day, retention_rate, churn_rate
```

### <a name="session_count-plugin"></a>session_count eklentisi

[**session_count eklentisi**](https://docs.microsoft.com/azure/kusto/query/session-count-plugin): Bir zaman çizelgesi üzerinden kimlik sütununa göre oturumların sayısını hesaplar.

Aşağıdaki sorgu oturum sayısını döndürür. Kullanıcı kimliği 100 zamanlı yuvalı bir zaman diliminde en az bir kez görünürse, oturum geri bakış penceresi 41 zamanlı yuvaysa oturum etkin kabul edilir.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAFWPQYvCQAyF74X%2bh3dZUCjYgfUgMkcP3r2XoZPqaM3INK4u7I%2ffzOwiNQRC8pKPl5EEnXfiYJEcHwmHcKUxMGFI8QoDidhoYBK6wdTVD%2bgpxB5dd6FvPSuzcwyMS2BvAzMlLP5gez%2fDrNt%2fCN4Z1iwRua5Kk2GPE6WZkY%2bMsRZt1m4pnqmXl9qouK2r1Qo75cUB5RlPQ%2bAgoWDzpPj%2bcuPdCWGiaVKp6%2bOdZbH3zYxmNFuNUhp8mmU%2bTWpWv8or%2fckl%2bQXutT48NwEAAA%3d%3d)**\]**

```Kusto
let _data = range Timeline from 1 to 9999 step 1
| extend __key = 1
| join kind=inner (range Id from 1 to 50 step 1 | extend __key=1) on __key
| where Timeline % Id == 0
| project Timeline, Id;
// End of data definition
_data
| evaluate session_count(Id, Timeline, 1, 10000, 100, 41)
| render linechart
```

### <a name="funnel_sequence-plugin"></a>funnel_sequence eklentisi

[**funnel_sequence eklentisi**](https://docs.microsoft.com/azure/kusto/query/funnel-sequence-plugin): Bir dizi durum almış kullanıcıların farklı sayısını hesaplar; diziye yol açan veya takip eden önceki ve sonraki durumların dağılımını gösterir.

Aşağıdaki sorgu, 2007'deki tüm Tornado olaylarını önce ve sonra hangi olayın gerçekleştiğini gösterir.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAEAGWOPYvCQBCG%2b0D%2bw3QmEEmieIqNVQrBRgxYiMhcdqKLyWzcnQiCP95V70DuYIrh5Xk%2f0hRWxpw1H8EwbMTYtrgSiwMnKNqJrtw8DNIU1vkcticUOGHXETv4ptpYgtJYRmWAnrbFGx39QbEWsv%2fIj7YwuHsZmx6FoO6ZqTk4uvTEFUVFp51RtFSJH4hWSt1SAsqj4r9olGXTYZb7i5Mw%2bJRnvzLkKhl%2fTXzAq668dc%2bAG2Orq2g3%2bBk22MfxA23MLGQQAQAA)**\]**

```Kusto
// Looking on StormEvents statistics:
// Q1: What happens before Tornado event?
// Q2: What happens after Tornado event?
StormEvents
| evaluate funnel_sequence(EpisodeId, StartTime, datetime(2007-01-01), datetime(2008-01-01), 1d,365d, EventType, dynamic(['Tornado']))
```

### <a name="funnel_sequence_completion-plugin"></a>funnel_sequence_completion eklentisi

[**funnel_sequence_completion eklentisi**](https://docs.microsoft.com/azure/kusto/query/funnel-sequence-completion-plugin): Tamamlanan sıra adımlarının hunisini farklı zaman dilimleri içinde hesaplar.

Aşağıdaki sorgu sıranın tamamlanma hunisini `Hail -> Tornado -> Thunderstorm -> Wind` denetler: bir saat, dört saat ve bir`[1h, 4h, 1d]`gün () "genel" kez.

**\[**[**Sorguyı çalıştırmak için tıklatın**](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA12QTYvCMBCG74L/YW6tkIV2XT9g8SjsnlvwICKhM9JAOqlJqrj4402CW0RIIB/PPLwzmjwcnZfWwwZQevKqo/yzKFYfRRnW7Hs60ZEhxjdi/UZcFaO5VuqPAjhfLvD/w9F5IG7iM95YdqrJ99mPVDoTkNXGskSTju3ASNZ5Y7t43wVhdhj9PVll0L1aylbAV9glJqyKldsLsXfTyR3oIvUQAsNpYCY95jg2puuDUhnOt71yBukXBVRxCnVoTjwnIlLX4rUzAUlf3/pEPYViDDd7AOyqowFQAQAA)**\]**

```Kusto
let _start = datetime(2007-01-01);
let _end = datetime(2008-01-01);
let _windowSize = 365d;
let _sequence = dynamic(['Hail', 'Tornado', 'Thunderstorm', 'Wind']);
let _periods = dynamic([1h, 4h, 1d]);
StormEvents
| evaluate funnel_sequence_completion(EpisodeId, StartTime, _start, _end, _windowSize, EventType, _sequence, _periods)
```

## <a name="functions"></a>İşlevler

Bu bölüm [**işlevleri**](https://docs.microsoft.com/azure/kusto/query/functions)kapsar: sunucuda depolanan yeniden kullanılabilir sorgular. Işlevler sorgular ve diğer işlevler tarafından çağrılabilir (özyinelemeli işlevler desteklenmez).

> [!NOTE]
> Yalnızca okunur olan yardım kümesinde işlevler oluşturamazsınız. Bu bölüm için kendi test kümenizi kullanın.

Aşağıdaki örnek, bir durum adını (`MyState`) bağımsız değişken olarak alan bir işlev oluşturur.

```Kusto
.create function with (folder="Demo")
MyFunction (MyState: string)
{
StormEvents
| where State =~ MyState
}
```

Aşağıdaki örnek, Teksas eyaleti için veri alan bir işlev çağırır.

```Kusto
MyFunction ("Texas")
| summarize count()
```

Aşağıdaki örnek, ilk adımda oluşturulan işlevi siler.

```Kusto
.drop function MyFunction
```

## <a name="next-steps"></a>Sonraki adımlar

[Kusto Sorgu Dili başvurusu](https://aka.ms/kustolangref)
