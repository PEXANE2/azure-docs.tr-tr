---
title: Azure Medya Hizmetleri YLE Filtre Oluşturma REST API | Microsoft Dokümanlar
description: Bu konu, istemcinizin bir akışın belirli bölümlerini akışı için bunları kullanabilmesi için filtrelerin nasıl oluşturulabileceğini açıklar. Medya Hizmetleri, bu seçici akışı elde etmek için dinamik bildirimler oluşturur.
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
ms.openlocfilehash: b778ad8c59cf51f92584cd3590f7d99244f37b2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76774947"
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a>Azure Medya Hizmetleri YLE Filtre Oluşturma REST API 
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [Geri kalanı](media-services-rest-dynamic-manifest.md)
> 
> 

2.17 sürümüyle başlayarak, Medya Hizmetleri varlıklarınız için filtreler tanımlamanıza olanak tanır. Bu filtreler, müşterilerinizin aşağıdaki gibi şeyler yapmayı seçmelerine olanak tanıyan sunucu tarafı kurallarıdır: videonun yalnızca bir bölümünü oynatma (videonun tamamını oynatmak yerine) veya yalnızca müşterinizin cihazının işleyebilir ses ve video yorumlamalarının bir alt kümesini belirtin (bunun yerine varlıkla ilişkili tüm yorumlamalar). Varlıklarınızın bu filtrelemi, müşterinizin belirtilen filtre(ler)e dayalı bir video akışı isteği üzerine oluşturulan **Dinamik Bildirim**ler aracılığıyla arşivlenir.

Filtreler ve Dinamik Bildirim ile ilgili daha ayrıntılı bilgi için [Dinamik bildirimlere genel bakış](media-services-dynamic-manifest-overview.md)bakın.

Bu makalede, filtreler oluşturmak, güncelleştirmek ve silmek için REST API'lerinin nasıl kullanılacağı gösterilmektedir. 

## <a name="types-used-to-create-filters"></a>Filtre oluşturmak için kullanılan türler
Filtreler oluşturulurken aşağıdaki türler kullanılır:  

* [Filtrele](https://docs.microsoft.com/rest/api/media/operations/filter)
* [Varlık Filtresi](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [SunumZaman Aralığı](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [FilterTrackSelect ve FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

> [!NOTE]
> 
> Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın.

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

## <a name="create-filters"></a>Filtre oluşturma
### <a name="create-global-filters"></a>Genel Filtreler Oluşturma
Genel bir Filtre oluşturmak için aşağıdaki HTTP isteklerini kullanın:  

#### <a name="http-request"></a>HTTP İsteği
İstek Üst Bilgileri

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

İstek gövdesi 

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




#### <a name="http-response"></a>HTTP Yanıt
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a>Yerel Varlık Filtreleri Oluşturma
Yerel bir AssetFilter oluşturmak için aşağıdaki HTTP isteklerini kullanın:  

#### <a name="http-request"></a>HTTP İsteği
İstek Üst Bilgileri

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

İstek gövdesi 

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

#### <a name="http-response"></a>HTTP Yanıt
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a>Liste filtreleri
### <a name="get-all-global-filters-in-the-ams-account"></a>TÜM global **Filtreleri**AMS hesabına alın
Filtreleri listelemek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP İsteği
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a>**Varlık Filtresi'ni**bir varlıkla ilişkilendirme
#### <a name="http-request"></a>HTTP İsteği
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a>Kimliğine göre bir **Varlık Filtresi** alın
#### <a name="http-request"></a>HTTP İsteği
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a>Filtreleri güncelleştir
Yeni özellik değerleri olan bir filtreyi güncelleştirmek için YAMA, PUT veya MERGE'yi kullanın.  Bu işlemler hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/dd541276.aspx)

Bir filtreyi güncellerseniz, akış bitiş noktasının kuralları yenilemesi iki dakika kadar sürebilir. İçerik bu filtre kullanılarak sunulduysa (ve yakınlıkve CDN önbelleklerinde önbelleğe alınmışsa), bu filtreyi güncelleştirmek oynatıcı hatalarına neden olabilir. Filtreyi güncelledikten sonra önbelleği temizleyin. Bu seçenek mümkün değilse, farklı bir filtre kullanmayı düşünün.  

### <a name="update-global-filters"></a>Genel Filtreleri Güncelleştir
Genel bir filtreyi güncelleştirmek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP İsteği
İstek üstleri: 

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

İstek gövdesi: 

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

### <a name="update-local-assetfilters"></a>Yerel Varlık Filtrelerini Güncelleştirme
Yerel bir filtreyi güncelleştirmek için aşağıdaki HTTP isteklerini kullanın: 

#### <a name="http-request"></a>HTTP İsteği
İstek üstleri: 

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

İstek gövdesi: 

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


## <a name="delete-filters"></a>Filtreleri silme
### <a name="delete-global-filters"></a>Genel Filtreleri Silme
Genel bir Filtreyi silmek için aşağıdaki HTTP isteklerini kullanın:

#### <a name="http-request"></a>HTTP İsteği
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN>  
    x-ms-version: 2.19 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a>Yerel Varlık Filtrelerini Silme
Yerel bir AssetFilter'i silmek için aşağıdaki HTTP isteklerini kullanın:

#### <a name="http-request"></a>HTTP İsteği
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19 
    Host: media.windows.net 

## <a name="build-streaming-urls-that-use-filters"></a>Filtreler kullanan akış URL'leri oluşturma
Varlıklarınızın nasıl yayımlayacağı ve teslim edilen hakkında bilgi için [bkz.](media-services-deliver-content-overview.md)

Aşağıdaki örnekler, akış URL'lerinize nasıl filtre ekleyeceğinizgösterilmektedir.

**MPEG DASH** 

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Canlı Akış (HLS) V4**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Canlı Akış (HLS) V3**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Kesintisiz Akış**

    http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)

    
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Dinamik manifestolara genel bakış](media-services-dynamic-manifest-overview.md)

