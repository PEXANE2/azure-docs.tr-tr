---
title: Azure Media Services REST API'si ile filtreler oluşturuluyor | Microsoft Docs
description: Bu konuda, istemcinizin bir akışın belirli bölümlerini akışa almak için bunları kullanabilmesi için filtrelerin nasıl oluşturulacağı açıklanmaktadır. Media Services, bu seçmeli akışa ulaşmak için dinamik bildirimler oluşturuyor.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: f7d23daf-7cd2-49c7-a195-ab902912ab3c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.reviewr: cenkdin
ms.openlocfilehash: a77da9726c6c081ba5129b9b8815c3ac560e3b54
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89256863"
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a>Azure Media Services REST API'si ile Filtreler oluşturma

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

2,17 sürümünden itibaren, Media Services varlıklarınız için filtreler tanımlamanızı sağlar. Bu filtreler, müşterilerinizin, bir videonun yalnızca bir bölümünü kayıttan yürütme (videonun tamamını çalmak yerine) veya yalnızca müşterinizin cihazının işleyebileceği ses ve video çevirilerinin bir alt kümesini (varlıkla ilişkili tüm yorumlamalar yerine) belirtmek için bir sunucu tarafı kurallarıdır. Varlıklarınızın bu şekilde filtrelenmesi, belirtilen filtre (ler) i temel alarak bir videoyu akışa almak için müşterinizin isteğiyle oluşturulan **dinamik bildirim**aracılığıyla arşivlenir.

Filtreler ve dinamik bildirimle ilgili daha ayrıntılı bilgi için bkz. [dinamik bildirimlere genel bakış](media-services-dynamic-manifest-overview.md).

Bu makalede, filtre oluşturmak, güncelleştirmek ve silmek için REST API 'Lerinin nasıl kullanılacağı gösterilmektedir. 

## <a name="types-used-to-create-filters"></a>Filtre oluşturmak için kullanılan türler
Şu türler, filtre oluştururken kullanılır:  

* [Filtrele](/rest/api/media/operations/filter)
* [AssetFilter](/rest/api/media/operations/assetfilter)
* [PresentationTimeRange](/rest/api/media/operations/presentationtimerange)
* [FilterTrackSelect ve FilterTrackPropertyCondition](/rest/api/media/operations/filtertrackselect)

