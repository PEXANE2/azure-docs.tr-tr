---
title: Aygıt simülasyonu ile Protokol Arabelleklerini Kullanma - Azure| Microsoft Dokümanlar
description: Bu nasıl yap'ı yapma kılavuzunda, Aygıt Simülasyonu çözüm hızlandırıcısından gönderilen telemetriyi seri hale getirmek için Protokol Aracılarını nasıl kullanacağınızı öğrenirsiniz.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/06/2018
ms.author: dobett
ms.openlocfilehash: bc08cd5183bcaac6cb77ccb0938b07893f082862
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78250219"
---
# <a name="serialize-telemetry-using-protocol-buffers"></a>Protokol Arabelleklerini kullanarak telemetriyi seri hale

Protokol Arabellekleri (Protobuf), yapılandırılmış veriler için ikili serileştirme biçimidir. Protobuf, XML'den daha küçük ve hızlı olma hedefiyle sadeliği ve performansı vurgulamak üzere tasarlanmıştır.

Aygıt Simülasyonu, protokol arabellek dilinin **proto3** sürümünü destekler.

Protobuf verileri serihale getirmek için derlenmiş kod gerektirdiğinden, Aygıt Simülasyonu'nun özel bir sürümünü oluşturmanız gerekir.

Bu nasıl yapılacağını gösteren adımlar, nasıl yapılacağını gösterir:

1. Geliştirme ortamı hazırlama
1. Bir aygıt modelinde Protobuf biçimini kullanmanın belirtin
1. Protobuf biçiminizi tanımlayın
1. Protobuf sınıfları oluşturma
1. Yerel olarak test edin

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapIlenler kılavuzundaki adımları izlemek için şunları yapmanız gerekir:

* Visual Studio Code. [Mac, Linux ve Windows için Visual Studio Kodu](https://code.visualstudio.com/download)indirebilirsiniz.
* .NET Çekirdek. [Mac, Linux ve Windows için .NET Core'u](https://www.microsoft.com/net/download)indirebilirsiniz.
* Postacı. [Mac, windows veya Linux için Postacı'yı](https://www.getpostman.com/apps)indirebilirsiniz.
* Azure aboneliğinize dağıtılan bir [IoT hub'ı.](../iot-hub/iot-hub-create-through-portal.md) Bu kılavuzdaki adımları tamamlamak için IoT hub'ın bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini Azure portalından alabilirsiniz.
* SQL API'yi kullanan ve [güçlü tutarlılık](../cosmos-db/manage-account.md)için yapılandırılan Azure aboneliğinize dağıtılan bir [Cosmos DB veritabanı.](../cosmos-db/create-sql-api-dotnet.md#create-account) Bu kılavuzdaki adımları tamamlamak için Cosmos DB veritabanının bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini Azure portalından alabilirsiniz.
* Azure aboneliğinize dağıtılan bir [Azure depolama hesabı.](../storage/common/storage-account-create.md) Bu kılavuzdaki adımları tamamlamak için depolama hesabının bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini Azure portalından alabilirsiniz.

## <a name="prepare-your-development-environment"></a>Geliştirme ortamınızı hazırlama

Geliştirme ortamınızı hazırlamak için aşağıdaki görevleri tamamlayın:

* Cihaz simülasyonu microservice için kaynak indirin.
* Depolama bağdaştırıcısı microservice için kaynağı indirin.
* Depolama bağdaştırıcısını microservice'i yerel olarak çalıştırın.

Bu makaledeki yönergeler Windows kullandığınızı varsayar. Başka bir işletim sistemi kullanıyorsanız, bazı dosya yollarını ve komutlarını ortamınıza uyacak şekilde ayarlamanız gerekebilir.

### <a name="download-the-microservices"></a>Mikro hizmetleri indirin

Uzaktan [İzleme Mikrohizmetlerini](https://github.com/Azure/remote-monitoring-services-dotnet/archive/master.zip) GitHub'dan yerel makinenizde uygun bir konuma indirin ve indirin. Bu depo, bu nasıl yapılsA için ihtiyacınız olan depolama bağdaştırıcısı microservice içerir.

Cihaz [simülasyonu microservice'i](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) GitHub'dan yerel makinenizde uygun bir konuma indirin ve indirin.

### <a name="run-the-storage-adapter-microservice"></a>Depolama adaptörmikroservice çalıştırın

Visual Studio Code'da **uzaktan izleme-hizmetler-dotnet-master\storage-adaptör** klasörünü açın. Çözülmemiş bağımlılıkları gidermek için geri **yükleme** düğmelerini tıklatın.

**.vscode/launch.json** dosyasını açın ve Cosmos DB bağlantı dizenizi **PCS\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** ortam değişkenine atayın.

> [!NOTE]
> Mikro hizmeti makinenizde yerel olarak çalıştırdığınızda, düzgün çalışması için Azure'da bir Cosmos DB örneği gerektirir.

Depolama bağdaştırıcısı microservice'i yerel olarak çalıştırmak için **Hata \> Ayıklama Başlat Hata Ayıklama'yı**tıklatın.

Visual Studio Code'daki **Terminal** penceresi, web hizmeti sağlık kontrolü için bir <http://127.0.0.1:9022/v1/status>URL de dahil olmak üzere çalışan mikro hizmetten çıktıyı gösterir: . Bu adrese gidince, durum "Tamam: Canlı ve iyi" olmalıdır.

Aşağıdaki adımları tamamlarken depolama bağdaştırıcısı microservice Visual Studio Code bu durumda çalışan bırakın.

## <a name="define-your-device-model"></a>Cihaz modelinizi tanımlayın

GitHub'dan indirdiğiniz **aygıt simülasyonu-dotnet-master** klasörünü Visual Studio Code'un yeni bir örneğinde açın. Çözülmemiş bağımlılıkları düzeltmek için geri **yükleme** düğmelerini tıklatın.

Bu nasıl yapIlir kılavuzunda, bir varlık izleyicisi için yeni bir aygıt modeli oluşturursunuz:

1. **Hizmetler\data\devicemodels** klasöründe **assettracker-01.json** adlı yeni bir aygıt modeli dosyası oluşturun.

1. Aygıt modeli **assettracker-01.json** dosyasındaki aygıt işlevini tanımlayın. Protobuf aygıt modelinin telemetri bölümü şunları yapmalı:

   * Cihazınız için oluşturduğunuz Protobuf sınıfının adını ekleyin. Aşağıdaki bölümde bu sınıfın nasıl oluşturacağınızı gösterilmektedir.
   * İleti biçimi olarak Protobuf belirtin.

     ```json
     {
     "SchemaVersion": "1.0.0",
     "Id": "assettracker-01",
     "Version": "0.0.1",
     "Name": "Asset Tracker",
     "Description": "An asset tracker with location, temperature, and humidity",
     "Protocol": "AMQP",
     "Simulation": {
       "InitialState": {
         "online": true,
         "latitude": 47.445301,
         "longitude": -122.296307,
         "temperature": 38.0,
         "humidity": 62.0
       },
       "Interval": "00:01:00",
       "Scripts": [
         {
           "Type": "javascript",
           "Path": "assettracker-01-state.js"
         }
       ]
     },
     "Properties": {
       "Type": "AssetTracker",
       "Location": "Field",
       "Latitude": 47.445301,
       "Longitude": -122.296307
     },
     "Telemetry": [
       {
         "Interval": "00:00:10",
         "MessageTemplate": "{\"latitude\":${latitude},\"longitude\":${longitude},\"temperature\":${temperature},\"humidity\":${humidity}}",
         "MessageSchema": {
           "Name": "assettracker-sensors;v1",
           "ClassName": "Microsoft.Azure.IoTSolutions.DeviceSimulation.Services.Models.Protobuf.AssetTracker",
           "Format": "Protobuf",
           "Fields": {
             "latitude": "double",
             "longitude": "double",
             "temperature": "double",
             "humidity": "double"
           }
         }
       }
     ]
     }
     ```

### <a name="create-device-behaviors-script"></a>Aygıt davranışları komut dosyası oluşturma

Cihazınızın nasıl davrandığını tanımlayan davranış komut dosyasını yazın. Daha fazla bilgi için bkz. [gelişmiş bir benzetimli aygıt oluştur.](iot-accelerators-device-simulation-advanced-device.md)

## <a name="define-your-protobuf-format"></a>Protobuf biçiminizi tanımlayın

Bir aygıt modeline sahip olduğunuzda ve ileti biçiminizi belirlediyseniz, bir **proto** dosyası oluşturabilirsiniz. **Proto** dosyasına şunları eklersiniz:

* Aygıt `csharp_namespace` modelinizdeki **ClassName** özelliğiyle eşleşen bir model.
* Her veri yapısı nın seri hale getirmek için bir ileti.
* İletideki her alan için bir ad ve yazı.

1. **Hizmetler\Modeller\Protobuf\proto** klasöründe **assettracker.proto** adlı yeni bir dosya oluşturun.

1. **Proto** dosyasındaki sözdizimi, ad alanı ve ileti şemasını aşağıdaki gibi tanımlayın:

    ```proto
    syntax = "proto3";

    option csharp_namespace = "Microsoft.Azure.IoTSolutions.DeviceSimulation.Services.Models.Protobuf";

    message AssetTracker {
      double latitude=1;
      double longitude=2;
      double temperature=5;
      double humidity=6;
    }
    ```

Her `=1` `=2` öğedeki işaretçiler, alanın ikili kodlamada kullandığı benzersiz bir etiket belirtir. 1-15 numaraları daha yüksek sayılardaha kodlamak için bir daha az bayt gerektirir.

## <a name="generate-the-protobuf-class"></a>Protobuf sınıfını oluşturun

bir **proto** dosyanız olduğunda, bir sonraki adım iletileri okumak ve yazmak için gereken sınıfları oluşturmaktır. Bu adımı tamamlamak için **Protoc** Protobuf derleyicisine ihtiyacınız var.

1. [Protobuf derleyicisini GitHub'dan indirin](https://github.com/protocolbuffers/protobuf/releases/download/v3.4.0/protoc-3.4.0-win32.zip)

1. Kaynak dizinini, hedef dizini ve **proto** dosyanızın adını belirterek derleyiciyi çalıştırın. Örnek:

    ```cmd
    protoc -I c:\temp\device-simulation-dotnet-master\Services\Models\Protobuf\proto --csharp_out=C:\temp\device-simulation-dotnet-master\Services\Models\Protobuf assettracker.proto
    ```

    Bu **komut, Hizmetler\Modeller\Protobuf** klasöründe **bir Assettracker.cs** dosyası oluşturur.

## <a name="test-protobuf-locally"></a>Protobuf'u yerel olarak test edin

Bu bölümde, önceki bölümlerde oluşturduğunuz varlık izci aygıtını yerel olarak sınarsınız.

### <a name="run-the-device-simulation-microservice"></a>Cihaz simülasyonu microservice çalıştırın

**.vscode/launch.json** dosyasını açın ve şunları atayın:

* **PCS\_\_IOTHUB CONNSTRING** ortam değişkenine IoT Hub bağlantı dizesi.
* **PCS\_AZURE\_DEPOLAMA\_HESAP** ortamı değişkenine depolama hesabı bağlantı dizesi.
* **PCS\_\_STORAGEADAPTER DOCUMENTDB\_CONNSTRING** ortam değişkenine Cosmos DB bağlantı dizesi.

**WebService/Properties/launchSettings.json** dosyasını açın ve şunları atayın:

* **PCS\_\_IOTHUB CONNSTRING** ortam değişkenine IoT Hub bağlantı dizesi.
* **PCS\_AZURE\_DEPOLAMA\_HESAP** ortamı değişkenine depolama hesabı bağlantı dizesi.
* **PCS\_\_STORAGEADAPTER DOCUMENTDB\_CONNSTRING** ortam değişkenine Cosmos DB bağlantı dizesi.

**WebService\appsettings.ini** dosyasını açın ve ayarları aşağıdaki gibi değiştirin:

#### <a name="configure-the-solution-to-include-your-new-device-model-files"></a>Çözümü yeni aygıt modeli dosyalarınızı içerecek şekilde yapılandırın

Varsayılan olarak, yeni aygıt modeli Nizson ve JS dosyalarınız yerleşik çözüme kopyalanmaz. Bunları açıkça eklemeniz gerekir.

Dahil olmak istediğiniz her dosya için **services\services.csproj** dosyasına bir giriş ekleyin. Örnek:

```xml
<None Update="data\devicemodels\assettracker-01.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
<None Update="data\devicemodels\scripts\assettracker-01-state.js">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
```

Microservice'i yerel olarak çalıştırmak için **Hata Ayıklama \> Başlat Hata Ayıklama'yı**tıklatın.

Visual Studio Code'daki **Terminal** penceresi çalışan mikro hizmetten çıktıgösterir.

Sonraki adımları tamamlarken, aygıt simülasyonu microservice Visual Studio Code bu durumda çalışan bırakın.

### <a name="set-up-a-monitor-for-device-events"></a>Aygıt olayları için monitör ayarlama

Bu bölümde, IoT hub'ınıza bağlı aygıtlardan gönderilen telemetriyi görüntülemek için bir olay monitörü ayarlamak için Azure CLI'yi kullanırsınız.

Aşağıdaki komut dosyası, IoT hub'ınızın adının **aygıt simülasyontesti**olduğunu varsayar.

```azurecli-interactive
# Install the IoT extension if it's not already installed
az extension add --name azure-iot

# Monitor telemetry sent to your hub
az iot hub monitor-events --hub-name device-simulation-test
```

Benzetimli aygıtları test ederken olay monitörünü çalışır durumda bırakın.

### <a name="create-a-simulation-with-the-asset-tracker-device-type"></a>Varlık izci cihaz türüile bir simülasyon oluşturma

Bu bölümde, varlık izci aygıt türünü kullanarak bir simülasyon çalıştırmak için aygıt simülasyonu microservice istemek için Postacı aracını kullanın. Postacı, bir web hizmetine REST istekleri göndermenizi sağlayan bir araçtır.

Postacı kurmak için:

1. Yerel makinenizde Postacı'yı açın.

1. ** \> Dosya Alma'yı**tıklatın. Ardından **Dosyaları Seç'i**tıklatın.

1. **Azure IoT Aygıt Simülasyonu çözüm\_hızlandırıcı.postacı koleksiyonu** ve **Azure IoT Aygıt Simülasyonu çözüm hızlandırıcı.postacı\_ortamını** seçin ve **Aç'ı**tıklatın.

1. Gönderebileceğiniz istekleri görüntülemek için **Azure IoT Aygıt Simülasyonu çözüm hızlandırıcısını** genişletin.

1. **Ortam Yok'u** tıklatın ve **Azure IoT Aygıt Simülasyonu çözüm hızlandırıcıyı**seçin.

Artık Postacı çalışma alanınızda cihaz simülasyonu microservice ile etkileşim de kullanabileceğiniz bir koleksiyon ve ortam yüklü.

Simülasyonu yapılandırmak ve çalıştırmak için:

1. Postacı koleksiyonunda varlık **izci simülasyonu oluştur'u** seçin ve **Gönder'i**tıklatın. Bu istek, benzetilen varlık izci aygıt türüdört örneği oluşturur.

1. Azure CLI penceresindeki olay izleme çıkışı, benzetilen aygıtlardan gelen telemetriyi gösterir.

Simülasyonu durdurmak için Postacı'daki **simülasyonu Durdur** isteğini seçin ve **Gönder'e**tıklayın.

### <a name="clean-up-resources"></a>Kaynakları temizleme

İki yerel çalışan mikro hizmeti Visual Studio Code örneklerinde durdurabilirsiniz **(Hata \> Ayıklama Durdurun).**

IoT Hub ve Cosmos DB örneklerini artık istemiyorsanız, gereksiz ücretleri önlemek için bunları Azure aboneliğinizden silin.

## <a name="iot-hub-support"></a>IoT Hub Desteği

Birçok IoT Hub özelliği, Protobuf veya diğer ikili biçimleri doğal olarak desteklemez. Örneğin, IoT Hub ileti yükünü işleyemeyeceğinden, ileti yüküne göre yönlendiremezsiniz. Ancak, ileti üstbilgilerine göre rota yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi telemetri göndermek için Protobuf kullanmak için Cihaz Simülasyonu özelleştirmek için nasıl öğrendim, bir sonraki adım [buluta özel bir görüntü dağıtmak](iot-accelerators-device-simulation-deploy-image.md)için şimdi öğrenmektir.
