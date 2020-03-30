---
title: 'Örnek: AdventureWorks Envanter veritabanını modelleyin'
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da dizin oluşturma ve tam metin arama için ilişkisel verileri nasıl modelleyip düzleştirilmiş bir veri kümesine dönüştüreceklerini öğrenin.
author: HeidiSteen
manager: nitinme
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: heidist
ms.openlocfilehash: edb6162724938962df8a7340afea6e930a0b1049
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72793001"
---
# <a name="example-model-the-adventureworks-inventory-database-for-azure-cognitive-search"></a>Örnek: Azure Bilişsel Arama için AdventureWorks Envanter veritabanını modelleme

Azure Bilişsel Arama, [dizin oluşturma (veri alma) ardışık dizinine](search-what-is-an-index.md)giriş olarak düzleştirilmiş bir satır kümesini kabul eder. Kaynak verileriniz bir SQL Server ilişkisel veritabanından kaynaklanıyorsa, bu makalede, AdventureWorks örnek veritabanını örnek olarak kullanarak dizin oluşturmadan önce düzleştirilmiş bir satır kümesi oluşturmak için bir yaklaşım gösterilmiştir.

## <a name="about-adventureworks"></a>AdventureWorks Hakkında

Bir SQL Server örneğiniz varsa, [AdventureWorks örnek veritabanını](https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017)biliyor olabilirsiniz. Bu veritabanında yer alan tablolar arasında ürün bilgilerini ortaya çıkaran beş tablo bulunmaktadır.

+ **ÜrünModel**: isim
+ **Ürün**: ad, renk, maliyet, boyut, ağırlık, resim, kategori (her satır belirli bir Ürün Modeline katılır)
+ **ProductDescription**: açıklama
+ **ProductModelProductDescription:** locale (her satır belirli bir dil için belirli bir ProductDescription için bir ProductModel'e katılır)
+ **ÜrünKategorisi**: ad, ana kategori

Tüm bu verilerin, bir arama dizinine yutulabilecek düzleştirilmiş bir satır kümesinde birleştirilmesi bu örneğin amacıdır. 

## <a name="considering-our-options"></a>Seçeneklerimizi göz önünde bulundurarak

Naif yaklaşım, Ürün tablosunda en özel bilgilere sahip olduğundan, Ürün tablosundaki (uygun yerlerde birleştirilmiş) tüm satırları dizine dizine almak olacaktır. Ancak, bu yaklaşım, arama dizinini bir sonuç kümesinde algılanan yinelemelere maruz bırakır. Örneğin, Road-650 modeli iki renk ve altı boyutta mevcuttur. "Yol bisikletleri" için bir sorgu daha sonra sadece boyut ve renk ile ayırt aynı modelin on iki örnekleri hakim olacaktır. Diğer altı yola özel model, arama nın nether dünyasına küme olacaktır: ikinci sayfa.

  ![Ürün listesi](./media/search-example-adventureworks/products-list.png "Ürün listesi")
 
Road-650 modelinin on iki seçeneği olduğuna dikkat edin. Bir-çok varlık satırları en iyi çok değer alanları veya önceden toplanan değer alanları arama dizini olarak temsil edilir.

Bu sorunu çözmek, hedef dizini ProductModel tablosuna taşımak kadar basit değildir. Bunu yapmak, Ürün tablosunda arama sonuçlarında hala temsil edilmesi gereken önemli verileri yok sayardı.

## <a name="use-a-collection-data-type"></a>Koleksiyon veri türünü kullanma

"Doğru yaklaşım" veritabanı modelinde doğrudan paralel olmayan bir arama şema özelliği kullanmaktır: **Collection(Edm.String)**. Bu yapı, Azure Bilişsel Arama dizini şemasında tanımlanır. Çok uzun (tek) bir dize yerine tek tek dizeleri listesini temsil etmek gerektiğinde Bir Koleksiyon veri türü kullanılır. Etiketleriniz veya anahtar kelimeleriniz varsa, bu alan için bir Koleksiyon veri türü kullanırsınız.

**Koleksiyon(Edm.String)** çok değerli dizin alanları "renk", "boyut" ve "görüntü" için tanımlayarak, yardımcı bilgiler yinelenen girişler ile dizin kirletmeden yönlü ve filtreleme için korunur. Benzer şekilde, sayısal Ürün alanlarına toplam işlevleri uygulayın, her bir ürün **listesi**Yerine **minListPrice** dizine.

Bu yapıları içeren bir dizin göz önüne alındığında, "dağ bisikletleri" için bir arama renk, boyut ve en düşük fiyat gibi önemli meta verileri korurken, ayrık bisiklet modelleri gösterir. Aşağıdaki ekran görüntüsü bir resim sağlar.

  ![Dağ bisikleti arama örneği](./media/search-example-adventureworks/mountain-bikes-visual.png "Dağ bisikleti arama örneği")

## <a name="use-script-for-data-manipulation"></a>Veri işleme için komut dosyalarını kullanma

Ne yazık ki, bu modelleme türü kolayca SQL deyimleri tek başına elde edilemez. Bunun yerine, verileri yüklemek için basit bir NodeJS komut dosyası kullanın ve ardından arama dostu JSON varlıklarına eşlenin.

Son veritabanı arama eşlemi aşağıdaki gibi görünür:

+ model (Edm.String: aranabilir, filtrelenebilir, alınabilir) "ProductModel.Name"
+ description_en (Edm.String: aranabilir) "ProductDescription" modeli için nerede kültür='en'
+ renk (Edm.String): aranabilir, filtrelenebilir, yüz yüze, çıkarılabilir): model için "Product.Color"dan benzersiz değerler
+ boyutu (Edm.String): aranabilir, filtrelenebilir, yüzyüze, çıkarılabilir: model için "Product.Size"dan benzersiz değerler
+ resim (Edm.String): alınabilen): model için "Product.ThumbnailPhoto" benzersiz değerler
+ minStandardCost (Edm.Double: filtrelenebilir, değiştirilebilir, sıralanabilir, alınabilen): model için tüm "Product.StandardCost" toplam minimum
+ minListPrice (Edm.Double: filtrelenebilir, değiştirilebilir, sıralanabilir, alınabilen): model için tüm "Product.ListPrice" toplam minimum
+ minWeight (Edm.Double: filtrelenebilir, değiştirilebilir, sıralanabilir, alınabilen): model için tüm "Product.Weight" toplam minimum
+ ürünler (Edm.String): aranabilir, filtrelenebilir, alınabilen): model için "Product.Name"den benzersiz değerler

ProductModel tablosuna Product ve ProductDescription ile katıldıktan sonra, sonucu hızlı bir şekilde dönüştürmek için [lodash](https://lodash.com/) (veya C#'daki Linq) kullanın:

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

Ortaya çıkan JSON şu na benzer:

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

Son olarak, burada ilk kayıt kümesini döndürmek için SQL sorgusudur. NodeJS uygulamama veri yüklemek için [mssql](https://www.npmjs.com/package/mssql) npm modüllerini kullandım.

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
> [Örnek: Azure Bilişsel Arama'da çok düzeyli yönlü taksonomlar](search-example-adventureworks-multilevel-faceting.md)