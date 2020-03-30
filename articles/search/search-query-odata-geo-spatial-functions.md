---
title: OData jeo-uzamsal fonksiyon başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında OData coğrafi uzamsal işlevleri, geo.distance ve geo.kesişmelerini kullanmak için sözdizimi ve başvuru belgeleri.
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
ms.openlocfilehash: 902996c1813931638012c78f81bd65c400bee7a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74113169"
---
# <a name="odata-geo-spatial-functions-in-azure-cognitive-search---geodistance-and-geointersects"></a>Azure Bilişsel Arama'da OData `geo.distance` coğrafi uzamsal işlevler - ve`geo.intersects`

Azure Bilişsel Arama, [OData filtre ifadelerindeki](query-odata-filter-orderby-syntax.md) coğrafi `geo.distance` uzamsal sorguları ve `geo.intersects` işlevleri aracılığıyla destekler. İşlev, `geo.distance` biri alan veya aralık değişkeni olmak üzere iki nokta arasındaki kilometre mesafesini, diğeri filtrenin bir parçası olarak geçirilen sabit olan mesafeyi döndürür. Belirli `geo.intersects` bir `true` nokta belirli bir çokgen içindeyse işlev döner, nokta bir alan veya aralık değişkeni ve çokgen filtrenin bir parçası olarak geçirilen sabit olarak belirtilir.

İşlev, `geo.distance` arama sonuçlarını belirli bir noktadan uzaklıklara göre sıralamak için [ **$orderby** parametresinde](search-query-odata-orderby.md) de kullanılabilir. **$orderby** için `geo.distance` sözdizimi **$filter**ile aynıdır. **$orderby** `geo.distance` kullanırken, uygulandığı alan türde `Edm.GeographyPoint` olmalı ve aynı zamanda **sıralanabilir**olmalıdır.

> [!NOTE]
> **$orderby** `geo.distance` parametresini kullanırken, işleve geçtiğiniz alanın yalnızca tek bir coğrafi nokta içermesi gerekir. Başka bir deyişle, türü `Edm.GeographyPoint` değil `Collection(Edm.GeographyPoint)`olmalıdır. Azure Bilişsel Arama'da toplama alanlarında sıralama yapmak mümkün değildir.

## <a name="syntax"></a>Sözdizimi

