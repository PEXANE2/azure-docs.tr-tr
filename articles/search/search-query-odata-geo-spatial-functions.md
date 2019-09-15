---
title: OData coğrafi uzamsal işlev başvurusu-Azure Search
description: Azure Search sorgularda OData coğrafi uzamsal işlevler, coğrafi. uzaklık ve coğrafi. kesişiyor.
ms.date: 09/13/2019
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
ms.openlocfilehash: 03220786c65ab510a632252b20d593cd96a90494
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003450"
---
# <a name="odata-geo-spatial-functions-in-azure-search---geodistance-and-geointersects"></a>Azure Search `geo.distance` ve içindeki OData coğrafi uzamsal işlevleri`geo.intersects`

Azure Search, `geo.distance` ve `geo.intersects` işlevleri aracılığıyla [OData filtre ifadelerinde](query-odata-filter-orderby-syntax.md) coğrafi uzamsal sorguları destekler. `geo.distance` İşlevi iki noktası arasındaki mesafeyi kilometre cinsinden, biri alan veya Aralık değişkeni, diğeri ise filtrenin bir parçası olarak geçen bir sabit değer olarak döndürür. `geo.intersects` Bu`true` işlev, belirli bir noktanın belirli bir çokgen içindeyse, noktanın bir alan veya Aralık değişkeni olduğu ve çokgenin, filtrenin bir parçası olarak geçirildiği bir sabit olarak belirtildiği şekilde belirtilir.

İşlev, arama sonuçlarını belirli bir noktadan uzaklığına göre sıralamak için [ **$OrderBy** parametresinde](search-query-odata-orderby.md) de kullanılabilir. `geo.distance` `geo.distance` **$OrderBy** ' deki sözdizimi **$Filter**' deki ile aynıdır. $OrderBy kullanılırken `geo.distance` ,geçerli olduğu alan türünde `Edm.GeographyPoint` olmalıdır ve aynı zamanda **sıralanabilir**olmalıdır.

> [!NOTE]
> $OrderBy parametresinde `geo.distance` kullanırken, işleve geçirdiğiniz alan yalnızca tek bir coğrafi nokta içermelidir. Diğer bir deyişle, türünde `Edm.GeographyPoint` `Collection(Edm.GeographyPoint)`olması gerekir. Azure Search içindeki koleksiyon alanlarında sıralama yapılamaz.

