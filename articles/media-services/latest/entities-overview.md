---
title: Medya Hizmetleri kuruluşlarının filtreleme, sipariş ve sayfalama
titleSuffix: Azure Media Services
description: Azure Media Services v3 varlıklarını filtreleme, sıralama ve sayfalama hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/21/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 7e4f1141a9d4bd58451782e8412063a22565556d
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80584525"
---
# <a name="filtering-ordering-and-paging-of-media-services-entities"></a>Medya Hizmetleri kuruluşlarının filtreleme, sipariş ve sayfalama

Bu konu, Azure Medya Hizmetleri v3 varlıklarını listelediğinizde kullanılabilen OData sorgu seçeneklerini ve pagination desteğini açıklar.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* `Datetime` Türdeki varlıkların özellikleri her zaman UTC biçimindedir.
* Bir istek göndermeden önce sorgu dizesindeki beyaz alan URL kodlanmış olmalıdır.

## <a name="comparison-operators"></a>Karşılaştırma işleçleri

Bir alanı sabit bir değerle karşılaştırmak için aşağıdaki işleçleri kullanabilirsiniz:

Eşitlik operatörleri:

- `eq`: Bir alanın sabit bir *değere eşit* olup olmadığını test edin.
- `ne`: Bir alanın sabit bir *değere eşit olup olmadığını* test edin.

Aralık operatörleri:

- `gt`: Bir alanın sabit bir *değerden büyük* olup olmadığını test edin.
- `lt`: Bir alanın sabit bir *değerden küçük* olup olmadığını test edin.
- `ge`: Bir alanın sabit bir *değerden büyük mü* yoksa eşit mi olduğunu test edin.
- `le`: Bir alanın sabit bir *değerden küçük mü* yoksa eşit mi olduğunu test edin.

## <a name="filter"></a>Filtre

Yalnızca `$filter` ilgilendiğiniz nesneleri bulmak için bir OData filtre parametresi sağlamak için kullanın.

Aşağıdaki REST örneği bir `alternateId` varlığın değerine filtre lenir:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$filter=properties/alternateId%20eq%20'unique identifier'
```

Varlığın oluşturulan tarihinde aşağıdaki C# örneği filtreler:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

## <a name="order-by"></a>Sipariş tarafından

Döndürülen nesneleri belirtilen parametreye göre sıralamak için kullanın. `$orderby` Örnek:  

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01$orderby=properties/created%20gt%202018-05-11T17:39:08.387Z
```

Sonuçları artan veya azalan sırada sıralamak için, `asc` `desc` bir boşlukla ayrılmış alan adını veya öğeadını ekleyerek. Örneğin: `$orderby properties/created desc`.

## <a name="skip-token"></a>Belirteci atla

Sorgu yanıtı çok sayıda öğe içeriyorsa, hizmet bir sonraki sonuç sayfasını almak için kullandığınız bir `$skiptoken` (`@odata.nextLink`) değerini döndürür. Tüm sonuç kümesini sayfalamak için kullanın.

Medya Hizmetleri v3'te sayfa boyutunu yapılandıramazsınız. Sayfa boyutu varlığın türüne göre değişir. Ayrıntılar için takip eden bölümleri tek tek okuyun.

Koleksiyonda çağrı yaparken varlıklar oluşturulur veya silinirse, değişiklikler döndürülen sonuçlara yansıtılır (bu değişiklikler koleksiyonun indirilmemiş kısmındaysa).

> [!TIP]
> Koleksiyonu `nextLink` numaralandırmak için her zaman kullanın ve belirli bir sayfa boyutuna bağlı olmayın.
>
> Değer, `nextLink` yalnızca birden fazla sayfa varlıklar varsa kullanılabilir.

Kullanıldığı yerin `$skiptoken` aşağıdaki örneğini göz önünde bulundurun. *Amstestaccount'u* hesap adınız ile değiştirdiğinizden ve *api sürüm* değerini en son sürüme ayarladığınızdan emin olun.

Bu gibi varlıkların bir listesini isterseniz:

```
GET  https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

Buna benzer bir yanıt alırsınız:

```
HTTP/1.1 200 OK

