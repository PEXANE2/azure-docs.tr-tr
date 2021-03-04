---
title: Android haritalar 'da veri odaklı stil Ifadeleri | Microsoft Azure haritaları
description: Veri odaklı stil ifadeleri hakkında bilgi edinin. Haritaların stillerini ayarlamak için Azure Maps Android SDK bu ifadeleri nasıl kullanacağınızı öğrenin.
author: rbrundritt
ms.author: richbrun
ms.date: 2/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 1babf1feb550109486089c45469ab4ce32f72cb3
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097423"
---
# <a name="data-driven-style-expressions-android-sdk"></a>Veri tabanlı stil ifadeleri (Android SDK)

İfadeler, bir veri kaynağındaki her bir şekil için tanımlanan özellikleri gözlemleyecek stil seçeneklerine iş mantığı uygulamanızı sağlar. İfadeler, bir veri kaynağındaki veya katmandaki verileri filtreleyebilir. İfadeler, if-deyimleri gibi koşullu mantığdan oluşabilir. Ayrıca, bunları kullanarak verileri işlemek için kullanılabilir: dize işleçleri, mantıksal işleçler ve matematik işleçleri.

Veri odaklı stiller, Stillendirme etrafında iş mantığını uygulamak için gereken kod miktarını azaltır. Katmanlarla kullanıldığında, ifadeler ayrı bir iş parçacığında işleme zamanında değerlendirilir. Bu işlevsellik, UI iş parçacığında iş mantığını değerlendirmeye kıyasla daha yüksek performans sağlar.

Azure Haritalar Android SDK Azure Maps web SDK 'Sı ile neredeyse tüm aynı stil ifadelerini destekler, böylece [veri odaklı stil ifadelerinde (Web SDK)](data-driven-style-expressions-web-sdk.md) özetlenen tüm kavramlar bir Android uygulamasına taşınır. Azure Haritalar Android SDK tüm stil ifadeleri `com.microsoft.azure.maps.mapcontrol.options.Expression` ad alanı altında bulunabilir. Birçok farklı tür stil ifadesi vardır.

