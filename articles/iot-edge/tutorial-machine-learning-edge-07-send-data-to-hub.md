---
title: "Öğretici: Saydam ağ geçidi üzerinden aygıt verileri gönderme - Azure IoT Edge'de Makine Öğrenimi"
description: Bu öğretici, geliştirme makinenizi, saydam ağ geçidi olarak yapılandırılan bir aygıttan geçerek IoT Hub'ına veri göndermek için benzetimli Bir IoT Edge aygıtı olarak nasıl kullanabileceğinizi gösterir.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/12/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 50f339b257110f0a5dc0ac08b9f40043ee384afb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74706902"
---
# <a name="tutorial-send-data-via-transparent-gateway"></a>Öğretici: Saydam ağ geçidi üzerinden veri gönderme

> [!NOTE]
> Bu makale, IoT Edge'de Azure Machine Learning'i kullanma yla ilgili bir öğretici için bir serinin parçasıdır. Bu makaleye doğrudan ulaştıysanız, en iyi sonuçlar için serinin [ilk makalesiile](tutorial-machine-learning-edge-01-intro.md) başlamanızı öneririz.

Bu makalede, geliştirme makinesini bir kez daha simüle edilmiş bir aygıt olarak kullanıyoruz, ancak verileri doğrudan IoT Hub'ına göndermek yerine aygıt saydam ağ geçidi olarak yapılandırılan IoT Edge aygıtına veri gönderir.

Simüle edilen cihaz veri gönderirken IoT Edge cihazının çalışmasını izliyoruz. Cihaz çalışma tamamlandıktan sonra, beklendiği gibi çalışılan her şeyi doğrulamak için depolama hesabımızdaki verilere bakıyoruz.

Bu adım genellikle bir bulut veya aygıt geliştiricisi tarafından gerçekleştirilir.

## <a name="review-device-harness"></a>Cihaz kayışı gözden geçirme

Aşağı (veya yaprak) aygıtını simüle etmek için [DeviceHarness projesini](tutorial-machine-learning-edge-03-generate-data.md) yeniden kullanın. Saydam ağ geçidine bağlanmak için iki ek şey daha gerekiyor:

* Alt aygıt (bu durumda geliştirme makinemiz) IoT Edge çalışma zamanı tarafından kullanılan sertifika yetkilisine güvenmek için sertifikayı kaydedin.
* Aygıt bağlantı dizesine kenar ağ geçidini tam nitelikli alan adı (FQDN) ekleyin.

Bu iki öğenin nasıl uygulandığını görmek için koda bakın.

1. Geliştirme makinenizde Visual Studio Code'u açın.

2. \\C:\\source IoTEdgeAndMlSample\\DeviceHarness'i açmak için **Dosya** > Aç**Klasörünü...** kullanın.

3. Program.cs'daki InstallCertificate() yöntemine bakın.

4. Kod sertifika yolunu bulursa, sertifikayı makineye yüklemek için CertificateManager.InstallCACert yöntemini çağırır.

5. Şimdi TurbofanDevice sınıfındaki GetIotHubDevice yöntemine bakın.

6. Kullanıcı "-g" seçeneğini kullanarak ağ geçidinin FQDN'sini belirttiğinde, bu değer bu yönteme aygıt bağlantı dizesine eklenen ağ geçidiFqdn olarak aktarılır.

   ```csharp
   connectionString = $"{connectionString};GatewayHostName={gatewayFqdn.ToLower()}";
   ```

## <a name="build-and-run-leaf-device"></a>Yaprak cihaz oluşturma ve çalıştırma

1. Visual Studio Code'da hala açık olan DeviceHarness projesiyle, projeyi oluşturun (Ctrl + Shift + B veya **Terminal** > **Run Build Task...**) ve iletişim kutusundan **Build'i** seçin.

2. Portaldaki IoT Edge cihazınızın sanal makinesine gidip **DNS adının** değerini genel bakışta kopyalayarak kenar ağ geçidiniz için tam nitelikli alan adını (FQDN) bulun.

3. Visual Studio Code terminalini **(Terminal** > **Yeni terminali)** `<edge_device_fqdn>` açın ve sanal makineden kopyaladığınız DNS adını değiştirerek aşağıdaki komutu çalıştırın:

   ```cmd
   dotnet run -- --gateway-host-name "<edge_device_fqdn>" --certificate C:\edgecertificates\certs\azure-iot-test-only.root.ca.cert.pem --max-devices 1
   ```

