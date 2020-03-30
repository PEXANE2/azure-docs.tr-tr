---
title: Haritada bir noktaya açılır pencere ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak bir noktaya açılır pencere eklemeyi öğreneceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 02/27/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: cf6424d2a6cbcfb7c5052201b5a9190c81fddaff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055943"
---
# <a name="add-a-popup-to-the-map"></a>Haritaya açılır pencere ekleme

Bu makalede, bir harita üzerinde bir noktaya pop-up eklemek nasıl gösterir.

## <a name="understand-the-code"></a>Kodu anlama

Aşağıdaki kod, bir sembol katmanı `name` `description` kullanarak haritaya sahip ve özellikleri olan bir nokta özelliği ekler. [Pop-up sınıfının](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup) bir örneği oluşturulur, ancak görüntülenmez. Açılan pencerenin açılmasını ve kapatılmasını tetiklemek için sembol katmanına fare olayları eklenir. İşaretçi simgesi havada gezinildiğinde, açılır `position` pencerenin özelliği işaretçinin konumuyla `content` güncelleştirilir ve seçenek, gezinme `name` `description` noktası özelliğinin ve özelliklerinin ve sarar'ının bazı HTML'si ile güncelleştirilir. Açılır pencere daha sonra `open` işlevini kullanarak haritada görüntülenir.

