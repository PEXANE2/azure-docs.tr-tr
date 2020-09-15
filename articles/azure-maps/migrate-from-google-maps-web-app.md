---
title: "Öğretici: Google Maps 'tan bir Web uygulaması geçirme | Microsoft Azure haritaları"
description: Bir Web uygulamasını Google Maps 'tan Microsoft Azure Maps 'a geçirme.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: devx-track-javascript
ms.openlocfilehash: bc5f10e34b929110763b53fe1016334ce9bfddd6
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090763"
---
# <a name="migrate-a-web-app-from-google-maps"></a>Google Maps 'tan bir Web uygulaması geçirme

Google Maps kullanan çoğu Web uygulaması Google Maps v3 JavaScript SDK 'sını kullanıyor. Azure Haritalar Web SDK 'Sı, geçirilecek Azure tabanlı uygun SDK 'dir. Azure Haritalar Web SDK 'Sı, etkileşimli haritaları kendi içerikleriyle ve Imagery ile özelleştirmenizi sağlar. Uygulamanızı hem Web 'de hem de mobil uygulamalarda çalıştırabilirsiniz. Bu denetimde büyük veri kümelerini yüksek performansla oluşturmanızı sağlayan WebGL bileşeni kullanılmaktadır. JavaScript veya TypeScript kullanarak bu SDK ile geliştirin.

