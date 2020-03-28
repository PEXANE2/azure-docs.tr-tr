---
title: "Öğretici: Google Haritalar'dan bir web uygulamasını geçirin | Microsoft Azure Haritaları"
description: Bir web uygulamasını Google Haritalar'dan Microsoft Azure Haritalar'a nasıl geçirebilirsiniz?
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: bdbf2a975cbdc3d06745b9375c1e6f8e751ddfd6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77914111"
---
# <a name="migrate-a-web-app-from-google-maps"></a>Google Haritalar'dan bir web uygulamasını geçirme

Google Haritalar'ı kullanan çoğu web uygulaması Google Maps V3 JavaScript SDK'yı kullanır. Azure Haritalar Web SDK, geçiş yapmak için uygun Azure tabanlı SDK'dır. Azure Haritalar Web SDK, etkileşimli haritaları kendi içeriğinizle ve görsellerinizle özelleştirmenize olanak tanır. Uygulamanızı hem web'de hem de mobil uygulamalarda çalıştırabilirsiniz. Bu denetimde büyük veri kümelerini yüksek performansla oluşturmanızı sağlayan WebGL bileşeni kullanılmaktadır. JavaScript veya TypeScript kullanarak bu SDK ile geliştirin.

Varolan bir web uygulamasını geçirerek geçirin, açık kaynak kodlu bir harita denetim kitaplığı kullanıp kullanmayacağından emin olanın. Açık kaynak harita denetim kitaplığı örnekleri şunlardır: Sezyum, Broşür ve OpenLayers. Açık kaynak kodlu harita denetim kitaplığı kullansa ve Azure Haritalar Web SDK'sını kullanmak istemeseniz bile uygulamanızı yine de geçirebilirsiniz. Bu durumda, uygulamanızı Azure Haritalar döşeme hizmetlerine[(yol kutucukuydu](https://docs.microsoft.com/rest/api/maps/render/getmaptile) \| [kutucukları)](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile)bağlayın. Azure Haritalar'ın yaygın olarak kullanılan bazı açık kaynak harita denetim kitaplıklarında nasıl kullanılacağı nasıI kullanılacağına ilişkin aşağıdaki noktalar ayrıntılı olarak açıklanmıştır.