```javascript
//Define an HTML template for a custom popup content laypout.
var popupTemplate = '<div class="customInfobox"><div class="name">{name}</div>{description}</div>';

//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

dataSource.add(new atlas.data.Feature(new atlas.data.Point([-122.1333, 47.63]), {
  name: 'Microsoft Building 41', 
  description: '15571 NE 31st St, Redmond, WA 98052'
}));

//Create a layer to render point data.
var symbolLayer = new atlas.layer.SymbolLayer(dataSource);

//Add the polygon and line the symbol layer to the map.
map.layers.add(symbolLayer);

//Create a popup but leave it closed so we can update it and display it later.
popup = new atlas.Popup({
  pixelOffset: [0, -18],
  closeButton: false
});

//Add a hover event to the symbol layer.
map.events.add('mouseover', symbolLayer, function (e) {
  //Make sure that the point exists.
  if (e.shapes && e.shapes.length > 0) {
    var content, coordinate;
    var properties = e.shapes[0].getProperties();
    content = popupTemplate.replace(/{name}/g, properties.name).replace(/{description}/g, properties.description);
    coordinate = e.shapes[0].getCoordinates();

    popup.setOptions({
      //Update the content of the popup.
      content: content,

      //Update the popup's position with the symbol's coordinate.
      position: coordinate

    });
    //Open the popup.
    popup.open(map);
  }
});

map.events.add('mouseleave', symbolLayer, function (){
  popup.close();
});
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Azure Haritalar'ı kullanarak açılır pencere ekleme' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Kalem <a href='https://codepen.io'>CodePen'de</a> <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Azure Haritalar 'a</a> göre Azure Haritalar 'ı kullanarak açılır pencere ekleme (<a href='https://codepen.io/azuremaps'>@azuremaps</a>)
</iframe>

## <a name="reusing-a-popup-with-multiple-points"></a>Birden çok noktayla açılır pencereyi yeniden kullanma

En iyi yaklaşımın bir açılır pencere oluşturmak ve yeniden kullanmak olduğu durumlar vardır. Örneğin, çok sayıda noktanız olabilir ve aynı anda yalnızca bir açılır pencere göstermek isteyebilirsiniz. Açılır pencereyi yeniden kullanarak, uygulama tarafından oluşturulan DOM öğelerinin sayısı büyük ölçüde azalır ve bu da daha iyi performans sağlayabilir. Aşağıdaki örnek 3 noktalı özellikler oluşturur. Bunlardan herhangi birini tıklattığınızda, bu nokta özelliğiiçin içerikle birlikte bir açılır pencere görüntülenir.

<br/>

<iframe height='500' scrolling='no' title='Birden Çok Pinle Pop-Up'ı Yeniden Kullanma' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>Birden Çok Pinli</a> Açılır Pencereyi Yeniden Kullanarak Kalem 'e bakın (<a href='https://codepen.io/azuremaps'>@azuremaps</a>)
</iframe>

## <a name="customizing-a-popup"></a>Açılır pencereyi özelleştirme

Varsayılan olarak, açılır pencerenin beyaz bir arka planı, alt kısmında bir işaretçi oku ve sağ üst köşede bir kapatma düğmesi vardır. Aşağıdaki örnek, açılır pencere `fillColor` seçeneğini kullanarak arka plan rengini siyaha değiştirir. Kapatma düğmesi `CloseButton` seçeneği false olarak ayarlayarak kaldırılır. Pop-up'ın HTML içeriği, pop-up'ın kenarlarından 10 piksel dolgulu olarak kullanır. Metin beyaz yapılır, bu yüzden güzel siyah arka plan üzerinde gösterir.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Özelleştirilmiş Popup" src="//codepen.io/azuremaps/embed/ymKgdg/?height=500&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
CodePen'de Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/ymKgdg/'>Özelleştirilmiş Açılır</a> Pencere <a href='https://codepen.io'>'ye</a>bakın .
</iframe>

## <a name="add-popup-templates-to-the-map"></a>Haritaya açılır pencere ekleme

Açılır pencereler, açılır pencereler için veri odaklı düzenler oluşturmayı kolaylaştırır. Aşağıdaki bölümler, özelliklerin özelliklerini kullanarak biçimlendirilmiş içerik oluşturmak için çeşitli açılır pencere şablonlarının kullanımını göstermektedir.

> [!NOTE]
> Varsayılan olarak, açılır pencereyi kullanan tüm içerik, güvenlik özelliği olarak bir iframe'in içine sandboxed olacaktır. Ancak, sınırlamalar vardır:
>
> - Tüm komut dosyaları, formlar, işaretçi kilidi ve üst gezinti işlevselliği devre dışı bırakılır. Tıklandığında bağlantıların yeni bir sekmede açılmasına izin verilir. 
> - iframe'lerde `srcdoc` parametreyi desteklemeyen eski tarayıcılar, az miktarda içerik işlemekle sınırlı olacaktır.
> 
> Açılan pencerelere yüklenen verilere güveniyorsanız ve bu komut dosyalarının açılır pencerelere yüklenmesini istiyorsanız, açılır pencere şablonları `sandboxContent` seçeneğini yanlış olarak ayarlayarak bunu devre dışı bırakabilirsiniz. 

### <a name="string-template"></a>Dize şablonu

String şablonu, yer tutucularının yerine özellik özelliklerinin değerlerini verir. Özelliğin özelliklerine String türünde bir değer atanması gerekmez. Örneğin, `value1` bir karşıcı tutar. Bu değerler daha sonra içerik özelliğine `popupTemplate`geçirilir. 

Seçenek, `numberFormat` görüntülenecek sayının biçimini belirtir. Belirtilmemişse, `numberFormat` kod açılır şablonlar tarih biçimini kullanır. `numberFormat` Seçenek, [Number.toLocaleString](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number/toLocaleString) işlevini kullanarak sayıları biçimlendirin. Büyük sayıları biçimlendirmek `numberFormat` [için, NumberFormat.format'taki](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat/format)işlevlerle seçeneği kullanmayı düşünün. Örneğin, aşağıdaki kod parçacığı kesir `maximumFractionDigits` basamak sayısını ikiyle sınırlamak için kullanır.

> [!Note]
> String şablonungörüntüleri işlemesinin tek bir yolu vardır. İlk olarak, String şablonu içinde bir görüntü etiketi olması gerekir. Görüntü etiketine geçirilen değer, görüntünün URL'si olmalıdır. Daha sonra, String şablonu ' nda doğru olarak ayarlanmış olması `isImage` `HyperLinkFormatOptions`gerekir. Seçenek, `isImage` köprünün görüntü için olduğunu ve köprünün görüntü etiketine yüklendiğini belirtir. Köprü tıklatıldığında, görüntü açılır.

```javascript
var templateOptions = {
  content: 'This template uses a string template with placeholders.<br/><br/> - Value 1 = {value1}<br/> - Value 2 = {value2/subValue}<br/> - Array value [2] = {arrayValue/2}',
  numberFormat: {
    maximumFractionDigits: 2
  }
};

