---
title: OData koleksiyon filtrelerini anlama-Azure Search
description: OData koleksiyon filtrelerinin Azure Search sorgularda nasıl çalıştığını anlama.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
ms.author: brjohnst
manager: nitinme
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
ms.openlocfilehash: 5c3a0205f5a9ac5115e78f1bc11f70b2c50a9714
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69647420"
---
# <a name="understanding-odata-collection-filters-in-azure-search"></a>Azure Search 'de OData koleksiyon filtrelerini anlama

Azure Search koleksiyon alanlarını [filtrelemek](query-odata-filter-orderby-syntax.md) için, [ `any` ve `all` işleçlerini](search-query-odata-collection-operators.md) **lambda ifadeleriyle**birlikte kullanabilirsiniz. Lambda ifadeleri, bir **Aralık değişkenine**başvuran Boole ifadeleridir. Ve işleçleri çoğu programlama dilinde bir `for` döngüye benzer, döngü değişkeninin rolünü alan Aralık değişkeni ve döngünün gövdesi olarak lambda ifadesi. `all` `any` Aralık değişkeni, döngünün yinelemesi sırasında koleksiyonun "Current" değerini alır.

En azından bu, kavramsal olarak nasıl çalıştığı. Azure Search, döngülerin nasıl çalıştığı konusunda `for` filtreleri çok farklı bir şekilde uygular. İdeal olarak, bu fark sizin için görünmez, ancak bazı durumlarda değildir. Nihai sonuç, lambda ifadeleri yazarken izlemeniz gereken kurallar vardır.

Bu makalede, Azure Search bu filtreleri nasıl yürüttüğünü inceleyerek koleksiyon filtreleri için kuralların neden olduğu açıklanmaktadır. Karmaşık lambda ifadeleriyle gelişmiş filtreler yazıyorsanız, filtrelerdeki nelerin ve neden olduğunu anlamak için bu makaleyi yararlı bulabilirsiniz.

Koleksiyon filtreleri kurallarının ne olduğu hakkında bilgi için, örnekler de dahil, bkz. [Azure Search OData koleksiyon filtrelerinde sorun giderme](search-query-troubleshoot-collection-filters.md).

## <a name="why-collection-filters-are-limited"></a>Koleksiyon filtrelerinin neden sınırlandırıldı

Tüm filtre özelliklerinin tüm koleksiyon türleri için desteklenmemesinin üç temel nedeni vardır:

1. Belirli veri türleri için yalnızca belirli işleçler desteklenir. Örneğin `true` , Boolean değerlerini karşılaştırmak ve `false` , `gt`vb. kullanarak `lt`, vb.
1. Azure Search, türündeki `Collection(Edm.ComplexType)`alanlarda **bağıntılı aramayı** desteklemez.
1. Azure Search koleksiyonlar da dahil olmak üzere tüm veri türlerinde filtre yürütmek için ters dizinler kullanır.

İlk neden, OData dilinin ve EDM türü sisteminin nasıl tanımlandığınıza ilişkin bir sonucudur. Son ikisi bu makalenin geri kalanında daha ayrıntılı olarak açıklanmıştır.

## <a name="correlated-versus-uncorrelated-search"></a>Bağıntılı ve bağıntılı olmayan arama

Karmaşık nesneler koleksiyonuna birden çok filtre ölçütü uygulanırken, *koleksiyonda her bir nesne*için uygulandıklarından, ölçütler **bağıntılı** bir şekilde yapılır. Örneğin, aşağıdaki filtre 100 ' dan düşük bir fiyata sahip en az bir Deluxe Oda olan oteller döndürür:

    Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 100)

Filtreleme *bağıntısız*ise yukarıdaki filtre, bir odanın Deluxe olduğu ve farklı bir odanın 100 ' den düşük bir taban oranına sahip olan oteller döndürebilir. Yani, lambda ifadesinin her iki yan tümcesi de aynı Aralık değişkenine `room`uygulandığından bu anlamlı değildir. Bu, bu tür filtrelerin bağıntılı olmasının nedenleridir.

