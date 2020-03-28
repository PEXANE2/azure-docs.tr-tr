---
title: 'Öğretici: IoT mekansal analitik uygulayın | Microsoft Azure Haritaları'
description: IoT Hub'ı Microsoft Azure Haritalar hizmet API'leriyle tümleştirin.
author: philmea
ms.author: philmea
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: cfea9aa7bfcc9a9698bb93bdf54797481b8539ce
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333970"
---
# <a name="tutorial-implement-iot-spatial-analytics-using-azure-maps"></a>Öğretici: Azure Haritalarını kullanarak IoT mekansal analitiği uygulayın

Bir IoT senaryosunda, uzay ve zamanda meydana gelen ilgili olayları yakalamak ve izlemek yaygındır. Örnek senaryolar filo yönetimi, varlık izleme, mobilite ve akıllı şehir uygulamalarını içerir. Bu öğretici, Azure Haritalar API'larını kullanarak bir çözüm deseni boyunca size rehberlik eder. İlgili olaylar, Olay Izgarası tarafından sağlanan olay abonelik modeli kullanılarak IoT Hub tarafından yakalanır.

Bu eğitimde olacak:

> [!div class="checklist"]
> * Bir IoT Hub oluşturma.
> * Veri Yükleme API'sini kullanarak Azure Haritalar,Veri hizmetine coğrafi çit alanı yükleyin.
> * Azure İşlevler'de bir işlev oluşturun ve Azure Haritalar mekansal analizini temel alan iş mantığı nı uygulayın.
> * Etkinlik Ağıtı üzerinden Azure işlevinden IoT aygıt telemetri etkinliklerine abone olun.
> * IoT Hub ileti yönlendirmesini kullanarak telemetri olaylarını filtreleyin.
> * İlgili olay verilerini depolamak için bir depolama hesabı oluşturun.
> * Araç içi bir IoT cihazını simüle edin.
    

## <a name="use-case"></a>Kullanım örneği

Bu çözüm, bir araç kiralama şirketinin kiralık otomobilleri için olayları izlemeyi ve kaydetmeyi planladığı bir senaryoyu göstermektedir. Araba kiralama şirketleri genellikle belirli bir coğrafi bölgeye araba kiralamak. Kiralanırken arabaların nerede olduğunu takip etmeleri gerekiyor. Seçilen coğrafi bölgeden ayrılan bir aracın örnekleri günlüğe kaydedilmelidir. Verileri günlüğe kaydetme, politikaların, ücretlerin ve diğer iş yönlerinin düzgün şekilde ele alınmasını sağlar.

Bizim kullanım durumumuzda, kiralık otomobiller, Azure IoT Hub'a düzenli olarak telemetri verileri gönderen IoT cihazlarıyla donatılmıştır. Telemetri geçerli konumu içerir ve otomobilin motorunun çalışıp çalışmadığını gösterir. Cihaz konumu şeması [jeouzamsal veriler için](https://github.com/Azure/IoTPlugandPlay/blob/master/Schemas/geospatial.md)IoT Tak ve Çalıştır şemasına bağlıdır. Kiralık arabanın cihaz telemetri şeması gibi görünüyor:

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

Amacımıza ulaşmak için araç içi cihaz telemetrisini kullanalım. Geofencing kurallarını uygulamak istiyoruz. Ve, biz araba hareket ettiğini gösteren bir olay aldığımızda cevap vermek istiyorum. Bunu yapmak için, IoT Hub'dan Event Grid üzerinden cihaz telemetri etkinliklerine abone olacağız. 

Olay Izgara'ya abone olmanın birkaç yolu vardır, bu öğreticide Azure İşlevlerini kullanırız. Azure Fonksiyonları Olay Tablosu'nda yayınlanan olaylara tepki verir. Ayrıca, Azure Haritalar mekansal analizlerine dayanan araç kiralama iş mantığını da uygular. 

