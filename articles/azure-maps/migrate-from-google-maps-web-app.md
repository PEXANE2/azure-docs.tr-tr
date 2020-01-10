---
title: Google Haritalar 'dan bir Web uygulaması geçirme | Microsoft Docs
description: Bir Web uygulamasını Google Maps 'tan Microsoft Azure Maps 'a geçirmeye yönelik bir öğretici.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: a414d7b15f81ab81783b66f8297a207afe569365
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75562179"
---
# <a name="migrate-a-web-app-from-google-maps"></a>Google Maps 'tan bir Web uygulaması geçirme

Google Maps kullanan çoğu Web uygulaması Google Maps v3 JavaScript SDK 'sını kullanıyor. Azure Haritalar Web SDK 'Sı, geçirilecek Azure tabanlı uygun SDK 'dir. Azure Haritalar Web SDK 'Sı, etkileşimli haritaları, Web veya mobil uygulamalarınızda görüntülenmek üzere kendi içeriklerinizi ve Imagery 'yi özelleştirmenize olanak tanır. Bu denetimde büyük veri kümelerini yüksek performansla oluşturmanızı sağlayan WebGL bileşeni kullanılmaktadır. JavaScript veya TypeScript kullanarak bu SDK ile geliştirin.

Var olan bir Web uygulamasını geçiriyorsanız, Cesium, leaflet ve Openkatmanları gibi bir açık kaynak eşleme denetim kitaplığı kullanıp kullanmın olup olmadığını kontrol edin. Bu ise ve Azure Maps web SDK 'sını kullanmak istemiyorsanız, uygulamanızı geçirmeye yönelik başka bir seçenek de açık kaynak eşleme denetimini kullanmaya devam eder ve Azure Maps kutucuk Hizmetleri 'ne ([yol kutucukları](https://docs.microsoft.com/rest/api/maps/render/getmaptile) \| [uydu kutucukları](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile)) bağlanır. Aşağıda, yaygın olarak kullanılan bazı açık kaynaklı harita denetim kitaplıklarında Azure maps kullanma hakkında ayrıntılar verilmiştir.

- Cesium-Web için 3B harita denetimi. [Kod örneği](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20Cesium%20JS) \| [belgeleri](https://cesiumjs.org/)
- Leaflet – Web için hafif 2B harita denetimi. [Kod örneği](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Azure%20Maps%20Raster%20Tiles%20in%20Leaflet%20JS) \| [belgeleri](https://leafletjs.com/)
- Openkatmanlar-Web için projeksiyonları destekleyen bir 2B harita denetimi. [Kod örneği](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20OpenLayers) \| [belgeleri](https://openlayers.org/)

## <a name="key-features-support"></a>Anahtar özellikleri desteği

Aşağıdaki tabloda, Google Maps v3 JavaScript SDK 'sindeki temel API özellikleri ve Azure Maps web SDK 'sında benzer bir API desteği listelenmektedir.

| Google Haritalar özelliği     | Azure Maps web SDK desteği |
|-------------------------|:--------------------------:|
| İşaretler                 | ✓                          |
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
| Yükseltme hizmeti       | Planlandı                    |

## <a name="notable-differences-in-the-web-sdks"></a>Web SDK 'lerinde önemli farklılıklar

Aşağıda, Google Maps ve Azure Maps web SDK 'Ları arasındaki önemli farklılıklar aşağıda verilmiştir:

- Azure Maps web SDK 'sına erişim için barındırılan bir uç nokta sağlamaya ek olarak, tercih edilen durumlarda Web SDK 'sını uygulamalara katıştırmak için de bir NPM paketi de mevcuttur. Daha fazla bilgi için bu [belgelere](how-to-use-map-control.md) bakın. Bu paket TypeScript tanımlarını da içerir.
- Azure haritalar 'da harita sınıfının bir örneğini oluşturduktan sonra, kodunuzun eşlemeyle etkileşime geçmeden önce haritaların `ready` veya `load` olayının tetiklenmesi için beklemesi gerekir. Bu, tüm harita kaynaklarının yüklenmiş ve erişilmeye hazırsa emin olmanızı sağlar.
- Her iki platform da temel haritalar için benzer bir döşeme sistemi kullanır, ancak Google Maps 'daki kutucuklar boyut olarak 256 pikselden, Azure Maps 'teki kutucuklar boyut olarak 512 piksel boyutlardır. Bu nedenle, Azure haritalar 'da Google Maps olarak aynı harita görünümünü almak için, Google Maps 'ta kullanılan bir yakınlaştırma düzeyinin Azure haritalar 'da bir tane tarafından çıkarılan olması gerekir.
- Azure haritalar "boylam, enlem" kullanır ve Google Maps 'taki koordinatlar "enlem, Boylam" olarak adlandırılır. Bu, en fazla GıS platformundan sonra gelen standart `[x, y]` hizalanır.
- Azure Haritalar Web SDK 'sindeki şekiller GeoJSON şemasına dayalıdır. Yardımcı sınıflar, [ *Atlas. Data* ad alanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data?view=azure-iot-typescript-latest)aracılığıyla sunulur. Ayrıca Atlas de vardır [ *.* ](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape)GeoJSON nesnelerini kaydırmak ve veri bağlanabilir bir şekilde güncellemenin ve bakımını kolaylaştırmak için kullanılabilecek şekil sınıfı.
- Azure haritalarındaki Koordinatlar, `[longitude, latitude]` veya New Atlas. Data. Position (Boylam, Enlem) biçiminde basit bir sayı dizisi olarak belirtibilen konum nesneleri olarak tanımlanır.
    > [!TIP]
    > Konum sınıfı, "enlem, Boylam" biçiminde olan koordinatları içeri aktarmaya yönelik statik bir yardımcı yönteme sahiptir. [Atlas. Data. Position. fromLatLng](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest) yöntemi genellikle Google Maps kodundaki `new google.maps.LatLng` yönteminin yerini alabilir.
- Haritaya eklenen her bir şekil üzerinde stil bilgilerini belirtmek yerine, Azure Maps stilleri verilerden ayırır. Veriler veri kaynaklarında depolanır ve Azure Maps kodunun verileri işlemek için kullandığı katmanları işlemek üzere bağlanır. Bu yaklaşım, gelişmiş performans avantajı sağlar. Ayrıca, birçok katman, çalışma mantığının şekil içinde tanımlanan özelliklere bağlı olarak bir katman içinde nasıl işleneceğini değiştirecek katman stili seçeneklerine eklenebileceği veri tabanlı stillendirme desteği sağlar.

## <a name="web-sdk-side-by-side-examples"></a>Web SDK yan yana örnekleri

Aşağıda, Web uygulamanızı Google Maps v3 JavaScript SDK 'dan Azure Maps web SDK 'sına geçirmenize yardımcı olacak genel kullanım durumlarını kapsayan her bir platform için kod örnekleri koleksiyonu verilmiştir. Web uygulamalarıyla ilgili kod örnekleri JavaScript 'te verilmiştir; Ancak Azure Maps, bir [NPM modülü](how-to-use-map-control.md)aracılığıyla ek bir seçenek olarak TypeScript tanımları da sağlar.

### <a name="load-a-map"></a>Harita yükleme

Her iki SDK 'da da bir haritanın yüklenmesi aynı adım kümesini izler:

- Map SDK 'sına bir başvuru ekleyin.
- Sayfanın gövdesine, eşleme için yer tutucu görevi görecek bir `div` etiketi ekleyin.
- Sayfa yüklendiğinde çağrılan bir JavaScript işlevi oluşturun.
- Karşılık gelen eşleme sınıfının bir örneğini oluşturun.

**Bazı önemli farklılıklar**

- Google Maps, API 'nin betik başvurusunda bir hesap anahtarının belirtilmesini gerektirir. Azure haritalar için kimlik doğrulama kimlik bilgileri, Map sınıfının seçenekleri olarak belirtilir. Bu bir abonelik anahtarı veya Azure Active Directory bilgileri olabilir.
- Google Maps, Haritayı yüklemek için bir başlatma işlevi çağırmak üzere kullanılan API 'nin betik başvurusunda bir geri çağırma işlevi alır. Azure haritalar ile sayfanın OnLoad olayının kullanılması gerekir.
- Haritanın işlenebileceği `div` öğesine başvurulduğunda, Azure Maps 'daki `Map` sınıfı, yalnızca Google Maps bir `HTMLElement` nesnesi gerektirdiğinde `id` değeri gerektirir.
- Azure haritalar 'daki Koordinatlar, `[longitude, latitude]`biçiminde basit bir sayı dizisi olarak belirtibilen konum nesneleri olarak tanımlanır.
- Azure haritalar 'daki yakınlaştırma düzeyi, platformlar arasındaki sistem boyutlarını döşeme farklılığı nedeniyle Google Maps örneğinden daha düşük bir düzeydir.
- Varsayılan olarak, Azure haritalar harita tuvaline yakınlaştırma düğmeleri ve harita stili düğmeleri gibi herhangi bir gezinti denetimi eklemez. Ancak harita stili seçici, yakınlaştırma düğmeleri, pusula veya döndürme denetimi ve bir aralıklı denetim ekleme denetimleri vardır.
- Harita örneğinin `ready` olayını izlemek için Azure Maps 'a bir olay işleyicisi eklenir. Bu, eşleme WebGL bağlamını ve gerekli tüm kaynakları yüklemeyi tamamladığında harekete geçmeyecektir. Bu olay işleyicisine herhangi bir yükleme sonrası kodu eklenebilir.

Aşağıdaki örneklerde, New York üzerinden ortalanan bir temel haritanın koordinatları (Boylam:-73,985, Latitude: 40,747) ve Google Maps 'ta yakınlaştırma düzeyi 12 ' de nasıl yükleneceği gösterilmektedir.

**Önce: Google Maps**

Aşağıdaki kod, bir Google Map 'in bir konum üzerinden ortalanmasını ve Yakınlaştırılıp yakınlaştırılmadığını gösteren bir örnektir.

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

Basit Google Maps ![](media/migrate-google-maps-web-app/simple-google-map.png)</center>

**Sonrasında: Azure Maps**

Aşağıdaki kod, harita stili denetim ve yakınlaştırma düğmeleriyle birlikte Azure Maps 'ta aynı görünümle bir haritanın nasıl yükleneceğini gösterir.

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

![basit Azure Maps](media/migrate-google-maps-web-app/simple-azure-maps.png)</center>

Azure haritalar harita denetimini ayarlama ve kullanma hakkında ayrıntılı belgeler [burada](how-to-use-map-control.md)bulunabilir.

> [!NOTE]
> Google Maps 'tan farklı olarak, Azure Maps harita yüklenirken bir başlangıç merkezi ve yakınlaştırma düzeyinin belirtilmesini gerektirmez. Eşleme yüklenirken bu bilgiler sağlanmazsa, eşleme kullanıcının hangi şehri kullandığını belirlemeyi dener ve Haritayı buraya ortalayın ve yakınlaştırın.

**Ek kaynaklar:**

- Azure haritalar Ayrıca, [burada](map-add-controls.md)açıklandığı gibi harita görünümünü döndürme ve ele döndürme için de gezinti denetimleri sağlar.

### <a name="localizing-the-map"></a>Haritayı yerelleştirme

Hedef kitleniz birden çok ülkede yayıldığında veya farklı dillerde konuşabilçalışıyorsa, yerelleştirme önemlidir.

**Önce: Google Maps**

Google Maps 'ı yerelleştirmek için, dil ve bölge parametreleri öğesine eklenir

```html
<script type="text/javascript" src=" https://maps.googleapis.com/maps/api/js?callback=initMap&key=[api_key]& language=[language_code]&region=[region_code]" async defer></script>
```

Dilde "fr-FR" olarak ayarlanmış Google Maps örneği aşağıda verilmiştir.

<center>

![Google Maps yerelleştirme](media/migrate-google-maps-web-app/google-maps-localization.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar haritanın dilini ve bölgesel görünümünü ayarlamanın iki farklı yolunu sağlar. İlk seçenek, bu bilgileri Global *Atlas* ad alanına eklemektir. Bu, uygulamanızdaki tüm harita denetim örneklerinin bu ayarlara göre varsayılan olarak oluşmasına neden olur. Aşağıdaki dil, dili Fransızca ("fr-FR") ve bölgesel görünüm olarak "Auto" olarak ayarlar:

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

İkinci seçenek, aşağıdaki gibi eşleme yüklenirken bu bilgileri eşleme seçeneklerine iletmektir:

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
> Azure haritalar ile aynı sayfada farklı dil ve bölge ayarlarıyla birden çok eşleme örneği yüklemek mümkündür. Ayrıca, bu ayarları haritadan yüklendikten sonra güncelleştirmek de mümkündür. Azure Maps 'ta desteklenen dillerin ayrıntılı bir listesi [burada](supported-languages.md)bulunabilir.

Aşağıda dil "fr" olarak ayarlanan ve Kullanıcı bölgesinin "fr-FR" olarak ayarlandığı bir Azure Maps örneği verilmiştir.

<center>

![Azure Maps yerelleştirme](media/migrate-google-maps-web-app/azure-maps-localization.png)</center>

### <a name="setting-the-map-view"></a>Harita görünümü ayarlanıyor

Hem Azure hem de Google Maps içindeki dinamik haritalar, JavaScript 'te uygun işlevler çağırarak programlı bir şekilde yeni coğrafi konumlara taşınabilir. Aşağıdaki örneklerde haritanın uydu hava imagery görüntüsünü nasıl görüntüleyeceği, haritanın koordinatlarla bir konum üzerinden ortalanıyor (Boylam:-111,0225, Latitude: 35,0272) ve Google Maps 'ta yakınlaştırma düzeyini 15 olarak değiştirme gösterilmektedir.

> [!NOTE]
> Azure haritalar daha büyük bir 512 piksellik kutucuk kullandığında, Google Maps, boyutlarda 256 piksel olan kutucukları kullanır. Bu, Google Maps ile aynı harita alanını yüklemek için Azure haritalar tarafından gerek duyulan ağ isteklerinin sayısını azaltır. Ancak, Tile Pyramids 'in harita denetimlerinde çalışma yöntemi nedeniyle, Azure haritalar 'daki daha büyük Kutucuklar, Google Maps 'ta bir eşleme olarak aynı görüntülenebilir alana ulaşmak için, Azure haritalar kullanırken Google Maps 'ta kullanılan yakınlaştırma düzeyini tek tek çıkarmanız gerekir.

**Önce: Google Maps**

Google Maps harita denetimi, eşlemenin merkezini ve yakınlaştırma düzeyini belirtmenize olanak tanıyan `setOptions` yöntemi kullanılarak programlı bir şekilde taşınabilir.

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

Azure haritalar 'da harita konumu, eşlemenin `setCamera` yöntemi kullanılarak programlı olarak değiştirilebilir ve `setStyle` yöntemi kullanılarak harita stili değişikliği değiştirilir. Azure haritalar 'daki koordinatların "boylam, enlem" biçiminde olduğunu ve yakınlaştırma düzeyi değerinin bir tane tarafından çıkartılır olduğunu unutmayın.

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

![Azure Maps kümesi görünümü](media/migrate-google-maps-web-app/azure-maps-set-view.jpeg)</center>

**Ek kaynaklar:**

- [Harita stili seçin](choose-map-style.md)
- [Desteklenen harita stilleri](supported-map-styles.md)

### <a name="adding-a-marker"></a>İşaretleyici ekleme

Azure haritalar 'da, bu nokta verilerinin haritada işlenebileceği birden çok yolu vardır;

- **HTML işaretçileri** : geleneksel DOM öğelerini kullanarak noktaları işler. HTML Işaretçileri sürüklemeyi destekler.
- **Sembol katmanı** : bir simge ve/veya metin Içeren noktaları WebGL bağlamı içinde işler.
- **Kabarcık katmanı** : noktaları haritada daireler olarak işler. Dairelerin yarıçapı, verilerdeki özelliklere göre ölçeklendirilebilir.

Hem sembol hem de kabarcık katmanları WebGL bağlamında işlenir ve haritada çok büyük noktaların kümelerini işleyebilir. Bu katmanlar, verilerin bir veri kaynağında depolanmasını gerektirir. `ready` olayı tetiklendikten sonra veri kaynakları ve işleme katmanları haritaya eklenmelidir. HTML Işaretçileri, sayfada DOM öğeleri olarak işlenir ve veri kaynağı kullanmaz. Bir sayfada daha fazla DOM öğesi varsa, sayfa ne kadar yavaş olur. Bir haritada birkaç yüz noktasından fazlasını işliyorsa, bunun yerine işleme katmanlarından birini kullanmanız önerilir.

Aşağıdaki örnekler, (Boylam:-0,2, Latitude: 51,5) konumundaki haritaya bir etiket olarak bulunan 10 sayısıyla bir işaret ekler.

**Önce: Google Maps**

Google Maps ile, `google.maps.Marker` sınıfını kullanarak ve bir eşlemeyi seçeneklerden biri olarak belirterek, işaretçiler haritaya eklenir.

```javascript
//Create a marker and add it to the map.
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(51.5, -0.2),
    label: '10',
    map: map
});
```

<center>

![Google Maps işaretleyici](media/migrate-google-maps-web-app/google-maps-marker.png)</center>

**Sonra: HTML Işaretçileri kullanarak Azure Maps**

Azure haritalar 'da HTML işaretçileri, haritada bir noktayı göstermek için kullanılabilir ve yalnızca eşleme üzerinde az sayıda nokta görüntülemesi gereken uygulamalar için önerilir. Bir HTML işaretleyicisi kullanmak için, `atlas.HtmlMarker` sınıfının bir örneğini oluşturun, metin ve konum seçeneklerini ayarlayın ve `map.markers.add` yöntemini kullanarak işaretçiyi haritaya ekleyin.

```javascript
//Create a HTML marker and add it to the map.
map.markers.add(new atlas.HtmlMarker({
    text: '10',
    position: [-0.2, 51.5]
}));
```

<center>

Azure haritalar HTML işaretçisini ![](media/migrate-google-maps-web-app/azure-maps-html-marker.png)</center>

**Sonra: bir sembol katmanı kullanarak Azure Maps**

Bir sembol katmanı kullanırken, verilerin bir veri kaynağına eklenmesi ve katmana eklenmiş veri kaynağı olması gerekir. Ayrıca, `ready` olay tetiklendikten sonra veri kaynağı ve katman haritaya eklenmelidir. Bir simgenin üzerinde benzersiz bir metin değeri işlemek için metin bilgilerinin, veri noktasının bir özelliği olarak depolanması gerekir ve bu özellik katmanın `textField` seçeneğinde başvurulmalıdır. Bu, HTML işaretçilerini kullanmaktan biraz daha çalışır, ancak birçok performans avantajı sağlar.

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

![Azure Maps sembol katmanını](media/migrate-google-maps-web-app/azure-maps-symbol-layer.png)</center>

**Ek kaynaklar:**

- [Veri kaynağı oluşturma](create-data-source-web-sdk.md)
- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [HTML Işaretçileri Ekle](map-add-custom-html.md)
- [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)
- [Sembol katmanı simgesi seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)
- [Sembol katmanı metin seçeneği](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)
- [HTML işaret sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)
- [HTML işaret seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

### <a name="adding-a-custom-marker"></a>Özel işaretleyici ekleme

Özel görüntüler, bir haritadaki noktaları temsil etmek için kullanılabilir. Aşağıdaki örnekte kullanılan görüntü, haritada bir noktayı (Latitude: 51,5, Boylam:-0,2) göstermek için özel bir görüntü kullanır ve işaretin konumunu, raptiye simgesinin noktasının haritada doğru konuma göre hizalanmasını sağlayacak şekilde kaydırır.

<center>

sarı iğne görüntüsünü ![](media/migrate-google-maps-web-app/ylw_pushpin.png)<br/>
YLW\_iğne. png</center>

**Önce: Google Maps**

Google Maps 'ta, görüntüye `url` içeren `Icon` bir nesne belirtilerek özel bir işaret oluşturulur. Bu bir `anchor` noktası, raptiye görüntüsünün noktasını haritadaki koordinatla hizalamak için kullanılır. Google Maps içindeki tutturucu değeri görüntünün sol üst köşesine göre.

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

![Google Maps özel işaretleyici](media/migrate-google-maps-web-app/google-maps-custom-marker.png)</center>

**Sonra: HTML Işaretçileri kullanarak Azure Maps**

Azure 'da HTML işaretçisi özelleştirmek için bir HTML `string` veya `HTMLElement` işaret `htmlContent` seçeneğine geçirilebilir. Azure haritalar 'da, belirlenen dokuz başvuru noktasından birini kullanarak konum koordinatına göre işaretin göreli konumunu belirtmek için bir `anchor` seçeneği kullanılır; "Orta", "üst", "alt", "sol", "sağ", "üst-sol", "üst-sağ", "alt-sol", "alt-sağ". İçerik varsayılan olarak HTML içeriğinin alt merkezine bağlanır. Google Maps 'tan kod geçirmeyi kolaylaştırmak için, `anchor` "üst-sol" olarak ayarlayın ve ardından Google Maps ' de kullanılan aynı uzaklığa sahip `pixelOffset` seçeneğini kullanın. Azure haritalar 'daki uzaklıklar, Google Maps 'ın ters yönünde taşınır, bu nedenle bunları eksi bir ile çarpın.

> [!TIP]
> Microsoft Edge 'de istenmeyen bir simge görüntüleyen varsayılan sürükleme davranışını devre dışı bırakmak için, HTML içeriğine bir stil olarak `pointer-events:none` ekleyin.

```javascript
map.markers.add(new atlas.HtmlMarker({
    htmlContent: '<img src="https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png" style="pointer-events: none;" />',
    anchor: 'top-left',
    pixelOffset: [-5, -30],
    position: [-0.2, 51.5]
}));
```

<center>

Azure Maps özel HTML işaretçisini ![](media/migrate-google-maps-web-app/azure-maps-custom-html-marker.png)</center>

**Sonra: bir sembol katmanı kullanarak Azure Maps**

Azure haritalar 'daki sembol katmanları özel görüntüleri de destekler, ancak görüntünün önce harita kaynaklarına yüklenmesi ve benzersiz bir KIMLIK ataması gerekir. Sembol katmanı daha sonra bu KIMLIĞE başvurabilir. Simge `offset` seçeneği kullanılarak görüntüde doğru noktaya hizalamak için simge kaydırılabilir. Azure haritalar 'da, bir `anchor` seçeneği, simgenin göreli konumunu, konum koordinatına göre, dokuz tanımlı başvuru noktalarından birini kullanarak belirtmek için kullanılır; "Orta", "üst", "alt", "sol", "sağ", "üst-sol", "üst-sağ", "alt-sol", "alt-sağ". İçerik varsayılan olarak HTML içeriğinin alt merkezine bağlanır. Google Maps 'tan kod geçirmeyi kolaylaştırmak için, `anchor` "üst-sol" olarak ayarlayın ve ardından Google Maps ' de kullanılan aynı uzaklığa sahip `offset` seçeneğini kullanın. Azure haritalar 'daki uzaklıklar, Google Maps 'ın ters yönünde taşınır, bu nedenle bunları eksi bir ile çarpın.

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

![Azure Maps özel simge sembol katmanı](media/migrate-google-maps-web-app/azure-maps-custom-icon-symbol-layer.png)</center>

> [!TIP]
> Gelişmiş özel noktayla işleme oluşturmak için birden çok işleme katmanını birlikte kullanın. Örneğin, farklı renkli daireler üzerinde aynı simgeye sahip birden fazla Pushpin 'e sahip olmak istiyorsanız, her renk için bir sembol katmanı oluşturmak yerine bir kabarcık katmanının üzerine bir sembol katmanını ve aynı veri kaynağına başvurmalarını sağlar. Bu, oluşturmaktan çok daha verimlidir ve haritanın farklı görüntüleri korumasını sağlar.

**Ek kaynaklar:**

- [Veri kaynağı oluşturma](create-data-source-web-sdk.md)
- [Sembol katmanı ekleme](map-add-pin.md)
- [HTML Işaretçileri Ekle](map-add-custom-html.md)
- [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)
- [Sembol katmanı simgesi seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)
- [Sembol katmanı metin seçeneği](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)
- [HTML işaret sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)
- [HTML işaret seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

### <a name="adding-a-polyline"></a>Çoklu çizgi ekleme

Polylines, haritada bir çizgiyi veya yolu göstermek için kullanılır. Aşağıdaki örneklerde, haritada kesik çizgili bir çoklu çizginin nasıl oluşturulacağı gösterilmektedir.

**Önce: Google Maps**

Google Maps 'ta, çoklu çizgi sınıfı bir seçenek kümesi alır. Çoklu çizginin `path` seçeneğinde koordinat dizisi geçirilir.

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

![Google Maps çoklu çizgi](media/migrate-google-maps-web-app/google-maps-polyline.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da çoklu çizgilerin çizgisini, LineString veya MultiLineString nesneleri olarak adlandırılır. Bu nesneler bir veri kaynağına eklenebilir ve çizgi katmanı kullanılarak oluşturulabilir.

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

![Azure Maps çoklu çizgi](media/migrate-google-maps-web-app/azure-maps-polyline.png)</center>

**Ek kaynaklar:**

- [Haritaya satır ekleme](map-add-line-layer.md)
- [Çizgi katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)
- [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)

### <a name="adding-a-polygon"></a>Çokgen ekleme

Çokgenler, haritadaki bir alanı temsil etmek için kullanılır. Azure haritalar ve Google Maps, çokgenler için çok benzer destek sağlar. Aşağıdaki örneklerde haritanın orta koordinatına göre bir üçgen oluşturan bir çokgen oluşturma gösterilmektedir.

**Önce: Google Maps**

Google Maps 'ta Çokgen sınıfı bir seçenekler kümesi alır. Çokgenin `paths` seçeneğinde koordinat dizisi geçirilir.

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

![Google Maps Çokgen](media/migrate-google-maps-web-app/google-maps-polygon.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da Çokgen ve MultiPolygon nesneleri bir veri kaynağına eklenebilir ve katmanlar kullanılarak haritada işlenebilir. Çokgenin alanı çokgen katmanında işlenebilir. Bir çokgenin ana hattı bir çizgi katmanı kullanılarak oluşturulabilir.

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

Azure haritalar Çokgen ![](media/migrate-google-maps-web-app/azure-maps-polygon.png)</center>

**Ek kaynaklar:**

- [Haritaya Çokgen ekleme](map-add-shape.md)
- [Haritaya daire ekleme](map-add-shape.md#add-a-circle-to-the-map)
- [Çokgen katman seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)
- [Çizgi katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)
- [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)

### <a name="display-an-info-window"></a>Bilgi penceresi görüntüle

Bir varlık için ek bilgi, Google Maps 'ta bir `google.maps.InfoWindow` sınıfı olarak haritada görüntülenebilir, Azure Maps 'ta bu, `atlas.Popup` sınıfı kullanılarak elde edilebilir. Aşağıdaki örnekler haritaya bir işaret ekler ve tıklandı bir bilgi penceresi/açılan pencere görüntüler.

**Önce: Google Maps**

Google Maps ile `google.maps.InfoWindow` Oluşturucu kullanılarak bir bilgi penceresi oluşturulur.

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

![Google Maps açılır](media/migrate-google-maps-web-app/google-maps-popup.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da, bir konum için ek bilgi göstermek üzere bir açılan pencere kullanılabilir. Bir HTML `string` veya `HTMLElement` nesnesi açılan pencerenin `content` seçeneğine geçirilebilir. Açılır pencereler isterseniz herhangi bir şekilden bağımsız olarak görüntülenebilir ve bu nedenle, `position` bir değer belirtilmesini gerektirir. Bir açılan pencere göstermek için `open` yöntemi çağırın ve açılan pencerenin üzerinde görüntüleneceği `map` geçirin.

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

Azure haritalar açılan ![](media/migrate-google-maps-web-app/azure-maps-popup.png)</center>

> [!NOTE]
> Bir sembol, kabarcık, çizgi veya Çokgen katmanıyla aynı şeyi yapmak için, katmanı bir işaret yerine haritalar olay koduna geçirmeniz yeterlidir.

**Ek kaynaklar:**

- [Açılan pencere Ekle](map-add-popup.md)
- [Medya Içeriğiyle açılan pencere](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popup%20with%20Media%20Content)
- [Şekillerdeki açılanlar](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popups%20on%20Shapes)
- [Çoklu PIN ile açılan pencereyi yeniden kullanma](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Reusing%20Popup%20with%20Multiple%20Pins)
- [Açılır Sınıf](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
- [Açılan seçenekler](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

### <a name="import-a-geojson-file"></a>GeoJSON dosyasını içeri aktar

Google Maps, `google.maps.Data` sınıfı aracılığıyla GeoJSON verilerinin yüklenmesini ve dinamik olarak stillendirilmesini destekler. Bu sınıfın işlevselliği, Azure haritalar 'ın veri odaklı stille çok daha fazlasını hizalar. Tek bir temel fark, Google Maps ile bir geri çağırma işlevi ve Kullanıcı arabirimi iş parçacığında ayrı olarak işlenen her bir özelliğin stillendirilmesini sağlayan iş mantığını belirtmektir. Azure haritalar 'da katmanlar, veri tabanlı ifadeleri stil seçenekleri olarak belirtmeyi destekler. Bu ifadeler ayrı bir iş parçacığında işleme zamanında işlenir ve daha fazla veri kümesinin daha hızlı işlenmesine olanak sağlar.

Aşağıdaki örneklerde, USG 'lerdeki son yedi güne ait tüm deprem coğrafi JSON akışı yüklenir ve bunları haritada ölçeklendirilmiş daireler olarak işler. Her dairenin rengi ve ölçeği, veri kümesindeki her bir özelliğin `"mag"` özelliğinde depolanan her bir deprem büyüklüğüne bağlıdır. Büyüklük beşten büyük veya buna eşitse daire kırmızı olacaktır, üç veya daha büyük ancak beşten küçükse daire turuncu, üçten küçükse daire ise yeşil olur. Her dairenin yarıçapı, 0,1 ile çarpıldığı büyüklük üsünü üstel olacaktır.

**Önce: Google Maps**

Google Maps 'ta, coğrafi JSON akışından yüklenen her bir özelliğe `map.data.loadGeoJson` yöntemi aracılığıyla iş mantığını uygulamak için kullanılacak `map.data.setStyle` yönteminde tek bir geri çağırma işlevi belirtilebilir.

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

GeoJSON, Azure Maps 'ta temel veri türüdür ve `datasource.importFromUrl` yöntemi kullanılarak kolayca bir veri kaynağına aktarılabilir. Kabarcık katmanı, bir veri kaynağındaki özelliklerin özelliklerine göre ölçeklendirilen daireleri işlemek için işlevsellik sağlar. Bir geri çağırma işlevi olması yerine, iş mantığı bir ifadeye dönüştürülür ve stil seçeneklerine geçirilir. İfadeler, iş mantığının başka bir iş parçacığına geçirilmesi ve özellik verilerine göre değerlendirilmesi için nasıl çalıştığını tanımlar. Birden çok veri kaynağı ve katmanı, her biri farklı iş mantığı olan Azure Maps 'a eklenebilir ve bu sayede birden fazla veri kümesinin haritada farklı yollarla işlenmesine izin verilir.

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

![Azure Maps GeoJSON](media/migrate-google-maps-web-app/azure-maps-geojson.png)</center>

**Ek kaynaklar:**

- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)

### <a name="marker-clustering"></a>İşaretleyici Kümelemesi

Haritadaki çok sayıda veri noktasını görselleştirirken, eşleme bozulur ve kullanımı zor olur. Nokta verisi Kümelemesi, bu kullanıcı deneyimini geliştirmek ve ayrıca performansı geliştirmek için kullanılabilir. Kümeleme noktası verileri, birbirine yakın olan nokta verisini birleştirme ve bunları haritada tek bir kümelenmiş veri noktası olarak temsil etme işlemidir. Kullanıcı haritada yakınlaştırdıktan sonra, kümeler kendi bireysel veri noktalarıyla kesilir.

Aşağıdaki örneklerde, eski haftadan gelen deprem verilerinin coğrafi JSON akışı yüklenir ve haritaya eklenir. Kümeler, içerdikleri noktaların sayısına bağlı olarak ölçekli ve renkli daireler olarak işlenir.

> [!NOTE]
> İşaretleyici Kümelemesi için kullanılan birkaç farklı algoritma vardır. Google ve Azure Maps biraz farklı algoritmalar kullanır. Bu nedenle, bazen kümelerdeki nokta dağılımı farklılık gösterebilir.

**Önce: Google Maps**

' Deki Google Maps işaretçileri, MarkerClusterer kitaplığına yüklenirken kümelenebilir. Küme simgeleri, adları beş ' a kadar olan ve aynı dizinde barındırılan görüntüler olacak şekilde sınırlandırılmıştır.

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

Azure haritalar 'da veriler bir veri kaynağı tarafından eklenir ve yönetilir. Katmanlar veri kaynaklarına bağlanır ve verileri bunlarda işler. Azure haritalar 'daki `DataSource` sınıfı çeşitli kümeleme seçenekleri sağlar.

- `cluster`: veri kaynağını küme noktası verilerine bildirir.
- `clusterRadius`-yarıçap, noktaların birlikte kümelemek için piksel cinsinden.
- `clusterMaxZoom`-kümelemenin gerçekleştiği en yüksek yakınlaştırma düzeyi. Bundan daha fazlasını yakınlaştırırsanız, tüm noktaları semboller olarak işlenir.
- `clusterProperties`-her küme içindeki tüm noktalara karşı ifadeler kullanılarak hesaplanan ve her küme noktasının özelliklerine eklenen özel özellikleri tanımlar.

Kümeleme etkinleştirildiğinde veri kaynağı, işleme için katmanlara kümelenmiş ve kümelenmemiş veri noktaları gönderir. Veri kaynağı yüzlerce binlerce veri noktası kümelemesine sahiptir. Kümelenmiş bir veri noktası üzerinde aşağıdaki özelliklere sahiptir:

| Özellik adı             | Tür    | Açıklama   |
|---------------------------|---------|---------------|
| `cluster`                 | boole | Özelliğin bir kümeyi temsil ettiğini belirtir. |
| `cluster_id`              | string  | Veri kaynağı `getClusterExpansionZoom`, `getClusterChildren`ve `getClusterLeaves` yöntemlerle kullanılabilecek benzersiz bir KIMLIK. |
| `point_count`             | number  | Kümenin içerdiği noktaların sayısı.  |
| `point_count_abbreviated` | string  | Long ise `point_count` değeri abbreviates bir dize. (örneğin, 4.000 4K olur)  |

`DataSource` sınıfı, `cluster_id`kullanarak bir kümeyle ilgili ek bilgilere erişmek için aşağıdaki yardımcı işleve sahiptir.

| Yöntem | Dönüş türü | Açıklama |
|--------|-------------|-------------|
| `getClusterChildren(clusterId: number)` | Promise&lt;dizi&lt;özelliği&lt;geometrisi,&gt; \| şekil&gt;&gt; | Sonraki yakınlaştırma düzeyinde verilen kümenin alt öğelerini alır. Bu alt öğeler şekil ve alt kümelerin bir birleşimi olabilir. Alt kümeler, ClusteredProperties ile eşleşen özelliklerle özellik olacaktır. |
| `getClusterExpansionZoom(clusterId: number)` | Promise&lt;numarası&gt; | Kümenin genişlemekte veya parçalanmasına başlayacağı yakınlaştırma düzeyini hesaplar. |
| `getClusterLeaves(clusterId: number, limit: number, offset: number)` | Promise&lt;dizi&lt;özelliği&lt;geometrisi,&gt; \| şekil&gt;&gt; | Kümedeki tüm noktaları alır. `limit` noktaların bir alt kümesini döndürecek şekilde ayarlayın ve `offset` için noktaları kullanın. |

Haritada kümelenmiş verileri işlerken genellikle iki veya daha fazla katmanın kullanılması en kolay yoldur. Aşağıdaki örnek üç katman, küme boyutunu, küme boyutunu metin olarak işlemek için bir sembol katmanını ve kümelenmemiş noktaları işlemek için ikinci bir sembol katmanını çizmek için bir kabarcık katmanı kullanır. [Küme noktası veri](clustering-point-data-web-sdk.md) belgelerinde vurgulanan Azure haritalar 'da kümelenmiş verileri işlemenin birçok farklı yolu vardır.

GeoJSON verileri, `DataSource` sınıfında `importDataFromUrl` işlevi kullanılarak Azure Maps 'ta doğrudan içeri aktarılabilir.

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

Azure Maps Kümelemesi ![](media/migrate-google-maps-web-app/azure-maps-clustering.png)</center>

**Ek kaynaklar:**

- [Sembol katmanı ekleme](map-add-pin.md)
- [Kabarcık katmanı ekleme](map-add-bubble-layer.md)
- [Küme noktası verileri](clustering-point-data-web-sdk.md)
- [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)

### <a name="add-a-heat-map"></a>Isı haritası ekleme

Nokta yoğunluğu haritaları olarak da bilinen ısı haritaları, bir dizi renk kullanan verilerin yoğunluğunu temsil etmek için kullanılan bir veri görselleştirmesi türüdür. Genellikle bir haritada "etkin noktalar" verilerini göstermek için kullanılır ve büyük nokta veri kümelerini işlemek için harika bir yoldur.

Aşağıdaki örneklerde, tüm deprem bir coğrafi JSON akışını USGS 'den geçen aya yükler ve `"mag"` özelliğinin ağırlık olarak kullanıldığı ağırlıklı bir ısı haritası olarak işler.

**Önce: Google Maps**

Google Maps 'ta, bir ısı haritası oluşturmak için, API komut dosyası URL 'sine `&libraries=visualization` eklenerek "görselleştirme" kitaplığının yüklenmesi gerekir. Google Maps 'taki ısı haritası katmanı doğrudan GeoJSON verilerini desteklemez ve bunun yerine verilerin önce indirililmesi ve ağırlıklı veri noktaları dizisine dönüştürülmesi gerekir.

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

Google Maps ısı haritasını ![](media/migrate-google-maps-web-app/google-maps-heatmap.png)</center>

**Sonrasında: Azure Maps**

Azure Maps ' de GeoJSON verilerini bir veri kaynağına yükleyin ve veri kaynağını bir ısı haritası katmanına bağlayın. Ağırlığa yönelik olarak kullanılacak özellik, bir ifade kullanılarak `weight` seçeneğine geçirilebilir. GeoJSON verileri, `DataSource` sınıfında `importDataFromUrl` işlevi kullanılarak Azure Maps 'ta doğrudan içeri aktarılabilir.

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

Azure haritalar ısı haritasını ![](media/migrate-google-maps-web-app/azure-maps-heatmap.png)</center>

**Ek kaynaklar:**

- [Isı haritası katmanı ekleme](map-add-heat-map-layer.md)
- [Isı haritası katman sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.heatmaplayer?view=azure-iot-typescript-latest)
- [Isı haritası katman seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)
- [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)

### <a name="overlay-a-tile-layer"></a>Döşeme katmanını kaplama

Google Maps 'ta görüntü Yerpaylaşımları olarak da bilinen döşeme katmanları, haritalar döşeme sistemiyle hizalamış, daha küçük döşeli görüntülerle bölünmüş büyük görüntülerin yerdeğiştirmesine olanak tanır. Bu, büyük görüntüleri veya çok büyük veri kümelerini fazla kullanmanın yaygın bir yoludur.

Aşağıdaki örnekler, Iowa çevresel Mesonet of Iowa çevre bir hava durumu radar kutucuk katmanını kaplamış.

**Önce: Google Maps**

Google Maps 'ta, döşeme katmanları `google.maps.ImageMapType` sınıfı kullanılarak oluşturulabilir.

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

Azure haritalar 'da, bir döşeme katmanı, diğer tüm katmanla aynı şekilde haritaya eklenebilir. X, y, zoom yertutucuları olan biçimli bir URL; `{x}`, `{y}`, sırasıyla `{z}`, kutucuklara nerede erişebileceğini bildirmek için kullanılır. Azure haritalar kutucuk katmanları `{quadkey}`, `{bbox-epsg-3857}` ve `{subdomain}` yer tutucuları de destekler.

> [!TIP]
> Azure haritalar katmanları, temel harita katmanları dahil olmak üzere diğer katmanların altına kolayca işlenebilirler. Genellikle, daha kolay okunabilmeleri için harita etiketlerinin altında döşeme katmanlarını işlemek tercih edilir. `map.layers.add` yöntemi ikinci bir parametre alır. Bu, aşağıdaki yeni katmanın ekleneceği katmanın kimliğidir. Harita etiketlerinin altına bir kutucuk katmanı eklemek için aşağıdaki kod kullanılabilir: `map.layers.add(myTileLayer, "labels");`

```javascript
//Create a tile layer and add it to the map below the label layer.
map.layers.add(new atlas.layer.TileLayer({
    tileUrl: 'https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png',
    opacity: 0.8,
    tileSize: 256
}), 'labels');
```

<center>

Azure haritalar kutucuk katmanını ![](media/migrate-google-maps-web-app/azure-maps-tile-layer.png)</center>

> [!TIP]
> Kutucuk istekleri, eşlemenin `transformRequest` seçeneği kullanılarak yakalanabilir. Bu, istenirse istekleri değiştirmenize veya üst bilgileri eklemenize olanak tanır.

**Ek kaynaklar:**

- [Döşeme katmanları ekleme](map-add-tile-layer.md)
- [Döşeme katmanı sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)
- [Döşeme katmanı seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest)

### <a name="show-traffic"></a>Trafiği göster

Trafik verileri hem Azure hem de Google Maps 'ta yer alabilir.

**Önce: Google Maps**

Google Maps 'ta trafik verileri, trafik katmanını kullanarak haritada yer alabilir.

```javascript
var trafficLayer = new google.maps.TrafficLayer();
trafficLayer.setMap(map);
```

<center>

Google Maps trafiğini ![](media/migrate-google-maps-web-app/google-maps-traffic.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar, trafiği görüntülemek için çeşitli farklı seçenekler sağlar. Yol kapanışları ve kazalardan dolayı gibi trafik olayları haritada simgeler olarak görüntülenebilir. Trafik akışı, renk kodlu yollar, haritada bulunabilir ve renkler, normal beklenen gecikmeye veya mutlak gecikmeye göre, postalanan hız sınırına göre temel olacak şekilde değiştirilebilir. Azure haritalar 'daki olay verileri her dakikada ve akış verilerinde iki dakikada bir güncelleştirilir.

```javascript
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

<center>

Azure haritalar trafiğini ![](media/migrate-google-maps-web-app/azure-maps-traffic.png)</center>

Azure haritalar 'daki trafik simgelerinden birine tıklarsanız, açılan pencerede ek bilgiler görüntülenir.

<center>

![Azure Maps Trafik olayı](media/migrate-google-maps-web-app/azure-maps-traffic-incident.png)</center>

**Ek kaynaklar:**

- [Haritada trafiği göster](map-show-traffic.md)
- [Trafik kaplama seçenekleri](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Traffic%20Overlay%20Options)

### <a name="add-a-ground-overlay"></a>Zemin kaplama ekleme

Hem Azure hem de Google Maps, Haritada kaydırma ve yakınlaştırma sırasında taşınmaları ve ölçeklendirilmesi için, haritada coğrafi olarak başvurulan görüntülerin üzerine yerleşimi destekler. Google Maps 'ta Bunlar, Azure haritalar 'da, görüntü katmanları olarak adlandırıldıkları sırada yukarı biçimlendirmeler olarak bilinir. Bunlar, kat planları oluşturmak, eski haritaları yerleştirmek veya bir DRA 'dan Imagery için harika.

**Önce: Google Maps**

Google Maps 'ta bir zemin kaplama oluştururken görüntünün URL 'sini ve haritada bağlanacağı bir sınırlayıcı kutuyu belirtmeniz gerekir. Bu örnek, haritadaki 1922 ' [dan yeni Jersey](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) 'in bir harita görüntüsünü yerdekiler.

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

![Google Maps görüntü kaplama](media/migrate-google-maps-web-app/google-maps-image-overlay.png)</center>

**Sonrasında: Azure Maps**

Azure haritalar 'da, coğrafi olarak başvurulan görüntüler `atlas.layer.ImageLayer` sınıfı kullanılarak kaplama olabilir. Bu sınıf, görüntünün dört köşesi için bir görüntünün URL 'sini ve bir koordinat kümesini gerektirir. Görüntünün aynı etki alanında barındırılması ya da CORs 'nin etkin olması gerekir.

> [!TIP]
> Yalnızca Kuzey, Güney, Doğu, Batı ve döndürme bilgilerine sahipseniz ve görüntünün her köşesi için koordinatları yoksa, statik [`atlas.layer.ImageLayer.getCoordinatesFromEdges`](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest#getcoordinatesfromedges-number--number--number--number--number-) yöntemini kullanabilirsiniz.

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

![Azure Maps görüntü kaplama](media/migrate-google-maps-web-app/azure-maps-image-overlay.png)</center>

**Ek kaynaklar:**

- [Görüntü kaplama](map-add-image-layer.md)
- [Görüntü katmanı sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

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
- [A 'dan B 'ye yönlendirmeler göster](map-route.md)
- [JQuery Kullanıcı arabirimi ile otomatik öneri ara](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Search%20Autosuggest%20and%20JQuery%20UI)

## <a name="google-maps-v3-to-azure-maps-web-sdk-class-mapping"></a>Google, v3 'yi Azure Maps web SDK sınıf eşlemesi ile eşleştirir

Aşağıdaki ekte, Google Maps v3 'de Azure Maps web SDK eşdeğerlerine kadar en yaygın olarak kullanılan sınıfların çapraz başvuru eşlemesi sağlanmaktadır.

### <a name="core-classes"></a>Çekirdek sınıflar

| Google Haritalar   | Azure Haritalar  |
|---------------|-------------|
| `google.maps.Map` | [Atlas. Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)  |
| `google.maps.InfoWindow` | [Atlas. Kutu](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)  |
| `google.maps.InfoWindowOptions` | [Atlas. PopupOptions](https://docs.microsoft.com/) |
| `google.maps.LatLng`  | [Atlas. Data. Position](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest)  |
| `google.maps.LatLngBounds` | [Atlas. Data. BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) |
| `google.maps.MapOptions`  | [Atlas. CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraoptions?view=azure-iot-typescript-latest)<br/>[Atlas. CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraboundsoptions?view=azure-iot-typescript-latest)<br/>[Atlas. ServiceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.serviceoptions?view=azure-iot-typescript-latest)<br/>[Atlas. StyleOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Userınteractionoptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.userinteractionoptions?view=azure-iot-typescript-latest) |
| `google.maps.Point`  | [Atlas. Pikselden](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.pixel?view=azure-iot-typescript-latest)   |

## <a name="overlay-classes"></a>Sınıf kaplama

| Google Haritalar  | Azure Haritalar  |
|--------------|-------------|
| `google.maps.Marker` | [Atlas. Htmlişaretleyici](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)<br/>[Atlas. Data. Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest)  |
| `google.maps.MarkerOptions`  | [Atlas. HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)<br/>[Atlas. Layer. SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)<br/>[Atlas. SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)<br/>[Atlas. IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)<br/>[Atlas. TextOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Layer. BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)<br/>[Atlas. BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest) |
| `google.maps.Polygon`  | [Atlas. Data. Çokgen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)               |
| `google.maps.PolygonOptions` |[Atlas. Layer. PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)<br/> [Atlas. PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)<br/> [Atlas. Layer. LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest)<br/> [Atlas. LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)|
| `google.maps.Polyline` | [Atlas. Data. LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest)         |
| `google.maps.PolylineOptions` | [Atlas. Layer. LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-maps-typescript-latest)<br/>[Atlas. LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-maps-typescript-latest) |
| `google.maps.Circle`  | Bkz [. haritaya daire ekleme](map-add-shape.md#add-a-circle-to-the-map)                                     |
| `google.maps.ImageMapType`  | [Atlas. TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)         |
| `google.maps.ImageMapTypeOptions` | [Atlas. TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest) |
| `google.maps.GroundOverlay`  | [Atlas. Layer. ımagelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)<br/>[Atlas. Imagelayeroptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest) |

## <a name="service-classes"></a>Hizmet sınıfları

Azure Haritalar Web SDK 'Sı Ayrıca yüklenebilen bir [hizmet modülünü](how-to-use-services-module.md) içerir. Bu modül, Azure Maps REST hizmetlerini bir Web API 'siyle sarmalayan ve JavaScript, TypeScript ve Node. js uygulamalarında kullanılabilir.

| Google Haritalar | Azure Haritalar  |
|-------------|-------------|
| `google.maps.Geocoder` | [Atlas. Service. SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |
| `google.maps.GeocoderRequest`  | [Atlas. SearchAddressOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressoptions?view=azure-iot-typescript-latest)<br/>[Atlas. SearchAddressRevrseOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreverseoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Searchaddresssmarcrossstreetoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreversecrossstreetoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Searchaddressstructureınfo](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressstructuredoptions?view=azure-iot-typescript-latest)<br/>[Atlas. SearchAlongRouteOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchalongrouteoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Searchbelirsizlik Zyoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchfuzzyoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Searchınsidegeometrik Yoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchinsidegeometryoptions?view=azure-iot-typescript-latest)<br/>[Atlas. Searchyaklaştığında Byoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchnearbyoptions?view=azure-iot-typescript-latest)<br/>[Atlas. SearchPOIOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoioptions?view=azure-iot-typescript-latest)<br/>[Atlas. SearchPOICategoryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoicategoryoptions?view=azure-iot-typescript-latest) |
| `google.maps.DirectionsService`  | [Atlas. Service. RouteUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest)  |
| `google.maps.DirectionsRequest`  | [Atlas. Hesaplakaldırma Teroutedirectionsoptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.calculateroutedirectionsoptions?view=azure-iot-typescript-latest) |
| `google.maps.places.PlacesService` | [Atlas. Service. SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |

## <a name="libraries"></a>Kitaplıklar

Kitaplıklar haritaya ek işlevsellik ekler. Bunların birçoğu Azure haritalar 'ın temel SDK 'nda bulunur. Bu Google Maps kitaplıklarının yerine kullanılacak bazı eşdeğer sınıflar şunlardır

| Google Haritalar           | Azure Haritalar   |
|-----------------------|--------------|
| Çizim kitaplığı       | [Çizim araçları modülü](set-drawing-options.md) |
| Geometri kitaplığı      | [Atlas. matematik](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)   |
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

