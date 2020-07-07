---
title: OData koleksiyon filtrelerini anlama
titleSuffix: Azure Cognitive Search
description: OData koleksiyon filtrelerinin, koleksiyonlara özgü sınırlamalar ve davranışlar dahil olmak üzere Azure Bilişsel Arama sorgularında nasıl çalıştığı mekanizması öğrenin.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: f6e8ed5baef9b8594bb1fe03942e831fd8264a56
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74113060"
---
# <a name="understanding-odata-collection-filters-in-azure-cognitive-search"></a>Azure Bilişsel Arama OData koleksiyon filtrelerini anlama

Azure Bilişsel Arama 'de koleksiyon alanlarını [filtrelemek](query-odata-filter-orderby-syntax.md) için, [ `any` ve `all` işleçlerini](search-query-odata-collection-operators.md) **lambda ifadeleriyle**birlikte kullanabilirsiniz. Lambda ifadeleri, bir **Aralık değişkenine**başvuran Boole ifadeleridir. `any`Ve `all` işleçleri `for` çoğu programlama dilinde bir döngüye benzer, döngü değişkeninin rolünü alan Aralık değişkeni ve döngünün gövdesi olarak lambda ifadesi. Aralık değişkeni, döngünün yinelemesi sırasında koleksiyonun "Current" değerini alır.

En azından bu, kavramsal olarak nasıl çalıştığı. Azure Bilişsel Arama, döngülerin nasıl çalıştığı konusunda filtreleri çok farklı bir şekilde uygular `for` . İdeal olarak, bu fark sizin için görünmez, ancak bazı durumlarda değildir. Nihai sonuç, lambda ifadeleri yazarken izlemeniz gereken kurallar vardır.

Bu makalede, Azure Bilişsel Arama 'in bu filtreleri nasıl yürüttüğünü inceleyerek koleksiyon filtreleri için kuralların neden olduğu açıklanmaktadır. Karmaşık lambda ifadeleriyle gelişmiş filtreler yazıyorsanız, filtrelerdeki nelerin ve neden olduğunu anlamak için bu makaleyi yararlı bulabilirsiniz.

Koleksiyon filtreleri kurallarının ne olduğu hakkında bilgi için, örnekler de dahil olmak üzere bkz. [Azure bilişsel arama 'Da OData koleksiyon filtrelerinde sorun giderme](search-query-troubleshoot-collection-filters.md).

## <a name="why-collection-filters-are-limited"></a>Koleksiyon filtrelerinin neden sınırlandırıldı

Tüm filtre özelliklerinin tüm koleksiyon türleri için desteklenmemesinin üç temel nedeni vardır:

1. Belirli veri türleri için yalnızca belirli işleçler desteklenir. Örneğin, Boolean değerlerini karşılaştırmak `true` ve `false` , vb. kullanarak, vb `lt` `gt` .
1. Azure Bilişsel Arama, türündeki alanlarda **bağıntılı aramayı** desteklemez `Collection(Edm.ComplexType)` .
1. Azure Bilişsel Arama, koleksiyonlar da dahil olmak üzere tüm veri türlerinde filtre yürütmek için ters dizinler kullanır.

İlk neden, OData dilinin ve EDM türü sisteminin nasıl tanımlandığınıza ilişkin bir sonucudur. Son ikisi bu makalenin geri kalanında daha ayrıntılı olarak açıklanmıştır.

## <a name="correlated-versus-uncorrelated-search"></a>Bağıntılı ve bağıntılı olmayan arama

Karmaşık nesneler koleksiyonuna birden çok filtre ölçütü uygulanırken, *koleksiyonda her bir nesne*için uygulandıklarından, ölçütler **bağıntılı** bir şekilde yapılır. Örneğin, aşağıdaki filtre 100 ' dan düşük bir fiyata sahip en az bir Deluxe Oda olan oteller döndürür:

    Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 100)

Filtreleme *bağıntısız*ise yukarıdaki filtre, bir odanın Deluxe olduğu ve farklı bir odanın 100 ' den düşük bir taban oranına sahip olan oteller döndürebilir. Yani, lambda ifadesinin her iki yan tümcesi de aynı Aralık değişkenine uygulandığından bu anlamlı değildir `room` . Bu, bu tür filtrelerin bağıntılı olmasının nedenleridir.

