---
title: 'Öğretici: Haritada yakındaki yerleri ara | Microsoft Azure Haritaları'
description: Bu eğitimde, Microsoft Azure Haritalar'ı kullanarak haritada ilgi çekici noktaları nasıl arayacağınızı öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 1/15/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 0b0cb92cd6b4918e28e143178a5cdbbbb19ac9af
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333627"
---
# <a name="tutorial-search-nearby-points-of-interest-using-azure-maps"></a>Öğretici: Azure Haritalar'ı kullanarak yakındaki ilgi çekici noktaları arayın

Bu öğreticide, Azure Haritalar hesabı ayarlama ve sonra Haritalar API’lerini kullanarak ilgi çekici bir noktayı arama işlemleri gösterilmektedir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Haritalar hesabı oluşturma
> * Haritalar hesabınızın birincil anahtarını alma
> * Harita denetimi API’sini kullanarak yeni bir web sayfası oluşturma
> * Haritalar arama hizmetini kullanarak yakınlardaki bir ilgi çekici noktayı bulma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com)oturum açın.

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Azure Maps hesabı oluşturma

Aşağıdaki adımları uygulayarak yeni bir Haritalar hesabı oluşturun:

1. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. *Market’te Ara* kutusuna **Haritalar** yazın.
3. *Sonuçlar* içinden **Haritalar**’ı seçin. Haritanın altında görüntülenen **Oluştur** düğmesine tıklayın.
4. **Haritalar Hesabı Oluştur** sayfasında aşağıdaki değerleri girin:
    * Bu hesap için kullanmak istediğiniz *Abonelik*.
    * Bu hesap için *Kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    * Yeni hesabınıza verilen *Ad*.
    * Bu hesabın *Fiyatlandırma katmanı.*
    * *Lisans*’ı ve *Gizlilik Bildirimi*’ni okuyun ve onay kutusunu işaretleyerek koşulları kabul edin.
    * **Oluştur** düğmesine tıklayın.

![Azure portalında Azure Haritalar hesabı oluşturma](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Hesabınızın birincil anahtarını alma

Haritalar hesabınız başarıyla oluşturulduktan sonra, Haritalar API’lerini sorgulamanıza olanak sağlayan anahtarı alın. Azure Haritalar hizmetlerini ararken hesabınızın birincil anahtarını abonelik anahtarı olarak kullanmanızı öneririz.

1. Portalda Haritalar hesabınızı açın.
2. Ayarlar bölümünde Kimlik **Doğrulama'yı**seçin.
3. **Birincil Anahtar**’ı panonuza kopyalayın. Bu öğreticinin ilerleyen kısmında kullanmak üzere bunu yerel olarak kaydedin.

![Azure portalında Birincil Anahtarı Alın](./media/tutorial-search-location/get-key.png)

Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](how-to-manage-authentication.md)bakın.

<a id="createmap"></a>

## <a name="create-a-new-map"></a>Yeni harita oluşturma

Harita Denetimi API kullanışlı bir istemci kitaplığıdır. Bu API, Haritalar'ı web uygulamanıza kolayca entegre etmenizi sağlar. Çıplak REST servis aramalarının karmaşıklığını gizler ve özelleştirilebilir bileşenlerle üretkenliğinizi artırır. Aşağıdaki adımlarda, Harita Denetimi API’sinin tümleşik olduğu statik bir HTML sayfasının nasıl oluşturulacağı gösterilmektedir.

