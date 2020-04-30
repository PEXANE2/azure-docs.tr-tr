---
title: Sıralama benzerliği algoritması
titleSuffix: Azure Cognitive Search
description: Sıralama için yeni benzerlik algoritması denemek üzere benzerlik algoritması ayarlama
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/13/2020
ms.openlocfilehash: 1975c13162316b4132bae34659b1c5af8e416573
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231620"
---
# <a name="ranking-algorithm-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de derecelendirme algoritması

> [!IMPORTANT]
> 15 Temmuz 2020 ' den itibaren, yeni oluşturulan arama hizmetleri otomatik olarak BM25 derecelendirme işlevini kullanır. Bu, çoğu durumda, geçerli varsayılan derecelendirmeden Kullanıcı beklentileriyle daha iyi uyum sağlayan arama derecelendirmelerini sağlar. Üst düzey derecelendirmenin ötesinde, BM25, belge boyutu gibi faktörlere bağlı olarak ayarlama sonuçları için yapılandırma seçenekleri de sunar.  
>
> Bu değişiklik ile, büyük olasılıkla arama sonuçlarının sıralamasına göre küçük değişiklikler görecaksınız. Bu değişikliğin etkisini test etmek isteyen kişiler için, BM25 algoritması API-Version 2019-05-06-Preview ' da kullanılabilir.  

Bu makalede, Preview API kullanılarak oluşturulan ve sorgulanan yeni dizinler için mevcut Arama hizmetlerinde yeni BM25 derecelendirme algoritmasını nasıl kullanabileceğiniz açıklanır.

Azure Bilişsel Arama, daha önce kullanılan *Classicbenzerlik* uygulamasının yerini alacak olan *BM25Similarity*BM25 algoritmasının resmi Lucene uygulamasını benimseme sürecinde Okapi. Daha eski Classıbenzerlik algoritması gibi, BM25Similarity, her belge sorgu çiftinin uygunluk düzeyini hesaplamak için değişken olarak sıklık (TF) ve ters belge sıklığı (ıDF) değişkenlerini kullanan bir TF-IDF benzeri alma işlevidir. Bu, daha sonra sıralama için kullanılır. 

BM25, klasik daha eski benzerlik algoritmasına benzer olsa da, bunun üzerinde iyileştirebilmek için 'ın kökünü dayalı bilgi alımı ' na götürür. BM25 Ayrıca, kullanıcının ilgi puanı ile eşleşen koşulların dönem sıklığıyla nasıl ölçeklendirileceğine karar vermesini sağlayan gelişmiş özelleştirme seçenekleri sunar.

## <a name="how-to-test-bm25-today"></a>BM25 'i bugün test etme

Yeni bir dizin oluşturduğunuzda, algoritmayı belirtmek için bir **benzerlik** özelliği ayarlayabilirsiniz. Aşağıda gösterildiği gibi `api-version=2019-05-06-Preview`öğesini kullanmanız gerekir.

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

**Benzerlik** özelliği, her iki algoritma de kullanılabilir olduğunda, yalnızca mevcut hizmetlerde yararlıdır. 

| Özellik | Açıklama |
|----------|-------------|
| liği | İsteğe bağlı. Geçerli değerler şunlardır *"#Microsoft. Azure. Search. Classicbenzerlik"* veya *"#Microsoft. Azure. Search. BM25Similarity"*. <br/> 15 `api-version=2019-05-06-Preview` Temmuz 2020 ' den önce oluşturulan bir arama hizmetinde veya sonraki bir sürümünü gerektirir. |

15 Temmuz 2020 ' den sonra oluşturulan yeni hizmetler için, BM25 otomatik olarak kullanılır ve tek benzerlik algoritmadır. Yeni bir hizmette **benzerliği** `ClassicSimilarity` ayarlamaya çalışırsanız, bu algoritma yeni bir hizmette desteklenmediğinden 400 hatası döndürülür.

15 Temmuz 2020 tarihinden önce oluşturulan mevcut hizmetler için klasik benzerlik varsayılan algoritmayı sürdürür. **Benzerlik** özelliği atlanırsa veya null olarak ayarlandıysa, Dizin klasik algoritmayı kullanır. Yeni algoritmayı kullanmak istiyorsanız, yukarıda açıklandığı gibi **benzerliği** ayarlamanız gerekecektir.

