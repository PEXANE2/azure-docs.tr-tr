---
title: Filtreleme, sıralama, Media Services varlıkların sayfalanması-Azure | Microsoft Docs
description: Bu makalede, Azure Media Services varlıkların filtrelenmesini, sıralanmasını ve sayfalama işlemlerini ele alınmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/11/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ed509ac8fea43a9c011bbbf76c1dc433cd78d43c
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72298957"
---
# <a name="filtering-ordering-paging-of-media-services-entities"></a>Media Services varlıkların filtrelenmesi, sıralanması, sayfalama

Bu konuda, Azure Media Services v3 varlıklarının listelenmesi sırasında kullanılabilen OData sorgu seçenekleri ve sayfalandırma desteği ele alınmaktadır.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Tarih saat türünde varlıkların özellikleri her zaman UTC biçimindedir.
* Sorgu dizesindeki boşluk, istek gönderilmeden önce URL kodlamalı olmalıdır.

## <a name="comparison-operators"></a>Karşılaştırma işleçleri

Bir alanı sabit değerle karşılaştırmak için aşağıdaki işleçleri kullanabilirsiniz:

Eşitlik işleçleri:

- `eq`: bir alanın sabit değere **eşit** olup olmadığını test edin
- `ne`: bir alanın sabit değere **eşit** olup olmadığını test edin

Aralık işleçleri:

- `gt`: bir alanın sabit değerden **büyük** olup olmadığını test edin
- `lt`: bir alanın sabit değerden **küçük** olup olmadığını test edin
- `ge`: bir alanın sabit değere **eşit veya ondan büyük** olup olmadığını test edin
- `le`: bir alanın sabit değere **eşit veya ondan küçük** olup olmadığını test edin

## <a name="filter"></a>Filtrele

**$Filter** -yalnızca ilgilendiğiniz nesneleri bulmak Için bir OData filtre parametresi sağlamak üzere filtre kullanın.

Aşağıdaki REST örneği bir varlığın AlternateId 'Si üzerinde filtreliyor:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$filter=properties/alternateId%20eq%20'unique identifier'
```

Aşağıdaki C# örnek, varlığın oluşturulma tarihinde filtreliyor:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```    

## <a name="order-by"></a>Sıralama ölçütü