- Sezyum - Web için bir 3D harita kontrolü. [Kod örnek](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20Cesium%20JS) \| [Belgeler](https://cesiumjs.org/)
- Broşür – Web için hafif 2B harita kontrolü. [Kod örnek](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Azure%20Maps%20Raster%20Tiles%20in%20Leaflet%20JS) \| [Belgeler](https://leafletjs.com/)
- OpenLayers - Web için projeksiyonları destekleyen bir 2B harita denetimi. [Kod örnek](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20OpenLayers) \| [Belgeler](https://openlayers.org/)

## <a name="key-features-support"></a>Temel özellikler desteği

Tablo, Google Haritalar V3 JavaScript SDK'daki temel API özelliklerini ve Azure Haritalar Web SDK'sında desteklenen API özelliğini listeler.

| Google Haritalar özelliği     | Azure Haritalar Web SDK desteği |
|-------------------------|:--------------------------:|
| İşaretler                 | ✓                          |
| İşaretçi kümeleme       | ✓                          |
| Çokgenler & Çokgenler    | ✓                          |
| Veri katmanları             | ✓                          |
| Zemin Kaplamaları         | ✓                          |
| Isı haritaları               | ✓                          |
| Kiremit Katmanları             | ✓                          |
| KML Katmanı               | ✓                          |
| Çizim araçları           | ✓                          |
| Geocoder hizmeti        | ✓                          |
| Yol tarifi hizmeti      | ✓                          |
| Mesafe Matrisi hizmeti | ✓                          |
| Yükseklik hizmeti       | Planlandı                    |

## <a name="notable-differences-in-the-web-sdks"></a>Web SDK'larında önemli farklar

Aşağıda, Google Haritalar ve Azure Haritalar Web SK'ları arasındaki bazı önemli farklar şunlardır:

- Azure Haritalar Web SDK'sına erişmek için barındırılan bir bitiş noktası sağlamanın yanı sıra, bir NPM paketi de mevcuttur. Web SDK paketini uygulamalara gömün. Daha fazla bilgi için bu [belgelere](how-to-use-map-control.md)bakın. Bu paket, TypeScript tanımlarını da içerir.
- Öncelikle Azure Haritalar'da Harita sınıfının bir örneğini oluşturmanız gerekir. Haritayla programlı olarak etkileşime girmeden önce haritaların `ready` veya `load` etkinliğin ateşlemesini bekleyin. Bu sipariş, tüm harita kaynaklarının yüklenmesini ve erişilmeye hazır olmasını sağlar.
- Her iki platform da temel haritalar için benzer bir döşeme sistemi kullanır. Google Haritalar'daki karolar 256 piksel boyutundadır; ancak Azure Haritalar'daki kutucuklar 512 piksel boyutundadır. Azure Haritalar'da Google Haritalar ile aynı harita görünümünü elde etmek için, Google Haritalar yakınlaştırma düzeyini Azure Haritalar'da bir numaraya kadar çıkarın.
- Google Haritalar'daki koordinatlar "enlem, boylam" olarak adlandırılırken, Azure Haritalar "boylam,enlem" kullanır. Azure Haritalar biçimi, çoğu GIS platformu tarafından izlenen standartla `[x, y]`hizalanır.
- Azure Haritalar Web SDK'daki şekiller GeoJSON şemasına dayanır. Yardımcı sınıflar [ *atlas.data* ad alanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data?view=azure-iot-typescript-latest)üzerinden ortaya çıkarır. Bir de [*atlas var. Şekil*](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape) sınıfı. GeoJSON nesnelerini sarmak, verilerin bağlanabilir şekilde güncelleştirilip korunmasını kolaylaştırmak için bu sınıfı kullanın.
- Azure Haritalar'daki koordinatlar Konum nesneleri olarak tanımlanır. Koordinat, biçimdeki `[longitude,latitude]`bir sayı dizisi olarak belirtilir. Veya, yeni atlas.data.Position (boylam, enlem) kullanılarak belirtilir.
    > [!TIP]
    > Position sınıfı, "enlem, boylam" biçiminde olan koordinatları almak için statik bir yardımcı yöntemi vardır. [Atlas.data.Position.fromLatLng](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest) yöntemi genellikle Google Haritalar `new google.maps.LatLng` kodundaki yöntemle değiştirilebilir.
- Azure Haritalar, haritaya eklenen her şekil için stil bilgilerini belirtmek yerine stilleri verilerden ayırır. Veriler bir veri kaynaklarında depolanır ve işleme katmanlarına bağlanır. Azure Haritalar kodu verileri işlemek için veri kaynaklarını kullanır. Bu yaklaşım gelişmiş performans avantajı sağlar. Ayrıca, birçok katman, iş mantığının katman stili seçeneklerine eklenebileceği veri tabanlı stili destekler. Bu destek, şekilde tanımlanan özelliklere göre tek tek şekillerin katman içinde nasıl işlenir olduğunu değiştirir.

## <a name="web-sdk-side-by-side-examples"></a>Web SDK yan yana örnekler

Bu koleksiyonda her platform için kod örnekleri vardır ve her örnek ortak bir kullanım örneğini kapsar. Web uygulamanızı Google Haritalar V3 JavaScript SDK'dan Azure Haritalar Web SDK'sına geçirmenize yardımcı olmak için tasarlanmıştır. Web uygulamalarıyla ilgili kod örnekleri JavaScript'te verilmiştir. Ancak, Azure Haritalar, [NPM modülü](how-to-use-map-control.md)aracılığıyla ek seçenek olarak TypeScript tanımlarını da sağlar.

### <a name="load-a-map"></a>Harita yükleme

Her iki SDK'nın da harita yüklemek için aynı adımları vardır:

- Harita SDK'ya bir başvuru ekleyin.
- Harita `div` için yer tutucu olarak hareket edecek sayfanın gövdesine bir etiket ekleyin.
- Sayfa yüklendiğinde çağrılan bir JavaScript işlevi oluşturun.
- İlgili harita sınıfının bir örneğini oluşturun.

**Bazı önemli farklar**

- Google haritalar, API'nin komut dosyası referansında bir hesap anahtarının belirtilmesini gerektirir. Azure Haritalar için kimlik doğrulama kimlik bilgileri, harita sınıfının seçenekleri olarak belirtilir. Bu kimlik bilgisi bir abonelik anahtarı veya Azure Etkin Dizin bilgileri olabilir.
- Google Haritalar, api'nin komut dosyası referansında, haritayı yüklemek için bir başlatma işlevi çağırmak için kullanılan bir geri arama işlevini kabul eder. Azure Haritalar'da, sayfanın yük olayı kullanılmalıdır.
- Haritanın `div` oluşturulacağı öğeye başvururken, Azure Haritalar'daki `Map` sınıf yalnızca `id` Google Haritalar'da `HTMLElement` bir nesne gerektirirken değeri gerektirir.
- Azure Haritalar'daki koordinatlar, biçimdeki `[longitude, latitude]`basit bir sayı dizisi olarak belirtilebilen Konum nesneleri olarak tanımlanır.
- Azure Haritalar'daki yakınlaştırma düzeyi, Google Haritalar'daki yakınlaştırma düzeyinden bir kat daha düşüktür. Bu tutarsızlık, iki platformun döşeme sisteminin boyutlarındaki farktan kaynaklanmasıdır.
- Azure Haritalar, harita tuvaline herhangi bir gezinti denetimi eklemez. Bu nedenle, varsayılan olarak, bir haritanın yakınlaştırma düğmeleri ve harita stili düğmeleri yoktur. Ancak, harita stili seçici, yakınlaştırma düğmeleri, pusula veya döndürme kontrolü ve bir adım denetimi eklemek için kontrol seçenekleri vardır.
- Harita örneğinin `ready` olayını izlemek için Azure Haritalar'a bir olay işleyicisi eklenir. Bu olay, harita WebGL bağlamını ve gerekli tüm kaynakları yüklemeyi bitirdiğinde ateşlenir. Bu olay işleyicisine, harita yüklemeyi tamamladıktan sonra çalıştırmak istediğiniz kodu ekleyin.

Aşağıdaki temel örnekler, koordinatlarda New York üzerinde ortalanmış bir harita yüklemek için Google Haritalar'ı kullanır. Boylam: -73.985, enlem: 40.747 ve harita 12 zoom düzeyindedir.

**Önce: Google Haritalar**

Bir konum üzerinde ortalanmış ve yakınlaştırılmış bir Google Haritası görüntüleyin.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(40.747, -73.985),
                zoom: 12
            });
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

Bu kodu bir tarayıcıda çalıştırmak, aşağıdaki resme benzeyen bir harita görüntüler:

<center>

![Basit Google Haritalar](media/migrate-google-maps-web-app/simple-google-map.png)</center>

**Sonra: Azure Haritalar**

Bir harita stili denetimi ve yakınlaştırma düğmeleriyle birlikte Azure Haritalar'da aynı görünüme sahip bir harita yükleyin.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-73.985, 40.747],  //Format coordinates as longitude, latitude.
                zoom: 11,   //Subtract the zoom level by one.

                //Specify authentication information when loading the map.
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {
                //Add zoom controls to bottom right of map.
                map.controls.add(new atlas.control.ZoomControl(), {
                    position: 'bottom-right'
                });

                //Add map style control in top left corner of map.
                map.controls.add(new atlas.control.StyleControl(), {
                    position: 'top-left'
                });
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

Bu kodu bir tarayıcıda çalıştırmak, aşağıdaki resme benzeyen bir harita görüntüler:

<center>

![Basit Azure Haritaları](media/migrate-google-maps-web-app/simple-azure-maps.png)</center>

Azure Haritalar harita denetiminin nasıl ayarlanıp kullanılacağıyla ilgili ayrıntılı belgeleri [buraya](how-to-use-map-control.md)tıklayarak bir web uygulamasında bulun.

> [!NOTE]
> Azure Haritalar, Google Haritalar'ın aksine, haritayı yüklemek için başlangıç merkezi ve yakınlaştırma düzeyi gerektirmez. Bu bilgiler haritayı yüklerken sağlanmazsa, Azure haritaları kullanıcının şehrini belirlemeye çalışır. Haritayı oraya merkezip yakınlaştıracak.

**Ek kaynaklar:**

- Azure Haritalar, [burada](map-add-controls.md)belgelendikçe harita görünümünü döndürmek ve oluşturmak için navigasyon denetimleri de sağlar.

### <a name="localizing-the-map"></a>Haritayı yerelleştirme

Hedef kitleniz birden fazla ülkeye yayılmışsa veya farklı diller konuşuyorsa, yerelleştirme önemlidir.

**Önce: Google Haritalar**

Google Haritalar'ı yerelleştirmek için dil ve bölge parametreleri ekleyin.

```html
<script type="text/javascript" src=" https://maps.googleapis.com/maps/api/js?callback=initMap&key=[api_key]& language=[language_code]&region=[region_code]" async defer></script>
```

Burada "fr-FR" olarak ayarlanmış dil ile Google Maps bir örnektir.

<center>

![Google Haritalar yerelleştirme](media/migrate-google-maps-web-app/google-maps-localization.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar, haritanın dilini ve bölgesel görünümünü ayarlamanın iki farklı yolunu sağlar. İlk seçenek, bu bilgileri global *atlas* ad alanına eklemektir. Uygulamanızdaki tüm harita denetimi örneklerinin bu ayarları varsayılan olarak karşılanmasına neden olur. Aşağıdaki dil Fransızca ("fr-FR") ve "otomatik" için bölgesel görünümü ayarlar:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

İkinci seçenek, haritayı yüklerken bu bilgileri harita seçeneklerine aktarmaktır. Böyle:

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'auto',

    authOptions: {
        authType: 'subscriptionKey',
        subscriptionKey: '<Your Azure Maps Key>'
    }
});
```

> [!NOTE]
> Azure Haritalar ile aynı sayfadaki birden çok harita örneğini farklı dil ve bölge ayarlarıyla yüklemek mümkündür. Bu ayarları yüklendikten sonra haritada da güncelleştirmek mümkündür. 

Azure Haritalar'da [desteklenen dillerin](supported-languages.md) ayrıntılı bir listesini bulun.

Burada, "fr" olarak ayarlanmış dil ve kullanıcı bölgesi "fr-FR" olarak ayarlanmış Azure Haritalar örneği verilmiştir.

<center>

![Azure Haritalar yerelleştirme](media/migrate-google-maps-web-app/azure-maps-localization.png)</center>

### <a name="setting-the-map-view"></a>Harita görünümünü ayarlama

Hem Azure hem de Google Haritalar'daki dinamik haritalar programlı olarak yeni coğrafi konumlara taşınabilir. Bunu yapmak için JavaScript'teki uygun işlevleri arayın. Örnekler, haritanın uydu havadan görüntülerini nasıl göstereceğini, haritayı bir konuma nasıl ortalayıp, Google Haritalar'da yakınlaştırma düzeyini 15'e nasıl değiştireceğini gösterir. Aşağıdaki konum koordinatları kullanılır: boylam: -111.0225 ve enlem: 35.0272.

> [!NOTE]
> Google Haritalar boyutları 256 piksel, Azure Haritalar ise daha büyük bir 512 piksel kutucuk kullanır. Bu nedenle, Azure Haritalar, Google Haritalar ile aynı harita alanını yüklemek için daha az sayıda ağ isteği gerektirir. Harita denetimlerinde kiremit piramitlerinin çalışma şekli nedeniyle, Azure Haritalar'ı kullanırken Google Haritalar'da kullanılan yakınlaştırma düzeyini bir numaraya çıkarmanız gerekir. Bu aritmetik işlem, Azure Haritalar'daki daha büyük döşemelerin Google Haritalar'dakiyle aynı harita alanını işlememesini sağlar,

**Önce: Google Haritalar**

Yöntemi kullanarak Google Haritalar `setOptions` harita denetimini taşıyın. Bu yöntem, haritanın merkezini ve yakınlaştırma düzeyini belirtmenizi sağlar.

```javascript
map.setOptions({
    mapTypeId: google.maps.MapTypeId.SATELLITE,
    center: new google.maps.LatLng(35.0272, -111.0225),
    zoom: 15
});
```

<center>

![Google Haritalar görünümü ayarlı](media/migrate-google-maps-web-app/google-maps-set-view.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar'da, `setCamera` yöntemi kullanarak harita konumunu ve yöntemi `setStyle` kullanarak harita stilini değiştirin. Azure Haritalar'daki koordinatlar "boylam, enlem" biçimindedir ve zum düzeyi değeri bir kişi tarafından çıkarılır.

```javascript
map.setCamera({
    center: [-111.0225, 35.0272],
    zoom: 14
});

map.setStyle({
    style: 'satellite'
});
```

<center>

![Azure Haritalar görünümü ayarlı](media/migrate-google-maps-web-app/azure-maps-set-view.jpeg)</center>

**Ek kaynaklar:**

- [Harita stili seçme](choose-map-style.md)
- [Desteklenen eşleme stilleri](supported-map-styles.md)

### <a name="adding-a-marker"></a>İşaretçi ekleme

Azure Haritalar'da, nokta verilerinin haritada işlenmenin birden çok yolu vardır:

- **HTML İşaretleyiciler** – Geleneksel DOM öğelerini kullanarak noktaları işler. HTML İşaretçileri sürüklemeyi destekler.
- **Sembol Katmanı** – WebGL bağlamında noktaları bir simge veya metinle işler.
- **Kabarcık Katmanı** – Haritada noktaları daireler halinde işler. Dairelerin yarıçapları verilerdeki özelliklere göre ölçeklendirilebilir.

WebGL bağlamında Sembol katmanlarını ve Kabarcık katmanlarını işle. Her iki katman da haritaüzerinde büyük nokta kümeleri işleyebilir. Bu katmanlar, verilerin bir veri kaynağında depolanmasını gerektirir. `ready` Olay ateşlendikten sonra veri kaynakları ve işleme katmanları haritaya eklenmelidir. HTML İşaretçileri sayfa içinde DOM öğesi olarak işlenir ve veri kaynağı kullanmaz. Bir sayfada ne kadar çok DOM öğesi ne kadar çok olursa, sayfa o kadar yavaş olur. Bir haritada birkaç yüzden fazla nokta işlettiyse, bunun yerine işleme katmanlarından birini kullanmanız önerilir.

Etiket olarak 10 numaranın yer aldığı haritaya bir işaretçi ekleyelim. Boylam kullanın: -0.2 ve enlem: 51.5.

**Önce: Google Haritalar**

Google Haritalar'da, `google.maps.Marker` sınıfı kullanarak haritaya işaretçiler ekleyin ve haritayı seçeneklerden biri olarak belirtin.

```javascript
//Create a marker and add it to the map.
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(51.5, -0.2),
    label: '10',
    map: map
});
```

<center>

![Google Haritalar işareti](media/migrate-google-maps-web-app/google-maps-marker.png)</center>

**Sonra: HTML İşaretçileri kullanarak Azure Haritalar**

Azure Haritalar'da, haritada bir noktayı görüntülemek için HTML işaretçileri kullanın. Haritada yalnızca az sayıda nokta görüntülemesi gereken uygulamalar için HTML işaretçileri önerilir. HTML işaretçisi kullanmak için `atlas.HtmlMarker` sınıfın bir örneğini oluşturun. Metin ve konum seçeneklerini ayarlayın ve `map.markers.add` yöntemi kullanarak işaretçiyi eşmeye ekleyin.

```javascript
//Create a HTML marker and add it to the map.
map.markers.add(new atlas.HtmlMarker({
    text: '10',
    position: [-0.2, 51.5]
}));
```

<center>

![Azure Haritalar HTML işaretçisi](media/migrate-google-maps-web-app/azure-maps-html-marker.png)</center>

**Sonra: Sembol Katmanı kullanarak Azure Haritalar**

Sembol katmanı için verileri bir veri kaynağına ekleyin. Veri kaynağını katmana takın. Ayrıca, `ready` olay ateşlendikten sonra veri kaynağı ve katmanı haritaya eklenmelidir. Bir sembolün üzerinde benzersiz bir metin değeri işlemek için metin bilgilerinin veri noktasının bir özelliği olarak depolanması gerekir. Özellik katman seçeneğinde `textField` başvurulmalıdır. Bu yaklaşım HTML işaretçileri kullanarak biraz daha fazla iş, ancak daha iyi performans.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-0.2, 51.5],
                zoom: 9,
                
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Create a point feature, add a property to store a label for it, and add it to the data source.
                datasource.add(new atlas.data.Feature(new atlas.data.Point([-0.2, 51.5]), {
                    label: '10'
                }));

                //Add a layer for rendering point data as symbols.
                map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
                    textOptions: {
                        //Use the label property to populate the text for the symbols.
                        textField: ['get', 'label'],
                        color: 'white',
                        offset: [0, -1]
                    }
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Haritalar sembol katmanı](media/migrate-google-maps-web-app/azure-maps-symbol-layer.png)</center>

**Ek kaynaklar:**

- [Veri kaynağı oluşturma](create-data-source-web-sdk.md)
- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [HTML İşaretleyicileri Ekle](map-add-custom-html.md)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)
- [Sembol katmanı simgesi seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)
- [Sembol katmanı metin seçeneği](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)
- [HTML işaretçi sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)
- [HTML işaretleyici seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

### <a name="adding-a-custom-marker"></a>Özel işaretçi ekleme

Noktaları haritada temsil etmek için Özel görseller kullanabilirsiniz. Aşağıdaki harita, haritada bir noktayı görüntülemek için özel bir resim kullanır. Nokta enlemde görüntülenir: 51.5 ve boylam: -0.2. Bağlantı işaretçinin konumunu dengeler, böylece retam simgesinin noktası haritadaki doğru konumla hizalanır.

<center>

![sarı pushpin görüntü](media/migrate-google-maps-web-app/ylw_pushpin.png)<br/>
ylw\_pushpin.png</center>

**Önce: Google Haritalar**

Görüntünün içini `url` içeren `Icon` bir nesne belirterek özel bir işaretçi oluşturun. Reşat görüntüsünün noktasını haritadaki koordinatla hizalamak için bir `anchor` nokta belirtin. Google Haritalar'daki bağlantı değeri, resmin sol üst köşesine göredir.

```javascript
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(51.5, -0.2),
    icon: {
        url: 'https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png',
        anchor: new google.maps.Point(5, 30)
    },
    map: map
});
```

<center>

![Google Haritalar özel işaretçisi](media/migrate-google-maps-web-app/google-maps-custom-marker.png)</center>

**Sonra: HTML İşaretçileri kullanarak Azure Haritalar**

BIR HTML işaretçisini özelleştirmek `string` `HTMLElement` için `htmlContent` bir HTML'yi veya işaretleyiciseçeneğine geçirin. İşaretçinin konum koordinatına göre göreli konumunu belirtmek için `anchor` seçeneği kullanın. Tanımlanan dokuz başvuru noktasından `anchor` birini seçene atayın. Bu tanımlanan noktalar şunlardır: "merkez", "üst", "alt", "sol", "sağ", "üst-sol", "üst-sağ", "alt-sol", "alt-sağ". İçerik varsayılan olarak html içeriğinin alt merkezine sabitlenir. Kodu Google Haritalar'dan geçirmeyi kolaylaştırmak için, `anchor` kodu "sol üstte" olarak `pixelOffset` ayarlayın ve ardından Google Haritalar'da kullanılan aynı ofset ile seçeneği kullanın. Azure Haritalar'daki uzaklıklar, Google Haritalar'daki uzaklıkların tersi yönde hareket eder. Yani, uzaklıkları eksi bir ile çarpın.

> [!TIP]
> Microsoft `pointer-events:none` Edge'de istenmeyen bir simge görüntüleyecek varsayılan sürükleme davranışını devre dışı atmak için html içeriğine stil olarak ekleyin.

```javascript
map.markers.add(new atlas.HtmlMarker({
    htmlContent: '<img src="https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png" style="pointer-events: none;" />',
    anchor: 'top-left',
    pixelOffset: [-5, -30],
    position: [-0.2, 51.5]
}));
```

<center>

![Azure Haritalar özel HTML işaretçisi](media/migrate-google-maps-web-app/azure-maps-custom-html-marker.png)</center>

**Sonra: Sembol Katmanı kullanarak Azure Haritalar**

Azure Haritalar'daki sembol katmanları özel görüntüleri de destekler. İlk olarak, resmi harita kaynaklarına yükleyin ve benzersiz bir kimlikle atayın. Sembol katmanındaki resme başvurun. Görüntüyü `offset` haritadaki doğru noktaya hizalamak için seçeneği kullanın. Konum `anchor` koordinatlarına göre sembolün göreli konumunu belirtmek için seçeneği kullanın. Tanımlanan dokuz başvuru noktasından birini kullanın. Bu noktalar şunlardır: "merkez", "üst", "alt", "sol", "sağ", "üst-sol", "üst-sağ", "alt-sol", "alt-sol". İçerik varsayılan olarak html içeriğinin alt merkezine sabitlenir. Kodu Google Haritalar'dan geçirmeyi kolaylaştırmak için, `anchor` kodu "sol üstte" olarak `offset` ayarlayın ve ardından Google Haritalar'da kullanılan aynı ofset ile seçeneği kullanın. Azure Haritalar'daki uzaklıklar, Google Haritalar'daki uzaklıkların tersi yönde hareket eder. Yani, uzaklıkları eksi bir ile çarpın.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-0.2, 51.5],
                zoom: 9,
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Load the custom image icon into the map resources.
                map.imageSprite.add('my-yellow-pin', 'https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png').then(function () {

                    //Create a data source and add it to the map.
                    datasource = new atlas.source.DataSource();
                    map.sources.add(datasource);

                    //Create a point and add it to the data source.
                    datasource.add(new atlas.data.Point([-0.2, 51.5]));

                    //Add a layer for rendering point data as symbols.
                    map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
                        iconOptions: {
                            //Set the image option to the id of the custom icon that was loaded into the map resources.
                            image: 'my-yellow-pin',
                            anchor: 'top-left',
                            offset: [-5, -30]
                        }
                    }));
                });
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Haritalar özel simge simge katmanı](media/migrate-google-maps-web-app/azure-maps-custom-icon-symbol-layer.png)</center>

> [!TIP]
> Gelişmiş özel noktaları işlemek için birden çok görüntüleme katmanını birlikte kullanın. Örneğin, farklı renkli dairelerüzerinde aynı simgeye sahip birden çok tokanız olmasını istediğinizi varsayalım. Her renk kaplaması için bir demet görüntü oluşturmak yerine, kabarcık katmanının üstüne bir sembol katmanı ekleyin. Pırburların aynı veri kaynağına başvurmasını iste. Bu yaklaşım, bir grup farklı görüntü oluşturmak ve korumaktan daha verimli olacaktır.

**Ek kaynaklar:**

- [Veri kaynağı oluşturma](create-data-source-web-sdk.md)
- [Sembol katmanı ekleme](map-add-pin.md)
- [HTML İşaretleyicileri Ekle](map-add-custom-html.md)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)
- [Sembol katmanı simgesi seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)
- [Sembol katmanı metin seçeneği](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)
- [HTML işaretçi sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)
- [HTML işaretleyici seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

### <a name="adding-a-polyline"></a>Polyline ekleme

Haritada bir satırı veya yolu temsil etmek için çok çizgileri kullanın. Haritada kesik li bir poliline oluşturalım.

**Önce: Google Haritalar**

Polyline sınıfı bir seçenek kümesini kabul eder. Polyline `path` seçeneğinde bir dizi koordinat geçirin.

```javascript
//Get the center of the map.
var center = map.getCenter();