4. Uygulama, sertifikayı geliştirme makinenize yüklemeyi dener. Olduğunda, güvenlik uyarısını kabul edin.

5. IoT Hub bağlantı dizesi için istendiğinde Azure IoT Hub aygıtları panelindeki elipsis **(...**) düğmesini tıklatın ve **Copy IoT Hub Bağlantı Dizesini**seçin. Değeri terminale yapıştırın.

6. Gibi çıktı göreceksiniz:

   ```output
   Found existing device: Client_001
   Using device connection string: HostName=<your hub>.azure-devices.net;DeviceId=Client_001;SharedAccessKey=xxxxxxx; GatewayHostName=iotedge-xxxxxx.<region>.cloudapp.azure.com
   Device: 1 Message count: 50
   Device: 1 Message count: 100
   Device: 1 Message count: 150
   Device: 1 Message count: 200
   Device: 1 Message count: 250
   ```

   Aygıtın IoT Edge saydam ağ geçidi aracılığıyla IoT Hub üzerinden iletişim kurmasına neden olan aygıt bağlantı dizesine "GatewayHostName" eklenmesine dikkat edin.

## <a name="check-output"></a>Çıktıyı kontrol edin

### <a name="iot-edge-device-output"></a>IoT Edge cihaz çıkışı

AvroFileWriter modülünden çıkan çıktı IoT Edge cihazına bakılarak kolayca gözlemlenebilir.

1. IoT Edge sanal makinenize SSH.

2. Diske yazılmış dosyaları arayın.

   ```bash
   find /data/avrofiles -type f
   ```

3. Komutun çıktısı aşağıdaki örnek gibi görünecektir:

   ```output
   /data/avrofiles/2019/4/18/22/10.avro
   ```

   Çalıştırmanın zamanlamasına bağlı olarak birden fazla dosyanız olabilir.