Azure işlevi içindeki kod, aracın coğrafi çitten ayrılıp ayrılmadığını denetler. Araç coğrafi çitten ayrıldıysa, Azure işlevi geçerli konumla ilişkili adres gibi ek bilgiler toplar. İşlev ayrıca, anlamlı olay verilerini olay koşullarının açıklamasını sağlamaya yardımcı olan bir veri blob depolamasında depolamak için mantık da uygular. 

Olay koşulları araç kiralama şirketi ve kiralama müşteri için yararlı olabilir. Aşağıdaki diyagram, sistemin üst düzey bir genel görünümünü sağlar.

 
  <center>

  ![Sisteme genel bakış](./media/tutorial-iot-hub-maps/system-diagram.png)
  
  </center>

Aşağıdaki şekil, mavi ile vurgulanan geofence alanını temsil eder. Kiralık aracın güzergahı yeşil bir hat ile gösterilir.

  ![Geofence rota](./media/tutorial-iot-hub-maps/geofence-route.png)


## <a name="prerequisites"></a>Ön koşullar 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bu öğreticideki adımları tamamlamak için öncelikle Azure portalında bir kaynak grubu oluşturmanız gerekir. Kaynak grubu oluşturmak için aşağıdaki adımları yapın:

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. **Kaynak grupları**’nı seçin.
    
   ![Kaynak grupları](./media/tutorial-iot-hub-maps/resource-group.png)

3. **Kaynak grupları altında** **Ekle'yi**seçin.
    
   ![Kaynak grubu ekleme](./media/tutorial-iot-hub-maps/add-resource-group.png) 

4. Aşağıdaki özellik değerlerini girin:
    * **Abonelik:** Azure aboneliğinizi seçin.
    * **Kaynak grubu:** Kaynak grubu adı olarak "ContosoRental" girin.
    * **Bölge:** Kaynak grubu için bir bölge seçin.  

    ![Kaynak grubu ayrıntıları](./media/tutorial-iot-hub-maps/resource-details.png)

    **Gözden Geçir + oluştur'u**seçin ve ardından sonraki sayfada **Oluştur'u** seçin.

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma 

