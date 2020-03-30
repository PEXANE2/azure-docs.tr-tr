---
title: İngilizce olmayan arama sorguları için çok dilli dizini oluşturma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama, Lucene ve Doğal Dil İşleme teknolojisinden microsoft'un dil çözümleyicilerinden yararlanarak 56 dili destekler.
manager: nitinme
author: yahnoosh
ms.author: jlembicz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: ca2bc66c755da2011cc7016f37b194caa6200d9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72793585"
---
# <a name="how-to-create-an-index-for-multiple-languages-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da birden çok dil için dizin oluşturma

Dizinler, örneğin, dile özgü dizeler için tek tek alanlar oluşturmak gibi, birden çok dilden içerik içeren alanlar içerebilir. Dizin oluşturma ve sorgulama sırasında en iyi sonuçlar için, uygun dil kurallarını sağlayan bir dil çözümleyicisi atayın. 

Azure Bilişsel Arama, Analyzer özelliğini kullanarak tek tek alanlara atanabilen hem Lucene hem de Microsoft'tan geniş bir dil çözümleyicisi sunar. Bu makalede açıklandığı gibi portalda bir dil çözümleyicisi de belirtebilirsiniz.

## <a name="add-analyzers-to-fields"></a>Alanlara çözümleyici ekleme

Bir alan oluşturulduğunda bir dil çözümleyicisi belirtilir. Varolan bir alan tanımına bir çözümleyici eklemek, dizinin üzerine yazmayı (ve yeniden yüklemeyi) veya özgünle aynı olan ancak çözümleyici atamasıyla yeni bir alan oluşturmayı gerektirir. Daha sonra size kolaylık kullanılmayan alanı silebilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın ve arama hizmetinizi bulun.
1. Yeni bir dizin başlatmak için hizmet panosunun üst kısmındaki komut çubuğunda **ekle dizini'ni** tıklatın veya varolan bir dizine eklediğiniz yeni alanlar üzerinde çözümleyici ayarlamak için varolan bir dizini açın.
1. Bir ad vererek alan tanımı başlatın.
1. Edm.String veri türünü seçin. Yalnızca dize alanları tam metin aranabilir.
1. Çözümleyici özelliğini etkinleştirmek için **Aranabilir** özniteliği ayarlayın. Bir dil çözümleyicisi yararlanmak için bir alan metin tabanlı olmalıdır.
1. Kullanılabilir çözümleyicilerden birini seçin. 

![Alan tanımı sırasında dil çözümleyicileri atama](media/search-language-support/select-analyzer.png "Alan tanımı sırasında dil çözümleyicileri atama")

Varsayılan olarak, tüm aranabilir alanlar dil-agnostik olan [Standart Lucene çözümleyicisini](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/analysis/standard/StandardAnalyzer.html) kullanır. Desteklenen çözümleyicilerin tam listesini görüntülemek [için](index-add-language-analyzers.md)bkz.

Portalda, çözümleyicilerin olduğu gibi kullanılması amaçlanmıştır. Özelleştirme veya filtreler ve belirteçleri belirli bir yapılandırma gerekiyorsa, kod [özel bir çözümleyici oluşturmanız](index-add-custom-analyzers.md) gerekir. Portal, özel çözümleyicilerin seçilmesini veya yapılandırılmayı desteklemez.

## <a name="query-language-specific-fields"></a>Dile özgü alanları sorgula

Dil çözümleyicisi bir alan için seçildikten sonra, o alan için her dizin oluşturma ve arama isteğiyle birlikte kullanılır. Farklı çözümleyiciler kullanılarak birden çok alana karşı bir sorgu verildiğinde, sorgu her alan için atanan çözümleyiciler tarafından bağımsız olarak işlenir.

Sorgu veren aracının dili biliniyorsa, **arama alanları** sorgu parametresini kullanarak bir arama isteği belirli bir alana kapsama alınabilir. Aşağıdaki sorgu yalnızca Lehçe'deki açıklamaya karşı verilecektir:

`https://[service name].search.windows.net/indexes/[index name]/docs?search=darmowy&searchFields=PolishContent&api-version=2019-05-06`

Dizininizi, yukarıda gösterilene benzer bir sorguya yapıştırmak için [**Arama gezginini**](search-explorer.md) kullanarak portaldan sorgulayabilirsiniz.

## <a name="boost-language-specific-fields"></a>Dile özel alanları artırma

Bazen sorgu veren aracının dili bilinmemektedir ve bu durumda sorgu tüm alanlara karşı aynı anda düzenlenebilir. Gerekirse, belirli bir dilde sonuç [tercihi puanlama profilleri](index-add-scoring-profiles.md)kullanılarak tanımlanabilir. Aşağıdaki örnekte, İngilizce olarak tanımda bulunan maçlar Lehçe ve Fransızca maçlara göre daha yüksek puan olacaktır:

    "scoringProfiles": [
      {
        "name": "englishFirst",
        "text": {
          "weights": { "description_en": 2 }
        }
      }
    ]

`https://[service name].search.windows.net/indexes/[index name]/docs?search=Microsoft&scoringProfile=englishFirst&api-version=2019-05-06`

## <a name="next-steps"></a>Sonraki adımlar

Bir .NET geliştiricisiyseniz, [Azure Bilişsel Arama .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Search) ve [Çözümleyici](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzer?view=azure-dotnet) özelliğini kullanarak dil çözümleyicilerini yapılandırabileceğinizi unutmayın. 