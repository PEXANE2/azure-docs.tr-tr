---
title: 'Örnek: Çok düzeyli falar'
titleSuffix: Azure Cognitive Search
description: Uygulama sayfalarına ekebileceğiniz iç içe dönük bir gezinti yapısı oluşturarak çok düzeyli taksonomlar için nasıl yönlü yapılar oluşturabileceğinizi öğrenin.
author: HeidiSteen
manager: nitinme
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.openlocfilehash: 8672fa0911d1a031205bb3340fa0c03ab9492a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792953"
---
# <a name="example-multi-level-facets-in-azure-cognitive-search"></a>Örnek: Azure Bilişsel Arama'da çok düzeyli yüzler

Azure Bilişsel Arama şemaları çok düzeyli taksonomi kategorilerini açıkça desteklemez, ancak bunları dizine ekin oluşturmadan önce içeriği manipüle ederek ve ardından sonuçlara bazı özel işleme uygulayarak yaklaşık olarak tahmin edebilirsiniz. 

## <a name="start-with-the-data"></a>Verilerle başlayın

Bu makaledeki örnek, Azure Bilişsel Arama'da çok düzeyli yüz lekarşılaşmayı göstermek için [AdventureWorks Inventory veritabanını modelle](search-example-adventureworks-modeling.md)önceki bir örnekte yer almaktadır.

AdventureWorks bir ebeveyn-çocuk ilişkisi ile basit bir iki düzey taksonomi vardır. Bu yapının sabit uzunluktaki taksonomi derinliklerinde taksonomiyi gruplandırmak için basit bir SQL birleştirme sorgusu kullanılabilir:

```T-SQL
SELECT 
  parent.Name Parent, category.Name Category, category.ProductCategoryId
FROM 
  SalesLT.ProductCategory category
LEFT JOIN
  SalesLT.ProductCategory parent
  ON category.ParentProductCategoryId=parent.ProductCategoryId
```

  ![Sorgu sonuçları](./media/search-example-adventureworks/prod-query-results.png "Sorgu sonuçları")

## <a name="indexing-to-a-collection-field"></a>Koleksiyon alanına dizin oluşturma

Bu yapıyı içeren dizinde, bu verileri depolamak için Azure Bilişsel Arama şemasında bir **Koleksiyon (Edm.String)** alanı oluşturun ve alan özniteliklerinin aranabilir, filtrelenebilir, değiştirilebilir ve geri alınabilir olmasını sağlar.

Şimdi, belirli bir taksonomi kategorisine atıfta bulunan içeriği dizilerken, taksonominin her düzeyinden metin içeren bir dizi olarak taksonomi gönderin. Örneğin, bir varlık `ProductCategoryId = 5 (Mountain Bikes)`için , alanı`[ "Bikes", "Bikes|Mountain Bikes"]`

Alt kategori değeri "Dağ Bisikletleri" üst kategori "Bisikletler" eklenmesine dikkat edin. Her alt kategori, tüm yolunu kök öğeye göre katıştırmalıdır. Boru karakter ayırıcısı rasgeledir, ancak tutarlı olmalıdır ve kaynak metinde görünmemelidir. Ayırıcı karakter, taksonomi ağacını fason sonuçlardan yeniden oluşturmak için uygulama kodunda kullanılacaktır.

## <a name="construct-the-query"></a>Sorguyu oluşturma

Sorguları verirken, aşağıdaki fason belirtimi (taksonomi nin yüz yüze taksonomi alanınız olduğu durumlarda) ekleyin:`facet = taxonomy,count:50,sort:value`

Sayım değeri, olası tüm taksonomi değerlerini döndürecek kadar yüksek olmalıdır. AdventureWorks verileri 41 farklı taksonomi `count:50` değeri içerir, bu nedenle yeterlidir.

  ![Yönlü filtre](./media/search-example-adventureworks/facet-filter.png "Yönlü filtre")

## <a name="build-the-structure-in-client-code"></a>İstemci kodunda yapı oluşturma

İstemci uygulama kodunuzda, aktüomi ağacını, boru karakterindeki her bir yönlü değeri bölerek yeniden oluştur.

```javascript
var sum = 0
  , categories = {children:{},length:0}
  , results = getSearchResults();
separator = separator || '|';
field = field || 'taxonomy';
results['@search.facets'][field].forEach(function(d) {
  var node = categories;
  var parts = d.value.split(separator);
  sum += d.count;
  parts.forEach(function(c, i) {
    if (!_(node.children).has(c)) {
      node.children[c] = {};
      node.children[c].count = d.count;
      node.children[c].children = {};
      node.children[c].length = 0;
      node.children[c].filter = parts.slice(0,i+1).join(separator);
      node.length++;
    }
    node = node.children[c];
  });
});
categories.count = sum;
```

**Kategoriler** nesne şimdi doğru sayıları ile katlanabilir taksonomi ağacı işlemek için kullanılabilir:

  ![çok düzeyli yönlü filtre](./media/search-example-adventureworks/multi-level-facet.png "çok düzeyli yönlü filtre")

 
Ağaçtaki her bağlantı ilgili filtreyi uygulamalıdır. Örnek:

+ **taksonomi/herhangi bir** `(x:x eq 'Accessories')` aksesuar şubesindeki tüm belgeleri iade eder
+ **taksonomi/herhangi bir** `(x:x eq 'Accessories|Bike Racks')` döner sadece Aksesuarları şube altında Bisiklet Rafları bir alt kategori ile belgeleri.

Bu teknik, daha derin taksonomi ağaçları ve farklı ana kategoriler altında oluşan yinelenen alt `Bike Components|Forks` kategoriler `Camping Equipment|Forks`(örneğin, ve) gibi daha karmaşık senaryoları kapsayacak şekilde ölçeklendirir.

> [!TIP]
> Sorgu hızı döndürülen yönlü lerin sayısından etkilenir. Çok büyük taksonomi kümelerini desteklemek için, her belge için üst düzey taksonomi değerini tutmak için bir facetable **Edm.String** alanı eklemeyi düşünün. Daha sonra yukarıdaki aynı tekniği uygulayın, ancak kullanıcı üst düzey bir düğüm genişletildiğinde yalnızca toplama-facet sorgusu (kök taksonomi alanında filtre) gerçekleştirin. Veya % 100 geri çağırma gerekli değilse, sadece makul bir sayıya fason sayısını azaltmak ve fason girişleri sayısına göre sıralanır emin olun.

## <a name="see-also"></a>Ayrıca bkz.

[Örnek: Azure Bilişsel Arama için AdventureWorks Envanter veritabanını modelleme](search-example-adventureworks-modeling.md)