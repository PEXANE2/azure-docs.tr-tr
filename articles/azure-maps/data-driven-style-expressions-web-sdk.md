---
title: Azure Haritalar Web SDK 'sında veri odaklı stil Ifadeleri | Microsoft Azure haritaları
description: Bu makalede, veri tabanlı stil ifadelerinin Microsoft Azure haritaları Web SDK 'sında nasıl kullanılacağı hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 4/4/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: c3f5fb2a387db6e672290fcf03d46c476b6211b6
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79276419"
---
# <a name="data-driven-style-expressions-web-sdk"></a>Veri tabanlı stil Ifadeleri (Web SDK)

İfadeler, bir veri kaynağındaki her bir şekil için tanımlanan özellikleri gözlemleyecek stil seçeneklerine iş mantığı uygulamanızı sağlar. İfadeler, bir veri kaynağındaki veya katmandaki verileri filtreleyebilir. İfadeler, if-deyimleri gibi koşullu mantığdan oluşabilir. Ayrıca, bunları kullanarak verileri işlemek için kullanılabilir: dize işleçleri, mantıksal işleçler ve matematik işleçleri.

Veri odaklı stiller, Stillendirme etrafında iş mantığını uygulamak için gereken kod miktarını azaltır. Katmanlarla kullanıldığında, ifadeler ayrı bir iş parçacığında işleme zamanında değerlendirilir. Bu işlevsellik, UI iş parçacığında iş mantığını değerlendirmeye kıyasla daha yüksek performans sağlar.

Bu videoda, Azure Maps web SDK 'sında veri odaklı stillendirme hakkında genel bakış sunulmaktadır.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Data-Driven-Styling-with-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

İfadeler JSON dizileri olarak temsil edilir. Dizideki bir ifadenin ilk öğesi, ifade işlecinin adını belirten bir dizedir. Örneğin, "+" veya "Case". Sonraki öğeler (varsa), ifadenin bağımsız değişkenlerdir. Her bağımsız değişken bir değişmez değer (dize, sayı, Boolean veya `null`) ya da başka bir ifade dizisi. Aşağıdaki sözde kod, bir ifadenin temel yapısını tanımlar. 

```javascript
[ 
    expression_operator, 
    argument0, 
    argument1, 
    …
] 
```

Azure Haritalar Web SDK 'Sı birçok tür ifadeyi destekler. İfadeler, kendi içinde veya diğer ifadelerle birlikte kullanılabilir.