4. Zaman damgalarına dikkat edin. AvroFileWriter modülü, son modifikasyon süresi 10 dakikadan fazla olduğunda dosyaları buluta\_yükler\_(avroFileWriter modülünde uploader.py'da MODIFIYE DOSYA ZAMANINA bakınız).

5. 10 dakika geçtikten sonra, modül dosyaları yüklemelidir. Yükleme başarılı olursa, dosyaları diskten siler.

### <a name="azure-storage"></a>Azure depolama alanı

Veri yönlendirmesini beklediğimiz depolama hesaplarına bakarak yaprak cihazlarımızın veri gönderme sonuçlarını gözlemleyebiliriz.

1. Geliştirme makinesinde Visual Studio Code açık.

2. Araştırma penceresindeki "AZURE DEPOLAMA" panelinde, depolama hesabınızı bulmak için ağaca gidin.

3. **Blob Containers** düğümlerini genişletin.

4. Öğreticinin önceki bölümünde yaptığımız çalışmalardan, **ruldata** kapsayıcısının RUL içeren iletiler içermesini bekliyoruz. **Ruldata** düğümlerini genişletin.

5. Bir veya daha fazla blob dosyaları `<IoT Hub Name>/<partition>/<year>/<month>/<day>/<hour>/<minute>`gibi adlı göreceksiniz: .

6. Dosyalardan birine sağ tıklayın ve dosyayı geliştirme makinenize kaydetmek için **Blob'u İndir'i** seçin.

7. Daha sonra **uploadturbofanfiles** düğüm genişletin. Bir önceki makalede, avroFileWriter modülü tarafından yüklenen dosyalar için hedef olarak bu konumu ayarladık.

8. Dosyalara sağ tıklayın ve geliştirme makinenize kaydetmek için **Blob İndir'i** seçin.

### <a name="read-avro-file-contents"></a>Avro dosya içeriğini okuyun

Bir Avro dosyasını okumak ve dosyadaki iletilerin JSON dizesini döndürmek için basit bir komut satırı yardımcı programı dahil ettik. Bu bölümde, biz yükleyecek ve çalıştırın.

1. Visual Studio Code **(Terminal** > **Yeni terminal)** terminali açın.

2. Hubavroreader'ı yükleyin:

   ```cmd
   pip install c:\source\IoTEdgeAndMlSample\HubAvroReader
   ```

3. **ruldata'dan**indirdiğiniz Avro dosyasını okumak için hubavroreader'ı kullanın.

   ```cmd
   hubavroreader <avro file with ath> | more
   ```

4. İletinin gövdesinin cihaz kimliği ve tahmin edilen RUL ile beklediğimiz gibi göründüğünü unutmayın.

   ```json
   {
       "Body": {
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "CycleTime": 1.0,
           "PredictedRul": 170.1723693909444
       },
       "EnqueuedTimeUtc": "<time>",
       "Properties": {
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "CreationTimeUtc": "01/01/0001 00:00:00",
           "EnqueuedTimeUtc": "01/01/0001 00:00:00"
       },
       "SystemProperties": {
           "connectionAuthMethod": "{\"scope\":\"module\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
           "connectionDeviceGenerationId": "636857841798304970",
           "connectionDeviceId": "aaTurbofanEdgeDevice",
           "connectionModuleId": "turbofanRouter",
           "contentEncoding": "utf-8",
           "contentType": "application/json",
           "correlationId": "3d0bc256-b996-455c-8930-99d89d351987",
           "enqueuedTime": "<time>",
           "iotHubName": "mledgeiotwalkthroughhub"
       }
   }
   ```

5. **Uploadturbofanfiles'den**indirdiğiniz Avro dosyasını geçerek aynı komutu çalıştırın.

6. Beklendiği gibi, bu iletiler orijinal iletideki tüm sensör verilerini ve çalışma ayarlarını içerir. Bu veriler, kenar aygıtımızdaki RUL modelini geliştirmek için kullanılabilir.

   ```json
   {
       "Body": {
           "CycleTime": 1.0,
           "OperationalSetting1": -0.0005000000237487257,
           "OperationalSetting2": 0.00039999998989515007,
           "OperationalSetting3": 100.0,
           "PredictedRul": 170.17236328125,
           "Sensor1": 518.6699829101562,
           "Sensor10": 1.2999999523162842,
           "Sensor11": 47.29999923706055,
           "Sensor12": 522.3099975585938,
           "Sensor13": 2388.010009765625,
           "Sensor14": 8145.31982421875,
           "Sensor15": 8.424599647521973,
           "Sensor16": 0.029999999329447746,
           "Sensor17": 391.0,
           "Sensor18": 2388.0,
           "Sensor19": 100.0,
           "Sensor2": 642.3599853515625,
           "Sensor20": 39.11000061035156,
           "Sensor21": 23.353700637817383,
           "Sensor3": 1583.22998046875,
           "Sensor4": 1396.8399658203125,
           "Sensor5": 14.619999885559082,
           "Sensor6": 21.610000610351562,
           "Sensor7": 553.969970703125,
           "Sensor8": 2387.9599609375,
           "Sensor9": 9062.169921875
       },
           "ConnectionDeviceId": "Client_001",
           "CorrelationId": "70df0c98-0958-4c8f-a422-77c2a599594f",
           "CreationTimeUtc": "0001-01-01T00:00:00+00:00",
           "EnqueuedTimeUtc": "<time>"
   }
   ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uçuça öğreticitarafından kullanılan kaynakları keşfetmeyi planlıyorsanız, oluşturduğunuz kaynakları temizlemek için yapılan işi bitirene kadar bekleyin. Devam etmeyi planlamıyorsanız, bunları silmek için aşağıdaki adımları kullanın:

1. Dev VM, IoT Edge VM, IoT Hub, depolama hesabı, makine öğrenimi çalışma alanı hizmeti (ve oluşturulan kaynaklar: konteyner kayıt defteri, uygulama bilgileri, anahtar kasası, depolama hesabı) tutmak için oluşturulan kaynak grubu(lar) silin.

2. [Azure dizüstü bilgisayarlardaki](https://notebooks.azure.com)makine öğrenimi projesini silin.

3. Repo'yu yerel olarak klonladıysanız, yerel repo'ya atıfta bulunan PowerShell veya VS Code pencerelerini kapatın, ardından repo dizini silin.

4. Yerel olarak sertifikalar oluşturduysanız, c:\\edgeCertificates klasörünü silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kenar cihazımıza sensör ve operasyonel veri gönderen bir yaprak cihazını simüle etmek için geliştirme makinemizi kullandık. Cihazdaki modüllerin yönlendirildiğini, sınıflandırdığını, devam ettiğini ve önce kenar cihazının gerçek zamanlı çalışmasını inceleyerek ve daha sonra depolama hesabına yüklenen dosyalara bakarak verileri yüklediğini doğruladık.

Daha fazla bilgiyi aşağıdaki sayfalarda bulabilirsiniz:

* [Aşağı akış cihazını Azure IoT Edge ağ geçidine bağlama](how-to-connect-downstream-device.md)
* [IoT Edge'de Azure Blob Depolama ile verileri kenarda depolama (önizleme)](how-to-store-data-blob.md)