## <a name="bm25-similarity-parameters"></a>BM25 benzerlik parametreleri

BM25 benzerlik, hesaplanan uygunluk Puanını denetlemek için iki Kullanıcı özelleştirilebilir parametre ekler.

### <a name="k1"></a>K1

*K1* parametresi, eşleşen koşulların Dönem sıklığı ile bir belge sorgu çiftinin son uygunluk puanı arasındaki ölçekleme işlevini denetler.

Sıfır değeri bir "ikili model" temsil eder ve bu terimin kaç kez göründüğüne bakılmaksızın, daha büyük bir K1 değeri, belgede aynı terimin daha fazla örneği bulunduğunda, puanın artmaya devam etmesine izin veriyorsa, tek bir eşleştirme teriminin katkısının her türlü eşleşen belge için aynı olması gerekir. Varsayılan olarak, Azure Bilişsel Arama K1 parametresi için 1,2 değerini kullanır. Daha yüksek bir K1 değeri kullanmak, birden çok terimi bir arama sorgusunun parçası olarak beklediği durumlarda önemli olabilir. Bu durumlarda, yalnızca tek bir tane, birden çok kez eşleşen belgeler üzerinde aranan farklı sorgu terimlerinin birçoğu ile eşleşen belgeleri tercih etmek isteyebilirsiniz. Örneğin, "Apollo spaceflight" terimlerini içeren belgeler için Dizin sorgulanırken "Apollo" terimini ("spaceflight") içeren bir makalenin Puanını, "Tollo" ve "spaceflight" gibi tek bir kez açıkça bahsettiğimiz başka bir makaleye kıyasla bir kaç düzine 
 
### <a name="b"></a>b

*B* parametresi, bir belgenin uzunluğunun ilgi Puanını nasıl etkilediğini denetler.

0,0 değeri, belgenin uzunluğunun puanı etkilemeyeceği anlamına gelir, ancak 1,0 değeri, ölçüdeki sıklık teriminin etkisinin belgenin uzunluğuna göre normalleştirilmesine neden olur. B parametresi için Azure Bilişsel Arama 'de kullanılan varsayılan değer 0,75 ' dir. Belgenin uzunluğuna göre sıklık sıklığını normalleştirmek, uzun belgeleri yarmak istiyoruz. Bazı durumlarda, çok daha kısa bir belge ile karşılaştırıldığında daha fazla ilgisiz terim, bazı durumlarda daha uzun belgelerde bulunabilir.

### <a name="setting-k1-and-b-parameters"></a>K1 ve b parametrelerini ayarlama

B veya K1 değerlerini özelleştirmek için, BM25 kullanırken onları benzerlik nesnesine özellikler olarak eklemeniz yeterlidir:

```json
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity",
        "b" : 0.5,
        "k1" : 1.3
    }
```

Benzerlik algoritması yalnızca dizin oluşturma zamanında ayarlanabilir. Bu, kullanılan benzerlik algoritmasının mevcut dizinler için değiştirimeyeceği anlamına gelir. BM25 kullanan mevcut bir dizin tanımı güncelleştirilirken *"b"* ve *"K1"* parametreleri değiştirilebilir. Var olan bir dizinde bu değerlerin değiştirilmesi, dizin oluşturma ve sorgu isteklerinizin başarısız olmasına neden olacak şekilde dizini en az birkaç saniyede çevrimdışı hale getirir. Bu nedenle, güncelleştirme isteğinizin sorgu dizesinde "Allowwındexını = true" parametresini ayarlamanız gerekir:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true
```

## <a name="see-also"></a>Ayrıca bkz.  

+ [REST API başvurusu](https://docs.microsoft.com/rest/api/searchservice/)   
+ [Dizinlerinize Puanlama profilleri ekleme](index-add-scoring-profiles.md)    
+ [Dizin API 'SI oluştur](https://docs.microsoft.com/rest/api/searchservice/create-index)   
+ [Azure Bilişsel Arama .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