1. Yerel makinenizde yeni bir dosya oluşturun ve bu dosyayı **MapSearch.html** olarak adlandırın.
2. Aşağıdaki HTML bileşenlerini dosyaya ekleyin:

   ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Search</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/service/2/atlas-service.min.js"></script>

        <script>
        function GetMap(){
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

   HTML üst bilgisinin Azure Harita Denetimi kitaplığı tarafından barındırılan CSS ve JavaScript kaynak dosyalarını içerdiğine dikkat edin. Sayfanın gövdesinde bulunan ve sayfa yüklendiğinde `GetMap` işlevini çağıracak olan `onload` olayına dikkat edin. İşlev, `GetMap` Azure Haritalar API'larına erişmek için satır içinde JavaScript kodunu içerir.

3. HTML dosyasının `GetMap` işlevine aşağıdaki JavaScript kodunu ekleyin. Dizeyi `<Your Azure Maps Key>` Haritalar hesabınızdan kopyaladığınız birincil anahtarla değiştirin.

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

   Bu segment, Azure Haritalar hesap anahtarınız için Harita Denetimi API’sini başlatır. `atlas`API ve ilgili görsel bileşenleri içeren ad alanıdır. `atlas.Map`görsel ve etkileşimli bir web haritası nın denetimini sağlar.

4. Değişikliklerinizi dosyaya kaydedin ve HTML sayfasını bir tarayıcıda açın. Gösterilen harita, hesap anahtarınızı kullanarak arayarak `atlas.Map` yapabileceğiniz en temel haritadır.

   ![Haritayı görüntüleme](./media/tutorial-search-location/basic-map.png)

5. `GetMap` işlevinde haritanın başlatıldığı bölümün altına aşağıdaki JavaScript kodunu ekleyin.

    ```JavaScript
    //Wait until the map resources are ready.
    map.events.add('ready', function() {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering point data.
        var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: 'pin-round-darkblue',
                anchor: 'center',
                allowOverlap: true
            },
            textOptions: {
                anchor: "top"
            }
        });

        map.layers.add(resultLayer);
    });
    ```

   Bu kod segmentinde, harita kaynakları yüklendiğinde ve haritaya erişilmeye hazır olduğunda yayınlanacak bir `ready` olay haritaya eklenir. Eşlemi `ready` olay işleyicisinde, sonuç verilerini depolamak için bir veri kaynağı oluşturulur. Bir simge katmanı oluşturulur ve veri kaynağına eklenir. Bu katman, veri kaynağındaki sonuç verilerinin nasıl işlenmeli olduğunu belirtir. Bu durumda, sonuç, sonuç koordinatı üzerinde ortalanmış koyu mavi yuvarlak pin simgesiyle işlenir ve diğer simgelerin çakışmasını sağlar. Sonuç katmanı harita katmanlarına eklenir.

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Arama özellikleri ekleme

Bu bölümde, haritanızda ilgi çekici bir nokta bulmak için Haritalar [Arama API'sının](https://docs.microsoft.com/rest/api/maps/search) nasıl kullanılacağı gösterilmektedir. Bu, geliştiricilerin adres, ilgi çekici nokta ve diğer coğrafi bilgileri araması için tasarlanmış bir RESTful API’dir. Arama hizmeti, belirtilen bir adrese enlem ve boylam bilgileri atar. Aşağıda açıklanan **Hizmet Modülü**, Haritalar Arama API'si ile konum bulmaya yönelik aramalarda kullanılabilir.

### <a name="service-module"></a>Hizmet Modülü

1. Harita `ready` olay işleyicisinde, aşağıdaki Javascript kodunu ekleyerek arama hizmeti URL'sini oluşturun.

    ```JavaScript
   // Use SubscriptionKeyCredential with a subscription key
   var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

   // Use subscriptionKeyCredential to create a pipeline
   var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

   // Construct the SearchURL object
   var searchURL = new atlas.service.SearchURL(pipeline); 
   ```

   Abonelik `SubscriptionKeyCredential` anahtarıyla `SubscriptionKeyCredentialPolicy` Azure Haritalar'daki HTTP isteklerini doğrulamak için bir araç oluşturur. İlke `atlas.service.MapsURL.newPipeline()` `SubscriptionKeyCredential` alır ve bir [Pipeline](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) örneği oluşturur. Azure `searchURL` Haritalar [Arama](https://docs.microsoft.com/rest/api/maps/search) işlemlerinin URL'sini temsil eder.

2. Ardından arama sorgusunu oluşturmak için aşağıdaki betik bloğunu ekleyin. Bu, Arama Hizmetinin temel arama API'si olan Belirsiz Arama Hizmetini kullanır. Belirsiz Arama Hizmeti adres, yer ve ilgi çekici nokta (POI) gibi çoğu belirsiz girişi işler. Bu kod, sağlanan enlem ve boylambelirtilen yarıçapı içinde yakındaki Benzin İstasyonları arar. Yanıttan bir GeoJSON özellik koleksiyonu daha `geojson.getFeatures()` sonra yöntem kullanılarak ayıklanır ve veri kaynağına eklenir ve bu da veri nin sembol katmanı üzerinden haritaüzerinde işlenmesiyle otomatik olarak sonuçlanır. Betiğin son bölümü haritanın [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak sonuçların sınırlayıcı kutusuna göre harita kamera görünümünü ayarlar.

    ```JavaScript
    var query =  'gasoline-station';
    var radius = 9000;
    var lat = 47.64452336193245;
    var lon = -122.13687658309935;

    searchURL.searchPOI(atlas.service.Aborter.timeout(10000), query, {
        limit: 10,
        lat: lat,
        lon: lon,
        radius: radius
    }).then((results) => {

        // Extract GeoJSON feature collection from the response and add it to the datasource
        var data = results.geojson.getFeatures();
        datasource.add(data);

        // set camera to bounds to show the results
        map.setCamera({
            bounds: data.bbox,
            zoom: 10
        });
    });
    ```

3. **MapSearch.html** dosyasını kaydedin ve tarayıcınızı yenileyin. Seattle merkezli haritayı, bölgedeki benzin istasyonlarının konumları için yuvarlak mavi iğnelerle görmelisiniz.

   ![Arama sonuçlarıyla haritayı görüntüleme](./media/tutorial-search-location/pins-map.png)

4. Tarayıcınıza aşağıdaki HTTP İsteğini girerek, haritanın işlediği ham verileri görebilirsiniz. \<Azure Haritalar Anahtarınız\> değerini birincil anahtarınızla değiştirin.

   ```http
   https://atlas.microsoft.com/search/poi/json?api-version=1.0&query=gasoline%20station&subscription-key=<subscription-key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Bu noktada MapSearch sayfası, belirsiz arama sorgusundan döndürülen ilgi çekici noktaların konumlarını görüntüleyebilir. Şimdi bazı etkileşimli özellikler ve konumlar hakkında daha fazla bilgi ekleyelim.

## <a name="add-interactive-data"></a>Etkileşimli veri ekleme

Şu ana kadar oluşturduğumuz harita, arama sonuçları için yalnızca boylam/enlem verilerine bakar. Ancak, Haritalar Arama hizmetinin döndürdettiği ham JSON, her benzin istasyonu hakkında ek bilgiler içerir. İsim ve sokak adresi de dahil. Etkileşimli açılır kutularla haritada bu verilere yer verebilirsiniz.

1. Bulanık arama hizmetini sorgulamak `ready` için koddan sonra harita olay işleyicisine aşağıdaki kod satırlarını ekleyin. Bu kod, bir Pop-up örneği oluşturur ve sembol katmanına bir fare üzerinde olay ekler.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```

    API, `*atlas.Popup` haritada gerekli konuma sabitlenmiş bir bilgi penceresi sağlar. 

2. Açılan penceredeki sonuç `GetMap` bilgilerinin üzerinde moused göstermek için işlev içinde aşağıdaki kodu ekleyin.

    ```JavaScript
    function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occurred on.

        var p = e.shapes[0].getProperties();
        var position = e.shapes[0].getCoordinates();

        //Create HTML from properties of the selected result.
        var html = `
          <div style="padding:5px">
            <div><b>${p.poi.name}</b></div>
            <div>${p.address.freeformAddress}</div>
            <div>${position[1]}, ${position[0]}</div>
          </div>`;

        //Update the content and position of the popup.
        popup.setPopupOptions({
            content: html,
            position: position
        });

        //Open the popup.
        popup.open(map);
    }
    ```

3. Dosyayı kaydedin ve tarayıcınızı yenileyin. Şimdi tarayıcıdaki haritada, herhangi bir arama raptiyesinin üzerine gelip beklediğinizde bilgi açılan pencereleri gösterilir.

    ![Azure Harita Denetimi ve Arama Hizmeti](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Maps hesabı oluşturma
> * Hesabınızın birincil anahtarını alma
> * Harita Denetimi API’sini kullanarak yeni web sayfası oluşturma
> * Arama Hizmeti’ni kullanarak yakınlardaki istenen konumları bulma

> [!div class="nextstepaction"]
> [Tam kaynak kodunu görüntüleme](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

> [!div class="nextstepaction"]
> [Canlı örneği görüntüle](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

Sonraki öğreticide, iki konum arasındaki bir yolun nasıl görüntüleneceği gösterilmektedir.

> [!div class="nextstepaction"]
> [Bir hedefe yönlendirme](./tutorial-route-location.md)
