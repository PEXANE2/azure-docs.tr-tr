---
title: 'Öğretici: IoT uzamsal analizlerini uygulama | Microsoft Azure haritaları'
description: IoT Hub Microsoft Azure Maps hizmet API 'Leriyle tümleştirin.
author: farah-alyasari
ms.author: v-faalya
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: f0367a195ca0aa5f26ff0819b00c50fabae1d271
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78359657"
---
# <a name="tutorial-implement-iot-spatial-analytics-using-azure-maps"></a>Öğretici: Azure Maps kullanarak IoT uzamsal Analizi uygulama

IoT senaryosunda, boşluk ve saat içinde oluşan ilgili olayları yakalamak ve izlemek yaygındır. Örnek senaryolar, filo yönetimi, varlık izleme, Mobility ve akıllı şehir uygulamaları içerir. Bu öğretici, Azure Maps API 'Lerini kullanarak bir çözüm deseninin size rehberlik eder. İlgili olaylar, Event Grid tarafından sunulan olay abonelik modeli kullanılarak IoT Hub tarafından yakalanır.

Bu öğreticide şunları yapmanız gerekir:

> [!div class="checklist"]
> * Bir IoT Hub oluşturma.
> * Veri yükleme API 'sini kullanarak Azure Maps, Data Service içindeki bölge bölge alanını karşıya yükleyin.
> * Azure Maps uzamsal analizler temelinde iş mantığı uygulayan Azure Işlevlerinde bir işlev oluşturun.
> * Azure işlevindeki IoT cihaz telemetri olaylarına Event Grid aracılığıyla abone olun.
> * IoT Hub ileti yönlendirme kullanarak telemetri olaylarını filtreleyin.
> * İlgili olay verilerini depolamak için bir depolama hesabı oluşturun.
> * Araç içi IoT cihazının benzetimini yapın.
    

## <a name="use-case"></a>Kullanım durumu

Bu çözüm, bir otomobil kiralama şirketinin, Kiralama otomobilleri için olayları izleyip günlüğe kaydetme planlarında yer aldığı bir senaryoyu gösterir. Araba kiralama şirketleri genellikle belirli bir coğrafi bölgeye kadar otomobilleri kiramıştır. Bu kişiler, yeniden kullanıldıkları sırada araba bulunulan yerlerinizi izlememeleri gerekir. Seçilen coğrafi bölgeyi terk eden bir otomobil örnekleri günlüğe kaydedilmelidir. Günlüğe kaydetme verileri, ilkelerin, ücretlerin ve diğer iş yönlerinin düzgün şekilde işlenebilmesini sağlar.

