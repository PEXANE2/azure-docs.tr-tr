---
title: Azure Haritalar Web SDK'da veri odaklı stil İfadeleri | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sında veri tabanlı stil ifadelerinin nasıl kullanılacağı hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 4/4/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: 3f15033095b02dd35c2d8d7bda60ca184df64c9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475028"
---
# <a name="data-driven-style-expressions-web-sdk"></a>Veri odaklı Stil İfadeleri (Web SDK)

İfadeler, bir veri kaynağında her şekil tanımlanan özellikleri gözlemleyen stil seçeneklerine iş mantığı uygulamanızı sağlar. İfadeler verileri bir veri kaynağında veya katmanda filtreleyebilir. İfadeler koşullu mantıktan oluşabilir, if-ifadeleri gibi. Ve, onlar kullanarak veri işlemek için kullanılabilir: dize işleçleri, mantıksal operatörler, ve matematiksel işleçler.

Veri odaklı stiller, stil oluşturma etrafında iş mantığını uygulamak için gereken kod miktarını azaltır. Katmanlarla kullanıldığında, ifadeler ayrı bir iş parçacığı üzerinde işleme zamanında değerlendirilir. Bu işlevsellik, Kullanıcı Arabirimi iş parçacığındaki iş mantığını değerlendirmeye kıyasla daha yüksek performans sağlar.

Bu video, Azure Haritalar Web SDK'sında veri odaklı stil egenel bir bakış sağlar.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Data-Driven-Styling-with-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

İfadeler JSON dizileri olarak temsil edilir. Dizideki bir ifadenin ilk öğesi, ifade işlecinin adını belirten bir dizedir. Örneğin, "+" veya "büyük/küçük harf". Sonraki öğeler (varsa) ifadebağımsız değişkenleri vardır. Her bağımsız değişken gerçek bir değerdir (dize, `null`sayı, boolean veya), veya başka bir ifade dizisidir. Aşağıdaki pseudocode bir ifadenin temel yapısını tanımlar. 

```javascript
[ 
    expression_operator, 
    argument0, 
    argument1, 
    …
] 
```

Azure Haritalar Web SDK birçok ifade türünü destekler. İfadeler kendi başlarına veya diğer ifadelerle birlikte kullanılabilir.