| İfade türü | Açıklama |
|---------------------|-------------|
| [Toplama ifadesi](#aggregate-expression) | Bir veri kümesi üzerinde işlenen ve bir `DataSource``clusterProperties` seçeneği ile kullanılabilen bir hesaplama tanımlayan bir ifade. |
| [Boole ifadeleri](#boolean-expressions) | Boolean ifadeleri, Boole karşılaştırmaları değerlendirmek için bir dizi Boole işleci sağlar. |
| [Renk ifadeleri](#color-expressions) | Renk ifadeleri renk değerleri oluşturmayı ve işlemeyi kolaylaştırır. |
| [Koşullu ifadeler](#conditional-expressions) | Koşullu ifadeler, IF-deyimleri gibi mantıksal işlemler sağlar. |
| [Veri ifadeleri](#data-expressions) | Bir özelliğindeki Özellik verilerine erişim sağlar. |
| [Ara ve adım ifadelerini enterpolala](#interpolate-and-step-expressions) | Enterpolasyonlu ve step ifadeleri, bir ara değerli eğri veya adım işlevi üzerinde değerleri hesaplamak için kullanılabilir. |
| [Katmana özgü ifadeler](#layer-specific-expressions) | Yalnızca tek bir katman için geçerli olan özel ifadeler. |
| [Matematik ifadeleri](#math-expressions) | Expression Framework içinde veri odaklı hesaplamalar gerçekleştirmek için matematik işleçleri sağlar. |
| [Dize işleci ifadeleri](#string-operator-expressions) | Dize operatörü ifadeleri, büyük/küçük harfe dönüştürme ve dönüştürme gibi dizeler üzerinde dönüştürme işlemleri gerçekleştirir. |
| [Tür ifadeleri](#type-expressions) | Tür ifadeleri, dizeler, sayılar ve Boole değerleri gibi farklı veri türlerini test etmek ve dönüştürmek için araçlar sağlar. |
| [Değişken bağlama ifadeleri](#variable-binding-expressions) | Değişken bağlama ifadeleri, bir hesaplamanın sonuçlarını bir değişkende depolar ve depolanan değeri yeniden hesaplamaya gerek kalmadan bir ifadenin başka bir yerinde birden çok kez başvurulur. |
| [Yakınlaştırma ifadesi](#zoom-expression) | İşleme zamanında haritanın geçerli yakınlaştırma düzeyini alır. |

Bu belgedeki tüm örnekler, farklı ifade türlerinin kullanılabileceği farklı yolları göstermek için aşağıdaki özelliği kullanır. 

```javascript
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
        "temperature": 72,
        "title": "Cafeteria", 
        "zoneColor": "red"
    }
}
```

## <a name="data-expressions"></a>Veri ifadeleri

Veri ifadeleri bir özelliğin özellik verilerine erişim sağlar. 

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['at', number, array]` | object | Diziden bir öğe alır. |
| `['geometry-type']` | string | Özelliğin geometri türünü alır: Point, MultiPoint, LineString, MultiLineString, Çokgen, MultiPolygon. |
| `['get', string]` | değer | Geçerli özelliğin özelliklerinden özellik değerini alır. İstenen özellik eksikse null değerini döndürür. |
| `['get', string, object]` | değer | Belirtilen nesnenin özelliklerinden özellik değerini alır. İstenen özellik eksikse null değerini döndürür. |
| `['has', string]` | boole | Özelliğin özelliklerinin belirtilen özelliğe sahip olup olmadığını belirler. |
| `['has', string, object]` | boole | Nesnenin özelliklerinin belirtilen özelliğe sahip olup olmadığını belirler. |
| `['id']` | değer | Özelliği varsa özelliğin KIMLIĞINI alır. |
| `['length', string | array]` | number | Bir dizenin veya dizinin uzunluğunu alır. |
| `['in', boolean | string | number, array]` | boole | Dizide bir öğe olup olmadığını belirler |
| `['in', substring, string]` | boole | Bir dizedeki alt dizenin mevcut olup olmadığını belirler |

**Örnekler**

Bir özelliğin özelliklerine, bir `get` ifadesi kullanılarak doğrudan bir ifadede erişilebilir. Bu örnek, bir kabarcık katmanının Color özelliğini belirtmek için özelliğinin "zoneColor" değerini kullanır. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: ['get', 'zoneColor'] //Get the zoneColor value.
});
```

Yukarıdaki örnek, tüm nokta özelliklerinin `zoneColor` özelliği varsa düzgün çalışacaktır. Aksi takdirde, renk büyük olasılıkla "siyah" a geri dönüş olur. Geri dönüş rengini değiştirmek için, özelliğin mevcut olup olmadığını denetlemek için `has` ifadesiyle birlikte `case` bir ifade kullanın. Özelliği yoksa, bir geri dönüş rengi döndürün.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'case', //Use a conditional case expression.

        ['has', 'zoneColor'],   //Check to see if feature has a "zoneColor" property
        ['get', 'zoneColor'],   //If it does, use it.

        'blue'  //If it doesn't, default to blue.
    ]
});
```

Balon ve sembol katmanları, varsayılan olarak bir veri kaynağındaki tüm şekillerin koordinatlarını işler. Bu davranış, bir çokgenin veya çizginin köşelerini vurgulayabilir. Katmanın `filter` seçeneği, bir Boolean ifadesinde `['geometry-type']` ifadesi kullanarak, oluşturduğu özelliklerin geometri türünü sınırlamak için kullanılabilir. Aşağıdaki örnek bir kabarcık katmanını yalnızca `Point` özelliklerinin işlenmesini sağlayacak şekilde sınırlandırır.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['==', ['geometry-type'], 'Point']
});
```

Aşağıdaki örnek hem `Point` hem de `MultiPoint` özelliklerinin işlenmesine izin verir. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]
});
```

Benzer şekilde, çokgenler ana hattı çizgi katmanlarında işlenir. Bir çizgi katmanında bu davranışı devre dışı bırakmak için, yalnızca `LineString` ve `MultiLineString` özelliklerine izin veren bir filtre ekleyin.  

## <a name="math-expressions"></a>Matematik ifadeleri

Matematik ifadeleri, ifade çerçevesi içinde veri odaklı hesaplamalar gerçekleştirmek için matematik işleçleri sağlar.

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['+', number, number, …]` | number | Belirtilen sayıların toplamını hesaplar. |
| `['-', number]` | number | Belirtilen sayı ile 0 çıkartır. |
| `['-', number, number]` | number | İkinci sayıya göre ilk sayıları çıkartır. |
| `['*', number, number, …]` | number | Belirtilen sayıları birlikte çarpar. |
| `['/', number, number]` | number | İlk sayıyı ikinci sayıya böler. |
| `['%', number, number]` | number | İlk sayıyı ikinci sayıya bölerken kalanı hesaplar. |
| `['^', number, number]` | number | İkinci sayının gücünden çıkarılan ilk değerin değerini hesaplar. |
| `['abs', number]` | number | Belirtilen sayının mutlak değerini hesaplar. |
| `['acos', number]` | number | Belirtilen sayının arkkosinüsünü hesaplar. |
| `['asin', number]` | number | Belirtilen sayının arksinüsünü hesaplar. |
| `['atan', number]` | number | Belirtilen sayının ark tanjantını hesaplar. |
| `['ceil', number]` | number | Sayıyı bir sonraki tam tamsayıya yuvarlar. |
| `['cos', number]` | number | Belirtilen sayının cos sayısını hesaplar. |
| `['e']` | number | Matematik sabiti `e`döndürür. |
| `['floor', number]` | number | Sayıyı, önceki tam tamsayıya yuvarlar. |
| `['ln', number]` | number | Belirtilen sayının doğal logaritmasını hesaplar. |
| `['ln2']` | number | Matematik sabiti `ln(2)`döndürür. |
| `['log10', number]` | number | Belirtilen sayının 10 tabanında logaritmasını hesaplar. |
| `['log2', number]` | number | Belirtilen sayının temel iki logaritmasını hesaplar. |
| `['max', number, number, …]` | number | Belirtilen sayı kümesindeki en büyük sayıyı hesaplar. |
| `['min', number, number, …]` | number | Belirtilen sayı kümesindeki minimum sayıyı hesaplar. |
| `['pi']` | number | Matematik sabiti `PI`döndürür. |
| `['round', number]` | number | Sayıyı en yakın tamsayıya yuvarlar. Yarı-değerler sıfırdan uzağa yuvarlanır. Örneğin, `['round', -1.5]`-2 olarak değerlendirilir. |
| `['sin', number]` | number | Belirtilen sayının sinüsünü hesaplar. |
| `['sqrt', number]` | number | Belirtilen sayının kare kökünü hesaplar. |
| `['tan', number]` | number | Belirtilen sayının tanjantını hesaplar. |

## <a name="aggregate-expression"></a>Toplama ifadesi

Toplama ifadesi bir veri kümesi üzerinde işlenen ve bir `DataSource``clusterProperties` seçeneği ile kullanılabilen bir hesaplamayı tanımlar. Bu ifadelerin çıktısı bir sayı veya Boole değeri olmalıdır. 

Toplama ifadesi üç değer alır: bir işleç değeri ve başlangıç değeri ve toplama işlemini uygulamak için bir veri içindeki her özellikten bir özelliği almak için bir ifade. Bu ifade aşağıdaki biçimdedir:

```javascript
[operator: string, initialValue: boolean | number, mapExpression: Expression]
```

- işleç: kümedeki her bir nokta için `mapExpression` tarafından hesaplanan tüm değerlere karşı uygulanan bir ifade işlevi. Desteklenen işleçler: 
    - Sayılar için: `+`, `*`, `max`, `min`
    - Boole değerleri için: `all``any`
- InitialValue: ilk hesaplanan değerin oluşturulduğu başlangıç değeri.
- mapExpression: veri kümesindeki her bir noktaya göre uygulanan bir ifade.

**Örnekler**

Bir veri kümesindeki tüm özelliklerin bir `revenue` özelliği varsa, bu sayı bir sayıdır. Daha sonra, bir kümede bulunan ve veri kümesinden oluşturulan tüm noktaların toplam geliri hesaplanabilir. Bu hesaplama aşağıdaki toplama ifadesi kullanılarak yapılır: `['+', 0, ['get', 'revenue']]`

## <a name="boolean-expressions"></a>Mantıksal ifadeler

Boolean ifadeleri, Boole karşılaştırmaları değerlendirmek için bir dizi Boole işleci sağlar.

Değerler karşılaştırılırken karşılaştırma kesin olarak yazılır. Farklı türlerin değerleri her zaman eşit kabul edilir. Türlerin ayrıştırma zamanında farklı olduğu bilinen durumlar geçersiz olarak değerlendirilir ve bir ayrıştırma hatası oluşturur. 

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['! ', boolean]` | boole | Mantıksal değilleme. Giriş `false``true` döndürür ve giriş `true`ise `false`. |
| `['!= ', value, value]` | boole | Giriş değerleri eşitse `true`, aksi takdirde `false` döndürür. |
| `['<', value, value]` | boole | İlk giriş ikinciden kesinlikle küçükse `true` döndürür, aksi takdirde `false`. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `['<=', value, value]` | boole | İlk giriş ikinciden küçükse `true` döndürür, aksi takdirde `false`. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `['==', value, value]` | boole | Giriş değerleri eşitse `true`, aksi takdirde `false` döndürür. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `['>', value, value]` | boole | İlk giriş ikinciden kesinlikle büyükse `true` döndürür, aksi takdirde `false`. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `['>=' value, value]` | boole | İlk giriş ikinciden büyükse veya eşitse `true`, aksi takdirde `false` döndürür. Bağımsız değişkenlerin her iki dize veya iki sayı olması gerekir. |
| `['all', boolean, boolean, …]` | boole | Tüm girişler `true``true` döndürür `false` Aksi takdirde. |
| `['any', boolean, boolean, …]` | boole | Girdilerden herhangi biri `true`, aksi takdirde `false` `true` döndürür. |

## <a name="conditional-expressions"></a>Koşullu ifadeler

Koşullu ifadeler, IF-deyimleri gibi mantıksal işlemler sağlar.

Aşağıdaki ifadeler giriş verilerinde koşullu mantık işlemleri gerçekleştirir. Örneğin, `case` ifadesi "if/then/else" mantığını sağlarken `match` ifadesi bir "switch-deyimi" gibi olur. 

### <a name="case-expression"></a>Case ifadesi

`case` ifadesi, "if/then/else" mantığını sağlayan bir koşullu ifade türüdür. Boolean koşulların bir listesi aracılığıyla bu tür ifade adımları. True olarak değerlendirmek için ilk Boole koşulunun çıkış değerini döndürür.

Aşağıdaki sözde kod `case` ifadesinin yapısını tanımlar. 

```javascript
[
    'case',
    condition1: boolean, 
    output1: value,
    condition2: boolean, 
    output2: value,
    ...,
    fallback: value
]
```

**Örnek**

Aşağıdaki örnek, `true`olarak değerlendirilen bir tane bulana kadar farklı Boolean koşullarında adımlar ve ilgili değeri döndürür. `true`hiçbir Boole koşulu değerlendirilirse, bir geri dönüş değeri döndürülür. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'case',

        //Check to see if the first boolean expression is true, and if it is, return its assigned result.
        ['has', 'zoneColor'],
        ['get', 'zoneColor'],

        //Check to see if the second boolean expression is true, and if it is, return its assigned result.
        ['all', ['has', ' temperature '], ['>', ['get', 'temperature'], 100]],
        'red',

        //Specify a default value to return.
        'green'
    ]
});
```

### <a name="match-expression"></a>Match ifadesi

`match` ifade, Logic gibi switch deyimi sağlayan bir koşullu ifade türüdür. Giriş, bir dize veya sayı döndüren `['get', 'entityType']` gibi herhangi bir ifade olabilir. Her etiket tek bir sabit değer değeri ya da değerlerinin tüm dizeler veya tüm sayılar olması gereken sabit değerler dizisi olmalıdır. Dizideki herhangi bir değer eşleşiyorsa, giriş eşleşir. Her etiket benzersiz olmalıdır. Giriş türü etiketlerin türüyle eşleşmiyorsa, sonuç geri dönüş değeri olacaktır.

Aşağıdaki sözde kod `match` ifadesinin yapısını tanımlar. 

```javascript
[
    'match',
    input: number | string,
    label1: number | string | (number | string)[], 
    output1: value,
    label2: number | string | (number | string)[], 
    output2: value,
    ...,
    fallback: value
]
```

**Örnekler**

Aşağıdaki örnek, bir kabarcık katmanındaki bir nokta özelliğinin `entityType` özelliğine bir eşleşme arar. Bir eşleşme bulursa, belirtilen değer döndürülür veya geri dönüş değerini döndürür.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'match',

        //Get the property to match.
        ['get', 'entityType'],

        //List the values to match and the result to return for each match.
        'restaurant', 'red',
        'park', 'green',

        //Specify a default value to return if no match is found.
        'black'
    ]
});
```

Aşağıdaki örnek, hepsi aynı değeri döndürmesi gereken bir etiket kümesini listelemek için bir diziyi kullanır. Bu yaklaşım, her bir etiketin ayrı ayrı listelenenden çok daha etkilidir. Bu durumda, `entityType` özelliği "Restoran" veya "grocery_store" ise, "kırmızı" rengi döndürülür.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'match',

        //Get the property to match.
        ['get', 'entityType'],

        //List the values to match and the result to return for each match.
        ['restaurant', 'grocery_store'], 'red',

        'park', 'green',

        //Specify a default value to return if no match is found.
        'black'
    ]
});
```

Aşağıdaki örnek bir "Array" veya "Array Contains" tür filtresini gerçekleştirmek için bir Match ifadesi kullanır. Bu durumda ifade, izin verilen kimlikler listesinde bir ID değeri olan verileri filtreler. Filtreler içeren ifadeler kullanılırken, sonucun bir Boole değeri olması gerekir.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: [
        'match',  

        //Get the property to match.
        ['get', 'id'],  

         //List of values to match.
        [24, 53, 98], 

        //If there is a match, return true.
        true,
    
        //Otherwise return false.
        false
    ]
});
```

### <a name="coalesce-expression"></a>Birleşim ifadesi

Bir `coalesce` ifade, ilk null olmayan değer alınana ve bu değeri döndürene kadar bir ifadeler kümesi aracılığıyla adımlar. 

Aşağıdaki sözde kod `coalesce` ifadesinin yapısını tanımlar. 

```javascript
[
    'coalesce', 
    value1, 
    value2, 
    …
]
```

**Örnek**

Aşağıdaki örnek bir sembol katmanının `textField` seçeneğini ayarlamak için bir `coalesce` ifadesi kullanır. `title` özelliği özellikte yoksa veya `null`olarak ayarlanırsa, ifade daha sonra boş bir dizeye geri dönecektir `subtitle` `null`özelliğini aramaya çalışır. 

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'coalesce',

            //Try getting the title property.
            ['get', 'title'],

            //If there is no title, try getting the subtitle. 
            ['get', 'subtitle'],

            //Default to an empty string.
            ''
        ]
    }
});
```

Aşağıdaki örnek, belirtilen görüntü adları listesinden harita hareketli görüntüsündeki kullanılabilir ilk görüntü simgesini almak için bir `coalesce` ifadesi kullanır.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    iconOptions: {
        image: [
            'coalesce',

            //Try getting the image with id 'missing-image'.
            ['image', 'missing-image'],

            //Specify an image id to fallback to. 
            'marker-blue'
        ]
    }
});
``` 

## <a name="type-expressions"></a>Tür ifadeleri

Tür ifadeleri, dizeler, sayılar ve Boole değerleri gibi farklı veri türlerini test etmek ve dönüştürmek için araçlar sağlar.

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['literal', array]`<br/><br/>`['literal', object]` | dizi \| nesnesi | Sabit bir dizi veya nesne değeri döndürür. Bir dizi ya da nesnenin bir ifade olarak değerlendirilmesini engellemek için bu ifadeyi kullanın. Bir dizi ya da nesnenin bir ifade tarafından döndürülmesi gerektiğinde bu gereklidir. |
| `['image', string]` | string | Harita görüntüsü Sprite öğesine belirtilen görüntü KIMLIĞININ yüklenip yüklenmediğini denetler. Eğer ise, KIMLIK döndürülür, aksi takdirde null döndürülür. |
| `['to-boolean', value]` | boole | Giriş değerini bir Boole değerine dönüştürür. Giriş boş bir dize, `0`, `false`, `null`veya `NaN`olduğunda sonuç `false`. Aksi takdirde `true`. |
| `['to-color', value]`<br/><br/>`['to-color', value1, value2…]` | color | Giriş değerini bir renge dönüştürür. Birden çok değer sağlanmışsa, ilk başarılı dönüştürme alınana kadar her biri sırayla değerlendirilir. Girdilerden hiçbiri dönüştürülemiyorsa, ifade bir hatadır. |
| `['to-number', value]`<br/><br/>`['to-number', value1, value2, …]` | number | Mümkünse, giriş değerini bir sayıya dönüştürür. Giriş `null` veya `false`, sonuç 0 ' dır. Giriş `true`, sonuç 1 ' dir. Giriş bir dizeyse, ECMAScript dil belirtiminin [ToNumber](https://tc39.github.io/ecma262/#sec-tonumber-applied-to-the-string-type) dize işlevini kullanarak bir sayıya dönüştürülür. Birden çok değer sağlanmışsa, ilk başarılı dönüştürme alınana kadar her biri sırayla değerlendirilir. Girdilerden hiçbiri dönüştürülemiyorsa, ifade bir hatadır. |
| `['to-string', value]` | string | Giriş değerini bir dizeye dönüştürür. Giriş `null`, sonuç `""`olur. Giriş bir Boole ise, sonuç `"true"` veya `"false"`. Giriş bir sayı ise, ECMAScript dil belirtiminin [ToString](https://tc39.github.io/ecma262/#sec-tostring-applied-to-the-number-type) Number işlevi kullanılarak bir dizeye dönüştürülür. Giriş bir renkeyse, CSS RGBA Color dize `"rgba(r,g,b,a)"`dönüştürülür. Aksi takdirde, giriş, ECMAScript dil belirtiminin [JSON. stringbelirt](https://tc39.github.io/ecma262/#sec-json.stringify) işlevi kullanılarak bir dizeye dönüştürülür. |
| `['typeof', value]` | string | Verilen değerin türünü tanımlayan bir dize döndürür. |

> [!TIP]
> Tarayıcı konsolunda `Expression name must be a string, but found number instead. If you wanted a literal array, use ["literal", [...]].` benzer bir hata iletisi görünürse, kodunuzda ilk değeri için bir dize olmayan bir dizi içeren bir ifade olduğu anlamına gelir. İfadenin bir dizi döndürmesini istiyorsanız, diziyi `literal` ifadesiyle sarın. Aşağıdaki örnek, iki sayı içeren bir dize olması gereken bir sembol katmanının Icon `offset` seçeneğini belirler. Bu, nokta özelliğinin `entityType` özelliğinin değerine göre iki fark değeri arasında seçim yapmak için bir `match` ifadesi kullanarak.
>
> ```javascript
> var layer = new atlas.layer.SymbolLayer(datasource, null, {
>     iconOptions: {
>         offset: [
>             'match',
>
>             //Get the entityType value.
>             ['get', 'entityType'],
>
>             //If the entity type is 'restaurant', return a different pixel offset. 
>             'restaurant', ['literal', [0, -10]],
>
>             //Default to value.
>             ['literal', [0, 0]]
>         ]
>     }
> });
> ```

## <a name="color-expressions"></a>Renk ifadeleri

Renk ifadeleri renk değerleri oluşturmayı ve işlemeyi kolaylaştırır.

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['rgb', number, number, number]` | color | `0` ve `255`ile `1`alfa bileşeni arasında aralığa gereken *kırmızı*, *yeşil*ve *mavi* bileşenlerden bir renk değeri oluşturur. Herhangi bir bileşen Aralık dışında olursa ifade bir hatadır. |
| `['rgba', number, number, number, number]` | color | `0` ve `255`arasında aralığa gereken *kırmızı*, *yeşil*, *mavi* bileşenlerden ve `0` ve `1`bir aralıktaki Alfa bileşeninden bir renk değeri oluşturur. Herhangi bir bileşen Aralık dışında olursa ifade bir hatadır. |
| `['to-rgba']` | \[sayı, sayı, sayı, sayı\] | Giriş renginin *kırmızı*, *yeşil*, *mavi*ve *Alfa* bileşenlerini içeren dört öğeli bir diziyi bu sırayla döndürür. |

**Örnek**

Aşağıdaki örnek, `255`*kırmızı* bir değeri olan bir RGB renk değeri ve `temperature` özelliğinin değeri tarafından `2.5` çarpılarak hesaplanan *yeşil* ve *mavi* değerler oluşturur. Sıcaklık değiştiğinde renk, farklı *kırmızı*gölgelerle değişecektir.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'rgb', //Create a RGB color value.

        255,    //Set red value to 255.

        ['*', 2.5, ['get', 'temperature']], //Multiple the temperature by 2.5 and set the green value.

        ['*', 2.5, ['get', 'temperature']]  //Multiple the temperature by 2.5 and set the blue value.
    ]
});
```

## <a name="string-operator-expressions"></a>Dize işleci ifadeleri

Dize operatörü ifadeleri, büyük/küçük harfe dönüştürme ve dönüştürme gibi dizeler üzerinde dönüştürme işlemleri gerçekleştirir. 

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['concat', string, string, …]` | string | Birden çok dizeyi birlikte birleştirir. Her değer bir dize olmalıdır. Gerekirse diğer değer türlerini dizeye dönüştürmek için `to-string` türü ifadesini kullanın. |
| `['downcase', string]` | string | Belirtilen dizeyi küçük harfe dönüştürür. |
| `['upcase', string]` | string | Belirtilen dizeyi büyük harfe dönüştürür. |

**Örnek**

Aşağıdaki örnek, nokta özelliğinin `temperature` özelliğini bir dizeye dönüştürür ve sonra "°F" öğesini sonuna ekler.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: ['concat', ['to-string', ['get', 'temperature']], '°F'],

        //Some additional style options.
        offset: [0, -1.5],
        size: 12,
        color: 'white'
    }
});
```

Yukarıdaki ifade, "64 °F" metni ile haritada, aşağıdaki görüntüde gösterildiği gibi çakışan bir PIN oluşturur.

<center>

![dize işleci ifadesi örneği](media/how-to-expressions/string-operator-expression.png) </center>

## <a name="interpolate-and-step-expressions"></a>Ara ve adım ifadelerini enterpolala

Enterpolasyonlu ve step ifadeleri, bir ara değerli eğri veya adım işlevi üzerinde değerleri hesaplamak için kullanılabilir. Bu ifadeler giriş olarak sayısal bir değer döndüren bir ifade alır, örneğin `['get',  'temperature']`. Giriş değeri, enterpolasyona veya Step işlevine en uygun değeri belirleyecek giriş ve çıkış değerlerinin çiftlerine göre değerlendirilir. Çıkış değerleri "Durdur" olarak adlandırılır. Her durun giriş değerleri bir sayı olmalı ve artan düzende olmalıdır. Çıkış değerleri bir sayı, sayı dizisi veya bir renk olmalıdır.

### <a name="interpolate-expression"></a>Enterpolageç ifadesi

Bir `interpolate` ifadesi, durdurma değerleri arasında ilişkilendirme yaparak sürekli ve düz bir değer kümesini hesaplamak için kullanılabilir. Renk değerleri döndüren `interpolate` ifadesi, sonuç değerlerinin seçildiği bir renk gradyanı üretir.

`interpolate` ifadesinde kullanılabilecek üç tür ilişkilendirme yöntemi vardır:
 
* `['linear']`-durak çifti arasında doğrusal bir şekilde enterpolasyonlar.
* `['exponential', base]`, duraklar arasında katlanarak üstel olarak enterpolasyonlar. `base` değeri, çıktının arttığı hızı denetler. Daha yüksek değerler, çıktıyı aralığın üst ucunda daha fazla artar. 1 ' e yakın `base` değeri daha erken artan bir çıktı üretir.
* `['cubic-bezier', x1, y1, x2, y2]`-verilen denetim noktaları tarafından tanımlanan [üçüncü dereceden Bezier eğrisini](https://developer.mozilla.org/docs/Web/CSS/timing-function) kullanarak enterpolasyonlar.

İşte bu farklı türlerde ara nesnelerin nasıl görüneceğine ilişkin bir örnek. 

| Doğrusal  | Üstel | Üçüncü dereceden Bezier |
|---------|-------------|--------------|
| ![Doğrusal ilişkilendirme grafiği](media/how-to-expressions/linear-interpolation.png) | ![Üstel enterpolasyon grafiği](media/how-to-expressions/exponential-interpolation.png) | ![Üçüncü dereceden Bezier enterpolasyon grafiği](media/how-to-expressions/bezier-curve-interpolation.png) |

Aşağıdaki sözde kod `interpolate` ifadesinin yapısını tanımlar. 

```javascript
[
    'interpolate',
    interpolation: ['linear'] | ['exponential', base] | ['cubic-bezier', x1, y1, x2, y2],
    input: number,
    stopInput1: number, 
    stopOutput1: value1,
    stopInput2: number, 
    stopOutput2: value2, 
    ...
]
```

**Örnek**

Aşağıdaki örnek, nokta özelliğinin `temperature` özelliğine bağlı olarak bir kabarcık katmanının `color` özelliğini ayarlamak için bir `linear interpolate` ifadesi kullanır. `temperature` değeri 60 ' den küçükse, "mavi" döndürülür. 60 arasındaysa ve 70 ' den küçükse, sarı döndürülür. 70 arasındaysa ve 80 ' den küçükse, "turuncu" döndürülür. 80 veya daha büyükse, "Red" döndürülür.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'interpolate',
        ['linear'],
        ['get', 'temperature'],
        50,        
        'blue',
        60,
        'yellow',
        70,
        'orange',
        80,
        'red'
    ]
});
```

Aşağıdaki görüntüde, yukarıdaki ifade için renklerin nasıl seçildiği gösterilmektedir.
 
<center>

![](media/how-to-expressions/interpolate-expression-example.png) geç ifadesi örneği </center>

### <a name="step-expression"></a>Adım ifadesi

`step` ifadesi, duraklar tarafından tanımlanan bir [piecewise-Constant işlevini](http://mathworld.wolfram.com/PiecewiseConstantFunction.html) değerlendirerek ayrık ve basamaklı sonuç değerlerini hesaplamak için kullanılabilir. 

Aşağıdaki sözde kod `step` ifadesinin yapısını tanımlar. 

```javascript
[
    'step',
    input: number,
    output0: value0,
    stop1: number, 
    output1: value1,
    stop2: number, 
    output2: value2, 
    ...
]
```

Adım ifadeleri, giriş değerinden hemen önce durun çıkış değerini ya da giriş ilk durmadan daha küçükse ilk giriş değerini döndürür. 

**Örnek**

Aşağıdaki örnek, nokta özelliğinin `temperature` özelliğine bağlı olarak bir kabarcık katmanının `color` özelliğini ayarlamak için bir `step` ifadesi kullanır. `temperature` değeri 60 ' den küçükse, "mavi" döndürülür. 60 arasındaysa ve 70 ' den küçükse, "sarı" döndürülür. 70 arasındaysa ve 80 ' den küçükse, "turuncu" döndürülür. 80 veya daha büyükse, "Red" döndürülür.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        'step',
        ['get', 'temperature'],
        'blue',
        60,
        'yellow',
        70,
        'orange',
        80,
        'red'
    ]
});
```

