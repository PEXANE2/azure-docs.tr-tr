---
title: 'Öğretici: Bir geofence oluşturun ve harita üzerinde cihazları izlemek | Microsoft Azure Haritaları'
description: Microsoft Azure Haritalar Uzamsal Hizmeti'ni kullanarak coğrafi çiti nasıl ayarlayıp, coğrafi çitlere göre aygıtları nasıl izleyeceğinizi öğrenin.
author: philmea
ms.author: philmea
ms.date: 1/15/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 126829f12d71e40511c26e781cb191988c1d031e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333860"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Öğretici: Azure Haritalar'ı kullanarak coğrafi çit oluşturma

Bu öğretici, Azure Haritalar'ı kullanarak coğrafi çit oluşturmanız için temel adımlardan geçmenizi sağlar. Bu senaryoyu göz önünde bulundurun, bir şantiye yöneticisi potansiyel tehlikeli ekipman izlemek zorundadır. Yönetici, ekipmanın seçilen genel inşaat alanlarında kaldığından emin olmak gerekir. Bu genel inşaat alanı sabit bir parametredir. Yönetmelikler, ekipmanın bu parametre içinde kalmasını gerektirir ve ihlaller Operasyon Müdürü'ne bildirilir.  

Bir geofence depolamak ve geofence göre ekipman konumunu kontrol etmek için Geofence API kullanmak Için Veri Yükleme API kullanın. Hem Veri Yükleme API'si hem de Geofence API'sı Azure Haritalar'dandır. Ayrıca, geofence sonuçlarını akışve geofence sonuçlarına dayalı bir bildirim oluşturmak için Azure Event Grid'i de kullanırız. Olay Izgarası hakkında daha fazla bilgi edinmek için [Azure Olay Ağıt'ına](https://docs.microsoft.com/azure/event-grid/overview)bakın.

Bu öğreticide nasıl ele alacağız:

> [!div class="checklist"]
> * Veri Yükleme API'sini kullanarak Azure Haritalar,Veri hizmetine coğrafi çit alanı yükleyin.
> *   Coğrafi çit olaylarını işlemek için bir Olay Izgarası ayarlayın.
> *   Kurulum geofence olayları işleyicisi.
> *   Logic Apps'ı kullanarak coğrafi çit olaylarına yanıt olarak uyarılar ayarlayın.
> *   Bir inşaat varlığının şantiyeiçinde olup olmadığını izlemek için Azure Haritalar coğrafi çit hizmeti API'lerini kullanın.


## <a name="prerequisites"></a>Ön koşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma 

S1 fiyatlandırma katmanına sahip bir Azure Haritalar hesabı aboneliği oluşturmak için [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin. [Birincil anahtarı alma](quick-demo-map-app.md#get-the-primary-key-for-your-account) adımlarında hesabınızın birincil anahtarını nasıl alacağınızı gösterir. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.

## <a name="upload-geofences"></a>Geofences yükleme

Ana geofence'in belirli bir son kullanma süresine sahip olan alt site1 olduğunu varsayıyoruz. Gereksinimlerinize göre daha fazla iç içe coğrafi çit oluşturabilirsiniz. Bu çit setleri, genel inşaat alanı içindeki farklı inşaat alanlarını izlemek için kullanılabilir. Örneğin, alt site1, zamanlamanın 1-4. subsite2, 5-7. Tüm bu çitler projenin başında tek bir veri kümesi olarak yüklenebilir. Bu çitler zaman ve mekana göre kuralları izlemek için kullanılır. 

Veri Yükleme API'sini kullanarak şantiye için geofence yüklemek için postacı uygulamasını kullanırız. [Postacı uygulamasını](https://www.getpostman.com/) yükleyin ve ücretsiz bir hesap yapın. 

Postacı uygulaması yüklendikten sonra, Azure Haritalar, Veri Yükleme API'sini kullanarak şantiye coğrafi çitini yüklemek için aşağıdaki adımları izleyin.

1. Postacı uygulamasını açın ve yeniyi tıklatın | Yeni oluşturun ve İstek'i seçin. Coğrafi çit yükleme verileri için bir İstek adı girin, kaydetmek için bir koleksiyon veya klasör seçin ve Kaydet'i tıklatın.

    ![Postacı kullanarak geofences yükleyin](./media/tutorial-geofence/postman-new.png)

2. Oluşturucu sekmesinde POST HTTP yöntemini seçin ve POST isteğinde bulunmak için aşağıdaki URL'yi girin.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    URL yolundaki GEOJSON parametresi yüklenen verilerin veri biçimini temsil eder.

3. **Param'ları**tıklatın ve POSTA isteği URL'si için kullanılacak aşağıdaki Anahtar/Değer çiftlerini girin. Birincil anahtar olarak da bilinen Azure Haritalar abonelik anahtarınızla {abonelik tuşu} değiştirin.
   
    ![Postacı'da yükleme verileri (geofence) parametreleri](./media/tutorial-geofence/postman-key-vals.png)

4. **Ardından Gövde'yi** tıklatın ve açılan listeden giriş biçimi olarak JSON'u seçin. Yüklenecek veriler olarak aşağıdaki JSON'u sağlayın:

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
              "expiredTime": "2019-01-15T00:00:00",
              "validityPeriod": [
                {
                  "startTime": "2019-01-08T01:00:00",
                  "endTime": "2019-01-08T17:00:00",
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

5. Yanıt üstbilgisini gönder ve gözden geçir'i tıklatın. Başarılı bir istek üzerine, **Konum** üstbilgisi URI durumunu içerir. Uri durumu aşağıdaki biçimdedir. UploadStatusId değeri { }arasında değildir. { } kullanıcının girmesi gereken değerleri veya farklı kullanıcı için farklı değerleri göstermek için yaygın bir uygulamadır.

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

6. Durumunuzu URI'yi kopyalayın ve abonelik anahtarını eklayın. Durum URI biçimi aşağıdaki gibi olmalıdır. Aşağıdaki biçimde {abonelik anahtarı}'nı değiştireceğinize, abonelik anahtarınızla { }'u dahil etmeyeceğinize dikkat edin.

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. `udId`Postacı uygulamasında yeni bir sekme açmak ve oluşturucu sekmesinde HTTP yöntemini seçin. Veri yüklemeniz başarılı olduysa, yanıt gövdesinde udId alırsınız. Daha sonra kullanmak için udId'i kopyalayın.

   ```JSON
   {
    "status": "Succeeded",
    "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udId}?api-version=1.0"
   }
   ```

## <a name="set-up-an-event-handler"></a>Olay işleyicisi ayarlama

Bu bölümde, bildirimleri alan bir olay işleyicisi oluşturuyoruz. Bu olay işleyicisi, herhangi bir ekipmanın giriş ve çıkış olaylarını Operasyon Yöneticisi'ne bildirmelidir.

Giriş ve çıkış olaylarını işlemek için iki [Logic Apps](https://docs.microsoft.com/azure/event-grid/event-handlers#logic-apps) hizmeti yapıyoruz. Logic Apps'taki olaylar tetiklendiğinde, sırayla daha fazla olay tetikler. Amaç, bu durumda Operasyon Yöneticisi'ne uyarı göndermektir. Aşağıdaki şekil geofence enter olay için bir Mantık Uygulaması oluşturulmasını göstermektedir. Benzer şekilde, çıkış olayı için başka bir tane oluşturabilirsiniz. Daha fazla bilgi için desteklenen tüm [etkinlik işleyicilerini](https://docs.microsoft.com/azure/event-grid/event-handlers) görebilirsiniz.

1. Azure portalında bir Mantık Uygulaması oluşturun. Azure Marketi'nde Mantık Uygulamasını seçin. Ardından **Oluştur** düğmesini seçin.

   ![Coğrafi çit olaylarını işlemek için Azure Logic Apps oluşturma](./media/tutorial-geofence/logic-app.png)

2. Mantık Uygulaması'nın ayarlar **menüsünde, Logic App Designer'a** gidin

3. Bir HTTP istek tetikleyicisi seçin ve ardından "Yeni Adım"ı seçin. Outlook bağlayıcısında, eylem olarak "e-posta gönder"i seçin
  
   ![Mantık Uygulamaları şema](./media/tutorial-geofence/logic-app-schema.png)

4. E-posta göndermek için alanları doldurun. HTTP URL'sini bırakın, "kaydet"i tıklattıktan sonra otomatik olarak

   ![Mantık Uygulamaları bitiş noktası oluşturma](./media/tutorial-geofence/logic-app-endpoint.png)

5. HTTP URL bitiş noktasını oluşturmak ve HTTP URL'sini kopyalamak için mantık uygulamasını kaydedin.

## <a name="create-an-azure-maps-events-subscription"></a>Azure Haritalar Etkinlikleri aboneliği oluşturma

Azure Haritalar üç etkinlik türünü destekler. Azure Haritalar tarafından desteklenen etkinlik türlerine [buradan](https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps)göz atabilirsiniz. Biri enter etkinliği, diğeri çıkış etkinlikleri için olmak üzere iki farklı etkinlik aboneliğine ihtiyacımız vardır.

Geofence enter olayları için bir olay aboneliği oluşturmak için aşağıdaki adımları izleyin. Geofence çıkış olaylarına benzer bir şekilde abone olabilirsiniz.

1. Azure Haritalar hesabınıza gidin. Panoda Abonelikler'i seçin. Abonelik adınızı tıklayın ve ayarlar menüsünden **etkinlikleri** seçin.

   ![Azure Haritalar hesabı Etkinlikleri'nde gezinme](./media/tutorial-geofence/events-tab.png)

2. Etkinlik aboneliği oluşturmak için etkinlikler sayfasından Etkinlik Aboneliği'ni seçin.

   ![Azure Haritalar Etkinlikleri aboneliği oluşturma](./media/tutorial-geofence/create-event-subscription.png)

3. Etkinlik aboneliğini adlandırın ve etkinlik türünü girin' e abone olun. Şimdi, Web Hook'u "Bitiş Noktası Türü" olarak seçin. "Bir bitiş noktası seçin" düğmesine tıklayın ve Mantık Uygulamanız HTTP URL bitiş noktanızı "{Endpoint} " olarak kopyalayın.

   ![Azure Haritalar Etkinlikler abonelik ayrıntıları](./media/tutorial-geofence/events-subscription.png)


## <a name="use-geofence-api"></a>Geofence API'yi kullanma

Geofence API'yi kullanarak bir **aygıtın**, bu durumda ekipmanın bir geofence'in içinde veya dışında olup olmadığını kontrol edebilirsiniz. Belirli bir ekipmanın zaman içinde taşındığı farklı konumlara karşı Geofence GET API'sını sorgulamanızı sağlar. Aşağıdaki şekil beş inşaat ekipmanları ile beş yerleri göstermektedir. 

> [!Note]
> Senaryo ve davranış, aşağıdaki şekilde beş farklı konumu yansıtacak şekilde aynı **aygıt kimliğine** dayanır.

"deviceId", cihazınızın konumunu sorgularken GET isteğinde cihazınız için sağladığınız benzersiz bir kimliktir. Arama geofence için bir eşzamanlı istek yaptığınızda **- GET API**, "deviceId" belirtilen geofence göre, bu cihaz için geofence olayları yayımlamayardımcı olur. Bu eğitimde, api'ye benzersiz bir "deviceId" ile eşzamanlı isteklerde bulunduk. Öğreticideki istekler, diyagramda olduğu gibi kronolojik sırada yapılır. Yanıttaki "isEventPublished" özelliği, bir aygıt geofence'e girdiğinde veya çıktığında yayınlanır. Bu öğretici ile takip etmek için bir cihaz kayıt gerekmez.

Diyagrama geri bakalım. Bu beş lokasyonun her biri, çitin karşı giriş ve çıkış durumunu değerlendirmek için kullanılır. Durum değişikliği oluşursa, coğrafi çit hizmeti Olay Izgarası tarafından Mantık Uygulamasına gönderilen bir olayı tetikler. Sonuç olarak, işlemin yöneticisi ilgili giriş veya çıkış bildirimini bir e-posta yoluyla alır.

![Azure Haritalar'da Geofence Haritası](./media/tutorial-geofence/geofence.png)

Postacı uygulamasında, yukarıda oluşturduğunuz koleksiyonda yeni bir sekme açın. Oluşturucu sekmesinde HTTP'yi AL yöntemini seçin:

Aşağıdaki beş HTTP GET GEofencing API istekleri, ekipman farklı konum koordinatları ile. Koordinatlar kronolojik sırada gözlendiği gibi. Her istek yanıt gövdesi tarafından takip edilir.
 
1. Konum 1:
    
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
   ![Geofence sorgusu 1](./media/tutorial-geofence/geofence-query1.png)

   Yukarıdaki yanıtta, ana geofence negatif mesafe ekipman geofence içinde olduğu anlamına gelir. Alt site geofence olumlu mesafe ekipman alt site geofence dışında olduğu anlamına gelir. 

2. Konum 2: 
   
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
    
   ![Geofence sorgusu 2](./media/tutorial-geofence/geofence-query2.png)

   Eğer önceki JSON yanıt dikkatle ekipman alt sitenin dışında bakarsanız, ama ana çit içinde. Hiçbir olay tetiklenmez ve e-posta gönderilmez.

3. Konum 3: 
  
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Geofence sorgusu 3](./media/tutorial-geofence/geofence-query3.png)

   Bir durum değişikliği meydana geldi ve şimdi ekipman hem ana hem de alt site geofences içinde. Bu değişiklik bir olayın yayımlanmasını ve Operasyon Yöneticisi'ne bir bildirim e-postası gönderilmesine neden olur.

4. Konum 4: 

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
  
   ![Geofence sorgusu 4](./media/tutorial-geofence/geofence-query4.png)

   İlgili yanıtı dikkatle gözlemleyerek, ekipman alt sitenin alt çitinden çıkmasına rağmen hiçbir olayın burada yayınlanmadığını fark edebilirsiniz. GET isteğinde kullanıcının belirtilen süresine bakarsanız, alt site coğrafi çitinin bu süre için süresinin dolduğunu görebilirsiniz. Ekipman hala ana geofence bulunmaktadır. Ayrıca yanıt gövdesi `expiredGeofenceGeometryId` altında alt site geofence geometri kimliğini görebilirsiniz.


5. Konum 5:
      
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63799&lon=-122.134505&userTime=2019-01-16&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Geofence sorgusu 5](./media/tutorial-geofence/geofence-query5.png)

   EkipmanAna şantiye geofence sol olduğunu görebilirsiniz. Bir olay yayımlanır ve Operasyon Yöneticisi'ne bir uyarı e-postası gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz: Veri Yükleme API'sini kullanarak Azure Haritalar ve Veri hizmetine yükleyerek coğrafi çitoluşturmayı. Ayrıca, geofence olaylarına abone olmak ve bunları işlemek için Azure Haritalar Olaylar Ağıt'ı kullanmayı da öğrendiniz. 

* Daha karmaşık bir mantık oluşturmak için JSON'u ayrıştırmak için Mantık Uygulamaları'nı nasıl kullanacağınızı öğrenmek için [Azure Mantık Uygulamaları'ndaki içerik türlerini işlemeye](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type)bakın.
* Olay Izgara'sındaki olay işleyicileri hakkında daha fazla şey öğrenmek için [Olay Izgara'sında desteklenen Etkinlik İşleyicileri'ne](https://docs.microsoft.com/azure/event-grid/event-handlers)bakın.