Aşağıdaki EBNF[(Genişletilmiş Backus-Naur Formu)](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)işlevlerin `geo.distance` ve `geo.intersects` işlevlerin gramerini ve üzerinde çalıştıkları jeo-uzamsal değerleri tanımlar:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*
```

Etkileşimli sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#geo_distance_call)

> [!NOTE]
> EBNF'nin tamamı [için Azure Bilişsel Arama için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

### <a name="geodistance"></a>geo.distance

İşlev `geo.distance` türüiki `Edm.GeographyPoint` parametre alır `Edm.Double` ve kilometre aralarında ki mesafe bir değer döndürür. Bu, genellikle mesafeleri metreler halinde döndüren OData coğrafi-uzamsal işlemleri destekleyen diğer hizmetlerden farklıdır.

Bir coğrafya noktası `geo.distance` sabit için parametrelerden biri ve diğeri ise bir alan yolu (veya bir filtre `Collection(Edm.GeographyPoint)`nin bir türü üzerinde tekrarlama durumunda bir araştırıcı) olmalıdır. Bu parametrelerin sırası önemli değil.

Coğrafya noktası sabiti, `geography'POINT(<longitude> <latitude>)'`boylam ve enlemin sayısal sabitler olduğu biçimdendir.

> [!NOTE]
> Bir `geo.distance` filtre kullanırken, işlev tarafından döndürülen mesafeyi sabit `lt` `le`, `gt`, `ge`veya . İşleçler `eq` ve `ne` mesafeler karşılaştırılırken desteklenmez. Örneğin, bu doğru bir `geo.distance`kullanım: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.

### <a name="geointersects"></a>geo.kesişiyor

İşlev `geo.intersects` `Edm.GeographyPoint` bir tür değişkeni `Edm.GeographyPolygon` ve sabiti alır ve nokta çokgenin sınırları `false` içindeyse, aksi takdirde bir `Edm.Boolean`  --  `true` değişken verir.

Çokgen, sınırlayıcı bir halkayı tanımlayan noktalar dizisi olarak depolanan iki boyutlu bir yüzeydir (aşağıdaki [örneklere](#examples) bakın). Çokgenin kapatılması gerekir, yani ilk ve son nokta kümeleri aynı olmalıdır. [Çokgendeki noktalar saat yönünün tersine olmalıdır.](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1)

### <a name="geo-spatial-queries-and-polygons-spanning-the-180th-meridian"></a>180. meridyen kapsayan jeo-uzamsal sorgular ve çokgenler

180. meridyeni (dateline yakın) içeren bir sorgu formüle eden birçok coğrafi uzamsal sorgu kitaplığı için ya yasaktır ya da çokgenin ikiye bölünmesi gibi bir geçici çözüm gerektirir, meridyenin her iki tarafında bir.

Azure Bilişsel Arama'da, sorgu şekli dikdörtgen şeklindeyse ve koordinatlarınız boylam ve enlem boyunca (örneğin) `geo.intersects(location, geography'POLYGON((179 65, 179 66, -179 66, -179 65, 179 65))'`Bir ızgara düzenine hizalanırsa, 180 derece boylam içeren coğrafi uzamsal sorgular beklendiği gibi çalışır. Aksi takdirde, dikdörtgen olmayan veya hizalanmamış şekiller için, bölünmüş çokgen yaklaşımını göz önünde bulundurun.  

### <a name="geo-spatial-functions-and-null"></a>Jeo-uzamsal fonksiyonlar ve`null`

Azure Bilişsel Arama'daki diğer tüm koleksiyon dışı `Edm.GeographyPoint` alanlar `null` gibi, tür alanları da değerler içerebilir. Azure Bilişsel Arama `geo.intersects` olan bir alanı `null`değerlendirdiğinde, sonuç `false`her zaman . Bu durumda `geo.distance` davranışı bağlam bağlıdır:

- Filtrelerde, `geo.distance` bir `null` alanın . `null` Bu, herhangi bir null `null` olmayan değer ile karşılaştırıldığında `false`değerlendirildiği için belgenin eşleşmeyeceği anlamına gelir.
- Sonuçları **$orderby**kullanarak `geo.distance` sıralarken, `null` bir alanın alanı mümkün olan en yüksek mesafeyle sonuçlanır. Bu tür bir alana sahip belgeler, sıralama `asc` yönü kullanıldığında diğerlerinden daha düşük sıralanır (varsayılan) ve yön. `desc`

## <a name="examples"></a>Örnekler

### <a name="filter-examples"></a>Filtre örnekleri

Belirli bir referans noktasına 10 kilometre mesafedeki tüm otelleri `Edm.GeographyPoint`bulun (konumun bir tür alanı olduğu yer):

    geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10

Çokgen olarak tanımlanan belirli bir görünüm portu içindeki tüm otelleri `Edm.GeographyPoint`bulun (konumun bir tür alanı olduğu). Çokgenin kapalı olduğunu (ilk ve son nokta kümelerinin aynı olması gerektiğini) ve [noktaların saat yönünün tersine sırayla listelemesi gerektiğini](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1)unutmayın.

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

### <a name="order-by-examples"></a>Siparişe göre örnekler

Otellerin `rating`azalan, daha sonra verilen koordinatlardan uzaklığa göre artan sırala:

    rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

Otelleri azalan sıraya göre `search.score` `rating`sıralayın ve sonra da verilen koordinatlardan uzaklığa göre artan sırada sıralayın, böylece aynı derecelendirmeye sahip iki otel arasında en yakın ıstabir ilk sırada listelenir:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama'daki Filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade dili genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifade sözdizimi başvurusu](search-query-odata-syntax-reference.md)
- [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