Aşağıdaki görüntüde, yukarıdaki ifade için renklerin nasıl seçildiği gösterilmektedir.
 
<center>

![Step ifadesi örneği](media/how-to-expressions/step-expression-example.png)
</center>

## <a name="layer-specific-expressions"></a>Katmana özgü ifadeler

Yalnızca belirli katmanlara uygulanan özel ifadeler.

### <a name="heat-map-density-expression"></a>Isı haritası yoğunluğu ifadesi

Isı haritası yoğunluğu ifadesi, ısı haritası katmanındaki her bir piksel için ısı haritası yoğunluğu değerini alır ve `['heatmap-density']`olarak tanımlanır. Bu değer, `0` ile `1`arasında bir sayıdır. Isı haritasını renklendirmek için kullanılan renk degradesini tanımlamak için bir `interpolation` veya `step` ifadesiyle birlikte kullanılır. Bu ifade yalnızca ısı haritası katmanının [Color seçeneğinde](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest#color) kullanılabilir.

> [!TIP]
> Dizin 0 ' daki, bir enterpolasyon ifadesinde veya bir adım renginin varsayılan renginden renk, veri bulunmayan alanın rengini tanımlar. 0 dizinindeki renk, bir arka plan rengi tanımlamak için kullanılabilir. Birçok, bu değeri saydam veya yarı saydam bir siyah olarak ayarlamayı tercih eder.

**Örnek**

Bu örnek, ısı haritasını işlemek için yumuşak bir renk gradyanı oluşturmak üzere bir Oluşturucu ilişkilendirme ifadesi kullanır. 

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    color: [
        'interpolate',
        ['linear'],
        ['heatmap-density'],
        0, 'transparent',
        0.01, 'purple',
        0.5, '#fb00fb',
        1, '#00c3ff'
    ]
});
```

Bir ısı haritasını renklendirmeye yönelik düz bir gradyan kullanmanın yanı sıra, renkler bir `step` ifadesi kullanılarak bir aralıklar kümesi içinde belirlenebilir. Isı haritasının renklendirilebilmesini için bir `step` ifadesi kullanılması, yoğunluğu bir dağılım veya radar stil eşlemesine benzer aralıklar halinde ayırır.  

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    color: [
        'step',
        ['heatmap-density'],
        'transparent',
        0.01, 'navy',
        0.25, 'navy',
        0.5, 'green',
        0.75, 'yellow',
        1, 'red'
    ]
});
```