var feature = new atlas.data.Feature(new atlas.data.Point([0, 0]), {
    title: 'Template 1 - String template',
    value1: 1.2345678,
    value2: {
        subValue: 'Pizza'
    },
    arrayValue: [3, 4, 5, 6]
});

var popup = new atlas.Popup({
  content: atlas.PopupTemplate.applyTemplate(feature.properties, templateOptions),
  position: feature.geometry.coordinates
});
```

### <a name="propertyinfo-template"></a>PropertyInfo şablonu

PropertyInfo şablonu özelliğin kullanılabilir özelliklerini görüntüler. Seçenek, `label` kullanıcıya görüntülenecek metni belirtir. `label` Belirtilmemişse, köprü görüntülenir. Ve köprü bir görüntüyse, "alt" etiketine atanan değer görüntülenir. Tarih `dateFormat` biçimi belirtir ve tarih biçimi belirtilmemişse, tarih bir dize olarak işlenir. Seçenek, `hyperlinkFormat` tıklanabilir bağlantıları işler, benzer `email` şekilde, seçenek tıklanabilir e-posta adreslerini işlemek için kullanılabilir.

PropertyInfo şablonu özellikleri son kullanıcıya görüntülemeden önce, özelliklerin gerçekten bu özellik için tanımlandığını yineleyen bir şekilde denetler. Ayrıca görüntüleyen stil ve başlık özelliklerini de yoksa. Örneğin, `color`, , `size` `anchor` `strokeOpacity`, , ve `visibility`. Bu nedenle, özellik yolu denetimi arka uçta tamamlandıktan sonra, PropertyInfo şablonu içeriği tablo biçiminde gösterir.

```javascript
var templateOptions = {
  content: [
    {
        propertyPath: 'createDate',
        label: 'Created Date'
    },
    {
        propertyPath: 'dateNumber',
        label: 'Formatted date from number',
        dateFormat: {
          weekday: 'long',
          year: 'numeric',
          month: 'long',
          day: 'numeric',
          timeZone: 'UTC',
          timeZoneName: 'short'
        }
    },
    {
        propertyPath: 'url',
        label: 'Code samples',
        hideLabel: true,
        hyperlinkFormat: {
          lable: 'Go to code samples!',
          target: '_blank'
        }
    },
    {
        propertyPath: 'email',
        label: 'Email us',
        hideLabel: true,
        hyperlinkFormat: {
          target: '_blank',
          scheme: 'mailto:'
        }
    }
  ]
};

var feature = new atlas.data.Feature(new atlas.data.Point([0, 0]), {
    title: 'Template 2 - PropertyInfo',
    createDate: new Date(),
    dateNumber: 1569880860542,
    url: 'https://aka.ms/AzureMapsSamples',
    email: 'info@microsoft.com'
}),

var popup = new atlas.Popup({
  content: atlas.PopupTemplate.applyTemplate(feature.properties, templateOptions),
  position: feature.geometry.coordinates
});
```

### <a name="multiple-content-templates"></a>Birden çok içerik şablonu

Bir özellik, String şablonu ve PropertyInfo şablonunun bir birleşimini kullanarak içeriği de görüntüleyebilir. Bu durumda, String şablonu yer tutucuları beyaz bir arka plan üzerinde değerler işler.  Ayrıca, PropertyInfo şablonu tablonun içinde tam genişlikte bir görüntü oluşturur. Bu örnekteki özellikler önceki örneklerde açıkladığımız özelliklere benzer.

```javascript
var templateOptions = {
  content: [
    'This template has two pieces of content; a string template with placeholders and a array of property info which renders a full width image.<br/><br/> - Value 1 = {value1}<br/> - Value 2 = {value2/subValue}<br/> - Array value [2] = {arrayValue/2}',
    [{
      propertyPath: 'imageLink',
      label: 'Image',
      hideImageLabel: true,
      hyperlinkFormat: {
        isImage: true
      }
    }]
  ],
  numberFormat: {
    maximumFractionDigits: 2
  }
};

