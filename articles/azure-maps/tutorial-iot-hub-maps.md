---
title: 'Öğretici: Microsoft Azure haritaları ile IoT uzamsal analizler uygulama'
description: IoT Hub Microsoft Azure Maps hizmet API 'Leriyle tümleştirin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/01/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 648feedfb82ad43af8f350bd25c9deb5d6ceec03
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90007290"
---
# <a name="tutorial-implement-iot-spatial-analytics-using-azure-maps"></a>Öğretici: Azure Maps kullanarak IoT uzamsal Analizi uygulama

IoT senaryosunda, boşluk ve saat içinde oluşan ilgili olayları yakalamak ve izlemek yaygındır. Örnek senaryolar, filo yönetimi, varlık izleme, Mobility ve akıllı şehir uygulamaları içerir. Bu öğretici, Azure Maps API 'Lerini kullanarak kullanılan otomobil kiralama hareketini izleyen bir çözümde size rehberlik eder.

Bu öğreticide şunları yapmanız gerekir:

> [!div class="checklist"]
> * Araba izleme verilerini günlüğe kaydetmek için bir Azure depolama hesabı oluşturun.
> * Veri yükleme API 'sini kullanarak Azure Maps veri hizmetine bölge bölge yükleyin.
> * Bir IoT Hub oluşturun ve bir cihazı kaydedin.
> * Azure Maps uzamsal analizler temelinde iş mantığı uygulayan Azure Işlevlerinde bir işlev oluşturun.
> * Azure işlevindeki IoT cihaz telemetri olaylarına Event Grid aracılığıyla abone olun.
> * IoT Hub ileti yönlendirme kullanarak telemetri olaylarını filtreleyin.