| İfade türü | Açıklama |
|---------------------|-------------|
| [Toplu ifade](#aggregate-expression) | Bir veri kümesi üzerinde işlenen ve bir `clusterProperties` `DataSource`. seçeneği yle kullanılabilen bir hesaplamayı tanımlayan bir ifade |
| [Mantıksal ifadeler](#boolean-expressions) | Boolean ifadeleri boolean karşılaştırmaları değerlendirmek için boolean işleçleri ifadeler kümesi sağlar. |
| [Renk ifadeleri](#color-expressions) | Renk ifadeleri, renk değerlerinin oluşturulmasını ve işlenmesini kolaylaştırır. |
| [Koşullu ifadeler](#conditional-expressions) | Koşullu ifadeler if-deyimleri gibi mantık işlemleri sağlar. |
| [Veri ifadeleri](#data-expressions) | Özellik verilerine bir özellikte erişim sağlar. |
| [Enterpolasyon ve Adım ifadeleri](#interpolate-and-step-expressions) | Enterpolasyon ve adım ifadeleri, enterpolasyonlu eğri veya adım işlevi boyunca değerleri hesaplamak için kullanılabilir. |
| [Katmana özgü ifadeler](#layer-specific-expressions) | Yalnızca tek bir katman için geçerli olan özel ifadeler. |
| [Matematik ifadeleri](#math-expressions) | Matematiksel işleçlerin ifade çerçevesinde veri odaklı hesaplamalar gerçekleştirmesini sağlar. |
| [String işleç ifadeleri](#string-operator-expressions) | String işleç ifadeleri, büyük/küçük/ciyi bağlama ve dönüştürme gibi dizeleri üzerinde dönüştürme işlemleri gerçekleştirir. |
| [Tür ifadeleri](#type-expressions) | Tür ifadeleri, dizeleri, sayılar ve boolean değerleri gibi farklı veri türlerini sınama ve dönüştürme araçları sağlar. |
| [Değişken bağlama ifadeleri](#variable-binding-expressions) | Değişken bağlama ifadeleri bir hesaplamanın sonuçlarını bir değişkende saklar ve depolanan değeri yeniden hesaplamak zorunda kalmadan bir ifadede birden çok kez başvurulan. |
| [Yakınlaştırma ifadesi](#zoom-expression) | Oluşturma zamanında haritanın geçerli yakınlaştırma düzeyini alır. |

Bu belgedeki tüm örnekler, farklı ifade türlerinin kullanılabileceğini niçin farklı şekillerde göstermek için aşağıdaki özelliği kullanır. 

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

Veri ifadeleri bir özellikteki özellik verilerine erişim sağlar. 

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['at', number, array]` | object | Bir diziden bir öğeyi alır. |
| `['geometry-type']` | string | Özelliğin geometri türünü alır: Point, MultiPoint, LineString, MultiLineString, Polygon, MultiPolygon. |
| `['get', string]` | value | Geçerli özelliğin özelliklerinden özellik değerini alır. İstenen özellik eksikse geçersiz olarak döndürür. |
| `['get', string, object]` | value | Sağlanan nesnenin özelliklerinden özellik değerini alır. İstenen özellik eksikse geçersiz olarak döndürür. |
| `['has', string]` | boole | Bir özelliğin özelliklerinin belirtilen özelliğe sahip olup olmadığını belirler. |
| `['has', string, object]` | boole | Nesnenin özelliklerinin belirtilen özelliğe sahip olup olmadığını belirler. |
| `['id']` | value | Varsa özelliğin kimliğini alır. |
| `['length', string | array]` | number | Bir dizenin veya dizinin uzunluğunu alır. |
| `['in', boolean | string | number, array]` | boole | Bir öğenin bir dizide var olup olmadığını belirler |
| `['in', substring, string]` | boole | Bir alt dize bir dize var olup olmadığını belirler |

**Örnekler**

Bir özelliğin özelliklerine bir ifade kullanılarak doğrudan `get` bir ifadede erişilebilir. Bu örnek, bir kabarcık katmanının renk özelliğini belirtmek için özelliğin "zoneColor" değerini kullanır. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    color: ['get', 'zoneColor'] //Get the zoneColor value.
});
```

Tüm nokta özellikleri `zoneColor` özelliği varsa, yukarıdaki örnek, iyi çalışacaktır. Onlar yoksa, renk büyük olasılıkla "siyah" geri dönecektir. Geri dönüş rengini değiştirmek için, özelliğin `case` `has` var olup olmadığını denetlemek için ifadeyle birlikte bir ifade kullanın. Özellik yoksa, bir geri dönüş rengi döndürün.

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

Kabarcık ve sembol katmanları varsayılan olarak, bir veri kaynağındaki tüm şekillerin koordinatlarını oluşturur. Bu davranış, çokgen veya bir satırın ön noktalarını vurgulayabilir. Katman `filter` seçeneği, boolean ifadesi içinde bir `['geometry-type']` ifade kullanarak, işlettiği özelliklerin geometri türünü sınırlamak için kullanılabilir. Aşağıdaki örnek, yalnızca `Point` özelliklerin işlenmesi için bir kabarcık katmanını sınırlar.

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['==', ['geometry-type'], 'Point']
});
```

Aşağıdaki örnek, `Point` hem `MultiPoint` de özelliklerin işlenmesine izin verir. 

```javascript
var layer = new atlas.layer.BubbleLayer(datasource, null, {
    filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]
});
```

Benzer şekilde, Çokgenlerin anahattı satır katmanlarında işlenir. Bu davranışı bir satır katmanında devre dışı bırakmak `LineString` için `MultiLineString` yalnızca izin veren ve özelliklere sahip bir filtre ekleyin.  

## <a name="math-expressions"></a>Matematik ifadeleri

Matematik ifadeleri, matematiksel işleçlerin ifade çerçevesinde veri odaklı hesaplamalar yapmasını sağlar.

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['+', number, number, …]` | number | Belirtilen sayıların toplamını hesaplar. |
| `['-', number]` | number | Belirtilen sayıya göre 0 çıkarır. |
| `['-', number, number]` | number | İlk sayıları ikinci sayıya göre çıkarır. |
| `['*', number, number, …]` | number | Belirtilen sayıları birlikte çarpar. |
| `['/', number, number]` | number | İlk sayıyı ikinci sayıya böler. |
| `['%', number, number]` | number | İlk sayıyı ikinci sayıya bölerken kalan sayıyı hesaplar. |
| `['^', number, number]` | number | İkinci sayının gücüne yükseltilen ilk değerin değerini hesaplar. |
| `['abs', number]` | number | Belirtilen sayının mutlak değerini hesaplar. |
| `['acos', number]` | number | Belirtilen sayının arkkozini hesaplar. |
| `['asin', number]` | number | Belirtilen sayının arkininin hesaplar. |
| `['atan', number]` | number | Belirtilen sayının arktantını hesaplar. |
| `['ceil', number]` | number | Numarayı bir sonraki tamsayıya yuvarlar. |
| `['cos', number]` | number | Belirtilen sayının kasını hesaplar. |
| `['e']` | number | Matematiksel sabiti `e`verir. |
| `['floor', number]` | number | Numarayı önceki tamsayıya yuvarlar. |
| `['ln', number]` | number | Belirtilen sayının doğal logaritmasını hesaplar. |
| `['ln2']` | number | Matematiksel sabiti `ln(2)`verir. |
| `['log10', number]` | number | Belirtilen sayının taban on logarithm hesaplar. |
| `['log2', number]` | number | Belirtilen sayının temel iki logaritmini hesaplar. |
| `['max', number, number, …]` | number | Belirtilen sayı kümesindeki en büyük sayıyı hesaplar. |
| `['min', number, number, …]` | number | Belirtilen sayı kümesindeki minimum sayıyı hesaplar. |
| `['pi']` | number | Matematiksel sabiti `PI`verir. |
| `['round', number]` | number | Numarayı en yakın sayamacına yuvarlar. Yarı değer sıfırdan yuvarlanır. Örneğin, `['round', -1.5]` -2 olarak değerlendirir. |
| `['sin', number]` | number | Belirtilen sayının sinüsünü hesaplar. |
| `['sqrt', number]` | number | Belirtilen sayının kare kökünü hesaplar. |
| `['tan', number]` | number | Belirtilen sayının teğetini hesaplar. |

## <a name="aggregate-expression"></a>Toplu ifade

Toplu ifade, bir veri kümesi üzerinde işlenen bir hesaplamayı tanımlar ve `clusterProperties` bir `DataSource`. seçeneğiyle kullanılabilir Bu ifadelerin çıktısı bir sayı veya boolean olmalıdır. 

Toplu ifade üç değer alır: bir işleç değeri ve ilk değer ve bir veri her özellikten bir özellik almak için bir ifade toplam işlemi uygulamak için. Bu ifadeaşağıdaki biçimi vardır:

```javascript
[operator: string, initialValue: boolean | number, mapExpression: Expression]
```

- işleç: Kümedeki her nokta `mapExpression` için hesaplanan tüm değerlere karşı uygulanan bir ifade işlevi. Desteklenen operatörler: 
    - Sayılar `+`için: `*` `max`, , ,`min`
    - Booleans için: `all`,`any`
- initialValue: İlk hesaplanan değerin karşı toplandığı başlangıç değeridir.
- mapExpression: Veri kümesindeki her noktaya karşı uygulanan ifade.

**Örnekler**

Veri kümesindeki tüm özelliklerin `revenue` bir özelliği varsa, bu bir sayıdır. Daha sonra, bir kümedeki veri kümesinden oluşturulan tüm noktaların toplam geliri hesaplanabilir. Bu hesaplama aşağıdaki toplu ifade kullanılarak yapılır:`['+', 0, ['get', 'revenue']]`

## <a name="boolean-expressions"></a>Mantıksal ifadeler

Boolean ifadeleri boolean karşılaştırmaları değerlendirmek için boolean işleçleri ifadeler kümesi sağlar.

Değerleri karşılaştırırken, karşılaştırma kesinlikle dizilir. Farklı türdeki değerler her zaman eşit olarak kabul edilir. Ayrıştırık zamanda türlerin farklı olduğu bilinen durumlar geçersiz kabul edilir ve ayrıştırılma hatasına neden olur. 

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['! ', boolean]` | boole | Mantıksal olumsuzluk. `true` Giriş ise `false`ve `false` giriş . `true` |
| `['!= ', value, value]` | boole | Giriş `true` değerleri eşit değilse, `false` aksi takdirde döndürür. |
| `['<', value, value]` | boole | İlk `true` giriş kesinlikle ikincisinden daha azsa, `false` aksi takdirde döndürür. Bağımsız değişkenlerin her iki dize veya her iki sayı olması gerekir. |
| `['<=', value, value]` | boole | İlk `true` giriş ikinciden küçük veya eşitse, `false` aksi takdirde döndürür. Bağımsız değişkenlerin her iki dize veya her iki sayı olması gerekir. |
| `['==', value, value]` | boole | Giriş `true` değerleri eşitse, `false` aksi takdirde döndürür. Bağımsız değişkenlerin her iki dize veya her iki sayı olması gerekir. |
| `['>', value, value]` | boole | İlk `true` giriş kesinlikle ikincisinden büyükse, `false` aksi takdirde döndürür. Bağımsız değişkenlerin her iki dize veya her iki sayı olması gerekir. |
| `['>=' value, value]` | boole | İlk `true` giriş ikinciden büyük veya ikinciye eşitse, `false` aksi takdirde döndürür. Bağımsız değişkenlerin her iki dize veya her iki sayı olması gerekir. |
| `['all', boolean, boolean, …]` | boole | Tüm `true` girişler `true`, `false` aksi takdirde döndürür. |
| `['any', boolean, boolean, …]` | boole | Herhangi `true` bir girdi `true`, `false` aksi takdirde döndürür. |

## <a name="conditional-expressions"></a>Koşullu ifadeler

Koşullu ifadeler if-deyimleri gibi mantık işlemleri sağlar.

Aşağıdaki ifadeler giriş verilerinde koşullu mantık işlemleri gerçekleştirir. Örneğin, `case` `match` ifade "if/then/else" mantığını sağlarken, ifade "anahtar-deyim" gibidir. 

### <a name="case-expression"></a>Büyük/küçük harf ifadesi

İfade, `case` "if/then/else" mantığını sağlayan koşullu ifade türüdür. Bu tür bir ifade boolean koşulları listesinden geçer. Doğru değerlendirmek için ilk boolean koşulunun çıkış değerini döndürür.

Aşağıdaki pseudocode `case` ifadenin yapısını tanımlar. 

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

Aşağıdaki örnek, farklı boolean koşullarını değerlendiren ve ilişkili `true`değeri döndüren bir tane bulana kadar ilerler. Hiçbir boolean koşulu `true`değerlendirirse, bir geri dönüş değeri döndürülür. 

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

### <a name="match-expression"></a>Eşleştirme ifadesi

İfade, `match` mantık gibi anahtar deyimi sağlayan koşullu ifade türüdür. Giriş, bir dize veya `['get', 'entityType']` sayı döndüren gibi herhangi bir ifade olabilir. Her etiket, değerleri tüm dizeleri veya tüm sayılar olmalıdır edebi değerler, ya tek bir gerçek değer veya bir dizi olmalıdır. Dizideki değerlerden herhangi biri eşleşiyorsa, giriş eşleşir. Her etiket benzersiz olmalıdır. Giriş türü etiketlerin türüyle eşleşmiyorsa, sonuç geri dönüş değeri olacaktır.

Aşağıdaki pseudocode `match` ifadenin yapısını tanımlar. 

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

Aşağıdaki örnek, bir `entityType` kabarcık katmanında bir eşleşme arar bir Nokta özelliğine bakar. Bir eşleşme bulursa, belirtilen değer döndürülür veya geri dönüş değerini döndürür.

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

Aşağıdaki örnek, tümü aynı değeri döndürecek bir etiket kümesini listelemek için bir dizi kullanır. Bu yaklaşım, her etiketi ayrı ayrı listelemeden çok daha verimlidir. Bu durumda, `entityType` özellik "restoran" veya "grocery_store" ise, renk "kırmızı" döndürülür.

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

Aşağıdaki örnekte bir "dizi içinde" veya "dizi içerir" türü filtresi gerçekleştirmek için bir eşleme ifadesi kullanır. Bu durumda, ifade, izin verilen kimlikler listesinde bulunan bir kimlik değeri olan verileri filtreler. Filtreleri olan ifadeleri kullanırken, sonucun boolean değeri olması gerekir.

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

### <a name="coalesce-expression"></a>Birleştirme ifadesi

İfade, `coalesce` ilk null olmayan değer elde edilene kadar bir ifade kümesini ilerler ve bu değeri döndürür. 

Aşağıdaki pseudocode `coalesce` ifadenin yapısını tanımlar. 

```javascript
[
    'coalesce', 
    value1, 
    value2, 
    …
]
```

**Örnek**

Aşağıdaki örnekte `coalesce` bir sembol `textField` katmanı seçeneğini ayarlamak için bir ifade kullanır. `title` Özellik özellikten eksikse veya `null`ayarlanmışsa, ifade daha `subtitle` sonra mülkü aramayı `null`dener, eksikse veya boş bir dize geri düşer. 

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

Aşağıdaki örnekte, `coalesce` haritada bulunan ilk kullanılabilir resim simgesini belirtilen resim adları listesinden almak için bir ifade kullanır.

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

Tür ifadeleri, dizeleri, sayılar ve boolean değerleri gibi farklı veri türlerini sınama ve dönüştürme araçları sağlar.

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['literal', array]`<br/><br/>`['literal', object]` | dizi \| nesnesi | Gerçek bir dizi veya nesne değeri döndürür. Bir dizi veya nesnenin ifade olarak değerlendirilmesini önlemek için bu ifadeyi kullanın. Bir dizi veya nesnenin bir ifade tarafından döndürülmesi gerektiğinde bu gereklidir. |
| `['image', string]` | string | Belirli bir görüntü kimliğinin haritalar görüntüsüne yüklenip yüklenmediğini denetler. Bu durumda, kimlik döndürülür, aksi takdirde null döndürülür. |
| `['to-boolean', value]` | boole | Giriş değerini boolean'a dönüştürür. Sonuç, `false` girişboş bir dize `0`olduğunda, `false` `null`, `NaN`, veya ; aksi `true`takdirde onun . |
| `['to-color', value]`<br/><br/>`['to-color', value1, value2…]` | color | Giriş değerini bir renge dönüştürür. Birden çok değer sağlanırsa, ilk başarılı dönüştürme elde edilene kadar her biri sırayla değerlendirilir. Girişlerden hiçbiri dönüştürülemezse, ifade bir hatadır. |
| `['to-number', value]`<br/><br/>`['to-number', value1, value2, …]` | number | Mümkünse giriş değerini bir sayıya dönüştürür. Giriş veya `false`, `null` sonuç 0 ise. Giriş ise, `true`sonuç 1'dir. Giriş bir dize ise, ECMAScript Dil Belirtimi'nin [ToNumber](https://tc39.github.io/ecma262/#sec-tonumber-applied-to-the-string-type) dize işlevini kullanarak bir sayıya dönüştürülür. Birden çok değer sağlanırsa, ilk başarılı dönüştürme elde edilene kadar her biri sırayla değerlendirilir. Girişlerden hiçbiri dönüştürülemezse, ifade bir hatadır. |
| `['to-string', value]` | string | Giriş değerini bir dize dönüştürür. Giriş ise, `null`sonuç `""`. Giriş bir boolean ise, sonuç `"true"` veya `"false"`. Giriş bir sayıise, ECMAScript Language Specification'un [ToString](https://tc39.github.io/ecma262/#sec-tostring-applied-to-the-number-type) numarası işlevini kullanarak bir dize dönüştürülür. Giriş bir renkse, CSS RGBA renk dizesi `"rgba(r,g,b,a)"`dönüştürülür. Aksi takdirde, giriş ECMAScript Dil [Belirtimi'nin JSON.stringify](https://tc39.github.io/ecma262/#sec-json.stringify) işlevini kullanarak bir dize dönüştürülür. |
| `['typeof', value]` | string | Verilen değerin türünü açıklayan bir dize döndürür. |

> [!TIP]
> Tarayıcı konsolunda `Expression name must be a string, but found number instead. If you wanted a literal array, use ["literal", [...]].` benzer bir hata iletisi görünüyorsa, kodunuzda ilk değeri için dize olmayan bir dizi olan bir ifade olduğu anlamına gelir. İfadenin bir dizi döndürmesini istiyorsanız, diziyi `literal` ifadeyle birlikte sarın. Aşağıdaki örnek, nokta `offset` özelliğinin `entityType` özelliğinin değerini temel alan iki ofset değeri `match` arasında seçim yapmak için bir ifade kullanarak, iki sayı içeren bir dizi olması gereken bir sembol katmanı simge seçeneğini ayarlar.
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

Renk ifadeleri, renk değerlerinin oluşturulmasını ve işlenmesini kolaylaştırır.

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['rgb', number, number, number]` | color | *Kırmızı,* *yeşil*ve *mavi* bileşenler arasında ve alfa `0` bileşeni `255`arasında bir renk `1`değeri oluşturur. Herhangi bir bileşen kapsama alanı dışındaysa, ifade bir hatadır. |
| `['rgba', number, number, number, number]` | color | *Kırmızı,* *yeşil,* *mavi* bileşenler `0` arasında ve arasında değişmelidir bir renk değeri oluşturur `0` `1`ve `255`bir alfa bileşeni bir aralık içinde ve . Herhangi bir bileşen kapsama alanı dışındaysa, ifade bir hatadır. |
| `['to-rgba']` | \[sayı, sayı, sayı, sayı\] | Bu sırada giriş renginin *kırmızı,* *yeşil,* *mavi*ve *alfa* bileşenlerini içeren dört öğeli bir dizi verir. |

**Örnek**

Aşağıdaki örnek, *kırmızı* `255`değeri olan bir RGB renk değeri ve `temperature` özelliğin değeriyle `2.5` çarpılarak hesaplanan *yeşil* ve *mavi* değerleri oluşturur. Sıcaklık değiştikçe, renk *kırmızının*farklı tonlarına dönüşür.

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

## <a name="string-operator-expressions"></a>String işleç ifadeleri

String işleç ifadeleri, büyük/küçük/ciyi bağlama ve dönüştürme gibi dizeleri üzerinde dönüştürme işlemleri gerçekleştirir. 

| İfadeler | Dönüş türü | Açıklama |
|------------|-------------|-------------|
| `['concat', string, string, …]` | string | Birden çok dizeleri birleştirir. Her değer bir dize olmalıdır. Gerekirse `to-string` diğer değer türlerini dize dönüştürmek için tür ifadesini kullanın. |
| `['downcase', string]` | string | Belirtilen dizeyi küçük harfe dönüştürür. |
| `['upcase', string]` | string | Belirtilen dizeyi büyük harfe dönüştürür. |

**Örnek**

Aşağıdaki örnek, nokta `temperature` özelliğinin özelliğini bir dize dönüştürür ve sonra sonuna "°F" eklenir.

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

Yukarıdaki ifade, aşağıdaki resimde gösterildiği gibi üzerine "64°F" metni nin kontup olduğu haritaüzerinde bir pin oluşturur.

<center>

![String işleci](media/how-to-expressions/string-operator-expression.png) ifadesi örneği</center>

## <a name="interpolate-and-step-expressions"></a>Enterpolasyon ve Adım ifadeleri

Enterpolasyon ve adım ifadeleri, enterpolasyonlu eğri veya adım işlevi boyunca değerleri hesaplamak için kullanılabilir. Bu ifadeler, örneğin, `['get',  'temperature']`girdiolarak sayısal bir değer döndüren bir ifade alır. Giriş değeri, enterpolasyonlu eğri veya adım işlevine en uygun değeri belirlemek için giriş ve çıkış değerleri çiftleri karşı değerlendirilir. Çıktı değerleri "duraklar" olarak adlandırılır. Her stop için giriş değerleri bir sayı olmalı ve artan sırada olmalıdır. Çıktı değerleri bir sayı ve sayı dizisi veya bir renk olmalıdır.

### <a name="interpolate-expression"></a>Enterpolasyon ifadesini

Bir `interpolate` ifade, dur değerleri arasında enterpolasyon yaparak sürekli, düzgün bir değer kümesini hesaplamak için kullanılabilir. Renk `interpolate` değerlerini döndüren bir ifade, sonuç değerlerinin seçildiği bir renk degradesi üretir.

Bir `interpolate` ifadede kullanılabilecek üç tür enterpolasyon yöntemi vardır:
 
* `['linear']`- Durak çifti arasında doğrusal olarak interpolates.
* `['exponential', base]`- Duraklar arasında katlanarak interpolates. Değer, `base` çıktının artış hızını denetler. Daha yüksek değerler, çıktının aralığın yüksek ucuna doğru daha fazla artmasına neden oltır. 1'e yakın bir `base` değer, daha doğrusal olarak artan bir çıktı üretir.
* `['cubic-bezier', x1, y1, x2, y2]`- Verilen kontrol noktaları tarafından tanımlanan [kübik Bezier eğrisi](https://developer.mozilla.org/docs/Web/CSS/timing-function) kullanılarak interpolates.

Burada interpolasyonlar bu farklı türde neye benzediğini bir örnektir. 

| Doğrusal  | Üstel | Kübik Bezier |
|---------|-------------|--------------|
| ![Doğrusal enterpolasyon grafiği](media/how-to-expressions/linear-interpolation.png) | ![Üstel enterpolasyon grafiği](media/how-to-expressions/exponential-interpolation.png) | ![Kübik Bezier enterpolasyon grafiği](media/how-to-expressions/bezier-curve-interpolation.png) |

Aşağıdaki pseudocode `interpolate` ifadenin yapısını tanımlar. 

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

Aşağıdaki örnek, `linear interpolate` nokta özelliğinin `color` özelliğine `temperature` göre bir kabarcık katmanının özelliğini ayarlamak için bir ifade kullanır. Değer `temperature` 60'tan küçükse, "mavi" döndürülür. 60 ile 70'in üzerindeyse, sarı iade edilir. 70 ile 80'den az ise, "turuncu" iade edilecektir. 80 veya daha büyükse, "kırmızı" iade edilir.

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

Aşağıdaki resim, yukarıdaki ifade için renklerin nasıl seçildiğini gösterir.
 
<center>

![Enterpolitasyon](media/how-to-expressions/interpolate-expression-example.png) ifade örneği</center>

### <a name="step-expression"></a>Adım ifadesi

Bir `step` ifade, duraklar tarafından tanımlanan [parça olarak sabit](http://mathworld.wolfram.com/PiecewiseConstantFunction.html) bir işlevi değerlendirerek ayrık, basamaklı sonuç değerlerini hesaplamak için kullanılabilir. 

Aşağıdaki pseudocode `step` ifadenin yapısını tanımlar. 

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

Adım ifadeleri, giriş değerinden hemen önce durdurmanın çıktı değerini veya giriş ilk duraktan küçükse ilk giriş değerini döndürer. 

**Örnek**

Aşağıdaki örnek, `step` nokta özelliğinin `color` özelliğine `temperature` göre bir kabarcık katmanının özelliğini ayarlamak için bir ifade kullanır. Değer `temperature` 60'tan küçükse, "mavi" döndürülür. 60 ile 70'ten küçükse, "sarı" iade edilir. 70 ile 80'den az ise, "turuncu" iade edilecektir. 80 veya daha büyükse, "kırmızı" iade edilir.

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

Aşağıdaki resim, yukarıdaki ifade için renklerin nasıl seçildiğini gösterir.
 
<center>

![Adım ifadesi örneği](media/how-to-expressions/step-expression-example.png)
</center>

## <a name="layer-specific-expressions"></a>Katmana özgü ifadeler

Yalnızca belirli katmanlar için geçerli özel ifadeler.

### <a name="heat-map-density-expression"></a>Isı haritası yoğunluk ifadesi

Isı haritası yoğunluğu ifadesi, Bir ısı haritası katmanındaki her piksel için ısı `['heatmap-density']`haritası yoğunluğu değerini alır ve . Bu değer ve `0` . `1` Isı eşlemi boyamak için kullanılan renk degradesini tanımlamak için bir `interpolation` veya `step` ifadeyle birlikte kullanılır. Bu ifade yalnızca ısı eşlemi katmanının [renk seçeneğinde](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest#color) kullanılabilir.

> [!TIP]
> İndeks 0'daki renk, bir enterpolasyon ifadesinde veya adım renginin varsayılan renginde, veri olmayan alanın rengini tanımlar. Dizin 0'daki renk, arka plan rengini tanımlamak için kullanılabilir. Çoğu kişi bu değeri saydam veya yarı saydam siyaha ayarlamayı tercih ediyor.

**Örnek**

Bu örnek, ısı eşlemi işlemek için düzgün bir renk degradesi oluşturmak için bir liner enterpolasyon ifadesi kullanır. 

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

Bir ısı eşlemi renklendirmek için düzgün bir degrade kullanmanın yanı sıra, `step` renkler bir ifade kullanılarak aralıkları kümesi içinde belirtilebilir. Isı `step` haritasını renklendirmek için bir ifade kullanmak yoğunluğu kontur veya radar stili haritasına benzeyen aralıklara ayırır.  

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

Daha fazla bilgi için [ısı haritası katmanı](map-add-heat-map-layer.md) ekle belgelerine bakın.

### <a name="line-progress-expression"></a>Satır ilerleme ifadesi

Satır ilerleme ifadesi, bir çizgi katmanındaki bir degrade çizgi `['line-progress']`boyunca ilerlemeyi alır ve . Bu değer 0 ile 1 arasında bir sayıdır. Bir `interpolation` veya `step` ifade ile birlikte kullanılır. Bu ifade yalnızca satır katmanının [strokeGradient seçeneği]( https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest#strokegradient) ile kullanılabilir. 

> [!NOTE]
> Satır `strokeGradient` katmanı seçeneği, veri `lineMetrics` kaynağının `true`'' olarak ayarlanabilmek için seçeneğini gerektirir.

**Örnek**

Bu örnek, `['line-progress']` bir satırın konturuna renk degradesi uygulamak için ifadeyi kullanır.

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

[Canlı örneğe bakın](map-add-line-layer.md#line-stroke-gradient)

### <a name="text-field-format-expression"></a>Metin alanı biçimi ifadesi

Metin alanı biçimi ifadesi, karışık `textField` metin biçimlendirmesi sağlamak için sembol katmanları `textOptions` özelliği seçeneğiyle kullanılabilir. Bu ifade, giriş dizeleri ve biçimlendirme seçenekleri kümesinin belirtilmesine izin verir. Bu ifadedeki her giriş dizesi için aşağıdaki seçenekler belirtilebilir.

 * `'font-scale'`- Yazı tipi boyutu için ölçekleme faktörbelirtir. Belirtilirse, bu değer `size` tek tek `textOptions` dize için özelliği geçersiz kılar.
 * `'text-font'`- Bu dize için kullanılması gereken bir veya daha fazla yazı tipi ailesi belirtir. Belirtilirse, bu değer `font` tek tek `textOptions` dize için özelliği geçersiz kılar.
 * `'text-color'`- İşleme yaparken metne uygulanacak bir renk belirtir. 

Aşağıdaki pseudocode metin alanı biçimi ifadesinin yapısını tanımlar. 

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

Aşağıdaki örnek, kalın bir yazı tipi ekleyerek ve özelliğin `title` özelliğinin yazı tipi boyutunu ölçeklendirerek metin alanını biçimlendiriyor. Bu örnek, `subtitle` yeni bir satırda özelliğini de ekler, aşağı doğru küçültülmüş yazı tipi boyutu ve kırmızı renkli.

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

Bu katman, aşağıdaki resimde gösterildiği gibi nokta özelliğini işleyecek:
 
<center>

![Biçimlendirilmiş metin alanı](media/how-to-expressions/text-field-format-expression.png) ile Nokta özelliğinin görüntüsü</center>

### <a name="number-format-expression"></a>Sayı biçimi ifadesi

İfade `number-format` yalnızca bir sembol `textField` katmanı seçeneğiyle kullanılabilir. Bu ifade, sağlanan sayıyı biçimlendirilmiş bir dize dönüştürür. Bu ifade [JavaScript'in Number.toLocalString](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) işlevini sarar ve aşağıdaki seçenekler kümesini destekler.

 * `locale`- Sayıları belirtilen dille hizaladan bir şekilde dizeleri dönüştürmek için bu seçeneği belirtin. BCP [47 dil etiketini](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation) bu seçeneğe geçirin.
 * `currency`- Sayıyı para birimini temsil eden bir dize dönüştürmek için. Olası değerler, ABD doları için "USD", euro için "EUR" veya Çin RMB için "CNY" gibi [ISO 4217 para birimi kodlarıdır.](https://en.wikipedia.org/wiki/ISO_4217)
 * `'min-fraction-digits'`- Sayının dize sürümüne dahil edilecek en az ondalık basamak sayısını belirtir.
 * `'max-fraction-digits'`- Sayının dize sürümüne dahil edilecek en fazla ondalık basamak sayısını belirtir.

Aşağıdaki pseudocode metin alanı biçimi ifadesinin yapısını tanımlar. 

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

Aşağıdaki örnekte, `number-format` nokta özelliğinin `revenue` özelliğinin ABD doları değeri `textField` görünecek şekilde bir sembol katmanı seçeneğinde nasıl işlendiğini değiştirmek için bir ifade kullanılır.

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

Bu katman, aşağıdaki resimde gösterildiği gibi nokta özelliğini işleyecek:

<center>

![Sayı biçimi](media/how-to-expressions/number-format-expression.png) ifade örneği</center>

### <a name="image-expression"></a>Görüntü ifadesi

Görüntü ifadesi, bir sembol `image` `textField` katmanının seçenekleri ve çokgen katmanı `fillPattern` seçeneği ile kullanılabilir. Bu ifade, istenen görüntünün stilde var olup olmadığını denetler `null`ve görüntünün şu anda stilde olup olmadığına bağlı olarak çözülmüş görüntü adını veya döndürüleceğini denetler. Bu doğrulama işlemi eşzamanlıdır ve görüntü bağımsız değişkeninde istemeden önce görüntünün stile eklenmesini gerektirir.

**Örnek**

Aşağıdaki örnekte, `image` sembol katmanında metin bulunan bir simge satır da eklemek için bir ifade kullanılır. 

```javascript
 //Load the custom image icon into the map resources.
map.imageSprite.add('wifi-icon', 'wifi.png').then(function () {

    //Create a data source and add it to the map.
    datasource = new atlas.source.DataSource();
    map.sources.add(datasource);

    //Create a point feature and add it to the data source.
    datasource.add(new atlas.data.Point(map.getCamera().center));

    //Add a layer for rendering point data as symbols.
    map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            image: 'none'
        },
        textOptions: {
            //Create a formatted text string that has an icon in it.
            textField: ["format", 'Ricky\'s ', ["image", "wifi-icon"], ' Palace']
        }
    }));
});
```

Bu katman, aşağıdaki resimde gösterildiği gibi sembol katmanındaki metin alanını işleyecek:

<center>

![Resim ifade](media/how-to-expressions/image-expression.png) örneği</center>

## <a name="zoom-expression"></a>Yakınlaştırma ifadesi

Bir `zoom` ifade, oluşturma zamanında haritanın geçerli yakınlaştırma düzeyini almak `['zoom']`için kullanılır ve . Bu ifade, haritanın en az ve maksimum yakınlaştırma düzeyi aralığı arasında bir sayı döndürür. Web ve Android için Azure Haritalar etkileşimli harita denetimleri 0 ile 24 arasında numaralanmış 25 yakınlaştırma seviyesini destekler. İfadeyi `zoom` kullanmak, haritanın yakınlaştırma düzeyi değiştirildikçe stillerin dinamik olarak değiştirilmesine olanak tanır. İfade `zoom` yalnızca ve `interpolate` `step` ifadeler ile kullanılabilir.

**Örnek**

Varsayılan olarak, ısı haritası katmanında işlenen veri noktalarının yarıçapı, tüm yakınlaştırma düzeyleri için sabit bir piksel yarıçapına sahiptir. Harita yakınlaştıkça, veriler bir araya toplanır ve ısı haritası katmanı farklı görünür. Her `zoom` veri noktasının haritanın aynı fiziksel alanını kapladığı her yakınlaştırma düzeyi için yarıçapı ölçeklendirmek için bir ifade kullanılabilir. Isı haritası tabakasının daha statik ve tutarlı görünmesini sağlayacaktır. Haritanın her yakınlaştırma düzeyi, önceki yakınlaştırma düzeyinin iki katı dikey ve yatay piksele sahiptir. Her zum seviyesiyle iki katına çıkacak şekilde yarıçapı ölçekleme, tüm yakınlaştırma düzeylerinde tutarlı görünen bir ısı haritası oluşturur. Aşağıda gösterildiği gibi bir `zoom` `base 2 exponential interpolation` ifade ile ifade kullanılarak gerçekleştirilebilir. 

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

[Canlı örneğe bakın](map-add-heat-map-layer.md#consistent-zoomable-heat-map)

## <a name="variable-binding-expressions"></a>Değişken bağlama ifadeleri

Değişken bağlama ifadeleri bir hesaplamanın sonuçlarını bir değişkende saklar. Böylece, hesaplama sonuçları başka bir yerde bir ifade birden çok kez başvurulabilir. Birçok hesaplama içeren ifadeler için yararlı bir optimizasyondur.

| İfadeler | Dönüş türü | Açıklama |
|--------------|---------------|--------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'hadi',<br/>&nbsp;&nbsp;&nbsp;&nbsp;name1: dize,<br/>&nbsp;&nbsp;&nbsp;&nbsp;value1: herhangi bir,<br/>&nbsp;&nbsp;&nbsp;&nbsp;name2: dize,<br/>&nbsp;&nbsp;&nbsp;&nbsp;value2: herhangi bir,<br/>&nbsp;&nbsp;&nbsp;&nbsp;…<br/>&nbsp;&nbsp;&nbsp;&nbsp;childExpression<br/>\] | | Sonucu döndüren alt `var` ifadedeki ifadeyle kullanılmak üzere bir veya daha fazla değeri değişken olarak depolar. |
| `['var', name: string]` | herhangi bir | İfade kullanılarak oluşturulan bir `let` değişkene başvurur. |

**Örnek**

Bu örnek, sıcaklık oranına göre geliri hesaplayan bir `case` ifade kullanır ve sonra bu değer üzerinde farklı boolean işlemleri değerlendirmek için bir ifade kullanır. İfade, `let` geliri sıcaklık oranına göre depolamak için kullanılır, böylece yalnızca bir kez hesaplanması gerekir. İfade, `var` bu değişkeni yeniden hesaplamak zorunda kalmadan gerektiğinde sık sık başvurur.

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

İfadeleri uygulayan daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"] 
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"] 
> [Baloncuk katmanı ekleme](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](map-add-shape.md)

> [!div class="nextstepaction"] 
> [Isı haritası katmanı ekleme](map-add-heat-map-layer.md)

İfadeleri destekleyen katman seçenekleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"] 
> [BubbleLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [HeatMapLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [LineLayer Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [ÇokgenLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"] 
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest) 