Ancak, tam metin arama için belirli bir Aralık değişkenine başvurmanız mümkün değildir. Bu şekilde, [tam bir Lucene sorgusu](query-lucene-syntax.md) vermek için kullanılabilir arama kullanıyorsanız:

    Rooms/Type:deluxe AND Rooms/Description:"city view"

oteller bir odanın bulunduğu yerde ve farklı bir odada açıklama içinde "şehir görünümü" bahsetmesini sağlayabilirsiniz. Örneğin, ile aşağıdaki belge `Id` `1` sorguyla eşleşir:

```json
{
  "value": [
    {
      "Id": "1",
      "Rooms": [
        { "Type": "deluxe", "Description": "Large garden view suite" },
        { "Type": "standard", "Description": "Standard city view room" }
      ]
    },
    {
      "Id": "2",
      "Rooms": [
        { "Type": "deluxe", "Description": "Courtyard motel room" }
      ]
    }
  ]
}
```

Bunun nedeni, tüm `Rooms/Type` belgedeki tüm analiz edilen koşullara `Rooms/Type` ve benzer şekilde `Rooms/Description` , aşağıdaki tablolarda gösterildiği gibi, öğesine başvuruda bulunuyor.

`Rooms/Type`Tam metin arama için nasıl depolanır:

| Dönem`Rooms/Type` | Belge kimlikleri |
| --- | --- |
| lü | 1, 2 |
| Stand | 1 |

`Rooms/Description`Tam metin arama için nasıl depolanır:

| Dönem`Rooms/Description` | Belge kimlikleri |
| --- | --- |
| Kurs | 2 |
| city | 1 |
| Bahçe | 1 |
| miktarda | 1 |
| Motel | 2 |
| Oda | 1, 2 |
| Stand | 1 |
| kurallarý | 1 |
| görüntüle | 1 |

Bu nedenle yukarıdaki filtreden farklı olarak, "bir `Type` odanın" Deluxe Oda "değerine eşit olduğu ve **aynı odanın** 100 ' den küçük olduğu belgeleri Eşleştir" `BaseRate` ifadesi " `Rooms/Type` Deluxe" teriminin bulunduğu belgeleri eşleştir ve `Rooms/Description` "şehir görünümü" ifadesini içerir. Alanları, ikinci durumda bağıntılı olabilecek ayrı odalar kavramı yoktur.

> [!NOTE]
> Azure Bilişsel Arama 'a eklenen bağıntılı arama desteğini görmek isterseniz, lütfen [Bu Kullanıcı ses öğesini](https://feedback.azure.com/forums/263029-azure-search/suggestions/37735060-support-correlated-search-on-complex-collections)oylayın.

## <a name="inverted-indexes-and-collections"></a>Ters dizinler ve koleksiyonlar

Karmaşık koleksiyonlar üzerinde, vb. gibi basit koleksiyonlar için çok daha az kısıtlama olduğunu fark etmiş olabilirsiniz `Collection(Edm.Int32)` `Collection(Edm.GeographyPoint)` . Bunun nedeni, Azure Bilişsel Arama karmaşık koleksiyonları gerçek bir alt belge koleksiyonları olarak depolarken basit koleksiyonlar tüm koleksiyonlar olarak depolanmaz.

Örneğin, `seasons` çevrimiçi bir satıcı için bir dizinde bulunan filtrelenebilir bir dize toplama alanı düşünün. Bu dizine yüklenmiş bazı belgeler şöyle görünebilir:

```json
{
  "value": [
    {
      "id": "1",
      "name": "Hiking boots",
      "seasons": ["spring", "summer", "fall"]
    },
    {
      "id": "2",
      "name": "Rain jacket",
      "seasons": ["spring", "fall", "winter"]
    },
    {
      "id": "3",
      "name": "Parka",
      "seasons": ["winter"]
    }
  ]
}
```

Alanın değerleri ters bir `seasons` **Dizin**adlı bir yapıda depolanır ve şuna benzer bir şekilde görünür:

| Terim | Belge kimlikleri |
| --- | --- |
| yay | 1, 2 |
| ini | 1 |
| düştü | 1, 2 |
| Kış | 2, 3 |

Bu veri yapısı, bir soruyu harika bir hızla yanıtlamak üzere tasarlandı: hangi belgeler belirli bir terime ait görünüyor? Bu sorunun yanıtlanması, bir koleksiyon üzerinde bir döngüden daha basit bir eşitlik denetimi gibi daha fazla işe yarar. Aslında, bu neden dize koleksiyonları için, Azure Bilişsel Arama yalnızca `eq` bir lambda ifadesinin içinde karşılaştırma işleci olarak izin verir `any` .

