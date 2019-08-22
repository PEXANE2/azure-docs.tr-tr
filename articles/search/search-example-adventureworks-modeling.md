---
title: 'Örnek: AdventureWorks envanter veritabanını modelleyin-Azure Search'
description: Azure Search ' de dizin oluşturma ve tam metin araması için, ilişkisel verileri nasıl modelleyeceğinizi ve düzleştirilmiş bir veri kümesine dönüştürmeyi öğrenin.
author: cstone
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: chstone
ms.openlocfilehash: 52ccf3edfca5b3481b038bd5d3449c1dd6354179
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69649911"
---
# <a name="example-model-the-adventureworks-inventory-database-for-azure-search"></a>Örnek: Azure Search için AdventureWorks envanter veritabanını modelleyin

Yapılandırılmış veritabanı içeriğini verimli bir arama dizininde modellemenin nadiren basit bir uygulamadır. Zamanlamayı planlama ve değiştirme yönetimi, kaynak satırların tablo ile Birleşik durumlarından, arama kullanımı kolay varlıklara kadar uzaklaşmasına kadar zorluk sergilemesine yardımcı olabilir. Bu makalede, veritabanından aramaya yönelik geçişte yaygın deneyimler vurgulamak için bulunan AdventureWorks örnek verileri kullanılmaktadır. 

## <a name="about-adventureworks"></a>AdventureWorks hakkında

Bir SQL Server örneğiniz varsa, AdventureWorks örnek veritabanı hakkında bilgi sahibi olabilirsiniz. Bu veritabanına dahil olan tablolar arasında ürün bilgilerini açığa çıkaran beş tablo bulunur.

