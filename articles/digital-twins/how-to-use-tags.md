---
title: Dijital TWINS 'e etiket ekleme
titleSuffix: Azure Digital Twins
description: Bkz. dijital TWINS 'de etiketleri uygulama
author: baanders
ms.author: baanders
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 70bf46de072a97eca810dda60a5331df14172ed6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100555149"
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

:::code language="json" source="~/digital-twins-docs-samples/models/tags.json" range="2-16":::

### <a name="add-marker-tags-to-digital-twins"></a>Dijital TWINS 'e işaret etiketleri ekleme

`tags`Özelliği bir Digital ikizi modelinin bir parçası olduktan sonra, bu özelliğin değerini ayarlayarak Digital ikizi içindeki işaretleyici etiketini ayarlayabilirsiniz. 

Üç TWINS için işaretçiyi dolduran bir örnek aşağıda verilmiştir `tags` :

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="TagPropertiesMarker":::

### <a name="query-with-marker-tags"></a>İşaret etiketleriyle sorgu

Etiketler dijital TWINS 'e eklendikten sonra, sorgular içindeki TWINS 'leri filtrelemek için Etiketler kullanılabilir. 

"Red" olarak etiketlenen tüm TWINS 'leri almak için bir sorgu aşağıda verilmiştir: 

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerTags1":::

Ayrıca, daha karmaşık sorgular için etiketleri birleştirebilirsiniz. Kırmızı değil, yuvarlatılmış olan tüm TWINS 'leri almak için bir sorgu aşağıda verilmiştir: 

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerTags2":::

## <a name="value-tags"></a>Değer etiketleri 

**Değer etiketi** , her etikete veya gibi bir değer vermek için kullanılan bir anahtar-değer çiftidir `"color": "blue"` `"color": "red"` . Bir değer etiketi oluşturulduktan sonra, etiketin değerini yoksayarak işaretleyici etiketi olarak da kullanılabilir. 

### <a name="add-value-tags-to-model"></a>Modele değer etiketleri Ekle 

Değer etiketleri, öğesinden öğesine [Dtdl](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md) eşlemesi olarak modellenir `string` `string` . Her ikisi de `mapKey` `mapValue` önemlidir. 

Bir ikizi modelinden, özellik olarak bir değer etiketi uygulayan bir alıntıdır:

:::code language="json" source="~/digital-twins-docs-samples/models/tags.json" range="17-31":::

### <a name="add-value-tags-to-digital-twins"></a>Dijital TWINS 'e değer etiketleri ekleme

İşaretçi etiketlerinde olduğu gibi, bu özelliğin değerini modelden ayarlayarak dijital ikizi değer etiketini ayarlayabilirsiniz `tags` . Bir değer etiketini işaret etiketi olarak kullanmak için `tagValue` alanı boş dize değeri () olarak ayarlayabilirsiniz `""` . 

Üç TWINS için değeri dolduran bir örnek aşağıda verilmiştir `tags` :

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="TagPropertiesValue":::

`red` `purple` Bu örnekte işaret etiketleri olarak kullanıldığını unutmayın.

### <a name="query-with-value-tags"></a>Değer etiketleriyle sorgu

İşaretçi etiketlerinde olduğu gibi, sorgularda bulunan TWINS 'leri filtrelemek için değer etiketlerini de kullanabilirsiniz. Değer etiketlerini ve işaret etiketlerini birlikte de kullanabilirsiniz.

Yukarıdaki örnekte, `red` işaret etiketi olarak kullanılıyor. Bu, "Red" olarak etiketlenen tüm TWINS 'leri almak için bir sorgu olduğunu unutmayın: 

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerTags1":::

Küçük (değer etiketi) ve kırmızı olmayan tüm varlıkları almak için bir sorgu aşağıda verilmiştir: 

:::code language="sql" source="~/digital-twins-docs-samples/queries/queries.sql" id="QueryMarkerValueTags":::

## <a name="next-steps"></a>Sonraki adımlar

Dijital ikizi modellerini tasarlama ve yönetme hakkında daha fazla bilgi edinin:
* [*Nasıl yapılır: DTDL modellerini yönetme*](how-to-manage-model.md)

İkizi grafiğini sorgulama hakkında daha fazla bilgi edinin:
* [*Nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md)