//Define a symbol using SVG path notation, with an opacity of 1.
var lineSymbol = {
    path: 'M 0,-1 0,1',
    strokeOpacity: 1,
    scale: 4
};

//Create the polyline.
var line = new google.maps.Polyline({
    path: [
        center,
        new google.maps.LatLng(center.lat() - 0.5, center.lng() - 1),
        new google.maps.LatLng(center.lat() - 0.5, center.lng() + 1)
    ],
    strokeColor: 'red',
    strokeOpacity: 0,
    strokeWeight: 4,
    icons: [{
        icon: lineSymbol,
        offset: '0',
        repeat: '20px'
    }]
});

//Add the polyline to the map.
line.setMap(map);
```

<center>

![Google Haritalar polyline](media/migrate-google-maps-web-app/google-maps-polyline.png)</center>

**Sonra: Azure Haritalar**

Çok çizgiler `LineString` denir `MultiLineString` veya nesneler. Bu nesneler bir veri kaynağına eklenebilir ve bir satır katmanı kullanılarak işlenebilir. Bir `LineString` veri kaynağına ekleyin, sonra veri `LineLayer` kaynağını işlemek için a'ya ekleyin.

```javascript
//Get the center of the map.
var center = map.getCamera().center;

//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a line and add it to the data source.
datasource.add(new atlas.data.LineString([
    center,
    [center[0] - 1, center[1] - 0.5],
    [center[0] + 1, center[1] - 0.5]
]));

