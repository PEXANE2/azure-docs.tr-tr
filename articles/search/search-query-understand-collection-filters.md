---
title: OData toplama filtrelerini anlama
titleSuffix: Azure Cognitive Search
description: Koleksiyonlara özgü sınırlamalar ve davranışlar da dahil olmak üzere Azure Bilişsel Arama sorgularında OData toplama filtrelerinin nasıl çalıştığının mekaniğini öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113060"
---
# <a name="understanding-odata-collection-filters-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da OData toplama filtrelerini anlama

Azure Bilişsel Arama'daki toplama alanlarına [filtre](query-odata-filter-orderby-syntax.md) uygulayabilmek için **lambda ifadeleri**ile [ `any` birlikte ve `all` işleçler](search-query-odata-collection-operators.md) kullanabilirsiniz. Lambda ifadeleri, **bir aralık değişkenine**başvuran Boolean ifadeleridir. Ve `any` `all` işleçler çoğu `for` programlama dilinde bir döngüye benzerdir, aralık değişkeni döngü değişkeni rolünü alır ve döngünün gövdesi olarak lambda ifadesidir. Aralık değişkeni, döngünün yinelemesi sırasında koleksiyonun "geçerli" değerini alır.

En azından kavramsal olarak böyle işliyor. Gerçekte, Azure Bilişsel Arama filtreleri döngülerin çalışma `for` şekline göre çok farklı bir şekilde uygular. İdeal olarak, bu fark sizin için görünmez olurdu, ancak bazı durumlarda değil. Sonuçta lambda ifadeler yazarken uymanız gereken kurallar vardır.

Bu makalede, Azure Bilişsel Arama'nın bu filtreleri nasıl yürüttüğünün araştırılmasıyla koleksiyon filtrelerine yönelik kuralların neden var olduğu açıklanmaktadır. Karmaşık lambda ifadeleri ile gelişmiş filtreler yazıyorsanız, bu makaleyi filtrelerde nelerin mümkün olduğunu ve neden mümkün olduğunu anlamanızda yararlı bulabilirsiniz.

Örnekler de dahil olmak üzere koleksiyon filtrelerinin kuralları hakkında bilgi için Azure [Bilişsel Arama'da Sorun Giderme OData toplama filtrelerine](search-query-troubleshoot-collection-filters.md)bakın.

## <a name="why-collection-filters-are-limited"></a>Koleksiyon filtreleri neden sınırlıdır?

Tüm filtre özelliklerinin tüm koleksiyon türleri için desteklenmemesi için üç temel nedeni vardır:

1. Yalnızca belirli işleçler belirli veri türleri için desteklenir. Örneğin, Boolean `true` değerlerini karşılaştırmak ve `false` ", `lt` `gt`ve benzeri" değerlerini kullanmak mantıklı değildir.
1. Azure Bilişsel Arama türü **correlated search** `Collection(Edm.ComplexType)`alanlarında ilişkili aramayı desteklemez.
1. Azure Bilişsel Arama, koleksiyonlar da dahil olmak üzere tüm veri türlerine filtre uygulamak için ters dizinler kullanır.

İlk neden, OData dili ve EDM türü sisteminin nasıl tanımlandığının bir sonucudur. Son iki bu makalenin geri kalanında daha ayrıntılı olarak açıklanmıştır.

## <a name="correlated-versus-uncorrelated-search"></a>Korelasyona karşı ilgisiz arama

Karmaşık nesneler koleksiyonu na birden çok filtre ölçütü uygulanırken, *ölçütler koleksiyondaki her nesne*için geçerli olduğundan **ilişkilendirilir.** Örneğin, aşağıdaki filtre, en az bir deluxe odası olan ve fiyatı 100'den az olan otelleri döndürür:

    Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 100)

Filtreleme *uncorrelated*ise, yukarıdaki filtre bir oda deluxe ve farklı bir oda 100'den az bir taban fiyatı olan oteller döndürebilir. Lambda ifadesinin her iki yan tümcesi aynı aralık değişkenine `room`uygulandığından, bu hiç mantıklı olmaz. Bu nedenle bu tür filtreler korelasyon vardır.

