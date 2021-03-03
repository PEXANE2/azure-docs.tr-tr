---
title: Anlam yanıtı yapısı
titleSuffix: Azure Cognitive Search
description: Bilişsel Arama anlam derecelendirme algoritmasını ve bir sonuç kümesinden ' anlam yanıtları ' ve ' anlam başlıkları ' yapısının nasıl yapılandırılacağını açıklar.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: febbfd0f3def8e681107ef78d9478463b1c2a872
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680400"
---
# <a name="semantic-ranking-and-responses-in-azure-cognitive-search"></a>Azure Bilişsel Arama anlam derecelendirmesi ve yanıtları

> [!IMPORTANT]
> Anlam derecelendirmesi algoritması ve anlam yanıtları/açıklamalı alt yazılar yalnızca önizleme REST API aracılığıyla kullanılabilir. Önizleme özellikleri, olduğu gibi, [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)altında sunulur.

Anlam derecelendirmesi, anahtar kelimelerin aksine doğal dilde ifade edilen sorgular için eğitilen bir anlam derecelendirme modeli kullanarak, arama sonuçlarının kesinliğini artırır.

Bu makalede anlam derecelendirmesi algoritması ve anlam yanıtının nasıl şekillendirilmiş olduğu açıklanmaktadır. Yanıt, hem düz metin hem de vurgular ve yanıtlar (isteğe bağlı) açıklamalı alt yazılar içerir.

+ Anlam başlıkları, arama sonuçlarından ayıklanan sorguyla ilgili metin yollardır. Bireysel içerik alanları, sonuçlar sayfası için çok büyük olduğunda bir sonucu özetlemeye yardımcı olabilirler. Açıklamalı alt yazılar özelliği semantik vurgular, böylece kullanıcıların, en ilgili belgeleri bulmasını sağlayan ve bu sayede genel kullanıcı deneyimini geliştirecek şekilde sorgu sonuçlarını hızla bir şekilde

+ Anlamsal yanıtlar, sorular gibi görünen sorgulara yanıt vermek için Bing 'den makine öğrenimi modellerini kullanır. Yanıtlar, sorgu sonuç kümesindeki en üstteki belgelerden ayıklanarak sorguyla en çok ilgili olan bir dizi listeden seçilir. Yanıtlar, arama sayfalarında işlemeyi seçebileceğiniz sorgu yanıt yükünde bağımsız, üst düzey bir nesne olarak döndürülür.

## <a name="prerequisites"></a>Önkoşullar

+ Anlam sorgu türü kullanılarak formül ifade alınmıştır. Daha fazla bilgi için bkz. [anlamsal sorgu oluşturma](semantic-how-to-query-request.md).

## <a name="understanding-a-semantic-response"></a>Anlamsal yanıtı anlama

Anlam yanıtı, puanlar, açıklamalı alt yazılar ve yanıtlar için yeni özellikler içerir. Anlamsal bir yanıt, [tam metin arama motoru](search-lucene-query-architecture.md)tarafından döndürülen ilk 50 sonucu kullanılarak standart yanıttan oluşturulmuştur ve daha sonra semantik Ranker kullanılarak yeniden derecelendirilir. 50 ' den fazla belirtilirse, ek sonuçlar döndürülür, ancak anlamsal olarak yeniden derecelendirilir.

Tüm sorgularda olduğu gibi, yanıt alınabilir olarak işaretlenmiş tüm alanlardan veya yalnızca Select ifadesinde listelenen alanlarla oluşur. Ayrıca "yanıt" alanı ve "resim yazıları" da içerir.

+ Her anlam sonucu için, varsayılan olarak, bir arama sayfasında işlemeyi seçebileceğiniz ayrı bir alan olarak döndürülen bir "yanıt" vardır. En fazla beş belirtebilirsiniz. Yanıtın formüllenmesi otomatikleştiriledir: başlangıçtaki sonuçlardaki tüm belgeleri okuma, extractive özetleme çalıştırma, ardından makine okuma kavrama ve son olarak yanıt alanındaki kullanıcının sorusuna doğrudan yanıt yükseltme.