+ **ProductModel**: Name
+ **Ürün**: ad, renk, maliyet, boyut, ağırlık, resim, kategori (her satır belirli bir ProductModel 'e katılır)
+ **ProductDescription**: Açıklama
+ **Productmodelproductdescription**: locale (her satır bir ProductModel 'e belirli bir dil için belirli bir ProductDescription öğesine katılır)
+ **ProductCategory**: ad, üst kategori

Bu verilerin tümünün, bir arama dizinine alıngirilebilecek düzleştirilmiş bir satır kümesinde birleştirilmesi, bir görevdir. 

## <a name="considering-our-options"></a>Seçeneklerimizi göz önünde bulundurarak

Ürün tablosu en belirli bilgilere sahip olduğundan, Naïve yaklaşımı, ürün tablosundaki (uygun olduğunda) tüm satırları dizinlemek olacaktır. Ancak, bu yaklaşım arama dizinini bir sonuç kümesinde algılanan yinelemeleri ortaya çıkarır. Örneğin, Road-650 modeli iki renkli ve altı boyut olarak kullanılabilir. Daha sonra, "Road Bisiklet" sorgusu, yalnızca boyut ve renge göre ayırt edilecek şekilde, aynı modelin her iki örneği tarafından uzlaştırılır. Diğer altı Road 'e özgü modelin her biri arama: sayfa 2 ' nin doğrulayamadı dünyasına eşit olarak dağıtılır.

  ![Ürün listesi](./media/search-example-adventureworks/products-list.png "Ürün listesi")
 
Road-650 modelinin on iki seçeneğe sahip olduğuna dikkat edin. Bire çok varlık satırları en iyi, arama dizininde çok değerli alanlar veya önceden toplanmış değer alanları olarak temsil edilir.

Bu sorunu çözmek, hedef dizini ProductModel tablosuna taşımak kadar basit değildir. Bunun yapılması, ürün tablosundaki arama sonuçlarında hala temsil edilmesi gereken önemli verileri yoksayar.

## <a name="use-a-collection-data-type"></a>Koleksiyon veri türü kullan

"Doğru yaklaşım" veritabanı modelinde doğrudan paralel olmayan bir arama şeması özelliğinden yararlanmadır: **Koleksiyon (EDM. String)** . Bir koleksiyon veri türü, çok uzun (tek) bir dize yerine tek tek dizelerin listesine sahip olduğunuzda kullanılır. Etiketlerde veya anahtar sözcüklerinizle karşılaşırsanız, bu alan için bir koleksiyon veri türü kullanırsınız.

"Color", "size" ve "image" için **koleksiyonun (EDM. String)** çok değerli dizin alanlarını tanımlayarak, çift yönlü bilgiler, dizini yinelenen girdilerle yoklamadan ve filtrelemeye yönelik olarak tutulur. Benzer şekilde, her bir ürün **ListPrice**yerine **minlistprice** dizini oluşturmak Için toplama işlevlerini sayısal ürün alanlarına uygulayın.

Bu yapılarla bir dizin verildiğinde, "Sıradağlar bisikletleri" araması ayrı Bisiklet modellerini gösterir ve renk, boyut ve en düşük fiyat gibi önemli meta verileri korur. Aşağıdaki ekran görüntüsünde bir çizim sunulmaktadır.

  ![Sıradağlar Bisiklet arama örneği](./media/search-example-adventureworks/mountain-bikes-visual.png "Sıradağlar Bisiklet arama örneği")

## <a name="use-script-for-data-manipulation"></a>Veri işleme için betiği kullan

Ne yazık ki, bu tür modellemenin tek başına SQL deyimleri aracılığıyla kolayca elde edilemez. Bunun yerine, verileri yüklemek için basit bir NodeJS betiği kullanın ve ardından bunu arama dostu JSON varlıklarına eşleyin.

Son veritabanı arama eşlemesi şöyle görünür:

+ Model (EDM. String: aranabilir, filtrelenebilir, alınabilir) "ProductModel.Name" adresinden
+ Kültür = ' en ' olduğunda model için "ProductDescription" kaynağından description_en (EDM. String: aranabilir)
+ Color (koleksiyon (EDM. String): aranabilir, filtrelenebilir, çok yönlü tablo, alınabilir): model için "Product. Color" öğesinden benzersiz değerler
+ Boyut (koleksiyon (EDM. String): aranabilir, filtrelenebilir, çok yönlü tablo, alınabilir): model için "Product. size" benzersiz değerleri
+ Image (koleksiyon (EDM. String): alınabilir): model için "Product. ThumbnailPhoto" öğesinden benzersiz değerler
+ Mınstandardcost (EDM. Double: filtrelenebilir, çok yönlü tablo, sıralanabilir, alınabilir): model için en az "Product. StandardCost" öğesinin toplamı
+ minListPrice (EDM. Double: filtrelenebilir, çok yönlü tablo, sıralanabilir, alınabilir): model için en az "Product. ListPrice" öğesinin toplamı
+ minWeight (EDM. Double: filtrelenebilir, çok yönlü tablo, sıralanabilir, alınabilir): model için en az "Product. Weight" öğesinin toplamı
+ Ürünler (koleksiyon (EDM. String): aranabilir, filtrelenebilir, alınabilir): model için "Product.Name" öğesinden benzersiz değerler

ProductModel tablosunu Product ve ProductDescription ile katıldıktan sonra, sonuç kümesi 'ni hızlıca dönüştürmek için [lodash](https://lodash.com/) ( C#veya LINQ ın) kullanın:

```javascript
var records = queryYourDatabase();
var models = _(records)
  .groupBy('ModelName')
  .values()
  .map(function(d) {
    return {
      model: _.first(d).ModelName,
      description: _.first(d).Description,
      colors: _(d).pluck('Color').uniq().compact().value(),
      products: _(d).pluck('ProductName').uniq().compact().value(),
      sizes: _(d).pluck('Size').uniq().compact().value(),
      images: _(d).pluck('ThumbnailPhotoFilename').uniq().compact().value(),
      minStandardCost: _(d).pluck('StandardCost').min(),
      maxStandardCost: _(d).pluck('StandardCost').max(),
      minListPrice: _(d).pluck('ListPrice').min(),
      maxListPrice: _(d).pluck('ListPrice').max(),
      minWeight: _(d).pluck('Weight').min(),
      maxWeight: _(d).pluck('Weight').max(),
    };
  })
  .value();
```

Sonuçta elde edilen JSON şöyle görünür:

```json
[
  {
    "model": "HL Road Frame",
    "colors": [
      "Black",
      "Red"
    ],
    "products": [
      "HL Road Frame - Black, 58",
      "HL Road Frame - Red, 58",
      "HL Road Frame - Red, 62",
      "HL Road Frame - Red, 44",
      "HL Road Frame - Red, 48",
      "HL Road Frame - Red, 52",
      "HL Road Frame - Red, 56",
      "HL Road Frame - Black, 62",
      "HL Road Frame - Black, 44",
      "HL Road Frame - Black, 48",
      "HL Road Frame - Black, 52"
    ],
    "sizes": [
      "58",
      "62",
      "44",
      "48",
      "52",
      "56"
    ],
    "images": [
      "no_image_available_small.gif"
    ],
    "minStandardCost": 868.6342,
    "maxStandardCost": 1059.31,
    "minListPrice": 1431.5,
    "maxListPrice": 1431.5,
    "minWeight": 961.61,
    "maxWeight": 1043.26
  }
]
```

Son olarak, ilk kayıt kümesini döndürmek için SQL sorgusu aşağıda verilmiştir. Verileri NodeJS uygulamama yüklemek için [MSSQL](https://www.npmjs.com/package/mssql) NPM modülünü kullandım.

```T-SQL
SELECT
  m.Name as ModelName,
  d.Description,
  p.Name as ProductName,
  p.*
FROM 
  SalesLT.ProductModel m
INNER JOIN 
  SalesLT.ProductModelProductDescription md
  ON m.ProductModelId = md.ProductModelId
INNER JOIN 
  SalesLT.ProductDescription d
  ON md.ProductDescriptionId = d.ProductDescriptionId
LEFT JOIN 
  SalesLT.product p
  ON m.ProductModelId = p.ProductModelId
WHERE
  md.Culture='en'
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek: Azure Search 'de çok düzeyli model sınıflandırmaları](search-example-adventureworks-multilevel-faceting.md)


