---
title: 'Öğretici: Seyahat moduna göre birden fazla rota bulun | Microsoft Azure Haritaları'
description: Bu eğitimde, Microsoft Azure Haritalar'ı kullanarak farklı seyahat modları için rotaları nasıl bulacağınızı öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 01/14/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: c7ed2421f468dfbb64c635683a7909b517105bc7
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333800"
---
# <a name="tutorial-find-routes-for-different-modes-of-travel-using-azure-maps"></a>Öğretici: Azure Haritalar'ı kullanarak farklı seyahat modları için rotalar bulma

Bu öğretici, Azure Haritalar hesabınızı ve rota hizmetini nasıl kullanacağınızı gösterir. Rota hizmeti, seyahat modunuzda öncelikli olarak ilgi çekici noktanıza giden yolu bulabilir. Haritanızda biri arabalar, diğeri kamyonlar için olmak üzere iki farklı rota görüntüleyebilirsiniz. Yönlendirme hizmeti, aracın yüksekliği ve ağırlığı nedeniyle veya aracın tehlikeli yük taşıması nedeniyle sınırlamaları dikkate alır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritanızda trafik akışını görselleştirme
> * Seyahat modunu bildiren yol sorguları oluşturma
> * Haritanızda birden fazla yol görüntüleme