{
"value":[
{
"name":"Asset 0","id":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 0","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-0000-0000-0000-000000000000","created":"2018-12-11T22:12:44.98Z","lastModified":"2018-12-11T22:15:48.003Z","container":"asset-00000000-0000-0000-0000-0000000000000","storageAccountName":"amsacctname","storageEncryptionFormat":"None"
}
},
// lots more assets
{
"name":"Asset 517","id":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaservices/amstestaccount/assets/Asset 517","type":"Microsoft.Media/mediaservices/assets","properties":{
"assetId":"00000000-0000-0000-0000-000000000000","created":"2018-12-11T22:14:08.473Z","lastModified":"2018-12-11T22:19:29.657Z","container":"asset-00000000-0000-0000-0000-000000000000","storageAccountName":"amsacctname","storageEncryptionFormat":"None"
}
}
],"@odata.nextLink":"https:// management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517"
}
```

Daha sonra aşağıdakiler için bir alma isteği göndererek bir sonraki sayfayı isteyebilirsiniz:

```
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

Aşağıdaki C# örneği, hesaptaki tüm akış bulucuları nasıl numaralandırmayı gösterir.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

## <a name="using-logical-operators-to-combine-query-options"></a>Sorgu seçeneklerini birleştirmek için mantıksal işleçleri kullanma

Medya Hizmetleri v3 **OR** ve **VE** mantıksal operatörleri destekler. 

Aşağıdaki REST örneği işin durumunu denetler:

```
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/qbtest/providers/Microsoft.Media/mediaServices/qbtest/transforms/VideoAnalyzerTransform/jobs?$filter=properties/state%20eq%20Microsoft.Media.JobState'Scheduled'%20or%20properties/state%20eq%20Microsoft.Media.JobState'Processing'&api-version=2018-07-01
```

Aynı sorguyu C#'da şöyle oluşturuyorsunuz: 

```csharp
var odataQuery = new ODataQuery<Job>("properties/state eq Microsoft.Media.JobState'Scheduled' or properties/state eq Microsoft.Media.JobState'Processing'");
client.Jobs.List(config.ResourceGroup, config.AccountName, VideoAnalyzerTransformName, odataQuery);
```

## <a name="filtering-and-ordering-options-of-entities"></a>Varlıkların filtreleme ve sipariş seçenekleri

Aşağıdaki tablo, filtreleme ve sıralama seçeneklerini farklı varlıklara nasıl uygulayabileceğinizi gösterir:

|Varlık adı|Özellik adı|Filtre|Sipariş verme|
|---|---|---|---|
|[Varlıklar](https://docs.microsoft.com/rest/api/media/assets/)|ad|`eq`, `gt`, `lt`, `ge`, `le`|`asc` ve `desc`|
||properties.alternateId |`eq`||
||properties.assetId |`eq`||
||özellikleri.oluşturulan| `eq`, `gt`, `lt`| `asc` ve `desc`|
|[İçerik anahtar ilkeleri](https://docs.microsoft.com/rest/api/media/contentkeypolicies)|ad|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||özellikleri.oluşturulan    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||properties.description    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`||
||properties.lastModi|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||properties.policyId|`eq`, `ne`||
|[İşler](https://docs.microsoft.com/rest/api/media/jobs)| ad  | `eq`            | `asc` ve `desc`|
||properties.state        | `eq`, `ne`        |                         |
||özellikleri.oluşturulan      | `gt`, `ge`, `lt`, `le`| `asc` ve `desc`|
||properties.lastModi | `gt`, `ge`, `lt`, `le` | `asc` ve `desc`| 
|[Akış lı bulucular](https://docs.microsoft.com/rest/api/media/streaminglocators)|ad|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||özellikleri.oluşturulan    |`eq`, `ne`, `ge`, `le`,  `gt`, `lt`|`asc` ve `desc`|
||properties.endTime    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
|[Akış ilkeleri](https://docs.microsoft.com/rest/api/media/streamingpolicies)|ad|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||özellikleri.oluşturulan    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
|[Dönüştürmeler](https://docs.microsoft.com/rest/api/media/transforms)| ad | `eq`            | `asc` ve `desc`|
|| özellikleri.oluşturulan      | `gt`, `ge`, `lt`, `le`| `asc` ve `desc`|
|| properties.lastModi | `gt`, `ge`, `lt`, `le`| `asc` ve `desc`|

## <a name="next-steps"></a>Sonraki adımlar

* [Varlıkları Listele](https://docs.microsoft.com/rest/api/media/assets/list)
* [İçerik Anahtar İlkelerini Listele](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)
* [İş İlanları Listele](https://docs.microsoft.com/rest/api/media/jobs/list)
* [Akış İlkelerini Listele](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)
* [Akış Lı Yer bulucuları listele](https://docs.microsoft.com/rest/api/media/streaminglocators/list)
* [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
* [Kotalar ve sınırlar](limits-quotas-constraints.md)
