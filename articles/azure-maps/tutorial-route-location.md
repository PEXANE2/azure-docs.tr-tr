---
title: 'Öğretici: Microsoft Azure Maps Route Service ve Map Control kullanarak yol yönlerini görüntüleme'
description: Microsoft Azure Maps Route Service ve Map Control kullanarak yol yönlerini görüntülemeyi öğrenin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/01/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc, devx-track-javascript
ms.openlocfilehash: 35a3f6d1e7894eec9baa4ea5432a8e3fec138a21
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90085051"
---
# <a name="tutorial-how-to-display-route-directions-using-azure-maps-route-service-and-map-control"></a>Öğretici: Azure Maps Route Service ve Map Control kullanarak yol yönlerini görüntüleme

Bu öğreticide, başlangıçtan bitiş noktasından yol yönlerini göstermek için Azure Maps [route hizmeti API 'si](https://docs.microsoft.com/rest/api/maps/route) ve [harita denetimi](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control) 'nin nasıl kullanılacağı gösterilmektedir. Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Harita denetimini bir Web sayfasında oluşturun ve görüntüleyin. 
> * [Sembol katmanlarını](map-add-pin.md) ve [çizgi katmanlarını](map-add-line-layer.md)tanımlayarak yolun görüntü işlemesini tanımlayın.
> * Başlangıç ve bitiş noktalarını göstermek için haritaya GeoJSON nesneleri oluşturun ve ekleyin.
> * [Yol yönlerini al API 'sini](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)kullanarak başlangıç ve bitiş noktalarından yol yönleri alın.

Örneğin tam kaynak kodunu [buradan](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)edinebilirsiniz. [Burada](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)canlı bir örnek bulabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

1. [Azure haritalar hesabı oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)
2. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account).

<a id="getcoordinates"></a>

## <a name="create-and-display-the-map-control"></a>Harita denetimi oluşturma ve görüntüleme

Aşağıdaki adımlarda, harita denetiminin bir Web sayfasında nasıl oluşturulacağı ve görüntüleneceği gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapRoute.html** olarak adlandırın.
2. Aşağıdaki HTML işaretlemesini kopyalayıp dosyaya yapıştırın.

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Route</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/service/2/atlas-service.min.js"></script>

        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```

    HTML üstbilgisi, Azure Harita Denetimi Kitaplığı tarafından barındırılan CSS ve JavaScript kaynak dosyalarını içerir. Gövdenin `onload` olayı `GetMap` işlevini çağırır. Sonraki adımda, harita denetimi başlatma kodunu ekleyeceğiz.

3. `GetMap` işlevine aşağıdaki JavaScript kodunu ekleyin. Dizeyi `<Your Azure Maps Key>` haritalar hesabınızdan kopyaladığınız birincil anahtarla değiştirin.

    ```JavaScript
   //Instantiate a map object
   var map = new atlas.Map("myMap", {
        //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
        authOptions: {
           authType: 'subscriptionKey',
           subscriptionKey: '<Your Azure Maps Key>'
        }
   });
   ```

4. Dosyayı kaydedin ve tarayıcınızda açın. Basit bir şekilde görüntülenir.

     :::image type="content" source="./media/tutorial-route-location/basic-map.png" alt-text="Harita denetiminin temel eşleme işlemesi":::

## <a name="define-route-display-rendering"></a>Yol görüntüleme işlemesini tanımla

Bu öğreticide, bir çizgi katmanı kullanarak yolu oluşturacağız. Başlangıç ve bitiş noktaları, bir sembol katmanı kullanılarak işlenir. Çizgi katmanları ekleme hakkında daha fazla bilgi için bkz. [haritaya çizgi katmanı ekleme](map-add-line-layer.md). Sembol katmanları hakkında daha fazla bilgi için bkz. [haritaya sembol katmanı ekleme](map-add-pin.md).

1. İşlevine aşağıdaki JavaScript kodunu ekleyin `GetMap` . Bu kod harita denetiminin `ready` olay işleyicisini uygular. Bu öğreticideki kodun geri kalanı `ready` olay işleyicisinin içine yerleştirilecek.

    ```JavaScript
    //Wait until the map resources are ready.
    map.events.add('ready', function() {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: '#2272B9',
            strokeWidth: 5,
            lineJoin: 'round',
            lineCap: 'round'
        }), 'labels');

        //Add a layer for rendering point data.
        map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: ['get', 'icon'],
                allowOverlap: true
           },
            textOptions: {
                textField: ['get', 'title'],
                offset: [0, 1.2]
            },
            filter: ['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']] //Only render Point or MultiPoints in this layer.
        }));
    });
    ```

    Harita denetiminin `ready` olay işleyicisinde, yolu başlangıçtan bitiş noktasına depolamak için bir veri kaynağı oluşturulur. Rota satırının nasıl işleneceğini tanımlamak için, bir çizgi katmanı oluşturulup veri kaynağına iliştirilir.  Rota satırının yol etiketlerini kapsaiçermediğinden emin olmak için, değerine sahip ikinci bir parametre geçirdik `'labels'` .

    Ardından, bir sembol katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işleneceğini belirtir. Her Point nesnesindeki özelliklerden simge görüntüsünü ve metin etiketi bilgilerini almak için ifadeler eklenmiştir. İfadeler hakkında daha fazla bilgi için bkz. [veri tabanlı stil ifadeleri](data-driven-style-expressions-web-sdk.md).

2. Başlangıç noktasını Microsoft olarak ve son noktası Seattle 'da bir gaz istasyonu olarak ayarlayın.  Harita denetiminin `ready` olay işleyicisine aşağıdaki kodu ekleyin.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end points of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: "Redmond",
        icon: "pin-blue"
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: "Seattle",
        icon: "pin-round-blue"
    });

    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 80
    });
    ```

    Bu kod, daha sonra veri kaynağına eklenen başlangıç ve bitiş noktalarını temsil eden iki [coğrafi JSON noktası nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur. 

    Son kod bloğu, başlangıç ve bitiş noktalarının Enlem ve boylasını kullanarak kamera görünümünü ayarlar. Başlangıç ve bitiş noktaları veri kaynağına eklenir. Başlangıç ve bitiş noktaları için sınırlayıcı kutu, `atlas.data.BoundingBox.fromData` işlevi kullanılarak hesaplanır. Bu sınırlama kutusu, işlevi kullanarak tüm yol üzerinde harita kameralarının görünümünü ayarlamak için kullanılır `map.setCamera` . Sembol simgelerinin piksel boyutlarını dengelemek için doldurma eklenir. Harita denetiminin setCamera özelliği hakkında daha fazla bilgi için bkz. [setcamera (CameraOptions | CameraBoundsOptions & AnimationOptions)](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-maps-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-&preserve-view=false) özelliği.

3. **MapRoute.html** 'yi kaydedin ve tarayıcınızı yenileyin. Eşleme artık Seattle üzerinden ortalanır. Teardrop mavi pin başlangıç noktasını işaretler. Yuvarlak mavi pin bitiş noktasını işaretler.

    :::image type="content" source="./media/tutorial-route-location/map-pins.png" alt-text="Yolların başlangıç ve bitiş noktasını haritada görüntüle":::

<a id="getroute"></a>

## <a name="get-route-directions"></a>Rota yönlerini al

Bu bölümde, yol yönlerini ve bir noktadan diğerine tahmini varış süresini almak için Azure Maps yol yönleri API 'sinin nasıl kullanılacağı gösterilmektedir.

>[!TIP]
>Azure haritalar yol Hizmetleri, *en hızlı*, *en kısa*, *ekonomik*veya *Thrilling* yollar gibi farklı yol türlerine göre yolları, uzaklık, trafik koşullarına ve kullanılan taşıma moduna göre planlamak için API 'ler sunar. Hizmet Ayrıca, kullanıcıların geçmiş trafik koşullarına göre gelecek yolları planlayabilmenizi sağlar. Kullanıcılar, belirli bir süre için yol sürelerinin tahminini tahmin edebilir. Daha fazla bilgi için bkz. [yol yönleri API 'Si edinme](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

1. `GetMap`İşlevinde, denetimin `ready` olay işleyicisinin Içinde, JavaScript koduna aşağıdakini ekleyin.

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   , `SubscriptionKeyCredential` `SubscriptionKeyCredentialPolicy` Azure Maps 'a ABONELIK anahtarıyla http isteklerinin kimliğini doğrulamak için bir oluşturur. , `atlas.service.MapsURL.newPipeline()` `SubscriptionKeyCredential` İlkeyi alır ve bir işlem [hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline) örneği oluşturur. , `routeURL` Azure Maps [yönlendirme](https://docs.microsoft.com/rest/api/maps/route) işlemlerine yönelik bir URL 'yi temsil eder.

2. Kimlik bilgilerini ve URL 'YI ayarladıktan sonra, denetimin olay işleyicisine aşağıdaki kodu ekleyin `ready` . Bu kod, yolu başlangıç noktasından bitiş noktasına oluşturur. `routeURL`Azure haritalar yönlendirme HIZMETI API 'sini rota yönlerini hesaplamak için ister. Yanıttan bir GeoJSON Özellik koleksiyonu daha sonra yöntemi kullanılarak ayıklanır `geojson.getFeatures()` ve veri kaynağına eklenir.

    ```JavaScript
    //Start and end point input to the routeURL
    var coordinates= [[startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]], [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]];

    //Make a search route request
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {
        //Get data features from response
        var data = directions.geojson.getFeatures();
        datasource.add(data);
    });
    ```

3. **MapRoute.html** dosyasını kaydedin ve web tarayıcınızı yenileyin. Harita şimdi yolu başlangıçtan bitiş noktasına görüntülemelidir.

     :::image type="content" source="./media/tutorial-route-location/map-route.png" alt-text="Azure harita denetimi ve yönlendirme hizmeti":::

    Örneğin tam kaynak kodunu [buradan](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)edinebilirsiniz. [Burada](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)canlı bir örnek bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğreticide, seyahat modu veya cargo türü gibi kısıtlamalarla bir rota sorgusunun nasıl oluşturulacağı gösterilmektedir. Daha sonra aynı haritada birden çok yol görüntüleyebilirsiniz.

> [!div class="nextstepaction"]
> [Farklı seyahat modları için yolları bulma](./tutorial-prioritized-routes.md)
