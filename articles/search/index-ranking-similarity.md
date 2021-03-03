---
title: Sıralama benzerliğini yapılandırma
titleSuffix: Azure Cognitive Search
description: Sıralama için yeni benzerlik algoritması denemek üzere benzerlik algoritması ayarlama
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 9f806b512ae8e118fca8f32115c8be3b493fd681
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101677793"
---
# <a name="configure-ranking-algorithms-in-azure-cognitive-search"></a>Azure Bilişsel Arama derecelendirme algoritmalarını yapılandırma

Azure Bilişsel Arama, iki benzerlik derecelendirme algoritmasını destekler:

+ 15 Temmuz 2020 ' ye kadar tüm arama hizmetleri tarafından kullanılan *Klasik bir benzerlik* algoritması.
+ 15 Temmuz 'dan sonra oluşturulan tüm Arama hizmetlerinde kullanılan *OKAPI BM25* algoritmasının bir uygulamasıdır.

BM25 derecelendirmesi, Kullanıcı beklentileriyle daha iyi uyum sağlayan arama derecelendirmelerine neden olduğunu eğilimi sağladığından yeni varsayılandır. Ayrıca, belge boyutu gibi faktörlere bağlı olarak ayarlama sonuçları için yapılandırma seçeneklerini sunar. 15 Temmuz 2020 ' den sonra oluşturulan yeni hizmetler için, BM25 otomatik olarak kullanılır ve tek benzerlik algoritmadır. Yeni bir hizmette benzerlik olarak benzerliği ayarlamaya çalışırsanız, bu algoritma hizmet tarafından desteklenmediğinden bir HTTP 400 hatası döndürülür.

15 Temmuz 2020 tarihinden önce oluşturulan eski hizmetler için, klasik benzerlik varsayılan algoritmaya devam eder. Daha eski hizmetler, aşağıda açıklandığı gibi, bir arama dizinindeki özellikleri BM25 çağırmak üzere ayarlayabilir. Klasik 'den BM25 'e geçiş yapıyorsanız, arama sonuçlarının nasıl sıralanmasına ilişkin bazı farklılıkları görmeyi bekleyebilir.

> [!NOTE]
> Anlamsal arama, beklentiler ve sonuçlar arasındaki boşluğu daha da daralan ek bir anlamsal yeniden derecelendirme algoritmasıdır. Diğer algoritmalardan farklı olarak, var olan bir sonuç kümesi üzerinde yineleme yapan bir eklenti özelliğidir. Önizleme anlam arama algoritmasını kullanmak için yeni bir hizmet oluşturmanız ve bir [anlamsal sorgu türü](semantic-how-to-query-request.md)belirtmeniz gerekir. Daha fazla bilgi için bkz. [anlam aramasına genel bakış](semantic-search-overview.md).

## <a name="create-a-search-index-for-bm25-scoring"></a>BM25 Puanlama için arama dizini oluşturma

15 Temmuz 2020 ' den önce oluşturulmuş bir arama hizmeti çalıştırıyorsanız, benzerlik özelliğini Dizin tanımında BM25Similarity veya Classıbenzerlik olarak ayarlayabilirsiniz. Benzerlik özelliği atlanırsa veya null olarak ayarlandıysa, Dizin klasik algoritmayı kullanır.

Benzerlik algoritması yalnızca dizin oluşturma zamanında ayarlanabilir. Ancak, BM25 ile bir dizin oluşturulduktan sonra, BM25 parametrelerini ayarlamak veya değiştirmek için mevcut dizini güncelleştirebilirsiniz.