> [!NOTE]
> 
> Media Services varlıklara erişirken, HTTP isteklerinizin belirli üstbilgi alanlarını ve değerlerini ayarlamanız gerekir. Daha fazla bilgi için bkz. [Media Services REST API Geliştirme Için kurulum](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API 'sine bağlanma hakkında daha fazla bilgi için bkz. [Azure AD kimlik doğrulamasıyla Azure MEDIA SERVICES API 'Sine erişme](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="create-filters"></a>Filtre oluşturma
### <a name="create-global-filters"></a>Genel filtreler oluşturma
Genel bir filtre oluşturmak için aşağıdaki HTTP isteklerini kullanın:  

#### <a name="http-request"></a>HTTP İsteği
İstek üst bilgileri

```console
POST https://media.windows.net/API/Filters HTTP/1.1 
DataServiceVersion:3.0 
MaxDataServiceVersion: 3.0 
Content-Type: application/json 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN> 
x-ms-version: 2.19 
x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
Host:media.windows.net 
```

İstek gövdesi 

```console
{  
   "Name":"GlobalFilter",
   "PresentationTimeRange":{  
      "StartTimestamp":"0",
      "EndTimestamp":"9223372036854775807",
      "PresentationWindowDuration":"12000000000",
      "LiveBackoffDuration":"0",
      "Timescale":"10000000"
   },
   "Tracks":[  
      {  
         "PropertyConditions":
              [  
            {  
               "Property":"Type",
               "Value":"audio",
               "Operator":"Equal"
            },
            {  
               "Property":"Bitrate",
               "Value":"0-2147483647",
               "Operator":"Equal"
            }
         ]
      }
   ]
}
```



#### <a name="http-response"></a>HTTP yanıtı

```console
HTTP/1.1 201 Created 
```

### <a name="create-local-assetfilters"></a>Yerel AssetFilters oluşturma
Yerel bir AssetFilter oluşturmak için aşağıdaki HTTP isteklerini kullanın:  

#### <a name="http-request"></a>HTTP İsteği
İstek üst bilgileri

```console
POST https://media.windows.net/API/AssetFilters HTTP/1.1 
DataServiceVersion: 3.0 
MaxDataServiceVersion: 3.0 
Content-Type: application/json 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN> 
x-ms-version: 2.19 
x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
Host: media.windows.net  
```

İstek gövdesi 

```console
{   
   "Name":"AssetFilter", 
   "ParentAssetId":"nb:cid:UUID:536e555d-1500-80c3-92dc-f1e4fdc6c592", 
   "PresentationTimeRange":{   
      "StartTimestamp":"0", 
      "EndTimestamp":"9223372036854775807", 
      "PresentationWindowDuration":"12000000000", 
      "LiveBackoffDuration":"0", 
      "Timescale":"10000000" 
   }, 
   "Tracks":[   
      {   
         "PropertyConditions": 
              [   
            {   
               "Property":"Type", 
               "Value":"audio", 
               "Operator":"Equal" 
            }, 
            {   
               "Property":"Bitrate", 
               "Value":"0-2147483647", 
               "Operator":"Equal" 
            } 
         ] 
      } 
   ] 
} 
```

#### <a name="http-response"></a>HTTP yanıtı

```console
HTTP/1.1 201 Created 
. . . 
```

## <a name="list-filters"></a>Liste filtreleri
### <a name="get-all-global-filters-in-the-ams-account"></a>AMS hesabındaki tüm genel **filtreleri**al
Filtreleri listelemek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP İsteği

```console
GET https://media.windows.net/API/Filters HTTP/1.1 
DataServiceVersion:3.0 
MaxDataServiceVersion: 3.0 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN> 
x-ms-version: 2.19 
Host: media.windows.net 
```

### <a name="get-assetfilters-associated-with-an-asset"></a>Bir varlıkla ilişkili **Assetfilter**'ları al
#### <a name="http-request"></a>HTTP İsteği

```console
GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
DataServiceVersion: 3.0 
MaxDataServiceVersion: 3.0 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN> 
x-ms-version: 2.19 
x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
Host: media.windows.net 
```

### <a name="get-an-assetfilter-based-on-its-id"></a>Kimliğine göre bir **Assetfiltresi** al
#### <a name="http-request"></a>HTTP İsteği

```console
GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
DataServiceVersion: 3.0 
MaxDataServiceVersion: 3.0 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN> 
x-ms-version: 2.19 
x-ms-client-request-id: 00000000
```

## <a name="update-filters"></a>Filtreleri Güncelleştir
Yeni özellik değerleriyle bir filtreyi güncelleştirmek için PATCH, PUT veya MERGE kullanın.  Bu işlemler hakkında daha fazla bilgi için bkz. [Patch, put, Merge](/openspecs/windows_protocols/ms-odata/59d5abd3-7b12-490a-a0e2-9d9324b91893).

Bir filtreyi güncelleştirirseniz akış uç noktasının kuralların yenilenmesi iki dakika kadar sürebilir. İçerik bu filtre kullanılarak sunulduysa (ve proxy 'lerde ve CDN önbelleklerde önbelleğe alınmışsa), bu filtrenin güncelleştirilmesi oynatıcı hatalara neden olabilir. Filtreyi güncelleştirdikten sonra önbelleğin işaretini kaldırın. Bu seçenek mümkün değilse, farklı bir filtre kullanmayı düşünün.  

### <a name="update-global-filters"></a>Genel filtreleri Güncelleştir
Genel bir filtreyi güncelleştirmek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP İsteği
İstek üst bilgileri: 

```console
MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
DataServiceVersion:3.0 
MaxDataServiceVersion: 3.0 
Content-Type: application/json 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN> 
x-ms-version: 2.19 
x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
Host: media.windows.net 
Content-Length: 384
```

İstek gövdesi: 

```console
{ 
   "Tracks":[   
      {   
         "PropertyConditions": 
         [   
            {   
               "Property":"Type", 
               "Value":"audio", 
               "Operator":"Equal" 
            }, 
            {   
               "Property":"Bitrate", 
               "Value":"0-2147483647", 
               "Operator":"Equal" 
            } 
         ] 
      } 
   ] 
} 
```

### <a name="update-local-assetfilters"></a>Yerel Assetfiltrelerini Güncelleştir
Yerel bir filtreyi güncelleştirmek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP İsteği
İstek üst bilgileri: 

```console
MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
DataServiceVersion: 3.0 
MaxDataServiceVersion: 3.0 
Content-Type: application/json 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN> 
x-ms-version: 2.19 
x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
Host: media.windows.net 
```

İstek gövdesi: 

```console
{ 
   "Tracks":[   
      {   
         "PropertyConditions": 
         [   
            {   
               "Property":"Type", 
               "Value":"audio", 
               "Operator":"Equal" 
            }, 
            {   
               "Property":"Bitrate", 
               "Value":"0-2147483647", 
               "Operator":"Equal" 
            } 
         ] 
      } 
   ] 
} 
```

## <a name="delete-filters"></a>Filtreleri Sil
### <a name="delete-global-filters"></a>Genel filtreleri Sil
Genel bir filtreyi silmek için aşağıdaki HTTP isteklerini kullanın:

#### <a name="http-request"></a>HTTP İsteği

```console
DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
DataServiceVersion:3.0 
MaxDataServiceVersion: 3.0 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN>  
x-ms-version: 2.19 
Host: media.windows.net 
```

### <a name="delete-local-assetfilters"></a>Yerel Assetfiltrelerini Sil
Yerel bir AssetFilter 'ı silmek için aşağıdaki HTTP isteklerini kullanın:

#### <a name="http-request"></a>HTTP İsteği

```console
DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
DataServiceVersion: 3.0 
MaxDataServiceVersion: 3.0 
Accept: application/json 
Accept-Charset: UTF-8 
Authorization: Bearer <ENCODED JWT TOKEN> 
x-ms-version: 2.19 
Host: media.windows.net 
```

## <a name="build-streaming-urls-that-use-filters"></a>Filtreler kullanan akış URL 'Leri oluşturma
Varlıklarınızı yayımlama ve teslim etme hakkında daha fazla bilgi için bkz. [müşterilere Içerik sunma genel bakış](media-services-deliver-content-overview.md).

Aşağıdaki örneklerde, akış URL 'Lerine filtrelerin nasıl ekleneceği gösterilmektedir.

**MPEG DASH** 

`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)`

**Apple HTTP Canlı Akışı (HLS) v4**

`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)`

**Apple HTTP Canlı Akışı (HLS) v3**

`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)`

**Kesintisiz Akış**

`http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)`

    
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Dinamik bildirimlere genel bakış](media-services-dynamic-manifest-overview.md)
