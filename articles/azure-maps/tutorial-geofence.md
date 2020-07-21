---
title: 'Öğretici: Microsoft Azure haritasında bölge alanı oluşturma ve cihazları izleme'
description: Microsoft Azure Maps uzamsal hizmetini kullanarak bölge sınırlaması ile ilişkili cihazları nasıl ayarlayacağınızı ve bu cihazların nasıl izleneceğini öğrenin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 7/15/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 775d98b992f2bca4441c868873ceaeb2389db81a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86517394"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Öğretici: Azure Haritalar’ı kullanarak bölge sınırı ayarlama

Bu öğreticide, aşağıdaki senaryo bağlamında Azure Maps bölge hizmetleri oluşturma ve kullanma hakkında temel bilgiler verilmektedir:

*Bir oluşturma Site Yöneticisi, bir yapı alanının perimetrelerini girip terk ettiğinde ekipmanları izmelidir. Her bir ekipman her çıktığında veya bu perimeters 'a girdiğinde, Operations Manager bir e-posta bildirimi gönderilir.*

Azure haritalar, yukarıdaki senaryoda bulunan oluşturma alanını girip çıkarken ekipmanların izlenmesini desteklemeye yönelik çeşitli hizmetler sunar. Bu öğreticide nasıl yapılacağını ele aldık:

> [!div class="checklist"]
> * İzlemek istediğimiz yapı alanı bölgelerini tanımlayan coğrafi bölge coğrafi [JSON verilerini](geofence-geojson.md) karşıya yükleyin. Azure haritalar hesabınıza Çokgen koordinatları olarak bölge 'leri yüklemek için [veri yükleme API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview) 'sini kullanacağız.
> * Tetiklendiğinde bölge bölgesi alanına girdiğinde ve bu alana çıkılırken oluşturma Operations Manager sitesine e-posta bildirimleri gönderecek iki [mantıksal uygulama](https://docs.microsoft.com/azure/event-grid/handler-webhooks#logic-apps) ayarlayın.
> * Azure haritalar bölge giriş ve çıkış olaylarına abone olmak için [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) kullanın. İki Logic Apps tanımlanmış HTTP uç noktalarını çağıran iki Web kancası olay aboneliği ayarlayacağız. Logic Apps, daha sonra, bir ekipman için uygun e-posta bildirimlerini daha sonra veya bölge girişi altına alacak şekilde gönderir.
> * Bir ekipman parçası çıktığında ve bölge alanları girdiğinde bildirim almak için [Arama bölge kümesi API 'sini](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) kullanın.

## <a name="prerequisites"></a>Önkoşullar

1. [Azure haritalar hesabı oluşturma](quick-demo-map-app.md#create-an-azure-maps-account)
2. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account).

Bu öğretici [Postman](https://www.postman.com/) uygulamasını kullanır, ancak farklı bir API geliştirme ortamı seçebilirsiniz.

## <a name="upload-geofencing-geojson-data"></a>Bölge sınırlaması coğrafi JSON verilerini karşıya yükle

Bu öğreticide, içeren bölge sınırlaması coğrafi JSON verilerini karşıya yükleyeceğiz `FeatureCollection` . , `FeatureCollection` Oluşturma sitesi içinde Çokgen alanları tanımlayan iki bölge içerir. İlk bölge için zaman aşımı süresi veya kısıtlama yoktur. İkinci bir, yalnızca iş saatleri boyunca sorgulanabilecek (9-5 P.M. PST) ve artık 1 Ocak 2022 ' den sonra geçerli olmayacaktır. GeoJSON biçimi hakkında daha fazla bilgi için bkz. [bölge sınırlama coğrafi JSON verileri](geofence-geojson.md).

>[!TIP]
>Bölge sınırlaması verilerinizi dilediğiniz zaman güncelleştirebilirsiniz. Verilerinizi güncelleştirme hakkında daha fazla bilgi için bkz. [veri yükleme API 'si](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)

1. Postman uygulamasını açın. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **koleksiyon**' ı seçin.  Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin.

2. İsteği oluşturmak için **Yeni** ' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin. İstek için bir **istek adı** girin. Önceki adımda oluşturduğunuz koleksiyonu seçin ve ardından **Kaydet**' i seçin.

3. Oluşturucu sekmesinde http **Post** yöntemini seçin ve bölge sınırlaması verilerini Azure Maps hizmetine yüklemek için aşağıdaki URL 'yi girin. Bu istek ve bu makalede bahsedilen diğer istekler için, `{Azure-Maps-Primary-Subscription-key}` birincil abonelik anahtarınızla değiştirin.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={Azure-Maps-Primary-Subscription-key}&api-version=1.0&dataFormat=geojson
    ```

    URL yolundaki _geojson_ parametresi, karşıya yüklenen verilerin veri biçimini temsil eder.

4. **Gövde** sekmesine tıklayın. **Ham**' ı ve sonra da giriş biçimi olarak **JSON** ' ı seçin. Aşağıdaki GeoJSON verilerini kopyalayıp **gövde** metin alanına yapıştırın:

   ```JSON
   {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13393688201903,
                  47.63829579223815
                ],
                [
                  -122.13389128446579,
                  47.63782047131512
                ],
                [
                  -122.13240802288054,
                  47.63783312249837
                ],
                [
                  -122.13238388299942,
                  47.63829037035086
                ],
                [
                  -122.13393688201903,
                  47.63829579223815
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "1"
          }
        },
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13374376296996,
                  47.63784758098976
                ],
                [
                  -122.13277012109755,
                  47.63784577367854
                ],
                [
                  -122.13314831256866,
                  47.6382813338708
                ],
                [
                  -122.1334782242775,
                  47.63827591198201
                ],
                [
                  -122.13374376296996,
                  47.63784758098976
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "2",
            "validityTime": {
            "expiredTime": "2022-01-01T00:00:00",
            "validityPeriod": [
                {
                  "startTime": "2020-07-15T16:00:00",
                  "endTime": "2020-07-15T24:00:00",
                  "recurrenceType": "Daily",
                  "recurrenceFrequency": 1,
                  "businessDayOnly": true
                }
              ]
            }
          }
        }
      ]
   }
   ```

5. Mavi **Gönder** düğmesine tıklayın ve isteğin işlemesini bekleyin. İstek tamamlandıktan sonra yanıtın **üstbilgiler** sekmesine gidin. Olan **konum** anahtarının değerini kopyalayın `status URL` .

    ```http
    https://atlas.microsoft.com/mapData/operations/<operationId>?api-version=1.0
    ```

6. API çağrısının durumunu denetlemek için üzerinde bir http isteği **alın** `status URL` . Kimlik doğrulaması için birincil abonelik anahtarınızı URL 'ye eklemeniz gerekir. **Get** isteği aşağıdaki URL 'yi beğenmelidir:

   ```HTTP
   https://atlas.microsoft.com/mapData/<operationId>/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. HTTP isteği **Al** işlemi başarıyla tamamlandığında, döndürür `resourceLocation` . , `resourceLocation` `udid` Karşıya yüklenen içerik için benzersiz bir içerir. `udid`Bu öğreticinin son bölümünde bölge Al API 'sini sorgulamak için bunu kaydetmeniz gerekir. İsteğe bağlı olarak, bir `resourceLocation` sonraki adımda bu kaynaktan meta verileri almak için URL 'yi kullanabilirsiniz.

      ```json
      {
          "status": "Succeeded",
          "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0"
      }
      ```

8. İçerik meta verilerini almak için, **GET** `resourceLocation` adım 7 ' de alınan URL 'de http isteği Al ' ı oluşturun. Kimlik doğrulaması için birincil abonelik anahtarınızı URL 'ye eklemediğinizden emin olun. **Get** isteği aşağıdaki URL 'yi beğenmelidir:

    ```http
   https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0&subscription-key={Azure-Maps-Primary-Subscription-key}
    ```

9. HTTP isteği **Al** işlemi başarıyla tamamlandığında, yanıt gövdesi `udid` `resourceLocation` adım 7 ' de belirtilen içeriği, gelecekteki içeriğe erişmek/indirmek için konumunu ve oluşturulma/güncelleme tarihi, boyutu vb. gibi içerikler hakkında bazı diğer meta verileri içerir. Genel yanıta bir örnek:

    ```json
    {
        "udid": "{udid}",
        "location": "https://atlas.microsoft.com/mapData/{udid}?api-version=1.0",
        "created": "7/15/2020 6:11:43 PM +00:00",
        "updated": "7/15/2020 6:11:45 PM +00:00",
        "sizeInBytes": 1962,
        "uploadStatus": "Completed"
    }
    ```

