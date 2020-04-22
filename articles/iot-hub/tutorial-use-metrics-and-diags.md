---
title: Azure IoT hub'ı ile ölçümleri ve tanılama günlüklerini ayarlama ve kullanma
description: Azure IoT hub'ı ile ölçümleri ve tanılama günlüklerini nasıl ayarlayıp kullanacağınızı öğrenin. Bu, hub'ınızın sahip olabileceği sorunları tanılamaya yardımcı olmak için çözümlenecek veriler sağlar.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 3/13/2019
ms.author: robinsh
ms.custom:
- mvc
- mqtt
ms.openlocfilehash: 3eda4cd8dc10bd9128186b2ff4f8d6ac0254fe5d
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770607"
---
# <a name="tutorial-set-up-and-use-metrics-and-diagnostic-logs-with-an-iot-hub"></a>Öğretici: IoT hub'ı ile ölçümleri ve tanılama günlüklerini ayarlama ve kullanma

Üretimde çalışan bir IoT Hub çözümünüz varsa, bazı ölçümler ayarlamak ve tanılama günlüklerini etkinleştirmek istiyorsunuz. Daha sonra bir sorun oluşursa, sorunu tanılamanıza ve daha hızlı bir şekilde düzeltmenize yardımcı olacak verilere sahip olursunuz. Bu makalede, tanılama günlüklerini nasıl etkinleştireceğiniz ve hatalar için bunları nasıl denetleyeceksiniz göreceğiniz. Ayrıca izlemek için bazı ölçümler ayarlarsınız ve ölçümler belirli bir sınıra geldiğinde bu uyarıyı uyarır. Örneğin, gönderilen telemetri iletilerinin sayısı belirli bir sınırı aştığında veya kullanılan ileti sayısı IoT Hub'ı için günde izin verilen ileti kotasına yaklaştığında size bir e-posta göndermiş olabilirsiniz. 

Bir örnek kullanım örneği pompalar bir IoT hub ile iletişim göndermek IoT cihazlar olduğu bir benzin istasyonudur. Kredi kartları doğrulanır ve son işlem bir veri deposuna yazılır. IoT aygıtları hub'a bağlanmayı ve ileti göndermeyi durdurursa, neler olup bittiğini görünürlüğünüz yoksa bunu düzeltmek çok daha zordur.

Bu öğretici, IoT hub'ına ileti göndermek için [IoT Hub Yönlendirme'deki](tutorial-routing.md) Azure örneğini kullanır.

Bu öğreticide, aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Azure CLI'yi kullanarak bir IoT hub'ı, benzetimli bir aygıt ve bir depolama hesabı oluşturun.  
> * Tanılama günlüklerini etkinleştirin.
> * Ölçümleri etkinleştirin.
> * Bu ölçümler için uyarılar ayarlayın. 
> * Hub'a ileti gönderen bir IoT aygıtını taklit eden bir uygulamayı indirin ve çalıştırın. 
> * Uyarılar çalışmaya başlayana kadar uygulamayı çalıştırın. 
> * Ölçüm sonuçlarını görüntüleyin ve tanı günlüklerini kontrol edin. 

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

