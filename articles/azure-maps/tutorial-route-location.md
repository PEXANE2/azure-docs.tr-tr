---
title: 'Öğretici: bir konuma yol bulma | Microsoft Azure haritaları'
description: Bir ilgi noktasına yol bulmayı öğrenin. Bkz. adres koordinatlarını ayarlama ve noktaya yönelik yönergeler için Azure Maps Route hizmetini sorgulama.
author: anastasia-ms
ms.author: v-stharr
ms.date: 01/14/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc, devx-track-javascript
ms.openlocfilehash: 0ff604e920ca3e0708fc21a1cadfe61646f4e30b
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88037584"
---
# <a name="tutorial-route-to-a-point-of-interest-using-azure-maps"></a>Öğretici: Azure haritalar 'ı kullanarak bir ilgi noktasına yönlendirme

Bu öğreticide, Azure Haritalar hesabınız ile Yönlendirme Hizmeti SDK’nızı kullanarak ilgi çekici noktanıza nasıl yol tarifi alabileceğiniz gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Adres koordinatlarını ayarlama
> * İstenen konuma yol tarifi almak için Sorgu Yönlendirme Hizmeti

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce [Hesap oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)' daki yönergeleri Izleyin, S1 fiyatlandırma katmanı ile bir abonelik gerekir. Hesabınız için birincil anahtarı almak üzere [birincil anahtar al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](how-to-manage-authentication.md).

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>Yeni harita oluşturma

Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapRoute.html** olarak adlandırın.
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

    Azure Harita Denetimi API’sinin bir bileşeni olan `atlas.Map`, görsel ve etkileşimli bir web haritası için denetim olanağı sunar.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur.

   ![Temel haritayı görüntüleme](media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Rotanın nasıl işleneceğini tanımlama

Bu öğreticide rota başlangıcı ve bitişi için bir simge ve rota yolu için bir çizgi kullanılarak basit bir rota işlenecektir.

1. Eşlemeyi başlattıktan sonra aşağıdaki JavaScript kodunu ekleyin.

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
    
    Haritalar `ready` olay işleyicisinde, yol satırını ve başlangıç ve bitiş noktalarını depolamak için bir veri kaynağı oluşturulur. Bir çizgi katmanı oluşturulup veri kaynağına eklenir ve rota çizgisinin nasıl işleneceği tanımlanır. Yol satırı, mavi renkli bir gölge olarak işlenir. Bu, beş piksellik bir genişliğe, yuvarlatılmış çizgi birleştirmelere ve büyük harflere sahip olacaktır. Katman haritaya eklenirken bu katmanın harita etiketlerinin altında işlenmesi gerektiğini belirten `'labels'` değerine sahip ikinci bir parametre geçirilir. Bu parametre, rota çizgisinin yol etiketlerini kapatmamasını sağlar. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işleneceğini belirtir. Bu durumda, her bir nokta nesnesindeki özelliklerden simge görüntüsünü ve metin etiketi bilgisini almak için ifadeler eklenmiştir. 
    
2. Bu öğretici için başlangıç noktası olarak Microsoft’u ve bitiş noktası olarak Seattle’da bir benzin istasyonunu ayarlayın. Maps `ready` olay işleyicisine aşağıdaki kodu ekleyin.

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

    Bu kod, yolun başlangıç ve bitiş noktalarını temsil eden iki [coğrafi JSON noktası nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur ve bu noktaları veri kaynağına ekler. Her noktaya `title` ve `icon` özelliği eklenir. Son blok, haritanın [Setcamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak başlangıç ve bitiş noktalarının Enlem ve boylamlarını kullanarak kamera görünümünü ayarlar.

3. **MapRoute.html** dosyasını kaydedin ve tarayıcınızı yenileyin. Artık harita Seattle üzerinden ortalanıyor ve başlangıç noktasını işaret eden mavi PIN 'i ve bitiş noktasını işaret eden yuvarlak mavi PIN 'i görebilirsiniz.

   ![Yolların başlangıç ve bitiş noktasını haritada görüntüle](media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Yol tarifini alma

Bu bölümde, Azure Maps Route hizmeti API 'sinin nasıl kullanılacağı gösterilmektedir. Yönlendirme hizmeti API 'SI, belirli bir başlangıç noktasından bir bitiş noktasına yol bulur. Bu hizmette, iki konum arasında *en hızlı*, *en kısa*, *ekonomik*veya *Thrilling* rotaları planlamak için API 'ler vardır. Bu hizmet ayrıca kullanıcıların Azure 'un kapsamlı geçmiş trafik veritabanını kullanarak gelecek yolları planlayasağlar. Kullanıcılar, seçilen gün ve saatte rota sürelerinin tahminini görebilir. Daha fazla bilgi için bkz. [Yol tariflerini alma](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Eşleme kaynakları erişilmeye başladıktan sonra yüklendiklerinden emin olmak için, aşağıdaki işlevlerin hepsi **Map Ready eventListener içine** eklenmelidir.

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

2. Kimlik bilgilerini ve URL 'YI ayarladıktan sonra, yolu baştan sona noktaya oluşturmak için aşağıdaki JavaScript kodunu ekleyin. `routeURL`Azure haritalar yönlendirme hizmetini yol yönlerini hesaplamak üzere ister. Yanıttan bir GeoJSON Özellik koleksiyonu daha sonra yöntemi kullanılarak ayıklanır `geojson.getFeatures()` ve veri kaynağına eklenir.

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

3. **MapRoute.html** dosyasını kaydedin ve web tarayıcınızı yenileyin. Haritalar API’leriyle başarılı bir şekilde bağlantı kurulması için aşağıdakine benzer bir harita görürsünüz.

    ![Azure Harita Denetimi ve Yönlendirme Hizmeti](./media/tutorial-route-location/map-route.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Adres koordinatlarını ayarlama
> * İlgi çekici noktaya yol tarifi almak için yönlendirme hizmetini sorgulama

> [!div class="nextstepaction"]
> [Tam kaynak kodunu görüntüle](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)

> [!div class="nextstepaction"]
> [Canlı örneği görüntüle](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)

Sonraki öğreticide, seyahat modu veya kargo türü gibi kısıtlamalarla bir yol sorgusu oluşturma ve aynı harita üzerinde birden fazla yolu görüntüleme işlemleri gösterilmektedir.

> [!div class="nextstepaction"]
> [Farklı seyahat modları için yolları bulma](./tutorial-prioritized-routes.md)
