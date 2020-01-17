---
title: 'Öğretici: bir haritada bölge alanı oluşturma ve cihazları izleme | Microsoft Azure haritaları'
description: Microsoft Azure Maps uzamsal hizmetini kullanarak bölge sınırlaması ile ilişkili cihazları nasıl ayarlayacağınızı ve bu cihazların nasıl izleneceğini öğrenin.
author: walsehgal
ms.author: v-musehg
ms.date: 1/15/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a88f03adab3beaea75ec2fa9a1c6f59b09739025
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76153165"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Öğretici: Azure haritalar 'ı kullanarak bölge bölge ayarlama

Bu öğretici, Azure haritalar 'ı kullanarak bölge sınırlaması ayarlama konusunda size kılavuzluk eder. Bir inşaat site yöneticisinin potansiyel olarak tehlikeli donanımları izlemesi için bu senaryoyu göz önünde bulundurun. Yöneticinin, ekipmanın seçili genel oluşturma alanlarında kalmasını sağlamak için gereklidir. Bu genel oluşturma alanı sabit bir parametredir. Düzenlemeler, ekipmanın Bu parametre içinde kalmasını gerektirir ve Operations Manager ihlaller raporlanır.  

Bölge listesini depolamak için veri yükleme API 'sini kullandık ve bölge ile ilgili donanım konumunu denetlemek için bölge sınırı API 'sini kullanın. Hem veri yükleme API 'SI hem de geofence API 'SI Azure haritalarından. Ayrıca bölge sonuçlarının akışını yapmak ve bölge sonuçlarına dayalı bir bildirim ayarlamak için Azure Event Grid de kullanırız. Event Grid hakkında daha fazla bilgi için bkz. [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview).

Bu öğreticide nasıl yapılacağını ele aldık:

> [!div class="checklist"]
> * Veri yükleme API 'sini kullanarak Azure Maps, Data Service içindeki bölge bölge alanını karşıya yükleyin.
> *   Bölge olaylarını işlemek için bir Event Grid ayarlayın.
> *   Bölge olayları işleyicisini ayarlayın.
> *   Logic Apps kullanarak bölge sınırı olaylarına yanıt olarak uyarıları ayarlayın.
> *   Bir oluşturma varlığının yapım sitesi içinde olup olmadığını izlemek için Azure haritalar bölge hizmeti API 'Lerini kullanın.


## <a name="prerequisites"></a>Ön koşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma 

S1 Fiyatlandırma Katmanı içeren bir Azure Maps hesabı aboneliği oluşturmak için [Hesap oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps) bölümündeki yönergeleri izleyin. [Birincil anahtarı al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımlar, hesabınızın birincil anahtarını nasıl alacağınızı gösterir. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).

## <a name="upload-geofences"></a>Bölge dilimleri karşıya yükle

Ana geofalanın, ayarlanan süre sonu zamanına sahip subsite1 olduğunu varsayıyoruz. Gereksinimlerinize göre daha fazla iç içe bölge oluşturabilirsiniz. Bu grup grupları, genel yapım alanındaki farklı oluşturma alanları izlemek için kullanılabilir. Örneğin, subsite1, zamanlamanın 1 ila 4 ' e kadar iş gerçekleştiği yerdir. subsite2, 5 ile 7 arasında çalışmanın gerçekleştiği yerdir. Bu tür tüm balıkların hepsi, projenin başlangıcında tek bir veri kümesi olarak yüklenebilir. Bu balıkların kuralları zaman ve alana göre izlemek için kullanılır. 