Ancak, tam metin arama için belirli bir aralık değişkenine başvurmanın bir yolu yoktur. Bunun gibi tam bir [Lucene sorgusu](query-lucene-syntax.md) vermek için alanlı arama kullanıyorsanız:

    Rooms/Type:deluxe AND Rooms/Description:"city view"

bir oda deluxe olduğu oteller geri alabilirsiniz, ve farklı bir oda açıklamasında "şehir görünümü" bahseder. Örneğin, aşağıdaki belge `Id` ile `1` sorgu eşleşir:

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

Bunun nedeni, `Rooms/Type` belgenin `Rooms/Type` tamamında alanın tüm çözümlenmiş terimlerini ifade `Rooms/Description`eder ve benzer şekilde aşağıdaki tablolarda gösterildiği gibi.

Tam `Rooms/Type` metin arama için nasıl saklanır:

| Dönem`Rooms/Type` | Belge Kimlikleri |
| --- | --- |
| Deluxe | 1, 2 |
| Standart | 1 |

Tam `Rooms/Description` metin arama için nasıl saklanır:

| Dönem`Rooms/Description` | Belge Kimlikleri |
| --- | --- |
| Avlu | 2 |
| city | 1 |
| Bahçe | 1 |
| Büyük | 1 |
| Motel | 2 |
| Oda | 1, 2 |
| Standart | 1 |
| Suite | 1 |
| görüntüle | 1 |

Yani yukarıdaki filtre, temelde "bir oda 'Deluxe `Type` Oda' eşit ve **aynı** oda `BaseRate` 100'den az olan eşleşen belgeler" `Rooms/Type` diyor, arama sorgusu `Rooms/Description` ""deluxe" terimi olan ve ifade "şehir görünümü" olan belgeleri maç diyor. Alanları ikinci durumda korelasyon olabilir bireysel odalar kavramı yoktur.

> [!NOTE]
> Azure Bilişsel Arama'ya eklenen ilişkili arama desteği görmek istiyorsanız, lütfen [bu Kullanıcı Sesi öğesi](https://feedback.azure.com/forums/263029-azure-search/suggestions/37735060-support-correlated-search-on-complex-collections)için oy verin.

## <a name="inverted-indexes-and-collections"></a>Ters dizinler ve koleksiyonlar

Karmaşık koleksiyonlar üzerinde lambda ifadeleri üzerinde `Collection(Edm.Int32)`çok daha az kısıtlama olduğunu fark etmiş `Collection(Edm.GeographyPoint)`olabilirsiniz gibi basit koleksiyonlar için vardır , , ve benzeri. Bunun nedeni, Azure Bilişsel Arama'nın karmaşık koleksiyonları gerçek alt belge koleksiyonları olarak depolaması ve basit koleksiyonların koleksiyon olarak depolanmasıdır.

Örneğin, çevrimiçi bir satıcı için `seasons` bir dizin gibi filtrelenebilir dize toplama alanını düşünün. Bu dizine yüklenen bazı belgeler aşağıdaki gibi görünebilir:

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

`seasons` Alanın değerleri ters **dizin**olarak adlandırılan bir yapıda depolanır, bu da şuna benzer:

| Sözleşme Dönemi | Belge Kimlikleri |
| --- | --- |
| Bahar | 1, 2 |
| Yaz | 1 |
| Sonbahar | 1, 2 |
| Kış | 2, 3 |

Bu veri yapısı bir soruyu büyük bir hızla yanıtlamak üzere tasarlanmıştır: Belirli bir terim hangi belgelerde görünür? Bu soruyu yanıtlamak, bir koleksiyon üzerindeki bir döngüden çok düz bir eşitlik denetimi gibi çalışır. Aslında, bu nedenle dize koleksiyonları için, Azure `eq` Bilişsel Arama sadece bir lambda ifade içinde bir karşılaştırma operatörü olarak izin verir `any`.

