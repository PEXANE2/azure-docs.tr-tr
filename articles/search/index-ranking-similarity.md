---
title: Sıralama Benzerlik Algoritması
titleSuffix: Azure Cognitive Search
description: Sıralama için yeni benzerlik algoritması denemek için benzerlik algoritması nasıl ayarlanır
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/13/2020
ms.openlocfilehash: c327440649300533c94c2a1956e3c45f433c9780
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409979"
---
# <a name="ranking-algorithm-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da sıralama algoritması

> [!IMPORTANT]
> 15 Temmuz 2020'den itibaren, yeni oluşturulan arama hizmetleri, çoğu durumda kullanıcı beklentilerini mevcut varsayılan sıralamadan daha iyi hizalayan arama sıralamaları sağlamak için kanıtlamış olan BM25 sıralama işlevini kullanır.  BM25, üstün sıralamanın ötesinde, belge boyutu gibi etkenlere bağlı olarak sonuçları ayarlayabilme yapılandırma seçenekleri de sağlar.  
>
> Bu değişiklikle, büyük olasılıkla arama sonuçlarınızın sıralanmasında küçük değişiklikler göreceksiniz.   Bu değişikliğin etkisini test etmek isteyenler için, 2019-05-06-Preview API'de BM25 puanlamasını yeni dizinler üzerinde etkinleştirme olanağı sağladık.  

Bu makalede, yeni BM25 sıralama algoritmasını kullanmak için 15 Temmuz 2020'den önce oluşturulan bir hizmeti nasıl güncelleştirebileceğiniz açıklanmaktadır.

Azure Bilişsel Arama, daha önce kullanılan *ClassicSimilarity* uygulamasının yerini alacak olan Okapi BM25 algoritması *BM25'in*resmi Lucene uygulamasını kullanacaktır. Eski ClassicBenzerlik algoritması gibi, BM25Benzerlik lik, her belge-sorgu çifti için alaka düzeyini hesaplamak için frekans (TF) terimini ve ters belge sıklığını (IDF) değişken olarak kullanan TF-IDF benzeri bir alma işlevidir, sıralama için kullanılır. Kavramsal olarak eski Klasik Benzerlik algoritmasına benzer olsa da, BM25 onu geliştirmek için olasılıksal bilgi alma kökünü alır. BM25 ayrıca, kullanıcının alaka düzeyi puanının eşleşen terimlerin terim sıklığıyla nasıl ölçeklendirildiklerine karar vermesine izin vermek gibi gelişmiş özelleştirme seçenekleri de sunar.

## <a name="how-to-test-bm25-today"></a>Bm25 bugün nasıl test edilebilir?

Yeni bir dizin oluşturduğunuzda, bir "benzerlik" özelliği ayarlayabilirsiniz. Aşağıda gösterildiği gibi *2019-05-06-Önizleme* sürümünü kullanmanız gerekir.

```
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2019-05-06-Preview
```

```json  
{
    "name": "indexName",
    "fields": [
        {
            "name": "id",
            "type": "Edm.String",
            "key": true
        },
        {
            "name": "name",
            "type": "Edm.String",
            "searchable": true,
            "analyzer": "en.lucene"
        },
        ...
    ],
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity"
    }
}
```

15 Temmuz 2020'den önce oluşturulan hizmetler için: Benzerlik atlanırsa veya null olarak ayarlanırsa, dizin eski klasik benzerlik algoritmasını kullanır.

15 Temmuz 2020'den sonra oluşturulan hizmetler için: Benzerlik atlanırsa veya null olarak ayarlanırsa, dizin yeni BM25 benzerlik algoritmasını kullanır.

Benzerlik değerini aşağıdaki iki değerden biri olarak da açıkça ayarlayabilirsiniz: *"#Microsoft.Azure.Search.ClassicSimilarity"* veya *"#Microsoft.Azure.Search.BM25Benzerliklik"*.