**$OrderBy** , döndürülen nesneleri belirtilen parametreye göre sıralamak için bunu kullanın. Örnek:    

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01$orderby=properties/created%20gt%202018-05-11T17:39:08.387Z
```

Sonuçları artan veya azalan düzende sıralamak için, alan adına `asc` veya `desc` ' i boşlukla ayırarak ekleyin. Örneğin, `$orderby properties/created desc`.

## <a name="skip-token"></a>Belirteci atla

**$skiptoken** -bir sorgu yanıtı çok sayıda öğe içeriyorsa, hizmet bir sonraki sonuç sayfasını elde etmek için kullandığınız bir atlama belirteci (`@odata.nextLink`) değeri döndürür. Bu, tüm sonuç kümesi boyunca sayfa eklemek için kullanılabilir.

V3 Media Services, sayfa boyutunu yapılandıramazsınız. Sayfa boyutu varlık türüne göre farklılık gösterir, lütfen ayrıntılar için izleyen bölümleri okuyun.

Koleksiyon üzerinden sayfalama sırasında varlıklar oluşturulursa veya silinirse, değişiklikler döndürülen sonuçlara yansıtılır (Bu değişiklikler indirilmemiş koleksiyonun parçasıysa). 

> [!TIP]
> Koleksiyonu numaralandırmak ve belirli bir sayfa boyutuna bağlı olmaması için her zaman `nextLink` kullanmanız gerekir.
>
> @No__t-0 yalnızca birden fazla varlık sayfası varsa mevcut olacaktır.

$Skiptoken kullanıldığı yerde aşağıdaki örneği göz önünde bulundurun. *Amstestaccount* değerini hesap adınızla değiştirdiğinizden ve *api sürümü* değerini en son sürüme ayarladığınızdan emin olun.

Aşağıdaki gibi varlıkların bir listesini istemeniz durumunda:

```
GET  https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01 HTTP/1.1
x-ms-client-request-id: dd57fe5d-f3be-4724-8553-4ceb1dbe5aab
Content-Type: application/json; charset=utf-8
```

Aşağıdakine benzer bir yanıt geri alacaksınız:

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

Ardından, için bir get isteği göndererek sonraki sayfayı isteyebilirsiniz:

```
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mediaresources/providers/Microsoft.Media/mediaServices/amstestaccount/assets?api-version=2018-07-01&$skiptoken=Asset+517
```

Aşağıdaki C# örnek, hesaptaki tüm akış konum belirleyicilerinin nasıl numaralandırılacağını gösterir.

```csharp
var firstPage = await MediaServicesArmClient.StreamingLocators.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingLocators.ListNextAsync(currentPage.NextPageLink);
}
```

## <a name="using-logical-operators-to-combine-query-options"></a>Sorgu seçeneklerini birleştirmek için mantıksal işleçler kullanma

Media Services v3, ' veya ' ve ' ve ' mantıksal işleçlerini destekler. 

Aşağıdaki REST örneği işin durumunu denetler:

```
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/qbtest/providers/Microsoft.Media/mediaServices/qbtest/transforms/VideoAnalyzerTransform/jobs?$filter=properties/state%20eq%20Microsoft.Media.JobState'Scheduled'%20or%20properties/state%20eq%20Microsoft.Media.JobState'Processing'&api-version=2018-07-01
```

Aynı sorguyu C# şöyle oluşturursunuz: 

```csharp
var odataQuery = new ODataQuery<Job>("properties/state eq Microsoft.Media.JobState'Scheduled' or properties/state eq Microsoft.Media.JobState'Processing'");
client.Jobs.List(config.ResourceGroup, config.AccountName, VideoAnalyzerTransformName, odataQuery);
```

## <a name="filtering-and-ordering-options-of-entities"></a>Varlıkların filtreleme ve sıralama seçenekleri

Aşağıdaki tabloda filtreleme ve sıralama seçeneklerinin farklı varlıklara nasıl uygulanabileceğini gösterilmektedir:

|Varlık adı|Özellik adı|Filtrele|Sipariş|
|---|---|---|---|
|[Varlıklar](https://docs.microsoft.com/rest/api/media/assets/)|ad|`eq`, `gt`, `lt`, `ge`, `le`|`asc` ve `desc`|
||Properties. AlternateId |`eq`||
||Properties. assetId |`eq`||
||Özellikler. oluşturuldu| `eq`, `gt`, `lt`| `asc` ve `desc`|
|[İçerik anahtarı ilkeleri](https://docs.microsoft.com/rest/api/media/contentkeypolicies)|ad|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||Özellikler. oluşturuldu    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||Properties. Description    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`||
||Properties. lastModified|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||Properties. PolicyId|`eq`, `ne`||
|[İşler](https://docs.microsoft.com/rest/api/media/jobs)| ad  | `eq`            | `asc` ve `desc`|
||Properties. State        | `eq`, `ne`        |                         |
||Özellikler. oluşturuldu      | `gt`, `ge`, `lt`, `le`| `asc` ve `desc`|
||Properties. lastModified | `gt`, `ge`, `lt`, `le` | `asc` ve `desc`| 
|[Akış Konumlandırıcı](https://docs.microsoft.com/rest/api/media/streaminglocators)|ad|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||Özellikler. oluşturuldu    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||Properties. BitişZamanı    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
|[Akış ilkeleri](https://docs.microsoft.com/rest/api/media/streamingpolicies)|ad|`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
||Özellikler. oluşturuldu    |`eq`, `ne`, `ge`, `le`, `gt`, `lt`|`asc` ve `desc`|
|[Dönüşümler](https://docs.microsoft.com/rest/api/media/transforms)| ad | `eq`            | `asc` ve `desc`|
|| Özellikler. oluşturuldu      | `gt`, `ge`, `lt`, `le`| `asc` ve `desc`|
|| Properties. lastModified | `gt`, `ge`, `lt`, `le`| `asc` ve `desc`|

## <a name="next-steps"></a>Sonraki adımlar

* [Varlıkları Listele](https://docs.microsoft.com/rest/api/media/assets/list)
* [Içerik anahtarı Ilkelerini Listele](https://docs.microsoft.com/rest/api/media/contentkeypolicies/list)
* [Işleri listeleme](https://docs.microsoft.com/rest/api/media/jobs/list)
* [Akış Ilkelerini Listele](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)
* [Akış Konumlandırıcı listesini Listele](https://docs.microsoft.com/rest/api/media/streaminglocators/list)
* [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
* [Kotalar ve sınırlamalar](limits-quotas-constraints.md)