Eşitlikten bir iktisadi, sonra aynı aralık değişkeninde birden fazla eşitlik denetimini `or`'. Bu cebir ve [niceleyicilerin dağıtıcı özelliği](https://en.wikipedia.org/wiki/Existential_quantification#Negation)sayesinde çalışır. Bu ifade:

    seasons/any(s: s eq 'winter' or s eq 'fall')

eşdeğerdir:

    seasons/any(s: s eq 'winter') or seasons/any(s: s eq 'fall')

ve iki `any` alt ifadenin her biri ters dizini kullanılarak verimli bir şekilde yürütülebilir. Ayrıca, [niceleyicilerin olumsuzlama yasası](https://en.wikipedia.org/wiki/Existential_quantification#Negation)sayesinde, bu ifade:

    seasons/all(s: s ne 'winter' and s ne 'fall')

eşdeğerdir:

    not seasons/any(s: s eq 'winter' or s eq 'fall')

bu yüzden kullanmak `all` mümkündür `ne` ve `and`.

> [!NOTE]
> Ayrıntılar bu belgenin kapsamı dışında olsa da, aynı ilkeler [coğrafi-uzamsal noktaların koleksiyonları için mesafe ve kesişim testlerini](search-query-odata-geo-spatial-functions.md) de genişletir. Bu nedenle, `any`içinde:
>
> - `geo.intersects`inkar edilemez
> - `geo.distance`kullanılarak `lt` karşılaştırılmalıdır veya`le`
> - ifadeler ile `or`birleştirilmelidir , değil`and`
>
> Converse kuralları için `all`geçerlidir.

`lt` `gt`Örneğin, , , `le`, ve `ge` işleçleri `Collection(Edm.Int32)` destekleyen veri türleri koleksiyonlarına filtre leme yaparken çok çeşitli ifadelere izin verilir. Özellikle, yanı `and` sıra `or` kullanabilirsiniz `any`, sürece altta yatan karşılaştırma ifadeleri kullanarak `and` **aralık karşılaştırmalar** içine kombine `or`edilir , daha sonra daha sonra daha sonra kullanılarak birleştirilir . Boolean ifadelerinin bu yapısına [Disjunctive Normal Form (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form)denir, aksi takdirde "AND'lerin OR'ları" olarak bilinir. Tersine, bu veri türleri `all` için lambda ifadeleri [Konjonktif Normal Form (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form), aksi takdirde "ORs ANDs" olarak bilinen olmalıdır. Azure Bilişsel Arama, dizeleri hızlı dönem arama yapabileceği gibi, ters dizinleri kullanarak verimli bir şekilde çalıştırabildiği için bu tür aralık karşılaştırmalarına izin verir.

Özetle, burada bir lambda ifade izin verilen için başparmak kuralları şunlardır:

- `any`İçinde, *pozitif kontroller* her zaman izin verilir, `geo.intersects`eşitlik `geo.distance` gibi, aralık karşılaştırmalar, ya da ile `lt` karşılaştırıldığında veya `le` (bu mesafe kontrol söz konusu olduğunda eşitlik gibi olarak "yakınlık" düşünün).
- `any`İçinde, `or` her zaman izin verilir. Yalnızca aralık `and` denetimlerini ifade edebilen veri türleri için ve yalnızca AND'lerin (DNF) ORs'lerini kullanıyorsanız kullanabilirsiniz.
- İçinde, `all`kurallar tersine çevrilir - yalnızca *negatif denetimlere* izin verilir, her zaman kullanabilirsiniz `and` ve yalnızca OR (CNF) AND'leri olarak ifade edilen aralık denetimleri için kullanabilirsiniz. `or`

Uygulamada, bunlar zaten kullanma olasılığınız en yüksek olan filtre türleridir. Yine de neyin mümkün olduğunu anlamak yararlı olacaktır.

Hangi tür filtrelere izin verilen ve hangilerinin izin verilmeyen belirli örnekler için [geçerli toplama filtrelerinin nasıl yazıldığına](search-query-troubleshoot-collection-filters.md#bkmk_examples)bakın.

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'da OData toplama filtrelerini sorun giderme](search-query-troubleshoot-collection-filters.md)
- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