## <a name="bm25-similarity-parameters"></a>BM25 benzerlik parametreleri

BM25 benzerliği, hesaplanan alaka düzeyini kontrol etmek için iki kullanıcıözelleştirilebilir parametre ekler:

### <a name="k1"></a>k1

*K1* parametresi, her eşleşen terimin terim sıklığı ile belge sorgu çiftinin son alaka düzeyi puanı arasındaki ölçekleme işlevini denetler.

Sıfır değeri, metinde bu terimin kaç kez göründüğünden bağımsız olarak, tek bir eşleşen terimin katkısının tüm eşleşen belgeler için aynı olduğu bir "ikili modeli" temsil ederken, daha büyük bir k1 değeri skorun daha fazla artmaya devam etmesini sağlar aynı terimin örnekleri belgede bulunur. Varsayılan olarak, Azure Bilişsel Arama k1 parametresi için 1,2 değeri kullanır. Birden fazla terimin bir arama sorgusunun parçası olmasını beklediğimiz durumlarda daha yüksek bir k1 değeri kullanmak önemli olabilir. Bu gibi durumlarda, yalnızca tek bir, birden çok kez eşleşen belgeler üzerinden aranan farklı sorgu terimlerinin çoğuyla eşleşen belgeleri tercih etmek isteyebiliriz. Örneğin, "Apollo Uzay Uçuşu" terimlerini içeren belgeler için dizini sorgularken, "Uzay Uçuşu" teriminden bahsetmeden, Yunan Mitolojisi ile ilgili bir makalenin puanını birkaç düzine kez düşürmek isteyebiliriz. açıkça hem "Apollo" ve "Spaceflight" kez sadece bir avuç bahseder başka bir makale. 
 
### <a name="b"></a>b

*b* parametresi, belgenin uzunluğunun alaka düzeyi puanını nasıl etkilediğini denetler.

0,0 değeri, belgenin uzunluğunun skoru etkilemeyeceği anlamına gelirken, 1,0 değeri terim sıklığının alaka düzeyi puanı üzerindeki etkisinin belgenin uzunluğuna göre normalleştirileceği anlamına gelir. B parametresi için Azure Bilişsel Arama'da kullanılan varsayılan değer 0,75'tir. Daha uzun belgeleri cezalandırmak istediğimiz durumlarda terim sıklığının belgenin uzunluğuna göre normalleştirilmesi yararlıdır. Bazı durumlarda, daha uzun belgeler (tam bir roman gibi), çok daha kısa belgelere kıyasla birçok alakasız terimler ilerler.

### <a name="setting-k1-and-b-parameters"></a>K1 ve b parametrelerinin ayarlanması

b veya k1 değerlerini özelleştirmek için, BM25 kullanırken benzerlik nesnesine özellik olarak eklemeniz yeterlidir:

```json
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity",
        "b" : 0.5,
        "k1" : 1.3
    }
```

Benzerlik algoritması yalnızca dizin oluşturma zamanında ayarlanabilir. Bu, kullanılan benzerlik algoritmasının varolan dizinler için değiştirilemeyeceği anlamına gelir. BM25 kullanan varolan bir dizin tanımı güncelleştirildiğinde *"b"* ve *"k1"* parametreleri değiştirilebilir. Varolan bir dizindeki bu değerleri değiştirmek dizini en az birkaç saniye için çevrimdışı na alır ve dizin oluşturma ve sorgu isteklerinin başarısız olmasını sağlar. Bu nedenle, güncelleştirme isteğinizin sorgu dizesinde "allowIndexDowntime=true" parametresini ayarlamanız gerekir:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true
```


## <a name="see-also"></a>Ayrıca bkz.  

 [Azure Bilişsel Arama REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [Dizininize puanlama profilleri ekleme](index-add-scoring-profiles.md)    
 [Azure Bilişsel Arama REST API &#40;Dizin oluşturma&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)   
  [Azure Bilişsel Arama .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