## <a name="syntax"></a>Sözdizimi

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
> [Azure Search için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#geo_distance_call)

> [!NOTE]
> Tüm EBNF için [Azure Search Için OData ifade sözdizimi başvurusuna](search-query-odata-syntax-reference.md) bakın.

### <a name="geodistance"></a>coğrafi. mesafe

İşlevi `geo.distance` , türünde `Edm.GeographyPoint` iki parametre alır ve bunlar arasındaki mesafe `Edm.Double` , kilometre cinsinden bir değer döndürür. Bu, genellikle ölçümlerdeki uzaklıkları döndüren OData coğrafi uzamsal işlemlerini destekleyen diğer hizmetlerden farklıdır.

Parametrelerinden `geo.distance` birinin bir Coğrafya noktası sabiti olması gerekir ve diğeri, bir alan yolu olmalıdır (veya bir filtre `Collection(Edm.GeographyPoint)`olması durumunda bir Aralık değişkeni olması gerekir). Bu parametrelerin sırası böyle değildir.

Coğrafya noktası sabiti, boylam ve enlem 'nin `geography'POINT(<longitude> <latitude>)'`Sayısal sabitler olduğu formdur.

> [!NOTE]
> Bir filtrede `geo.distance` kullanırken,,, veya `gt` `lt` `le` kullanarak`ge`işlev tarafından döndürülen uzaklığı bir sabit ile karşılaştırmalısınız. Uzaklıklar karşılaştırılırken `ne` işleçler `eq` ve desteklenmez. Örneğin, bu doğru bir kullanımdır `geo.distance`:. `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`

### <a name="geointersects"></a>coğrafi. kesişiyor

`Edm.Boolean` `Edm.GeographyPolygon`  --  `false` `true` İşlevi, türünde `Edm.GeographyPoint` bir değişken ve bir sabiti alır ve nokta çokgenin sınırları içindeyse, aksi takdirde bir değeri döndürür. `geo.intersects`

Çokgen, bir sınırlayıcı halkasını tanımlayan bir punto sırası olarak depolanan iki boyutlu bir yüzeydir (aşağıdaki [örneklere](#examples) bakın). Poligonun kapatılması gerekir, yani ilk ve son nokta kümeleri aynı olmalıdır. [Bir çokgen Içindeki noktaların saatin ters sırada olması gerekir](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

### <a name="geo-spatial-queries-and-polygons-spanning-the-180th-meridian"></a>180. Meridyen 'i kapsayan coğrafi uzamsal sorgular ve çokgenler

Çok sayıda coğrafi uzamsal sorgu kitaplığı, 180. meriyen 'i (Dateline yakınında) içeren bir sorguyu formül dışında sınırlar veya bir geçici çözüm gerektirir (örneğin, her iki tarafında da çokgeni ikiye bölmek gibi).

Azure Search, sorgu şekli dikdörtgen ise ve koordinatlarınız boylam ve Enlem (örneğin, `geo.intersects(location, geography'POLYGON((179 65, 179 66, -179 66, -179 65, 179 65))'`) üzerinde bir ızgara düzenine hizalandıysanız, 180 derece boylam içeren coğrafi uzamsal sorgular beklendiği gibi çalışır. Aksi halde, dikdörtgen olmayan veya hizalanmamış şekiller için bölünmüş Çokgen yaklaşımını göz önünde bulundurun.  

### <a name="geo-spatial-functions-and-null"></a>Coğrafi uzamsal işlevler ve`null`

Azure Search içindeki diğer tüm koleksiyon olmayan alanlar gibi, türündeki `Edm.GeographyPoint` alanlar değer içerebilir. `null` Bir alan `geo.intersects` `false`için Azure Search değerlendirirken, sonuç her zaman olur. `null` Bu durumda öğesinin `geo.distance` davranışı, bağlama göre değişir:

- `null`Bir `geo.distance` alanınfiltrelerindesonucuolur`null` . Bu, null olmayan hiçbir değere `null` karşılık olarak değerlendirilmediği için `false`belgenin eşleşmeyeceği anlamına gelir.
- Sonuçları **$OrderBy**kullanarak sıralarken, `geo.distance` bir `null` alanın en fazla olası uzaklığa neden olur. Bu alan içeren belgeler, sıralama yönü `asc` kullanıldığında (varsayılan) ve diğer tüm diğerlerine `desc`göre daha yükseği olduğunda, bu alana sahip olan belgeler diğerlerinden daha düşük sıralanır.

## <a name="examples"></a>Örnekler

### <a name="filter-examples"></a>Filtre örnekleri

Belirli bir başvuru noktasındaki 10 kiloters içindeki tüm oteller bul (konum, konum türünde `Edm.GeographyPoint`bir alandır):

    geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10

Belirli bir görünüm içindeki tüm otelleri bir çokgen (konum türünde `Edm.GeographyPoint`bir alandır) olarak tanımlanan bir görünüm içinde bulun. Çokgenin kapatıldığını unutmayın (ilk ve son nokta kümeleri aynı olmalıdır) ve [noktaların saatin tersi sırada listelenmesi gerekir](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

### <a name="order-by-examples"></a>Sıralama örnekleri

Oteller `rating`, daha sonra verilen koordinatlardan uzaklıktan artan düzende sıralayın:

    rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

Oteller `search.score` ve ardından, aynı derecelendirmelere `rating`sahip iki otel arasında, en yakın bir değer olacak şekilde, ve daha sonra verilen koordinatlardan uzaklıktan artan düzende bir şekilde sıralayın.

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Sonraki adımlar  

- [Azure Search filtreler](search-filters.md)
- [Azure Search için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Search için OData ifade söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
