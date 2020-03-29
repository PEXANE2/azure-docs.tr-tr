---
title: Bing Resim Arama API v5'ten v7'ye yükseltme
titleSuffix: Azure Cognitive Services
description: Bu yükseltme kılavuzu, Bing Resim Arama API'sının sürüm 5 ve sürüm 7 arasındaki değişiklikleri açıklar. Sürüm 7'yi kullanmak üzere güncelleştirmeniz gereken uygulamanızın bölümlerini belirlemenize yardımcı olmak için bu kılavuzu kullanın.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 7F78B91F-F13B-40A4-B8A7-770FDB793F0F
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: scottwhi
ms.openlocfilehash: c4c6b95996206cfb38ea3f77b89c3ebe3c2c0026
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68883499"
---
# <a name="bing-image-search-api-v7-upgrade-guide"></a>Bing Resim Arama API v7 yükseltme kılavuzu

Bu yükseltme kılavuzu, Bing Resim Arama API'sının sürüm 5 ve sürüm 7 arasındaki değişiklikleri tanımlar. Sürüm 7'yi kullanmak üzere güncelleştirmeniz gereken uygulamanızın bölümlerini belirlemenize yardımcı olmak için bu kılavuzu kullanın.

## <a name="breaking-changes"></a>Yeni değişiklikler

### <a name="endpoints"></a>Uç Noktalar

- Bitiş noktasının sürüm numarası v5'ten v7'ye değiştirildi. Örneğin, https:\//api.cognitive.microsoft.com/bing/\*\*v7.0**/images/search.

### <a name="error-response-objects-and-error-codes"></a>Hata yanıt nesneleri ve hata kodları

- Tüm başarısız istekleri artık `ErrorResponse` yanıt gövdesinde bir nesne içermelidir.

- Nesneye `Error` aşağıdaki alanlar eklendi.  
  - `subCode`&mdash;Mümkünse hata kodunu ayrık kovalara bölümler
  - `moreDetails`&mdash;`message` Alanda açıklanan hata hakkında ek bilgi


- V5 hata kodlarını aşağıdaki olası `code` `subCode` ve değerlerle değiştirdi.

|Kod|Subcode|Açıklama
|-|-|-
|SunucuHatası|Beklenmeyen Hata<br/>Kaynak Hatası<br/>Uygulanmadı|Bing, alt kod koşullarından herhangi biri oluştuğunda ServerError'ı döndürür. HTTP durum kodu 500 ise yanıt bu hataları içerir.
|Geçersizİstek|ParametreEksik<br/>ParametreGeçersiz Değer<br/>httpİzin verilen<br/>Engellendi|Bing, isteğin herhangi bir bölümü geçerli olmadığında Geçersiz İstek'i döndürür. Örneğin, gerekli bir parametre eksik veya bir parametre değeri geçerli değil.<br/><br/>Hata ParameterMissing veya ParameterGeçersizDeğer ise, HTTP durum kodu 400'dür.<br/><br/>Hata httpNotAllowed ise, HTTP durum kodu 410.
|RateLimitAşıldı||Bing, sorgularınızı saniyede (QPS) veya aylık sorgularınızı (QPM) kotanızı aştığınızda RateLimitExceeded'i döndürür.<br/><br/>QPS'yi aştıysanız Bing, HTTP durum kodu 429'u ve QPM'yi aştıysanız 403'ü döndürür.
|Geçersiz Yetkilendirme|Yetkilendirme Eksik<br/>YetkilendirmeRedundancy|Bing, arayan kişinin kimliğini doğrulayamıyorsa Geçersiz Yetkilendirme'yi döndürür. Örneğin, `Ocp-Apim-Subscription-Key` üstbilgi eksik veya abonelik anahtarı geçerli değil.<br/><br/>Birden fazla kimlik doğrulama yöntemi belirtirseniz artıklık oluşur.<br/><br/>Hata Geçersiz Yetkilendirme ise, HTTP durum kodu 401'dir.
|Yetersiz Yetkilendirme|YetkilendirmeDevre Dışı<br/>Yetkilendirme Süresi Doldu|Arayan kaynağa erişmek için izinleri yoksa Bing Yetersiz Yetkilendirme döndürür. Bu, abonelik anahtarı devre dışı bırakılmışsa veya süresi dolmuşsa oluşabilir. <br/><br/>Hata Yetersiz Yetkilendirme ise, HTTP durum kodu 403'dür.

