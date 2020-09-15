---
title: 'Öğretici: Microsoft Azure Maps ile belirli seyahat modları için yolları bulma ve görüntüleme'
description: Azure haritalar 'ı kullanarak belirli seyahat modlarına yönelik yolları bulma ve görüntüleme hakkında bilgi edinin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/10/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc, devx-track-javascript
ms.openlocfilehash: a5b8dba920db0227c400e62ef7ddaf718d27c78a
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90085068"
---
# <a name="tutorial-find-and-display-routes-for-different-modes-of-travel-using-azure-maps"></a>Öğretici: Azure Maps kullanarak farklı seyahat modları için yolları bulma ve görüntüleme

Bu öğreticide, kargo türü ile hem özel araçlar hem de ticari araçlar (structuralks) için yol yönlerini göstermek üzere Azure Maps [route hizmeti](https://docs.microsoft.com/rest/api/maps/route) ve [harita denetimi](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control) 'nin nasıl kullanılacağı gösterilmektedir `USHazmatClass2` . Ayrıca, bir haritadaki gerçek zamanlı trafik verilerini görselleştirirken size kılavuzluk ederiz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Harita denetimini bir Web sayfasında oluşturma ve görüntüleme
> * Bir haritada gerçek zamanlı trafik verilerini işleme
> * Haritada özel ve ticari araç yolları isteme ve görüntüleme

## <a name="prerequisites"></a>Önkoşullar

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. [Azure haritalar hesabı oluşturun](quick-demo-map-app.md#create-an-azure-maps-account).

3. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account). Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](how-to-manage-authentication.md).

Örneğin tam kaynak kodunu [buradan](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)edinebilirsiniz. [Burada](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)canlı bir örnek bulabilirsiniz.

## <a name="create-a-new-web-page-using-the-map-control-api"></a>Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma

Aşağıdaki adımlarda, harita denetiminin bir Web sayfasında nasıl oluşturulacağı ve görüntüleneceği gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapTruckRoute.html** olarak adlandırın.
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

    :::image type="content" source="./media/tutorial-prioritized-routes/basic-map.png" alt-text="Harita denetiminin temel eşleme işlemesi":::

## <a name="render-real-time-traffic-data-on-a-map"></a>Bir haritada gerçek zamanlı trafik verilerini işleme

1. İşlevine aşağıdaki JavaScript kodunu ekleyin `GetMap` . Bu kod harita denetiminin `ready` olay işleyicisini uygular. Bu öğreticideki kodun geri kalanı `ready` olay işleyicisinin içine yerleştirilecek.

    ```javascript
    map.events.add("ready", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```

    Map `ready` olay işleyicisinde, haritadaki trafik akışı ayarı `relative` , boş akışa göre yolun hızı olan olarak ayarlanır. Daha fazla trafik seçeneği için bkz. [TrafficOptions Interface](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.trafficoptions?view=azure-maps-typescript-latest&preserve-view=false).

2. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızın sayfasını yenileyin. Los Angeles gibi herhangi bir şehirde yakınlaştırırsanız, Streets 'in geçerli trafik akışı verileriyle birlikte göründüğünü görürsünüz.

    :::image type="content" source="./media/tutorial-prioritized-routes/traffic-map.png" alt-text="Bir haritadaki trafiği görüntüleme":::

<a id="queryroutes"></a>

## <a name="define-route-display-rendering"></a>Yol görüntüleme işlemesini tanımla

Bu öğreticide iki rota hesaplanacak ve haritada işlenecektir. İlk yol özel bir araç (otomobil) için hesaplanacak. İkinci yol, bir ticari araç (kamyon) için, sonuçlar arasındaki farkı göstermek üzere hesaplanır. İşlendiğinde, haritada yolun başlangıç ve bitiş noktaları için bir sembol simgesi, her yol yolu için ise farklı renklerle yönlendirme çizgisi geometrileri görüntülenir. Çizgi katmanları ekleme hakkında daha fazla bilgi için bkz. [haritaya çizgi katmanı ekleme](map-add-line-layer.md). Sembol katmanları hakkında daha fazla bilgi için bkz. [haritaya sembol katmanı ekleme](map-add-pin.md).

