---
title: Arama dizinindeki dile göre filtreleme
titleSuffix: Azure Cognitive Search
description: Çok dilli aramayı desteklemek için filtre ölçütleri, sorgu yürütmesini dile özgü alanlara kapsama.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 5dbf32610e54df4ff009d4cb0a0b080babb4ec73
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74112072"
---
# <a name="how-to-filter-by-language-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da dile göre filtreleme 

Çok dilli bir arama uygulamasında ki temel gereksinimlerden biri, kullanıcının kendi dilinde arama yapma ve sonuçları alma yeteneğidir. Azure Bilişsel Arama'da, çok dilli bir uygulamanın dil gereksinimlerini karşılamanın bir yolu, dizeleri belirli bir dilde depolamaya adanmış bir dizi alan oluşturmak ve ardından tam metin aramasını sorgu sırasında yalnızca bu alanlara kısıtlamaktır.

İstekteki sorgu parametreleri hem arama işlemini kapsamak hem de sunmak istediğiniz arama deneyimiyle uyumlu içerik sağlamayan alanların sonuçlarını kırpmak için kullanılır.

| Parametreler | Amaç |
|-----------|--------------|
| **aramaAlanlar** | Tam metin aramasını adlandırılmış alanlar listesiyle sınırlar. |
| **$select** | Yanıtı yalnızca belirttiğiniz alanları içerecek şekilde kırpıyor. Varsayılan olarak, tüm alınan alanlar döndürülür. **$select** parametresi, hangilerinin geri döneceğini seçmenizi sağlar. |

Bu tekniğin başarısı alan içeriğinin bütünlüğüne bağlıdır. Azure Bilişsel Arama dizeleri çevirmez veya dil algılama gerçekleştirmez. Alanların beklediğiniz dizeleri içerdiğinden emin olmak size kalmış.

## <a name="define-fields-for-content-in-different-languages"></a>Farklı dillerdeki içerik alanlarını tanımlama

Azure Bilişsel Arama'da sorgular tek bir dizin hedeflemez. Tek bir arama deneyiminde dile özgü dizeleri sağlamak isteyen geliştiriciler genellikle değerleri depolamak için ayrılmış alanları tanımlar: İngilizce dizeleri için bir alan, Fransızca için bir alan, ve benzeri. 

Aşağıda gösterilen [gayrimenkul örneği](search-get-started-portal.md) de dahil olmak üzere örneklerimizde, aşağıdaki ekran görüntüsüne benzer alan tanımları görmüş olabilirsiniz. Bu örnekte, bu dizindeki alanların dil çözümleyici sayıltıcı atamalarının nasıl görüldüğüne dikkat edin. Dizeleri içeren alanlar, hedef dilin dilsel kurallarını işlemek üzere tasarlanmış bir çözümleyiciyle eşleştiğinde tam metin aramasında daha iyi performans gösterir.

  ![](./media/search-filters-language/lang-fields.png)

> [!Note]
> Dil çözümleyicileriyle alan tanımlarını gösteren kod örnekleri [için](https://docs.microsoft.com/azure/search/search-create-index-dotnet) bkz. [Define an index (REST)](search-create-index-rest-api.md)

## <a name="build-and-load-an-index"></a>Bir dizin oluşturma ve yükleme

Bir ara (ve belki de bariz) adım oluşturmak ve bir sorgu formüle etmeden önce [dizin doldurmak](https://docs.microsoft.com/azure/search/search-create-index-dotnet) olmasıdır. Biz bütünlük için burada bu adım söz. Dizin kullanılabilir olup olmadığını belirlemek için bir yolu [portalda](https://portal.azure.com)dizinler listesini kontrol etmektir.

## <a name="constrain-the-query-and-trim-results"></a>Sorguyu kısıtlama ve sonuçları kırpma

Sorgudaki parametreler, aramayı belirli alanlara sınırlamak ve ardından senaryonuza yardımcı olmayan alanların sonuçlarını kırpmak için kullanılır. Aramayı Fransızca dizeleri içeren alanlara bağlama hedefi göz önüne alındığında, **aramayı Alanları'nı** kullanarak sorguyu o dilde dizeleri içeren alanlarda hedeflemek için kullanırsınız. 

Varsayılan olarak, bir arama geri alınabilen olarak işaretlenmiş tüm alanları döndürür. Bu nedenle, sağlamak istediğiniz dile özgü arama deneyimine uymayan alanları hariç tutmak isteyebilirsiniz. Özellikle, aramayı Fransızca dizeleri olan bir alanla sınırlandırıyorsanız, büyük olasılıkla sonuçlarınızdan İngilizce dizeleri olan alanları hariç tutmak isteyebilirsiniz. **$select** sorgu parametresini kullanmak, hangi alanların arama uygulamasına döndürüldi üzerinde denetim sağlar.

```csharp
parameters =
    new SearchParameters()
    {
        searchFields = "description_fr" 
        Select = new[] { "description_fr"  }
    };
```
> [!Note]
> Sorguda $filter bağımsız değişken olmamasına rağmen, bu kullanım örneği filtre kavramlarıyla güçlü bir şekilde bağlıdır, bu nedenle bunu bir filtreleme senaryosu olarak salıyoruz.

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
+ [Dil çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support)
+ [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)
+ [Belgelerde Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents)

