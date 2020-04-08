---
title: Azure Haritalar uzamsal IO modülü nasıl kullanılır | Microsoft Azure Haritaları
description: Azure Haritalar Web SDK tarafından sağlanan Uzamsal IO modüllerini nasıl kullanacağınızı öğrenin. Bu modül, geliştiricilerin uzamsal verileri Azure Haritalar web sdk ile tümleştirmesini kolaylaştırmak için sağlam özellikler sağlar.
author: philmea
ms.author: philmea
ms.date: 02/28/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 1de9dd9721700418b1aa9ba661fc070db1dbedcc
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804648"
---
# <a name="how-to-use-the-azure-maps-spatial-io-module"></a>Azure Haritalar Uzamsal IO modülü nasıl kullanılır?

Azure Haritalar Web SDK, JavaScript veya TypeScript kullanarak uzamsal verileri Azure Haritalar web SDK ile birleştiren **Uzamsal IO modüllerini**sağlar. Bu modüldeki sağlam özellikler, geliştiricilerin şunları

- [Ortak uzamsal veri dosyalarını okuma ve yazma.](spatial-io-read-write-spatial-data.md) Desteklenen dosya biçimleri şunlardır: KML, KMZ, GPX, GeoRSS, GML, GeoJSON ve csv dosyaları uzamsal bilgi içeren sütunlar içeren. Ayrıca Bilinen Metni (WKT) destekler.
- [Açık Jeouzamsal Konsorsiyum (OGC) hizmetlerine bağlanın ve Azure Haritalar web SDK ile tümleştirin. Yer kaplama Web Harita Hizmetleri (WMS) ve Web Harita Döşeme Hizmetleri (WMTS) harita üzerinde katmanlar olarak.](spatial-io-add-ogc-map-layer.md)
- [Web Özellik Hizmeti'ndeki (WFS) sorgu verileri.](spatial-io-connect-wfs-service.md)
- [Stil bilgilerini içeren ve en az kodu kullanarak otomatik olarak işlemelerini sağlayan karmaşık veri kümelerini yerle bir edin.](spatial-io-add-simple-data-layer.md)
- [Kaldıraç yüksek hızlı XML ve sınırlı dosya okuyucu ve yazar sınıfları.](spatial-io-core-operations.md)

Bu kılavuzda, bir web uygulamasında Uzamsal IO modüllerini nasıl entegre edebileceğimizi ve kullanacağımızı öğreneceğiz.

Bu video, Azure Haritalar Web SDK'daki Uzamsal IO modülüne genel bir bakış sağlar.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Easily-integrate-spatial-data-into-the-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0" title="Uzamsal verileri Azure Haritalar'a kolayca entegre edin - Microsoft Channel 9 Video"></iframe>


> [!WARNING]
> Yalnızca güvendiğiniz bir kaynaktan gelen verileri ve hizmetleri kullanın, özellikle de başka bir etki alanından başvuruyorsanız. Uzamsal IO modülü riski en aza indirmek için adımlar atar, ancak en güvenli yaklaşım uygulamanıza herhangi bir danagerous veri ile başlamak için izin vermez. 

## <a name="prerequisites"></a>Ön koşullar

Uzamsal IO modüllerini kullanabilmeniz için [önce bir Azure Haritalar hesabı oluşturmanız](https://docs.microsoft.com/azure/azure-maps/quick-demo-map-app#create-an-account-with-azure-maps) ve hesabınız için birincil abonelik [anahtarını almanız](https://docs.microsoft.com/azure/azure-maps/quick-demo-map-app#get-the-primary-key-for-your-account)gerekir.

## <a name="installing-the-spatial-io-module"></a>Uzamsal IO modülünün yüklenmesi

İki seçenekten birini kullanarak Azure Haritalar uzamsal IO modüllerini yükleyebilirsiniz:

* Azure Haritalar mekansal IO modülü için genel olarak barındırılan Azure CDN. Bu seçenek için, HTML dosyasının `<head>` öğesindeki JavaScript'e bir başvuru eklersiniz.

    ```html
    <script src="https://atlas.microsoft.com/sdk/javascript/spatial/0/atlas-spatial.js"></script>
    ```

* [Azure-haritalar-mekansal-io](https://www.npmjs.com/package/azure-maps-spatial-io) için kaynak kodu yerel olarak yüklenebilir ve ardından uygulamanızla birlikte barındırılabilir. Bu paket, TypeScript tanımlarını da içerir. Bu seçenek için, paketi yüklemek için aşağıdaki komutu kullanın:

    ```sh
    npm install azure-maps-spatial-io
    ```

    Ardından, HTML belgesinin `<head>` öğesinde JavaScript'e bir başvuru ekleyin:

    ```html
    <script src="node_modules/azure-maps-spatial-io/dist/atlas-spatial.min.js"></script>
    ```

## <a name="using-the-spatial-io-module"></a>Uzamsal IO modüllerini kullanma

1. Yeni bir HTML dosyası oluşturun.

2. Azure Haritalar Web SDK'sını yükleyin ve harita denetimini başlatma. Ayrıntılar için [Azure Haritalar harita kontrol](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control) kılavuzuna bakın. Bu adımı bitirdikten sonra, HTML dosyanız şu şekilde görünmelidir:

    ```html
    <!DOCTYPE html>
    <html>

    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.js"></script>

        <script type='text/javascript'>

            var map;

            function GetMap() {
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
                map.events.add('ready', function() {

                    // Write your code here to make sure it runs once the map resources are ready

                });
            }
        </script>
    </head>

    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>

    </html>
    ```

2. Azure Haritalar mekansal IO modüllerini yükleyin. Bu alıştırma için Azure Haritalar mekansal IO modülü için CDN'yi kullanın. Aşağıdaki başvuruyu HTML `<head>` dosyanızın öğesine ekleyin:

    ```html
    <script src="https://atlas.microsoft.com/sdk/javascript/spatial/0/atlas-spatial.js"></script>
    ```

3. Bir `datasource`, başlangıç ve haritaya veri kaynağı ekleyin. Bir `layer`, başlangıç ve harita katmanına veri kaynağı ekleyin. Ardından, hem veri kaynağını hem de katmanı işleyin. Bir sonraki adımda kodun tamamını görmek için aşağı kaydırmadan önce, veri kaynağı ve katman kodu parçacıklarını koymak için en iyi yerleri düşünün. Haritayı programlı bir şekilde kullanmadan önce harita kaynağının hazır olduğunu beklememiz gerektiğini hatırlayın.

    ```javascript
    var datasource, layer;
    ```

    ve

    ```javascript
    //Create a data source and add it to the map.
    datasource = new atlas.source.DataSource();
    map.sources.add(datasource);
    
    //Add a simple data layer for rendering the data.
    layer = new atlas.layer.SimpleDataLayer(datasource);
    map.layers.add(layer);
    ```

4. Hepsini bir araya getirerek, HTML kodunuz aşağıdaki kod gibi görünmelidir. Bu örnek, bir URL'den bir XML dosyasının nasıl okunduğunu gösterir. Ardından, dosyanın özellik verilerini haritaya yükleyin ve görüntüleyin. 

    ```html
    <!DOCTYPE html>
    <html>

    <head>
        <title>Spatial IO Module Example</title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.js"></script>

        <!-- Add reference to the Azure Maps Spatial IO module. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/spatial/0/atlas-spatial.js"></script>

        <script type='text/javascript'>
            var map, datasource, layer;

            function GetMap() {
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
                map.events.add('ready', function() {

                    //Create a data source and add it to the map.
                    datasource = new atlas.source.DataSource();
                    map.sources.add(datasource);

                    //Add a simple data layer for rendering the data.
                    layer = new atlas.layer.SimpleDataLayer(datasource);
                    map.layers.add(layer);

                    //Read an XML file from a URL or pass in a raw XML string.
                    atlas.io.read('superCoolKmlFile.xml').then(r => {
                        if (r) {
                            //Add the feature data to the data source.
                            datasource.add(r);

                            //If bounding box information is known for data, set the map view to it.
                            if (r.bbox) {
                                map.setCamera({
                                    bounds: r.bbox,
                                    padding: 50
                                });
                            }
                        }
                    });
                });
            }
        </script>
    </head>

    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>

    </html>
    ```

5. Birincil anahtarınızla değiştirmeyi `<Your Azure Maps Key>` unutmayın. HTML dosyanızı açın ve aşağıdaki resme benzer sonuçlar görürsünüz:

    <center>

    ![Uzamsal Veri Örneği](./media/how-to-use-spatial-io-module/spatial-data-example.png)

    </center>

## <a name="next-steps"></a>Sonraki adımlar

Burada gösterdiğimiz özellik, Mekansal IO modülünde bulunan birçok özellikten sadece biridir. Uzamsal IO modülündeki diğer işlevlerin nasıl kullanılacağını öğrenmek için aşağıdaki kılavuzları okuyun:

> [!div class="nextstepaction"]
> [Basit bir veri katmanı ekleme](spatial-io-add-simple-data-layer.md)

> [!div class="nextstepaction"]
> [Uzamsal verileri okuma ve yazma](spatial-io-read-write-spatial-data.md)

> [!div class="nextstepaction"]
> [OGC harita katmanı ekleme](spatial-io-add-ogc-map-layer.md)

> [!div class="nextstepaction"]
> [WFS hizmetine bağlanma](spatial-io-connect-wfs-service.md)

> [!div class="nextstepaction"]
> [Temel operasyonlardan yararlanın](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)

Azure Haritalar Uzamsal IO belgelerine bakın:

> [!div class="nextstepaction"]
> [Azure Haritalar Uzamsal IO paketi](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/)