Veri yükleme API 'sini kullanarak inşaat sitesinin bölge bölge 'sini karşıya yüklemek için Postman uygulamasını kullanıyoruz. [Postman uygulamasını](https://www.getpostman.com/) yükleyip ücretsiz bir hesap yapın. 

Postman uygulaması yüklendikten sonra, Azure Maps, veri yükleme API 'sini kullanarak inşaat sitesini karşıya yüklemek için aşağıdaki adımları izleyin.

1. Postman uygulamasını açın ve yeni ' ye tıklayın | Yeni oluştur ve Istek seç. Bölge bölge verilerini karşıya yükleme için bir Istek adı girin, kaydedilecek bir koleksiyon veya klasör seçin ve Kaydet ' e tıklayın.

    ![Postman kullanarak bölge dilimleri yükleme](./media/tutorial-geofence/postman-new.png)

2. Oluşturucu sekmesinde HTTP SONRASı yöntemi ' ni seçin ve POST isteği oluşturmak için aşağıdaki URL 'YI girin.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    URL yolundaki GEOJSON parametresi, karşıya yüklenen verilerin veri biçimini temsil eder.

3. **Parametreler**' e tıklayın ve post isteği URL 'si için kullanılacak aşağıdaki anahtar/değer çiftlerini girin. {Subscription-Key} değerini birincil anahtar olarak da bilinen Azure Maps abonelik anahtarınızla değiştirin.
   
    ![Postman 'da verileri karşıya yükleme (bölge bölge) parametreleri](./media/tutorial-geofence/postman-key-vals.png)

4. **Gövde** ' ye tıklayın, ham giriş biçimini seçin ve açılan listeden giriş BIÇIMI olarak JSON ' ı seçin. Karşıya yüklenecek veri olarak aşağıdaki JSON 'ı sağlayın:

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

5. Gönder ' e tıklayın ve yanıt üst bilgisini gözden geçirin. Başarılı bir istek olduğunda, **konum** ÜSTBILGISI durum URI 'sini içerir. Durum URI 'SI aşağıdaki biçimdedir. 

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

6. Durum URI 'nizi kopyalayın ve abonelik anahtarını ekleyin. Durum URI biçimi aşağıdaki gibi olmalıdır. Aşağıdaki biçimde, {} dahil olmak üzere, abonelik anahtarınızla birlikte {Subscription-Key} öğesini değiştirdiğine dikkat edin.

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. `udId`almak için, Postman uygulamasında yeni bir sekme açın ve Oluşturucu sekmesinde HTTP yöntemi Al ' ı seçin. önceki adımdan durum URI 'sindeki bir GET isteği yapın. Karşıya veri yükleme işlemi başarılı olduysa yanıt gövdesinde Uıdıd alacaksınız. Uıdıd ' i daha sonra kullanmak üzere kopyalayın.

   ```JSON
   {
    "udid" : "{udId}"
   }
   ```

## <a name="set-up-an-event-handler"></a>Olay işleyicisi ayarlama

Bu bölümde, bildirimleri alan bir olay işleyicisi oluşturacağız. Bu olay işleyicisi, her donatımın giriş ve çıkış olayları hakkında Operations Manager bilgilendirmelidir.

Olayları işlemek, girmek ve çıkış yapmak için iki [Logic Apps](https://docs.microsoft.com/azure/event-grid/event-handlers#logic-apps) hizmeti sunuyoruz. Logic Apps tetikdeki olaylar, sırayla daha fazla etkinlik tetikler. Fikir, bu e-postalarda uyarı göndermek Operations Manager. Aşağıdaki şekilde, bölge sınırı Enter olayı için bir mantıksal uygulama oluşturma gösterilmektedir. Benzer şekilde, çıkış olayı için başka bir tane de oluşturabilirsiniz. Daha fazla bilgi için [desteklenen tüm olay işleyicilerini](https://docs.microsoft.com/azure/event-grid/event-handlers) görebilirsiniz.

1. Azure portal bir mantıksal uygulama oluşturma

   ![Bölge olaylarını işlemek için Azure Logic Apps oluşturma](./media/tutorial-geofence/logic-app.png)

2. Mantıksal uygulama için ayarlar menüsünde **mantıksal uygulama Tasarımcısı** ' na gidin.

3. Bir HTTP isteği tetikleyicisi seçin ve ardından "yeni adım" seçeneğini belirleyin. Outlook Bağlayıcısı ' nda, eylem olarak "e-posta gönder" seçeneğini belirleyin
  
   ![Logic Apps şeması](./media/tutorial-geofence/logic-app-schema.png)

4. E-posta göndermek için alanları girin. HTTP URL 'sini bırakın, "Kaydet" i tıkladıktan sonra otomatik olarak oluşturulur

   ![Logic Apps uç noktası oluşturma](./media/tutorial-geofence/logic-app-endpoint.png)

5. HTTP URL uç noktasını oluşturmak için mantıksal uygulamayı kaydedin ve HTTP URL 'sini kopyalayın.

## <a name="create-an-azure-maps-events-subscription"></a>Azure haritalar olayları aboneliği oluşturma

Azure haritalar üç olay türünü destekler. Azure haritalar 'ın desteklenen olay türlerine [buradan] (https://docs.microsoft.com/azure/event-grid/event-schema-azure-maps bakabilirsiniz. Biri, giriş olayı ve diğeri çıkış olayları için olmak üzere iki farklı olay aboneliğine ihtiyacımız var.

Bölge giriş olayları için bir olay aboneliği oluşturmak üzere aşağıdaki adımları izleyin. Bölge sınırı çıkış olaylarına benzer bir şekilde abone olabilirsiniz.

1. Azure haritalar hesabınıza gidin. Panoda, abonelikler ' i seçin. Abonelik adı ' na tıklayın ve ayarlar menüsünde **Olaylar** ' ı seçin.

   ![Azure Maps hesap olaylarına gidin](./media/tutorial-geofence/events-tab.png)

2. Bir olay aboneliği oluşturmak için olaylar sayfasından olay aboneliği ' ni seçin.

   ![Azure haritalar olayları aboneliği oluşturma](./media/tutorial-geofence/create-event-subscription.png)

3. Olayların aboneliğini adlandırın ve ENTER olay türüne abone olun. Şimdi "uç nokta türü" olarak Web kancası ' yi seçin. "Bir uç nokta seçin" düğmesine tıklayın ve mantıksal uygulamanızın HTTP URL uç noktasını "{Endpoint}" içine kopyalayın

   ![Azure haritalar olayları abonelik ayrıntıları](./media/tutorial-geofence/events-subscription.png)


## <a name="use-geofence-api"></a>Geofence API kullanma

Bu durumda, bir **cihazın**bölge içinde mi yoksa dışında mı olduğunu denetlemek Için GEOFENCE API 'sini kullanabilirsiniz. Belirli bir ekipmanın zaman içinde taşındığı farklı konumlara karşı bölge Al API 'sini sorgulamasına olanak sağlar. Aşağıdaki şekilde, beş oluşturma ekipmanıyla birlikte beş konum gösterilmektedir. 

> [!Note]
> Senaryo ve davranış, aşağıdaki şekilde beş farklı konumu yansıtacak şekilde aynı **cihaz kimliğini** temel alır.

"DeviceID", kendi konumunu sorgularken, GET isteğinde cihazınız için sağladığınız benzersiz bir KIMLIKTIR. **Arama bölge sınırı-Get API**'sine zaman uyumsuz bir istek yaptığınızda, "DeviceID", söz konusu cihaz için bölge oluşturma olaylarını belirtilen bölge kimliğine göre yayımlamaya yardımcı olur. Bu öğreticide, benzersiz bir "DeviceID" ile API 'ye zaman uyumsuz istekler yaptık. Öğreticideki istekler diyagramda olduğu gibi kronolojik sırayla yapılır. Yanıttaki "ıbventyayınlanan" özelliği, bir cihaz bölge alanına girdiğinde veya bu işlemden çıktığında yayımlanır. Bu öğreticiyle izlemek için bir cihaz kaydetmeniz gerekmez.

Diyagramda geri bakmasına izin verir. Bu beş konumların her biri, dilimçine karşı bölge girme ve çıkış durumu değişikliğini değerlendirmek için kullanılır. Bir durum değişikliği gerçekleşirse, bölge sınırı hizmeti, mantıksal uygulamaya Event Grid tarafından gönderilen bir olayı tetikler. Sonuç olarak, işlemin Yöneticisi bir e-posta yoluyla ilgili ENTER veya çıkış bildirimini alır.

![Azure haritalar 'da bölge sınırı Haritası](./media/tutorial-geofence/geofence.png)

Postman uygulamasında, yukarıda oluşturduğunuz koleksiyonda yeni bir sekme açın. Oluşturucu sekmesinde HTTP yöntemini Al ' ı seçin:

Aşağıda, donanımların farklı konum koordinatlarına sahip beş HTTP GET Geoflik API isteği bulunur. Koordinatlar kronolojik sırayla gözlemlenmiştir. Her isteğin ardından yanıt gövdesi gelir.
 
1. Konum 1:
    
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
   ![Geofence sorgu 1](./media/tutorial-geofence/geofence-query1.png)

   Yukarıdaki yanıtta, ana bölge listesinden olumsuz mesafe, ekipmanın bölge alanı içinde olduğu anlamına gelir. Alt site bölge bölge değerinden olumlu mesafe, ekipmanın alt site bölge sınırı dışında olduğu anlamına gelir. 

2. Konum 2: 
   
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
    
   ![Geofence sorgu 2](./media/tutorial-geofence/geofence-query2.png)

   Yukarıdaki JSON yanıtına dikkatlice bakarsanız, ekipman alt sitenin dışındadır, ancak ana çit içindedir. Hiçbir olay tetiklenmez ve hiçbir e-posta gönderilmez.

3. Konum 3: 
  
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Geofence sorgu 3](./media/tutorial-geofence/geofence-query3.png)

   Bir durum değişikliği gerçekleşti ve artık ekipman hem ana hem de alt site bölge içindedir. Bu değişiklik, bir olayın yayımlanmasını ve Operations Manager bir bildirim e-postası gönderilmesini sağlar.

4. Konum 4: 

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.637988&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```
  
   ![Geofence sorgu 4](./media/tutorial-geofence/geofence-query4.png)

   Karşılık gelen yanıtı dikkatle gözlemleyerek, ekipman alt site bölge değerinden çıksa bile burada hiçbir olay yayımlanmadığını unutmayın. Kullanıcının GET isteğinde belirtilen zamanına bakarsanız, bu süre için alt sitenin bölge süresinin dolduğunu görebilirsiniz. Ekipman hala ana bölge içinde. Yanıt gövdesinde `expiredGeofenceGeometryId` alt sitenin bölge bölge numarasını da görebilirsiniz.


5. Konum 5:
      
   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63799&lon=-122.134505&userTime=2019-01-16&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

   ![Geofence sorgu 5](./media/tutorial-geofence/geofence-query5.png)

   Ekipmanın ana yapım site bölge bölgesine ayrıldığı hakkında bilgi alabilirsiniz. Bir olay yayımlanır ve Operations Manager bir uyarı e-postası gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, veri yükleme API 'sini kullanarak Azure haritalar ve veri hizmeti 'ne karşıya yükleyerek bölge oluşturma 'yı nasıl ayarlayabileceğiniz anlatılmaktadır. Ayrıca bölge olaylarına abone olmak ve bunları işlemek için Azure Maps olayları kılavuzunu nasıl kullanacağınızı öğrendiniz. 

* Daha karmaşık bir mantık oluşturmak için JSON ayrıştırmak üzere Logic Apps kullanmayı öğrenmek için [Azure Logic Apps içerik türlerini işleme](https://docs.microsoft.com/azure/logic-apps/logic-apps-content-type)bölümüne bakın.
* Event Grid içindeki olay işleyicileri hakkında daha fazla bilgi edinmek için [Event Grid desteklenen olay işleyicileri](https://docs.microsoft.com/azure/event-grid/event-handlers)bölümüne bakın.
