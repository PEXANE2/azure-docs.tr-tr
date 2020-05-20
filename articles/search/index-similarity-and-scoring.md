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
ms.openlocfilehash: 00cf806bf6575fd96af435abf8d0b3dd8734338a
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83679663"
---
# <a name="similarity-and-scoring-in-azure-cognitive-search"></a>Azure Bilişsel Arama benzerlik ve Puanlama

Puanlama, tam metin arama sorguları için arama sonuçlarında döndürülen her öğe için bir arama puanı hesaplamasına başvurur. Puan bir öğenin geçerli arama işlemi bağlamıyla ne kadar ilgili olduğunun göstergesidir. Puan ne kadar yüksekse öğe o kadar ilgilidir. Arama sonuçlarında, her öğe için hesaplanan arama puanlarına bağlı olarak, öğeler yüksek ve düşük arasında sıralanır. 

Varsayılan olarak, ilk 50 yanıtta döndürülür, ancak **$top** parametresini kullanarak daha küçük veya daha fazla sayıda öğe döndürebilir (tek bir yanıtta 1000 ' e kadar) **$Skip** ve sonraki sonuç kümesini elde edebilirsiniz.

Arama puanı, verilerin istatistiksel özelliklerine ve sorgusuna göre hesaplanır. Azure Bilişsel Arama, arama terimiyle eşleşen belgeleri ( [searchMode](https://docs.microsoft.com/rest/api/searchservice/search-documents#searchmodeany--all-optional)'e bağlı olarak) bulur, bu, arama teriminin birçok örneğini içeren favoring belgelerdir. Terim veri dizini genelinde nadir ise ve belge içinde ortak olduğunda arama puanı daha da artar. Bu yaklaşıma yönelik bu yaklaşımın temelini, *tf-ıDF veya* Term Frequency-ters belge sıklığı olarak bilinir.

Arama puanı değerleri, bir sonuç kümesi boyunca yinelenebilir. Birden çok isabetle aynı arama puanı varsa, aynı puanlanmış öğelerin sıralaması tanımlanmamıştır ve kararlı değildir. Sorguyu yeniden çalıştırın ve özellikle de ücretsiz hizmeti veya birden çok çoğaltmaya sahip faturalandırılabilir bir hizmeti kullanıyorsanız, öğelerin vardiya konumunu görebilirsiniz. Aynı puan ile iki öğe verildiğinde, ilk olarak bir tane görünecek garanti yoktur.

Yinelenen puanlar arasındaki bağlamanın kesilmesini istiyorsanız, ilk sıraya puan olarak bir **$OrderBy** yan tümce ekleyebilir ve sonra başka bir sıralanabilir alana göre sıralama yapabilirsiniz (örneğin, `$orderby=search.score() desc,Rating desc` ). Daha fazla bilgi için bkz. [$OrderBy](https://docs.microsoft.com/azure/search/search-query-odata-orderby).

> [!NOTE]
> Bir `@search.score = 1.00` puanlanmış veya derecelendirılmamış sonuç kümesini gösterir. Puan tüm sonuçlar genelinde tek bir değer. Puanlanmamış sonuçlar sorgu formu belirsiz arama, joker karakter veya Regex sorguları veya bir **$Filter** ifadesi olduğunda oluşur. 

## <a name="scoring-profiles"></a>Puanlama modelleri

Özel bir *Puanlama profili*tanımlayarak farklı alanların derecelendirildiği yöntemi özelleştirebilirsiniz. Puanlama profilleri, arama sonuçlarında öğelerin derecelendirmesi üzerinde daha fazla denetim sağlar. Örneğin, gelir potansiyelini temel alarak öğeleri artırmak, daha yeni öğeleri yükseltmek ya da envanterdeki çok uzun olan öğeleri artırmak isteyebilirsiniz. 

Puanlama profili, Dizin tanımının ağırlıklı alanlar, işlevler ve parametrelerden oluşan bir parçasıdır. Tanımlama hakkında daha fazla bilgi için bkz. [Puanlama profilleri](index-add-scoring-profiles.md).

<a name="scoring-statistics"></a>

## <a name="scoring-statistics-and-sticky-sessions-preview"></a>Puanlama istatistikleri ve yapışkan oturumlar (Önizleme)

Ölçeklenebilirlik için Azure Bilişsel Arama her dizini bir parçalama işlemi aracılığıyla yatay olarak dağıtır, bu da bir dizinin bölümlerinin fiziksel olarak ayrı olduğu anlamına gelir.

Varsayılan olarak, bir belgenin puanı *bir parça içindeki*verilerin istatistiksel özelliklerine göre hesaplanır. Bu yaklaşım genellikle büyük yapı verileri için bir sorun değildir ve tüm parçalar genelinde bilgilere göre puanı hesaplamak için gerekenden daha iyi performans sağlar. Yani, bu performans iyileştirmesinin kullanılması, farklı parçalar halinde iki benzer belge (veya benzer belge) farklı ilgi puanları ile sonlanmasına neden olabilir.

Puanı tüm parçalar genelinde istatistiksel özelliklere göre hesaplamak isterseniz, [sorgu parametresi](https://docs.microsoft.com/rest/api/searchservice/search-documents) olarak *scoringStatistics = Global* ekleyerek bunu yapabilirsiniz (veya *"scoringStatistics": "Global"* i [sorgu isteğinin](https://docs.microsoft.com/rest/api/searchservice/search-documents)gövde parametresi olarak ekleyebilirsiniz).

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?scoringStatistics=global&api-version=2019-05-06-Preview&search=[search term]
  Content-Type: application/json
  api-key: [admin or query key]  
```
ScoringStatistics kullanmak, aynı çoğaltmadaki tüm parçaların aynı sonuçları sağlamasına emin olur. Yani, her zaman dizininizdeki en son değişikliklerle güncelleştirildiğinden farklı çoğaltmalar birbirinden biraz farklı olabilir. Bazı senaryolarda, kullanıcılarınızın "sorgu oturumu" sırasında daha tutarlı sonuçlar almasını isteyebilirsiniz. Bu senaryolarda, `sessionId` sorgularınızı bir parçası olarak sağlayabilirsiniz. , `sessionId` Benzersiz bir kullanıcı oturumuna başvurmak için oluşturduğunuz benzersiz bir dizedir.

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?sessionId=[string]&api-version=2019-05-06-Preview&search=[search term]
  Content-Type: application/json
  api-key: [admin or query key]  
```
Aynı şekilde `sessionId` kullanıldığında, kullanıcılarınızın göreceği sonuçların tutarlılığını artırarak aynı çoğaltmayı hedeflemek için bir en iyi çaba denemesi yapılır. 

> [!NOTE]
> Aynı `sessionId` değerleri tekrar tekrar kullanmak, isteklerin çoğaltmalar genelinde yük dengelenmesini etkileyebilir ve arama hizmetinin performansını olumsuz yönde etkileyebilir. SessionID olarak kullanılan değer bir ' _ ' karakteriyle başlayamaz.

## <a name="similarity-ranking-algorithms"></a>Benzerlik derecelendirme algoritmaları

Azure Bilişsel Arama, iki farklı benzerlik derecelendirme algoritmasını destekler: *Klasik bir benzerlik* algoritması ve *Okapi BM25* algoritmasının resmi uygulama (Şu anda önizleme aşamasındadır). Klasik benzerlik algoritması varsayılan algoritmadır, ancak 15 Temmuz tarihinden itibaren oluşturulan yeni hizmetler yeni BM25 algoritmasını kullanır. Bu, yeni hizmetlerde kullanılabilecek tek algoritmadır.

Şimdilik, hangi benzerlik derecelendirme algoritmasını kullanmak istediğinizi belirtebilirsiniz. Daha fazla bilgi için bkz. [Derecelendirme algoritması](index-ranking-similarity.md).

Aşağıdaki video segmenti, Azure Bilişsel Arama 'de kullanılan derecelendirme algoritmalarının açıklamasına hızlı bir şekilde iletir. Daha fazla arka plan için tam video izleyebilirsiniz.

> [!VIDEO https://www.youtube.com/embed/Y_X6USgvB1g?version=3&start=322&end=643]

## <a name="see-also"></a>Ayrıca bkz.

 [Puanlama profilleri](index-add-scoring-profiles.md) [REST API başvurusu](https://docs.microsoft.com/rest/api/searchservice/)   
 [Arama belgeleri API 'SI](https://docs.microsoft.com/rest/api/searchservice/search-documents)   
 [Azure Bilişsel Arama .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
