---
title: OData coğrafi uzamsal işlev başvurusu
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama sorgularında, OData coğrafi uzamsal işlevleri, coğrafi. uzaklık ve coğrafi. kesişimleri kullanımı için sözdizimi ve başvuru belgeleri.
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
ms.openlocfilehash: 376cece922ca424ec78011224852b1fa5499da16
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934846"
---
# <a name="odata-geo-spatial-functions-in-azure-cognitive-search---geodistance-and-geointersects"></a>Azure Bilişsel Arama OData coğrafi uzamsal işlevleri- `geo.distance` ve `geo.intersects`

Azure Bilişsel Arama, ve işlevleri aracılığıyla [OData filtre ifadelerinde](query-odata-filter-orderby-syntax.md) coğrafi uzamsal sorguları destekler `geo.distance` `geo.intersects` . `geo.distance`İşlevi iki noktası arasındaki mesafeyi kilometre cinsinden, biri alan veya Aralık değişkeni, diğeri ise filtrenin bir parçası olarak geçen bir sabit değer olarak döndürür. Bu `geo.intersects` işlev, `true` belirli bir noktanın belirli bir çokgen içindeyse, noktanın bir alan veya Aralık değişkeni olduğu ve çokgenin, filtrenin bir parçası olarak geçirildiği bir sabit olarak belirtildiği şekilde belirtilir.

`geo.distance`İşlev, arama sonuçlarını belirli bir noktadan uzaklığına göre sıralamak için [ **$OrderBy** parametresinde](search-query-odata-orderby.md) de kullanılabilir. `geo.distance` **$OrderBy** ' deki sözdizimi **$Filter**' deki ile aynıdır. $Orderby kullanılırken `geo.distance` , **$orderby**geçerli olduğu alan türünde olmalıdır `Edm.GeographyPoint` ve aynı zamanda **sıralanabilir**olmalıdır.

> [!NOTE]
> `geo.distance` **$OrderBy** parametresinde kullanırken, işleve geçirdiğiniz alan yalnızca tek bir coğrafi nokta içermelidir. Diğer bir deyişle, türünde olması gerekir `Edm.GeographyPoint` `Collection(Edm.GeographyPoint)` . Azure Bilişsel Arama 'de koleksiyon alanlarını sıralamak mümkün değildir.

## <a name="syntax"></a>Syntax

Aşağıdaki EBNF ([Genişletilmiş Backus-Naur formu](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)), `geo.distance` ve işlevlerinin dilbilgisini ve `geo.intersects` üzerinde çalıştıkları coğrafi uzamsal değerleri tanımlar:

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

Etkileşimli bir sözdizimi diyagramı da kullanılabilir:

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#geo_distance_call)

> [!NOTE]
> Tüm EBNF için bkz. [Azure bilişsel arama Için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md) .

### <a name="geodistance"></a>coğrafi. mesafe

`geo.distance`İşlevi, türünde iki parametre alır `Edm.GeographyPoint` ve `Edm.Double` bunlar arasındaki mesafe, kilometre cinsinden bir değer döndürür. Bu, genellikle ölçümlerdeki uzaklıkları döndüren OData coğrafi uzamsal işlemlerini destekleyen diğer hizmetlerden farklıdır.

Parametrelerinden birinin bir `geo.distance` Coğrafya noktası sabiti olması gerekir ve diğeri, bir alan yolu olmalıdır (veya bir filtre olması durumunda bir Aralık değişkeni olması gerekir `Collection(Edm.GeographyPoint)` ). Bu parametrelerin sırası böyle değildir.

Coğrafya noktası sabiti, `geography'POINT(<longitude> <latitude>)'` boylam ve enlem 'nin Sayısal sabitler olduğu formdur.

> [!NOTE]
> Bir filtrede kullanırken,,, `geo.distance` veya kullanarak işlev tarafından döndürülen uzaklığı bir sabit ile karşılaştırmalısınız `lt` `le` `gt` `ge` . `eq` `ne` Uzaklıklar karşılaştırılırken işleçler ve desteklenmez. Örneğin, bu doğru bir kullanımdır `geo.distance` : `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5` .

### <a name="geointersects"></a>coğrafi. kesişiyor

