---
title: Arama dizininde dile göre filtrele
titleSuffix: Azure Cognitive Search
description: Dile özgü alanlara sorgu yürütmeyi kapsayan çok dilli aramayı desteklemek için ölçütleri filtreleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/22/2020
ms.openlocfilehash: 3bd0f01193b1e6579d60b581b43ad71ebb6001e3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87084203"
---
# <a name="how-to-filter-by-language-in-azure-cognitive-search"></a>Azure Bilişsel Arama dile göre filtreleme 

Çok dilli arama uygulamasındaki önemli bir gereksinim, kullanıcının kendi dilinde arama ve sonuç alma olanağıdır. Azure Bilişsel Arama, çok dilli bir uygulamanın dil gereksinimlerini karşılamanın bir yolu, belirli bir dilde dizeleri depolamak için ayrılmış bir dizi alan oluşturmaktır ve sonra tam metin aramasını sorgu sırasında yalnızca bu alanlarla kısıtlar.

İstekteki sorgu parametreleri, her iki arama işlemini de kapsam için kullanılır ve ardından, iletmek istediğiniz arama deneyimiyle uyumlu içerik sağlamayan alanların sonuçlarını kırpabilir.

| Parametreler | Amaç |
|-----------|--------------|
| **searchFields** | Tam metin aramasını adlandırılmış alanlar listesiyle sınırlandırır. |
| **$select** | Yanıtı yalnızca belirttiğiniz alanları içerecek şekilde kırpar. Varsayılan olarak, tüm alınabilir alanlar döndürülür. **$Select** parametresi, hangi hangilerinin dönebileceği seçmenize olanak sağlar. |

Bu teknik, alan içeriklerinin bütünlüğünden başarılı oldu. Azure Bilişsel Arama dizeleri çevirmez veya dil algılamayı gerçekleştirmez. Alanların, istediğiniz dizeleri içerdiğinden emin olmanız gerekir.

## <a name="define-fields-for-content-in-different-languages"></a>Farklı dillerdeki içerik için alanları tanımlama

Azure Bilişsel Arama 'de sorgular tek bir dizini hedefler. Tek bir arama deneyiminde dile özgü dizeler sağlamak isteyen geliştiriciler genellikle değerleri depolamak için ayrılmış alanlar tanımlar: Ingilizce dizeler için bir alan, Fransızca için bir, vb. 

Aşağıdaki örnek, farklı dillerdeki içerikleri içeren birkaç dize alanına sahip olan [gerçek emlak örneğinden](search-get-started-portal.md) biridir. Bu dizindeki alanlar için dil çözümleyici atamalarına dikkat edin. Dizeleri içeren alanlar, hedef dilin dil kurallarını işlemek için mühendislik uygulanan bir çözümleyici ile eşleştirildiği zaman tam metin aramasında daha iyi işlem yapar.

  ![Gerçek emlak örneğinin alanlar ekranını gösteren ekran görüntüsü. Bir grup alan, dil Çözümleyicisi atamalarının vurgulanan alanlarla ilgili dillere nasıl karşılık geldiğini göstermek için vurgulanır.](./media/search-filters-language/lang-fields.png)

> [!Note]
> Dil Çözümleyicileri ile alan tanımlarını gösteren kod örnekleri için bkz. [Dizin tanımlama (.net)](https://docs.microsoft.com/azure/search/search-create-index-dotnet) ve [DIZIN tanımlama (REST)](search-create-index-rest-api.md).

## <a name="build-and-load-an-index"></a>Dizin oluşturma ve yükleme

Ara (ve belirgin) bir adım sorgu oluşturmadan önce [dizini derleyip doldurmanız](https://docs.microsoft.com/azure/search/search-create-index-dotnet) gerekir. Bu adımdan daha fazla bahsedin. Dizinin kullanılabilir olup olmadığını belirlemenin bir yolu, [portaldaki](https://portal.azure.com)dizinler listesini denetleyerek.

## <a name="constrain-the-query-and-trim-results"></a>Sorgu ve kırpma sonuçlarını kısıtlama

Sorgudaki parametreler, aramayı belirli alanlarla sınırlandırmak için kullanılır ve sonra herhangi bir alanın sonuçlarını senaryonuz için faydalı olmayan şekilde kırpabilir. Arama, Fransızca dizeleri içeren alanlara kısıtlama hedefi verildiğinde, sorguyu Bu dildeki dizeleri içeren alanlara hedeflemek için **Searchfields** 'i kullanırsınız. 

Varsayılan olarak, bir arama alınabilir olarak işaretlenen tüm alanları döndürür. Bu nedenle, sağlamak istediğiniz dile özgü arama deneyimiyle uyumlu olmayan alanları dışlamak isteyebilirsiniz. Özellikle, Fransızca dizelerini içeren bir alanla aramayı sınırlandırdıysanız, muhtemelen sonuçlardan Ingilizce dizeler içeren alanları dışlamak isteyebilirsiniz. **$Select** Query parametresinin kullanılması, çağıran uygulamaya hangi alanların döndürüleceğini denetlemenizi sağlar.

```csharp
parameters =
    new SearchParameters()
    {
        searchFields = "description_fr" 
        Select = new[] { "description_fr"  }
    };
```
> [!Note]
> Sorguda $filter bağımsız değişken olmasa da, bu kullanım örneği filtre kavramlarıyla ilgili olarak özellikle bir filtreleme senaryosu olarak sunulur.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama filtreler](search-filters.md)
+ [Dil çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support)
+ [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)
+ [Belgelerde Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents)

