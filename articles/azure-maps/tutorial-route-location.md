---
title: 'Öğretici: Bir konuma giden yolu bulma | Microsoft Azure Haritaları'
description: Bu öğretici, Microsoft Azure Haritalar Yönlendirme Hizmeti'ni kullanarak haritadaki bir konuma (ilgi çekici nokta) rotayı nasıl işleyini gösterir.
author: philmea
ms.author: philmea
ms.date: 01/14/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 98c36176ecd2996e5f735c52017162a076ef4bde
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333770"
---
# <a name="tutorial-route-to-a-point-of-interest-using-azure-maps"></a>Öğretici: Azure Haritalar'ı kullanarak ilgi çekici bir noktaya yönlendirin

Bu öğreticide, Azure Haritalar hesabınız ile Yönlendirme Hizmeti SDK’nızı kullanarak ilgi çekici noktanıza nasıl yol tarifi alabileceğiniz gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Adres koordinatlarını ayarlama
> * İstenen konuma yol tarifi almak için Sorgu Yönlendirme Hizmeti

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden [önce, Hesap Oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps)yönergeleri izleyin, S1 fiyatlandırma katmanına sahip bir aboneliğe ihtiyacınız vardır. Hesabınızın birincil anahtarını almak için [birincil anahtarı almak](quick-demo-map-app.md#get-the-primary-key-for-your-account) için adımları izleyin. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](how-to-manage-authentication.md)bakın.

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

    Azure Harita Denetimi API’sinin bir bileşeni olan `atlas.Map`, görsel ve etkileşimli bir web haritası için denetim olanağı sunar.

4. Dosyayı kaydedin ve tarayıcınızda açın. Bu noktada, daha fazla geliştirebileceğiniz temel bir haritanız olur.

   ![Temel haritayı görüntüleme](media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Rotanın nasıl işleneceğini tanımlama

Bu öğreticide rota başlangıcı ve bitişi için bir simge ve rota yolu için bir çizgi kullanılarak basit bir rota işlenecektir.

1. Haritayı aldıktan sonra aşağıdaki JavaScript kodunu ekleyin.

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
    
    Haritalar `ready` olay işleyicisinde, rota çizgisini ve başlangıç ve bitiş noktalarını depolamak için bir veri kaynağı oluşturulur. Bir çizgi katmanı oluşturulup veri kaynağına eklenir ve rota çizgisinin nasıl işleneceği tanımlanır. Rota hattı mavigüzel bir gölge olarak işlenecektir. Beş piksel genişliğinde, yuvarlak çizgi birleştirir ve üst üste sahip olur. Katman haritaya eklenirken bu katmanın harita etiketlerinin altında işlenmesi gerektiğini belirten `'labels'` değerine sahip ikinci bir parametre geçirilir. Bu parametre, rota çizgisinin yol etiketlerini kapatmamasını sağlar. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, başlangıç ve bitiş noktalarının nasıl işlenir olduğunu belirtir. Bu durumda, simge görüntüsü ve metin etiketi bilgilerini her nokta nesnesindeki özelliklerden almak için ifadeler eklenmiştir. 
    
2. Bu öğretici için başlangıç noktası olarak Microsoft’u ve bitiş noktası olarak Seattle’da bir benzin istasyonunu ayarlayın. Haritalar `ready` olay işleyicisi, aşağıdaki kodu ekleyin.

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

    Bu kod, rotanın başlangıç ve bitiş noktalarını temsil edecek iki [GeoJSON Point nesnesi](https://en.wikipedia.org/wiki/GeoJSON) oluşturur ve noktaları veri kaynağına ekler. Her noktaya `title` ve `icon` özelliği eklenir. Son blok, Harita'nın [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak başlangıç ve bitiş noktalarının enlem ve boylamını kullanarak kamera görünümünü ayarlar.

3. **MapRoute.html** dosyasını kaydedin ve tarayıcınızı yenileyin. Şimdi harita Seattle üzerinde ortalanır ve başlangıç noktasını işaretleme mavi pin ve bitiş noktasını işaretleme yuvarlak mavi pin görebilirsiniz.

   ![Rotaları haritada başlangıç ve bitiş noktasını görüntüleme](media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Yol tarifini alma

Bu bölümde, Azure Haritalar rota hizmeti API'nin nasıl kullanılacağı gösterilmektedir. Rota hizmeti API, rotayı belirli bir başlangıç noktasından bitiş noktasına bulur. Bu hizmet içinde, en *hızlı,* *en kısa,* *eko*veya iki konum arasında *heyecan verici* rotalar planlamak için API'ler vardır. Bu hizmet ayrıca, kullanıcıların Azure'un kapsamlı tarihi trafik veritabanını kullanarak gelecekte rotaplanlamalarına da olanak tanır. Kullanıcılar seçilen gün ve saatte rota sürelerinin tahminini görebilir. Daha fazla bilgi için bkz. [Yol tariflerini alma](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Harita kaynaklarına erişilmeye hazır olduktan sonra yüklendiklerinden emin olmak için aşağıdaki işlevlerin tümü **haritahazır eventListener içinde** eklenmelidir.

1. GetMap işlevinde, JavaScript koduna aşağıdakileri ekleyin.

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   Abonelik `SubscriptionKeyCredential` anahtarıyla `SubscriptionKeyCredentialPolicy` Azure Haritalar'daki HTTP isteklerini doğrulamak için bir araç oluşturur. İlke `atlas.service.MapsURL.newPipeline()` `SubscriptionKeyCredential` alır ve bir [Pipeline](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) örneği oluşturur. Azure `routeURL` Haritalar [Rotası](https://docs.microsoft.com/rest/api/maps/route) işlemlerinin URL'sini temsil eder.

2. Kimlik bilgilerini ve URL'yi ayarladıktan sonra, rotayı baştan sona oluşturmak için aşağıdaki JavaScript kodunu ekleyin. Rota `routeURL` yönergelerini hesaplamak için Azure Haritalar rota hizmeti istekleri. Yanıttan bir GeoJSON özellik koleksiyonu daha `geojson.getFeatures()` sonra yöntem kullanılarak ayıklanır ve veri kaynağına eklenir.

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
> [Tam kaynak kodunu görüntüleme](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)

> [!div class="nextstepaction"]
> [Canlı örneği görüntüle](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)

Sonraki öğreticide, seyahat modu veya kargo türü gibi kısıtlamalarla bir yol sorgusu oluşturma ve aynı harita üzerinde birden fazla yolu görüntüleme işlemleri gösterilmektedir.

> [!div class="nextstepaction"]
> [Farklı seyahat modları için yolları bulma](./tutorial-prioritized-routes.md)