Azure Haritalar mekansal analizini temel alan iş mantığını uygulamak için, oluşturduğumuz kaynak grubunda bir Azure Haritalar hesabı oluşturmamız gerekir. S1 fiyatlandırma katmanına sahip bir Azure Haritalar hesabı aboneliği oluşturmak için [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin. Hesabınız için [birincil](quick-demo-map-app.md#get-the-primary-key-for-your-account) anahtarı nızı almak için birincil anahtarı al'daki adımları izleyin. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](how-to-manage-authentication.md)bakın.



### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Olay verilerini günlüğe kaydetmek için, tüm Azure Depolama hizmetlerine erişim sağlayan genel amaçlı bir **v2depolama** oluştururuz: blobs, dosyalar, kuyruklar, tablolar ve diskler.  Verileri blob olarak depolamak için bu depolama hesabını "ContosoRental" kaynak grubuna yerleştirmemiz gerekir. Bir depolama hesabı oluşturmak için, bir depolama hesabı oluşturma yönergesine [uyun.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal) Sonra lekeleri depolamak için bir konteyner oluşturmamız gerekir. Bunu yapmak için aşağıdaki adımları izleyin:

1. "Depolama hesabınızda - blob, dosya, tablo, sıra" kapsayıcılara gidin.

    ![Blob](./media/tutorial-iot-hub-maps/blobs.png)

2. Sol üstteki kapsayıcı düğmesini tıklatın ve kapsayıcınızı "contoso-rental-logs" olarak adlandırın ve "Tamam"ı tıklatın.

    ![blob-konteyner](./media/tutorial-iot-hub-maps/blob-container.png)

3. Depolama hesabınızdaki **Access tuşlarına** gidin ve "depolama hesabı adı" ve "erişim anahtarı"nı kopyalayın. Daha sonraki bir adımda ihtiyaç duyulacak.

    ![erişim anahtarları](./media/tutorial-iot-hub-maps/access-keys.png)


Şimdi, olay verilerini kaydetmek için bir depolama hesabımız ve bir konteynerimiz var. Sonra, bir IoT hub'ı oluşturacağız.

### <a name="create-an-iot-hub"></a>IoT Hub'ı oluşturma

IoT Hub bulutta yönetilen bir hizmettir. IoT Hub, bir IoT uygulaması ile tarafından yönetilen aygıtlar arasında çift yönlü iletişim için merkezi bir ileti merkezi görevi görür. Aygıt telemetri iletilerini bir Olay Izgarasına yönlendirmek için "ContosoRental" kaynak grubunda bir IoT Hub'ı oluşturun. İletileri otomobilin motor durumuna göre filtrelediğimiz bir ileti rotası tümleştirmesi ayarlayın. Ayrıca araç hareket ettiğinde Olay Izgara'sına cihaz telemetri mesajları göndereceğiz.

> [!Note] 
> IoT Hub'ın Olay Grid'de aygıt telemetri olaylarını yayımlama işlevi Genel önizlemededir. Genel önizleme özellikleri **Doğu ABD, Batı ABD, Batı Avrupa, Azure Hükümeti, Azure China 21Vianet** ve **Azure Almanya**dışındaki tüm bölgelerde kullanılabilir. 

[Bir IoT Hub bölümü oluşturmadaki](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet#create-an-iot-hub)adımları izleyerek bir Iot Hub oluşturun.


### <a name="register-a-device"></a>Cihaz kaydetme 

IoT Hub'ına bağlanabilmek için bir aygıtın kaydedilmesi gerekir. Bir aygıtı IoT hub'ına kaydettirmek için aşağıdaki adımları izleyin:

1. IoT Hub'ınızda "IoT cihazları" bıçağına tıklayın ve "Yeni"yi tıklayın.

    ![add-device](./media/tutorial-iot-hub-maps/add-device.png)

2. Aygıt sayfası oluştur'da, IoT aygıtınızı adlandırın ve "Kaydet"i tıklatın.
    
    ![kayıt cihazı](./media/tutorial-iot-hub-maps/register-device.png)

3. Bu **bağlantı** dizesiyle bir yer tutucuyu değiştirmeniz gereken daha sonraki bir adımda kullanmak için aygıtınızın Birincil Bağlantı Dizesini kaydedin.

    ![add-device](./media/tutorial-iot-hub-maps/connection-string.png)

## <a name="upload-geofence"></a>Geofence yükle

[Geofence'i](https://docs.microsoft.com/azure/azure-maps/geofence-geojson) Azure Haritalar hizmetine Azure Haritalar yükleme API'sini kullanarak yüklemek için [Postacı uygulamasını](https://www.getpostman.com) kullanırız. Araba bu geofence dışında herhangi bir olay günlüğe kaydedilir.

Postacı uygulamasını açın ve Azure Haritalar, Veri Yükleme API'sini kullanarak geofence'i yüklemek için aşağıdaki adımları izleyin.  

1. Postacı uygulamasında, yeni | Yeni oluşturun ve İstek'i seçin. Coğrafi çit yükleme verileri için bir İstek adı girin, kaydetmek için bir koleksiyon veya klasör seçin ve Kaydet'i tıklatın.

    ![Postacı kullanarak geofences yükleyin](./media/tutorial-iot-hub-maps/postman-new.png)

2. Oluşturucu sekmesinde POST HTTP yöntemini seçin ve POST isteğinde bulunmak için aşağıdaki URL'yi girin.

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```
    
    URL yolundaki `dataFormat` parametreye karşı "geojson" değeri yüklenen verilerin biçimini temsil eder.

3. **Param'ları**tıklatın ve POSTA isteği URL'si için kullanılacak aşağıdaki Anahtar/Değer çiftlerini girin. Abonelik anahtarı değerini Azure Haritalar anahtarınızla değiştirin.
   
    ![Anahtar Değeri params Postacı](./media/tutorial-iot-hub-maps/postman-key-vals.png)

4. **Ardından Gövde'yi** **tıklatın** ve açılan listeden giriş biçimi olarak **JSON'u (uygulama/metin)** seçin. [Burada](https://raw.githubusercontent.com/Azure-Samples/iothub-to-azure-maps-geofencing/master/src/Data/geofence.json?token=AKD25BYJYKDJBJ55PT62N4C5LRNN4)JSON veri dosyasını açın ve yüklemek ve **Gönder'i**tıklatın veri olarak gövde bölümünde Json kopyalayın .
    
    ![veri gönderme](./media/tutorial-iot-hub-maps/post-json-data.png)
    
5. Yanıt üstbilgilerini gözden geçirin. Başarılı bir istek üzerine, **Konum** üstbilgisi yükleme isteğinin geçerli durumunu denetlemek için URI durumunu içerir. Uri durumu aşağıdaki biçimde olacaktır. 

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

6. Durumunuzu URI kopyalayın `subscription-key` ve ona bir parametre ekin. Azure Haritalar hesap abonelik anahtarınızın değerini `subscription-key` parametreye atayın. Durum URI biçimi aşağıdaki gibi olmalı `{Subscription-key}` ve abonelik anahtarıile değiştirilmelidir.

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

7. Almak için `udId` Postacı uygulamasında yeni bir sekme açın ve oluşturucu sekmesinde HTTP yöntemini alın'ı seçin ve uri durumunda get isteğinde bulunun. Veri yüklemeniz başarılı olduysa, yanıt gövdesinde bir udId alırsınız. Daha sonra kullanmak için udId'i kopyalayın.

   ```JSON
   {
    "udid" : "{udId}"
   }
   ```


Daha sonra "ContosoRental" kaynak grubunda bir Azure İşi oluşturacağız ve ardından aygıt telemetri iletilerini filtrelemek için IoT Hub'da bir ileti rotası kuracağız.


## <a name="create-an-azure-function-and-add-an-event-grid-subscription"></a>Azure İşlevi oluşturun ve Olay Ağı aboneliği ekleyin

Azure İşlevler, açık bir şekilde bilgi işlem altyapısını sağlama veya yönetmeye gerek kalmadan, isteğe bağlı kod çalıştırmamızı sağlayan sunucusuz bir bilgi işlem hizmetidir. Azure İşlevler hakkında daha fazla bilgi edinmek için [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) belgelerine göz atın. 

İşleviçinde uyguladığımız mantık, geofence durumunu değerlendirmek için araç içi cihaz telemetrisinden gelen konum verilerini kullanmaktır. Belirli bir aracın geofence dışına gitmesi durumunda, fonksiyon [Arama Adresi Ters API alın](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)üzerinden konumun adresi gibi daha fazla bilgi toplar. Bu API, belirli bir konum koordinatı insan tarafından anlaşılabilir bir sokak adresine çevirir. 

İlgili tüm olay bilgileri daha sonra blob deposunda tutulur. Adım 5 aşağıdaki bu tür mantık uygulayan yürütülebilir kod noktaları. Blob depolama hesabındaki blob kapsayıcısına veri günlükleri gönderen bir Azure İşlevi oluşturmak ve buna bir Olay Kılavuz aboneliği eklemek için aşağıdaki adımları izleyin.

1. Azure portal panosunda bir kaynak oluşturma seçeneğini belirleyin. Kullanılabilir kaynak türleri listesinden **Bilgi İşlem'i** seçin ve ardından **İşlev Uygulaması'nı**seçin.

    ![oluşturma-kaynak](./media/tutorial-iot-hub-maps/create-resource.png)

2. **İşlev Uygulaması** oluşturma sayfasında, işlev uygulamanızı adlandırın. **Kaynak Grubu**altında, **varolan ları kullan'ı**seçin ve açılan listeden "ContosoRental"ı seçin. Runtime Stack olarak ".NET Core" seçeneğini belirleyin. **Barındırma**altında, **Depolama hesabı**için, önceki bir adımdan depolama hesabı adını seçin. Önceki adımımızda, depolama hesabı **v2storage**adını verdik.  Ardından, **Gözden Geçir+Oluştur'u**seçin.
    
    ![oluşturma-uygulama](./media/tutorial-iot-hub-maps/rental-app.png)

2. İşlev uygulaması ayrıntılarını gözden geçirin ve "Oluştur"u seçin.

3. Uygulama oluşturulduktan sonra, uygulamaiçin bir işlev eklememiz gerekir. Fonksiyon uygulamasına gidin. Bir işlev eklemek için **Yeni işlevi** tıklatın ve geliştirme ortamı olarak **Portal'ı** seçin. Ardından Devam **et'i**seçin.

    ![oluşturma-fonksiyonu](./media/tutorial-iot-hub-maps/function.png)

4. **Daha fazla şablon** seçin ve **Şablonları Bitir ve görüntüle'yi**tıklatın. 

5. **Azure Olay Izgara Tetikleyicisi**ile şablonu seçin. İstenen uzantıları yükleyin, işlevi adlandırın ve **Oluştur'u**seçin.

    ![fonksiyon şablonu](./media/tutorial-iot-hub-maps/eventgrid-funct.png)
    
    **Azure Olay Hub Tetikleyicisi** ve Azure Olay **Izgara Tetikleyicisi** benzer simgelere sahiptir. **Azure Olay Izgara Tetikleyicisini**seçtiğinizden emin olun.

6. [C# kodunu](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/blob/master/src/Azure%20Function/run.csx) işlevinize kopyalayın.
 
7. C# komut dosyasında aşağıdaki parametreleri değiştirin. **Kaydet**'e tıklayın. Henüz **Çalıştır'ı** tıklatmayın
    * **SUBSCRIPTION_KEY** Azure Haritalar hesabı birincil abonelik anahtarınızla değiştirin.
    * **UDID'i** yüklediğiniz geofence'in udId'i ile değiştirin, 
    * Komut dosyasındaki **CreateBlobAsync** işlevi, veri depolama hesabında olay başına bir blob oluşturur. **ACCESS_KEY,** **ACCOUNT_NAME**ve **STORAGE_CONTAINER_NAME** depolama hesabınızın erişim anahtarı, hesap adı ve veri depolama kapsayıcısıyla değiştirin.

10. Olay **Izgara ekle aboneliğini**tıklatın.
    
    ![add-event-grid](./media/tutorial-iot-hub-maps/add-egs.png)

11. **EVENT SUBSCRIPTION DETAILS** altında abonelik bilgilerini doldurun, etkinlik aboneliğinizi adlandırın. Olay Şeması için "Olay Izgara Şeması"nı seçin. **KONU AYRINTILARI** altında Konu türü olarak "Azure IoT Hub Hesapları"nı seçin. Kaynak grubunu oluşturmak için kullandığınız aboneliği seçin, "Kaynak Grubu" olarak "ContosoRental"ı seçin. "Kaynak" olarak oluşturduğunuz IoT Hub'ını seçin. Olay Türü olarak **Aygıt Telemetrisini** seçin. Bu seçenekleri seçtikten sonra, "Konu Türü" değişikliğini otomatik olarak "IoT Hub"ı görürsünüz.

    ![olay-grid-abonelik](./media/tutorial-iot-hub-maps/af-egs.png)
 

## <a name="filter-events-using-iot-hub-message-routing"></a>IoT Hub ileti yönlendirmekullanarak olayları filtreleme

Azure İşlevi'ne Olay Izgara aboneliği ekledikten sonra, IoT Hub'ın **İleti Yönlendirme** bıçağında Olay Grid'ine varsayılan bir ileti rotası görürsünüz. İleti yönlendirme, farklı veri türlerini çeşitli uç noktalara yönlendirmenize olanak tanır. Örneğin, aygıt telemetri iletilerini, aygıt yaşam döngüsü olaylarını ve aygıt ikiz değiştirme olaylarını yönlendirebilirsiniz. IoT hub ileti yönlendirmesi hakkında daha fazla bilgi edinmek için Bkz. [IoT Hub ileti yönlendirmesini kullanın.](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)

![hub-EG-rota](./media/tutorial-iot-hub-maps/hub-route.png)

Örnek senaryomuzda, kiralık aracın hareket ettiği tüm iletileri filtrelemek istiyoruz. Bu tür aygıt telemetri olaylarını Olay Izgarası'nda yayımlamak için, özelliğin `Engine` **"AÇI"** olduğu olayları filtrelemek için yönlendirme sorgusunu kullanırız. İleti yönlendirme sözdizimi hakkında daha fazla bilgi edinmek için IoT aygıttan buluta iletileri sorgulamanın çeşitli yolları vardır, [Bkz. IoT Hub ileti yönlendirme.](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-routing-query-syntax) Yönlendirme sorgusu oluşturmak için **RouteToEventGrid** rotasına tıklayın ve **Yönlendirme sorgusunu** **"Engine='ON'** ile değiştirin ve **Kaydet'i**tıklatın. Şimdi IoT hub sadece Motor AÇI OLDUĞU cihaz telemetri yayınlayacak.

![hub-EG filtresi](./media/tutorial-iot-hub-maps/hub-filter.png)


## <a name="send-telemetry-data-to-iot-hub"></a>Telemetri verilerini IoT Hub'ına gönderme

Azure İşlevimiz çalışmaya başladıktan sonra, telemetri verilerini Artık IoT Hub'ına gönderebiliriz ve bu verileri Olay Izgarasına yönlendirebiliriz. Kiralık bir aracın araç içi aygıtının konum verilerini simüle etmek için C# uygulamasını kullanalım. Uygulamayı çalıştırmak için geliştirme makinenizde .NET Core SDK 2.1.0 veya daha büyük olması gerekir. IoT Hub'a simüle edilmiş telemetri verileri göndermek için aşağıdaki adımları izleyin.

1. [RentalCarSimulation](https://github.com/Azure-Samples/iothub-to-azure-maps-geofencing/tree/master/src/rentalCarSimulation) C# projesini indirin. 

2. simulatedCar.cs dosyasını seçtiğiniz bir metin düzenleyicisinde açın ve `connectionString` aygıtı kaydettirdiğinizde kaydettiğiniz dosyanın değerini değiştirin ve dosyadaki değişiklikleri kaydedin.
 
3. Makinenizde .NET Core yüklü olduğundan emin olun. Yerel terminal pencerenizde, C# projesinin kök klasörüne gidin ve benzetimli aygıt uygulaması için gerekli paketleri yüklemek için aşağıdaki komutu çalıştırın:
    
    ```cmd/sh
    dotnet restore
    ```

4. Aynı terminalde, kiralık araç simülasyonu uygulamasını oluşturmak ve çalıştırmak için aşağıdaki komutu çalıştırın:

    ```cmd/sh
    dotnet run
    ```

  Yerel terminaliniz aşağıdaki gibi görünmelidir.

  ![Terminal çıkışı](./media/tutorial-iot-hub-maps/terminal.png)

Şimdi blob depolama konteyner açarsanız, aracın geofence dışında olduğu yerler için dört blobs görmek gerekir.

![Blob girin](./media/tutorial-iot-hub-maps/blob.png)

Aşağıdaki harita, aracın geofence dışında olduğu dört nokta gösterir, düzenli zaman aralıklarında oturum.

![ihlal haritası](./media/tutorial-iot-hub-maps/violation-map.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde kullanılan Azure Haritalar API'larını incelemek için bkz:

* [Arama Adresini Ters Alma](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Geofence alın](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

Azure Haritalar REST API'lerinin tam listesi için bkz:

* [Azure Haritalar REST API'leri](https://docs.microsoft.com/rest/api/maps/spatial/getgeofence)

IoT Tak ve Çalıştır hakkında daha fazla bilgi edinmek için bkz:

* [IoT Tak Çalıştır](https://docs.microsoft.com/azure/iot-pnp)

IoT için Azure sertifikalı cihazların listesini almak için aşağıdakileri ziyaret edin:

* [Azure sertifikalı cihazlar](https://catalog.azureiotsolutions.com/)

Aygıtı bulut telemetrisine nasıl göndereceğiz ve diğer yol hakkında daha fazla bilgi edinmek için bkz:

* [Cihazdan telemetri gönderme](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-dotnet)