Daha fazla bilgi için bkz. [ısı haritası katmanı ekleme](map-add-heat-map-layer.md) belgeleri.

### <a name="line-progress-expression"></a>Satır ilerleme ifadesi

Bir satır ilerleme ifadesi, ilerleme durumunu çizgi katmanında bir gradyan çizgisi üzerinde alır ve `['line-progress']`olarak tanımlanır. Bu değer 0 ile 1 arasında bir sayıdır. Bir `interpolation` veya `step` ifadesiyle birlikte kullanılır. Bu ifade yalnızca çizgi katmanının [Strokegradient seçeneğiyle]( https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest#strokegradient) birlikte kullanılabilir. 

> [!NOTE]
> Çizgi katmanının `strokeGradient` seçeneği, veri kaynağının `lineMetrics` seçeneğinin `true`olarak ayarlanmasını gerektirir.

**Örnek**

Bu örnek, bir çizginin konturuna bir renk gradyanı uygulamak için `['line-progress']` ifadesini kullanır.

```javascript
var layer = new atlas.layer.LineLayer(datasource, null, {
    strokeGradient: [
        'interpolate',
        ['linear'],
        ['line-progress'],
        0, "blue",
        0.1, "royalblue",
        0.3, "cyan",
        0.5, "lime",
        0.7, "yellow",
        1, "red"
    ]
});
```

[Bkz. canlı örnek](map-add-line-layer.md#line-stroke-gradient)

### <a name="text-field-format-expression"></a>Metin alanı biçim ifadesi

Metin alanı biçimi ifadesi, karışık metin biçimlendirmesi sağlamak üzere sembol katmanları `textOptions` özelliğinin `textField` seçeneği ile kullanılabilir. Bu ifade, bir giriş dizesi ve biçimlendirme seçenekleri kümesinin belirtilmesini sağlar. Bu ifadedeki her giriş dizesi için aşağıdaki seçenekler belirlenebilir.

 * `'font-scale'`-yazı tipi boyutu için ölçekleme faktörünü belirtir. Belirtilmişse, bu değer tek dize için `textOptions` `size` özelliğini geçersiz kılar.
 * `'text-font'`-bu dize için kullanılması gereken bir veya daha fazla yazı tipi ailelerini belirtir. Belirtilmişse, bu değer tek dize için `textOptions` `font` özelliğini geçersiz kılar.
 * `'text-color'`-işleme sırasında metne uygulanacak rengi belirtir. 

Aşağıdaki sözde kod, metin alanı biçim ifadesinin yapısını tanımlar. 

```javascript
[
    'format', 
    input1: string, 
    options1: { 
        'font-scale': number, 
        'text-font': string[],
        'text-color': color
    },
    input2: string, 
    options2: { 
        'font-scale': number, 
        'text-font': string[] ,
        'text-color': color
    },
    …
]
```

**Örnek**

Aşağıdaki örnek, bir kalın yazı tipi ekleyerek ve özelliğin `title` özelliğinin yazı tipi boyutunu ölçeklendirerek metin alanını biçimlendirir. Bu örnek ayrıca, ölçeklenmiş bir yazı tipi boyutu ve kırmızı renkli olan bir yeni satır üzerinde özelliğin `subtitle` özelliğini de ekler.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'format',

            //Bold the title property and scale its font size up.
            ['get', 'title'],
            {
                'text-font': ['literal', ['StandardFont-Bold']],
                'font-scale': 1.25
            },

            '\n', {},   //Add a new line without any formatting.

            //Scale the font size down of the subtitle property. 
            ['get', 'subtitle'],
            { 
                'font-scale': 0.75, 
                'text-color': 'red' 
            }
        ]
    }
});
```

Bu katman, aşağıdaki görüntüde gösterildiği gibi nokta özelliğini oluşturacak:
 
<center>

![biçimlendirilen metin alanı](media/how-to-expressions/text-field-format-expression.png) nokta özelliğinin görüntüsü </center>

### <a name="number-format-expression"></a>Sayı biçimi ifadesi

`number-format` ifadesi yalnızca bir sembol katmanının `textField` seçeneği ile kullanılabilir. Bu ifade, belirtilen sayıyı biçimli bir dizeye dönüştürür. Bu ifade, JavaScript [Number. toLocalString](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) işlevini sarmalayan ve aşağıdaki seçenek kümesini destekler.

 * `locale`-sayıları, belirtilen dille hizalanan bir şekilde dizelere dönüştürmek için bu seçeneği belirtin. Bu seçeneğe bir [BCP 47 Language etiketi](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation) geçirin.
 * `currency`, sayıyı para birimini temsil eden bir dizeye dönüştürmek Için. Olası değerler [ıso 4217 para birimi kodlarıdır](https://en.wikipedia.org/wiki/ISO_4217), örneğin ABD Doları IÇIN "USD", Euro IÇIN "EUR" veya Çince RMB IÇIN "CNY" gibi.
 * `'min-fraction-digits'`-sayının dize sürümüne dahil edilecek en az ondalık basamak sayısını belirtir.
 * `'max-fraction-digits'`-sayının dize sürümüne dahil edilecek en fazla ondalık basamak sayısını belirtir.

Aşağıdaki sözde kod, metin alanı biçim ifadesinin yapısını tanımlar. 

```javascript
[
    'number-format', 
    input: number, 
    options: {
        locale: string, 
        currency: string, 
        'min-fraction-digits': number, 
        'max-fraction-digits': number
    }
]
```

**Örnek**

Aşağıdaki örnek, nokta özelliğinin `revenue` özelliğinin bir sembol katmanının `textField` seçeneğinde, ABD Doları değeri gibi bir değer görünecek şekilde nasıl işleneceğini değiştirmek için bir `number-format` ifadesi kullanır.

```javascript
var layer = new atlas.layer.SymbolLayer(datasource, null, {
    textOptions: {
        textField: [
            'number-format', 
            ['get', 'revenue'], 
            { ‘currency’: 'USD' }
        ],

        offset: [0, 0.75]
    }
});
```

Bu katman, aşağıdaki görüntüde gösterildiği gibi nokta özelliğini oluşturacak:

<center>

![sayı biçimi ifadesi örneği](media/how-to-expressions/number-format-expression.png) </center>

## <a name="zoom-expression"></a>Yakınlaştırma ifadesi

Bir `zoom` ifadesi, işleme zamanında haritanın geçerli yakınlaştırma düzeyini almak için kullanılır ve `['zoom']`olarak tanımlanır. Bu ifade haritanın en düşük ve en yüksek yakınlaştırma düzeyi aralığı arasında bir sayı döndürür. Azure, Web ve Android için etkileşimli harita denetimlerini, 0 ile 24 arasında numaralandırılmış 25 yakınlaştırma düzeyi destekler. `zoom` ifadesinin kullanılması, haritanın yakınlaştırma düzeyi değiştiği için stillerin dinamik olarak değiştirilmesini sağlar. `zoom` ifadesi yalnızca `interpolate` ve `step` ifadelerle kullanılabilir.

**Örnek**

Varsayılan olarak, ısı haritası katmanında işlenen veri noktalarının yarıçapının tüm yakınlaştırma düzeyleri için sabit bir piksel yarıçapı vardır. Eşleme yakınlaştırıldığında, veri toplar ve ısı haritası katmanı farklı görünüyor. Her bir yakınlaştırma düzeyi için yarıçapı ölçeklendirmek için bir `zoom` ifadesi kullanılabilir, her bir veri noktasının de haritanın aynı fiziksel alanını ele alır. Bu, ısı haritası katmanının daha statik ve tutarlı görünmesini sağlayacak. Haritanın her yakınlaştırma düzeyinin, önceki yakınlaştırma düzeyiyle dikey ve yatay olarak iki piksel daha vardır. Her yakınlaştırma düzeyiyle iki katına çıkar, yarıçapı ölçeklendirin, tüm yakınlaştırma düzeylerinde tutarlı görünen bir ısı haritası oluşturur. Aşağıda gösterildiği gibi `base 2 exponential interpolation` ifadesiyle `zoom` ifadesi kullanılarak gerçekleştirilebilir. 

```javascript 
var layer = new atlas.layer.HeatMapLayer(datasource, null, {
    radius: [
        'interpolate',
        ['exponential', 2],
        ['zoom'],
        
        //For zoom level 1 set the radius to 2 pixels.
        10, 2,

        //Between zoom level 1 and 19, exponentially scale the radius from 2 pixels to 10,000 pixels.
        19, 10000
    ]
};
```

[Bkz. canlı örnek](map-add-heat-map-layer.md#consistent-zoomable-heat-map)

## <a name="variable-binding-expressions"></a>Değişken bağlama ifadeleri

Değişken bağlama ifadeleri, bir hesaplamanın sonuçlarını bir değişkende depolar. Bu nedenle, hesaplama sonuçlarının bir ifadenin başka bir yerinde birden çok kez başvurulabilmesini sağlayabilirsiniz. Birçok hesaplamayı içeren ifadeler için yararlı bir iyileştirmedir.

| İfadeler | Dönüş türü | Açıklama |
|--------------|---------------|--------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;' Let ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;name1: String,<br/>&nbsp;&nbsp;&nbsp;&nbsp;değer1: Any,<br/>&nbsp;&nbsp;&nbsp;&nbsp;AD2: dize,<br/>&nbsp;&nbsp;&nbsp;&nbsp;değer2: Any,<br/>&nbsp;&nbsp;&nbsp;&nbsp;...<br/>&nbsp;&nbsp;&nbsp;&nbsp;childExpression<br/>\] | | Bir veya daha fazla değeri, sonucu döndüren alt ifadede `var` ifadesi tarafından kullanılmak üzere değişkenler olarak depolar. |
| `['var', name: string]` | kaydedilmemiş | `let` ifadesi kullanılarak oluşturulan bir değişkene başvurur. |

**Örnek**

Bu örnek, sıcaklığa göre geliri hesaplayan bir ifade kullanır ve ardından bu değerde farklı Boole işlemlerini değerlendirmek için bir `case` ifadesi kullanır. `let` ifadesi, geliri sıcaklık oranına göre depolamak için kullanılır, böylece yalnızca bir kez hesaplanmaları gerekir. `var` ifade, bu değişkene, yeniden hesaplamayı gerektirmeden gereken sıklıkta başvurur.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: [
        //Divide the point features `revenue` property by the `temperature` property and store it in a variable called `ratio`.
        'let', 'ratio', ['/', ['get', 'revenue'], ['get', 'temperature']],
        //Evaluate the child expression in which the stored variable will be used.
        [
            'case',

            //Check to see if the ratio is less than 100, return 'red'.
            ['<', ['var', 'ratio'], 100],
            'red',

            //Check to see if the ratio is less than 200, return 'green'.
            ['<', ['var', 'ratio'], 200],
            'green',

            //Return `blue` for values greater or equal to 200.
            'blue'
        ]
    ]
});
```

## <a name="next-steps"></a>Sonraki adımlar

İfadeler uygulayan daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"] 
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"] 
> [Kabarcık katmanı ekleme](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı Ekle](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı Ekle](map-add-shape.md)

> [!div class="nextstepaction"] 
> [Isı haritası katmanı ekleme](map-add-heat-map-layer.md)

İfadeleri destekleyen katman seçenekleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"] 
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest) 
