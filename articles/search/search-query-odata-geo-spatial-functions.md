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
ms.openlocfilehash: 902996c1813931638012c78f81bd65c400bee7a1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74113169"
---
# <a name="odata-geo-spatial-functions-in-azure-cognitive-search---geodistance-and-geointersects"></a>Azure Bilişsel Arama OData coğrafi uzamsal işlevleri- `geo.distance` ve`geo.intersects`

Azure Bilişsel Arama, `geo.distance` ve `geo.intersects` işlevleri aracılığıyla [OData filtre ifadelerinde](query-odata-filter-orderby-syntax.md) coğrafi uzamsal sorguları destekler. `geo.distance` İşlevi iki noktası arasındaki mesafeyi kilometre cinsinden, biri alan veya Aralık değişkeni, diğeri ise filtrenin bir parçası olarak geçen bir sabit değer olarak döndürür. Bu `geo.intersects` işlev `true` , belirli bir noktanın belirli bir çokgen içindeyse, noktanın bir alan veya Aralık değişkeni olduğu ve çokgenin, filtrenin bir parçası olarak geçirildiği bir sabit olarak belirtildiği şekilde belirtilir.

`geo.distance` İşlev, arama sonuçlarını belirli bir noktadan uzaklığına göre sıralamak için [ **$OrderBy** parametresinde](search-query-odata-orderby.md) de kullanılabilir. `geo.distance` **$OrderBy** ' deki sözdizimi **$Filter**' deki ile aynıdır. $Orderby kullanılırken `geo.distance` , **$orderby**geçerli olduğu alan türünde `Edm.GeographyPoint` olmalıdır ve aynı zamanda **sıralanabilir**olmalıdır.

> [!NOTE]
> $Orderby parametresinde `geo.distance` kullanırken, **$orderby** işleve geçirdiğiniz alan yalnızca tek bir coğrafi nokta içermelidir. Diğer bir deyişle, türünde `Edm.GeographyPoint` olması gerekir. `Collection(Edm.GeographyPoint)` Azure Bilişsel Arama 'de koleksiyon alanlarını sıralamak mümkün değildir.

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
> [Azure Bilişsel Arama için OData sözdizimi diyagramı](https://azuresearch.github.io/odata-syntax-diagram/#geo_distance_call)

> [!NOTE]
> Tüm EBNF için bkz. [Azure bilişsel arama Için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md) .

### <a name="geodistance"></a>coğrafi. mesafe

İşlevi `geo.distance` , türünde `Edm.GeographyPoint` iki parametre alır ve bunlar arasındaki mesafe `Edm.Double` , kilometre cinsinden bir değer döndürür. Bu, genellikle ölçümlerdeki uzaklıkları döndüren OData coğrafi uzamsal işlemlerini destekleyen diğer hizmetlerden farklıdır.

Parametrelerinden birinin bir Coğrafya noktası `geo.distance` sabiti olması gerekir ve diğeri, bir alan yolu olmalıdır (veya bir filtre olması durumunda bir Aralık değişkeni olması gerekir `Collection(Edm.GeographyPoint)`). Bu parametrelerin sırası böyle değildir.

Coğrafya noktası sabiti, boylam ve enlem 'nin `geography'POINT(<longitude> <latitude>)'`Sayısal sabitler olduğu formdur.

> [!NOTE]
> Bir filtrede `geo.distance` kullanırken, `lt`,, veya `le` `gt` `ge`kullanarak işlev tarafından döndürülen uzaklığı bir sabit ile karşılaştırmalısınız. Uzaklıklar `eq` karşılaştırılırken `ne` işleçler ve desteklenmez. Örneğin, bu doğru bir kullanımdır `geo.distance`:. `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`

### <a name="geointersects"></a>coğrafi. kesişiyor

`geo.intersects` İşlevi, türünde `Edm.GeographyPoint` bir değişken ve `Edm.GeographyPolygon` bir sabiti alır ve nokta çokgenin sınırları `Edm.Boolean`  --  `true` içindeyse, `false` Aksi takdirde bir değeri döndürür.

Çokgen, bir sınırlayıcı halkasını tanımlayan bir punto sırası olarak depolanan iki boyutlu bir yüzeydir (aşağıdaki [örneklere](#examples) bakın). Poligonun kapatılması gerekir, yani ilk ve son nokta kümeleri aynı olmalıdır. [Bir çokgen Içindeki noktaların saatin ters sırada olması gerekir](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

### <a name="geo-spatial-queries-and-polygons-spanning-the-180th-meridian"></a>180. Meridyen 'i kapsayan coğrafi uzamsal sorgular ve çokgenler

Çok sayıda coğrafi uzamsal sorgu kitaplığı, 180. meriyen 'i (Dateline yakınında) içeren bir sorguyu formül dışında sınırlar veya bir geçici çözüm gerektirir (örneğin, her iki tarafında da çokgeni ikiye bölmek gibi).

Azure Bilişsel Arama, sorgu şekli dikdörtgen ise ve koordinatlarınız boylam ve Enlem (örneğin, `geo.intersects(location, geography'POLYGON((179 65, 179 66, -179 66, -179 65, 179 65))'`) üzerinde bir ızgara düzenine hizalandıysanız, 180 derece boylam içeren coğrafi uzamsal sorgular beklendiği gibi çalışacaktır. Aksi halde, dikdörtgen olmayan veya hizalanmamış şekiller için bölünmüş Çokgen yaklaşımını göz önünde bulundurun.  

### <a name="geo-spatial-functions-and-null"></a>Coğrafi uzamsal işlevler ve`null`

Azure Bilişsel Arama içindeki diğer tüm koleksiyon olmayan alanlar gibi, türündeki `Edm.GeographyPoint` alanlar değer içerebilir. `null` Azure Bilişsel Arama, olan `geo.intersects` bir alanı değerlendirirken `null`, sonuç her zaman olur `false`. Bu durumda öğesinin `geo.distance` davranışı, bağlama göre değişir:

- Bir `geo.distance` `null` alanın filtrelerinde sonucu olur `null`. Bu, null olmayan hiçbir değere `null` karşılık olarak değerlendirilmediği için `false`belgenin eşleşmeyeceği anlamına gelir.
- Sonuçları **$OrderBy**kullanarak sıralarken, `geo.distance` bir `null` alanın en fazla olası uzaklığa neden olur. Bu alan içeren belgeler, sıralama yönü `asc` kullanıldığında (varsayılan) ve diğer tüm diğerlerine göre daha yükseği olduğunda, bu alana sahip olan belgeler diğerlerinden daha düşük sıralanır. `desc`

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

- [Azure Bilişsel Arama filtreler](search-filters.md)
- [Azure Bilişsel Arama için OData ifade diline genel bakış](query-odata-filter-orderby-syntax.md)
- [Azure Bilişsel Arama için OData ifadesi söz dizimi başvurusu](search-query-odata-syntax-reference.md)
- [Azure Bilişsel Arama REST API &#40;belgelerde arama yapın&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