- [Visual Studio'u](https://www.visualstudio.com/)yükleyin. 

- Posta alabilen bir e-posta hesabı.

- 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu öğreticideki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="set-up-resources"></a>Kaynakları ayarlama

Bu öğretici için bir IoT hub'ı, bir depolama hesabı ve simüle edilmiş bir IoT aygıtına ihtiyacınız vardır. Bu kaynaklar Azure CLI veya Azure PowerShell kullanılarak oluşturulabilir. Tüm kaynaklar için aynı kaynak grubunu ve konumunu kullanın. Sonunda kaynak grubunu silerek her şeyi tek adımda kaldırabilirsiniz.

Bunlar gerekli adımlardır.

1. Kaynak [grubu](../azure-resource-manager/management/overview.md)oluşturun. 

2. Bir IoT hub'ı oluşturun.

3. Standard_LRS çoğaltmasıyla standart bir V1 depolama hesabı oluşturun.

4. Hub'ınıza iletiler gönderen simülasyon cihazı için cihaz kimliği oluşturun. Test aşaması için anahtarı kaydedin.

### <a name="set-up-resources-using-azure-cli"></a>Azure CLI kullanarak kaynak ayarlama

Bu betiği kopyalayıp Cloud Shell'e yapıştırın. Zaten oturum açmış olduğunuzu varsayarak, betiği bir kerede bir satır olmak üzere çalıştırır. Yeni kaynaklar ContosoResources kaynak grubunda oluşturulur.

Genel olarak benzersiz olması gereken değişkenlere `$RANDOM` eklenmiştir. Betik çalıştırıldığında ve değişkenler ayarlandığında, rastgele bir sayısal dize oluşturulur ve sabit dizenin sonuna eklenerek benzersiz hale getirilmesi sağlanır.

```azurecli-interactive

# This is the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-iot

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
location=westus
resourceGroup=ContosoResources
iotDeviceName=Contoso-Test-Device 

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, so add a random number to the end.
iotHubName=ContosoTestHub$RANDOM
echo "IoT hub name = " $iotHubName

# Create the IoT hub in the Free tier.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku F1 --location $location

# The storage account name must be globally unique, so add a random number to the end.
storageAccountName=contosostoragemon$RANDOM
echo "Storage account name = " $storageAccountName

# Create the storage account.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName 

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

```

>[!NOTE]
>Aygıt kimliğini oluştururken aşağıdaki hatayı alabilirsiniz: *IoT Hub ContosoTestHub'ın iothubowner ilkesi için anahtar bulunamadı.* Bu hatayı düzeltmek için Azure CLI IoT Uzantısı'nı güncelleştirin ve komut dosyasındaki son iki komutu yeniden çalıştırın. 
>
>Burada uzantıyı güncelleştirmek için komutu. Bunu Bulut Kabuğu örneğinde çalıştırın.
>
>```cli
>az extension update --name azure-iot
>```

## <a name="enable-the-diagnostic-logs"></a>Tanılama günlüklerini etkinleştirme 

Yeni bir IoT hub'ı oluşturduğunuzda [tanılama günlükleri](../azure-monitor/platform/platform-logs-overview.md) varsayılan olarak devre dışı bırakılır. Bu bölümde, hub'ınız için tanılama günlüklerini etkinleştirin.

1. İlk olarak, portalda hub'ınızda zaten değilseniz, **Kaynak gruplarını** tıklatın ve Contoso-Resources kaynak grubunu tıklatın. Görüntülenen kaynaklar listesinden hub'ı seçin. 

2. IoT Hub bıçağındaki **İzleme** bölümüne bakın. **Tanılama ayarları**'na tıklayın. 

   ![IoT Hub bıçağının tanı ayarları nın bir parçasını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/01-diagnostic-settings.png)


3. Abonelik ve kaynak grubunun doğru olduğundan emin olun. **Kaynak Türü**altında, **Tümünü Seç'in**işaretlerini kaldırın, ardından **IoT Hub'ı**arayın ve denetleyin. (Yine *Tümünü Seç'in* yanındaki onay işaretini koyar, yalnızca yoksay.) **Kaynak**altında hub adını seçin. Ekranınız bu görüntüye benzemeli: 

   ![IoT Hub bıçağının tanı ayarları nın bir parçasını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/02-diagnostic-settings-start.png)

4. Şimdi **tanılamayı aç'ı**tıklatın. Tanılama ayarları bölmesi görüntülenir. Tanılama günlükleri ayarlarınızın adını "diags-hub" olarak belirtin.

5. **Arşiv'i bir depolama hesabına**denetleyin. 

   ![Tanılamanın bir depolama hesabına arşivleme ayarını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/03-diagnostic-settings-storage.png)

    **Depolama hesabı seç** ekranını görmek için **Yapıl'ı** tıklatın, doğru ekranı seçin *(contosostoragemon),* ve Tanılama ayarları bölmesine dönmek için **Tamam'ı** tıklatın. 

   ![Tanılama günlüklerinin bir depolama hesabına arşivlene ayarını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/04-diagnostic-settings-after-storage.png)

6. **LOG** **altında, Bağlantıları** ve **Aygıt Telemetrisini**denetleyin ve **Bekletme'yi (gün)** her biri için 7 güne ayarlayın. Tanılama ayarları ekranınız artık şu görüntüye benzemeli:

   ![Son tanı günlüğü ayarlarını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/05-diagnostic-settings-done.png)

7. Ayarları kaydetmek için **Kaydet**’e tıklayın. Tanılama ayarları bölmesini kapatın.

Daha sonra, tanılama günlüklerine baktığınızda, aygıtın bağlantı sını görebilir ve günlüğe kaydetmeyi kesebilirsiniz. 

## <a name="set-up-metrics"></a>Ölçümleri ayarlama 

Artık iletilerin hub'a ne zaman gönderildiğine dikkat etmek için bazı ölçümler ayarlayın. 

1. IoT hub'ı için ayarlar **bölmesinde, İzleme** bölümündeki **Ölçümler** seçeneğini tıklatın.

2. Ekranın üst kısmında, **Son 24 saat (Otomatik)** tıklatın. Görünen açılır durumda, **Zaman Aralığı**için Son **4 saat'i** seçin ve **Zaman Taneciklerini** yerel saate **1 dakikaya**ayarlayın. Bu ayarları kaydetmek için **Uygula'yı** tıklatın. 

   ![Ölçümler zaman ayarlarını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/06-metrics-set-time-range.png)

3. Varsayılan olarak bir metrik giriş vardır. Kaynak grubunu varsayılan olarak ve metrik ad alanı olarak bırakın. **Metrik** açılır listesinde, **gönderilen Telemetri iletilerini**seçin. **Toplamayı** **Toplamolarak**Ayarlayın.

   ![Gönderilen telemetri iletileri için metrik eklemeyi gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/07-metrics-telemetry-messages-sent.png)


4. Şimdi **grafiğe** başka bir metrik eklemek için metrik ekle'yi tıklatın. Kaynak grubunuzu seçin (**ContosoTestHub**). **Metrik'in**altında, **kullanılan ileti sayısının toplamını**seçin. **Toplama** **için, Avg'yi**seçin. 

   Şimdi *ekranınız, gönderilen Telemetri iletileri*için en aza indirgenmiş ölçütü ve *kullanılan toplam ileti sayısı*için yeni metriği gösterir.

   ![Gönderilen telemetri iletileri için metrik eklemeyi gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/07-metrics-num-messages-used.png)

   **Panoya Sabitle'yi**tıklatın. Yeniden erişebilmeniz için Azure portalınızın panosuna sabitler. Panoya sabitlemezseniz, ayarlarınız tutulmaz.

## <a name="set-up-alerts"></a>Uyarıları ayarlama

Portaldaki merkeze git. **Kaynak Grupları'nı**tıklatın, *ContosoResources'ı*seçin, ardından IoT Hub *ContosoTestHub'ı*seçin. 

IoT Hub henüz [Azure Monitor'daki ölçümlere](/azure/azure-monitor/platform/data-collection#metrics) geçirilemedi; [klasik uyarıları](/azure/azure-monitor/platform/alerts-classic.overview)kullanmak zorunda.

1. **İzleme**altında, **Uyarıları** tıklatın Bu ana uyarı ekranını gösterir. 

   ![Klasik uyarıların nasıl bulunup bulunulacağı ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/08-find-classic-alerts.png)

2. Klasik uyarılara buradan ulaşmak için **klasik uyarıları görüntüle'yi**tıklatın. 

    ![Klasik uyarılar ekranını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/09-view-classic-alerts.png)

    Şu alanları doldurun: 

    **Abonelik**: Bu alanı geçerli aboneliğinize bırakın.

    **Kaynak**: Bu alanı *Ölçümler*olarak ayarlayın.

    **Kaynak grubu**: Bu alanı geçerli kaynak grubunuz *contosoResources*olarak ayarlayın. 

    **Kaynak türü**: Bu alanı IoT Hub olarak ayarlayın. 

    **Kaynak**: IoT hub'ınız *ContosoTestHub'ı*seçin.

3. Yeni bir uyarı ayarlamak için **metrik uyarı ekle'yi (klasik)** tıklatın.

    Şu alanları doldurun:

    **Ad**: *Telemetri-iletileri*gibi uyarı kuralınız için bir ad verin.

    **Açıklama**: *1000 telemetri iletisi gönderildiğinde uyarı*gibi uyarınızın açıklamasını sağlayın. 

    **Kaynak**: Bunu *Ölçümler*olarak ayarlayın.

    **Abonelik**, **Kaynak grubu**ve **Kaynak,** görünüm klasik **uyarıları** ekranında seçtiğiniz değerlere ayarlanmalıdır. 

    **Metrik'i** *gönderilen Telemetri iletilerine*ayarlayın.

    ![Gönderilen telemetri iletileri için klasik bir uyarı ayarını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/10-alerts-add-rule-telemetry-top.png)

4. Grafikten sonra aşağıdaki alanları ayarlayın:

   **Koşul**: *Büyük*olarak ayarlayın.

   **Eşik**: 1000 olarak ayarlayın.

   **Periyot**: *Son 5 dakikaya*göre ayarlanır.

   **Bildirim e-posta alıcıları**: E-posta adresinizi buraya koyun. 

   ![Uyarılar ekranının alt yarısını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/11-alerts-add-rule-bottom.png)

   Uyarıyı kaydetmek için **Tamam'ı** tıklatın. 

5. Şimdi *kullanılan iletilerin toplam sayısı*için başka bir uyarı ayarlayın. Bu metrik, kullanılan ileti sayısı IoT hub'ının kotasına yaklaşırken bir uyarı göndermek istiyorsanız, hub'ın yakında iletileri reddetmeye başlayacağını bildirmek için yararlıdır.

   Klasik **uyarıları Görüntüle** ekranında **metrik uyarı ekle 'yi (klasik)** tıklatın ve ardından Ekle **kuralı** bölmesinde bu alanları doldurun.

   **Ad**: *Kullanılan ileti sayısı*gibi uyarı kuralınız için bir ad verin.

   **Açıklama**: *Kotaya yaklaşırken uyarı*gibi uyarınızın açıklamasını sağlayın.

   **Kaynak**: Bu alanı *Ölçümler*olarak ayarlayın.

    **Abonelik**, **Kaynak grubu**ve **Kaynak,** görünüm klasik **uyarıları** ekranında seçtiğiniz değerlere ayarlanmalıdır. 

    **Metrik'i** *kullanılan ileti lerin toplam sayısına*ayarlayın.

6. Grafiğin altında, aşağıdaki alanları doldurun:

   **Koşul**: *Büyük*olarak ayarlayın.

   **Eşik**: 1000 olarak ayarlayın.

   **Dönem**: Bu alanı *son 5 dakikaya*göre ayarlayın. 

   **Bildirim e-posta alıcıları**: E-posta adresinizi buraya koyun. 

   Kuralı kaydetmek için **Tamam**’a tıklayın. 

5. Şimdi klasik uyarılar bölmesinde iki uyarı görmelisiniz: 

   ![Yeni uyarı kurallarıyla klasik uyarılar ekranını gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/12-alerts-done.png)

6. Uyarı bölmesini kapatın. 
    
    Bu ayarlarla, gönderilen ileti sayısı 400'den büyük olduğunda ve kullanılan toplam ileti sayısı NUMBER'ı aştığında bir uyarı alırsınız.

## <a name="run-simulated-device-app"></a>Simülasyon Cihazı uygulamasını çalıştırma

Betik ayarlama bölümünün başlarında, IoT cihazı kullanarak simülasyonu yapılacak bir cihaz ayarlamıştınız. Bu bölümde, IoT Hub'a cihazdan buluta iletiler gönderen bir cihazın simülasyonunu yapan bir .NET konsol uygulaması indireceksiniz.  

[IoT Cihaz Simülasyonu](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) çözümünü indirin. Bu bağlantı içinde çeşitli uygulamalar ile bir repo indirir; aradığınız çözüm iot-hub/Tutorials/Routing/' dedir.

Kodu Visual Studio'da açmak için çözüm dosyasına (SimulatedDevice.sln) çift tıklayın, sonra da Program.cs'yi açın. `{iot hub hostname}` değerini IoT hub'ı konak adıyla değiştirin. IoT hub'ı konak adı **{iot-hub-adı}.azure-devices.net** biçimindedid. bu öğreticide, hub konak adı olarak **ContosoTestHub.azure-devices.net** kullanılır. Ardından, `{device key}` değerini daha önce simülasyon cihazını ayarlarken kaydettiğiniz cihaz anahtarıyla değiştirin. 

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHub.azure-devices.net";
        // This is the primary key for the device. This is in the portal. 
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key. 
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Çalıştırma ve test etme 

Program.cs, ileti `Task.Delay` gönderme arasındaki süreyi 1 saniyeden 0,01 saniyeye düşüren 1000'den 10'a değiştirin. Bu gecikmenin kısaltılması gönderilen ileti sayısını artırır.

```csharp
await Task.Delay(10);
```

Konsol uygulamasını çalıştırın. Birkaç dakika bekleyin (10-15). Benzetimli aygıttan uygulamanın konsol ekranındaki hub'a gönderilen iletileri görebilirsiniz.

### <a name="see-the-metrics-in-the-portal"></a>Portaldaki ölçümlere bakın

Ölçümlerinizi Pano'dan açın. Saat değerlerini *1 dakikalık*bir zaman parçalı ile *Son 30 dakika* olarak değiştirin. Gönderilen telemetri iletilerini ve grafikte kullanılan toplam ileti sayısını ve grafiğin altındaki en son sayıları gösterir.

   ![Ölçümleri gösteren ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/13-metrics-populated.png)

### <a name="see-the-alerts"></a>Uyarıları görme

Uyarılara geri dön. **Kaynak gruplarını**tıklatın, *ContosoResources'ı*seçin, ardından *ContosoTestHub*merkezini seçin. Hub için görüntülenen özellikler sayfasında **Uyarılar'ı**seçin ve **ardından klasik uyarıları görüntüleyin.** 

Gönderilen ileti sayısı sınırı aştığında, e-posta uyarıları almaya başlarsınız. Etkin uyarılar olup olmadığını görmek için hub'ınıza gidin ve **Uyarılar'ı**seçin. Etkin olan uyarıları ve herhangi bir uyarı varsa size gösterir. 

   ![Uyarıları gösteren ekran görüntüsü ateşlendi.](./media/tutorial-use-metrics-and-diags/14-alerts-firing.png)

Telemetri iletileri için uyarıyı tıklatın. Bu metrik sonucu ve sonuçları ile bir grafik gösterir. Ayrıca, uyarı ateş sizi uyarmak için gönderilen e-posta bu görüntü gibi görünüyor:

   ![Uyarıları gösteren e-postanın ekran görüntüsü ateşlendi.](./media/tutorial-use-metrics-and-diags/15-alert-email.png)

### <a name="see-the-diagnostic-logs"></a>Tanılama günlüklerine bakın

Tanı günlüklerinizi blob depolamasına dışa aktarılacak şekilde ayarlarsınız. Kaynak grubunuza gidin ve depolama hesabınızı *contosostoragemon'unuzu*seçin. Blobs'u seçin, ardından kapsayıcı *öngörülerini-günlükleri-bağlantıları*açın. Geçerli tarihe gelene kadar ayrıntıya inin ve en son dosyayı seçin. 

   ![Tanılama günlüklerini görmek için depolama kabına sondaj ekran görüntüsü.](./media/tutorial-use-metrics-and-diags/16-diagnostics-logs-list.png)

İndir'i indirmek ve açmak için **İndir'i** tıklatın. Hub'a ileti gönderirken aygıtın bağlanan ve bağlantı kesilen günlüklerini görürsünüz. Burada bir örnek:

``` json
{ 
  "time": "2018-12-17T18:11:25Z", 
  "resourceId": 
    "/SUBSCRIPTIONS/your-subscription-id/RESOURCEGROUPS/CONTOSORESOURCES/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/CONTOSOTESTHUB", 
  "operationName": "deviceConnect", 
  "category": "Connections", 
  "level": "Information", 
  "properties": 
      {"deviceId":"Contoso-Test-Device",
       "protocol":"Mqtt",
       "authType":null,
       "maskedIpAddress":"73.162.215.XXX",
       "statusCode":null
       }, 
  "location": "westus"
}
{ 
   "time": "2018-12-17T18:19:25Z", 
   "resourceId": 
     "/SUBSCRIPTIONS/your-subscription-id/RESOURCEGROUPS/CONTOSORESOURCES/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/CONTOSOTESTHUB", 
    "operationName": "deviceDisconnect", 
    "category": "Connections", 
    "level": "Error", 
    "resultType": "404104", 
    "resultDescription": "DeviceConnectionClosedRemotely", 
    "properties": 
        {"deviceId":"Contoso-Test-Device",
         "protocol":"Mqtt",
         "authType":null,
         "maskedIpAddress":"73.162.215.XXX",
         "statusCode":"404"
         }, 
    "location": "westus"
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme 

Bu eğitimde oluşturduğunuz tüm kaynakları kaldırmak için kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda, IoT hub'ını, depolama hesabını ve kaynak grubunun kendisini kaldırır. Gösterge paneline sabitlenmiş ölçümleriniz varsa, her birinin sağ üst köşesindeki üç noktayı tıklatıp **Kaldır'ı**seçerek bunları el ile kaldırmanız gerekir.

Kaynak grubunu kaldırmak için [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanın.

```azurecli-interactive
az group delete --name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri gerçekleştirerek ölçümleri ve tanılama günlüklerini nasıl kullanacağınızı öğrendiniz:

> [!div class="checklist"]
> * Azure CLI'yi kullanarak bir IoT hub'ı, benzetimli bir aygıt ve bir depolama hesabı oluşturun.  
> * Tanılama günlüklerini etkinleştirin. 
> * Ölçümleri etkinleştirin.
> * Bu ölçümler için uyarılar ayarlayın. 
> * Hub'a ileti gönderen bir IoT aygıtını taklit eden bir uygulamayı indirin ve çalıştırın. 
> * Uyarılar çalışmaya başlayana kadar uygulamayı çalıştırın. 
> * Ölçüm sonuçlarını görüntüleyin ve tanı günlüklerini kontrol edin. 

IoT cihazı durumunun nasıl yönetileceğini öğrenmek için sonraki öğreticiye geçin. 

> [!div class="nextstepaction"]
> [Cihazlarınızı bir arka uç hizmetinden yapılandırma](tutorial-device-twins.md)