- Aşağıdaki önceki hata kodlarını yeni kodlara eşler. v5 hata kodlarına bağımlı bir şekilde ele geçirdiyseniz, kodunuzu buna göre güncelleştirin.

|Sürüm 5 kodu|Sürüm 7 code.subCode
|-|-
|İstekParametreEksik|Geçersizİstek.ParametreEksik
İstekParametreGeçersiz Değer|Geçersizİstek.ParametreGeçersiz Değer
Kaynak Erişim Reddedildi|Yetersiz Yetkilendirme
Aşan Hacim|RateLimitAşıldı
AşıldıQpsLimit|RateLimitAşıldı
Devre dışı|Yetersiz Yetkilendirme.YetkilendirmeDevre Dışı
Beklenmeyen Hata|ServerError.unexpectedError
DataSourceErrors|ServerError.ResourceError
Yetkilendirme Eksik|Geçersiz Yetkilendirme.YetkilendirmeEksik
httpİzin verilen|Geçersizİstek.HttpNotAllowed
UserAgentEksik|Geçersizİstek.ParametreEksik
Uygulanmadı|ServerError.NotImplemented
Geçersiz Yetkilendirme|Geçersiz Yetkilendirme
Geçersiz Yetkilendirme Yöntemi|Geçersiz Yetkilendirme
Çoklu Yetkilendirme Yöntemi|GeçersizYetkilendirme.YetkilendirmeReduncy
Süresi Dolmuş YetkilendirmeToken|Yetersiz Yetkilendirme.Yetkilendirme Süresi Doldu
Yetersiz Kapsam|Yetersiz Yetkilendirme
Engellendi|Geçersizİstek.Engellendi



### <a name="query-parameters"></a>Sorgu parametreleri

- Sorgu parametresini `modulesRequested` [modüllere](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)yeniden adlandırın.  

- Ek Açıklamalar Etiketlere Yeniden Adlandırıldı. Etiketler [için modülsorgu](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) parametrebakın.  

- ShoppingSources filtre değerinin desteklenen pazarlarının listesini yalnızca EN-US olarak değiştirildi. Bkz. [imageType](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype).  


### <a name="image-insights-changes"></a>Görüntü öngörüleri değişiklikleri

- `annotations` [ImagesInsights](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse) alanının adı `imageTags`.' olarak değiştirildi.  

- Nesnenin `AnnotationModule` adının [ImageTagsModule](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetagsmodule)olduğu.  