| İfade türü | Açıklama |
|---------------------|-------------|
| [Mantıksal ifadeler](#boolean-expressions) | Boolean ifadeleri, Boole karşılaştırmaları değerlendirmek için bir dizi Boole işleci sağlar. |
| [Renk ifadeleri](#color-expressions) | Renk ifadeleri renk değerleri oluşturmayı ve işlemeyi kolaylaştırır. |
| [Koşullu ifadeler](#conditional-expressions) | Koşullu ifadeler, IF-deyimleri gibi mantıksal işlemler sağlar. |
| [Veri ifadeleri](#data-expressions) | Bir özelliğindeki Özellik verilerine erişim sağlar. |
| [Ara ve adım ifadelerini enterpolala](#interpolate-and-step-expressions) | Enterpolasyonlu ve step ifadeleri, bir ara değerli eğri veya adım işlevi üzerinde değerleri hesaplamak için kullanılabilir. |
| [JSON tabanlı ifadeler](#json-based-expressions) | , Web SDK için oluşturulan stil ham JSON tabanlı ifadelerin Android SDK birlikte yeniden kullanılmasını kolaylaştırır. |  
| [Katmana özgü ifadeler](#layer-specific-expressions) | Yalnızca tek bir katman için geçerli olan özel ifadeler. |
| [Matematik ifadeleri](#math-expressions) | Expression Framework içinde veri odaklı hesaplamalar gerçekleştirmek için matematik işleçleri sağlar. |
| [Dize işleci ifadeleri](#string-operator-expressions) | Dize operatörü ifadeleri, büyük/küçük harfe dönüştürme ve dönüştürme gibi dizeler üzerinde dönüştürme işlemleri gerçekleştirir. |
| [Tür ifadeleri](#type-expressions) | Tür ifadeleri, dizeler, sayılar ve Boole değerleri gibi farklı veri türlerini test etmek ve dönüştürmek için araçlar sağlar. |
| [Değişken bağlama ifadeleri](#variable-binding-expressions) | Değişken bağlama ifadeleri, bir hesaplamanın sonuçlarını bir değişkende depolar ve depolanan değeri yeniden hesaplamaya gerek kalmadan bir ifadenin başka bir yerinde birden çok kez başvurulur. |
| [Yakınlaştırma ifadesi](#zoom-expression) | İşleme zamanında haritanın geçerli yakınlaştırma düzeyini alır. |

> [!NOTE]
> İfadelerin sözdizimi büyük ölçüde Java ve Kotlin içinde aynıdır. Belgeler Kotlin olarak ayarlandıysa, ancak Java için kod blokları ' na sahipseniz, kod her iki dilde de aynıdır.

Belgenin bu bölümündeki tüm örnekler, bu ifadelerin kullanılabileceği farklı yolları göstermek için aşağıdaki özelliği kullanır.

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.13284, 47.63699]
    },
    "properties": {
        "id": 123,
        "entityType": "restaurant",
        "revenue": 12345,
        "subTitle": "Building 40", 
        "temperature": 64,
        "title": "Cafeteria", 
        "zoneColor": "purple",
        "abcArray": ["a", "b", "c"],
        "array2d": [["a", "b"], ["x", "y"]],
        "_style": {
            "fillColor": "red"
        }
    }
}
```

Aşağıdaki kod, bu coğrafi JSON özelliğinin bir uygulamada el ile nasıl oluşturulacağını gösterir.

::: zone pivot="programming-language-java-android"

```Java
//Create a point feature.
Feature feature = Feature.fromGeometry(Point.fromLngLat(-100, 45));

//Add properties to the feature.
feature.addNumberProperty("id", 123);
feature.addStringProperty("entityType", "restaurant");
feature.addNumberProperty("revenue", 12345);
feature.addStringProperty("subTitle", "Building 40");
feature.addNumberProperty("temperature", 64);
feature.addStringProperty("title", "Cafeteria");
feature.addStringProperty("zoneColor", "purple");

JsonArray abcArray = new JsonArray();
abcArray.add("a");
abcArray.add("b");
abcArray.add("c");

feature.addProperty("abcArray", abcArray);

JsonArray array1 = new JsonArray();
array1.add("a");
array1.add("b");

JsonArray array2 = new JsonArray();
array1.add("x");
array1.add("y");

JsonArray array2d = new JsonArray();
array2d.add(array1);
array2d.add(array2);

feature.addProperty("array2d", array2d);

JsonObject style = new JsonObject();
style.addProperty("fillColor", "red");

feature.addProperty("_style", style);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a point feature.
val feature = Feature.fromGeometry(Point.fromLngLat(-100, 45))

//Add properties to the feature.
feature.addNumberProperty("id", 123)
feature.addStringProperty("entityType", "restaurant")
feature.addNumberProperty("revenue", 12345)
feature.addStringProperty("subTitle", "Building 40")
feature.addNumberProperty("temperature", 64)
feature.addStringProperty("title", "Cafeteria")
feature.addStringProperty("zoneColor", "purple")

val abcArray = JsonArray()
abcArray.add("a")
abcArray.add("b")
abcArray.add("c")

feature.addProperty("abcArray", abcArray)

val array1 = JsonArray()
array1.add("a")
array1.add("b")

val array2 = JsonArray()
array1.add("x")
array1.add("y")

val array2d = JsonArray()
array2d.add(array1)
array2d.add(array2)

feature.addProperty("array2d", array2d)

val style = JsonObject()
style.addProperty("fillColor", "red")

feature.addProperty("_style", style)
```

::: zone-end

Aşağıdaki kod, JSON nesnesinin strıngıt sürümünün bir uygulamadaki GeoJSON özelliğine nasıl seri hale alınacağını gösterir.

::: zone pivot="programming-language-java-android"

```java
String featureString = "{\"type\":\"Feature\",\"geometry\":{\"type\":\"Point\",\"coordinates\":[-122.13284,47.63699]},\"properties\":{\"id\":123,\"entityType\":\"restaurant\",\"revenue\":12345,\"subTitle\":\"Building 40\",\"temperature\":64,\"title\":\"Cafeteria\",\"zoneColor\":\"purple\",\"abcArray\":[\"a\",\"b\",\"c\"],\"array2d\":[[\"a\",\"b\"],[\"x\",\"y\"]],\"_style\":{\"fillColor\":\"red\"}}}";

Feature feature = Feature.fromJson(featureString);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val featureString = "{\"type\":\"Feature\",\"geometry\":{\"type\":\"Point\",\"coordinates\":[-122.13284,47.63699]},\"properties\":{\"id\":123,\"entityType\":\"restaurant\",\"revenue\":12345,\"subTitle\":\"Building 40\",\"temperature\":64,\"title\":\"Cafeteria\",\"zoneColor\":\"purple\",\"abcArray\":[\"a\",\"b\",\"c\"],\"array2d\":[[\"a\",\"b\"],[\"x\",\"y\"]],\"_style\":{\"fillColor\":\"red\"}}}"

val feature = Feature.fromJson(featureString)
```

::: zone-end

## <a name="json-based-expressions"></a>JSON tabanlı ifadeler

Azure Haritalar Web SDK 'Sı Ayrıca bir JSON dizisi kullanılarak temsil edilen veri tabanlı stil ifadelerini destekler. Bu aynı ifadeler Android SDK yerel sınıfı kullanılarak yeniden oluşturulabilir `Expression` . Alternatif olarak, bu JSON tabanlı ifadeler, gibi bir Web işlevi kullanılarak bir dizeye dönüştürülebilir `JSON.stringify` ve `Expression.raw(String rawExpression)` yöntemine geçirilir. Örneğin, aşağıdaki JSON ifadesini alın.

```javascript
var exp = ['get','title'];
JSON.stringify(exp); // = "['get','title']"
```

Yukarıdaki ifadenin strıngısınbulunan sürümü şöyle olabilir `"['get','title']"` ve Android SDK şu şekilde okunabilir.

::: zone pivot="programming-language-java-android"

```java
Expression exp = Expression.raw("['get','title']")
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val exp = Expression.raw("['get','title']")
```

::: zone-end

Bu yaklaşımın kullanılması, Azure haritalar kullanan mobil uygulamalar ve Web uygulamaları arasında stil ifadelerin yeniden kullanılmasını kolaylaştırır.

Bu videoda, Azure haritalar 'da veri tabanlı stillendirme hakkında genel bakış sunulmaktadır.

</br>

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Data-Driven-Styling-with-Azure-Maps/player?format=ny]

## <a name="data-expressions"></a>Veri ifadeleri

Veri ifadeleri bir özelliğin özellik verilerine erişim sağlar.

| Expression | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `accumulated()` | sayı | Şimdiye kadar biriktirilen bir küme özelliğinin değerini alır. |
| `at(number | Expression, Expression)` | değer | Diziden bir öğe alır. |
| `geometryType()` | string | Özelliğin geometri türünü alır: Point, MultiPoint, LineString, MultiLineString, Çokgen, MultiPolygon. |
| `get(string | Expression)` \| `get(string | Expression, Expression)` | değer | Belirtilen nesnenin özelliklerinden özellik değerini alır. İstenen özellik eksikse null değerini döndürür. |
| `has(string | Expression)` \| `has(string | Expression, Expression)` | boolean | Özelliğin özelliklerinin belirtilen özelliğe sahip olup olmadığını belirler. |
| `id()` | değer | Özelliği varsa özelliğin KIMLIĞINI alır. |
| `in(string | number | Expression, Expression)` | boolean | Dizide bir öğe olup olmadığını belirler |
| `length(string | Expression)` | sayı | Bir dizenin veya dizinin uzunluğunu alır. |
| `properties()`| değer | Özellik özellikleri nesnesini alır. |

Aşağıdaki Web SDK stili ifadeleri Android SDK desteklenmez:

- dizini
- 'ın

**Örnekler**

Bir özelliğin özelliklerine, bir ifade kullanarak doğrudan bir ifadede erişilebilir `get` . Bu örnek, `zoneColor` bir kabarcık katmanının Color özelliğini belirtmek için özelliğinin değerini kullanır.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    //Get the zoneColor value.
    bubbleColor(get("zoneColor"))
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    //Get the zoneColor value.
    bubbleColor(get("zoneColor"))
)
```

::: zone-end

Yukarıdaki örnek, tüm nokta özelliklerinin özelliği varsa düzgün çalışacaktır `zoneColor` . Aksi takdirde renk, büyük olasılıkla "siyah" a geri dönecektir. Geri dönüş rengini değiştirmek için, `switchCase` `has` özelliğinin mevcut olup olmadığını denetlemek için ifadesiyle birlikte bir ifade kullanın. Özelliği yoksa, bir geri dönüş rengi döndürün.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(
        //Use a conditional case expression.
        switchCase(
            //Check to see if feature has a "zoneColor" 
            has("zoneColor"), 

            //If it does, use it.
            get("zoneColor"), 

            //If it doesn't, default to blue.
            literal("blue")
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(
        //Use a conditional case expression.
        switchCase(
            //Check to see if feature has a "zoneColor" 
            has("zoneColor"), 

            //If it does, use it.
            get("zoneColor"), 

            //If it doesn't, default to blue.
            literal("blue")
        )
    )
)
```

::: zone-end

Balon ve sembol katmanları, varsayılan olarak bir veri kaynağındaki tüm şekillerin koordinatlarını işler. Bu davranış, bir çokgenin veya çizginin köşelerini vurgulayabilir. `filter`Katman seçeneği, Boolean ifadesinde bir ifade kullanarak, oluşturduğu özelliklerin geometri türünü sınırlamak için kullanılabilir `geometryType` . Aşağıdaki örnek, yalnızca özelliklerin işlenmesi için bir kabarcık katmanını sınırlandırır `Point` .

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    filter(eq(geometryType(), "Point"))
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    filter(eq(geometryType(), "Point"))
)
```

::: zone-end

Aşağıdaki örnek hem hem de `Point` `MultiPoint` özelliklerinin işlenmesine izin verir.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    filter(any(eq(geometryType(), "Point"), eq(geometryType(), "MultiPoint")))
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    filter(any(eq(geometryType(), "Point"), eq(geometryType(), "MultiPoint")))
)
```

::: zone-end

Benzer şekilde, çokgenler ana hattı çizgi katmanlarında işlenir. Bir çizgi katmanında bu davranışı devre dışı bırakmak için yalnızca ve özelliklerine izin veren bir filtre ekleyin `LineString` `MultiLineString` .  

Veri ifadelerinin nasıl kullanılacağına ilişkin bazı ek örnekler aşağıda verilmiştir:

```java
//Get item [2] from an array "properties.abcArray[1]" = "c"
at(2, get("abcArray"))

//Get item [0][1] from a 2D array "properties.array2d[0][1]" = "b"
at(1, at(0, get("array2d")))

//Check to see if a value is in an array "properties.abcArray.indexOf('a') !== -1" = true
in("a", get("abcArray"))

//Get the length of an array "properties.abcArray.length" = 3
length(get("abcArray"))

//Get the value of a subproperty "properties._style.fillColor" = "red"
get("fillColor", get("_style"))

//Check that "fillColor" exists as a subproperty of "_style".
has("fillColor", get("_style"))
```

## <a name="math-expressions"></a>Matematik ifadeleri

Matematik ifadeleri, ifade çerçevesi içinde veri odaklı hesaplamalar gerçekleştirmek için matematik işleçleri sağlar.

| Expression | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `abs(number | Expression)` | sayı | Belirtilen sayının mutlak değerini hesaplar. |
| `acos(number | Expression)` | sayı | Belirtilen sayının arkkosinüsünü hesaplar. |
| `asin(number | Expression)` | sayı | Belirtilen sayının arksinüsünü hesaplar. |
| `atan(number | Expression)` | sayı | Belirtilen sayının ark tanjantını hesaplar. |
| `ceil(number | Expression)` | sayı | Sayıyı bir sonraki tam tamsayıya yuvarlar. |
| `cos(number | Expression)` | sayı | Belirtilen sayının cos sayısını hesaplar. |
| `division(number, number)` \| `division(Expression, Expression)` | sayı | İlk sayıyı ikinci sayıya böler. Web SDK denk ifadesi: `/` |
| `e()` | sayı | Matematik sabitini döndürür `e` . |
| `floor(number | Expression)` | sayı | Sayıyı, önceki tam tamsayıya yuvarlar. |
| `log10(number | Expression)` | sayı | Belirtilen sayının 10 tabanında logaritmasını hesaplar. |
| `log2(number | Expression)` | sayı | Belirtilen sayının temel iki logaritmasını hesaplar. |
| `ln(number | Expression)` | sayı | Belirtilen sayının doğal logaritmasını hesaplar. |
| `ln2()` | sayı | Matematik sabitini döndürür `ln(2)` . |
| `max(numbers... | expressions...)` | sayı | Belirtilen sayı kümesindeki en büyük sayıyı hesaplar. |
| `min(numbers... | expressions...)` | sayı | Belirtilen sayı kümesindeki minimum sayıyı hesaplar. |
| `mod(number, number)` \| `mod(Expression, Expression)` | sayı | İlk sayıyı ikinci sayıya bölerken kalanı hesaplar. Web SDK denk ifadesi: `%` |
| `pi()` | sayı | Matematik sabitini döndürür `PI` . |
| `pow(number, number)` \| `pow(Expression, Expression)` | sayı | İkinci sayının gücünden çıkarılan ilk değerin değerini hesaplar. |
| `product(numbers... | expressions...)` | sayı | Belirtilen sayıları birlikte çarpar. Web SDK denk ifadesi: `*` |
| `round(number | Expression)` | sayı | Sayıyı en yakın tamsayıya yuvarlar. Yarı-değerler sıfırdan uzağa yuvarlanır. Örneğin, `round(-1.5)` olarak değerlendirilir `-2` . |
| `sin(number | Expression)` | sayı | Belirtilen sayının sinüsünü hesaplar. |
| `sqrt(number | Expression)` | sayı | Belirtilen sayının kare kökünü hesaplar. |
| `subtract(number | Expression` | sayı | Belirtilen sayı ile 0 çıkartır. |
| `subtract(number | Expression, number | Expression)` | sayı | İkinci sayıya göre ilk sayıları çıkartır. |
| `sum(numbers... | expressions...)` | sayı | Belirtilen sayıların toplamını hesaplar. |
| `tan(number | Expression)` | sayı | Belirtilen sayının tanjantını hesaplar. |

## <a name="boolean-expressions"></a>Mantıksal ifadeler

Boolean ifadeleri, Boole karşılaştırmaları değerlendirmek için bir dizi Boole işleci sağlar.

Değerler karşılaştırılırken karşılaştırma kesin olarak yazılır. Farklı türlerin değerleri her zaman eşit kabul edilir. Türlerin ayrıştırma zamanında farklı olduğu bilinen durumlar geçersiz olarak değerlendirilir ve bir ayrıştırma hatası oluşturur.

| Expression | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `all(Expression...)` | boolean | `true`Tüm girdilerin `true` , yoksa, döndürür `false` . |
| `any(Expression...)` | boolean | Girdilerden `true` herhangi biri ise `true` , `false` Aksi takdirde döndürür. |
| `eq(Expression compareOne, Expression | boolean | number | string compareTwo)` \| `eq(Expression compareOne, Expression | string compareTwo, Expression collator)` | boolean | `true`Giriş değerlerinin eşitse, `false` Aksi takdirde döndürür. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `gt(Expression compareOne, Expression | boolean | number | string compareTwo)` \| `gt(Expression compareOne, Expression | string compareTwo, Expression collator)` | boolean | `true`İlk girişin ikinciden kesinlikle büyük olup olmadığını döndürür, `false` Aksi takdirde. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `gte(Expression compareOne, Expression | boolean | number | string compareTwo)` \| `gte(Expression compareOne, Expression | string compareTwo, Expression collator)` | boolean | `true`İlk girişin ikinciden büyük veya ona eşit olup olmadığını döndürür, `false` Aksi takdirde. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `lt(Expression compareOne, Expression | boolean | number | string compareTwo)` \| `lt(Expression compareOne, Expression | string compareTwo, Expression collator)` | boolean | `true`İlk giriş ikinciden kesinlikle küçükse döndürür, `false` Aksi takdirde. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `lte(Expression compareOne, Expression | boolean | number | string compareTwo)` \| `lte(Expression compareOne, Expression | string compareTwo, Expression collator)` | boolean | `true`İlk girişin ikinciden küçük veya ona eşit olup olmadığını döndürür, `false` Aksi takdirde. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `neq(Expression compareOne, Expression | boolean | number | string compareTwo)` \| `neq(Expression compareOne, Expression | string compareTwo, Expression collator)` | boolean | `true`Giriş değerleri eşit değilse döndürür, `false` Aksi takdirde. |
| `not(Expression | boolean)` | boolean | Mantıksal değilleme. `true`Girişin olup olmadığını ve girişin olup olmadığını döndürür `false` `false` `true` . |

## <a name="conditional-expressions"></a>Koşullu ifadeler

Koşullu ifadeler, IF-deyimleri gibi mantıksal işlemler sağlar.

Aşağıdaki ifadeler giriş verilerinde koşullu mantık işlemleri gerçekleştirir. Örneğin, ifade " `switchCase` if/then/else" mantığını, ifade ise " `match` switch-deyimi" gibi sağlar.

### <a name="switch-case-expression"></a>Anahtar durumu ifadesi

`switchCase`İfade, "if/then/else" mantığını sağlayan bir koşullu ifade türüdür. Boolean koşulların bir listesi aracılığıyla bu tür ifade adımları. True olarak değerlendirmek için ilk Boole koşulunun çıkış değerini döndürür.

Aşağıdaki sözde kod, ifadenin yapısını tanımlar `switchCase` .

```java
switchCase(
    condition1: boolean expression, 
    output1: value,
    condition2: boolean expression, 
    output2: value,
    ...,
    fallback: value
)
```

**Örnek**

Aşağıdaki örnek, olarak değerlendirilen bir tane bulana kadar farklı Boolean koşullarında adımlar sağlar `true` ve ardından ilgili değeri döndürür. Hiçbir Boole koşulu değerlendirilirse `true` , bir geri dönüş değeri döndürülür.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(
        switchCase(
            //Check to see if the first boolean expression is true, and if it is, return its assigned result.
            //If it has a zoneColor property, use its value as a color.
            has("zoneColor"), toColor(get("zoneColor")),

            //Check to see if the second boolean expression is true, and if it is, return its assigned result.
            //If it has a temperature property with a value greater than or equal to 100, make it red.
            all(has("temperature"), gte(get("temperature"), 100)), color(Color.RED),
            
            //Specify a default value to return. In this case green.
            color(Color.GREEN)
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(
        switchCase(
            //Check to see if the first boolean expression is true, and if it is, return its assigned result.
            //If it has a zoneColor property, use its value as a color.
            has("zoneColor"), toColor(get("zoneColor")),

            //Check to see if the second boolean expression is true, and if it is, return its assigned result.
            //If it has a temperature property with a value greater than or equal to 100, make it red.
            all(has("temperature"), gte(get("temperature"), 100)), color(Color.RED),
            
            //Specify a default value to return. In this case green.
            color(Color.GREEN)
        )
    )
)
```

::: zone-end

### <a name="match-expression"></a>Match ifadesi

`match`İfade, Logic gibi switch deyimi sağlayan bir koşullu ifade türüdür. Giriş, `get( "entityType")` bir dize veya sayı döndüren gibi herhangi bir ifade olabilir. Her durun tek bir sabit değer değeri ya da değerlerinin tümü dize veya tüm sayılar olması gereken değişmez değerler dizisi olan bir etiketi olmalıdır. Dizideki herhangi bir değer eşleşiyorsa, giriş eşleşir. Her durdurma etiketi benzersiz olmalıdır. Giriş türü etiketlerin türüyle eşleşmiyorsa, sonuç varsayılan geri dönüş değeri olacaktır.

Aşağıdaki sözde kod, ifadenin yapısını tanımlar `match` .

```java
match(Expression input, Expression defaultOutput, Expression.Stop... stops)
```

**Örnekler**

Aşağıdaki örnek, bir `entityType` kabarcık katmanındaki bir nokta özelliğinin özelliğine bir eşleşme arayacağını bakar. Bir eşleşme bulursa, belirtilen değer döndürülür veya geri dönüş değerini döndürür.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(
        match(
            //Get the input value to match.
            get("entityType"),

            //Specify a default value to return if no match is found.
            color(Color.BLACK),

            //List the values to match and the result to return for each match.

            //If value is "restaurant" return "red".
            stop("restaurant", color(Color.RED)),

            //If value is "park" return "green".
            stop("park", color(Color.GREEN))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(
        match(
            //Get the input value to match.
            get("entityType"),

            //Specify a default value to return if no match is found.
            color(Color.BLACK),

            //List the values to match and the result to return for each match.

            //If value is "restaurant" return "red".
            stop("restaurant", color(Color.RED)),

            //If value is "park" return "green".
            stop("park", color(Color.GREEN))
        )
    )
)
```

::: zone-end

Aşağıdaki örnek, hepsi aynı değeri döndürmesi gereken bir etiket kümesini listelemek için bir diziyi kullanır. Bu yaklaşım, her bir etiketin ayrı ayrı listelenenden çok daha etkilidir. Bu durumda, `entityType` özellik "Restoran" veya "grocery_store" ise, "kırmızı" rengi döndürülür.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(
        match(
            //Get the input value to match.
            get("entityType"),

            //Specify a default value to return if no match is found.
            color(Color.BLACK),

            //List the values to match and the result to return for each match.

            //If value is "restaurant" or "grocery_store" return "red".
            stop(Arrays.asList("restaurant", "grocery_store"), color(Color.RED)),

            //If value is "park" return "green".
            stop("park", color(Color.GREEN))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(
        match(
            //Get the input value to match.
            get("entityType"),

            //Specify a default value to return if no match is found.
            color(Color.BLACK),

            //List the values to match and the result to return for each match.

            //If value is "restaurant" or "grocery_store" return "red".
            stop(arrayOf("restaurant", "grocery_store"), color(Color.RED)),

            //If value is "park" return "green".
            stop("park", color(Color.GREEN))
        )
    )
)
```

::: zone-end

### <a name="coalesce-expression"></a>Birleşim ifadesi

`coalesce`İlk null olmayan değer alınana ve bu değeri döndürene kadar bir ifade kümesi aracılığıyla ifade adımları.

Aşağıdaki sözde kod, ifadenin yapısını tanımlar `coalesce` .

```java
coalesce(Expression... input)
```

**Örnek**

Aşağıdaki örnek bir `coalesce` sembol katmanının seçeneğini ayarlamak için bir ifade kullanır `textField` . Özelliği özellikte yoksa `title` veya olarak ayarlandıysa `null` , ifade daha sonra özelliği aramaya çalışır `subTitle` , veya eksikse, `null` daha sonra boş bir dizeye geri dönecektir.

::: zone pivot="programming-language-java-android"

```java
SymbolLayer layer = new SymbolLayer(source,
    textField(
        coalesce(
            //Try getting the title property.
            get("title"),

            //If there is no title, try getting the subTitle. 
            get("subTitle"),

            //Default to an empty string.
            literal("")
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = SymbolLayer(source,
    textField(
        coalesce(
            //Try getting the title property.
            get("title"),

            //If there is no title, try getting the subTitle. 
            get("subTitle"),

            //Default to an empty string.
            literal("")
        )
    )
)
```

::: zone-end

## <a name="type-expressions"></a>Tür ifadeleri

Tür ifadeleri, dizeler, sayılar ve Boole değerleri gibi farklı veri türlerini test etmek ve dönüştürmek için araçlar sağlar.

| Expression | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `array(Expression)` | Object [] | Girişin bir dizi olduğunu onaylar. |
| `bool(Expression)` | boolean | Giriş değerinin bir Boole olduğunu onaylar. |
| `collator(boolean caseSensitive, boolean diacriticSensitive)` \| `collator(boolean caseSensitive, boolean diacriticSensitive, java.util.Locale locale)` \| `collator(Expression caseSensitive, Expression diacriticSensitive)` \| `collator(Expression caseSensitive, Expression diacriticSensitive, Expression locale)` | Collator | Yerel ayara bağlı karşılaştırma işlemlerinde kullanılmak üzere bir Harmanlayıcı döndürür. Büyük/küçük harfe duyarlı ve aksan duyarlı seçenekler varsayılan olarak false şeklindedir. Yerel ayar bağımsız değişkeni, kullanılacak yerel ayarın IETF dili etiketini belirtir. Hiçbiri sağlanmazsa, varsayılan yerel ayar kullanılır. İstenen yerel ayar kullanılamıyorsa, Harmanlayıcı sistem tarafından tanımlanan bir geri dönüş yerel ayarı kullanır. Yerel ayar geri dönüş davranışının sonuçlarını sınamak için çözümlenmiş-locale ' i kullanın.  |
| `literal(boolean \| number \| string \| Object \| Object[])` | Boolean \| sayı \| dize \| nesnesi \| nesnesi [] | Sabit bir dizi veya nesne değeri döndürür. Bir dizi ya da nesnenin bir ifade olarak değerlendirilmesini engellemek için bu ifadeyi kullanın. Bir dizi ya da nesnenin bir ifade tarafından döndürülmesi gerektiğinde bu gereklidir. |
| `number(Expression)` | sayı | Giriş değerinin bir sayı olduğunu onaylar. |
| `object(Expression)` | Nesne | Giriş değerinin bir nesne olduğunu onaylar. |
| `string(Expression)` | string | Giriş değerinin bir dize olduğunu onaylar. |
| `toArray(Expression)` | Object [] | İfadeyi bir JSON nesne dizisine dönüştürür. |
| `toBool(Expression)` | boolean | Giriş değerini bir Boole değerine dönüştürür. |
| `toNumber(Expression)` | sayı | Mümkünse, giriş değerini bir sayıya dönüştürür. |
| `toString(Expression)` | string | Giriş değerini bir dizeye dönüştürür. |
| `typeoOf(Expression)` | string | Verilen değerin türünü tanımlayan bir dize döndürür. |

## <a name="color-expressions"></a>Renk ifadeleri

Renk ifadeleri renk değerleri oluşturmayı ve işlemeyi kolaylaştırır.

| Expression | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `color(int)` | color | Color tamsayı değerini bir Color ifadesine dönüştürür. |
| `rgb(Expression red, Expression green, Expression blue)` \| `rgb(number red, number green, number blue)` | color | Ve arasında aralığa gereken *kırmızı*, *yeşil* ve *mavi* bileşenlerden bir renk değeri oluşturur `0` `255` ve bir alfa bileşeni `1` . Herhangi bir bileşen Aralık dışında olursa ifade bir hatadır. |
| `rgba(Expression red, Expression green, Expression blue, Expression alpha)` \| `rgba(number red, number green, number blue, number alpha)` | color | Ve arasında aralığa gereken *kırmızı*, *yeşil*, *mavi* bileşenlerden `0` `255` ve bir ve aralığı içindeki bir Alfa bileşeninden bir renk değeri oluşturur `0` `1` . Herhangi bir bileşen Aralık dışında olursa ifade bir hatadır. |
| `toColor(Expression)` | color  | Giriş değerini bir renge dönüştürür. |
| `toRgba(Expression)` | color | Giriş renginin kırmızı, yeşil, mavi ve Alfa bileşenlerini içeren dört öğeli bir diziyi bu sırayla döndürür. |

**Örnek**

Aşağıdaki örnek, *kırmızı* değeri olan bir RGB renk değeri `255` ve özelliğinin değeri ile çarpılarak hesaplanan *yeşil* ve *mavi* değerler oluşturur `2.5` `temperature` . Sıcaklık değiştiğinde renk, farklı *kırmızı* gölgelerle değişecektir.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(
        //Create a RGB color value.
        rgb(
            //Set red value to 255. Wrap with literal expression since using expressions for other values.
            literal(255f),    

            //Multiple the temperature by 2.5 and set the green value.
            product(literal(2.5f), get("temperature")), 

            //Multiple the temperature by 2.5 and set the blue value.
            product(literal(2.5f), get("temperature")) 
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(
        //Create a RGB color value.
        rgb(
            //Set red value to 255. Wrap with literal expression since using expressions for other values.
            literal(255f),    

            //Multiple the temperature by 2.5 and set the green value.
            product(literal(2.5f), get("temperature")), 

            //Multiple the temperature by 2.5 and set the blue value.
            product(literal(2.5f), get("temperature")) 
        )
    )
)
```

::: zone-end

Tüm renk parametreleri sayı ise, bunları ifadesiyle sarmasına gerek yoktur `literal` . Örnek:

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(
        //Create a RGB color value.
        rgb(
            255f,  //Set red value to 255.

            150f,  //Set green value to 150.

            0f     //Set blue value to 0.
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(
        //Create a RGB color value.
        rgb(
            255f,  //Set red value to 255.

            150f,  //Set green value to 150.

            0f     //Set blue value to 0.
        )
    )
)
```

::: zone-end

> [!TIP]
> Dize renk değerleri, yöntemi kullanılarak bir renge dönüştürülebilir `android.graphics.Color.parseColor` . Aşağıdaki, onaltılı renk dizesini bir katmanla birlikte kullanılabilecek bir renk ifadesine dönüştürür.
>
> ```java
> color(parseColor("#ff00ff"))
> ```

## <a name="string-operator-expressions"></a>Dize işleci ifadeleri

Dize operatörü ifadeleri, büyük/küçük harfe dönüştürme ve dönüştürme gibi dizeler üzerinde dönüştürme işlemleri gerçekleştirir.

| Expression | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `concat(string...)` \| `concat(Expression...)` | string | Birden çok dizeyi birlikte birleştirir. Her değer bir dize olmalıdır. `toString`Gerekirse, diğer değer türlerini dizeye dönüştürmek için tür ifadesini kullanın. |
| `downcase(string)` \| `downcase(Expression)` | string | Belirtilen dizeyi küçük harfe dönüştürür. |
| `isSupportedScript(string)` \| `isSupportedScript(Expression)`| boolean | Giriş dizesinin geçerli yazı tipi yığını tarafından desteklenen bir karakter kümesi kullanıp kullanmadığını belirler. Örnek: `isSupportedScript("ಗೌರವಾರ್ಥವಾಗಿ")` |
| `resolvedLocale(Expression collator)` | string | Belirtilen Harmanlayıcı tarafından kullanılan yerel ayarın IETF dil etiketini döndürür. Bu, varsayılan sistem yerel ayarını belirlemekte veya istenen bir yerel ayarın başarıyla yüklenip yüklenmediğini belirleyebilmesi için kullanılabilir. |
| `upcase(string)` \| `upcase(Expression)` | string | Belirtilen dizeyi büyük harfe dönüştürür. |

**Örnek**

Aşağıdaki örnek, `temperature` nokta özelliğinin özelliğini bir dizeye dönüştürür ve sonra "°f" öğesini sonuna ekler.

::: zone pivot="programming-language-java-android"

```java
SymbolLayer layer = new SymbolLayer(source,
    textField(
        concat(Expression.toString(get("temperature")), literal("°F"))
    ),

    //Some additional style options.
    textOffset(new Float[] { 0f, -1.5f }),
    textSize(12f),
    textColor("white")
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = SymbolLayer(source,
    textField(
        concat(Expression.toString(get("temperature")), literal("°F"))
    ),

    //Some additional style options.
    textOffset(new Float[] { 0f, -1.5f }),
    textSize(12f),
    textColor("white")
)
```

::: zone-end

Yukarıdaki ifade, "64 °F" metni ile haritada, aşağıdaki görüntüde gösterildiği gibi çakışan bir PIN oluşturur.

![Dize işleci ifade örneği](media/how-to-expressions/string-operator-expression.png)

## <a name="interpolate-and-step-expressions"></a>Ara ve adım ifadelerini enterpolala

Enterpolasyonlu ve step ifadeleri, bir ara değerli eğri veya adım işlevi üzerinde değerleri hesaplamak için kullanılabilir. Bu ifadeler, giriş olarak sayısal bir değer döndüren bir ifadeyi alır (örneğin,) `get("temperature")` . Giriş değeri, enterpolasyona veya Step işlevine en uygun değeri belirleyecek giriş ve çıkış değerlerinin çiftlerine göre değerlendirilir. Çıkış değerleri "Durdur" olarak adlandırılır. Her durun giriş değerleri bir sayı olmalı ve artan düzende olmalıdır. Çıkış değerleri bir sayı, sayı dizisi veya bir renk olmalıdır.

### <a name="interpolate-expression"></a>Enterpolageç ifadesi

Bir `interpolate` ifade, durdurma değerleri arasında ilişkilendirme yaparak sürekli, düzgün bir değer kümesini hesaplamak için kullanılabilir. `interpolate`Renk değerleri döndüren bir ifade, sonuç değerlerinin seçildiği bir renk gradyanı üretir. `interpolate`İfade aşağıdaki biçimlere sahiptir:

```java
//Stops consist of two expressions.
interpolate(Expression.Interpolator interpolation, Expression number, Expression... stops)

//Stop expression wraps two values.
interpolate(Expression.Interpolator interpolation, Expression number, Expression.Stop... stops)
```

Bir ifadede kullanılabilecek üç tür ilişkilendirme yöntemi vardır `interpolate` :

| Ad | Açıklama |
|------|-------------|
| `linear()` | Durak çifti arasında doğrusal bir şekilde enterpolasyonlar.  |
| `exponential(number)` \| `exponential(Expression)` | Duraklar arasında katlanarak enterpolasyonlar. Bir "taban" belirtilir ve çıktının arttığı hızı denetler. Daha yüksek değerler, çıktıyı aralığın üst ucunda daha fazla artar. 1 ' e yakın bir "taban" değeri daha erken artan bir çıktı üretir.|
| `cubicBezier(number x1, number y1, number x2, number y2)` \| `cubicBezier(Expression x1, Expression y1, Expression x2, Expression y2)` | Verilen denetim noktaları tarafından tanımlanan [üçüncü dereceden Bezier eğrisini](https://developer.mozilla.org/docs/Web/CSS/timing-function) kullanarak enterpolasyonlar. |

`stop`İfadenin biçimi vardır `stop(stop, value)` .

İşte bu farklı türlerde ara nesnelerin nasıl görüneceğine ilişkin bir örnek.

| Doğrusal  | Üstel | Üçüncü dereceden Bezier |
|---------|-------------|--------------|
| ![Doğrusal ilişkilendirme grafiği](media/how-to-expressions/linear-interpolation.png) | ![Üstel enterpolasyon grafiği](media/how-to-expressions/exponential-interpolation.png) | ![Üçüncü dereceden Bezier enterpolasyon grafiği](media/how-to-expressions/bezier-curve-interpolation.png) |

**Örnek**

Aşağıdaki örnek, `linear interpolate` `bubbleColor` nokta özelliğinin özelliğine dayalı bir kabarcık katmanının özelliğini ayarlamak için bir ifade kullanır `temperature` . `temperature`Değer 60 ' den küçükse, "mavi" döndürülür. 60 arasındaysa ve 70 ' den küçükse, sarı döndürülür. 70 arasındaysa ve 80 ' den küçükse, "turuncu" ( `#FFA500` ) döndürülür. 80 veya daha büyükse, "Red" döndürülür.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(
        interpolate(
            linear(),
            get("temperature"),
            stop(50, color(Color.BLUE)),
            stop(60, color(Color.YELLOW)),
            stop(70, color(parseColor("#FFA500"))),
            stop(80, color(Color.RED))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(
        interpolate(
            linear(),
            get("temperature"),
            stop(50, color(Color.BLUE)),
            stop(60, color(Color.YELLOW)),
            stop(70, color(parseColor("#FFA500"))),
            stop(80, color(Color.RED))
        )
    )
)
```

::: zone-end

Aşağıdaki görüntüde, yukarıdaki ifade için renklerin nasıl seçildiği gösterilmektedir.

![Enterpolageç ifade örneği](media/how-to-expressions/interpolate-expression-example.png)

### <a name="step-expression"></a>Adım ifadesi

Bir `step` ifade, duraklar tarafından tanımlanan bir [piecewise-sabit işlevi](http://mathworld.wolfram.com/PiecewiseConstantFunction.html) değerlendirerek ayrık ve basamaklı sonuç değerlerini hesaplamak için kullanılabilir.

`interpolate`İfade aşağıdaki biçimlere sahiptir:

```java
step(Expression input, Expression defaultOutput, Expression... stops)

step(Expression input, Expression defaultOutput, Expression.Stop... stops)

step(Expression input, number defaultOutput, Expression... stops)

step(Expression input, number defaultOutput, Expression.Stop... stops)

step(number input, Expression defaultOutput, Expression... stops)

step(number input, Expression defaultOutput, Expression.Stop... stops)

step(number input, number defaultOutput, Expression... stops)

step(number input, number defaultOutput, Expression.Stop... stops)
```

Adım ifadeleri, giriş değerinden hemen önce durun çıkış değerini ya da giriş ilk durmadan daha küçükse ilk giriş değerini döndürür.

**Örnek**

Aşağıdaki örnek, `step` `bubbleColor` nokta özelliğinin özelliğine dayalı bir kabarcık katmanının özelliğini ayarlamak için bir ifade kullanır `temperature` . `temperature`Değer 60 ' den küçükse, "mavi" döndürülür. 60 arasındaysa ve 70 ' den küçükse, "sarı" döndürülür. 70 arasındaysa ve 80 ' den küçükse, "turuncu" döndürülür. 80 veya daha büyükse, "Red" döndürülür.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(
        step(
            get("temperature"),
            color(Color.BLUE),
            stop(60, color(Color.YELLOW)),
            stop(70, color(parseColor("#FFA500"))),
            stop(80, color(Color.RED))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(
        step(
            get("temperature"),
            color(Color.BLUE),
            stop(60, color(Color.YELLOW)),
            stop(70, color(parseColor("#FFA500"))),
            stop(80, color(Color.RED))
        )
    )
)
```

::: zone-end

Aşağıdaki görüntüde, yukarıdaki ifade için renklerin nasıl seçildiği gösterilmektedir.

![Adım ifadesi örneği](media/how-to-expressions/step-expression-example.png)

## <a name="layer-specific-expressions"></a>Katmana özgü ifadeler

Yalnızca belirli katmanlara uygulanan özel ifadeler.

### <a name="heat-map-density-expression"></a>Isı haritası yoğunluğu ifadesi

Isı haritası yoğunluğu ifadesi, ısı haritası katmanındaki her bir piksel için ısı haritası yoğunluğu değerini alır ve olarak tanımlanır `heatmapDensity` . Bu değer ile arasında bir sayıdır `0` `1` . `interpolation` `step` Isı haritasını renklendirmek için kullanılan renk degradesini tanımlamak için veya ifadesiyle birlikte kullanılır. Bu ifade yalnızca `heatmapColor` ısı haritası katmanının seçeneğinde kullanılabilir.

> [!TIP]
> Dizin 0 ' daki, bir enterpolasyon ifadesinde veya bir adım renginin varsayılan renginden renk, veri bulunmayan alanın rengini tanımlar. 0 dizinindeki renk, bir arka plan rengi tanımlamak için kullanılabilir. Birçok, bu değeri saydam veya yarı saydam bir siyah olarak ayarlamayı tercih eder.

**Örnek**

Bu örnek, ısı haritasını işlemek için yumuşak bir renk gradyanı oluşturmak üzere bir Oluşturucu ilişkilendirme ifadesi kullanır.

::: zone pivot="programming-language-java-android"

```java
HeatMapLayer layer = new HeatMapLayer(source,
    heatmapColor(
        interpolate(
            linear(),
            heatmapDensity(),
            stop(0, color(Color.TRANSPARENT)),
            stop(0.01, color(Color.MAGENTA)),
            stop(0.5, color(parseColor("#fb00fb"))),
            stop(1, color(parseColor("#00c3ff")))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = HeatMapLayer(source,
    heatmapColor(
        interpolate(
            linear(),
            heatmapDensity(),
            stop(0, color(Color.TRANSPARENT)),
            stop(0.01, color(Color.MAGENTA)),
            stop(0.5, color(parseColor("#fb00fb"))),
            stop(1, color(parseColor("#00c3ff")))
        )
    )
)
```

::: zone-end

Bir ısı haritasını renklendirmeye yönelik düz bir gradyan kullanmanın yanı sıra, renkler bir ifade kullanılarak bir aralıklar kümesi içinde belirlenebilir `step` . `step`Isı haritasının renklendirilebilmesini için bir ifade kullanılması, yoğunluğu bir dağılım veya radar stil eşlemesine benzer aralıklar halinde ayırır.

::: zone pivot="programming-language-java-android"

```java
HeatMapLayer layer = new HeatMapLayer(source,
    heatmapColor(
        step(
            heatmapDensity(),
            color(Color.TRANSPARENT),
            stop(0.01, color(parseColor("#000080"))),
            stop(0.25, color(parseColor("#000080"))),
            stop(0.5, color(Color.GREEN)),
            stop(0.5, color(Color.YELLOW)),
            stop(1, color(Color.RED))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = HeatMapLayer(source,
    heatmapColor(
        step(
            heatmapDensity(),
            color(Color.TRANSPARENT),
            stop(0.01, color(parseColor("#000080"))),
            stop(0.25, color(parseColor("#000080"))),
            stop(0.5, color(Color.GREEN)),
            stop(0.5, color(Color.YELLOW)),
            stop(1, color(Color.RED))
        )
    )
)
```

::: zone-end

Daha fazla bilgi için bkz. [ısı haritası katmanı ekleme](map-add-heat-map-layer-android.md) belgeleri.

### <a name="line-progress-expression"></a>Satır ilerleme ifadesi

Bir satır ilerleme ifadesi, ilerleme durumunu çizgi katmanında bir gradyan çizgisi üzerinde alır ve olarak tanımlanır `lineProgress()` . Bu değer 0 ile 1 arasında bir sayıdır. Or ifadesi ile birlikte kullanılır `interpolation` `step` . Bu ifade yalnızca `strokeGradient` çizgi katmanının seçeneğiyle birlikte kullanılabilir.

> [!NOTE]
> `strokeGradient`Çizgi katmanının seçeneği, `lineMetrics` veri kaynağı seçeneğinin olarak ayarlanmasını gerektirir `true` .

**Örnek**

Bu örnek, `lineProgress()` bir satırın konturuna bir renk gradyanı uygulamak için ifadesini kullanır.

::: zone pivot="programming-language-java-android"

```java
LineLayer layer = new LineLayer(source,
    strokeGradient(
        interpolate(
            linear(),
            lineProgress(),
            stop(0, color(Color.BLUE)),
            stop(0.1, color(Color.argb(255, 65, 105, 225))), //Royal Blue
            stop(0.3, color(Color.CYAN)),
            stop(0.5, color(Color.argb(255,0, 255, 0))), //Lime
            stop(0.7, color(Color.YELLOW)),
            stop(1, color(Color.RED))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = LineLayer(source,
    strokeGradient(
        interpolate(
            linear(),
            lineProgress(),
            stop(0, color(Color.BLUE)),
            stop(0.1, color(Color.argb(255, 65, 105, 225))), //Royal Blue
            stop(0.3, color(Color.CYAN)),
            stop(0.5, color(Color.argb(255,0, 255, 0))), //Lime
            stop(0.7, color(Color.YELLOW)),
            stop(1, color(Color.RED))
        )
    )
)
```

::: zone-end

[Bkz. canlı örnek](map-add-line-layer.md#line-stroke-gradient)

### <a name="text-field-format-expression"></a>Metin alanı biçim ifadesi

`format`İfade, `textField` karışık metin biçimlendirmesi sağlamak için sembol katmanının seçeneğiyle birlikte kullanılabilir. Bu ifade bir veya daha fazla ifadeyi `formatEntry` , metin alanına eklenecek bir dize ve kümesi belirten bir ifade alır `formatOptions` .

| İfade | Açıklama |
|------------|-------------|
| `format(Expression...)` | Karışık biçimli metin alanı girişlerinde kullanılacak ek açıklamaları içeren biçimli metni döndürür. |
| `formatEntry(Expression text)` \| `formatEntry(Expression text, Expression.FormatOption... formatOptions)` \| `formatEntry(String text)` \| `formatEntry(String text, Expression.FormatOption... formatOptions)` | İfadede kullanılmak üzere biçimli bir dize girişi döndürür `format` . |

Aşağıdaki biçim seçenekleri mevcuttur:

| İfade | Açıklama |
|------------|-------------|
| `formatFontScale(number)` \| `formatFontScale(Expression)` | Yazı tipi boyutu için ölçekleme faktörünü belirtir. Belirtilmişse, bu değer `textSize` tek dize için özelliği geçersiz kılar. |
| `formatTextFont(string[])` \| `formatTextFont(Expression)` | İşleme sırasında metne uygulanacak rengi belirtir. |

**Örnek**

Aşağıdaki örnek, bir kalın yazı tipi ekleyerek ve özelliğin özelliğinin yazı tipi boyutunu ölçeklendirerek metin alanını biçimlendirir `title` . Bu örnek ayrıca `subTitle` , ölçeklenmiş bir yazı tipi boyutuyla bir yeni satır üzerinde özelliğin özelliğini de ekler.

::: zone pivot="programming-language-java-android"

```java
SymbolLayer layer = new SymbolLayer(source,
    textField(
        format(
            //Bold the title property and scale its font size up.
            formatEntry(
                get("title"),
                formatTextFont(new String[] { "StandardFont-Bold" }),
                formatFontScale(1.25)),

            //Add a new line without any formatting.
            formatEntry("\n"),

            //Scale the font size down of the subTitle property.
            formatEntry(
                get("subTitle"),
                formatFontScale(0.75))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = SymbolLayer(source,
    textField(
        format(
            //Bold the title property and scale its font size up.
            formatEntry(
                get("title"),
                formatTextFont(arrayOf("StandardFont-Bold")),
                formatFontScale(1.25)),

            //Add a new line without any formatting.
            formatEntry("\n"),

            //Scale the font size down of the subTitle property.
            formatEntry(
                get("subTitle"),
                formatFontScale(0.75))
        )
    )
)
```

::: zone-end

Bu katman, aşağıdaki görüntüde gösterildiği gibi nokta özelliğini oluşturacak:

![Biçimli metin alanı olan nokta özelliğinin görüntüsü](media/how-to-expressions/text-field-format-expression.png)

## <a name="zoom-expression"></a>Yakınlaştırma ifadesi

Bir `zoom` ifade, işleme zamanında haritanın geçerli yakınlaştırma düzeyini almak için kullanılır ve olarak tanımlanır `zoom()` . Bu ifade haritanın en düşük ve en yüksek yakınlaştırma düzeyi aralığı arasında bir sayı döndürür. Azure, Web ve Android için etkileşimli harita denetimlerini, 0 ile 24 arasında numaralandırılmış 25 yakınlaştırma düzeyi destekler. İfadenin kullanılması, `zoom` haritanın yakınlaştırma düzeyi değiştiği için stillerin dinamik olarak değiştirilmesini sağlar. `zoom`İfade yalnızca `interpolate` ve `step` ifadeleriyle birlikte kullanılabilir.

**Örnek**

Varsayılan olarak, ısı haritası katmanında işlenen veri noktalarının yarıçapının tüm yakınlaştırma düzeyleri için sabit bir piksel yarıçapı vardır. Eşleme yakınlaştırıldığında, veri toplar ve ısı haritası katmanı farklı görünüyor. Her bir `zoom` yakınlaştırma düzeyi için yarıçapı ölçeklendirmek için, her bir veri noktasının haritanın aynı fiziksel alanını kapsaması gibi bir ifade kullanılabilir. Bu, ısı haritası katmanının daha statik ve tutarlı görünmesini sağlayacak. Haritanın her yakınlaştırma düzeyinin, önceki yakınlaştırma düzeyiyle dikey ve yatay olarak iki piksel daha vardır. Her yakınlaştırma düzeyiyle iki katına çıkar, yarıçapı ölçeklendirin, tüm yakınlaştırma düzeylerinde tutarlı görünen bir ısı haritası oluşturur. Bu, `zoom` `base 2 exponential interpolation` En düşük yakınlaştırma düzeyi için piksel yarıçapı ve aşağıda gösterildiği gibi hesaplanan en yüksek yakınlaştırma düzeyi için ölçeklendirilmiş bir yarıçap ile ifade kullanılarak gerçekleştirilebilir `2 * Math.pow(2, minZoom - maxZoom)` .

::: zone pivot="programming-language-java-android"

```java
HeatMapLayer layer = new HeatMapLayer(source,
    heatmapRadius(
        interpolate(
            exponential(2),
            zoom(),

            //For zoom level 1 set the radius to 2 pixels.
            stop(1, 2),

            //Between zoom level 1 and 19, exponentially scale the radius from 2 pixels to 2 * (maxZoom - minZoom)^2 pixels.
            stop(19, 2 * Math.pow(2, 19 - 1))
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = HeatMapLayer(source,
    heatmapRadius(
        interpolate(
            exponential(2),
            zoom(),

            //For zoom level 1 set the radius to 2 pixels.
            stop(1, 2),

            //Between zoom level 1 and 19, exponentially scale the radius from 2 pixels to 2 * (maxZoom - minZoom)^2 pixels.
            stop(19, 2 * Math.pow(2, 19 - 1))
        )
    )
)
```

::: zone-end

## <a name="variable-binding-expressions"></a>Değişken bağlama ifadeleri

Değişken bağlama ifadeleri, bir hesaplamanın sonuçlarını bir değişkende depolar. Bu nedenle, hesaplama sonuçlarının bir ifadenin başka bir yerinde birden çok kez başvurulabilmesini sağlayabilirsiniz. Birçok hesaplamayı içeren ifadeler için yararlı bir iyileştirmedir.

| Expression | Dönüş türü | Açıklama |
|--------------|---------------|--------------|
| `let(Expression... input)` | | Bir veya daha fazla değeri `var` , sonucu döndüren alt ifadede ifadesi tarafından kullanılmak üzere değişkenler olarak depolar. |
| `var(Expression expression)` \| `var(string variableName)` | Nesne | İfadesi kullanılarak oluşturulan bir değişkene başvurur `let` . |

**Örnek**

Bu örnek, sıcaklığa göre geliri hesaplayan bir ifade kullanır ve `case` Bu değer üzerinde farklı Boole işlemlerini değerlendirmek için bir ifade kullanır. Bu `let` ifade, geliri sıcaklık oranına göre depolamak için kullanılır, böylece yalnızca bir kez hesaplanmaları gerekir. `var`İfade, bu değişkene, yeniden hesaplamayı gerektirmeden gereken sıklıkta başvurur.

::: zone pivot="programming-language-java-android"

```java
BubbleLayer layer = new BubbleLayer(source,
    bubbleColor(           
        let(
            //Divide the point features `revenue` property by the `temperature` property and store it in a variable called `ratio`.
            literal("ratio"), division(get("revenue"), get("temperature")),

            //Evaluate the child expression in which the stored variable will be used.
            switchCase(
                //Check to see if the ratio is less than 100, return 'red'.
                lt(var("ratio"), 100), color(Color.RED),

                //Check to see if the ratio is less than 200, return 'green'.
                lt(var("ratio"), 200), color(Color.GREEN),

                //Return `blue` for values greater or equal to 200.
                color(Color.BLUE)
            )
        )
    )
);
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
val layer = BubbleLayer(source,
    bubbleColor(           
        let(
            //Divide the point features `revenue` property by the `temperature` property and store it in a variable called `ratio`.
            literal("ratio"), division(get("revenue"), get("temperature")),

            //Evaluate the child expression in which the stored variable will be used.
            switchCase(
                //Check to see if the ratio is less than 100, return 'red'.
                lt(var("ratio"), 100), color(Color.RED),

                //Check to see if the ratio is less than 200, return 'green'.
                lt(var("ratio"), 200), color(Color.GREEN),

                //Return `blue` for values greater or equal to 200.
                color(Color.BLUE)
            )
        )
    )
)
```

::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

İfadeleri destekleyen katmanlar hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](how-to-add-symbol-to-android-map.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer-android.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](android-map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [Isı haritası ekleme](map-add-heat-map-layer-android.md)
