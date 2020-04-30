---
title: Benzerlik ve Puanlama genel bakış
titleSuffix: Azure Cognitive Search
description: Benzerlik ve Puanlama kavramlarını ve bir geliştiricinin Puanlama sonucunu özelleştirmek için neler yapabileceğini açıklar.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 9f9cc4c29b117c83595a36c4e28b1edb428c3cde
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82254128"
---
# <a name="similarity-and-scoring-in-azure-cognitive-search"></a>Azure Bilişsel Arama benzerlik ve Puanlama

Puanlama, tam metin arama sorguları için arama sonuçlarında döndürülen her öğe için bir arama puanı hesaplamasına başvurur. Puan bir öğenin geçerli arama işlemi bağlamıyla ne kadar ilgili olduğunun göstergesidir. Puan ne kadar yüksekse öğe o kadar ilgilidir. Arama sonuçlarında, her öğe için hesaplanan arama puanlarına bağlı olarak, öğeler yüksek ve düşük arasında sıralanır. 

Varsayılan olarak, ilk 50 yanıtta döndürülür, ancak **$top** parametresini kullanarak daha küçük veya daha fazla sayıda öğe döndürebilir (tek bir yanıtta 1000 ' e kadar) **$Skip** ve sonraki sonuç kümesini elde edebilirsiniz.

Arama puanı, verilerin istatistiksel özelliklerine ve sorgusuna göre hesaplanır. Azure Bilişsel Arama, arama terimiyle eşleşen belgeleri ( [searchMode](https://docs.microsoft.com/rest/api/searchservice/search-documents#searchmodeany--all-optional)'e bağlı olarak) bulur, bu, arama teriminin birçok örneğini içeren favoring belgelerdir. Terim veri dizini genelinde nadir ise ve belge içinde ortak olduğunda arama puanı daha da artar. Bu yaklaşıma yönelik bu yaklaşımın temelini, *tf-ıDF veya* Term Frequency-ters belge sıklığı olarak bilinir.

Arama puanı değerleri, bir sonuç kümesi boyunca yinelenebilir. Birden çok isabetle aynı arama puanı varsa, aynı puanlanmış öğelerin sıralaması tanımlanmamıştır ve kararlı değildir. Sorguyu yeniden çalıştırın ve özellikle de ücretsiz hizmeti veya birden çok çoğaltmaya sahip faturalandırılabilir bir hizmeti kullanıyorsanız, öğelerin vardiya konumunu görebilirsiniz. Aynı puan ile iki öğe verildiğinde, ilk olarak bir tane görünecek garanti yoktur.

Yinelenen puanlar arasındaki bağlamanın kesilmesini istiyorsanız, ilk sıraya puan olarak bir **$OrderBy** yan tümce ekleyebilir ve sonra başka bir sıralanabilir alana göre sıralama yapabilirsiniz (örneğin, `$orderby=search.score() desc,Rating desc`). Daha fazla bilgi için bkz. [$OrderBy](https://docs.microsoft.com/azure/search/search-query-odata-orderby).

> [!NOTE]
> Bir `@search.score = 1.00` puanlanmış veya derecelendirılmamış sonuç kümesini gösterir. Puan tüm sonuçlar genelinde tek bir değer. Puanlanmamış sonuçlar sorgu formu belirsiz arama, joker karakter veya Regex sorguları veya bir **$Filter** ifadesi olduğunda oluşur. 

## <a name="scoring-profiles"></a>Puanlama modelleri

Özel bir *Puanlama profili*tanımlayarak farklı alanların derecelendirildiği yöntemi özelleştirebilirsiniz. Puanlama profilleri, arama sonuçlarında öğelerin derecelendirmesi üzerinde daha fazla denetim sağlar. Örneğin, gelir potansiyelini temel alarak öğeleri artırmak, daha yeni öğeleri yükseltmek ya da envanterdeki çok uzun olan öğeleri artırmak isteyebilirsiniz. 

Puanlama profili, Dizin tanımının ağırlıklı alanlar, işlevler ve parametrelerden oluşan bir parçasıdır. Tanımlama hakkında daha fazla bilgi için bkz. [Puanlama profilleri](index-add-scoring-profiles.md).

## <a name="scoring-statistics"></a>Puanlama istatistikleri

Ölçeklenebilirlik için Azure Bilişsel Arama her dizini bir parçalama işlemi aracılığıyla yatay olarak dağıtır, bu da bir dizinin bölümlerinin fiziksel olarak ayrı olduğu anlamına gelir.

Varsayılan olarak, bir belgenin puanı *bir parça içindeki*verilerin istatistiksel özelliklerine göre hesaplanır. Bu yaklaşım genellikle büyük yapı verileri için bir sorun değildir ve tüm parçalar genelinde bilgilere göre puanı hesaplamak için gerekenden daha iyi performans sağlar. Yani, bu performans iyileştirmesinin kullanılması, farklı parçalar halinde iki benzer belge (veya benzer belge) farklı ilgi puanları ile sonlanmasına neden olabilir.

Puanı tüm parçalar genelinde istatistiksel özelliklere göre hesaplamak isterseniz, [sorgu parametresi](https://docs.microsoft.com/rest/api/searchservice/search-documents) olarak *scoringStatistics = Global* ekleyerek bunu yapabilirsiniz (veya *"scoringStatistics": "Global"* i [sorgu isteğinin](https://docs.microsoft.com/rest/api/searchservice/search-documents)gövde parametresi olarak ekleyebilirsiniz).

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?scoringStatistics=global
  Content-Type: application/json
  api-key: [admin key]  
```

> [!NOTE]
> `scoringStatistics` Parametresi için bir yönetici API anahtarı gereklidir.

## <a name="similarity-ranking-algorithms"></a>Benzerlik derecelendirme algoritmaları

Azure Bilişsel Arama, iki farklı benzerlik derecelendirme algoritmasını destekler: *Klasik bir benzerlik* algoritması ve *Okapi BM25* algoritmasının resmi uygulama (Şu anda önizleme aşamasındadır). Klasik benzerlik algoritması varsayılan algoritmadır, ancak 15 Temmuz tarihinden itibaren oluşturulan yeni hizmetler yeni BM25 algoritmasını kullanır. Bu, yeni hizmetlerde kullanılabilecek tek algoritmadır.

Şimdilik, hangi benzerlik derecelendirme algoritmasını kullanmak istediğinizi belirtebilirsiniz. Daha fazla bilgi için bkz. [Derecelendirme algoritması](index-ranking-similarity.md).

## <a name="watch-this-video"></a>Bu videoyu izleyin

Bu 16 dakikalık videoda, yazılım mühendisi Raouf Merouşe, nasıl dizin oluşturma, sorgulama ve Puanlama profilleri oluşturma işlemini açıklar. Belgeleriniz dizinlenmekte ve alındıkça, bu sayede çok daha iyi bir fikir elde edersiniz.

>[!VIDEO https://channel9.msdn.com/Shows/AI-Show/Similarity-and-Scoring-in-Azure-Cognitive-Search/player]

+ 2-3 dakika dizin oluşturma: metin işleme ve sözcük temelli analiz.
+ 3-4 dakika, dizin oluşturma: ters dizinler.
+ 4-6 dakika sorgulama: alma ve derecelendirme.
+ 7-16 dakika Puanlama profillerini içerir.

## <a name="see-also"></a>Ayrıca bkz.

 [Puanlama profilleri](index-add-scoring-profiles.md) [REST API başvurusu](https://docs.microsoft.com/rest/api/searchservice/)   
 [Arama belgeleri API 'SI](https://docs.microsoft.com/rest/api/searchservice/search-documents)   
 [Azure Bilişsel Arama .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