+ "Caption", düz metin olarak veya vurgularla döndürülen belge içeriğinin ayıklama temelli bir özetlemesinin olduğunu belirtir. Açıklamalı alt yazılar otomatik olarak dahil edilir ve gizlenemez. Vurgular, hangi dizelerin vurgulaması gerektiğini belirlemek için makine okuma kavrama kullanılarak uygulanır. Doğru belgeyi bulmak için bir sonuç sayfasını hızlıca tarayabilmeniz için en ilgili yollarda dikkat çekici bir çizim yapın.

Anlamsal olarak yeniden derecelendirilen sonuç kümesi, değer ile sonuçlanır @search.rerankerScore . Bir OrderBy yan tümcesi eklerseniz, bu yan tümce anlam sorgusunda desteklenmediğinden bir HTTP 400 hatası döndürülür.

## <a name="example"></a>Örnek

, @search.rerankerScore Standartta birlikte bulunur @search.score ve sonuçları yeniden derecelendirmek için kullanılır.

Aşağıdaki sorgu verildiğinde:

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2020-06-30-Preview
{
    "search": "newer hotel near the water with a great restaurant",
    "queryType": "semantic",
    "queryLanguage": "en-us",
    "answers": "none",
    "searchFields": "HotelName,Category,Description",
    "select": "HotelId,HotelName,Description,Category",
    "count": true
}
```

Aşağıdaki sonucu, anlamsal bir yanıt temsilcisini görmeyi bekleyebilir. Bu sonuçlar, "" ile derecelendirilen yalnızca ilk üç yanıtı gösterir @search.rerankerScore . Önce Oceanside çare 'in nasıl derecelendirdiğine dikkat edin. Varsayılan "" sıralaması kapsamında, @search.score Bu sonuç ikinci yerde, iz bittikten sonra döndürülür.

```http
{
    "@odata.count": 31,
    "@search.answers": [],
    "value": [
        {
            "@search.score": 1.8920634,
            "@search.rerankerScore": 1.1091284966096282,
            "@search.captions": [
                {
                    "text": "Oceanside Resort. Budget. New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the piper, rooftop pool, waterfront dining & more.",
                    "highlights": "<em>Oceanside Resort.</em> Budget. New Luxury Hotel.  Be the first to stay.<em> Bay views</em> from every room, location near the piper, rooftop pool, waterfront dining & more."
                }
            ],
            "HotelId": "18",
            "HotelName": "Oceanside Resort",
            "Description": "New Luxury Hotel.  Be the first to stay. Bay views from every room, location near the piper, rooftop pool, waterfront dining & more.",
            "Category": "Budget"
        },
        {
            "@search.score": 2.5204072,
            "@search.rerankerScore": 1.0731962407007813,
            "@search.captions": [
                {
                    "text": "Trails End Motel. Luxury. Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
                    "highlights": "<em>Trails End Motel.</em> Luxury. Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport."
                }
            ],
            "HotelId": "40",
            "HotelName": "Trails End Motel",
            "Description": "Only 8 miles from Downtown.  On-site bar/restaurant, Free hot breakfast buffet, Free wireless internet, All non-smoking hotel. Only 15 miles from airport.",
            "Category": "Luxury"
        },
        {
            "@search.score": 1.4104348,
            "@search.rerankerScore": 1.06992666143924,
            "@search.captions": [
                {
                    "text": "Winter Panorama Resort. Resort and Spa. Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs.",
                    "highlights": "<em>Winter Panorama Resort</em>. Resort and Spa. Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs."
                }
            ],
            "HotelId": "12",
            "HotelName": "Winter Panorama Resort",
            "Description": "Newly-renovated with large rooms, free 24-hr airport shuttle & a new restaurant. Rooms/suites offer mini-fridges & 49-inch HDTVs.",
            "Category": "Resort and Spa"
        }
```

## <a name="next-steps"></a>Sonraki adımlar

+ [Anlamsal aramaya genel bakış](semantic-search-overview.md)
+ [Benzerlik algoritması](index-ranking-similarity.md)
+ [Anlamsal sorgu oluşturma](semantic-how-to-query-request.md)
+ [Temel bir sorgu oluşturma](search-query-create.md)