## <a name="prerequisites"></a>Ön koşullar
Devam etmeden [önce, hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin ve S1 fiyatlandırma katmanını seçin. Hesabınızın birincil anahtarını almak için [birincil anahtarı almak](quick-demo-map-app.md#get-the-primary-key-for-your-account) için adımları izleyin. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](how-to-manage-authentication.md)bakın.

## <a name="create-a-new-map"></a>Yeni harita oluşturma

Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapTruckRoute.html** olarak adlandırın.
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

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

    HTML üst bilgisinin Azure Harita Denetimi kitaplığı tarafından barındırılan CSS ve JavaScript kaynak dosyalarını içerdiğine dikkat edin. Sayfanın gövdesinde bulunan ve sayfa yüklendiğinde `GetMap` işlevini çağıracak olan `onload` olayına dikkat edin. Bu işlev, Azure Haritalar API’lerine erişime yönelik satır içi JavaScript kodunu içerir.

3. `GetMap` işlevine aşağıdaki JavaScript kodunu ekleyin. Dizeyi `<Your Azure Maps Key>` Haritalar hesabınızdan kopyaladığınız birincil anahtarla değiştirin.

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

    Sınıf, `atlas.Map` görsel ve etkileşimli bir web eşlemi denetimini sağlar ve Azure Eş Denetimi API'sinin bir bileşenidir.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur.

   ![Temel haritayı görüntüleme](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Trafik akışını görselleştirme

1. Haritaya trafik akışı görüntüsünü ekleyin. Haritalar `ready` olayı, haritalar kaynakları yüklenene ve onunla güvenli bir şekilde etkileşime hazır olana kadar bekler.

    ```javascript
    map.events.add("ready", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```

    Harita `ready` olay işleyicisinde, haritadaki trafik akışı `relative`ayarı, serbest akışa göre yolun hızı olan olarak ayarlanır. Bunu trafik hızı olan `absolute` veya serbest akışla farklılık gösteren bağıl hız `relative-delay` olarak da ayarlayabilirsiniz.

2. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızın sayfasını yenileyin. Haritayla etkileşime girip Los Angeles'a yakınlaştırıyorsanız, sokakları geçerli trafik verileriyle birlikte görmeniz gerekir.

   ![Trafiği haritada görüntüleme](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>Rotanın nasıl işleneceğini tanımlama

Bu öğreticide iki rota hesaplanacak ve haritada işlenecektir. Rotalardan biri arabalar, diğeri de tırlar için kullanılabilir durumdaki yolları kullanacaktır. İşlendiğinde, rotanın başlangıcı ve sonu için bir sembol simgesi ve her rota yolu için farklı renkli çizgiler görüntüleriz.

1. Haritayı aldıktan sonra, haritalar `ready` olay işleyicisi aşağıdaki JavaScript kodunu ekleyin.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('ready', function () {

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
    });
    ```
    
    Haritalar `ready` olay işleyicisinde, rota çizgilerini ve başlangıç ve bitiş noktalarını depolamak için bir veri kaynağı oluşturulur. Bir çizgi katmanı oluşturulup veri kaynağına eklenir ve rota çizgisinin nasıl işleneceği tanımlanır. Rota çizgisi özelliğinden çizgi kalınlığını ve rengini almak için ifadeler kullanılır. Katman haritaya eklenirken bu katmanın harita etiketlerinin altında işlenmesi gerektiğini belirten `'labels'` değerine sahip ikinci bir parametre geçirilir. Bu, rota hattının yol etiketlerini kapsamamasını sağlar. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işleneceğini belirtir. Bu durumda, simge görüntüsü ve metin etiketi bilgilerini her nokta nesnesindeki özelliklerden almak için ifadeler eklenmiştir. 
    
2. Bu öğreticide başlangıç noktasını Seattle’daki Fabrikam adlı hayali şirket, varış noktasını ise bir Microsoft ofisi olarak ayarlayın. Haritalar `ready` olay işleyicisi, aşağıdaki kodu ekleyin.

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
    ```

    Bu kod, yolun başlangıç ve bitiş noktalarını temsil eden iki [GeoJSON nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur. Her noktaya `title` ve `icon` özelliği eklenir.

3. Ardından haritaya başlangıç ve bitiş noktalarını sabitlemek için aşağıdaki JavaScript kodunu ekleyin:

    ```JavaScript
    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });
    ```

    Başlangıç ve bitiş noktaları veri kaynağına eklenir. Başlangıç ve bitiş noktaları için sınırlayıcı kutu, `atlas.data.BoundingBox.fromData` işlevi kullanılarak hesaplanır. Bu sınırlayıcı kutu, `map.setCamera` işlevi kullanarak harita kameralarının tüm rota üzerindeki görünümünü ayarlamak için kullanılır. Simgelerin piksel boyutlarını telafi etmek için iç boşluk eklenir.

4. Dosyayı kaydedin ve iğneleri haritanızda görmek için tarayıcınızı yenileyin. Şimdi harita Seattle üzerinde ortalanır. Başlangıç noktasını işaretlemek için yuvarlak mavi pimi ve bitiş noktasını işaretleyen mavi pimi görebilirsiniz.

   ![Başlangıç ve bitiş noktaları ile haritayı görüntüleme](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Ulaşım yöntemine göre önceliklendirilmiş yolları işleme

Bu bölümde, Haritalar rota hizmeti API'sini nasıl kullanacağınızı gösterir. Rota API' si, aktarım modunuzu temel alan belirli bir başlangıç noktasından bitiş noktasına kadar birden çok rota bulmak için kullanılır. Rota hizmeti, *en hızlı,* *en kısa,* *eko*veya *heyecan verici* rotaları planlamak için API'ler sağlar. API'ler yalnızca iki konum arasındaki yolları planlamakla kalmıyor, aynı zamanda mevcut trafik koşullarını da dikkate alır. 

Rota API'si, kullanıcıların Azure'un kapsamlı tarihi trafik veritabanını kullanarak gelecekte rotalar planlamalarına olanak tanır. API, belirli bir gün ve saatiçin rota sürelerini tahmin edebilir. Daha fazla bilgi için bkz. [getRouteDirections](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). 

Harita tamamen yüklendikten sonra yüklendiklerinden emin olmak için aşağıdaki kod bloklarının tümü **harita yük etkinliğiListener'a** eklenmelidir.

1. GetMap işlevinde, Javascript koduna aşağıdakileri ekleyin.

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   Abonelik `SubscriptionKeyCredential` anahtarıyla `SubscriptionKeyCredentialPolicy` Azure Haritalar'daki HTTP isteklerini doğrulamak için bir araç oluşturur. İlke `atlas.service.MapsURL.newPipeline()` `SubscriptionKeyCredential` alır ve bir [Pipeline](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) örneği oluşturur. Azure `routeURL` Haritalar [Rotası](https://docs.microsoft.com/rest/api/maps/route) işlemlerinin URL'sini temsil eder.

2. Kimlik bilgilerini ve URL'yi ayarladıktan sonra, USHazmatClass2 sınıfı kargo taşıyan bir kamyon için baştan sona bir rota oluşturmak ve sonuçları görüntülemek için aşağıdaki JavaScript kodunu ekleyin.

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

    Yukarıdaki bu kod parçacığı, [GetRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-maps-typescript-latest) yöntemi yle Azure Haritalar yönlendirme hizmetini sorgular. Rota çizgisi daha sonra `geojson.getFeatures()` geojson özellik koleksiyonundan yöntem kullanılarak çıkarılan yanıttan ayıklanır. Rota satırı daha sonra veri kaynağına eklenir. 0 dizin, veri kaynağındaki diğer satırlardan önce işlenmesini sağlar. Kamyon rota hesaplaması genellikle bir araç rota hesaplamadaha yavaş olacak gibi bu yapılır. Kamyon rota hattı araç rotasından sonra veri kaynağına eklenirse, bunun üzerinde işleme yapılır. Kamyon rota hattına iki özellik, mavinin güzel bir tonu olan kontur rengi ve dokuz piksel kontur genişliği eklenir.

3. Bir araba için bir rota oluşturmak ve sonuçları görüntülemek için aşağıdaki JavaScript kodunu ekleyin.

    ```JavaScript
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {

        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.  
        datasource.add(routeLine);
    });
    ```

    Yukarıdaki bu kod parçacığı, [GetRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-maps-typescript-latest) yöntemi yle Azure Haritalar yönlendirme hizmetini sorgular. Rota çizgisi daha sonra `geojson.getFeatures()` geojson özellik koleksiyonundan yöntem kullanılarak çıkarılan yanıttan ayıklanır. Rota satırı daha sonra veri kaynağına eklenir. Araç rota hattına iki özellik, mor bir ton olan kontur rengi ve beş piksel kontur genişliği eklenir.  

4. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızı yenileyerek sonucu gözlemleyin. Haritalar API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz.

    ![Azure Yönlendirme Hizmeti ile önceliklendirilen yollar](./media/tutorial-prioritized-routes/prioritized-routes.png)

    Kamyon güzergahı kalın mavi, araba güzergahı ise ince mordur. Araba güzergahı I-90 üzerinden Washington Gölü'nden geçiyor. Tüneller yerleşim alanlarına yakın olduğu için tehlikeli atık kargo sınırlıdır. USHazmatClass2 kargo tipini belirten kamyon güzergahı farklı bir otoyol kullanmaya yönlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritanızda trafik akışını görselleştirme
> * Seyahat modunu bildiren yol sorguları oluşturma
> * Haritanızda birden fazla yol görüntüleme

> [!div class="nextstepaction"]
> [Tam kaynak kodunu görüntüleme](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)

> [!div class="nextstepaction"]
> [Canlı örneği görüntüle](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)

Bir sonraki öğretici, Azure Haritalar'ı kullanarak basit bir mağaza bulucu oluşturma işlemini gösterir.

> [!div class="nextstepaction"]
> [Azure Haritalar'ı kullanarak mağaza bulucu oluşturma](./tutorial-create-store-locator.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)