1. Harita denetiminin `ready` olay işleyicisine aşağıdaki kodu ekleyin.

    ```JavaScript

    //Create a data source and add it to the map.
    datasource = new atlas.source.DataSource();
    map.sources.add(datasource);

    //Add a layer for rendering the route lines and have it render under the map labels.
    map.layers.add(new atlas.layer.LineLayer(datasource, null, {
        strokeColor: ['get', 'strokeColor'],
        strokeWidth: ['get', 'strokeWidth'],
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

    ```

    Harita denetiminin `ready` olay işleyicisinde, yolu baştan sona depolamak için bir veri kaynağı oluşturulur. [İfadeler](data-driven-style-expressions-web-sdk.md) , yönlendirme satırı özelliğindeki özelliklerden çizgi genişliğini ve rengini almak için kullanılır. Rota satırının yol etiketlerini kapsaiçermediğinden emin olmak için, değerine sahip ikinci bir parametre geçirdik `'labels'` .

    Ardından, bir sembol katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işleneceğini belirtir. Her Point nesnesindeki özelliklerden simge görüntüsünü ve metin etiketi bilgilerini almak için ifadeler eklenmiştir. İfadeler hakkında daha fazla bilgi için bkz. [veri tabanlı stil ifadeleri](data-driven-style-expressions-web-sdk.md).

