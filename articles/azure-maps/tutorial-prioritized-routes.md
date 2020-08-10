---
title: 'Öğretici: seyahat moduna göre birden çok yol bulma | Microsoft Azure haritaları'
description: Azure haritalar 'ı kullanarak belirli seyahat modlarına yönelik rotaları ilgi çekici bir şekilde nasıl bulacağınızı öğrenin. Haritalar üzerinde birden çok yol görüntüleme konusuna bakın.
author: anastasia-ms
ms.author: v-stharr
ms.date: 01/14/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc, devx-track-javascript
ms.openlocfilehash: 7d17b9474edef245f7db16f33c72d722f356f712
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88037601"
---
# <a name="tutorial-find-routes-for-different-modes-of-travel-using-azure-maps"></a>Öğretici: Azure Maps kullanarak farklı seyahat modları için rotalar bulma

Bu öğreticide, Azure haritalar hesabınızı ve yönlendirme hizmetini nasıl kullanacağınız gösterilmektedir. Rota hizmeti, seyahat noktanize göre önceliklendirilebilmeniz için rotayı bulabilir. Biri, bir diğeri de bir adet otomobiller için bir tane olmak üzere Haritada iki farklı yol görüntüleyebilirsiniz. Yönlendirme hizmeti, aracın yüksekliği ve ağırlığı nedeniyle ya da araç tehlikeli bir Cargo kullanıyorsa, bu durum dikkate alınmaz. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritanızda trafik akışını görselleştirme
> * Seyahat modunu bildiren yol sorguları oluşturma
> * Haritanızda birden fazla yol görüntüleme