Ancak, tam metin arama için belirli bir Aralık değişkenine başvurmanız mümkün değildir. Bu şekilde, [tam bir Lucene sorgusu](query-lucene-syntax.md) vermek için kullanılabilir arama kullanıyorsanız:

    Rooms/Type:deluxe AND Rooms/Description:"city view"

oteller bir odanın bulunduğu yerde ve farklı bir odada açıklama içinde "şehir görünümü" bahsetmesini sağlayabilirsiniz. Örneğin, ile `Id` `1` aşağıdaki belge sorguyla eşleşir:

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

Bunun nedeni, tüm `Rooms/Type` belgedekitüm`Rooms/Type` analiz edilen koşullara ve benzer şekilde ,aşağıdakitablolardagösterildiğigibi,öğesinebaşvurudabulunuyor.`Rooms/Description`

Tam `Rooms/Type` metin arama için nasıl depolanır:

| Dönem`Rooms/Type` | Belge kimlikleri |
| --- | --- |
| lü | 1, 2 |
| standart | 1\. |

Tam `Rooms/Description` metin arama için nasıl depolanır:

| Dönem`Rooms/Description` | Belge kimlikleri |
| --- | --- |
| Kurs | 2 |
| city | 1\. |
| Bahçe | 1\. |
| miktarda | 1\. |
| Motel | 2 |
| yı | 1, 2 |
| standart | 1\. |
| kurallarý | 1\. |
| görüntüle | 1\. |

Bu nedenle, yukarıdaki filtrenin aksine `Type` "bir odanın ' lüks ' e eşit olduğu ve `BaseRate` aynı odanın 100 ' den küçük olduğu belgeleri bul" ifadesi "Deluxe 'in bulunduğu `Rooms/Type` belgeleri Eşleştir" diyor "ve `Rooms/Description` " şehir görünümü "ifadesi vardır. Alanları, ikinci durumda bağıntılı olabilecek ayrı odalar kavramı yoktur.

