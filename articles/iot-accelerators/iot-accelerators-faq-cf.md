---
title: Bağlı Fabrika çözümü SSS - Azure | Microsoft Dokümanlar
description: Bu makalede, Bağlı Fabrika çözüm hızlandırıcıiçin sık sorulan soruları yanıtlar. GitHub deposuna bağlantılar içerir.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: f2f8c08176f80436a339924adb4b2a09338a548a
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81313595"
---
# <a name="frequently-asked-questions-for-connected-factory-solution-accelerator"></a>Connected Factory çözüm hızlandırıcısı için sık sorulan sorular

Ayrıca bakınız, IoT çözüm hızlandırıcıları için genel [SSS.](iot-accelerators-faq.md)

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerator"></a>Çözüm hızlandırıcının kaynak kodunu nerede bulabilirim?

Kaynak kodu aşağıdaki GitHub deposunda depolanır:

* [Bağlı Fabrika çözüm hızlandırıcısı](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-is-opc-ua"></a>OPC UA nedir?

OPC Unified Architecture (UA), 2008 yılında yayımlanan, platformbağımsız, hizmet odaklı birlikte çalışabilirlik standardıdır. OPC UA, endüstri bilgisayarları, PLC'ler ve sensörler gibi çeşitli endüstriyel sistemler ve cihazlar tarafından kullanılır. OPC UA, OPC Classic belirtimlerinin işlevselliğini yerleşik güvenlikle genişletilebilir tek bir çerçeveye entegre eder. OPC Vakfı tarafından yönlendirilen bir standarttır. [OPC Vakfı,](https://opcfoundation.org/) 440'tan fazla üyesi olan kar amacı gütmeyen bir kuruluştur. Organizasyonun amacı, çok satıcılı, çok platformlu, güvenli ve güvenilir birlikte çalışabilirliği kolaylaştırmak için OPC belirtimlerini kullanmaktır:

* Altyapı
* Belirtimler
* Teknoloji
* İşlemler

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-solution-accelerator"></a>Microsoft neden Bağlı Fabrika çözüm hızlandırıcısı için OPC UA'yı seçti?

Microsoft, açık, tescilli olmayan, platformdan bağımsız, endüstritarafından tanınan ve kanıtlanmış bir standart olduğu için OPC UA'yı seçti. Bu, çok çeşitli üretim süreçleri ve ekipmanları arasında birlikte çalışabilirlik sağlayan Endüstri 4.0 (RAMI4.0) referans mimari çözümleri için bir gerekliliktir. Microsoft, Industrie 4.0 çözümleri oluşturmak için müşterilerinden talep görür. OPC UA desteği, müşterilerin hedeflerine ulaşmaları için engelin düşürülmesine yardımcı olur ve onlara anında iş değeri sağlar.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>Simülasyon VM'ye genel bir IP adresi nasıl ekleyebilirim?

IP adresini eklemek için iki seçeneğiniz var:

* [Depodaki](https://github.com/Azure/azure-iot-connected-factory)PowerShell `Simulation/Factory/Add-SimulationPublicIp.ps1` komut dosyasını kullanın. Dağıtım adınızı parametre olarak geçirin. Yerel bir dağıtım `<your username>ConnFactoryLocal`için. Komut dosyası VM IP adresi yazdırır.

* Azure portalında, dağıtımınızın kaynak grubunu bulun. Yerel dağıtım dışında, kaynak grubunda çözüm veya dağıtım adı olarak belirttiğiniz ad vardır. Yapı komut dosyasını kullanan yerel bir dağıtım için `<your username>ConnFactoryLocal`kaynak grubunun adı. Şimdi kaynak grubuna yeni bir **Genel IP adresi** kaynağı ekleyin.

> [!NOTE]
> Her iki durumda da, [Ubuntu web sitesindetalimatları](https://wiki.ubuntu.com/Security/Upgrades)izleyerek en son düzeltme emüleklerini yüklediğinizden emin olun. VM'inize genel bir IP adresi üzerinden erişilebildiği sürece yüklemeyi güncel tutun.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Simülasyon VM'nin genel IP adresini nasıl kaldıracağım?

IP adresini kaldırmak için iki seçeneğiniz var:

* PowerShell komut dosyası Simülasyonu/Fabrika/Kaldır-SimülasyonPublicIp.ps1 [deposunu](https://github.com/Azure/azure-iot-connected-factory)kullanın. Dağıtım adınızı parametre olarak geçirin. Yerel bir dağıtım `<your username>ConnFactoryLocal`için. Komut dosyası VM IP adresi yazdırır.

* Azure portalında, dağıtımınızın kaynak grubunu bulun. Yerel dağıtım dışında, kaynak grubunda çözüm veya dağıtım adı olarak belirttiğiniz ad vardır. Yapı komut dosyasını kullanan yerel bir dağıtım için `<your username>ConnFactoryLocal`kaynak grubunun adı. Şimdi kaynak grubundan **Genel IP adresi** kaynağını kaldırın.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>Simülasyon VM'de nasıl oturum açabilirim?

Simülasyon VM'de oturum açma yalnızca [depodaki](https://github.com/Azure/azure-iot-connected-factory)PowerShell komut dosyasını `build.ps1` kullanarak çözümünüzü dağıttıysanız desteklenir.

Çözümü www.azureiotsolutions.com'dan dağıttıysanız, VM'de oturum açamazsınız. Parola rasgele oluşturulduğundan ve sıfırlayamadığınız için oturum açamazsınız.

1. VM'ye genel bir IP adresi ekleyin. Simülasyon [VM'ye genel bir IP adresi nasıl ekleyebilirim?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. VM'nin IP adresini kullanarak VM'nize bir SSH oturumu oluşturun.
1. Kullanılacak kullanıcı adı: `docker`.
1. Kullanılacak parola, dağıtmak için kullandığınız sürüme bağlıdır:
    * 1 Haziran 2017'den önce build.ps1 komut dosyası kullanılarak `Passw0rd`dağıtılan çözümler için şifre: .
    * 1 Haziran 2017 tarihinden sonra build.ps1 komut dosyası kullanılarak dağıtılan `<name of your deployment>.config.user` çözümler için dosyada parolayı bulabilirsiniz. Parola **VmAdminPassword** ayarında depolanır. `build.ps1` Komut dosyası parametresini kullanarak belirtmediğiniz sürece parola dağıtım zamanında rasgele oluşturulur`-VmAdminPassword`

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Simülasyon VM'deki tüm docker işlemlerini nasıl durdurup başlatabilirim?

1. Simülasyon VM oturum açın. Simülasyon [VM'de nasıl oturum açabilirim?](#how-do-i-sign-in-to-the-simulation-vm)
1. Hangi kapsayıcıların etkin olup `docker ps`olduğunu kontrol etmek için çalıştırın: .
1. Tüm simülasyon kaplarını durdurmak `./stopsimulation`için çalıştırın: .
1. Tüm simülasyon kaplarını başlatmak için:
    * **IOTHUB_CONNECTIONSTRING**adı olan bir kabuk değişkeni dışa aktarma. `<name of your deployment>.config.user` Dosyadaki **IotHubOwnerConnectionString** ayarının değerini kullanın. Örneğin:

        ```sh
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * `./startsimulation` öğesini çalıştırın.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>VM'deki simülasyonu nasıl güncelleyebilirim?

Simülasyonda herhangi bir değişiklik yaptıysanız, [depodaki](https://github.com/Azure/azure-iot-connected-factory) PowerShell komut dosyasını `updatedimulation` `build.ps1` komutu kullanarak kullanabilirsiniz. Bu komut dosyası tüm simülasyon bileşenlerini oluşturur, VM'deki simülasyonu durdurur, yükler, yükler ve başlatır.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>Çözümüm tarafından kullanılan IoT hub'ının bağlantı dizesini nasıl bulabilirim?

Çözümünüzü `build.ps1` [depodaki](https://github.com/Azure/azure-iot-connected-factory)komut dosyasıyla dağıttıysanız, bağlantı dizesi `<name of your deployment>.config.user` dosyadaki **IotHubOwnerConnectionString'in** değeridir.

Azure portalını kullanarak bağlantı dizesini de bulabilirsiniz. Dağıtımınızın kaynak grubundaki IoT Hub kaynağında bağlantı dize ayarlarını bulun.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Bağlı Fabrika simülasyonu hangi IoT Hub cihazlarını kullanır?

Simülasyon kendi kendine aşağıdaki cihazları kaydeder:

* proxy.beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.rio.corp.contoso
* proxy.seattle.corp.contoso
* yayıncı.beijing.corp.contoso
* yayıncı.capetown.corp.contoso
* yayıncı.mumbai.corp.contoso
* yayıncı.munich0.corp.contoso
* yayıncı.rio.corp.contoso
* yayıncı.seattle.corp.contoso

[DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) veya [Azure CLI aracı için IoT uzantısını](https://github.com/Azure/azure-iot-cli-extension) kullanarak, çözümünüzünüzün kullanmakta olduğu IoT hub'ına hangi aygıtların kayıtlı olduğunu kontrol edebilirsiniz. Aygıt gezgini kullanmak için, dağıtımınızda IoT hub'ı için bağlantı dizesine ihtiyacınız vardır. Azure CLI için IoT uzantısını kullanmak için IoT Hub adınıza ihtiyacınız var.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Simülasyon bileşenlerinden günlük verilerini nasıl alabilirim?

Simülasyondaki tüm bileşenler dosyaları günlüğe kaydetmek için bilgileri kaydeder. Bu dosyalar klasördeki `home/docker/Logs`VM'de bulunabilir. Günlükleri almak [için, depodaki](https://github.com/Azure/azure-iot-connected-factory)PowerShell `Simulation/Factory/Get-SimulationLogs.ps1` komut dosyasını kullanabilirsiniz.

Bu komut dosyasının VM'de oturum açması gerekiyor. Oturum açma için kimlik bilgileri sağlamanız gerekebilir. Kimlik bilgilerini bulmak [için simülasyon VM'de nasıl oturum açabilirim?](#how-do-i-sign-in-to-the-simulation-vm)

Komut dosyası, henüz bir IP adresi yoksa VM'ye genel bir IP adresi ekler/kaldırır ve kaldırır. Komut dosyası, tüm günlük dosyalarını bir arşive koyar ve arşivi geliştirme iş istasyonuna indirir.

Alternatif olarak SSH üzerinden VM'de oturum açın ve günlük dosyalarını çalışma zamanında inceleyin.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Simülasyonun buluta veri gönderip göndermedığını nasıl kontrol edebilirim?

[DeviceExplorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) veya [Azure IoT CLI Uzantılı izleme olayları](https://docs.microsoft.com/cli/azure/ext/azure-iot/iot/hub?view=azure-cli-latest#ext-azure-iot-az-iot-hub-monitor-events) komutuyla, IoT Hub'a gönderilen verileri belirli aygıtlardan inceleyebilirsiniz. Bu araçları kullanmak için, dağıtımınızdaki IoT hub'ı bağlantı dizesini bilmeniz gerekir. Bkz. [Çözümüm tarafından kullanılan IoT hub'ının bağlantı dizesini nasıl bulabilirim?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Yayımcı aygıtlarından biri tarafından gönderilen verileri inceleyin:

* yayıncı.beijing.corp.contoso
* yayıncı.capetown.corp.contoso
* yayıncı.mumbai.corp.contoso
* yayıncı.munich0.corp.contoso
* yayıncı.rio.corp.contoso
* yayıncı.seattle.corp.contoso

IoT Hub'a gönderilen veri görmüyorsanız, simülasyonla ilgili bir sorun var demektir. İlk analiz adımı olarak simülasyon bileşenlerinin günlük dosyalarını analiz etmelisiniz. Bkz. [Simülasyon bileşenlerinden günlük verilerini nasıl alabilirim?](#how-can-i-get-log-data-from-the-simulation-components) Ardından, simülasyonu durdurmaya ve başlatmaya çalışın ve hala gönderilen veri yoksa simülasyonu tamamen güncelleyin. [VM'deki simülasyonu nasıl güncelleyebilirim?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="how-do-i-enable-an-interactive-map-in-my-connected-factory-solution"></a>Bağlı Fabrika çözümümde etkileşimli bir haritayı nasıl etkinleştirebilirim?

Bağlı Fabrika çözümünde etkileşimli bir haritayı etkinleştirmek için bir Azure Haritalar hesabınız olması gerekir.

[www.azureiotsolutions.com](https://www.azureiotsolutions.com)dağıtılırken dağıtım işlemi, çözüm hızlandırıcı hizmetlerini içeren kaynak grubuna bir Azure Haritalar hesabı ekler.

Bağlı Fabrika GitHub deposundaki komut dosyasını `build.ps1` kullanarak dağıtırken, `$env:MapApiQueryKey` yapı penceresindeki ortam değişkenini [Azure Haritalar hesabınızın anahtarına](../azure-maps/how-to-manage-account-keys.md)ayarlayın. Etkileşimli harita daha sonra otomatik olarak etkinleştirilir.

Dağıtımdan sonra çözüm hızlandırıcınıza bir Azure Haritalar hesap anahtarı da ekleyebilirsiniz. Azure portalına gidin ve Bağlı Fabrika dağıtımınızda Uygulama Hizmeti kaynağına erişin. Bir bölüm Uygulama **ayarları**bulduğunuz **Uygulama ayarlarına**gidin. **MapApiQueryKey'i** [Azure Haritalar hesabınızın anahtarına](../azure-maps/how-to-manage-account-keys.md)ayarlayın. Ayarları kaydedin ve genel **bakışa** gidin ve Uygulama Hizmetini yeniden başlatın.

### <a name="how-do-i-create-an-azure-maps-account"></a>Azure Haritalar hesabını nasıl oluştururum?

Azure [Haritalar hesabınızı ve anahtarlarınızı nasıl yönetebilirsiniz?](../azure-maps/how-to-manage-account-keys.md)

### <a name="how-to-obtain-your-azure-maps-account-key"></a>Azure Haritalar hesap anahtarınızı nasıl edinir?

Azure [Haritalar hesabınızı ve anahtarlarınızı nasıl yönetebilirsiniz?](../azure-maps/how-to-manage-account-keys.md)

### <a name="how-do-enable-the-interactive-map-while-debugging-locally"></a>Yerel olarak hata ayıklama sırasında etkileşimli haritayı nasıl etkinleştirin?

Yerel olarak hata ayıklama yaparken etkileşimli haritayı etkinleştirmek için, `MapApiQueryKey` dosyalardaki `local.user.config` ve `<yourdeploymentname>.user.config` dağıtımınızın kökündeki ayarın değerini daha önce kopyaladığınız **QueryKey** değerine ayarlayın.

### <a name="how-do-i-use-a-different-image-at-the-home-page-of-my-dashboard"></a>Panomun ana sayfasında farklı bir resmi nasıl kullanırım?

panoana sayfa io gösterilen statik görüntüyü değiştirmek için, `WebApp\Content\img\world.jpg`görüntüyü değiştirin. Ardından WebApp'ı yeniden oluşturup yeniden dağıtın.

### <a name="how-do-i-use-non-opc-ua-devices-with-connected-factory"></a>Bağlı Fabrika ile OPC Olmayan UA aygıtlarını nasıl kullanırım?

OPC UA olmayan cihazlardan telemetri verilerini Bağlı Fabrika'ya göndermek için:

1. [Dosyadaki Bağlı Fabrika topolojisinde yeni bir istasyon](iot-accelerators-connected-factory-configure.md) yapılandırın. `ContosoTopologyDescription.json`

1. Bağlı Fabrika uyumlu JSON formatında telemetri verilerini yutma:

    ```json
    [
      {
        "ApplicationUri": "<the_value_of_OpcUri_of_your_station",
        "DisplayName": "<name_of_the_datapoint>",
        "NodeId": "value_of_NodeId_of_your_datapoint_in_the_station",
        "Value": {
          "Value": <datapoint_value>,
          "SourceTimestamp": "<timestamp>"
        }
      }
    ]
    ```

1. `<timestamp>` Biçimi:`2017-12-08T19:24:51.886753Z`

1. Bağlı Fabrika Uygulama Hizmetini yeniden başlatın.

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Tahmine Dayalı Bakım çözüm hızlandırıcıya genel bakış](iot-accelerators-predictive-overview.md)
* [Bağlı Fabrika çözüm hızlandırıcıyı dağıtın](quickstart-connected-factory-deploy.md)
* [Baştan sona IoT güvenliği](/azure/iot-fundamentals/iot-security-ground-up)