## <a name="prerequisites"></a>Ön koşullar
Devam etmeden önce, [Hesap oluşturma](quick-demo-map-app.md#create-an-azure-maps-account) bölümündeki yönergeleri Izleyin ve S1 fiyatlandırma katmanını seçin. Hesabınız için birincil anahtarı almak üzere [birincil anahtar al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](how-to-manage-authentication.md).

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

    `atlas.Map`Sınıfı, bir görsel ve etkileşimli Web eşlemesi için denetim sağlar ve Azure harıta denetımı API 'sinin bir bileşenidir.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur.

   ![Temel haritayı görüntüleme](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Trafik akışını görselleştirme

1. Haritaya trafik akışı görüntüsünü ekleyin. Haritalar `ready` olayı, haritalar kaynakları yüklenmeden ve güvenle etkileşime geçmeye hazırlanana kadar bekler.

    ```javascript
    map.events.add("ready", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```

    Map `ready` olay işleyicisinde, haritadaki trafik akışı ayarı `relative` , boş akışa göre yolun hızı olan olarak ayarlanır. Bunu trafik hızı olan `absolute` veya serbest akışla farklılık gösteren bağıl hız `relative-delay` olarak da ayarlayabilirsiniz.

2. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızın sayfasını yenileyin. Eşlemle etkileşim kurabilir ve Los Angeles 'ta yakınlaştırırsanız, geçerli trafik verileriyle birlikte bir cadde görmeniz gerekir.

   ![Bir haritadaki trafiği görüntüleme](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>Rotanın nasıl işleneceğini tanımlama

Bu öğreticide iki rota hesaplanacak ve haritada işlenecektir. Rotalardan biri arabalar, diğeri de tırlar için kullanılabilir durumdaki yolları kullanacaktır. İşlendiğinde, yolun başlangıcı ve bitişi için bir sembol simgesi, her yol yolu için de farklı renkli satırlar görüntülenir.

1. Eşlemeyi başlattıktan sonra Maps olay işleyicisine aşağıdaki JavaScript kodunu ekleyin `ready` .

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
    
    Haritalar `ready` olay işleyicisinde, yol çizgilerini ve başlangıç ve bitiş noktalarını depolamak için bir veri kaynağı oluşturulur. Bir çizgi katmanı oluşturulup veri kaynağına eklenir ve rota çizgisinin nasıl işleneceği tanımlanır. Rota çizgisi özelliğinden çizgi kalınlığını ve rengini almak için ifadeler kullanılır. Katman haritaya eklenirken bu katmanın harita etiketlerinin altında işlenmesi gerektiğini belirten `'labels'` değerine sahip ikinci bir parametre geçirilir. Bu, rota satırının yol etiketlerini kapsamamasını sağlar. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işleneceğini belirtir. Bu durumda, her bir nokta nesnesindeki özelliklerden simge görüntüsünü ve metin etiketi bilgisini almak için ifadeler eklenmiştir. 
    
2. Bu öğreticide başlangıç noktasını Seattle’daki Fabrikam adlı hayali şirket, varış noktasını ise bir Microsoft ofisi olarak ayarlayın. Maps `ready` olay işleyicisine aşağıdaki kodu ekleyin.

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

    Başlangıç ve bitiş noktaları veri kaynağına eklenir. Başlangıç ve bitiş noktaları için sınırlayıcı kutu, `atlas.data.BoundingBox.fromData` işlevi kullanılarak hesaplanır. Bu sınırlama kutusu, işlevi kullanarak tüm yol üzerinde harita kameralarının görünümünü ayarlamak için kullanılır `map.setCamera` . Simgelerin piksel boyutlarını telafi etmek için iç boşluk eklenir.

4. Dosyayı kaydedin ve iğneleri haritanızda görmek için tarayıcınızı yenileyin. Artık harita Seattle üzerinden ortalanır. Yuvarlak mavi pin ' i başlangıç noktasını işaretleyip son noktayı işaretleyerek mavi PIN 'i görebilirsiniz.

   ![Başlangıç ve bitiş noktaları ile haritayı görüntüleme](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Ulaşım yöntemine göre önceliklendirilmiş yolları işleme

Bu bölümde, Maps Route Service API 'sinin nasıl kullanılacağı gösterilmektedir. Yönlendirme API 'SI, taşıma modınızdan yola çıkarak belirli bir başlangıç noktasından bitiş noktasına birden çok yol bulmak için kullanılır. Rota hizmeti, *en hızlı*, *en kısa*, *ekonomik*veya *Thrilling* yollarını planlamak için API 'ler sağlar. API 'Leri yalnızca iki konum arasında yönlendirmeler planı yoktur, ancak aynı zamanda geçerli trafik koşullarını da dikkate alırlar. 

Yönlendirme API 'SI, kullanıcıların Azure 'un kapsamlı geçmiş trafik veritabanını kullanarak gelecek yolları planlayasağlar. API, belirli bir gün ve saat için rota sürelerini tahmin edebilir. Daha fazla bilgi için bkz. [getRouteDirections](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). 

Eşleme tamamen yüklendikten sonra yüklendiklerinden emin olmak için, aşağıdaki kod bloklarının hepsi **Map Load eventListener içine** eklenmelidir.

1. GetMap işlevinde, JavaScript koduna aşağıdakini ekleyin.

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   , `SubscriptionKeyCredential` `SubscriptionKeyCredentialPolicy` Azure Maps 'a ABONELIK anahtarıyla http isteklerinin kimliğini doğrulamak için bir oluşturur. , `atlas.service.MapsURL.newPipeline()` `SubscriptionKeyCredential` İlkeyi alır ve bir işlem [hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) örneği oluşturur. , `routeURL` Azure Maps [yönlendirme](https://docs.microsoft.com/rest/api/maps/route) işlemlerine yönelik bir URL 'yi temsil eder.

2. Kimlik bilgilerini ve URL 'yi ayarladıktan sonra, USHazmatClass2 sınıflandırılan kargo 'yu taşıyan bir kamyon için başlangıçtan bitiş noktasına bir yol oluşturmak üzere aşağıdaki JavaScript kodunu ekleyin ve sonuçları görüntüleyin.

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

    Yukarıdaki kod parçacığı, Azure Maps yönlendirme hizmetini [Getrouteyönleri](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-maps-typescript-latest) yöntemiyle sorgular. Daha sonra yol satırı, yöntemi kullanılarak ayıklanan yanıttan coğrafi JSON özelliği koleksiyonundan ayıklanır `geojson.getFeatures()` . Yol satırı daha sonra veri kaynağına eklenir. 0 dizini, veri kaynağındaki diğer satırlardan önce oluşturulmasını sağlar. Bu işlem, kamyon rotası hesaplamasının genellikle bir otomobil rota hesaplamasından daha yavaş olacağı için yapılır. Kamyon rotası satırı, araba yönlendirdikten sonra veri kaynağına eklenirse, üzerine işlenir. Kamyonun yol satırına iki özellik eklenir, iyi bir mavi gölge ve dokuz piksellik bir vuruş genişliği.

3. Bir otomobil için yol oluşturmak ve sonuçları göstermek için aşağıdaki JavaScript kodunu ekleyin.

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

    Yukarıdaki kod parçacığı, Azure Maps yönlendirme hizmetini [Getrouteyönleri](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-maps-typescript-latest) yöntemiyle sorgular. Daha sonra yol satırı, yöntemi kullanılarak ayıklanan yanıttan coğrafi JSON özelliği koleksiyonundan ayıklanır `geojson.getFeatures()` . Yol satırı daha sonra veri kaynağına eklenir. Oto yol satırına iki özellik eklenir, Mor Gölge olan bir kontur rengi ve beş piksellik bir kontur genişliği.  

4. **MapTruckRoute.html** dosyasını kaydedin ve tarayıcınızı yenileyerek sonucu gözlemleyin. Haritalar API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz.

    ![Azure Yönlendirme Hizmeti ile önceliklendirilen yollar](./media/tutorial-prioritized-routes/prioritized-routes.png)

    Kamyonun yolu kalın mavi, araba yolu ise ince mor. Araba rotası,, yöresel alanlarındaki tünellerden geçen I-90 aracılığıyla Gölü üzerinden gider. Tüneller yöresel alanlara yakın olduğu için, tehlikeli çöp kargo kısıtlı olur. USHazmatClass2 kargo türünü belirten kamyon rotası, farklı bir Highway kullanımına yönlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritanızda trafik akışını görselleştirme
> * Seyahat modunu bildiren yol sorguları oluşturma
> * Haritanızda birden fazla yol görüntüleme

> [!div class="nextstepaction"]
> [Tam kaynak kodunu görüntüle](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)

> [!div class="nextstepaction"]
> [Canlı örneği görüntüle](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)

Sonraki öğreticide, Azure Maps kullanarak basit bir depolama Konumlandırıcı oluşturma işlemi gösterilmektedir.

> [!div class="nextstepaction"]
> [Azure haritalar kullanarak bir mağaza Bulucu oluşturma](./tutorial-create-store-locator.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)