Var olan bir Web uygulamasını geçiriyorsanız, bir açık kaynak eşleme denetim kitaplığı kullanıp kullanmın olup olmadığını kontrol edin. Açık kaynak eşleme denetim kitaplığı örnekleri şunlardır: Cesium, leaflet ve Openkatmanları. Bir açık kaynak eşleme denetim kitaplığı kullandığından ve Azure Maps web SDK 'sını kullanmak istemediğinizde bile uygulamanızı geçirebilirsiniz. Böyle bir durumda, uygulamanızı Azure Maps kutucuk Hizmetleri 'ne ([yol kutucukları](https://docs.microsoft.com/rest/api/maps/render/getmaptile) \| [uydu kutucukları](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile)) bağlayın. Aşağıda, yaygın olarak kullanılan bazı açık kaynaklı harita denetim kitaplıklarında Azure maps kullanma hakkında ayrıntılı bilgi verilmiştir.

- Cesium-Web için 3B harita denetimi. [Kod örneği](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20Cesium%20JS) \| [Belgeler](https://cesiumjs.org/)
- Leaflet – Web için hafif 2B harita denetimi. [Kod örneği](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Azure%20Maps%20Raster%20Tiles%20in%20Leaflet%20JS) \| [Belgeler](https://leafletjs.com/)
- Openkatmanlar-Web için projeksiyonları destekleyen bir 2B harita denetimi. [Kod örneği](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20OpenLayers) \| [Belgeler](https://openlayers.org/)

Bir JavaScript çerçevesi kullanılarak geliştirilirken, aşağıdaki açık kaynaklı projelerden biri yararlı olabilir:

- [ng-Azure-Maps](https://github.com/arnaudleclerc/ng-azure-maps) -Azure haritaları etrafında angular 10 sarmalayıcı.
- [AzureMapsControl. Components](https://github.com/arnaudleclerc/AzureMapsControl.Components) -bir Azure Maps Blazor bileşeni.
- [Azure Maps 'e tepki verme bileşeni](https://github.com/WiredSolutions/react-azure-maps) -Azure Maps denetimi için bir tepki düzeyi.
- [Vue Azure Maps](https://github.com/rickyruiz/vue-azure-maps) -Vue uygulaması Için bir Azure Maps bileşeni.

## <a name="key-features-support"></a>Anahtar özellikleri desteği

Tabloda, Google Maps v3 JavaScript SDK 'sindeki temel API özellikleri ve Azure Maps web SDK 'sında desteklenen API özelliği listelenmiştir.

| Google Haritalar özelliği     | Azure Maps web SDK desteği |
|-------------------------|:--------------------------:|
| İşaretleyiciler                 | ✓                          |
| İşaretleyici Kümelemesi       | ✓                          |
| Polylines & çokgenler    | ✓                          |
| Veri katmanları             | ✓                          |
| Zemin Yerpaylaşımları         | ✓                          |
| Isı haritaları               | ✓                          |
| Döşeme katmanları             | ✓                          |
| KML katmanı               | ✓                          |
| Çizim araçları           | ✓                          |
| Geocoder hizmeti        | ✓                          |
| Yönergeler hizmeti      | ✓                          |
| Uzaklık matrisi hizmeti | ✓                          |
| Yükseltme hizmeti       | Planlandı                     |

## <a name="notable-differences-in-the-web-sdks"></a>Web SDK 'lerinde önemli farklılıklar

Aşağıda, Google Maps ve Azure Maps web SDK 'Ları arasındaki bazı önemli farklılıklar aşağıda verilmiştir:

- Azure Maps web SDK 'sına erişmek için barındırılan bir uç nokta sağlamaya ek olarak, bir NPM paketi mevcuttur. Web SDK paketini uygulamalara ekleyin. Daha fazla bilgi için bu [belgelere](how-to-use-map-control.md)bakın. Bu paket TypeScript tanımlarını da içerir.
- Önce Azure haritalar 'da Map sınıfının bir örneğini oluşturmanız gerekir. `ready` `load` Harita veya etkinliğin, eşleme ile programlı bir şekilde etkileşimde bulunmak için başlamasını bekleyin. Bu sipariş, tüm harita kaynaklarının yüklenmiş ve erişilmeye hazırsa emin olmanızı sağlar.
- Her iki platformda de temel haritalar için benzer bir döşeme sistemi kullanılır. Google Maps 'teki Kutucuklar, Dimension 'da 256 pikseldir; Ancak, Azure haritalar 'daki kutucuklar boyut olarak 512 pikseldir. Azure haritalar 'da Google Maps olarak aynı harita görünümünü almak için, Google Maps 'ın yakınlaştırma düzeyini Azure haritalar 'da bir sayı kadar çıkarın.
- Google Maps 'taki koordinatlar `latitude,longitude` , Azure haritalar tarafından kullanıldığı sırada olarak adlandırılır `longitude,latitude` . Azure haritalar biçimi standart ile hizalanır `[x, y]` ve bu, en fazla GIS platformundan gelir.
- Azure Haritalar Web SDK 'sindeki şekiller GeoJSON şemasına dayalıdır. Yardımcı sınıflar, [ *Atlas. Data* ad alanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data)aracılığıyla sunulur. Ayrıca Atlas de vardır [*. Şekil*](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape) sınıfı. Bu sınıfı, verileri bağlanabilir şekilde güncelleştirmeyi ve bakımını kolaylaştırmak için GeoJSON nesnelerini kaydırmak üzere kullanın.
- Azure haritalarındaki koordinatlar konum nesneleri olarak tanımlanır. Bir koordinat, biçimde sayı dizisi olarak belirtilir `[longitude,latitude]` . Ya da yeni Atlas. Data. Position (Boylam, Enlem) kullanılarak belirtilir.
    > [!TIP]
    > Konum sınıfı, "enlem, Boylam" biçiminde olan koordinatları içeri aktarmaya yönelik statik bir yardımcı yönteme sahiptir. [Atlas. Data. Position. fromLatLng](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position) yöntemi genellikle `new google.maps.LatLng` Google Maps kodundaki yöntemiyle değiştirilebilir.
- Haritaya eklenen her bir şekil üzerinde stil bilgilerini belirtmek yerine, Azure Maps stilleri verilerden ayırır. Veriler bir veri kaynağında depolanır ve işleme katmanlarına bağlanır. Azure haritalar kodu, verileri işlemek için veri kaynaklarını kullanır. Bu yaklaşım, gelişmiş performans avantajı sağlar. Ayrıca, birçok katman, iş mantığının katman stili seçeneklerine eklenebileceği veri tabanlı stillendirme desteği sağlar. Bu destek, tek tek şekillerin, şekil içinde tanımlanan özelliklere göre bir katman içinde nasıl işleneceğini değiştirir.

## <a name="web-sdk-side-by-side-examples"></a>Web SDK yan yana örnekleri

Bu koleksiyonda her platform için kod örnekleri bulunur ve her örnek ortak kullanım durumunu içerir. Web uygulamanızı Google Maps v3 JavaScript SDK 'dan Azure Maps web SDK 'sına geçirmenize yardımcı olmak üzere tasarlanmıştır. Web uygulamalarıyla ilgili kod örnekleri JavaScript 'te verilmiştir. Ancak Azure Maps, bir [NPM modülü](how-to-use-map-control.md)aracılığıyla ek bir seçenek olarak TypeScript tanımları da sağlar.


**Konu başlıkları**

- [Harita yükleme](#load-a-map)
- [Haritayı yerelleştirme](#localizing-the-map)
- [Harita görünümü ayarlanıyor](#setting-the-map-view)
- [İşaretleyici ekleme](#adding-a-marker)
- [Özel işaretleyici ekleme](#adding-a-custom-marker)
- [Çoklu çizgi ekleme](#adding-a-polyline)
- [Çokgen ekleme](#adding-a-polygon)
- [Bilgi penceresi görüntüle](#display-an-info-window)
- [GeoJSON dosyasını içeri aktar](#import-a-geojson-file)- 
- [İşaretleyici Kümelemesi](#marker-clustering)
- [Isı haritası ekleme](#add-a-heat-map)
- [Döşeme katmanını kaplama](#overlay-a-tile-layer)
- [Trafik verilerini gösterme](#show-traffic-data)
- [Zemin kaplama ekleme](#add-a-ground-overlay)
- [KML verilerini haritaya ekleme](#add-kml-data-to-the-map)


### <a name="load-a-map"></a>Harita yükleme

Her iki SDK da bir eşlemeyi yüklemek için aynı adımlara sahiptir:

- Map SDK 'sına bir başvuru ekleyin.
- `div`Sayfanın gövdesine bir etiket ekleyin, bu, eşleme için bir yer tutucu görevi görür.
- Sayfa yüklendiğinde çağrılan bir JavaScript işlevi oluşturun.
- Karşılık gelen eşleme sınıfının bir örneğini oluşturun.

**Bazı önemli farklılıklar**

- Google Maps, API 'nin betik başvurusunda bir hesap anahtarının belirtilmesini gerektirir. Azure haritalar için kimlik doğrulama kimlik bilgileri, Map sınıfının seçenekleri olarak belirtilir. Bu kimlik bilgisi bir abonelik anahtarı veya Azure Active Directory bilgileri olabilir.
- Google Maps API 'nin betik başvurusunda bir geri çağırma işlevi kabul eder, bu, eşlemeyi yüklemek için bir başlatma işlevi çağırmak için kullanılır. Azure haritalar ile sayfanın OnLoad olayı kullanılmalıdır.
- `div`Haritanın işlenebileceği öğeye başvurulduğunda, `Map` Azure Maps 'taki sınıf yalnızca `id` Google Maps bir nesne gerektirdiğinde değeri gerektirir `HTMLElement` .
- Azure haritalar 'daki Koordinatlar, biçimde bir basit sayı dizisi olarak belirtime konum nesneleri olarak tanımlanır `[longitude, latitude]` .
- Azure haritalar 'daki yakınlaştırma düzeyi, Google Maps 'taki yakınlaştırma düzeyinden bir düzey daha düşüktür. Bu tutarsızlık, iki platformun döşeme sistem boyutlarındaki farkın farkından kaynaklanır.
- Azure haritalar harita tuvaline hiçbir gezinti denetimi eklemez. Bu nedenle, varsayılan olarak haritada yakınlaştırma düğmeleri ve harita stili düğmeleri yoktur. Ancak harita stili seçici, yakınlaştırma düğmeleri, pusula ya da döndürme denetimi ve bir aralıklı denetim eklemek için denetim seçenekleri mevcuttur.
- Harita örneğinin olayını izlemek için Azure Maps 'a bir olay işleyicisi eklenir `ready` . Bu olay, eşleme WebGL bağlamını ve gerekli tüm kaynakları yüklemeyi tamamladığında harekete geçmeyecektir. Bu olay işleyicisine haritanın yüklenmesi tamamlandıktan sonra çalıştırmak istediğiniz herhangi bir kodu ekleyin.

Aşağıdaki temel örneklerde, New York üzerinde ortalanmış bir harita yüklemek için Google haritaları kullanılmaktadır. Boylam:-73,985, Enlem: 40,747 ve eşleme, 12 ' nin yakınlaştırma düzeyidir.

**Önce: Google Maps**

Bir konumdan ortalanmış ve yakınlaştırılmış bir Google Map görüntüleme.

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

Bu kodun bir tarayıcıda çalıştırılması, aşağıdaki görüntü gibi görünen bir eşlem görüntüler:

<center>

![Basit Google haritaları](media/migrate-google-maps-web-app/simple-google-map.png)</center>

**Sonrasında: Azure Maps**

Harita stili denetimi ve yakınlaştırma düğmeleriyle birlikte Azure Maps 'ta aynı görünümle bir harita yükleyin.

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

Bu kodun bir tarayıcıda çalıştırılması, aşağıdaki görüntü gibi görünen bir eşlem görüntüler:

<center>

![Basit Azure haritaları](media/migrate-google-maps-web-app/simple-azure-maps.png)</center>

Bir Web uygulamasında Azure Maps harita denetimini ayarlama ve kullanma hakkında ayrıntılı bilgi edinmek için [buraya](how-to-use-map-control.md)tıklayın.

> [!NOTE]
> Google Maps 'tan farklı olarak, Azure Maps haritanın yüklenmesi için bir başlangıç merkezi ve yakınlaştırma düzeyi gerektirmez. Bu bilgiler eşleme yüklenirken sağlanmazsa, Azure Maps kullanıcının şehrini belirlemeyi dener. Haritayı buraya ortalayın ve yakınlaştırın.

**Ek kaynaklar:**

- Azure haritalar, [burada](map-add-controls.md)gösterildiği gibi harita görünümünü döndürme ve ele döndürme için de gezinti denetimleri sağlar.

### <a name="localizing-the-map"></a>Haritayı yerelleştirme

Hedef kitleniz birden fazla ülkeye/bölgeye yayıldığında veya farklı dillerde konuşabilmek için yerelleştirme önemlidir.

**Önce: Google Maps**

Google Maps 'ı yerelleştirmek için dil ve bölge parametreleri ekleyin.

```html
<script type="text/javascript" src=" https://maps.googleapis.com/maps/api/js?callback=initMap&key=[api_key]& language=[language_code]&region=[region_code]" async defer></script>
```

Dilde "fr-FR" olarak ayarlanmış Google Maps örneği aşağıda verilmiştir.

<center>

![Google Haritalar yerelleştirme](media/migrate-google-maps-web-app/google-maps-localization.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar haritanın dilini ve bölgesel görünümünü ayarlamanın iki farklı yolunu sağlar. İlk seçenek, bu bilgileri Global *Atlas* ad alanına eklemektir. Bu, uygulamanızdaki tüm harita denetim örneklerinin bu ayarlara göre varsayılan olarak oluşmasına neden olur. Aşağıdaki dil, dili Fransızca ("fr-FR") ve bölgesel görünüm olarak "Auto" olarak ayarlar:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

İkinci seçenek, eşleme yüklenirken bu bilgileri eşleme seçeneklerine iletmektir. Böyle:

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
> Azure haritalar ile aynı sayfada farklı dil ve bölge ayarlarıyla birden çok eşleme örneği yüklemek mümkündür. Bu ayarların, yüklendikten sonra haritada güncelleştirilmesi de mümkündür. 

Azure haritalar 'da [desteklenen dillerin](supported-languages.md) ayrıntılı bir listesini bulun.

Aşağıda dil "fr" olarak ayarlanan ve Kullanıcı bölgesinin "fr-FR" olarak ayarlandığı bir Azure Maps örneği verilmiştir.

<center>

![Azure haritalar yerelleştirme](media/migrate-google-maps-web-app/azure-maps-localization.png)</center>

### <a name="setting-the-map-view"></a>Harita görünümü ayarlanıyor

Hem Azure hem de Google Maps içindeki dinamik haritalar, programlı olarak yeni coğrafi konumlara taşınabilir. Bunu yapmak için JavaScript 'te uygun işlevleri çağırın. Örneklerde, haritada uydu havadan görüntüleme, Haritayı bir konum üzerine ortalama gösterme ve Google Maps 'ta yakınlaştırma düzeyini 15 olarak değiştirme işlemlerinin nasıl yapılacağı gösterilmektedir. Aşağıdaki konum koordinatları kullanılır: Boylam:-111,0225 ve Enlem: 35,0272.

> [!NOTE]
> Azure haritalar, daha büyük bir 512 piksellik kutucuk kullandığında, Google Maps, boyutlarda 256 piksel olan kutucukları kullanır. Bu nedenle Azure haritalar, Google Maps ile aynı harita alanını yüklemek için daha az sayıda ağ isteği gerektirir. Tile Pyramids 'in harita denetimlerinde çalışma yöntemi nedeniyle, Google Maps 'ta kullanılan yakınlaştırma düzeyini Azure haritalar kullanırken bir sayı ile çıkarmanız gerekir. Bu aritmetik işlem, Azure haritalarındaki daha büyük kutucukların Google Haritalar 'da olduğu gibi aynı harita alanını işlemesini sağlar.

**Önce: Google Maps**

Yöntemini kullanarak Google Maps harita denetimini taşıyın `setOptions` . Bu yöntem haritanın merkezini ve yakınlaştırma düzeyini belirtmenize olanak tanır.

```javascript
map.setOptions({
    mapTypeId: google.maps.MapTypeId.SATELLITE,
    center: new google.maps.LatLng(35.0272, -111.0225),
    zoom: 15
});
```

<center>

![Google Maps kümesi görünümü](media/migrate-google-maps-web-app/google-maps-set-view.png)</center>

**Sonrasında: Azure Maps**

Azure Haritalar ' da yöntemi kullanarak harita konumunu değiştirin `setCamera` ve yöntemi kullanarak harita stilini değiştirin `setStyle` . Azure haritalar 'daki koordinatlar "boylam, enlem" biçimindedir ve yakınlaştırma düzeyi değeri bir ile çıkarılır.

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

![Azure haritalar kümesi görünümü](media/migrate-google-maps-web-app/azure-maps-set-view.jpeg)</center>

**Ek kaynaklar:**

- [Harita stili seçme](choose-map-style.md)
- [Desteklenen eşleme stilleri](supported-map-styles.md)

### <a name="adding-a-marker"></a>İşaretleyici ekleme

Azure haritalar 'da, bir noktada, bu noktada nokta verilerinin işlenebileceği birçok yol vardır:

- **HTML işaretçileri** : geleneksel DOM öğelerini kullanarak noktaları işler. HTML Işaretçileri sürüklemeyi destekler.
- **Sembol katmanı** : noktaları WebGL bağlamı içinde bir simge veya metin ile işler.
- **Kabarcık katmanı** : noktaları haritada daireler olarak işler. Dairelerin yarıçapı, verilerdeki özelliklere göre ölçeklendirilebilir.

Web GL bağlamı içindeki sembol katmanlarını ve kabarcık katmanlarını işleme. Her iki katman de Haritada büyük küme kümelerini işleyebilir. Bu katmanlar, verilerin bir veri kaynağında depolanmasını gerektirir. Veri kaynakları ve işleme katmanları, `ready` olay tetiklendikten sonra haritaya eklenmelidir. HTML Işaretçileri, sayfada DOM öğeleri olarak işlenir ve bir veri kaynağı kullanmaz. Bir sayfada daha fazla DOM öğesi varsa, sayfa ne kadar yavaş olur. Bir haritada birkaç yüz noktasından fazlasını işliyorsa, bunun yerine işleme katmanlarından birini kullanmanız önerilir.

Bir etiket olarak bulunan 10 sayısıyla birlikte haritaya bir işaret ekleyelim. Boylam kullanın:-0,2 ve Enlem: 51,5.

**Önce: Google Maps**

Google Maps ile, sınıfı kullanarak haritaya işaretçiler ekleyin `google.maps.Marker` ve eşlemeyi seçeneklerden biri olarak belirtin.

```javascript
//Create a marker and add it to the map.
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(51.5, -0.2),
    label: '10',
    map: map
});
```

<center>

![Google Haritalar işaretçisi](media/migrate-google-maps-web-app/google-maps-marker.png)</center>

**Sonra: HTML Işaretçileri kullanarak Azure Maps**

Azure haritalar 'da, haritada bir nokta göstermek için HTML işaretçileri kullanın. HTML işaretçileri, yalnızca haritada az sayıda noktayı görüntülemesi gereken uygulamalar için önerilir. Bir HTML işaretleyicisi kullanmak için, sınıfının bir örneğini oluşturun `atlas.HtmlMarker` . Metin ve konum seçeneklerini ayarlayın ve yöntemi kullanarak işaretçiyi haritaya ekleyin `map.markers.add` .

```javascript
//Create a HTML marker and add it to the map.
map.markers.add(new atlas.HtmlMarker({
    text: '10',
    position: [-0.2, 51.5]
}));
```

<center>

![Azure haritalar HTML işaretleyicisi](media/migrate-google-maps-web-app/azure-maps-html-marker.png)</center>

**Sonra: bir sembol katmanı kullanarak Azure Maps**

Bir sembol katmanı için verileri bir veri kaynağına ekleyin. Veri kaynağını katmana iliştirin. Ayrıca, olay tetiklendikten sonra veri kaynağı ve katman haritaya eklenmelidir `ready` . Bir simgenin üzerinde benzersiz bir metin değeri işlemek için metin bilgilerinin, veri noktasının bir özelliği olarak depolanması gerekir. Özelliği `textField` katmanın seçeneğinde başvurulmalıdır. Bu yaklaşım, HTML işaretçilerini kullanmaktan biraz daha çalışır, ancak performansı daha iyi hale gelir.

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

![Azure haritalar sembol katmanı](media/migrate-google-maps-web-app/azure-maps-symbol-layer.png)</center>

**Ek kaynaklar:**

- [Veri kaynağı oluşturma](create-data-source-web-sdk.md)
- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [HTML Işaretçileri Ekle](map-add-custom-html.md)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)
- [Sembol katmanı simgesi seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions)
- [Sembol katmanı metin seçeneği](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions)
- [HTML işaret sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker)
- [HTML işaret seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions)

### <a name="adding-a-custom-marker"></a>Özel işaretleyici ekleme

Bir haritadaki noktaları temsil etmek için özel görüntüler kullanabilirsiniz. Aşağıdaki eşleme, haritada bir noktayı göstermek için özel bir görüntü kullanır. Nokta, Enlem: 51,5 ve Boylam:-0,2 ' de görüntülenir. Tutturucu, işaretin konumunu, raptiye simgesinin noktasının haritada doğru konuma göre hizalanmasını sağlayacak şekilde kaydırır.

<center>

![sarı raptiye resmi](media/migrate-google-maps-web-app/yellow-pushpin.png)<br/>
yellow-pushpin.png</center>

**Önce: Google Maps**

Resme görüntüsünü içeren bir nesne belirterek özel bir işaret oluşturun `Icon` `url` . `anchor`İğne resminin noktasını haritadaki koordinatla hizalamak için bir nokta belirtin. Google Maps 'daki tutturucu değeri görüntünün sol üst köşesine göre değişir.

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

![Google Maps özel işaretçisi](media/migrate-google-maps-web-app/google-maps-custom-marker.png)</center>

**Sonra: HTML Işaretçileri kullanarak Azure Maps**

Bir HTML imini özelleştirmek için, bir HTML `string` veya `HTMLElement` `htmlContent` işaret seçeneğine geçin. `anchor`Konum koordinatına göre işaretin göreli konumunu belirtmek için seçeneğini kullanın. Seçeneğe tanımlanmış dokuz başvuru noktasından birini atayın `anchor` . Bu tanımlı kapılar şunlardır: "Center", "top", "bottom", "left", "Right", "top-left", "top-right", "Bottom-left", "Bottom-right". İçerik varsayılan olarak HTML içeriğinin alt merkezine bağlanır. Google Maps 'tan kod geçirmeyi kolaylaştırmak için, ' ı `anchor` "üst-sol" olarak ayarlayın ve ardından `pixelOffset` Google Maps ' de kullanılan aynı uzaklığa sahip seçeneğini kullanın. Azure haritalar 'daki uzaklıklar, Google Maps 'taki uzakların ters yönünde taşınır. Bu nedenle, uzaklıkları eksi bir ile çarpın.

> [!TIP]
> `pointer-events:none`Microsoft Edge 'de varsayılan sürükleme davranışını devre dışı bırakmak için HTML içeriğine bir stil olarak ekleyin ve bu, istenmeyen bir simge görüntüler.

```javascript
map.markers.add(new atlas.HtmlMarker({
    htmlContent: '<img src="https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png" style="pointer-events: none;" />',
    anchor: 'top-left',
    pixelOffset: [-5, -30],
    position: [-0.2, 51.5]
}));
```

<center>

![Azure haritalar özel HTML işaretçisi](media/migrate-google-maps-web-app/azure-maps-custom-html-marker.png)</center>

**Sonra: bir sembol katmanı kullanarak Azure Maps**

Azure haritalar 'daki sembol katmanları, özel görüntüleri de destekler. İlk olarak, görüntüyü eşleme kaynaklarına yükleyin ve benzersiz bir KIMLIK ile atayın. Sembol katmanındaki görüntüye başvurun. `offset`Görüntüyü haritada doğru noktaya hizalamak için seçeneğini kullanın. `anchor`Simgenin konum koordinatlarına göre göreli konumunu belirtmek için seçeneğini kullanın. Dokuz tanımlı başvuru noktalarından birini kullanın. Bu noktaları şunlardır: "Center", "top", "bottom", "left", "Right", "top-left", "top-right", "Bottom-left", "Bottom-right". İçerik varsayılan olarak HTML içeriğinin alt merkezine bağlanır. Google Maps 'tan kod geçirmeyi kolaylaştırmak için, ' ı `anchor` "üst-sol" olarak ayarlayın ve ardından `offset` Google Maps ' de kullanılan aynı uzaklığa sahip seçeneğini kullanın. Azure haritalar 'daki uzaklıklar, Google Maps 'taki uzakların ters yönünde taşınır. Bu nedenle, uzaklıkları eksi bir ile çarpın.

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

![Azure haritalar özel simgesi sembol katmanı](media/migrate-google-maps-web-app/azure-maps-custom-icon-symbol-layer.png)</center>

> [!TIP]
> Gelişmiş özel noktaları işlemek için çoklu işleme katmanlarını birlikte kullanın. Örneğin, farklı renkli daireler üzerinde aynı simgeye sahip birden fazla Pushpin olmasını istediğinizi varsayalım. Her renk kaplama için bir dizi görüntü oluşturmak yerine, kabarcık katmanının üzerine bir sembol katmanı ekleyin. Pushpin 'in aynı veri kaynağına başvurması gerekir. Bu yaklaşım, farklı görüntülerin oluşturulması ve saklanması bakımından daha etkili olacaktır.

**Ek kaynaklar:**

- [Veri kaynağı oluşturma](create-data-source-web-sdk.md)
- [Sembol katmanı ekleme](map-add-pin.md)
- [HTML Işaretçileri Ekle](map-add-custom-html.md)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)
- [Sembol katmanı simgesi seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions)
- [Sembol katmanı metin seçeneği](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions)
- [HTML işaret sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker)
- [HTML işaret seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions)

### <a name="adding-a-polyline"></a>Çoklu çizgi ekleme

Harita üzerindeki bir çizgiyi veya yolu göstermek için çoklu çizgilerin çizgisini kullanın. Haritada kesikli bir çoklu çizgi oluşturalım.

**Önce: Google Maps**

Çoklu çizgi sınıfı bir seçenek kümesini kabul eder. `path`Çoklu çizgi seçeneğinde koordinat dizisini geçirin.

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

![Google Haritalar çoklu çizgi](media/migrate-google-maps-web-app/google-maps-polyline.png)</center>

**Sonrasında: Azure Maps**

Polylines, `LineString` veya nesneleri olarak adlandırılır `MultiLineString` . Bu nesneler bir veri kaynağına eklenebilir ve çizgi katmanı kullanılarak oluşturulabilir. `LineString`Bir veri kaynağına ekleyin, ardından veri kaynağını bir öğesine ekleyerek işlemek için bir öğesine ekleyin `LineLayer` .

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

![Azure haritalar çoklu çizgi](media/migrate-google-maps-web-app/azure-maps-polyline.png)</center>

**Ek kaynaklar:**

- [Haritaya satır ekleme](map-add-line-layer.md)
- [Çizgi katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="adding-a-polygon"></a>Çokgen ekleme

Azure haritalar ve Google Maps, çokgenler için benzer destek sağlar. Çokgenler, haritadaki bir alanı temsil etmek için kullanılır. Aşağıdaki örneklerde haritanın orta koordinatına göre bir üçgen oluşturan bir çokgen oluşturma gösterilmektedir.

**Önce: Google Maps**

Çokgen sınıfı bir seçenek kümesini kabul eder. Poligonun seçeneğine bir dizi koordinat geçirin `paths` .

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

![Google Haritalar Çokgen](media/migrate-google-maps-web-app/google-maps-polygon.png)</center>

**Sonrasında: Azure Maps**

`Polygon`Veri kaynağına bir veya `MultiPolygon` nesne ekleyin. Katman kullanarak nesneyi haritada işleme. Çokgen katmanını kullanarak bir çokgen alanını işleme. Ve bir çizgi katmanını kullanarak bir çokgen ana hattını işleme.

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

![Azure haritalar Çokgen](media/migrate-google-maps-web-app/azure-maps-polygon.png)</center>

**Ek kaynaklar:**

- [Haritaya Çokgen ekleme](map-add-shape.md)
- [Haritaya daire ekleme](map-add-shape.md#add-a-circle-to-the-map)
- [Çokgen katman seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions)
- [Çizgi katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="display-an-info-window"></a>Bilgi penceresi görüntüle

Bir varlık için ek bilgi, `google.maps.InfoWindow` Google Maps 'ta bir sınıf olarak haritada görüntülenebilir. Azure haritalar 'da, bu işlev sınıfı kullanılarak elde edilebilir `atlas.Popup` . Sonraki örneklerde haritaya bir işaretleyici eklenir. İşaretçiye tıklandığında bir bilgi penceresi veya bir açılan pencere görüntülenir.

**Önce: Google Maps**

Oluşturucuyu kullanarak bir bilgi penceresi örneği oluşturun `google.maps.InfoWindow` .

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

![Google Haritalar açılan menüsü](media/migrate-google-maps-web-app/google-maps-popup.png)</center>

**Sonrasında: Azure Maps**

Konum hakkındaki ek bilgileri göstermek için açılan menüyü kullanalım. `string` `HTMLElement` Açılan menü SEÇENEĞINE bir HTML veya nesne geçirin `content` . İsterseniz, açılan pencereler herhangi bir şekilden bağımsız şekilde görüntülenebilir. Bu nedenle, açılan pencereler bir `position` değer belirtilmesini gerektirir. Değeri belirtin `position` . Açılan pencereyi göstermek için `open` yöntemini çağırın ve `map` açılan pencerenin üzerinde görüntüleneceği geçirin.

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

![Azure haritalar açılan menüsü](media/migrate-google-maps-web-app/azure-maps-popup.png)</center>

> [!NOTE]
> Seçili katmanı bir işaretleyici yerine haritalar olay koduna geçirerek bir sembol, kabarcık, çizgi veya Çokgen katman ile aynı şeyi yapabilirsiniz.

**Ek kaynaklar:**

- [Açılır pencere ekleme](map-add-popup.md)
- [Medya Içeriğiyle açılan pencere](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popup%20with%20Media%20Content)
- [Şekillerdeki açılanlar](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popups%20on%20Shapes)
- [Çoklu PIN ile açılan pencereyi yeniden kullanma](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Reusing%20Popup%20with%20Multiple%20Pins)
- [Açılır Sınıf](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup)
- [Açılan seçenekler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions)

### <a name="import-a-geojson-file"></a>GeoJSON dosyasını içeri aktar

Google Maps, coğrafi JSON verilerinin sınıf aracılığıyla yüklenmesini ve dinamik olarak stillendirilmesini destekler `google.maps.Data` . Bu sınıfın işlevselliği, Azure haritalar 'ın veri odaklı stille çok daha fazlasını hizalar. Ancak, önemli bir farklılık vardır. Google Maps ile bir geri çağırma işlevi belirtirsiniz. Kullanıcı arabirimi iş parçacığında ayrı olarak işlenen her bir özelliğin stillendirilmesini sağlayan iş mantığı. Ancak Azure Maps 'ta katmanlar, veri tabanlı ifadeleri stil seçenekleri olarak belirtmeyi destekler. Bu ifadeler ayrı bir iş parçacığında işleme zamanında işlenir. Azure haritalar yaklaşımı işleme performansını geliştirir. Bu avantaj, daha büyük veri kümelerinin hızla işlenmesi gerektiğinde fark edilir.

Aşağıdaki örneklerde, USGS 'nin son yedi gününe ait tüm deprem coğrafi JSON akışı yüklenir. Deprem verileri haritada ölçeklendirilmiş daireler olarak işlenir. Her dairenin rengi ve ölçeği, `"mag"` veri kümesindeki her bir özelliğin özelliğinde depolanan her bir deprem büyüklüğü temel alır. Büyüklük beşten büyük veya buna eşitse daire kırmızı olur. Üç veya daha büyükse, beşten küçükse daire turuncu olur. Üçten küçükse daire yeşil olur. Her dairenin yarıçapı, 0,1 ile çarpıldığı büyüklük üsünü üstel olacaktır.

**Önce: Google Maps**

Yönteminde tek bir geri çağırma işlevi belirtin `map.data.setStyle` . Geri arama işlevinin içinde her bir özelliğe iş mantığı uygulayın. GeoJSON akışını `map.data.loadGeoJson` yöntemiyle yükleyin.

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

![Google Maps GeoJSON](media/migrate-google-maps-web-app/google-maps-geojson.png)</center>

**Sonrasında: Azure Maps**

GeoJSON, Azure Maps 'ta temel veri türüdür. Yöntemini kullanarak bir veri kaynağına içeri aktarın `datasource.importFromUrl` . Balon katmanını kullanın. Kabarcık katmanı, bir veri kaynağındaki özelliklerin özelliklerine göre ölçeklendirilen daireleri işlemek için işlevsellik sağlar. Bir geri çağırma işlevi olması yerine, iş mantığı bir ifadeye dönüştürülür ve stil seçeneklerine geçirilir. İfadeler, iş mantığının nasıl çalıştığını tanımlar. İfadeler, başka bir iş parçacığına geçirilebilir ve özellik verilerine göre değerlendirilir. Azure haritalar 'a her biri farklı iş mantığına sahip birden çok veri kaynağı ve katman eklenebilir. Bu özellik, haritada farklı yollarla birden çok veri kümesinin işlenmesine izin verir.

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

![Azure haritalar coğrafi JSON](media/migrate-google-maps-web-app/azure-maps-geojson.png)</center>

**Ek kaynaklar:**

- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="marker-clustering"></a>İşaretleyici Kümelemesi

Haritada çok sayıda veri noktasını görselleştirirken, noktaları birbirleriyle çakışabilir. Örtüşme, haritanın karmaşık görünmesini ve eşlemenin okunması ve kullanılması zor hale gelir. Kümeleme noktası verileri, birbirine yakın olan ve bunları haritada tek bir kümelenmiş veri noktası olarak temsil eden veri noktalarını birleştirme işlemidir. Kullanıcı haritada yakınlaştırdıktan sonra, kümeler kendi bireysel veri noktalarıyla kesilir. Küme verileri, Kullanıcı deneyimini ve eşleme performansını artıracak şekilde puntolar.

Aşağıdaki örneklerde kod, eski haftadan gelen deprem verilerinin coğrafi JSON akışını yükler ve bunu haritaya ekler. Kümeler ölçekli ve renkli daireler olarak işlenir. Dairelerin ölçeği ve rengi içerdikleri noktaların sayısına bağlıdır.

> [!NOTE]
> Google Maps ve Azure Maps biraz farklı kümeleme algoritmaları kullanır. Bu nedenle, bazen kümelerdeki nokta dağılımı farklılık gösterir.

**Önce: Google Maps**

Küme işaretçileri için MarkerCluster kitaplığını kullanın. Küme simgeleri, adları adı olarak beş ile bir arasında olan görüntülerle sınırlıdır. Aynı dizinde barındırılır.

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

![Google Maps Kümelemesi](media/migrate-google-maps-web-app/google-maps-clustering.png)</center>

**Sonrasında: Azure Maps**

Veri kaynağındaki verileri ekleyin ve yönetin. Veri kaynaklarını ve katmanları bağlayıp verileri işleme. `DataSource`Azure Maps 'taki Sınıf çeşitli kümeleme seçenekleri sağlar.

- `cluster` – Veri kaynağını küme noktası verilerine bildirir.
- `clusterRadius` -Bit cinsinden noktaları birlikte kümelemek için yarıçap.
- `clusterMaxZoom` -Kümelemenin gerçekleştiği en yüksek yakınlaştırma düzeyi. Bu düzeyden daha fazla yakınlaştırırsanız, tüm noktaları semboller olarak işlenir.
- `clusterProperties` -Her küme içindeki tüm noktalara karşı ifadeler kullanılarak hesaplanan ve her küme noktasının özelliklerine eklenen özel özellikleri tanımlar.

Kümeleme etkinleştirildiğinde veri kaynağı, işleme için katmanlara kümelenmiş ve kümelenmemiş veri noktaları gönderir. Veri kaynağı yüzlerce binlerce veri noktası kümelemesine sahiptir. Kümelenmiş bir veri noktası aşağıdaki özelliklere sahiptir:

| Özellik adı             | Tür    | Açıklama   |
|---------------------------|---------|---------------|
| `cluster`                 | boolean | Özelliğin bir kümeyi temsil ettiğini belirtir. |
| `cluster_id`              | string  | Küme için, veri kaynağı `getClusterExpansionZoom` , ve yöntemleriyle kullanılabilecek benzersiz BIR kimlik `getClusterChildren` `getClusterLeaves` . |
| `point_count`             | sayı  | Kümenin içerdiği noktaların sayısı.  |
| `point_count_abbreviated` | string  | Long ise değeri abbreviates bir dize `point_count` . (örneğin, 4.000 4K olur)  |

`DataSource`Sınıfı, kullanarak bir kümeyle ilgili ek bilgilere erişmek için aşağıdaki yardımcı işleve sahiptir `cluster_id` .

| Yöntem | Dönüş türü | Açıklama |
|--------|-------------|-------------|
| `getClusterChildren(clusterId: number)` | Promise &lt; dizi &lt; özelliği &lt; geometrisi, herhangi bir &gt; \| Şekil&gt;&gt; | Sonraki yakınlaştırma düzeyinde verilen kümenin alt öğelerini alır. Bu alt öğeler şekil ve alt kümelerin bir birleşimi olabilir. Alt kümeler, ClusteredProperties ile eşleşen özelliklerle özellik olacaktır. |
| `getClusterExpansionZoom(clusterId: number)` | Promise &lt; numarası&gt; | Kümenin genişlemekte veya parçalanmasına başlayacağı yakınlaştırma düzeyini hesaplar. |
| `getClusterLeaves(clusterId: number, limit: number, offset: number)` | Promise &lt; dizi &lt; özelliği &lt; geometrisi, herhangi bir &gt; \| Şekil&gt;&gt; | Kümedeki tüm noktaları alır. `limit`' İ, noktaların bir alt kümesini döndürecek şekilde ayarlayın ve ' ı `offset` noktaları üzerinden kullanın. |

Haritada kümelenmiş verileri işlerken genellikle iki veya daha fazla katman kullanmak en iyisidir. Aşağıdaki örnek üç katman kullanır. Kümelerin boyutuna bağlı olarak ölçeklendirilmiş renkli daireler çizmek için kabarcık katmanı. Küme boyutunu metin olarak işlemek için bir sembol katmanı. Ayrıca, kümelenmemiş noktaları işlemek için ikinci bir sembol katmanını kullanır. Kümelenmiş verileri işlemenin birçok farklı yolu vardır. Daha fazla bilgi için bkz. [küme noktası verileri](clustering-point-data-web-sdk.md) belgeleri.

`importDataFromUrl` `DataSource` Azure haritalar eşlemesi içinde, sınıfındaki Işlevi kullanarak geojson verilerini doğrudan içeri aktarın.

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

![Azure haritalar Kümelemesi](media/migrate-google-maps-web-app/azure-maps-clustering.png)</center>

**Ek kaynaklar:**

- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="add-a-heat-map"></a>Isı haritası ekleme

Nokta yoğunluğu haritaları olarak da bilinen ısı haritaları, veri görselleştirmesinin bir türüdür. Bunlar, bir aralıktaki renkler kullanılarak verilerin yoğunluğunu göstermek için kullanılırlar. Ayrıca, genellikle bir haritada "etkin noktalar" verilerini göstermek için kullanılır. Isı haritaları, büyük nokta veri kümelerini işlemek için harika bir yoldur.

Aşağıdaki örnekler, tüm deprem bir coğrafi JSON akışını, USGS 'lerden geçen aya yükler ve bunları ağırlıklı bir ısı haritası olarak işler. `"mag"`Özelliği ağırlık olarak kullanılır.

**Önce: Google Maps**

Bir ısı haritası oluşturmak için, API betik URL 'sine ekleyerek "görselleştirme" kitaplığını yükleyin `&libraries=visualization` . Google Maps 'taki ısı haritası katmanı doğrudan GeoJSON verilerini desteklemez. İlk olarak, verileri indirin ve ağırlıklı veri noktaları dizisine dönüştürün:

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

![Google Maps ısı haritası](media/migrate-google-maps-web-app/google-maps-heatmap.png)</center>

**Sonrasında: Azure Maps**

GeoJSON verilerini bir veri kaynağına yükleyin ve veri kaynağını bir ısı haritası katmanına bağlayın. Ağırlığa yönelik olarak kullanılacak özellik, `weight` bir ifade kullanılarak seçeneğe geçirilebilir. Sınıf üzerindeki işlevini kullanarak coğrafi JSON verilerini doğrudan Azure Maps 'e aktarın `importDataFromUrl` `DataSource` .

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

![Azure haritalar ısı haritası](media/migrate-google-maps-web-app/azure-maps-heatmap.png)</center>

**Ek kaynaklar:**

- [Isı haritası katmanı ekleme](map-add-heat-map-layer.md)
- [Isı haritası katman sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.heatmaplayer)
- [Isı haritası katman seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions)
- [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

### <a name="overlay-a-tile-layer"></a>Döşeme katmanını kaplama

Azure haritalar 'daki kutucuk katmanları, Google Maps 'ta görüntü Yerpaylaşımları olarak bilinir. Döşeme katmanları, haritalar döşeme sistemiyle birlikte hizalanmış, daha küçük döşeli görüntülerle bölünmüş büyük görüntülerin yer kaplamasını sağlar. Bu yaklaşım, büyük görüntüleri veya büyük veri kümelerini kaplayan yaygın olarak kullanılan bir yaklaşımdır.

Aşağıdaki örnekler, Iowa çevresel Mesonet of Iowa çevre bir hava durumu radar kutucuk katmanını kaplamış.

**Önce: Google Maps**

Google Maps 'ta, döşeme katmanları sınıfı kullanılarak oluşturulabilir `google.maps.ImageMapType` .

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

![Google Maps kutucuk katmanı](media/migrate-google-maps-web-app/google-maps-tile-layer.png)</center>

**Sonrasında: Azure Maps**

Haritaya benzer bir kutucuk katmanını diğer herhangi bir katman gibi ekleyin. X, y, zoom yertutucuları olan bir biçimli URL kullanın; , bir `{x}` `{y}` katmana, `{z}`  KUTUCUKLARA nerede erişebileceğini bildirmek için. Azure haritalar kutucuk katmanları `{quadkey}` , `{bbox-epsg-3857}` ve `{subdomain}` yer tutucuları de destekler.

> [!TIP]
> Azure haritalar katmanları, temel harita katmanları dahil olmak üzere diğer katmanların altına kolayca işlenebilirler. Genellikle, daha kolay okunabilmeleri için harita etiketlerinin altında döşeme katmanlarını işlemek tercih edilir. `map.layers.add`Yöntemi, aşağıdaki yeni katmanın ekleneceği katmanın kimliği olan ikinci bir parametreyi alır. Harita etiketlerinin altına bir kutucuk katmanı eklemek için şu kodu kullanın: `map.layers.add(myTileLayer, "labels");`

```javascript
//Create a tile layer and add it to the map below the label layer.
map.layers.add(new atlas.layer.TileLayer({
    tileUrl: 'https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png',
    opacity: 0.8,
    tileSize: 256
}), 'labels');
```

<center>

![Azure haritalar kutucuk katmanı](media/migrate-google-maps-web-app/azure-maps-tile-layer.png)</center>

> [!TIP]
> Kutucuk istekleri `transformRequest` harita seçeneği kullanılarak yakalanabilir. Bu, istenirse istekleri değiştirmenize veya üst bilgileri eklemenize olanak tanır.

**Ek kaynaklar:**

- [Kutucuk katmanları ekleme](map-add-tile-layer.md)
- [Döşeme katmanı sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer)
- [Döşeme katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions)

### <a name="show-traffic-data"></a>Trafik verilerini gösterme

Trafik verileri hem Azure hem de Google Maps 'ta yer alabilir.

**Önce: Google Maps**

Trafik katmanını kullanarak haritadaki trafik verilerini kaplama.

```javascript
var trafficLayer = new google.maps.TrafficLayer();
trafficLayer.setMap(map);
```

<center>

![Google Haritalar trafiği](media/migrate-google-maps-web-app/google-maps-traffic.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar, trafiği görüntülemek için çeşitli farklı seçenekler sağlar. Yol kapanışları ve kazalardan dolayı gibi trafik olaylarını haritada simgeler olarak görüntüleyin. Harita üzerindeki trafik akışını ve renk kodlu yolların yer kaplamasını sağlama. Renkler, normal beklenen gecikmeye veya mutlak gecikmeye göre, gönderilen hız sınırına göre değiştirilebilir. Azure haritalar 'daki olay verileri her dakikada bir güncelleştirilir ve veri güncelleştirmelerini iki dakikada bir akar.

Seçenekler için istenen değerleri atayın `setTraffic` .

```javascript
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

<center>

![Azure haritalar trafiği](media/migrate-google-maps-web-app/azure-maps-traffic.png)</center>

Azure haritalar 'daki trafik simgelerinden birine tıklarsanız, açılan pencerede ek bilgiler görüntülenir.

<center>

![Azure haritalar trafik olayı](media/migrate-google-maps-web-app/azure-maps-traffic-incident.png)</center>

**Ek kaynaklar:**

- [Haritada trafiği göster](map-show-traffic.md)
- [Trafik kaplama seçenekleri](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Traffic%20Overlay%20Options)

### <a name="add-a-ground-overlay"></a>Zemin kaplama ekleme

Hem Azure hem de Google Maps, haritada coğrafi olarak başvurulan görüntüleri fazla düzenlemeyi destekler. Coğrafi olarak başvurulan görüntüler, Haritada kaydırma ve yakınlaştırma sırasında taşınır ve ölçeklendirebilir. Google Maps 'ta, coğrafi olarak başvurulan görüntüler, Azure haritalar 'da görüntü katmanları olarak adlandırıldıkları sırada arka üste biçimlendirmeler olarak bilinir. Kat planları oluşturmak, eski haritaları yerleştirmek veya bir DRA 'dan Imagery için harika.

**Önce: Google Maps**

Görüntünün haritada bağlanacağı görüntünün URL 'sini ve bir sınırlayıcı kutuyu belirtin. Bu örnek, haritadaki 1922 ' [dan yeni Jersey](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) 'in bir harita görüntüsünü yerdekiler.

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

Bu kodun bir tarayıcıda çalıştırılması, aşağıdaki görüntü gibi görünen bir eşlem görüntüler:

<center>

![Google Maps görüntü kaplaması](media/migrate-google-maps-web-app/google-maps-image-overlay.png)</center>

**Sonrasında: Azure Maps**

`atlas.layer.ImageLayer`Coğrafi olarak başvurulan görüntülerin kaplaması için sınıfını kullanın. Bu sınıf, görüntünün dört köşesi için bir görüntünün URL 'sini ve bir koordinat kümesini gerektirir. Görüntünün aynı etki alanında barındırılması ya da CORs 'nin etkin olması gerekir.

> [!TIP]
> Yalnızca Kuzey, Güney, Doğu, Batı ve döndürme bilgilerine sahipseniz ve görüntünün her bir köşesine ait koordinatlara sahip değilseniz, statik [`atlas.layer.ImageLayer.getCoordinatesFromEdges`](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer#getcoordinatesfromedges-number--number--number--number--number-) yöntemini kullanabilirsiniz.

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

![Azure haritalar görüntü kaplama](media/migrate-google-maps-web-app/azure-maps-image-overlay.png)</center>

**Ek kaynaklar:**

- [Resme katman ekleme](map-add-image-layer.md)
- [Görüntü katmanı sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer)

### <a name="add-kml-data-to-the-map"></a>KML verilerini haritaya ekleme

Hem Azure hem de Google Maps, haritada KML, KMZ ve GeoRSS verilerini içeri aktarabilir ve işleyebilir. Azure haritalar Ayrıca GPX, GML, uzamsal CSV dosyaları, GeoJSON, Iyi bilinen metin (WKT), Web eşleme Hizmetleri (WMS), Web eşleme kutucuk Hizmetleri (WMTS) ve web özelliği Hizmetleri (WFS) destekler. Azure haritalar dosyaları yerel olarak belleğe okur ve çoğu durumda çok daha büyük KML dosyaları işleyebilir. 

**Önce: Google Maps**


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

Bu kodun bir tarayıcıda çalıştırılması, aşağıdaki görüntü gibi görünen bir eşlem görüntüler:

<center>

![Google Maps KML](media/migrate-google-maps-web-app/google-maps-kml.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da GeoJSON, Web SDK 'sında kullanılan ana veri biçimidir, ek uzamsal veri biçimleri de [uzamsal GÇ modülü](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/)kullanılarak kolayca tümleştirilebilir. Bu modülde hem okuma hem de yazma uzamsal verileri için işlevler bulunur ve bu uzamsal veri biçimlerinden herhangi birinden verileri kolayca işleyebilen basit bir veri katmanı da vardır. Uzamsal bir veri dosyasındaki verileri okumak için, bir URL 'ye veya ham verileri işleve dize veya blob olarak geçirin `atlas.io.read` . Bu, dosyadan daha sonra haritaya eklenebilen tüm ayrıştırılmış verileri döndürür. Daha çok daha fazla stil bilgisi içerdiği için KML, en çok uzamsal veri biçiminden biraz daha karmaşıktır. `SpatialDataLayer`Sınıfı, bu stillerin büyük bölümünü oluşturmayı destekler, ancak özellik verileri yüklenmeden önce simge görüntülerinin haritaya yüklenmesi gerekir ve arka kaplamalar haritaya ayrı olarak eklenir. Bir URL aracılığıyla veri yüklerken, CORs 'yi etkinleştirmiş bir uç noktada barındırılmalıdır veya bir proxy hizmeti okuma işlevine seçenek olarak geçirilmelidir. 

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

![Azure haritalar KML](media/migrate-google-maps-web-app/azure-maps-kml.png)</center>

**Ek kaynaklar:**

- [Atlas. IO. Read işlevi](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io#read-string---arraybuffer---blob--spatialdatareadoptions-)
- [SimpleDataLayer](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.layer.simpledatalayer)
- [SimpleDataLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.simpledatalayeroptions)

## <a name="additional-code-samples"></a>Ek kod örnekleri

Google Maps geçişi ile ilgili bazı ek kod örnekleri aşağıda verilmiştir:

- [Çizim araçları](map-add-drawing-toolbar.md)
- [Eşlemeyi Iki parmak kaydırma ile sınırla](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Limit%20Map%20to%20Two%20Finger%20Panning)
- [Sınır kaydırma tekerleği yakınlaştırmasını sınırla](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Limit%20Scroll%20Wheel%20Zoom)
- [Tam ekran denetimi oluşturma](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Create%20a%20Fullscreen%20Control)

**Servislere**

- [Azure haritalar Hizmetleri modülünü kullanma](how-to-use-services-module.md)
- [İlgi noktası arama](map-search-location.md)
- [Bir koordinatdan bilgi edinme (ters coğrafi kod)](map-get-information-from-coordinate.md)
- [A'dan B'ye yönleri gösterme](map-route.md)
- [JQuery Kullanıcı arabirimi ile otomatik öneri ara](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Search%20Autosuggest%20and%20JQuery%20UI)

## <a name="google-maps-v3-to-azure-maps-web-sdk-class-mapping"></a>Google, v3 'yi Azure Maps web SDK sınıf eşlemesi ile eşleştirir

Aşağıdaki ek, Google Maps v3 ve eşdeğer Azure Maps web SDK 'sında yaygın olarak kullanılan sınıfların çapraz bir başvurusunu sağlar.

### <a name="core-classes"></a>Çekirdek sınıflar

| Google Haritalar   | Azure Haritalar  |
|---------------|-------------|
| `google.maps.Map` | [Atlas. Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)  |
| `google.maps.InfoWindow` | [Atlas. Kutu](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup)  |
| `google.maps.InfoWindowOptions` | [Atlas. PopupOptions](https://docs.microsoft.com/) |
| `google.maps.LatLng`  | [Atlas. Data. Position](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position)  |
| `google.maps.LatLngBounds` | [Atlas. Data. BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox) |
| `google.maps.MapOptions`  | [Atlas. CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraoptions)<br/>[Atlas. CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraboundsoptions)<br/>[Atlas. ServiceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.serviceoptions)<br/>[Atlas. StyleOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions)<br/>[Atlas. Userınteractionoptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.userinteractionoptions) |
| `google.maps.Point`  | [Atlas. Pikselden](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.pixel)   |

## <a name="overlay-classes"></a>Sınıf kaplama

| Google Haritalar  | Azure Haritalar  |
|--------------|-------------|
| `google.maps.Marker` | [atlas.HtmLişaretleyici](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker)<br/>[Atlas. Data. Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point)  |
| `google.maps.MarkerOptions`  | [atlas.HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions)<br/>[Atlas. Layer. SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer)<br/>[Atlas. SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions)<br/>[Atlas. IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions)<br/>[Atlas. TextOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions)<br/>[Atlas. Layer. BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer)<br/>[Atlas. BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions) |
| `google.maps.Polygon`  | [Atlas. Data. Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon)               |
| `google.maps.PolygonOptions` |[Atlas. Layer. PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer)<br/> [Atlas. PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions)<br/> [Atlas. Layer. LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer)<br/> [Atlas. LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions)|
| `google.maps.Polyline` | [Atlas. Data. LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring)         |
| `google.maps.PolylineOptions` | [Atlas. Layer. LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer)<br/>[Atlas. LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions) |
| `google.maps.Circle`  | Bkz [. haritaya daire ekleme](map-add-shape.md#add-a-circle-to-the-map)                                     |
| `google.maps.ImageMapType`  | [Atlas. TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer)         |
| `google.maps.ImageMapTypeOptions` | [Atlas. TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions) |
| `google.maps.GroundOverlay`  | [Atlas. Layer. ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer)<br/>[Atlas. Imagelayeroptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions) |

## <a name="service-classes"></a>Hizmet sınıfları

Azure Haritalar Web SDK 'Sı, ayrı olarak yüklenebilen bir hizmet modülünü içerir. Bu modül, Azure Maps REST hizmetlerini bir Web API 'siyle sarmalayan ve JavaScript, TypeScript ve Node.js uygulamalarında kullanılabilir.

| Google Haritalar | Azure Haritalar  |
|-------------|-------------|
| `google.maps.Geocoder` | [Atlas. Service. SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl)  |
| `google.maps.GeocoderRequest`  | [Atlas. SearchAddressOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressoptions)<br/>[Atlas. SearchAddressRevrseOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreverseoptions)<br/>[Atlas. Searchaddresssmarcrossstreetoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreversecrossstreetoptions)<br/>[Atlas. Searchaddressstructureınfo](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressstructuredoptions)<br/>[Atlas. SearchAlongRouteOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchalongrouteoptions)<br/>[Atlas. Searchbelirsizlik Zyoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchfuzzyoptions)<br/>[Atlas. Searchınsidegeometrik Yoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchinsidegeometryoptions)<br/>[Atlas. Searchyaklaştığında Byoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchnearbyoptions)<br/>[Atlas. SearchPOIOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoioptions)<br/>[Atlas. SearchPOICategoryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoicategoryoptions) |
| `google.maps.DirectionsService`  | [Atlas. Service. RouteUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl)  |
| `google.maps.DirectionsRequest`  | [Atlas. Hesaplakaldırma Teroutedirectionsoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.calculateroutedirectionsoptions) |
| `google.maps.places.PlacesService` | [Atlas. Service. SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl)  |

## <a name="libraries"></a>Kitaplıklar

Kitaplıklar haritaya ek işlevsellik ekler. Bu kitaplıkların birçoğu Azure haritalar 'ın temel SDK 'larıdır. Bu Google Maps kitaplıklarının yerine kullanılacak bazı eşdeğer sınıflar şunlardır

| Google Haritalar           | Azure Haritalar   |
|-----------------------|--------------|
| Çizim kitaplığı       | [Çizim araçları modülü](set-drawing-options.md) |
| Geometri kitaplığı      | [Atlas. matematik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math)   |
| Görselleştirme kitaplığı | [Isı haritası katmanı](map-add-heat-map-layer.md) |

## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar Web SDK 'Sı hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Harita denetimini kullanma](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Hizmetler modülünü kullanma](how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Çizim araçları modülünü kullanma](set-drawing-options.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)

> [!div class="nextstepaction"]
> [Azure Maps web SDK hizmeti API başvuru belgeleri](https://docs.microsoft.com/javascript/api/azure-maps-control/)