`geo.intersects`İşlevi, türünde bir değişken ve bir `Edm.GeographyPoint` sabiti alır `Edm.GeographyPolygon` ve `Edm.Boolean`  --  `true` nokta çokgenin sınırları içindeyse, aksi takdirde bir değeri döndürür `false` .

Çokgen, bir sınırlayıcı halkasını tanımlayan bir punto sırası olarak depolanan iki boyutlu bir yüzeydir (aşağıdaki [örneklere](#examples) bakın). Poligonun kapatılması gerekir, yani ilk ve son nokta kümeleri aynı olmalıdır. [Bir çokgen Içindeki noktaların saatin ters sırada olması gerekir](/rest/api/searchservice/supported-data-types#Anchor_1).

### <a name="geo-spatial-queries-and-polygons-spanning-the-180th-meridian"></a>180. Meridyen 'i kapsayan coğrafi uzamsal sorgular ve çokgenler

Çok sayıda coğrafi uzamsal sorgu kitaplığı, 180. meriyen 'i (Dateline yakınında) içeren bir sorguyu formül dışında sınırlar veya bir geçici çözüm gerektirir (örneğin, her iki tarafında da çokgeni ikiye bölmek gibi).

Azure Bilişsel Arama, sorgu şekli dikdörtgen ise ve koordinatlarınız boylam ve Enlem (örneğin,) üzerinde bir ızgara düzenine hizalandıysanız, 180 derece boylam içeren coğrafi uzamsal sorgular beklendiği gibi çalışacaktır `geo.intersects(location, geography'POLYGON((179 65, 179 66, -179 66, -179 65, 179 65))'` . Aksi halde, dikdörtgen olmayan veya hizalanmamış şekiller için bölünmüş Çokgen yaklaşımını göz önünde bulundurun.  

### <a name="geo-spatial-functions-and-null"></a>Coğrafi uzamsal işlevler ve `null`

Azure Bilişsel Arama içindeki diğer tüm koleksiyon olmayan alanlar gibi, türündeki alanlar `Edm.GeographyPoint` `null` değer içerebilir. Azure Bilişsel Arama, `geo.intersects` olan bir alanı değerlendirirken `null` , sonuç her zaman olur `false` . Bu durumda öğesinin davranışı, `geo.distance` bağlama göre değişir:

- `geo.distance`Bir alanın filtrelerinde `null` sonucu olur `null` . Bu, `null` null olmayan hiçbir değere karşılık olarak değerlendirilmediği için belgenin eşleşmeyeceği anlamına gelir `false` .
- Sonuçları **$OrderBy**kullanarak sıralarken, `geo.distance` bir `null` alanın en fazla olası uzaklığa neden olur. Bu alan içeren belgeler, sıralama yönü `asc` kullanıldığında (varsayılan) ve diğer tüm diğerlerine göre daha yükseği olduğunda, bu alana sahip olan belgeler diğerlerinden daha düşük sıralanır `desc` .

## <a name="examples"></a>Örnekler

### <a name="filter-examples"></a>Filtre örnekleri

Belirli bir başvuru noktasındaki 10 kiloters içindeki tüm oteller bul (konum, konum türünde bir alandır `Edm.GeographyPoint` ):

```odata-filter-expr
    geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10
```

Belirli bir görünüm içindeki tüm otelleri bir çokgen (konum türünde bir alandır) olarak tanımlanan bir görünüm içinde bulun `Edm.GeographyPoint` . Çokgenin kapatıldığını unutmayın (ilk ve son nokta kümeleri aynı olmalıdır) ve [noktaların saatin tersi sırada listelenmesi gerekir](/rest/api/searchservice/supported-data-types#Anchor_1).

```odata-filter-expr
    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')
```

### <a name="order-by-examples"></a>Sıralama örnekleri

Oteller `rating` , daha sonra verilen koordinatlardan uzaklıktan artan düzende sıralayın:

```odata-filter-expr
    rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```

Oteller `search.score` ve `rating` ardından, aynı derecelendirmelere sahip iki otel arasında, en yakın bir değer olacak şekilde, ve daha sonra verilen koordinatlardan uzaklıktan artan düzende bir şekilde sıralayın.

```odata-filter-expr
    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc
```

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Bilişsel Arama filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](/rest/api/searchservice/Search-Documents)