//Add a layer for rendering line data.
map.layers.add(new atlas.layer.LineLayer(datasource, null, {
    strokeColor: 'red',
    strokeWidth: 4,
    strokeDashArray: [3, 3]
}));
```

<center>

![Azure Haritalar polyline](media/migrate-google-maps-web-app/azure-maps-polyline.png)</center>

**Ek kaynaklar:**

- [Haritaya satır ekleme](map-add-line-layer.md)
- [Çizgi katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="adding-a-polygon"></a>Çokgen ekleme

Azure Haritalar ve Google Haritalar çokgenler için benzer destek sağlar. Çokgenler haritaüzerinde bir alanı temsil etmek için kullanılır. Aşağıdaki örnekler, haritanın merkez koordinatına dayalı bir üçgen oluşturan bir çokgenin nasıl oluşturulacak olduğunu gösterir.

**Önce: Google Haritalar**

Çokgen sınıfı bir dizi seçeneği kabul eder. Çokgen `paths` seçeneğine koordinatlar bir dizi geçirin.

```javascript
//Get the center of the map.
var center = map.getCenter();

//Create a polygon.
var polygon = new google.maps.Polygon({
    paths: [
        center,
        new google.maps.LatLng(center.lat() - 0.5, center.lng() - 1),
        new google.maps.LatLng(center.lat() - 0.5, center.lng() + 1),
        center
    ],
    strokeColor: 'red',
    strokeWeight: 2,
    fillColor: 'rgba(0, 255, 0, 0.5)'
});

//Add the polygon to the map
polygon.setMap(map);
```

<center>

![Google Haritalar çokgen](media/migrate-google-maps-web-app/google-maps-polygon.png)</center>

**Sonra: Azure Haritalar**

Bir `Polygon` veri `MultiPolygon` kaynağına bir veya nesne ekleyin. Nesneyi katmanlar kullanarak haritada işle. Çokgen tabakası kullanarak çokgenin alanını işleyin. Ve, bir çizgi katmanı kullanarak çokgenin anahatını işleyin.

```javascript
//Get the center of the map.
var center = map.getCamera().center;

//Create a data source and add it to the map.
datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a polygon and add it to the data source.
datasource.add(new atlas.data.Polygon([
    center,
    [center[0] - 1, center[1] - 0.5],
    [center[0] + 1, center[1] - 0.5],
    center
]));

//Add a polygon layer for rendering the polygon area.
map.layers.add(new atlas.layer.PolygonLayer(datasource, null, {
    fillColor: 'rgba(0, 255, 0, 0.5)'
}));

//Add a line layer for rendering the polygon outline.
map.layers.add(new atlas.layer.LineLayer(datasource, null, {
    strokeColor: 'red',
    strokeWidth: 2
}));
```

<center>

![Azure Haritalar çokgen](media/migrate-google-maps-web-app/azure-maps-polygon.png)</center>

**Ek kaynaklar:**

- [Haritaya çokgen ekleme](map-add-shape.md)
- [Haritaya daire ekleme](map-add-shape.md#add-a-circle-to-the-map)
- [Çokgen katman seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)
- [Çizgi katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="display-an-info-window"></a>Bilgi Pencereni Görüntüleme

Bir varlık için ek bilgiler Haritada `google.maps.InfoWindow` Google Haritalar'da sınıf olarak görüntülenebilir. Azure Haritalar'da bu işlevsellik `atlas.Popup` sınıf kullanılarak elde edilebilir. Sonraki örnekler eşmeye bir işaretçi ekler. İşaretçi tıklatıldığında, bir bilgi penceresi veya açılır pencere görüntülenir.

**Önce: Google Haritalar**

Oluşturucuyu kullanarak bir bilgi `google.maps.InfoWindow` penceresini anında hazırla.

```javascript
//Add a marker in which to display an infowindow for.
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(47.6, -122.33),
    map: map
});

//Create an infowindow.
var infowindow = new google.maps.InfoWindow({
    content: '<div style="padding:5px"><b>Hello World!</b></div>'
});

//Add a click event to the marker to open the infowindow.
marker.addListener('click', function () {
    infowindow.open(map, marker);
});
```

<center>

![Google Haritalar açılır pencere](media/migrate-google-maps-web-app/google-maps-popup.png)</center>

**Sonra: Azure Haritalar**

Konum hakkında ek bilgiler görüntülemek için açılır pencereyi kullanalım. Bir HTML `string` `HTMLElement` veya nesneyi açılır pencere `content` seçeneğine geçirin. İsterseniz açılır pencereler herhangi bir şekilde bağımsız olarak görüntülenebilir. Bu nedenle, Pop-up'ların belirtilmesi gereken bir `position` değer gerekir. Değeri `position` belirtin. Açılır pencereyi görüntülemek için `open` yöntemi arayın `map` ve açılır pencerenin görüntüleneceği alanı geçirin.

```javascript
//Add a marker to the map in which to display a popup for.
var marker = new atlas.HtmlMarker({
    position: [-122.33, 47.6]
});

//Add the marker to the map.
map.markers.add(marker);

//Create a popup.
var popup = new atlas.Popup({
    content: '<div style="padding:5px"><b>Hello World!</b></div>',
    position: [-122.33, 47.6],
    pixelOffset: [0, -35]
});