| İstemci kitaplığı | Benzerlik özelliği |
|----------------|---------------------|
| .NET  | [Searchındex. benzerlik](/dotnet/api/azure.search.documents.indexes.models.searchindex.similarity) |
| Java | [Searchındex. Setbenzerlik](/java/api/com.azure.search.documents.indexes.models.searchindex.setsimilarity) |
| JavaScript | [Searchındex. benzerlik](/javascript/api/@azure/search-documents/searchindex#similarity) |
| Python | [Searchındex 'te benzerlik özelliği](/python/api/azure-search-documents/azure.search.documents.indexes.models.searchindex) |

### <a name="rest-example"></a>REST örneği

Ayrıca, aşağıdaki örnekte gösterildiği gibi [REST API](/rest/api/searchservice/create-index)de kullanabilirsiniz:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2020-06-30
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

## <a name="bm25-similarity-parameters"></a>BM25 benzerlik parametreleri

BM25 benzerlik, hesaplanan uygunluk Puanını denetlemek için iki Kullanıcı özelleştirilebilir parametre ekler. Dizin oluşturma sırasında BM25 parametrelerini veya dizin oluşturma sırasında BM25 algoritması belirtilmişse Dizin güncelleştirmesini ayarlayabilirsiniz.

| Özellik | Tür | Açıklama |
|----------|------|-------------|
| K1 | sayı | Bir belge sorgu çiftinin son uygunluk puanına, eşleşen koşulların Dönem sıklığı arasındaki ölçekleme işlevini denetler. Değerler genellikle 0,0 ile 3,0 arasında, varsayılan olarak 1,2. </br></br>0,0 değeri bir "ikili model" temsil eder; burada tek bir eşleştirme terimi, bu terimin kaç kez göründüğüne bakılmaksızın tüm eşleşen belgeler için aynı olduğunda, daha büyük bir K1 değeri, belgede aynı terimin daha fazla örneği bulunduğunda bu puanın artmaya devam etmesine izin verir. </br></br>Daha yüksek bir K1 değeri kullanmak, birden çok terimi bir arama sorgusunun parçası olarak beklediği durumlarda önemli olabilir. Bu durumlarda, yalnızca tek bir tane, birden çok kez eşleşen belgeler üzerinde aranan farklı sorgu terimlerinin birçoğu ile eşleşen belgeleri tercih etmek isteyebilirsiniz. Örneğin, "Apollo spaceflight" terimlerini içeren belgeler için Dizin sorgulanırken "Apollo" terimini ("spaceflight" koşulunu) içeren Yunanca Mythology hakkında bir makalenin Puanını, "Tollo" ve "spaceflight" gibi tek bir kez açıkça bahsetme gibi başka bir makaleye kıyasla |
| b | sayı | Bir belge uzunluğunun, ilgi Puanını nasıl etkilediğini denetler. Değerler, varsayılan olarak 0,75 ile 0 ile 1 arasındadır. </br></br>0,0 değeri, belgenin uzunluğunun puanı etkilemeyeceği anlamına gelir, ancak 1,0 değeri, ölçüdeki sıklık teriminin etkisinin belgenin uzunluğuna göre normalleştirilmesine neden olur. </br></br>Belgenin uzunluğuna göre sıklık sıklığını normalleştirmek, uzun belgeleri yarmak istiyoruz. Bazı durumlarda, çok daha kısa bir belge ile karşılaştırıldığında daha fazla ilgisiz terim, bazı durumlarda daha uzun belgelerde bulunabilir. |

### <a name="setting-k1-and-b-parameters"></a>K1 ve b parametrelerini ayarlama

B veya K1 değerlerini ayarlamak veya değiştirmek için, BM25 benzerlik nesnesine ekleyin. Var olan bir dizinde bu değerlerin ayarlanması veya değiştirilmesi, dizini en az birkaç saniyede çevrimdışı hale getirir ve etkin dizin oluşturma ve sorgu isteklerinin başarısız olmasına neden olur. Sonuç olarak, Güncelleştirme isteğinin "Allowındexkesinti = true" parametresini ayarlamanız gerekir:

```http
PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=2020-06-30&allowIndexDowntime=true
{
    "similarity": {
        "@odata.type": "#Microsoft.Azure.Search.BM25Similarity",
        "b" : 0.5,
        "k1" : 1.3
    }
}
```

## <a name="see-also"></a>Ayrıca bkz.  

+ [REST API başvurusu](/rest/api/searchservice/)
+ [Dizinlerinize Puanlama profilleri ekleme](index-add-scoring-profiles.md)
+ [Dizin API 'SI oluştur](/rest/api/searchservice/create-index)
+ [Azure Bilişsel Arama .NET SDK](/dotnet/api/overview/azure/search)