2. Başlangıç noktasını, Istanbul olarak fabrikam adlı kurgusal bir şirket olarak ve bitiş noktasını ise Microsoft Office olarak ayarlayın.  Harita denetiminin `ready` olay işleyicisine aşağıdaki kodu ekleyin.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-round-blue'
    });

    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });

    ```

    Bu kod, daha sonra veri kaynağına eklenen başlangıç ve bitiş noktalarını temsil eden iki [coğrafi JSON noktası nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur.

    Son kod bloğu, başlangıç ve bitiş noktalarının Enlem ve boylasını kullanarak kamera görünümünü ayarlar. Başlangıç ve bitiş noktaları veri kaynağına eklenir. Başlangıç ve bitiş noktaları için sınırlayıcı kutu, `atlas.data.BoundingBox.fromData` işlevi kullanılarak hesaplanır. Bu sınırlama kutusu, işlevi kullanarak tüm yol üzerinde harita kameralarının görünümünü ayarlamak için kullanılır `map.setCamera` . Sembol simgelerinin piksel boyutlarını dengelemek için doldurma eklenir. Harita denetiminin setCamera özelliği hakkında daha fazla bilgi için bkz. [setcamera (CameraOptions | CameraBoundsOptions & AnimationOptions)](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-maps-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-&preserve-view=false) özelliği.

3. **TruckRoute.html** 'yi kaydedin ve tarayıcınızı yenileyin. Eşleme artık Seattle üzerinden ortalanır. Teardrop mavi pin başlangıç noktasını işaretler. Yuvarlak mavi pin bitiş noktasını işaretler.

   :::image type="content" source="./media/tutorial-prioritized-routes/pins-map.png" alt-text="Başlangıç ve bitiş noktaları ile haritayı görüntüleme":::

<a id="multipleroutes"></a>

## <a name="request-and-display-private-and-commercial-vehicle-routes-on-a-map"></a>Haritada özel ve ticari araç yolları isteme ve görüntüleme

Bu bölümde, taşıma modınızdan yola çıkarak bir noktadan diğerine yönlendirmeler almak için Azure Maps Route hizmeti 'nin nasıl kullanılacağı gösterilmektedir. İki taşıma modundan birini kullanacağız: kamyon ve otomobil.

>[!TIP]
>Rota hizmeti, *en hızlı*, *en kısa*, *ekonomik*veya *Thrilling* yollarını uzaklık, trafik koşullarına ve kullanılan taşıma moduna göre planlamak için API 'ler sağlar. Hizmet Ayrıca, kullanıcıların geçmiş trafik koşullarına göre gelecek yolları planlayabilmenizi sağlar. Kullanıcılar, belirli bir süre için yol sürelerinin tahminini tahmin edebilir. Daha fazla bilgi için bkz. [yol yönleri API 'Si edinme](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

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

2. Kimlik bilgilerini ve URL 'YI ayarladıktan sonra, başlangıçtan bitiş noktasına bir kamyon yol yolu oluşturmak için aşağıdaki JavaScript kodunu ekleyin. Bu yol, sınıflı Cargo 'yu taşıyan bir kamyon için oluşturulur ve görüntülenir `USHazmatClass2` .

    ```JavaScript
    //Start and end point input to the routeURL
    var coordinates= [[startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]], [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]];

    //Make a search route request for a truck vehicle type
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates,{
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then((directions) => {
        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```

    Yukarıdaki kod, Azure Maps yol [yönleri API 'si](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl#calculateroutedirections-aborter--geojson-position----calculateroutedirectionsoptions-)aracılığıyla Azure Maps Route hizmetini sorgular. Daha sonra yol satırı, yöntemi kullanılarak ayıklanan yanıttan coğrafi JSON özelliği koleksiyonundan ayıklanır `geojson.getFeatures()` . Son olarak, yol satırı veri kaynağına eklenir. Kamyonun rota hesaplamasının genellikle bir otomobil rota hesaplamasından daha yavaş olması nedeniyle, kamyon yolunun veri kaynağındaki diğer satırlardan önce oluşturulmasını sağlamak için bunu 0 dizinine ekliyoruz. Kamyon rotası satırı, araba yönlendirdikten sonra veri kaynağına eklenirse, üzerine işlenir. Kamyon yol satırına iki özellik eklenir: mavi bir kontur rengi ve dokuz piksellik bir kontur genişliği.

    >[!TIP]
    > Azure haritalar yol yönleri API 'sine yönelik tüm olası seçenekleri ve değerleri görmek için bkz. [yol gönderme yönlendirmeleri Için URI parametreleri](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#uri-parameters).

3. Şimdi bir otomobil yolu oluşturmak için aşağıdaki JavaScript kodunu ekleyin.

    ```JavaScript
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {

        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source. This will add the car route after the truck route.  
        datasource.add(routeLine);
    });
    ```

    Yukarıdaki kod, Azure Maps yönlendirme hizmeti 'ni  [Azure haritalar yol yönleri API 'si](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl#calculateroutedirections-aborter--geojson-position----calculateroutedirectionsoptions-) yöntemiyle sorgular. Daha sonra yol satırı, yöntemi kullanılarak ayıklanan yanıttan coğrafi JSON özelliği koleksiyonundan ayıklanır `geojson.getFeatures()` . Son olarak, yol satırı veri kaynağına eklenir. Kamyon yol satırına iki özellik eklenir: mor vuruş rengi ve beş piksellik kontur genişliği.

4. **TruckRoute.html** dosyasını kaydedin ve Web tarayıcınızı yenileyin. Harita Şimdi kamyon ve araba yollarını görüntülemelidir.

    :::image type="content" source="./media/tutorial-prioritized-routes/prioritized-routes.png" alt-text="Azure Yönlendirme Hizmeti bir haritadaki özel ve ticari araç rotaları":::

    Kamyonun yolu, kalın mavi bir çizgi kullanılarak görüntülenir. Araba yolu ince mor bir çizgi kullanılarak görüntülenir. Araba rotası, u-90 ile Gölü, yöresel alanlarının altındaki tünellerden geçen bir noktadan geçer. Tüneller yöresel alanlara yakın olduğu için, tehlikeli çöp kargo kısıtlı olur. Kargo türünü belirten kamyon rotası, `USHazmatClass2` farklı bir Highway kullanımına yönlendirilir.

    Örneğin tam kaynak kodunu [buradan](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)edinebilirsiniz. [Burada](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)canlı bir örnek bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki öğreticide, Azure Maps kullanarak basit bir depolama Konumlandırıcı oluşturma işlemi gösterilmektedir.

> [!div class="nextstepaction"]
> [Azure haritalar kullanarak bir mağaza Bulucu oluşturma](./tutorial-create-store-locator.md)