## <a name="prerequisites"></a>Önkoşullar

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. [Azure haritalar hesabı oluşturun](quick-demo-map-app.md#create-an-azure-maps-account).

3. Birincil anahtar veya abonelik anahtarı olarak da bilinen [birincil bir abonelik anahtarı alın](quick-demo-map-app.md#get-the-primary-key-for-your-account). Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](how-to-manage-authentication.md).

4. [Bir kaynak grubu oluşturun](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups). Bu öğreticide kaynak grubumuzu *ContosoRental*adı vereceğiz, ancak istediğiniz adı seçebilirsiniz.

5. [Rentalcarsimülasyonu C# projesini](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation)indirin.

Bu öğretici [Postman](https://www.postman.com/) uygulamasını kullanır, ancak farklı bir API geliştirme ortamı seçebilirsiniz.

## <a name="use-case-rental-car-tracking"></a>Kullanım örneği: Kiralama arabası izleme

Bu öğreticide aşağıdaki senaryo gösterilmektedir: bir otomobil kiralama şirketi, kira arabasının konum bilgilerini, uzaklığı ve çalışma durumunu günlüğe kaydetmek istiyor. Ayrıca, şirket her bir otomobil doğru yetkili coğrafi bölgeyi terk ettiğinde bu bilgileri depolayacağız.

Kullanım örneğimizde, Kiralama otomobilleri düzenli olarak Azure IoT Hub telemetri verileri gönderen IoT cihazlarıyla donatılmıştır. Telemetri geçerli konumu içerir ve otomobil altyapısının çalışıp çalışmadığını gösterir. Cihaz konumu şeması, [Jeo uzamsal veriler Için ıot Tak ve kullan şemasına](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v1-preview/schemas/geospatial.md)uyar. Kiralık otomobil cihaz telemetri şeması aşağıdaki JSON kodu gibi görünür:

```JSON
{
    "data": {
        "properties": {
            "Engine": "ON"
        },
        "systemProperties": {
            "iothub-content-type": "application/json",
            "iothub-content-encoding": "utf-8",
            "iothub-connection-device-id": "ContosoRentalDevice",
            "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
            "iothub-connection-auth-generation-id": "636959817064335548",
            "iothub-enqueuedtime": "2019-06-18T00:17:20.608Z",
            "iothub-message-source": "Telemetry"
        },
        "body": {
            "location": {
                "type": "Point",
                "coordinates": [ -77.025988698005662, 38.9015330523316 ]
            }
        }
    }
}
```

Bu öğreticide yalnızca bir araç izliyoruz. Azure hizmetlerini ayarladıktan sonra, araç simülatörünü çalıştırmak için [Rentalcarsimülasyonu C# projesini ](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) indirmeniz gerekir. Olaydan işlev yürütmeye kadar tüm işlem aşağıdaki adımlarda özetlenmiştir:

1. Araç içi cihaz, telemetri verilerini IoT Hub 'a gönderir.

2. Araba altyapısı çalışıyorsa, IoT Hub, telemetri verilerini Event Grid yayımlar.

3. Bir Azure işlevi, cihaz telemetri olaylarına ait olay aboneliği nedeniyle tetiklenir.

4. İşlev, araç cihazının Konum koordinatlarını, olay zamanını ve cihaz KIMLIĞINI günlüğe kaydeder. Daha sonra, arabasının bölge dışı olup olmadığını anlamak için [uzamsal bölge Get API](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence) 'sini kullanır. Bölge sınırları dışında ilerlüğünde, işlev olaydan alınan konum verilerini blob kapsayımuza depolar. Ayrıca, işlevimiz, koordinat konumunu açık bir adrese çevirmek için [ters adres aramasını](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgular ve cihaz konumu verilerinin geri kalanı ile depolar.

Aşağıdaki diyagramda sisteme yüksek düzeyde bir genel bakış sunulmaktadır.

   :::image type="content" source="./media/tutorial-iot-hub-maps/system-diagram.png" border="false" alt-text="Sisteme genel bakış":::

Aşağıdaki şekilde, bölge sınırı alanını mavi olarak vurgular. Kiralık Araba rotası yeşil bir çizgi ile belirtilir.

   :::image type="content" source="./media/tutorial-iot-hub-maps/geofence-route.png" border="false" alt-text="Geofence rotası":::

## <a name="create-an-azure-storage-account"></a>Azure depolama hesabı oluşturma

Araba ihlali izleme verilerini depolamak için, kaynak grubunuzda [genel amaçlı v2 depolama hesabı](https://docs.microsoft.com/azure/storage/common/storage-account-overview#general-purpose-v2-accounts) oluşturacağız. Bir kaynak grubu oluşturmadıysanız, [kaynak grubu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups)bölümündeki yönergeleri izleyin. Bu öğreticide kaynak grubumuzu *ContosoRental*adı vereceğiz.

Depolama hesabı oluşturmak için [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)bölümündeki yönergeleri izleyin. Bu öğreticide, *contosorentalstorage*depolama hesabını adlandırdık, ancak istediğiniz herhangi bir şey için ad verebilirsiniz.

Depolama Hesabınız başarıyla oluşturulduktan sonra, günlüğe kaydetme verilerini depolamak için bir kapsayıcı oluşturmanız gerekir.

1. Yeni oluşturulan depolama hesabınıza gidin. Temel bileşenler bölümündeki **kapsayıcılar** bağlantısına tıklayın.

    :::image type="content" source="./media/tutorial-iot-hub-maps/containers.png" alt-text="BLOB depolama için kapsayıcılar":::

2. Sol üst köşedeki **+ kapsayıcı** düğmesine tıklayın. Tarayıcının sağ tarafında bir panel görüntülenir. Kapsayıcınızı *contoso-Kiralama günlüklerine* adlandırın ve **Oluştur**' a tıklayın.

     :::image type="content" source="./media/tutorial-iot-hub-maps/container-new.png" alt-text="Blob kapsayıcısı oluşturma":::

3. Depolama hesabınızdaki **erişim tuşları** dikey penceresine gidin ve **depolama hesabı adını** ve **anahtar** değerini **KEY1** bölümüne kopyalayın. [Azure Işlevi oluşturma ve Event Grid aboneliği ekleme](#create-an-azure-function-and-add-an-event-grid-subscription) bölümünde her iki değere de ihtiyacımız olacak.

    :::image type="content" source="./media/tutorial-iot-hub-maps/access-keys.png" alt-text="Depolama hesabı adını ve anahtarını Kopyala":::

## <a name="upload-a-geofence"></a>Bölge listesini karşıya yükle

Azure Maps hizmetine [bölge bölge 'sini yüklemek](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) Için şimdi [Postman uygulamasını](https://www.getpostman.com) kullanacağız. Bölge alanı, Kiralama aracımız için yetkili coğrafi bölgeyi tanımlar.  Bir arabasının bölge alanının dışına taşınıp taşınmadığını öğrenmek için Azure işlevimizde bölge bölge kullanımı 'nı kullanacağız.

Postman uygulamasını açın ve Azure Maps verilerini karşıya yükleme API 'sini kullanarak bölge bölge 'YI karşıya yüklemek için aşağıdaki adımları izleyin.  

1. Postman uygulamasını açın. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **koleksiyon**' ı seçin.  Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin.

2. İsteği oluşturmak için **Yeni** ' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin. İstek için bir **istek adı** girin. Önceki adımda oluşturduğunuz koleksiyonu seçin ve ardından **Kaydet**' i seçin.

3. Oluşturucu sekmesinde http **Post** yöntemini seçin ve VERI yükleme API 'sine bölge bölge adresini yüklemek için aşağıdaki URL 'yi girin. `{subscription-key}`Birincil abonelik anahtarınızla değiştirdiğinizden emin olun.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

    URL yolundaki parametreye karşılık gelen "geojson" değeri, `dataFormat` karşıya yüklenen verilerin biçimini temsil eder.

4. **Gövde** ' ye tıklayın, **Ham** giriş biçimi ' ni seçin ve açılan listeden giriş biçimi olarak **JSON** ' ı seçin. JSON veri dosyasını [burada](https://raw.githubusercontent.com/Azure-Samples/iothub-to-azure-maps-geofencing/master/src/Data/geofence.json?token=AKD25BYJYKDJBJ55PT62N4C5LRNN4)açın ve JSON 'ı Body bölümüne kopyalayın. **Gönder**’e tıklayın.

5. Mavi **Gönder** düğmesine tıklayın ve isteğin işlemesini bekleyin. İstek tamamlandıktan sonra yanıtın **üstbilgiler** sekmesine gidin. Olan **konum** anahtarının değerini kopyalayın `status URL` .

    ```http
    https://atlas.microsoft.com/mapData/operations/<operationId>?api-version=1.0
    ```

6. API çağrısının durumunu denetlemek için üzerinde bir http isteği **alın** `status URL` . Kimlik doğrulaması için birincil abonelik anahtarınızı URL 'ye eklemeniz gerekir. **Get** isteği aşağıdaki URL 'yi beğenmelidir:

   ```HTTP
   https://atlas.microsoft.com/mapData/<operationId>/status?api-version=1.0&subscription-key={subscription-key}

7. When the **GET** HTTP request completes successfully, it will return a `resourceLocation`. The `resourceLocation` contains the unique `udid` for the uploaded content. You'll need to copy this `udid` for later use in this tutorial.

      ```json
      {
          "status": "Succeeded",
          "resourceLocation": "https://atlas.microsoft.com/mapData/metadata/{udid}?api-version=1.0"
      }
      ```

## <a name="create-an-azure-iot-hub"></a>Azure IoT hub’ı oluşturma

Azure IoT Hub, bir IoT uygulaması ile yönettiği cihazlar arasında güvenli ve güvenilir çift yönlü iletişim imkanı sunar.  Senaryolarımızda, Kiralama arabasının konumunu öğrenmek için araç cihazımızda bilgi almak istiyoruz. Bu bölümde, *ContosoRental* kaynak grubu Içinde bir IoT Hub oluşturacağız. IoT Hub 'ı, cihaz telemetri olaylarımızı yayımlamaktan sorumludur.

> [!NOTE]
> IoT Hub 'ın cihaz telemetri olaylarını Event Grid yayımlama işlevselliği genel önizlemede. Genel Önizleme özellikleri **Doğu ABD, Batı ABD, Batı Avrupa, Azure Kamu, Azure Çin 21Vianet** ve **Azure Almanya**dışındaki tüm bölgelerde kullanılabilir.

*ContosoRental* kaynak grubunda bir IoT Hub 'ı oluşturmak için [IoT Hub oluşturma](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub)' daki adımları izleyin.

## <a name="register-a-device-in-iot-hub"></a>IoT Hub 'a cihaz kaydetme

IoT Hub kimlik kayıt defterine kaydedilmedikleri takdirde cihazlar IoT Hub 'ına bağlanamaz. Senaryolarımızda, *InVehicleDevice*adında tek bir cihaz oluşturacağız. Cihazı IoT Hub 'ında oluşturmak ve kaydetmek için [IoT Hub 'ına yeni bir cihaz kaydetme](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal#register-a-new-device-in-the-iot-hub)bölümündeki adımları izleyin. Daha sonraki bir adımda kullanacağımız için cihazınızın **birincil bağlantı dizesini** kopyalamadığınızdan emin olun.

## <a name="create-an-azure-function-and-add-an-event-grid-subscription"></a>Azure Işlevi oluşturma ve bir Event Grid aboneliği ekleme

Azure Işlevleri, işlem altyapısını açıkça sağlamak veya yönetmek zorunda kalmadan, küçük kod parçalarını ("işlevler") çalıştırmanıza olanak sağlayan sunucusuz bir işlem hizmetidir. Azure Işlevleri hakkında daha fazla bilgi edinmek için bkz. [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) belgeleri.

Bir işlev, belirli bir olay tarafından "tetiklenir". Senaryolarımızda Event Grid tetikleyicisi tarafından tetiklenen bir işlev oluşturacağız. IoT Hub cihazı telemetri olayları için bir olay aboneliği oluşturarak tetikleyici ve işlev arasındaki ilişkiyi oluşturacağız. Bir cihaz telemetri olayı gerçekleştiğinde, işlevimiz bir uç nokta olarak çağrılacaktır ve [daha önce IoT Hub 'a Kaydolduğumuz cihaz](#register-a-device-in-iot-hub)için ilgili verileri alır.

İşlevimizin içereceği C# betik kodu [burada](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx)görünebilirler.

Şimdi Azure işlevimizi ayarlayacağız.

1. Azure portal panosunda **kaynak oluştur ' a**tıklayın. Arama metin kutusuna **işlev uygulaması** yazın. **İşlev uygulaması**' ye tıklayın. **Oluştur**’a tıklayın.

2. **İşlev uygulaması** oluşturma sayfasında, işlev uygulamanızı adlandırın. **Kaynak grubu**altında, açılan listeden *ContosoRental* öğesini seçin.  **Çalışma zamanı yığını**olarak *.NET Core* ' u seçin. Ileri ' ye tıklayın, sayfanın alt kısmındaki **>barındırma ** .

    :::image type="content" source="./media/tutorial-iot-hub-maps/rental-app.png" alt-text="İşlev uygulaması oluşturma":::

3. **Depolama hesabı**Için, [Azure depolama hesabı oluşturma](#create-an-azure-storage-account)bölümünde oluşturduğunuz depolama hesabını seçin. **Gözden geçir ve oluştur**’a tıklayın.

4. İşlev uygulaması ayrıntıları ' nı gözden geçirin ve **Oluştur**' a tıklayın.

5. Uygulama oluşturulduktan sonra buna bir işlev ekleyeceğiz. İşlev uygulamasına gidin. **İşlevler** dikey penceresine tıklayın. Sayfanın üst kısmındaki **+ Ekle** ' ye tıklayın. İşlev şablonu paneli görüntülenir. Panelde aşağı kaydırın. **Azure Event Grid tetikleyicisi**' ne tıklayın.

     >[!WARNING]
    > **Azure Olay Hub 'ı tetikleyicisi** ve **Azure Event Grid tetikleyici** şablonlarının benzer adları vardır. **Azure Event Grid tetikleyici** şablonuna tıkladığınızdan emin olun.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create.png" alt-text="İşlev oluşturma":::

6. İşleve bir ad verin. Bu öğreticide, *GetGeoFunction*adını kullanacağız, ancak dilediğiniz adı kullanabilirsiniz. **Işlev oluştur ' a**tıklayın.

7. Sol taraftaki menüde **kod + test** dikey penceresine tıklayın. [C# betiğini](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx) kopyalayıp kod penceresine yapıştırın.

     :::image type="content" source="./media/tutorial-iot-hub-maps/function-code.png" alt-text="Kodu işlev penceresine Kopyala/Yapıştır":::

8. C# kodunda, aşağıdaki parametreleri değiştirin. **Kaydet**’e tıklayın. Henüz **Test/Çalıştır** öğesine tıklamayın
    * **SUBSCRIPTION_KEY** Azure Maps hesabı birincil abonelik anahtarınızla değiştirin.
    * **UDID** `udid` değerini, [bölge yükleme](#upload-a-geofence)sırasında karşıya yüklediğiniz bölge alanı ile değiştirin.
    * Betikteki **Createblobasync** işlevi, veri depolama hesabında olay başına bir blob oluşturur. **ACCESS_KEY**, **account_name**ve **STORAGE_CONTAINER_NAME** değerini depolama hesabınızın erişim anahtarı, hesap adı ve veri depolama kapsayıcınızla değiştirin. Bu değerler, [Azure depolama hesabı oluşturma](#create-an-azure-storage-account)bölümünde depolama hesabınızı oluşturduğunuzda oluşturulmuştur.

9. Sol taraftaki menüde **tümleştirme** dikey penceresine tıklayın. Diyagramda **Event Grid tetikleyicisi** ' ne tıklayın. Tetikleyici için bir ad yazın, *Eventgridevent*ve **Event Grid aboneliği oluştur**' a tıklayın.

     :::image type="content" source="./media/tutorial-iot-hub-maps/function-integration.png" alt-text="Olay aboneliği ekleyin":::

10. Abonelik ayrıntılarını doldurun. Olay aboneliğini adlandırın. *Olay şeması*Için *Event Grid şeması*' nı seçin. **Konu türleri**için *Azure IoT Hub hesapları*' nı seçin. **Kaynak grubu**için, Bu öğreticinin başlangıcında oluşturduğunuz kaynak grubunu seçin. **Kaynak**Için, [Azure IoT Hub oluşturma](#create-an-azure-iot-hub)bölümünde oluşturduğunuz IoT Hub 'ını seçin. **Olay türlerine filtre**Için *cihaz telemetrisi*' ni seçin. Bu seçenekleri belirledikten sonra *IoT Hub*, **konu türü** değişikliğini görürsünüz. **Sistem konu adı**için, kaynağınız ile aynı adı kullanabilirsiniz.  Son olarak, **uç nokta ayrıntıları** bölümünde **uç nokta seç** ' e tıklayın. Tüm ayarları kabul edin ve **Seçimi Onayla**' ya tıklayın.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create-event-subscription.png" alt-text="Olay aboneliği oluştur":::

11. Ayarlarınızı gözden geçirin. Uç noktanın, bu bölümün başlangıcında oluşturduğunuz işlevi belirttiğinden emin olun. **Oluştur**’a tıklayın.

    :::image type="content" source="./media/tutorial-iot-hub-maps/function-create-event-subscription-confirm.png" alt-text="Olay aboneliği oluşturma onayı":::

12. Şimdi **düzenleme tetikleyicisi** paneline geri dönersiniz. **Kaydet**’e tıklayın.

## <a name="filter-events-using-iot-hub-message-routing"></a>IoT Hub ileti yönlendirme kullanarak olayları filtreleme

Azure Işlevine Event Grid aboneliği eklediğinizde, belirtilen IoT Hub 'ında otomatik olarak bir mesajlaşma yolu oluşturulur. İleti yönlendirme, farklı veri türlerini çeşitli uç noktalara yönlendirmenize olanak tanır. Örneğin, cihaz telemetri iletilerini, cihaz yaşam döngüsü olaylarını ve cihaz ikizi değişiklik olaylarını yönlendirebilirsiniz. IoT Hub ileti yönlendirme hakkında daha fazla bilgi için bkz. [IoT Hub ileti yönlendirmeyi kullanma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c).

:::image type="content" source="./media/tutorial-iot-hub-maps/hub-route.png" alt-text="IoT Hub 'da ileti yönlendirme":::

Örnek senaryomızda, yalnızca Kiralık otomobil hareket ettirilerek ileti almak istiyoruz. `Engine`Özelliğin **"on"** değerine eşit olduğu olayları filtrelemek için bir yönlendirme sorgusu oluşturacağız. Yönlendirme sorgusu oluşturmak için, **Routetoeventgrid** yoluna tıklayın ve **yönlendirme SORGUSUNU** **' "üzerinde" Engine = '** ile değiştirin ve **Kaydet**' e tıklayın. Artık IoT Hub 'ı yalnızca altyapının açık olduğu cihaz telemetrisini yayımlayacak.

:::image type="content" source="./media/tutorial-iot-hub-maps/hub-filter.png" alt-text="Yönlendirme iletilerini filtrele":::

>[!TIP]
>IoT cihazdan buluta iletileri sorgulamak için çeşitli yollar vardır. ileti yönlendirme sözdizimi hakkında daha fazla bilgi edinmek için bkz. [IoT Hub Message Routing](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax).

## <a name="send-telemetry-data-to-iot-hub"></a>Telemetri verilerini IoT Hub gönder

Azure Işlevimiz çalışır duruma getirildikten sonra artık IoT Hub 'ına telemetri verileri gönderebiliriz ve bu, Event Grid yönlendirmektedir. Bir kiralama arabasının yerleşik bir cihazına ait konum verilerinin benzetimini yapmak için bir C# uygulaması kullanalım. Uygulamayı çalıştırmak için geliştirme makinenizde .NET Core SDK 2.1.0 veya üzeri bir sürümü gerekir. IoT Hub benzetimi yapılan telemetri verilerini göndermek için aşağıdaki adımları izleyin.

1. Daha önce yapmadıysanız, [Rentalcarsimülasyonu](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) C# projesini indirin.

2. SimulatedCar.cs dosyasını istediğiniz bir metin düzenleyicisinde açın ve değerini, `connectionString` cihazı kaydettiğinizde kaydettiğiniz ile değiştirin ve dosyadaki değişiklikleri dosyaya kaydedin.

3. Makinenizde .NET Core yüklü olduğundan emin olun. Yerel Terminal pencerenizde, C# projesinin kök klasörüne gidin ve sanal cihaz uygulaması için gerekli paketleri yüklemek üzere aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet restore
    ```

4. Aynı terminalde, Kiralık otomobil benzetim uygulamasını derlemek ve çalıştırmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

  Yerel terminalinizde aşağıdaki gibi görünmelidir.

:::image type="content" source="./media/tutorial-iot-hub-maps/terminal.png" alt-text="Terminal çıkışı":::

Blob Storage kapsayıcısını şimdi açarsanız, aracın bölge alanının dışında olduğu konumlar için dört blob görebilirsiniz.

:::image type="content" source="./media/tutorial-iot-hub-maps/blob.png" alt-text="Kapsayıcı içindeki Blobları görüntüle":::

Aşağıdaki haritada bölge bölge dışında dört araç konum noktası gösterilmektedir. Her konum düzenli zaman aralıklarıyla günlüğe kaydedilir.

:::image type="content" source="./media/tutorial-iot-hub-maps/violation-map.png" alt-text="İhlalin eşlemesi":::

## <a name="explore-azure-maps-and-iot"></a>Azure haritalar ve IoT 'yi keşfet

Bu öğreticide kullanılan Azure Haritalar API 'Lerini araştırmak için, bkz.:

* [Arama adresini tersine al](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Bölge bölge al](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Azure haritalar REST API 'lerinin tüm listesi için bkz.:

* [Azure haritalar REST API 'Leri](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

IoT Tak ve Kullan hakkında daha fazla bilgi edinmek için bkz.:

* [IoT Tak Çalıştır](https://docs.microsoft.com/azure/iot-pnp)

IoT için Azure Sertifikalı cihazların listesini almak için şu adresi ziyaret edin:

* [Azure Sertifikalı cihazlar](https://catalog.azureiotsolutions.com/)

## <a name="next-steps"></a>Sonraki Adımlar

Cihaza bulut telemetrisine nasıl gönderileceğini ve bu konuda daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Cihazdan telemetri gönderme](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet)