## <a name="create-logic-app-workflows"></a>Mantıksal uygulama iş akışları oluşturma

Bu bölümde, bir e-posta bildirimi tetiklenecek iki [mantıksal uygulama](https://docs.microsoft.com/azure/event-grid/handler-webhooks#logic-apps) uç noktası oluşturacağız. Her bitiş noktası çağrıldığında e-posta bildirimleri gönderecek ilk tetikleyiciyi nasıl oluşturacağınız hakkında göstereceğiz.

1. [Azure portalında](https://portal.azure.com) oturum açın

2. [Azure portalının](https://portal.azure.com) sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.

3. Market 'te *Ara* kutusuna **Logic App**yazın.

4. *Sonuçlardan* **Logic App**' i seçin. **Oluştur** düğmesine tıklayın.

5. **Mantıksal uygulama** sayfasında, aşağıdaki değerleri girin:
    * Bu mantıksal uygulama için kullanmak istediğiniz *abonelik* .
    * Bu mantıksal uygulamanın *kaynak grubu* adı. Kaynak grubu için *Yeni oluştur* veya *Mevcut olanı kullan* seçeneğini belirleyebilirsiniz.
    * Mantıksal uygulamanızın *mantıksal uygulama adı* . Bu durumda, ad olarak kullanacağız `Equipment-Enter` .

    Bu öğreticinin amaçları doğrultusunda, kalan değerleri varsayılan ayarlarında tutun.

    :::image type="content" source="./media/tutorial-geofence/logic-app-create.png" alt-text="Mantıksal uygulama oluşturma":::

6. **Gözden geçir + oluştur** düğmesine tıklayın. Ayarlarınızı gözden geçirin ve dağıtımı göndermek için **Oluştur** ' a tıklayın. Dağıtım başarıyla tamamlandığında **Kaynağa Git ' e**tıklayın. **Logic App Designer** 'a yönlendirilirsiniz

7. Şimdi bir tetikleyici türü seçeceğiz. Bir bit ' i ortak bir *tetikleyici ile kullanmaya başlayın** bölümüne gidin. **BIR http isteği alındığında**üzerine tıklayın.

     :::image type="content" source="./media/tutorial-geofence/logic-app-trigger.png" alt-text="Mantıksal uygulama HTTP tetikleyicisi oluşturma":::

8. Tasarımcının sağ üst köşesindeki **Kaydet** ' e tıklayın. **Http post URL 'si** otomatik olarak oluşturulacaktır. Bir olay uç noktası oluşturmak için sonraki bölümde ihtiyacınız olacak şekilde URL 'YI kaydedin.

    :::image type="content" source="./media/tutorial-geofence/logic-app-httprequest.png" alt-text="Mantıksal uygulama HTTP Isteği URL 'SI ve JSON":::

9. **+ Yeni adım**' ı seçin. Şimdi bir eylem seçeceğiz. `outlook.com email`Arama kutusuna yazın. **Eylemler** listesinde, aşağı kaydırın ve **e-posta gönder (v2)** seçeneğine tıklayın.
  
    :::image type="content" source="./media/tutorial-geofence/logic-app-designer.png" alt-text="Mantıksal uygulama Tasarımcısı oluşturma":::

10. Outlook.com hesabınızda oturum açın. Mantıksal uygulamanın hesaba erişmesine izin vermek için **Evet** ' i tıklatın. E-posta göndermek için alanları girin.

    :::image type="content" source="./media/tutorial-geofence/logic-app-email.png" alt-text="Mantıksal uygulama oluşturma e-posta gönder adımı":::

    >[!TIP]
    > `geometryId` `deviceId` Event Grid tarafından gönderilen verileri okumak üzere mantıksal uygulamayı yapılandırarak e-posta bildirimlerinde veya gibi geojson yanıt verilerini alabilirsiniz. Mantıksal uygulamanın, olay verilerini kullanmak ve e-posta bildirimlerine iletmek üzere nasıl yapılandırılacağı hakkında bilgi için bkz. [öğretici: Event Grid ve Logic Apps kullanarak Azure IoT Hub olayları hakkında e-posta bildirimleri gönderme](https://docs.microsoft.com/azure/event-grid/publish-iot-hub-events-to-logic-apps).

11. Logic Apps tasarımcısının sol üst köşesindeki **Kaydet** ' e tıklayın.

12. Ekipman yapım sitesinden çıktığında yöneticiyi bilgilendirmek için ikinci bir mantıksal uygulama oluşturmak üzere 3-11 arası adımları tekrarlayın. Mantıksal uygulamayı adlandırın `Equipment-Exit` .

## <a name="create-azure-maps-events-subscriptions"></a>Azure haritalar olay abonelikleri oluşturma

Azure haritalar üç olay türünü destekler. Azure haritalar desteklenen olay türlerine [buradan](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps)göz atabilirsiniz.  İki farklı olay aboneliği oluşturabileceğiz: bir adet bölge ve bölge girme çıkış olayları için bir tane.

Bölge giriş olayları için bir olay aboneliği oluşturmak üzere aşağıdaki adımları izleyin. Adımları benzer bir şekilde tekrarlayarak bölge, çıkış olaylarına abone olabilirsiniz.

1. Azure haritalar hesabınıza gidin. Panoda, **abonelikler**' i seçin. Abonelik adı ' na tıklayın ve ayarlar menüsünde **Olaylar** ' ı seçin.

    :::image type="content" source="./media/tutorial-geofence/events-tab.png" alt-text="Azure Maps hesap olaylarına gidin":::

2. Bir olay aboneliği oluşturmak için olaylar sayfasından **+ olay aboneliği** ' ni seçin.

    :::image type="content" source="./media/tutorial-geofence/create-event-subscription.png" alt-text="Azure haritalar olayları aboneliği oluşturma":::

3. **Olay aboneliği oluştur** sayfasında, aşağıdaki değerleri girin:
    * Olay aboneliğinin *adı* .
    * *Olay şeması* *Event Grid şema*olmalıdır.
    * Bu olay aboneliğinin *Sistem konu adı* . Bu durumda, kullanacağız `Contoso-Construction` .
    * *Olay türlerine filtrele*' de, `Geofence Entered` olay türü olarak öğesini seçin.
    * *Uç nokta türü*için öğesini seçin `Web Hook` .
    * *Uç nokta*için, önceki bölümde oluşturduğunuz mantıksal uygulama giriş uç noktası IÇIN http post URL 'sini kopyalayın. Kaydetmeyi unuttuysanız, Logic App Designer 'a geri dönüp HTTP tetikleyici adımından kopyalamanız yeterlidir.

    :::image type="content" source="./media/tutorial-geofence/events-subscription.png" alt-text="Azure haritalar olayları abonelik ayrıntıları":::

4. **Oluştur**’a tıklayın.

5. Önceki bölümde oluşturduğunuz Logic App çıkış uç noktası için 1-4 adımlarını yineleyin. Adım 3 ' te `Geofence Exited` olay türü olarak seçtiğinizden emin olun.

## <a name="use-search-geofence-get-api"></a>Arama bölge bölge Al API 'sini kullanma

Şimdi, bir ekipman parçası bölge dilimleri girdiğinde ya da çıktığında Operations Manager e-posta bildirimleri göndermek için [Arama bölge kullanımı API](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) 'sini kullanacağız.

Her ekipman bir içerir `deviceId` . Bu öğreticide, benzersiz KIMLIĞI olan tek bir ekipman parçasını izliyoruz `device_1` .

Daha anlaşılır olması için aşağıdaki diyagramda, zaman içindeki donanımların, bölge sayısı dışında bir yerde olan *Başlangıç* konumundan başlayan beş konumu gösterilmektedir. Bu öğreticinin amaçları doğrultusunda, bu konumda cihazı sorguladığımızdan *Başlangıç* konumu tanımsızdır.

[Arama bölge sınırı Get API](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) 'sini ilk bölge sınırı girişi veya çıkışı gösteren bir ekipman konumuyla sorgulıyoruz, Event Grid Operations Manager e-posta bildirimi göndermek için uygun mantıksal uygulama uç noktasını çağırır.

Aşağıdaki bölümlerin her biri, donanımların beş farklı konum koordinasını kullanarak HTTP GET bölge sınırlaması API 'SI isteklerini sağlar.

![Azure haritalar 'da bölge sınırı Haritası](./media/tutorial-geofence/geofence.png)

### <a name="equipment-location-1-47638237-122132483"></a>Ekipman konumu 1 (47.638237,-122,132483)

1. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin.  İstek için bir **istek adı** girin. Ad, *konum 1*' i kullanacağız. Bölge sınırlaması olan [coğrafi JSON verilerini karşıya yükle bölümünde](#upload-geofencing-geojson-data)oluşturduğunuz koleksiyonu seçin ve ardından **Kaydet**' i seçin.

2. Oluşturucu sekmesinde http yöntemini **Al** ' ı seçin ve aşağıdaki URL 'yi girin, `{Azure-Maps-Primary-Subscription-key}` birincil abonelik anahtarınızla ve bölge sınırlaması olan `{udid}` `udid` [coğrafi JSON verilerini karşıya yükleme bölümüne](#upload-geofencing-geojson-data)kaydettiğinizden emin olun.

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

3. **Gönder** düğmesine tıklayın. Aşağıdaki GeoJSON yanıt penceresinde görünür.

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_1",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.638291,
          "nearestLon": -122.132483
        },
        {
          "deviceId": "device_1",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": 999.0,
          "nearestLat": 47.638053,
          "nearestLon": -122.13295
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ```

4. Yukarıdaki GeoJSON yanıtında, ana site bölge listesinden olumsuz mesafe, ekipmanın bölge bölgesi içinde olduğu anlamına gelir. Alt site bölge bölge değerinden olumlu mesafe, ekipmanın alt site bölge sınırı dışında olduğu anlamına gelir. Bu cihaz, ana site bölge adı içinde ilk kez bulunduğu `isEventPublished` için, parametresi olarak ayarlanır `true` ve Operations Manager, ekipmanın bölge bölgesine girdiği bir e-posta bildirimi almış olabilir.

### <a name="location-2-4763800-122132531"></a>Konum 2 (47.63800,-122,132531)

1. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin.  İstek için bir **istek adı** girin. Ad, *Konum 2*' yi kullanacağız. Bölge sınırlaması olan [coğrafi JSON verilerini karşıya yükle bölümünde](#upload-geofencing-geojson-data)oluşturduğunuz koleksiyonu seçin ve ardından **Kaydet**' i seçin.

2. Oluşturucu sekmesinde http yöntemini **Al** ' ı seçin ve aşağıdaki URL 'yi girin, `{Azure-Maps-Primary-Subscription-key}` birincil abonelik anahtarınızla ve bölge sınırlaması olan `{udid}` `udid` [coğrafi JSON verilerini karşıya yükleme bölümüne](#upload-geofencing-geojson-data)kaydettiğinizden emin olun.

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

3. **Gönder** düğmesine tıklayın. Aşağıdaki GeoJSON yanıt penceresinde görünür:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.637997,
          "nearestLon": -122.132399
        },
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": 999.0,
          "nearestLat": 47.63789,
          "nearestLon": -122.132809
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": false
    }
    ````

4. Yukarıdaki GeoJSON yanıtında, ekipman ana site bölge bölgesine kaldı ve alt site bölge bölge bölge 'sini girmemiştir. Sonuç olarak, parametresi olarak `isEventPublished` ayarlanır `false` ve Operations Manager herhangi bir e-posta bildirimi almaz.

### <a name="location-3-4763810783315048-12213336020708084"></a>Konum 3 (47.63810783315048,-122.13336020708084)

1. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin.  İstek için bir **istek adı** girin. Ad, *Konum 3*' ü kullanacağız. Bölge sınırlaması olan [coğrafi JSON verilerini karşıya yükle bölümünde](#upload-geofencing-geojson-data)oluşturduğunuz koleksiyonu seçin ve ardından **Kaydet**' i seçin.

2. Oluşturucu sekmesinde http yöntemini **Al** ' ı seçin ve aşağıdaki URL 'yi girin, `{Azure-Maps-Primary-Subscription-key}` birincil abonelik anahtarınızla ve bölge sınırlaması olan `{udid}` `udid` [coğrafi JSON verilerini karşıya yükleme bölümüne](#upload-geofencing-geojson-data)kaydettiğinizden emin olun.

    ```HTTP
      https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
      ```

3. **Gönder** düğmesine tıklayın. Aşağıdaki GeoJSON yanıt penceresinde görünür:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.638294,
          "nearestLon": -122.133359
        },
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": -999.0,
          "nearestLat": 47.638161,
          "nearestLon": -122.133549
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ````

4. Yukarıdaki GeoJSON yanıtında, ekipman ana site bölge bölgesine kaldı, ancak alt site bölge bölge bölgesine girdi. Sonuç olarak, parametresi olarak `isEventPublished` ayarlanır `true` ve Operations Manager, ekipmanın bölge sınırlaması olduğunu belirten bir e-posta bildirimi alır.

    >[!NOTE]
    >Ekipman iş saatlerinden sonra alt siteye taşınmışsa, hiçbir olay yayımlanmaz ve Operations Manager hiçbir bildirim almaz.  

### <a name="location-4-47637988-1221338344"></a>Konum 4 (47.637988,-122,1338344)

1. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin.  İstek için bir **istek adı** girin. Ad, *konum 4*' ü kullanacağız. Bölge sınırlaması olan [coğrafi JSON verilerini karşıya yükle bölümünde](#upload-geofencing-geojson-data)oluşturduğunuz koleksiyonu seçin ve ardından **Kaydet**' i seçin.

2. Oluşturucu sekmesinde http yöntemini **Al** ' ı seçin ve aşağıdaki URL 'yi girin, `{Azure-Maps-Primary-Subscription-key}` birincil abonelik anahtarınızla ve bölge sınırlaması olan `{udid}` `udid` [coğrafi JSON verilerini karşıya yükleme bölümüne](#upload-geofencing-geojson-data)kaydettiğinizden emin olun.

    ```HTTP
    https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.637988&userTime=2023-01-16&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
    ```

3. **Gönder** düğmesine tıklayın. Aşağıdaki GeoJSON yanıt penceresinde görünür:

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.637985,
          "nearestLon": -122.133907
        }
      ],
      "expiredGeofenceGeometryId": [
        "2"
      ],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": false
    }
    ````

4. Yukarıdaki GeoJSON yanıtında, ekipman ana site bölge bölgesine çıkımıştır, ancak alt site bölge bölge değerinden çıkıldı. Ancak, fark ederseniz `userTime` değer `expiredTime` bölge verilerinde tanımlananla daha sonra olur. Sonuç olarak, parametresi olarak `isEventPublished` ayarlanır `false` ve Operations Manager bir e-posta bildirimi almaz.

### <a name="location-547637988-1221338344"></a>Konum 5 (47.637988,-122,1338344)

1. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin.  İstek için bir **istek adı** girin. Ad, *konum 4*' ü kullanacağız. Bölge sınırlaması olan [coğrafi JSON verilerini karşıya yükle bölümünde](#upload-geofencing-geojson-data)oluşturduğunuz koleksiyonu seçin ve ardından **Kaydet**' i seçin.

2. Oluşturucu sekmesinde http yöntemini **Al** ' ı seçin ve aşağıdaki URL 'yi girin, `{Azure-Maps-Primary-Subscription-key}` birincil abonelik anahtarınızla ve bölge sınırlaması olan `{udid}` `udid` [coğrafi JSON verilerini karşıya yükleme bölümüne](#upload-geofencing-geojson-data)kaydettiğinizden emin olun.

    ```HTTP
    https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
    ```

3. **Gönder** düğmesine tıklayın. Aşağıdaki GeoJSON yanıt penceresinde görünür:

    ```json
    {
      "geometries": [
      {
        "deviceId": "device_01",
        "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
        "geometryId": "1",
        "distance": -999.0,
        "nearestLat": 47.637985,
        "nearestLon": -122.133907
      },
      {
        "deviceId": "device_01",
        "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
        "geometryId": "2",
        "distance": 999.0,
        "nearestLat": 47.637945,
        "nearestLon": -122.133683
      }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ````

4. Yukarıdaki GeoJSON yanıtında, ekipman ana site bölge bölgesine çıkış yaptı. Sonuç olarak, parametresi olarak `isEventPublished` ayarlanır `true` ve Operations Manager, ekipmanın bir bölge kapsamında çıkış yapıldığını belirten bir e-posta bildirimi alır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Logic Apps içerik türlerini işleme](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type)

> [!div class="nextstepaction"]
> [Event Grid ve Logic Apps kullanarak e-posta bildirimleri gönderin](https://docs.microsoft.com/azure/event-grid/publish-iot-hub-events-to-logic-apps)

> [!div class="nextstepaction"]
> [Event Grid 'de desteklenen Olaylar işleyicileri](https://docs.microsoft.com/azure/event-grid/event-handlers).