//Add a click event to the marker to open the popup.
map.events.add('click', marker, function () {
    //Open the popup
    popup.open(map);
});
```

<center>

![Azure Haritalar açılır pencere](media/migrate-google-maps-web-app/azure-maps-popup.png)</center>

> [!NOTE]
> Aynı şeyi bir işaretçi yerine seçilen katmanı haritalar olay koduna geçirerek bir sembol, kabarcık, çizgi veya çokgen katmanıyla da yapabilirsiniz.

**Ek kaynaklar:**

- [Açılır pencere ekleme](map-add-popup.md)
- [Medya İçeriği ile Pop-Up](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popup%20with%20Media%20Content)
- [Şekiller üzerinde Pop-up](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popups%20on%20Shapes)
- [Birden Çok Pinle Pop-Up'ı Yeniden Kullanma](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Reusing%20Popup%20with%20Multiple%20Pins)
- [Pop-up sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
- [Pop-up seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

### <a name="import-a-geojson-file"></a>GeoJSON dosyayı alma

Google Haritalar, GeoJSON verilerinin sınıf `google.maps.Data` aracılığıyla yüklenmesi ve dinamik olarak şekillendirinin destekler. Bu sınıfın işlevselliği, Azure Haritalar'ın veri odaklı stiliyle çok daha uyumludur. Ama arada önemli bir fark var. Google Haritalar ile bir geri arama işlevi belirtirsiniz. Her özelliği şekillendirmek için iş mantığı, UI iş parçacığında ayrı ayrı işlenir. Ancak Azure Haritalar'da katmanlar, veri tabanlı ifadeleri stil seçenekleri olarak belirtmeyi destekler. Bu ifadeler ayrı bir iş parçacığı üzerinde işleme zamanında işlenir. Azure Haritalar yaklaşımı, görüntüleme performansını artırır. Daha büyük veri kümelerinin hızlı bir şekilde işlenmesi gerektiğinde bu avantaj fark edilir.

Aşağıdaki örnekler USGS son yedi gün içinde tüm depremlerin bir GeoJSON besleme yük. Deprem verileri harita üzerinde ölçeklenmiş daireler olarak işler. Her dairenin rengi ve ölçeği, veri kümesindeki her özelliğin `"mag"` özelliğinde depolanan her depremin büyüklüğüne dayanır. Büyüklük beşten büyük veya eşitse, daire kırmızı olur. Eğer daha büyük veya üçe eşit, ama beşten az ise, daire turuncu olacaktır. Eğer üçten azsa, çember yeşil olur. Her dairenin yarıçapı 0.1 ile çarpılarak büyüklük üstel olacaktır.

**Önce: Google Haritalar**

Yöntemde tek bir geri `map.data.setStyle` arama işlevi belirtin. Geri arama işlevinin içinde, her özelliğe iş mantığı uygulayın. GeoJSON beslemesini yöntemle yükleyin. `map.data.loadGeoJson`

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2
            });

            //Define a callback to style each feature.
            map.data.setStyle(function (feature) {

                //Extract the 'mag' property from the feature.
                var mag = parseFloat(feature.getProperty('mag'));

                //Set the color value to 'green' by default.
                var color = 'green';

                //If the mag value is greater than 5, set the color to 'red'.
                if (mag >= 5) {
                    color = 'red';
                }
                //If the mag value is greater than 3, set the color to 'orange'.
                else if (mag >= 3) {
                    color = 'orange';
                }

                return /** @type {google.maps.Data.StyleOptions} */({
                    icon: {
                        path: google.maps.SymbolPath.CIRCLE,

                        //Scale the radius based on an exponential of the 'mag' value.
                        scale: Math.exp(mag) * 0.1,
                        fillColor: color,
                        fillOpacity: 0.75,
                        strokeWeight: 2,
                        strokeColor: 'white'
                    }
                });
            });

            //Load the data feed.
            map.data.loadGeoJson(earthquakeFeed);
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Google Haritalar GeoJSON](media/migrate-google-maps-web-app/google-maps-geojson.png)</center>

**Sonra: Azure Haritalar**

GeoJSON, Azure Haritalar'daki temel veri türüdür. Yöntemi kullanarak bir veri `datasource.importFromUrl` kaynağına aktarın. Bir kabarcık tabakası kullanın. Kabarcık katmanı, bir veri kaynağındaki özelliklerin özelliklerine göre ölçeklenmiş daireler oluşturma işlevi sağlar. Geri arama işlevi yerine, iş mantığı bir ifadeye dönüştürülür ve stil seçeneklerine aktarılır. İfadeler, iş mantığının nasıl çalıştığını tanımlar. İfadeler başka bir iş parçacığına aktarılabilir ve özellik verilerine göre değerlendirilebilir. Azure Haritalar'a her biri farklı iş mantığına sahip birden çok veri kaynağı ve katmanı eklenebilir. Bu özellik, birden çok veri kümesinin harita üzerinde farklı şekillerde işlenmesine olanak tanır.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Load the earthquake data.
                datasource.importDataFromUrl(earthquakeFeed);

                //Create a layer to render the data points as scaled circles.
                map.layers.add(new atlas.layer.BubbleLayer(datasource, null, {
                    //Make the circles semi-transparent.
                    opacity: 0.75,

                    color: [
                        'case',

                        //If the mag value is greater than 5, return 'red'.
                        ['>=', ['get', 'mag'], 5],
                        'red',

                        //If the mag value is greater than 3, return 'orange'.
                        ['>=', ['get', 'mag'], 3],
                        'orange',

                        //Return 'green' as a fallback.
                        'green'
                    ],

                    //Scale the radius based on an exponential of the 'mag' value.
                    radius: ['*', ['^', ['e'], ['get', 'mag']], 0.1]
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Haritalar GeoJSON](media/migrate-google-maps-web-app/azure-maps-geojson.png)</center>

**Ek kaynaklar:**

- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="marker-clustering"></a>İşaretçi kümeleme

Haritada birçok veri noktası görselleştirildiğinde, noktalar birbiriyle çakışabilir. Çakışma, haritanın dağınık görünmesini sağlar ve haritanın okunması ve kullanılması zorlaşır. Nokta verilerini kümeleme, birbirine yakın olan veri noktalarını birleştirme ve bunları haritada tek bir kümelenmiş veri noktası olarak temsil etme işlemidir. Kullanıcı haritaya yakınlaştıkça, kümeler kendi veri noktalarına ayrılır. Kullanıcı deneyimini ve harita performansını geliştirmek için veri noktalarını kümeleyin.

Aşağıdaki örneklerde, kod geçen haftadan bir GeoJSON deprem veri beslemesi yükler ve haritaya ekler. Kümeler ölçeklenmiş ve renkli daireler olarak işlenir. Dairelerin ölçeği ve rengi içerdikleri nokta sayısına bağlıdır.

> [!NOTE]
> Google Haritalar ve Azure Haritalar biraz farklı kümeleme algoritmaları kullanır. Bu nedenle, bazen kümelerde nokta dağılımı değişir.

**Önce: Google Haritalar**

İşaretçileri kümelemek için MarkerCluster kitaplığını kullanın. Küme simgeleri, adolarak 1'den 5'e kadar olan görüntülerle sınırlıdır. Aynı dizinde barındırılan lar.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2
            });

            //Download the GeoJSON data.
            fetch(earthquakeFeed)
                .then(function (response) {
                    return response.json();
                }).then(function (data) {
                    //Loop through the GeoJSON data and create a marker for each data point.
                    var markers = [];

                    for (var i = 0; i < data.features.length; i++) {

                        markers.push(new google.maps.Marker({
                            position: new google.maps.LatLng(data.features[i].geometry.coordinates[1], data.features[i].geometry.coordinates[0])
                        }));
                    }

                    //Create a marker clusterer instance and tell it where to find the cluster icons.
                    var markerCluster = new MarkerClusterer(map, markers,
                        { imagePath: 'https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/m' });
                });
        }
    </script>

    <!-- Load the marker cluster library. -->
    <script src="https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/markerclusterer.js"></script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Google Haritalar kümeleme](media/migrate-google-maps-web-app/google-maps-clustering.png)</center>

**Sonra: Azure Haritalar**

Veri kaynağına veri ekleme ve yönetme. Veri kaynaklarını ve katmanlarını bağlayın ve verileri işle. Azure `DataSource` Haritalar'daki sınıf çeşitli kümeleme seçenekleri sağlar.

- `cluster`– Veri kaynağını işaret verilerini kümele'ye bildirir.
- `clusterRadius`- Pikselyarıçapı birlikte noktaları kümelemek için.
- `clusterMaxZoom`- Kümelemenin gerçekleştiği maksimum yakınlaştırma düzeyi. Bu seviyeden daha fazla yakınlaştırırsanız, tüm noktalar sembol olarak işlenir.
- `clusterProperties`- Her küme içindeki tüm noktalara karşılık ifadeler kullanılarak hesaplanan ve her küme noktasının özelliklerine eklenen özel özellikleri tanımlar.

Kümeleme etkinleştirildiğinde, veri kaynağı işleme için katmanlara kümelenmiş ve kümelenmemiş veri noktaları gönderir. Veri kaynağı yüz binlerce veri noktasını kümeleme yeteneğine sahiptir. Kümelenmiş bir veri noktası aşağıdaki özelliklere sahiptir:

| Özellik adı             | Tür    | Açıklama   |
|---------------------------|---------|---------------|
| `cluster`                 | boole | Özellik bir kümeyi temsil edip temsil eder gösterir. |
| `cluster_id`              | string  | Veri Kaynağı `getClusterExpansionZoom`, `getClusterChildren`ve `getClusterLeaves` yöntemleri ile kullanılabilecek küme için benzersiz bir kimlik. |
| `point_count`             | number  | Kümenin içerdiği nokta sayısı.  |
| `point_count_abbreviated` | string  | Uzunsa `point_count` değeri kısaltan bir dize. (örneğin, 4.000 4K olur)  |

Sınıf, `DataSource` `cluster_id`bir küme hakkında ek bilgilere erişmek için aşağıdaki yardımcı işlevine sahiptir.

| Yöntem | Dönüş türü | Açıklama |
|--------|-------------|-------------|
| `getClusterChildren(clusterId: number)` | Söz&lt;&lt;Dizisi&lt;Özellik&gt; \| Geometrisi, herhangi bir Şekil&gt;&gt; | Verilen kümenin alt larını bir sonraki yakınlaştırma düzeyinde alır. Bu çocuklar şekiller ve alt kümelerin bir birleşimi olabilir. Alt kümeler Clustered Properties eşleşen özelliklere sahip özellikler olacaktır. |
| `getClusterExpansionZoom(clusterId: number)` | Söz&lt;numarası&gt; | Kümenin genişlemeye başlayacağı veya parçalanacağı bir yakınlaştırma düzeyi hesaplar. |
| `getClusterLeaves(clusterId: number, limit: number, offset: number)` | Söz&lt;&lt;Dizisi&lt;Özellik&gt; \| Geometrisi, herhangi bir Şekil&gt;&gt; | Kümedeki tüm noktaları alır. Noktaların `limit` bir alt kümesini döndürmek için `offset` ayarlayın ve noktaları n için sayfayı kullanın. |

Kümelenmiş verileri haritaüzerinde işlerken, genellikle iki veya daha fazla katman kullanmak en iyisidir. Aşağıdaki örnekte üç katman kullanır. Kümelerin boyutuna göre ölçekli renkli daireler çizmek için bir kabarcık katmanı. Küme boyutunu metin olarak işlemek için bir sembol katmanı. Ve, kümelenmemiş noktaları işlemek için ikinci bir sembol katmanı kullanır. Kümelenmiş verileri işlemenin başka birçok yolu vardır. Daha fazla bilgi için [Küme noktası veri](clustering-point-data-web-sdk.md) belgelerine bakın.

Azure Haritalar haritasının içindeki `importDataFromUrl` sınıftaki `DataSource` işlevi kullanarak GeoJSON verilerini doğrudan içe aktarın.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource(null, {
                    //Tell the data source to cluster point data.
                    cluster: true
                });
                map.sources.add(datasource);

                //Create layers for rendering clusters, their counts and unclustered points and add the layers to the map.
                map.layers.add([
                    //Create a bubble layer for rendering clustered data points.
                    new atlas.layer.BubbleLayer(datasource, null, {
                        //Scale the size of the clustered bubble based on the number of points inthe cluster.
                        radius: [
                            'step',
                            ['get', 'point_count'],
                            20,         //Default of 20 pixel radius.
                            100, 30,    //If point_count >= 100, radius is 30 pixels.
                            750, 40     //If point_count >= 750, radius is 40 pixels.
                        ],

                        //Change the color of the cluster based on the value on the point_cluster property of the cluster.
                        color: [
                            'step',
                            ['get', 'point_count'],
                            'lime',            //Default to lime green. 
                            100, 'yellow',     //If the point_count >= 100, color is yellow.
                            750, 'red'         //If the point_count >= 750, color is red.
                        ],
                        strokeWidth: 0,
                        filter: ['has', 'point_count'] //Only rendered data points which have a point_count property, which clusters do.
                    }),

                    //Create a symbol layer to render the count of locations in a cluster.
                    new atlas.layer.SymbolLayer(datasource, null, {
                        iconOptions: {
                            image: 'none' //Hide the icon image.
                        },
                        textOptions: {
                            textField: ['get', 'point_count_abbreviated'],
                            offset: [0, 0.4]
                        }
                    }),

                    //Create a layer to render the individual locations.
                    new atlas.layer.SymbolLayer(datasource, null, {
                        filter: ['!', ['has', 'point_count']] //Filter out clustered points from this layer.
                    })
                ]);

                //Retrieve a GeoJSON data set and add it to the data source. 
                datasource.importDataFromUrl(earthquakeFeed);
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Haritalar kümeleme](media/migrate-google-maps-web-app/azure-maps-clustering.png)</center>

**Ek kaynaklar:**

- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="add-a-heat-map"></a>Isı haritası ekleme

Nokta yoğunluğu haritaları olarak da bilinen ısı haritaları, bir veri görselleştirme türüdür. Bir renk aralığı kullanarak verilerin yoğunluğunu temsil etmek için kullanılırlar. Ve genellikle veri "sıcak noktalar" bir harita üzerinde göstermek için kullanılır. Isı haritaları büyük nokta veri kümeleri işlemek için harika bir yoldur.

Aşağıdaki örnekler, usgs, son bir ay içinde tüm depremlerin bir GeoJSON besleme yük ve ağırlıklı bir ısı haritası olarak onları hale getirir. Özellik `"mag"` ağırlık olarak kullanılır.

**Önce: Google Haritalar**

Isı eşlemi oluşturmak için, API `&libraries=visualization` komut dosyası URL'sine ekleyerek "görselleştirme" kitaplığını yükleyin. Google Haritalar'daki ısı haritası katmanı GeoJSON verilerini doğrudan desteklemez. İlk olarak, verileri indirin ve ağırlıklı veri noktaları bir dizi dönüştürün:

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson';

        function initMap() {

            var map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2,
                mapTypeId: 'satellite'
            });

            //Download the GeoJSON data.
            fetch(url).then(function (response) {
                return response.json();
            }).then(function (res) {
                var points = getDataPoints(res);

                var heatmap = new google.maps.visualization.HeatmapLayer({
                    data: points
                });
                heatmap.setMap(map);
            });
        }

        function getDataPoints(geojson, weightProp) {
            var points = [];

            for (var i = 0; i < geojson.features.length; i++) {
                var f = geojson.features[i];

                if (f.geometry.type === 'Point') {
                    points.push({
                        location: new google.maps.LatLng(f.geometry.coordinates[1], f.geometry.coordinates[0]),
                        weight: f.properties[weightProp]
                    });
                }
            }

            return points;
        } 
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]&libraries=visualization" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Google Haritalar ısı haritası](media/migrate-google-maps-web-app/google-maps-heatmap.png)</center>

**Sonra: Azure Haritalar**

GeoJSON verilerini bir veri kaynağına yükleyin ve veri kaynağını Bir ısı haritası katmanına bağlayın. Ağırlık için kullanılacak özellik bir ifade kullanılarak `weight` seçeneğine geçirilebilir. Sınıfdaki `importDataFromUrl` işlevi kullanarak GeoJSON verilerini doğrudan `DataSource` Azure Haritalar'a aktarın.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,
                style: 'satellite',

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Load the earthquake data.
                datasource.importDataFromUrl(earthquakeFeed);

                //Create a layer to render the data points as a heat map.
                map.layers.add(new atlas.layer.HeatMapLayer(datasource, null, {
                    weight: ['get', 'mag'],
                    intensity: 0.005,
                    opacity: 0.65,
                    radius: 10
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Haritalar ısı haritası](media/migrate-google-maps-web-app/azure-maps-heatmap.png)</center>

**Ek kaynaklar:**

- [Isı haritası katmanı ekleme](map-add-heat-map-layer.md)
- [Isı haritası katman sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.heatmaplayer?view=azure-iot-typescript-latest)
- [Isı haritası katman seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="overlay-a-tile-layer"></a>Döşeme katmanını kaplama

Azure Haritalar'daki döşeme katmanları, Google Haritalar'daki Resim kaplamaları olarak bilinir. Döşeme katmanları, haritalar döşeme sistemiyle uyumlu küçük karo görüntülere ayrılmış büyük görüntüleri kaplamanızı sağlar. Bu yaklaşım, büyük görüntüleri veya büyük veri kümelerini kaplamak için yaygın olarak kullanılan bir yaklaşımdır.

Aşağıdaki örnekler Iowa State Üniversitesi Iowa Çevre Mesonet bir hava radar kiremit tabakası bindirme.

**Önce: Google Haritalar**

Google Haritalar'da, `google.maps.ImageMapType` sınıf kullanılarak döşeme katmanları oluşturulabilir.

```javascript
map.overlayMapTypes.insertAt(0, new google.maps.ImageMapType({
    getTileUrl: function (coord, zoom) {
        return "https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/" + zoom + "/" + coord.x + "/" + coord.y;
    },
    tileSize: new google.maps.Size(256, 256),
    opacity: 0.8
}));
```

<center>

![Google Haritalar döşeme katmanı](media/migrate-google-maps-web-app/google-maps-tile-layer.png)</center>

**Sonra: Azure Haritalar**

Haritaya diğer katmanlar gibi benzer bir döşeme katmanı ekleyin. x, y, yakınlaştırma yer tutucularında bulunan biçimlendirilmiş bir URL kullanın; `{x}`, `{y}` `{z}` , katmana fayanslara nerede erişeceğini söylemek için. Azure Haritalar döşeme katmanları `{quadkey}` `{bbox-epsg-3857}`da `{subdomain}` destek , ve yer tutucuları.

> [!TIP]
> Azure Haritalar'da katmanlar, temel harita katmanları da dahil olmak üzere diğer katmanların altında kolayca işlenebilir. Genellikle, okunması kolay olacak şekilde harita etiketlerinin altında kiremit katmanları işlemek için tercih edilir. Yöntem, `map.layers.add` yeni katmanı eklemek için katmanın kimliği olan ikinci bir parametre alır. Harita etiketlerinin altına bir döşeme katmanı eklemek için şu kodu kullanın:`map.layers.add(myTileLayer, "labels");`

```javascript
//Create a tile layer and add it to the map below the label layer.
map.layers.add(new atlas.layer.TileLayer({
    tileUrl: 'https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png',
    opacity: 0.8,
    tileSize: 256
}), 'labels');
```

<center>

![Azure Haritalar döşeme katmanı](media/migrate-google-maps-web-app/azure-maps-tile-layer.png)</center>

> [!TIP]
> Döşeme istekleri harita `transformRequest` seçeneği kullanılarak yakalanabilir. Bu, istenirse üstbilgileri değiştirmenize veya üstbilgi eklemenize olanak sağlar.

**Ek kaynaklar:**

- [Kutucuk katmanları ekleme](map-add-tile-layer.md)
- [Kiremit katmanı sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)
- [Döşeme katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest)

### <a name="show-traffic"></a>Trafiği gösterme

Trafik verileri hem Azure hem de Google haritalarının üzerine işlenebilir.

**Önce: Google Haritalar**

Trafik katmanını kullanarak haritadaki trafik verilerini yerle bir edin.

```javascript
var trafficLayer = new google.maps.TrafficLayer();
trafficLayer.setMap(map);
```

<center>

![Google Haritalar trafiği](media/migrate-google-maps-web-app/google-maps-traffic.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar, trafiği görüntülemek için birkaç farklı seçenek sunar. Yol kapatmalar ve kazalar gibi trafik olaylarını haritada simgeler olarak görüntüleyin. Haritaüzerinde yer kaplama trafik akışı ve renk kodlu yollar. Renkler, normal beklenen gecikmeye veya mutlak gecikmeye göre deftere nakledilen hız sınırına göre değiştirilebilir. Azure Haritalar'daki olay verileri her dakika güncellenir ve akış verileri her iki dakikada bir güncellenir.

Seçenekler için `setTraffic` istenen değerleri atayın.

```javascript
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

