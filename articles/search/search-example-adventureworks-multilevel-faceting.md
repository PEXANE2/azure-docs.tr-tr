---
title: 'Örnek: Çok düzeyli modeller-Azure Search'
description: Birden çok düzeyli Taksonomiler için, uygulama sayfalarına dahil ettiğiniz iç içe geçmiş bir gezinti yapısı oluşturma hakkında bilgi edinin.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.openlocfilehash: 9a56bba55f9b3a59126168bc2bbbd50927c3fc78
ms.sourcegitcommit: 32242bf7144c98a7d357712e75b1aefcf93a40cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70274092"
---
# <a name="example-multi-level-facets-in-azure-search"></a>Örnek: Azure Arama’daki çok düzeyli modeller

Azure Search şemaları, çok düzeyli taksonomi kategorilerini açık bir şekilde desteklemez, ancak dizin oluşturma işleminden önce içeriği düzenleyerek ve sonra sonuçlara özel bir işleme uygulayarak bunları yaklaşık olarak yapabilirsiniz. 

## <a name="start-with-the-data"></a>Verilerle başlayın

Bu makaledeki örnek, Azure Search ' de çok düzeyli bir şekilde tam olarak göstermek için [AdventureWorks envanter veritabanını modelleyen](search-example-adventureworks-modeling.md)önceki bir örneği oluşturur.

AdventureWorks 'in üst-alt ilişkisi olan basit bir iki düzeyli bir sınıflandırması vardır. Bu yapının sabit uzunluklu taksonomi derinlikleri için, bir basit SQL JOIN sorgusu, taksonomiyi gruplamak için kullanılabilir:

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

Bu yapıyı içeren dizinde, bu verileri depolamak için Azure Search şemasında bir **koleksiyon (EDM. String)** alanı oluşturun ve alan özniteliklerinin aranabilir, filtrelenebilir, çok yönlü tablo ve alınabilir durumda olduğundan emin olun.

Artık, belirli bir taksonomi kategorisine başvuran içeriği dizinlerken, taksonomiyi taksonomi düzeyinden metin içeren bir dizi olarak gönderir. Örneğin, ile `ProductCategoryId = 5 (Mountain Bikes)`bir varlık için alanını şu şekilde gönder`[ "Bikes", "Bikes|Mountain Bikes"]`

"Sıradağlar bisikletleri" alt kategori değerindeki "Bisiklet" ana kategorisinin eklenmesine dikkat edin. Her alt kategori, tüm yolunu kök öğesine göre katıştırmalıdır. Kanal karakter ayırıcısı rastgele, ancak tutarlı olmalıdır ve kaynak metinde görünmemelidir. Ayırıcı karakter, uygulama kodunda, model sonuçlarından taksonomi ağacını yeniden oluşturmak için kullanılır.

## <a name="construct-the-query"></a>Sorguyu oluşturun

Sorgu verirken, aşağıdaki model belirtimini (Taksonomi, çok yönlü tablo taksonomi alanınız) dahil edin:`facet = taxonomy,count:50,sort:value`

Count değeri, tüm olası taksonomi değerlerini döndürecek kadar yüksek olmalıdır. AdventureWorks verileri 41 farklı taksonomi değerleri içerir, bu nedenle `count:50` yeterlidir.

  Çok ![yönlü filtre] Çok (./media/search-example-adventureworks/facet-filter.png "yönlü filtre")

## <a name="build-the-structure-in-client-code"></a>Yapıyı istemci kodunda oluştur

İstemci uygulama kodunuzda, kanal karakteri üzerindeki her bir model değerini bölerek taksonomi ağacını yeniden yapılandırma.

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

**Kategoriler** nesnesi artık, doğru sayımlar içeren daraltılabilir bir taksonomi ağacını işlemek için kullanılabilir:

  ![çok yönlü çok yönlü filtre](./media/search-example-adventureworks/multi-level-facet.png "çok yönlü çok yönlü filtre")

 
Ağaçtaki her bağlantı ilgili filtreyi uygulamalıdır. Örneğin:

+ **Taksonomi/any** `(x:x eq 'Accessories')` , aksesuarlar dalındaki tüm belgeleri döndürür
+ **Taksonomi/any** `(x:x eq 'Accessories|Bike Racks')` yalnızca aksesuar dalında Bisiklet raflarının alt kategorisini içeren belgeleri döndürür.

Bu teknik, daha derin taksonomi ağaçları ve farklı üst Kategoriler altında oluşan yinelenen alt kategoriler gibi daha karmaşık senaryoları kapsayacak şekilde ölçeklendirecektir (örneğin `Bike Components|Forks` , `Camping Equipment|Forks`ve).

> [!TIP]
> Sorgu hızı döndürülen model sayısından etkilenir. Çok büyük taksonomi kümelerini desteklemek için, her belge için en üst düzey taksonomi değerini tutacak bir çok yönlü **Edm. String** alanı eklemeyi göz önünde bulundurun. Daha sonra aynı tekniği uygulayın, ancak kullanıcı üst düzey bir düğümü genişlediğinde koleksiyon modeli sorgusunu (kök taksonomi alanında filtrelenmiş) gerçekleştirin. Ya da% 100 geri çekme gerekli değilse, model sayısını makul bir sayıya küçültün ve model girişlerinin sayıma göre sıralandığına emin olun.

## <a name="see-also"></a>Ayrıca bkz.

[Örnek: Azure Search için AdventureWorks envanter veritabanını modelleyin](search-example-adventureworks-modeling.md)