var feature = new atlas.data.Feature(new atlas.data.Point([0, 0]), {
    title: 'Template 3 - Multiple content template',
    value1: 1.2345678,
    value2: {
    subValue: 'Pizza'
    },
    arrayValue: [3, 4, 5, 6],
    imageLink: 'https://azuremapscodesamples.azurewebsites.net/common/images/Pike_Market.jpg'
});

var popup = new atlas.Popup({
  content: atlas.PopupTemplate.applyTemplate(feature.properties, templateOptions),
  position: feature.geometry.coordinates
});
```

### <a name="points-without-a-defined-template"></a>Tanımlı bir şablonu olmayan noktalar

Pop-up şablonu String şablonu, PropertyInfo şablonu veya her ikisinin birleşimi olarak tanımlanmadığında varsayılan ayarları kullanır. `title` Yalnızca atanan özellikler olduğunda, açılır pencerede sağ üst köşede bir yakın düğme olan beyaz bir arka plan `description` gösterir. Ve, küçük ve orta ekranlarda, alt kısmında bir ok gösterir. Varsayılan ayarlar, tablo nun dışındaki tüm `title` özellikler `description`için tablonun içinde ve . Varsayılan ayarlara geri dönse bile, açılır pencere yine de programlı olarak değiştirilebilir. Örneğin, kullanıcılar köprü algılamayı kapatabilir ve varsayılan ayarlar diğer özellikler için de geçerli olacaktır.

CodePen'deki haritadaki noktaları tıklatın. Haritada aşağıdaki açılır pencerelerin her biri için bir nokta vardır: String şablonu, PropertyInfo şablonu ve Birden çok içerik şablonu. Ayrıca, şablonların varsayılan ayarları kullanarak nasıl işlettiğini gösteren üç nokta da vardır.

<br/>

<iframe height='500' scrolling='no' title='PopupŞablonlar' src='//codepen.io/azuremaps/embed/dyovrzL/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/dyovrzL/'>Popup Şablonlarına</a> bakın .
</iframe>

## <a name="reuse-popup-template"></a>Açılır pencereyi yeniden kullanma

Açılır pencereyi yeniden kullanmaya benzer şekilde, açılır pencere şablonlarını yeniden kullanabilirsiniz. Bu yaklaşım, birden çok nokta için aynı anda yalnızca bir açılır pencere göstermek istediğinizde yararlıdır. Açılır pencereyi yeniden kullanarak, uygulama tarafından oluşturulan DOM öğelerinin sayısı azalır ve bu da uygulama performansınızı artırır. Aşağıdaki örnekte üç nokta için aynı açılır pencere kullanır. Bunlardan herhangi birini tıklattığınızda, bu nokta özelliğiiçin içerikle birlikte bir açılır pencere görüntülenir.

<br/>

<iframe height='500' scrolling='no' title='Yeniden KullanmaPopupTemplate' src='//codepen.io/azuremaps/embed/WNvjxGw/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/WNvjxGw/'>Yeniden KullanımPopupTemplate</a> ()
</iframe>

## <a name="popup-events"></a>Pop-up etkinlikleri

Açılır pencereler açılabilir, kapatılabilir ve sürüklenebilir. Açılır pencere sınıfı, geliştiricilerin bu olaylara tepki göstermesine yardımcı olacak etkinlikler sağlar. Aşağıdaki örnek, kullanıcı açılırken, kapandığında veya açılır pencereyi sürüklediğinde hangi olayların alev leden olduğunu vurgular. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Pop-up etkinlikleri" src="//codepen.io/azuremaps/embed/BXrpvB/?height=500&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre Kalem <a href='https://codepen.io/azuremaps/pen/BXrpvB/'>Açılır Pencere etkinliklerine</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Açılan Pencere](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup)

> [!div class="nextstepaction"]
> [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions)

> [!div class="nextstepaction"]
> [PopupTemplate](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popuptemplate)

Tam kod örnekleri için aşağıdaki harika makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](./map-add-pin.md)

> [!div class="nextstepaction"]
> [HTML işaretçisi ekleme](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](map-add-shape.md)