Kullanım örneğimizde, Kiralama otomobilleri düzenli olarak Azure IoT Hub telemetri verileri gönderen IoT cihazlarıyla donatılmıştır. Telemetri geçerli konumu içerir ve otomobil altyapısının çalışıp çalışmadığını gösterir. Cihaz konumu şeması, [Jeo uzamsal veriler Için ıot Tak ve kullan şemasına](https://github.com/Azure/IoTPlugandPlay/blob/master/Schemas/geospatial.md)uyar. Kiralık otomobil cihaz telemetri şeması şöyle görünür:

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

Hedefimizi gerçekleştirmek için araç içi cihaz telemetrisini kullanalım. Bölge sınırlaması kuralları yürütmek istiyoruz. Arabasının taşındığını belirten bir olay alırızda yanıt vermek istiyoruz. Bunu yapmak için, Event Grid üzerinden IoT Hub cihaz telemetri olaylarına abone olacaksınız. 

Event Grid abone olmanın birkaç yolu vardır. Bu öğreticide Azure Işlevleri kullanırız. Azure Işlevleri Event Grid yayımlanan olaylara yeniden davranır. Ayrıca, Azure Maps uzamsal analizine dayalı olan otomobil kiralama iş mantığını da uygular. 

Azure işlevi içindeki kod, araç 'nın bölge bölge 'nin soluna doğru olup olmadığını denetler. Araç bölge sınırlaması altına alıyorsa Azure işlevi, geçerli konumla ilişkili adres gibi ek bilgiler toplar. İşlev Ayrıca, olay koşulları açıklamasının sağlanmasına yardımcı olan bir veri blobu depolamada anlamlı olay verilerini depolamak için mantığı uygular. 

Olay koşulları, otomobil kiralama şirketi ve Kiralama müşterisi için yararlı olabilir. Aşağıdaki diyagramda sisteme yüksek düzeyde bir genel bakış sunulmaktadır.

 
  <center>

  ![Sisteme genel bakış](./media/tutorial-iot-hub-maps/system-diagram.png)
  
  </center>

Aşağıdaki şekil, mavi renkle vurgulanmış bölge alanı alanını temsil eder. Kiralık araç rotası yeşil bir çizgi ile belirtilir.

  ![Geofence rotası](./media/tutorial-iot-hub-maps/geofence-route.png)


## <a name="prerequisites"></a>Önkoşullar 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bu öğreticideki adımları tamamlayabilmeniz için öncelikle Azure portal bir kaynak grubu oluşturmanız gerekir. Bir kaynak grubu oluşturmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Kaynak grupları**’nı seçin.
    
   ![Kaynak grupları](./media/tutorial-iot-hub-maps/resource-group.png)

3. **Kaynak grupları**altında **Ekle**' yi seçin.
    
   ![Kaynak Grubu Ekle](./media/tutorial-iot-hub-maps/add-resource-group.png) 

4. Aşağıdaki özellik değerlerini girin:
    * **Abonelik:** Azure aboneliğinizi seçin.
    * **Kaynak grubu:** Kaynak grubu adı olarak "ContosoRental" yazın.
    * **Bölge:** Kaynak grubu için bir bölge seçin.  

    ![Kaynak grubu ayrıntıları](./media/tutorial-iot-hub-maps/resource-details.png)

    **Gözden geçir + oluştur**' u seçin ve ardından sonraki sayfada **Oluştur** ' u seçin.

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma 

Azure haritalar uzamsal analizlerini temel alan iş mantığını uygulamak için, oluşturduğumuz kaynak grubunda bir Azure Maps hesabı oluşturuyoruz. S1 Fiyatlandırma Katmanı içeren bir Azure Maps hesabı aboneliği oluşturmak için [Hesap oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps) bölümündeki yönergeleri izleyin. Hesabınız için birincil anahtarınızı almak üzere [birincil anahtar al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](how-to-manage-authentication.md).



### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Olay verilerini günlüğe kaydetmek için, tüm Azure depolama hizmetlerine erişim sağlayan genel amaçlı bir **v2storage** oluşturacağız: Bloblar, dosyalar, kuyruklar, tablolar ve diskler.  Bu depolama hesabını, verileri Bloblar olarak depolamak için "ContosoRental" kaynak grubuna yerleştirmemiz gerekir. Depolama hesabı oluşturmak için [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal)' daki yönergeleri izleyin. Daha sonra Blobları depolamak için bir kapsayıcı oluşturmanız gerekir. Bunu yapmak için aşağıdaki adımları izleyin:

1. "Depolama hesabı-blob, dosya, tablo, kuyruk" içinde kapsayıcılar ' a gidin.

    ![larını](./media/tutorial-iot-hub-maps/blobs.png)

2. Sol üstteki kapsayıcı düğmesine tıklayın ve kapsayıcınızı "contoso-Kiralık-Logs" olarak adlandırın ve "Tamam" a tıklayın.

    ![blob kapsayıcısı](./media/tutorial-iot-hub-maps/blob-container.png)

3. Depolama hesabınızdaki **erişim tuşları** dikey penceresine gidin ve "depolama hesabı adı" ve "erişim anahtarı" ' nı kopyalayın. Daha sonraki bir adımda bu gereklidir.

    ![erişim tuşları](./media/tutorial-iot-hub-maps/access-keys.png)


Şimdi, olay verilerini günlüğe kaydetmek için bir depolama hesabı ve kapsayıcı sunuyoruz. Daha sonra bir IoT Hub oluşturacağız.

### <a name="create-an-iot-hub"></a>IoT Hub'ı oluşturma

IoT Hub, bulutta yönetilen bir hizmettir. IoT Hub, bir IoT uygulaması ve BT tarafından yönetilen cihazlar arasında çift yönlü iletişim için bir merkezi ileti hub 'ı görevi görür. Cihaz telemetri iletilerini bir Event Grid yönlendirmek için, "ContosoRental" kaynak grubu içinde bir IoT Hub oluşturun. İletileri arabasının altyapı durumuna göre filtreleyecek bir ileti yolu tümleştirmesi ayarlayın. Ayrıca, otomobil hareket ettirdiğinde cihaz telemetri iletilerini Event Grid de göndereceğiz.

> [!Note] 
> IoT Hub cihaz telemetri olaylarını Event Grid yayımlama işlevselliği genel önizlemede. Genel Önizleme özellikleri **Doğu ABD, Batı ABD, Batı Avrupa, Azure Kamu, Azure Çin 21Vianet** ve **Azure Almanya**dışındaki tüm bölgelerde kullanılabilir. 

[IoT Hub oluşturma bölümündeki](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub)adımları Izleyerek IoT Hub 'ı oluşturun.


### <a name="register-a-device"></a>Cihaz kaydetme 

IoT Hub bağlanmak için bir cihazın kayıtlı olması gerekir. Bir cihazı IoT Hub 'a kaydetmek için aşağıdaki adımları izleyin:

1. IoT Hub, "IoT cihazları" dikey penceresine tıklayın ve "yeni" seçeneğine tıklayın.

    ![Cihaz Ekle](./media/tutorial-iot-hub-maps/add-device.png)

2. Cihaz Oluştur sayfasında IoT cihazınızı adlandırın ve "Kaydet" e tıklayın.
    
    ![kayıt-cihaz](./media/tutorial-iot-hub-maps/register-device.png)

3. Cihazınızın **birincil bağlantı dizesini** , bu bağlantı dizesiyle birlikte bir yer tutucu değiştirmeniz gereken sonraki bir adımda kullanmak üzere kaydedin.

    ![Cihaz Ekle](./media/tutorial-iot-hub-maps/connection-string.png)

## <a name="upload-geofence"></a>Bölge bölge yükleme

Azure haritalar verilerini karşıya yükleme API 'sini kullanarak [bölge bölge 'Sini](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) Azure Maps hizmetine yüklemek Için [Postman uygulamasını](https://www.getpostman.com) kullanacağız. Araba bu bölge dışında olduğunda herhangi bir olay günlüğe kaydedilir.

Postman uygulamasını açın ve Azure Maps, veri yükleme API 'sini kullanarak bölge bölge 'yi karşıya yüklemek için aşağıdaki adımları izleyin.  

1. Postman uygulamasında yeni ' ye tıklayın | Yeni oluştur ve Istek seç. Bölge bölge verilerini karşıya yükleme için bir Istek adı girin, kaydedilecek bir koleksiyon veya klasör seçin ve Kaydet ' e tıklayın.

    ![Postman kullanarak bölge dilimleri yükleme](./media/tutorial-iot-hub-maps/postman-new.png)

2. Oluşturucu sekmesinde HTTP SONRASı yöntemi ' ni seçin ve POST isteği oluşturmak için aşağıdaki URL 'YI girin.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    URL yolundaki `dataFormat` parametresine karşılık gelen "geojson" değeri, karşıya yüklenen verilerin biçimini temsil eder.

3. **Parametreler**' e tıklayın ve post isteği URL 'si için kullanılacak aşağıdaki anahtar/değer çiftlerini girin. Abonelik anahtarı değerini Azure haritalar anahtarınızla değiştirin.
   
    ![Anahtar-değer params Postman](./media/tutorial-iot-hub-maps/postman-key-vals.png)

4. **Gövde** ' ye tıklayın, **Ham** giriş biçimi ' ni seçin ve açılan listeden giriş biçimi olarak **JSON (uygulama/metin)** öğesini seçin. [Burada](https://raw.githubusercontent.com/Azure-Samples/iothub-to-azure-maps-geofencing/master/src/Data/geofence.json?token=AKD25BYJYKDJBJ55PT62N4C5LRNN4)JSON veri dosyasını açın ve sonra, gövde bölümünde JSON öğesini karşıya yüklenecek veriler olarak kopyalayın ve **Gönder**' e tıklayın.
    
    ![veri Gönder](./media/tutorial-iot-hub-maps/post-json-data.png)
    
5. Yanıt üst bilgilerini gözden geçirin. Başarılı bir istek olduğunda, **konum** üstbilgisi karşıya yükleme isteğinin geçerli durumunu denetlemek IÇIN durum URI 'sini içerir. Durum URI 'SI aşağıdaki biçimde olacaktır. 

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

6. Durum URI 'nizi kopyalayın ve buna bir `subscription-key` parametresi ekleyin. Azure haritalar hesabı abonelik anahtarınızın değerini `subscription-key` parametresine atayın. Durum URI biçimi aşağıdaki gibi olmalıdır ve `{Subscription-key}` abonelik anahtarınızla değiştirilmelidir.

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. `udId` ' ı almak için, Postman uygulamasında yeni bir sekme açın ve Oluşturucu sekmesinde HTTP yöntemi Al ' ı seçin ve durum URI 'sinde bir GET isteği yapın. Karşıya veri yükleme işlemi başarılı olduysa yanıt gövdesinde bir UDID alırsınız. Uıdıd ' i daha sonra kullanmak üzere kopyalayın.

   ```JSON
   {
    "udid" : "{udId}"
   }
   ```


Daha sonra, "ContosoRental" kaynak grubu içinde bir Azure Işlevi oluşturacak ve cihaz telemetri iletilerini filtrelemek için IoT Hub bir ileti yolu ayarlayacağız.


## <a name="create-an-azure-function-and-add-an-event-grid-subscription"></a>Azure Işlevi oluşturma ve bir Event Grid aboneliği ekleme

Azure Işlevleri, işlem altyapısını açıkça sağlamaya veya yönetmeye gerek kalmadan isteğe bağlı olarak kod çalıştırmamızı sağlayan sunucusuz bir işlem hizmetidir. Azure Işlevleri hakkında daha fazla bilgi edinmek için [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) belgelerine göz atın. 

İşlevinde uygulamadığımız mantık, bölge durumunu değerlendirmek için araç içi cihaz telemetrisinden gelen konum verilerini kullanmaktır. Belirli bir araç bölge alanının dışında kaldığında, işlev [arama adresini al ters API 'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)aracılığıyla konumun adresi gibi daha fazla bilgi toplar. Bu API, belirli bir konum koordinatını insan tarafından anlaşılabilir bir sokak adresine çevirir. 

Tüm ilgili olay bilgileri daha sonra Blob deposunda tutulur. Aşağıdaki 5. adım, böyle bir mantığı uygulayan çalıştırılabilir kodu gösterir. BLOB depolama hesabındaki blob kapsayıcısına veri günlüklerini gönderen ve buna Event Grid bir abonelik ekleyen bir Azure Işlevi oluşturmak için aşağıdaki adımları izleyin.

1. Azure portal panosunda kaynak oluştur ' u seçin. Kullanılabilir kaynak türleri listesinden **işlem** ' ı seçin ve ardından **işlev uygulaması**' yi seçin.

    ![kaynak oluştur](./media/tutorial-iot-hub-maps/create-resource.png)

2. **İşlev uygulaması** oluşturma sayfasında, işlev uygulamanızı adlandırın. **Kaynak grubu**altında, **var olanı kullan**' ı seçin ve açılan listeden "ContosoRental" öğesini seçin. Çalışma zamanı yığını olarak ".NET Core" u seçin. **Barındırma**, **depolama hesabı**için bir önceki adımdan depolama hesabı adını seçin. Önceki adımımızda, **v2storage**depolama hesabını adlandırdık.  Ardından, **gözden geçir + oluştur**' u seçin.
    
    ![oluşturma-uygulama](./media/tutorial-iot-hub-maps/rental-app.png)

2. İşlev uygulaması ayrıntıları ' nı gözden geçirin ve "Oluştur" ' u seçin.

3. Uygulama oluşturulduktan sonra buna bir işlev eklememiz gerekir. İşlev uygulamasına gidin. Bir işlev eklemek için **yeni işlev** ' e tıklayın ve geliştirme ortamı olarak **Portal içi** ' yi seçin. Sonra **devam**' ı seçin.

    ![oluşturma-işlev](./media/tutorial-iot-hub-maps/function.png)

4. **Daha fazla şablon** seçin ve son ' a tıklayın **ve şablonları görüntüleyin**. 

5. **Azure Event Grid tetikleyicisi**olan şablonu seçin. İstenirse uzantıları yükler, işlevi adlandırın ve **Oluştur**' u seçin.

    ![işlev şablonu](./media/tutorial-iot-hub-maps/eventgrid-funct.png)
    
    **Azure Olay Hub 'ı tetikleyicisi** ve **Azure Event Grid tetikleyicisi** benzer simgelere sahiptir. **Azure Event Grid tetikleyiciyi**seçtiğinizden emin olun.

6. Kodu işlevinizin içine kopyalayın. [ C# ](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx)
 
7. C# Komut dosyasında aşağıdaki parametreleri değiştirin. **Kaydet** düğmesine tıklayın. Henüz **Çalıştır** 'ı tıklamayın
    * **SUBSCRIPTION_KEY** Azure Maps hesabı birincil abonelik anahtarınızla değiştirin.
    * **UDID** 'yi karşıya yüklediğiniz bölge sayısının UDID değeriyle değiştirin, 
    * Betikteki **Createblobasync** işlevi, veri depolama hesabında olay başına bir blob oluşturur. **ACCESS_KEY**, **account_name**ve **STORAGE_CONTAINER_NAME** değerini depolama hesabınızın erişim anahtarı, hesap adı ve veri depolama kapsayıcınızla değiştirin.

10. **Event Grid abonelik Ekle**' ye tıklayın.
    
    ![Add-Event-Grid](./media/tutorial-iot-hub-maps/add-egs.png)

11. Abonelik ayrıntılarını doldurun, **olay ABONELIĞI ayrıntıları** bölümünde olay aboneliğinizi adlandırın. Olay şeması için "Event Grid şeması" öğesini seçin. Konu **ayrıntıları** ' nın altında, konu türü olarak "Azure IoT Hub hesapları" nı seçin. Kaynak grubunu oluşturmak için kullandığınız aboneliği seçin, "kaynak grubu" olarak "ContosoRental" öğesini seçin. "Kaynak" olarak oluşturduğunuz IoT Hub seçin. **Cihaz telemetrisini** olay türü olarak seçin. Bu seçenekleri belirledikten sonra, "IoT Hub" için "konu türü" değişikliğini otomatik olarak görürsünüz.

    ![olay-kılavuz-abonelik](./media/tutorial-iot-hub-maps/af-egs.png)
 

## <a name="filter-events-using-iot-hub-message-routing"></a>IoT Hub ileti yönlendirme kullanarak olayları filtreleme

Azure Işlevine Event Grid aboneliği ekledikten sonra, IoT Hub **Ileti yönlendirme** dikey penceresinde Event Grid bir varsayılan ileti yolu görürsünüz. İleti yönlendirme, farklı veri türlerini çeşitli uç noktalara yönlendirmenizi sağlar. Örneğin, cihaz telemetri iletilerini, cihaz yaşam döngüsü olaylarını ve cihaz ikizi değişiklik olaylarını yönlendirebilirsiniz. IoT Hub ileti yönlendirme hakkında daha fazla bilgi için bkz. [IoT Hub ileti yönlendirmeyi kullanma](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c).

![Merkez-örn. rota](./media/tutorial-iot-hub-maps/hub-route.png)

Örnek senaryolarımızda, Kiralama aracın hareket ettiği tüm iletileri filtrelemek istiyoruz. Event Grid cihaz telemetri olaylarını yayımlamak için, `Engine` özelliğinin **"açık"** olduğu olayları filtrelemek için yönlendirme sorgusunu kullanacağız. IoT cihazdan buluta iletileri sorgulamak için çeşitli yollar vardır. ileti yönlendirme sözdizimi hakkında daha fazla bilgi edinmek için bkz. [IoT Hub Message Routing](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax). Yönlendirme sorgusu oluşturmak için, **Routetoeventgrid** yoluna tıklayın ve **yönlendirme SORGUSUNU** **' "üzerinde" Engine = '** ile değiştirin ve **Kaydet**' e tıklayın. Artık IoT Hub 'ı yalnızca altyapının açık olduğu cihaz telemetrisini yayımlayacak.

![Merkez-örn. filtre](./media/tutorial-iot-hub-maps/hub-filter.png)


## <a name="send-telemetry-data-to-iot-hub"></a>Telemetri verilerini IoT Hub gönder

Azure Işlevimiz çalışır duruma getirildikten sonra, artık IoT Hub telemetri verileri gönderebilir ve bu, Event Grid yönlendirir. Bir kira arabasının C# yerleşik bir cihazına ait konum verilerinin benzetimini yapmak için bir uygulama kullanalım. Uygulamayı çalıştırmak için geliştirme makinenizde .NET Core SDK 2.1.0 veya üzeri bir sürümü gerekir. IoT Hub benzetimi yapılan telemetri verilerini göndermek için aşağıdaki adımları izleyin.

1. [Rentalcarsimülasyon](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) C# projesini indirin. 

2. SimulatedCar.cs dosyasını istediğiniz bir metin düzenleyicisinde açın ve `connectionString` değerini, cihazı kaydettiğiniz sırada kaydettiğiniz değerle değiştirin ve değişiklikleri dosyaya kaydedin.
 
3. Makinenizde .NET Core yüklü olduğundan emin olun. Yerel Terminal pencerenizde, C# projenin kök klasörüne gidin ve sanal cihaz uygulaması için gerekli paketleri yüklemek üzere aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    dotnet restore
    ```

4. Aynı terminalde, Kiralık otomobil benzetim uygulamasını derlemek ve çalıştırmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

  Yerel terminalinizde aşağıdaki gibi görünmelidir.

  ![Terminal çıkışı](./media/tutorial-iot-hub-maps/terminal.png)

Blob Storage kapsayıcısını şimdi açarsanız, aracın bölge alanının dışında olduğu konumlar için dört blob görmeniz gerekir.

![Blobu girin](./media/tutorial-iot-hub-maps/blob.png)

Aşağıdaki haritada, araç, düzenli zaman aralıklarıyla günlüğe kaydedilen bölge sınırlarını aşacak olan dört noktayı gösterir.

![ihlalin eşlemesi](./media/tutorial-iot-hub-maps/violation-map.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide kullanılan Azure Haritalar API 'Lerini araştırmak için, bkz.:

* [Arama adresini tersine al](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Bölge bölge al](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Azure haritalar REST API 'lerinin tüm listesi için bkz.:

* [Azure haritalar REST API 'Leri](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

IoT Tak ve Kullan hakkında daha fazla bilgi edinmek için bkz.:

* [IoT Tak ve Kullan](https://docs.microsoft.com/azure/iot-pnp)

IoT için Azure Sertifikalı cihazların listesini almak için şu adresi ziyaret edin:

* [Azure Sertifikalı cihazlar](https://catalog.azureiotsolutions.com/)

Cihaza bulut telemetrisine nasıl gönderileceğini ve bu konuda daha fazla bilgi edinmek için bkz.:

* [Cihazdan telemetri gönderme](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet)