- Nesneye `Annotation` [Etiket](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#tag)olarak yeniden adlandırıldı `confidence` ve alanı kaldırdı.  

- `insightsSourcesSummary` [Görüntü](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image) nesnesinin alanının adı `insightsMetadata`.  

- Nesnenin `InsightsSourcesSummary` adı [InsightsMetadata](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightsmetadata)olarak değiştirildi.  

- Bitiş `https://api.cognitive.microsoft.com/bing/v7.0/images/details` noktası eklendi. /images/search bitiş noktası yerine görüntü öngörüleri istemek için bu bitiş noktasını kullanın. [Bkz. Resim İstatistikleri](./image-insights.md).

- Aşağıdaki sorgu parametreleri artık yalnızca `/images/details` bitiş noktasıyla geçerlidir.  

    -   [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken)  
    -   [Modül](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)  
    -   [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imgurl)  
    -   [Taksi](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cab)  
    -   [Cal](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cal)  
    -   [car](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#car)  
    -   [Kedi](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#cat)  
    -   [Ct](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#ct)  

- Nesnenin `ImageInsightsResponse` adı [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsights)olarak değiştirildi.  

- [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsights) nesnesinde aşağıdaki alanların veri türlerini değiştirdi.  

    -   `relatedCollections` Alanın türünü `ImageGallery[]` [RelatedCollectionsModule](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#relatedcollectionsmodule)olarak değiştirildi.  

    -   `pagesIncluding` Alanın türünü [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagesmodule) `Image[]` olarak değiştirildi.  

    -   `relatedSearches` Alanın türünü `Query[]` [RelatedSearchesModule](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#relatedsearchesmodule)olarak değiştirildi.  

    -   `recipes` Alanın türünü `Recipe[]` [TariflerModülüne](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#recipesmodule)değiştirdi.  

    -   `visuallySimilarImages` Alanın türünü [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagesmodule) `Image[]` olarak değiştirildi.  

    -   `visuallySimilarProducts` Alanın türünü [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagesmodule) `ProductSummaryImage[]` olarak değiştirildi.  

    -   Nesneyi `ProductSummaryImage` kaldırdı ve ürünle ilgili alanları [Görüntü](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#image) nesnesine taşıdı. Nesne, `Image` yalnızca görüntü görüntü içgörü yanıtında görsel olarak benzer ürünlerin bir parçası olarak dahil edildiğinde ürünle ilgili alanları içerir.  

    -   `recognizedEntityGroups` Alanın türünü Tanınan Varlıklar `RecognizedEntityGroup[]` [Modülüne](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#recognizedentitiesmodule)değiştirildi.  

-   `categoryClassification` [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse) alanının adı `annotations`'' olarak değiştirildi `AnnotationsModule`ve türünü .  

### <a name="images-answer"></a>Görüntüler yanıt

-   [Görüntülerden](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images)ekranAlKaynaklarıRozetleri ve ekranLarıRecipeSourcesBadges alanları kaldırıldı .  

-   `nextOffsetAddCount` [Görüntüler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) alanının adı `nextOffset`. Ofset kullanma şekliniz de değişti. Daha önce, [mahsup](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#offset) sorgu parametresini `nextOffsetAddCount` değere ve önceki mahsup değerine ve sonuçtaki görüntü sayısına ayarlarsınız. Şimdi, `nextOffset` değeri `offset` ayarlayın.  


## <a name="non-breaking-changes"></a>Kırılmayan değişiklikler

### <a name="query-parameters"></a>Sorgu parametreleri

- Olası bir görüntü Olarak Saydam eklendiFiltre değeri [yazın.](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagetype) Saydam filtre yalnızca saydam arka plana sahip görüntüleri döndürür.

- Any'yi olası bir [lisans](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#license) filtresi değeri olarak ekledi. Herhangi bir filtre yalnızca lisans altında olan görüntüleri döndürür.

- [maxFileSize](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#maxfilesize) ve [minFileSize](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#minfilesize) sorgu parametreleri eklendi. Dosya boyutları aralığındaki görüntüleri döndürmek için bu filtreleri kullanın.  

- [MaxHeight](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#maxheight), [minHeight](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#minheight), [maxWidth](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#maxwidth), [minWidth](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#minwidth) sorgu parametreleri eklendi. Yükseklik ve genişlik aralığındaki görüntüleri döndürmek için bu filtreleri kullanın.  

### <a name="object-changes"></a>Nesne değişiklikleri

- Teklif `description` nesnesine alanları [Offer](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#offer) ve alanları `lastUpdated` eklendi.  

- `name` [Alanı ImageGallery](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imagegallery) nesnesine ekledi.  

- `similarTerms` [Görüntüler](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#images) nesnesine eklendi. Bu alan, kullanıcının sorgu dizesine benzer terimlerin listesini içerir.  