<center>

![Azure Haritalar trafiği](media/migrate-google-maps-web-app/azure-maps-traffic.png)</center>

Azure Haritalar'daki trafik simgelerinden birine tıklarsanız, açılır pencerede ek bilgiler görüntülenir.

<center>

![Azure Haritalar trafik olayı](media/migrate-google-maps-web-app/azure-maps-traffic-incident.png)</center>

**Ek kaynaklar:**

- [Haritada trafiği göster](map-show-traffic.md)
- [Trafik bindirme seçenekleri](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Traffic%20Overlay%20Options)

### <a name="add-a-ground-overlay"></a>Zemin kaplaması ekleme

Hem Azure hem de Google haritaları, haritadaki coğrafi başvurulanmış görüntülerin yerle bir edilmesine destek olur. Coğrafi referanslı görüntüler, haritayı kaydırıp yakınlaştırdıkça hareket ettirin ve ölçeklendirin. Google Haritalar'da, coğrafi referanslı görüntüler yer kaplamaları olarak bilinirken, Azure Haritalar'da görüntü katmanları olarak adlandırılırlar. Onlar bina kat planları için harika, eski haritalar, ya da bir drone görüntüleri örtünme.

**Önce: Google Haritalar**

Bindirmek istediğiniz resme URL'yi ve görüntüyü haritaya bağlamak için sınırlayıcı bir kutu belirtin. Bu örnek, [1922'deki Newark New Jersey](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) haritasının haritasını haritaya bindirmektedir.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map, historicalOverlay;

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(40.740, -74.18),
                zoom: 12
            });

            var imageBounds = {
                north: 40.773941,
                south: 40.712216,
                east: -74.12544,
                west: -74.22655
            };

            historicalOverlay = new google.maps.GroundOverlay(
                'https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg',
                imageBounds);
            historicalOverlay.setMap(map);
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id="myMap" style="position:relative;width:600px;height:400px;"></div>
</body>
</html>
```

Bu kodu bir tarayıcıda çalıştırmak, aşağıdaki resme benzeyen bir harita görüntüler:

<center>

![Google Haritalar resim kaplaması](media/migrate-google-maps-web-app/google-maps-image-overlay.png)</center>

**Sonra: Azure Haritalar**

Coğrafi `atlas.layer.ImageLayer` başvurulan görüntüleri kaplamak için sınıfı kullanın. Bu sınıf, görüntünün URL'si ve görüntünün dört köşesi için bir dizi koordinat gerektirir. Görüntü aynı etki alanında barındırılmalı veya CO'lar etkinleştirilmiş olmalıdır.

> [!TIP]
> Yalnızca kuzey, güney, doğu, batı ve dönüş bilgileriniz varsa ve görüntünün her köşesi için koordinatlarınız yoksa, statik [`atlas.layer.ImageLayer.getCoordinatesFromEdges`](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest#getcoordinatesfromedges-number--number--number--number--number-) yöntemi kullanabilirsiniz.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-74.18, 40.740],
                zoom: 12,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create an image layer and add it to the map.
                map.layers.add(new atlas.layer.ImageLayer({
                    url: 'newark_nj_1922.jpg',
                    coordinates: [
                        [-74.22655, 40.773941], //Top Left Corner
                        [-74.12544, 40.773941], //Top Right Corner
                        [-74.12544, 40.712216], //Bottom Right Corner
                        [-74.22655, 40.712216]  //Bottom Left Corner
                    ]
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Haritalar görüntü kaplaması](media/migrate-google-maps-web-app/azure-maps-image-overlay.png)</center>

**Ek kaynaklar:**

- [Resme katman ekleme](map-add-image-layer.md)
- [Resim katmanı sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

## <a name="add-kml-to-the-map"></a>Haritaya KML ekleme

Hem Azure hem de Google haritaları Haritaüzerinde KML, KMZ ve GeoRSS verilerini içe aktarabilir ve işleyebilir. Azure Haritalar ayrıca GPX, GML, uzamsal CSV dosyaları, GeoJSON, Tanınmış Metin (WKT), Web Haritalama Hizmetleri (WMS), Web Haritalama Döşeme hizmetleri (WMTS) ve Web Özellik Hizmetlerini (WFS) destekler. Azure Haritalar dosyaları yerel olarak belleğe okur ve çoğu durumda çok daha büyük KML dosyalarını işleyebilir. 

**Önce: Google Haritalar**


```javascript
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map, historicalOverlay;

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(0, 0),
                zoom: 1
            });

             var layer = new google.maps.KmlLayer({
              url: 'https://googlearchive.github.io/js-v2-samples/ggeoxml/cta.kml',
              map: map
            });
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id="myMap" style="position:relative;width:600px;height:400px;"></div>
</body>
</html>
```

Bu kodu bir tarayıcıda çalıştırmak, aşağıdaki resme benzeyen bir harita görüntüler:

<center>

![Google Haritalar resim kaplaması](media/migrate-google-maps-web-app/google-maps-kml.png)</center>

**Sonra: Azure Haritalar**

Azure Haritalar'da GeoJSON, web SDK'da kullanılan ana veri biçimidir, ek uzamsal veri biçimleri [mekansal IO modülü](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/)kullanılarak kolayca entegre edilebilir. Bu modül, uzamsal verileri okumak ve yazmak için işlevlere sahiptir ve aynı zamanda bu uzamsal veri biçimlerinin herhangi birinden kolayca veri işleyebilir basit bir veri katmanı içerir. Uzamsal bir veri dosyasındaki verileri okumak için, bir URL'de veya `atlas.io.read` işleve dize veya blob olarak ham veri geçmenız yeterlidir. Bu, dosyadaki daha sonra eşe eklenebilir tüm ayrıştırılmış verileri döndürecektir. KML, çok daha fazla şekillendirme bilgisi içerdiğinden, çoğu uzamsal veri biçiminden biraz daha karmaşıktır. Sınıf `SpatialDataLayer` bu stillerin çoğunluğunu oluşturmayı destekler, ancak simgelerin görüntülerinin özellik verilerini yüklemeden önce haritaya yüklenmesi gerekir ve yer kaplamaları ayrı olarak haritaya katman olarak eklenmeli. Verileri bir URL üzerinden yüklerken, KOD'lar etkin bir bitiş noktasında barındırılmalı veya bir proxy hizmeti okuma işlevine seçenek olarak geçirilmelidir. 

```javascript
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <!-- Add reference to the Azure Maps Spatial IO module. -->
    <script src="https://atlas.microsoft.com/sdk/javascript/spatial/0/atlas-spatial.js"></script>

    <script type='text/javascript'>
        var map, datasource, layer;

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                view: 'Auto',

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {
            
                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Add a simple data layer for rendering the data.
                layer = new atlas.layer.SimpleDataLayer(datasource);
                map.layers.add(layer);

                //Read a KML file from a URL or pass in a raw KML string.
                atlas.io.read('https://googlearchive.github.io/js-v2-samples/ggeoxml/cta.kml').then(async r => {
                    if (r) {

                        //Check to see if there are any icons in the data set that need to be loaded into the map resources.
                        if (r.icons) {
                            //For each icon image, create a promise to add it to the map, then run the promises in parrallel.
                            var imagePromises = [];

                            //The keys are the names of each icon image.
                            var keys = Object.keys(r.icons);

                            if (keys.length !== 0) {
                                keys.forEach(function (key) {
                                    imagePromises.push(map.imageSprite.add(key, r.icons[key]));
                                });

                                await Promise.all(imagePromises);
                            }
                        }

                        //Load all features.
                        if (r.features && r.features.length > 0) {
                            datasource.add(r.features);
                        }

                        //Load all ground overlays.
                        if (r.groundOverlays && r.groundOverlays.length > 0) {
                            map.layers.add(r.groundOverlays);
                        }

                        //If bounding box information is known for data, set the map view to it.
                        if (r.bbox) {
                            map.setCamera({ bounds: r.bbox, padding: 50 });
                        }
                    }
                });
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Azure Haritalar görüntü kaplaması](media/migrate-google-maps-web-app/azure-maps-kml.png)</center>