> [!NOTE]
> Azure Search öğesine eklenen bağıntılı arama desteğini görmek istiyorsanız lütfen [Bu Kullanıcı ses öğesini](https://feedback.azure.com/forums/263029-azure-search/suggestions/37735060-support-correlated-search-on-complex-collections)oylayın.

## <a name="inverted-indexes-and-collections"></a>Ters dizinler ve koleksiyonlar

Karmaşık koleksiyonlar üzerinde, `Collection(Edm.Int32)` `Collection(Edm.GeographyPoint)`vb. gibi basit koleksiyonlar için çok daha az kısıtlama olduğunu fark etmiş olabilirsiniz. Bunun nedeni, Azure Search karmaşık koleksiyonları alt belgelerin gerçek koleksiyonları olarak depolarken basit koleksiyonlar tüm koleksiyonlar olarak depolanmaz.

Örneğin, çevrimiçi bir satıcı için bir dizinde `seasons` bulunan filtrelenebilir bir dize toplama alanı düşünün. Bu dizine yüklenmiş bazı belgeler şöyle görünebilir:

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

`seasons` Alanın değerleri **ters bir dizin**adlı bir yapıda depolanır ve şuna benzer bir şekilde görünür:

| Terim | Belge kimlikleri |
| --- | --- |
| yay | 1, 2 |
| ini | 1\. |
| düştü | 1, 2 |
| Kış | 2, 3 |

Bu veri yapısı, harika hızda bir soruyu yanıtlamak için tasarlanmıştır: Belirli bir terim hangi belgelerde görünür? Bu sorunun yanıtlanması, bir koleksiyon üzerinde bir döngüden daha basit bir eşitlik denetimi gibi daha fazla işe yarar. Aslında, bu neden dize koleksiyonları için Azure Search, yalnızca bir Lambda `eq` `any`ifadesinin içinde karşılaştırma işleci olarak izin verir.

Daha sonra eşitlik ile `or`aynı Aralık değişkeninde birden çok eşitlik denetimini nasıl birleştirebileceğinizi inceleyeceğiz. Sonuç olarak [, nicelik belirteçleri ve dağıtılabilir özelliği](https://en.wikipedia.org/wiki/Existential_quantification#Negation)için teşekkürler. Bu ifade:

    seasons/any(s: s eq 'winter' or s eq 'fall')

eşdeğerdir:

    seasons/any(s: s eq 'winter') or seasons/any(s: s eq 'fall')

ve iki `any` alt ifadenin her biri, ters dizin kullanılarak etkili bir şekilde çalıştırılabilir. Ayrıca, [nicelik sayısının Olumsuzlaştırma yasaları](https://en.wikipedia.org/wiki/Existential_quantification#Negation)sayesinde bu ifade:

    seasons/all(s: s ne 'winter' and s ne 'fall')

eşdeğerdir:

    not seasons/any(s: s eq 'winter' or s eq 'fall')

ve `all` `ne` ile birlikte kullanılması mümkün değildir. `and`

> [!NOTE]
> Ayrıntılar bu belgenin kapsamının ötesinde olsa da, aynı ilkeler [coğrafi uzamsal noktaların koleksiyonları için uzaklık ve kesişim testlerine](search-query-odata-geo-spatial-functions.md) de genişletilir. Bunun nedeni `any`:
>
> - `geo.intersects`değillenemez
> - `geo.distance`, veya kullanılarak `lt` karşılaştırılmalıdır`le`
> - ifadeler, ile `or`birleştirilmelidir, ancak`and`
>
> Convero kuralları için `all`geçerlidir.

`lt`Örneğin gibi `gt`,,, ve`ge` işleçlerini`Collection(Edm.Int32)` destekleyen veri türleri koleksiyonlarında filtrelendiğinde daha geniş bir dizi ifadeye izin verilir. `le` Özellikle, temel alınan karşılaştırma `and` ifadeleri kullanarak `any` `or` `or` `and` **Aralık karşılaştırmaları** içinde birleştirildiğinde ve daha sonra kullanarak daha fazla birleştirilmiş olan ' de, içinde ve ' yi kullanabilirsiniz. Bu Boole ifadesi yapısına, "and Iler" olarak bilinen, ayırt edici [normal form (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form)adı verilir. Buna karşılık, bu veri `all` türleri için lambda ifadeleri, "and of ors" olarak da bilinen, [ayırt edici normal biçimde (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form)olmalıdır. Azure Search, bu tür sınırlar karşılaştırmalarına izin verir, tıpkı dizeler için hızlı terim arama yapabildiğinden olduğu gibi, ters kullanılan dizinleri kullanarak bunları yürütebildiğinden.

Özet bölümünde, bir lambda ifadesinde izin verilen nesnelerin kaydırma kuralları aşağıda verilmiştir:

- İçinde `any`, *pozitif denetimler* her zaman eşitlik, `geo.intersects`Aralık `geo.distance` karşılaştırmaları, veya ile `lt` karşılaştırıldığında veya `le` ("eleess" öğesinin denetim geldiğinde eşitlik gibi düşünün uzaklık).
- İçinde `any` ,`or` her zaman izin verilir. Yalnızca, yalnızca `and` and (DNF) ' i kullanıyorsanız, yalnızca Express Aralık denetimleri için kullanabileceğiniz veri türleri için kullanabilirsiniz.
- İçinde `all`kurallar ters çevrilir; yalnızca *negatif denetimlerine* izin verilir, her zaman kullanabilirsiniz `and` ve yalnızca and of ORS (CNF) olarak ifade edilen Aralık denetimleri için kullanabilirsiniz. `or`

Uygulamada, bunlar en büyük olasılıkla kullanabileceğiniz filtrelerin türleridir. Mümkün olan nesnelerin sınırlarını anlamak yine de yararlıdır.

Hangi tür filtrelerin izin verileceğini ve hangilerinin geçerli olduğunu gösteren belirli örnekler için bkz. [geçerli koleksiyon filtreleri yazma](search-query-troubleshoot-collection-filters.md#bkmk_examples).

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search 'de OData koleksiyon filtreleri sorunlarını giderme](search-query-troubleshoot-collection-filters.md)
- [Azure Search filtreler](search-filters.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