Daha sonra eşitlik ile aynı Aralık değişkeninde birden çok eşitlik denetimini nasıl birleştirebileceğinizi inceleyeceğiz `or` . Sonuç olarak [, nicelik belirteçleri ve dağıtılabilir özelliği](https://en.wikipedia.org/wiki/Existential_quantification#Negation)için teşekkürler. Bu ifade:

    seasons/any(s: s eq 'winter' or s eq 'fall')

eşittir:

    seasons/any(s: s eq 'winter') or seasons/any(s: s eq 'fall')

ve iki alt ifadenin her biri, `any` ters dizin kullanılarak etkili bir şekilde çalıştırılabilir. Ayrıca, [nicelik sayısının Olumsuzlaştırma yasaları](https://en.wikipedia.org/wiki/Existential_quantification#Negation)sayesinde bu ifade:

    seasons/all(s: s ne 'winter' and s ne 'fall')

eşittir:

    not seasons/any(s: s eq 'winter' or s eq 'fall')

`all`ve ile birlikte kullanılması mümkün değildir `ne` `and` .

> [!NOTE]
> Ayrıntılar bu belgenin kapsamının ötesinde olsa da, aynı ilkeler [coğrafi uzamsal noktaların koleksiyonları için uzaklık ve kesişim testlerine](search-query-odata-geo-spatial-functions.md) de genişletilir. Bunun nedeni `any` :
>
> - `geo.intersects`değillenemez
> - `geo.distance`, veya kullanılarak karşılaştırılmalıdır `lt``le`
> - ifadeler, ile birleştirilmelidir `or` , ancak`and`
>
> Convero kuralları için geçerlidir `all` .

Örneğin gibi,,, `lt` `gt` `le` ve `ge` işleçlerini destekleyen veri türleri koleksiyonlarında filtrelendiğinde daha geniş bir dizi ifadeye izin verilir `Collection(Edm.Int32)` . Özellikle, `and` `or` `any` temel alınan Karşılaştırma ifadeleri kullanarak **Aralık karşılaştırmaları** içinde birleştirildiğinde ve daha `and` sonra kullanarak daha fazla birleştirilmiş `or` olan ' de, içinde ve ' yi kullanabilirsiniz. Bu Boole ifadesi yapısına, "and Iler" olarak bilinen, ayırt edici [normal form (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form)adı verilir. Buna karşılık, bu veri türleri için lambda ifadeleri, `all` "and of ors" olarak da bilinen, [ayırt edici normal biçimde (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form)olmalıdır. Azure Bilişsel Arama, dize için hızlı bir arama yapmak gibi, bunları tersine çevrilmiş dizinler kullanarak yürütebildiğinden, bu tür Aralık karşılaştırmaları yapılmasına izin verir.

Özet bölümünde, bir lambda ifadesinde izin verilen nesnelerin kaydırma kuralları aşağıda verilmiştir:

- İçinde `any` , *pozitif denetimler* her zaman eşitlik, Aralık karşılaştırmaları, ya da `geo.intersects` `geo.distance` ile karşılaştırılır veya `lt` `le` (mesafeyi denetlemek için "closeness" gibi) eşitlik gibi her zaman izin verilir.
- İçinde `any` , `or` her zaman izin verilir. `and`Yalnızca, yalnızca and (DNF) ' i kullanıyorsanız, yalnızca Express Aralık denetimleri için kullanabileceğiniz veri türleri için kullanabilirsiniz.
- İçinde `all` kurallar ters çevrilir; yalnızca *negatif denetimlerine* izin verilir, `and` her zaman kullanabilirsiniz ve `or` yalnızca and of ORS (CNF) olarak ifade edilen Aralık denetimleri için kullanabilirsiniz.

Uygulamada, bunlar en büyük olasılıkla kullanabileceğiniz filtrelerin türleridir. Mümkün olan nesnelerin sınırlarını anlamak yine de yararlıdır.

Hangi tür filtrelerin izin verileceğini ve hangilerinin geçerli olduğunu gösteren belirli örnekler için bkz. [geçerli koleksiyon filtreleri yazma](search-query-troubleshoot-collection-filters.md#bkmk_examples).

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama OData koleksiyon filtreleri sorunlarını giderme](search-query-troubleshoot-collection-filters.md)
- [Azure Bilişsel Arama filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