**Ek kaynaklar:**

- [atlas.io.oku fonksiyonu](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io?view=azure-maps-typescript-latest#read-string---arraybuffer---blob--spatialdatareadoptions-)
- [SimpleDataLayer](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.layer.simpledatalayer)
- [SimpleDataLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.simpledatalayeroptions)

## <a name="additional-code-samples"></a>Ek kod örnekleri

Google Haritalar geçişiyle ilgili bazı ek kod örnekleri şunlardır:

- [Çizim araçları](map-add-drawing-toolbar.md)
- [Haritayı İki Parmak Kaydırma ile Sınırla](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Limit%20Map%20to%20Two%20Finger%20Panning)
- [Kaydırma Tekerleği Yakınlaştırmayı Sınırla](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Limit%20Scroll%20Wheel%20Zoom)
- [Tam Ekran Denetimi Oluşturma](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Create%20a%20Fullscreen%20Control)

**Hizmetleri:**

- [Azure Haritalar hizmetleri modüllerini kullanma](how-to-use-services-module.md)
- [İlgi noktası arama](map-search-location.md)
- [Koordinattan bilgi alın (ters coğrafi kod)](map-get-information-from-coordinate.md)
- [A'dan B'ye yönleri gösterme](map-route.md)
- [JQuery UI ile Otomatik Öner'i ara](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Search%20Autosuggest%20and%20JQuery%20UI)

## <a name="google-maps-v3-to-azure-maps-web-sdk-class-mapping"></a>Google Maps V3 ' den Azure Haritalar Web SDK sınıf eşlemesine

Aşağıdaki ek, Google Haritalar V3'te ve eşdeğer Azure Haritalar Web SDK'sında yaygın olarak kullanılan sınıfların çapraz başvurularını sağlar.

### <a name="core-classes"></a>Temel Sınıflar

| Google Haritalar   | Azure Haritalar  |
|---------------|-------------|
| `google.maps.Map` | [Atlas. Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)  |
| `google.maps.InfoWindow` | [Atlas. Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)  |
| `google.maps.InfoWindowOptions` | [Atlas. PopupOptions](https://docs.microsoft.com/) |
| `google.maps.LatLng`  | [atlas.data.Pozisyon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest)  |
| `google.maps.LatLngBounds` | [atlas.data.BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) |
| `google.maps.MapOptions`  | [Atlas. Kamera Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraoptions?view=azure-iot-typescript-latest)<br/>[Atlas. CameraBoundsSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraboundsoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Servis Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.serviceoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Stil Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Kullanıcı EtkileşimSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.userinteractionoptions?view=azure-iot-typescript-latest) |
| `google.maps.Point`  | [Atlas. Piksel](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.pixel?view=azure-iot-typescript-latest)   |

## <a name="overlay-classes"></a>Bindirme Sınıfları

| Google Haritalar  | Azure Haritalar  |
|--------------|-------------|
| `google.maps.Marker` | [Atlas. HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)<br/>[atlas.data.Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest)  |
| `google.maps.MarkerOptions`  | [Atlas. HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)<br/>[atlas.layer.SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)<br/>[Atlas. SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)<br/>[Atlas. Simge Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Textoptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)<br/>[atlas.layer.BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)<br/>[Atlas. BubbleLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest) |
| `google.maps.Polygon`  | [atlas.data.Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)               |
| `google.maps.PolygonOptions` |[atlas.layer.PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)<br/> [Atlas. ÇokgenLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)<br/> [atlas.layer.LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest)<br/> [Atlas. LineLayer Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)|
| `google.maps.Polyline` | [atlas.data.LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest)         |
| `google.maps.PolylineOptions` | [atlas.layer.LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-maps-typescript-latest)<br/>[Atlas. LineLayer Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-maps-typescript-latest) |
| `google.maps.Circle`  | Bkz. [Haritaya daire ekleme](map-add-shape.md#add-a-circle-to-the-map)                                     |
| `google.maps.ImageMapType`  | [Atlas. TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)         |
| `google.maps.ImageMapTypeOptions` | [Atlas. TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest) |
| `google.maps.GroundOverlay`  | [atlas.layer.ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)<br/>[Atlas. ImageLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest) |

## <a name="service-classes"></a>Hizmet Sınıfları

Azure Haritalar Web SDK,ayrı olarak yüklenebilen bir hizmet modülü içerir. Bu modül Azure Haritalar REST hizmetlerini bir web API ile sarar ve JavaScript, TypeScript ve Node.js uygulamalarında kullanılabilir.

| Google Haritalar | Azure Haritalar  |
|-------------|-------------|
| `google.maps.Geocoder` | [atlas.service.SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |
| `google.maps.GeocoderRequest`  | [Atlas. Arama Adres Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressoptions?view=azure-iot-typescript-latest)<br/>[Atlas. AramaAddressRevrseOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreverseoptions?view=azure-iot-typescript-latest)<br/>[Atlas. AramaAdresiReverseCrossStreetOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreversecrossstreetoptions?view=azure-iot-typescript-latest)<br/>[Atlas. AramaAdresYapılandırılmış Seçenekler](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressstructuredoptions?view=azure-iot-typescript-latest)<br/>[Atlas. SearchalongRouteOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchalongrouteoptions?view=azure-iot-typescript-latest)<br/>[Atlas. AramaFuzzyOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchfuzzyoptions?view=azure-iot-typescript-latest)<br/>[Atlas. AramaInsideGeometryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchinsidegeometryoptions?view=azure-iot-typescript-latest)<br/>[Atlas. AraNearbyOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchnearbyoptions?view=azure-iot-typescript-latest)<br/>[Atlas. SearchPOIOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoioptions?view=azure-iot-typescript-latest)<br/>[Atlas. SearchPOICategoryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoicategoryoptions?view=azure-iot-typescript-latest) |
| `google.maps.DirectionsService`  | [atlas.service.RouteUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest)  |
| `google.maps.DirectionsRequest`  | [Atlas. RotaYönlendirme Seçeneklerini Hesapla](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.calculateroutedirectionsoptions?view=azure-iot-typescript-latest) |
| `google.maps.places.PlacesService` | [atlas.service.SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |

## <a name="libraries"></a>Kitaplıklar

Kitaplıklar haritaya ek işlevsellik ekler. Bu kitaplıkların çoğu Azure Haritalar'ın temel SDK'sındadır. İşte bu Google Haritalar kitaplıkları yerine kullanılacak bazı eşdeğer sınıflar

| Google Haritalar           | Azure Haritalar   |
|-----------------------|--------------|
| Çizim kitaplığı       | [Çizim araçları modülü](set-drawing-options.md) |
| Geometri kütüphanesi      | [atlas.matematik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)   |
| Görselleştirme kitaplığı | [Isı haritası katmanı](map-add-heat-map-layer.md) |

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar Web SDK hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Harita denetimini kullanma](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Hizmetler modülü nasıl kullanılır?](how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Çizim araçları modülü nasıl kullanılır?](set-drawing-options.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)

