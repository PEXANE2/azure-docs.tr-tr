---
title: Dijital TWINS 'e etiket ekleme
titleSuffix: Azure Digital Twins
description: Bkz. dijital TWINS 'de etiketleri uygulama
author: baanders
ms.author: baanders
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 59f68909e2f3704fea5c38e3f1535f5996b284ab
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100288"
---
# <a name="add-tags-to-digital-twins"></a>Dijital TWINS 'e etiket ekleme 

Dijital iklerinizi daha fazla tanımlamak ve kategorilere ayırmak için etiket kavramını kullanabilirsiniz. Özellikle, kullanıcılar, Azure dijital TWINS örnekleri içinde [samanlıkta etiketleri](https://project-haystack.org/doc/TagModel)gibi var olan sistemlerden etiketleri çoğaltmak isteyebilir. 

Bu belge, dijital TWINS 'de etiket uygulamak için kullanılabilecek desenleri açıklar.

Etiketler ilk olarak, bir dijital ikizi tanımlayan [model](concepts-models.md) içinde özellikler olarak eklenir. Bu özellik daha sonra modele göre oluşturulduğunda ikizi üzerinde ayarlanır. Bundan sonra Etiketler, TWINS 'nizi tanımlamak ve filtrelemek için [sorgularda](concepts-query-language.md) kullanılabilir.

## <a name="marker-tags"></a>İşaret etiketleri 

**İşaretleyici etiketi** , "mavi" veya "kırmızı" gibi bir dijital ikizi işaretlemek veya sınıflandırmak için kullanılan basit bir dizedir. Bu dize etiketin adıdır ve işaret etiketlerinin anlamlı bir değeri yoktur — etiket yalnızca kendi varlığına (veya devamsızlığa) göre önemlidir. 

### <a name="add-marker-tags-to-model"></a>Modele işaret etiketleri ekleme 

İşaret etiketleri, öğesinden öğesine [Dtdl](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) eşlemesi olarak modellenir `string` `boolean` . `mapValue`Etiketin bulunması önemli olduğundan, Boolean yoksayılır. 

Bir ikizi modelinden, bir işaretçi etiketini özellik olarak uygulayan bir alıntı aşağıda verilmiştir:

```json
{
  "@type": "Property",
  "name": "tags",
  "schema": {
    "@type": "Map",
    "mapKey": {
      "name": "tagName",
      "schema": "string"
    },
    "mapValue": {
      "name": "tagValue",
      "schema": "boolean"
    }
  }
}
```

### <a name="add-marker-tags-to-digital-twins"></a>Dijital TWINS 'e işaret etiketleri ekleme

`tags`Özelliği bir Digital ikizi modelinin bir parçası olduktan sonra, bu özelliğin değerini ayarlayarak Digital ikizi içindeki işaretleyici etiketini ayarlayabilirsiniz. 

Üç TWINS için işaretçiyi dolduran bir örnek aşağıda verilmiştir `tags` :

```csharp
entity-01: "tags": { "red": true, "round": true } 
entity-02: "tags": { "blue": true, "round": true } 
entity-03: "tags": { "red": true, "large": true } 
```

### <a name="query-with-marker-tags"></a>İşaret etiketleriyle sorgu

Etiketler dijital TWINS 'e eklendikten sonra, sorgular içindeki TWINS 'leri filtrelemek için Etiketler kullanılabilir. 

"Red" olarak etiketlenen tüm TWINS 'leri almak için bir sorgu aşağıda verilmiştir: 

```sql
select * from digitaltwins where is_defined(tags.red) 
```

Ayrıca, daha karmaşık sorgular için etiketleri birleştirebilirsiniz. Kırmızı değil, yuvarlatılmış olan tüm TWINS 'leri almak için bir sorgu aşağıda verilmiştir: 

```sql
select * from digitaltwins where not is_defined(tags.red) and is_defined(tags.round) 
```

## <a name="value-tags"></a>Değer etiketleri 

**Değer etiketi** , her etikete veya gibi bir değer vermek için kullanılan bir anahtar-değer çiftidir `"color": "blue"` `"color": "red"` . Bir değer etiketi oluşturulduktan sonra, etiketin değerini yoksayarak işaretleyici etiketi olarak da kullanılabilir. 

### <a name="add-value-tags-to-model"></a>Modele değer etiketleri Ekle 

Değer etiketleri, öğesinden öğesine [Dtdl](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) eşlemesi olarak modellenir `string` `string` . Her ikisi de `mapKey` `mapValue` önemlidir. 

Bir ikizi modelinden, özellik olarak bir değer etiketi uygulayan bir alıntıdır:

```json
{
  "@type": "Property",
  "name": "tags",
  "schema": {
    "@type": "Map",
    "mapKey": {
      "name": "tagName",
      "schema": "string"
    },
    "mapValue": {
      "name": "tagValue",
      "schema": "string"
    }
  }
} 
```

### <a name="add-value-tags-to-digital-twins"></a>Dijital TWINS 'e değer etiketleri ekleme

İşaretçi etiketlerinde olduğu gibi, bu özelliğin değerini modelden ayarlayarak dijital ikizi değer etiketini ayarlayabilirsiniz `tags` . Bir değer etiketini işaret etiketi olarak kullanmak için `tagValue` alanı boş dize değeri () olarak ayarlayabilirsiniz `""` . 

Üç TWINS için değeri dolduran bir örnek aşağıda verilmiştir `tags` :

```csharp
entity-01: "tags": { "red": "", "size": "large" } 
entity-02: "tags": { "purple": "", "size": "small" } 
entity-03: "tags": { "red": "", "size": "small" } 
```

`red` `purple` Bu örnekte işaret etiketleri olarak kullanıldığını unutmayın.

### <a name="query-with-value-tags"></a>Değer etiketleriyle sorgu

İşaretçi etiketlerinde olduğu gibi, sorgularda bulunan TWINS 'leri filtrelemek için değer etiketlerini de kullanabilirsiniz. Değer etiketlerini ve işaret etiketlerini birlikte de kullanabilirsiniz.

Yukarıdaki örnekte, `red` işaret etiketi olarak kullanılıyor. "Red" olarak etiketlenen tüm TWINS 'leri almak için bir sorgu aşağıda verilmiştir: 

```sql
select * from digitaltwins where is_defined(tags.red) 
```

Küçük (değer etiketi) ve kırmızı olmayan tüm varlıkları almak için bir sorgu aşağıda verilmiştir: 

```sql
select * from digitaltwins where not is_defined(tags.red) and tags.size = 'small' 
```

## <a name="next-steps"></a>Sonraki adımlar

Dijital ikizi modellerini tasarlama ve yönetme hakkında daha fazla bilgi edinin:
* [*Nasıl yapılır: özel modelleri yönetme*](how-to-manage-model.md)

İkizi grafiğini sorgulama hakkında daha fazla bilgi edinin:
* [*Nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md)