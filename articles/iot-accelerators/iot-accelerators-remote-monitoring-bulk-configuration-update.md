---
title: Uzaktan İzleme'ye bağlı cihazları toplu olarak yönetme - Azure | Microsoft Dokümanlar
description: Bu eğitimde, uzaktan izleme çözümüne bağlı aygıtları toplu olarak nasıl yönetilenöğrenebilirsiniz.
author: Philmea
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: philmea
ms.openlocfilehash: eaca93ac8a4e8c660be9618aefb27921a4e0a2eb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77565587"
---
# <a name="tutorial-manage-your-connected-devices-in-bulk"></a>Öğretici: Bağlı cihazlarınızı toplu olarak yönetme

Bu eğitimde, bağlı aygıtlarınızın yapılandırmasını toplu olarak yönetmek için Uzaktan İzleme çözüm hızlandırıcısını kullanırsınız.

Contoso'da bir operatör olarak, yeni bir firmware sürümü ile aygıtlar bir grup yapılandırmak gerekir. Her cihazdaki firmware'i tek tek güncellemek zorunda kalmak istemezsinuz. Firmware'i bir aygıt grubunda güncellemek için Uzaktan İzleme çözüm hızlandırıcısında aygıt gruplarını ve otomatik aygıt yönetimini kullanabilirsiniz. Aygıt grubuna eklediğiniz herhangi bir cihaz, aygıt çevrimiçi olur olmaz en son yazılımları alır.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Cihaz grubu oluşturma
> * Firmware'i hazırlayın ve barındırın
> * Azure portalında aygıt yapılandırması oluşturma
> * Uzaktan İzleme çözümünüze aygıt yapılandırması alma
> * Yapılandırmayı aygıt grubundaki aygıtlara dağıtma
> * Dağıtımı izleme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

<!--
If this is going to be a tutorial - we need to split this include into two so that we can accommodate the additional prerequisites:

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]
-->

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi takip etmek için Azure aboneliğinizde Uzaktan İzleme çözümü hızlandırıcısının dağıtılmış örneğine sahip olmanız gerekir.

Uzaktan İzleme çözümü hızlandırıcısını henüz dağıtmadıysanız [Bulut tabanlı uzaktan izleme çözümü dağıtma](quickstart-remote-monitoring-deploy.md) hızlı başlangıcını tamamlamanız gerekir.

Firmware dosyalarınızı barındırmak için bir Azure depolama hesabına ihtiyacınız vardır. Varolan bir depolama hesabı kullanabilir veya aboneliğinizde [yeni bir depolama hesabı oluşturabilirsiniz.](../storage/common/storage-account-create.md)

Öğretici, örnek bir aygıt olarak bir [IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) aygıtı kullanır.

Yerel makinenizde aşağıdaki yazılımların yüklü olarak yüklenmesi gerekir:

* [Görsel Stüdyo Kodu (VS Kodu)](https://code.visualstudio.com/).
* [Azure IoT Çalışma Tezgahı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) VS Kodu uzantısı.

Başlamadan önce:

* [IoT DevKit cihazınızdaki bootloader'ın sürüm 1.4.0 veya daha yüksek olduğundan](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/)emin olun.
* IoT DevKit SDK'nın bootloader ile aynı sürümde olduğundan emin olun. IoT DevKit SDK'yı VS Kodu'ndaki Azure IoT Çalışma Tezgahı'nı kullanarak güncelleyebilirsiniz. Komut paletini açın ve **Arduino girin: Board Manager**. Daha fazla bilgi için [bkz.](../iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started.md#prepare-the-development-environment)

Ayrıca, uzaktan izleme çözüm hızlandırıcınıza en az bir IoT DevKit cihazı bağlamanız gerekir. Bir IoT DevKit cihazını bağlamadıysanız, [MXChip IoT DevKit AZ3166'yı IoT Uzaktan İzleme çözüm hızlandırıcısına bağlayın.](iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2.md)

## <a name="navigate-to-the-dashboard"></a>Panoya gidin

Uzaktan İzleme çözümü panosunu tarayıcınızda görüntülemek için önce [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasına gidin. Azure aboneliği kimlik bilgilerinizi kullanarak oturum açmanız istenebilir.

Ardından [Hızlı başlangıç](quickstart-remote-monitoring-deploy.md) ile dağıttığınız Uzaktan İzleme çözümü hızlandırıcısına ait kutucukta bulunan **Başlat** öğesine tıklayın.

## <a name="create-a-device-group"></a>Cihaz grubu oluşturma

Firmware'i bir aygıt grubunda otomatik olarak güncellemek için, aygıtların Uzaktan İzleme çözümünüzdeki bir aygıt grubunun üyeleri olması gerekir:

1. **Cihazlar** sayfasında, çözüm hızlandırıcısına bağlandığınız tüm **IoT DevKit** aygıtlarını seçin. Ardından **Jobs** (İşler) öğesine tıklayın.

1. **İşler** panelinde, **Etiketler'i**seçin, iş adını **AddDevKitTag'a**ayarlayın ve ardından **Y**değerine sahip **IsDevKitDevice** adlı bir metin etiketi ekleyin. Ardından **Uygula'yı**tıklatın.

1. Artık bir aygıt grubu oluşturmak için etiket değerlerini kullanabilirsiniz. **Cihazlar** sayfasında, **aygıt gruplarını yönet'i**tıklatın.

1. **IsDevKitDevice** etiket adını kullanan bir metin filtresi oluşturun ve durum daki **Y** değerini belirtin. Aygıt grubunu **IoT DevKit aygıtları olarak kaydedin.**

Bu eğitimin ilerleyen saatlerinde, tüm üyelerin yazılımını güncelleyen bir aygıt yapılandırması uygulamak için bu aygıt grubunu kullanırsınız.

## <a name="prepare-and-host-the-firmware"></a>Firmware'i hazırlayın ve barındırın

[Azure IoT Workbench](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) VS Code uzantısı, firmware güncelleştirmesi için örnek aygıt kodunu içerir.

### <a name="open-the-firmware-ota-sample-in-vs-code"></a>Firmware OTA örneğini VS Kodu'nda açın

1. IoT DevKit'inizin bilgisayarınıza bağlı olmadığından emin olun. VS Kodu'nu başlatın ve DevKit'i bilgisayarınıza bağlayın.

1. Komut paletini açmak için **F1** tuşuna basın, **IoT Workbench**yazın ve seçin: Örnekler . Ardından yönetim kurulu olarak **IoT DevKit'i** seçin.

1. **Firmware OTA'yı** bulun ve **Örnek Aç'ı**tıklatın. Yeni bir VS Kodu penceresi açılır ve **firmware_ota** proje klasörünü gösterir:

    ![IoT Workbench, Firmware OTA örneğini seçin](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-firmware-example.png)

### <a name="build-the-new-firmware"></a>Yeni firmware oluşturun

Aygıt firmware ilk sürümü 1.0.0 olduğunu. Yeni firmware daha yüksek bir sürüm numarası olmalıdır.

1. VS Code'da **FirmwareOTA.ino** dosyasını `currentFirmwareVersion` açın `1.0.0` `1.0.1`ve aşağıdakilerden değiştirin:

    ![Firmware sürümünü değiştirme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Komut paletini açın, ardından **IoT Workbench**yazın ve seçin: Aygıt. Ardından kodu derlemek için Aygıt Derlemesi'ni seçin: **Device Compile**

    ![Cihaz derleme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-compile.png)

    VS Code, derlenen dosyayı `.build` projedeki klasöre kaydeder. Ayarlarınıza bağlı olarak, VS Kodu `.build` klasörü explorer görünümünde gizleyebilir.

### <a name="generate-the-crc-value-and-calculate-the-firmware-file-size"></a>CRC değerini oluşturun ve firmware dosya boyutunu hesaplayın

1. Komut paletini açın, ardından **IoT Workbench**yazın ve seçin: Aygıt. Ardından **CRC Oluştur'u**seçin:

    ![CRC oluştur](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-crc.png)

1. VS Kodu CRC değerini, firmware dosya adını ve yolunu ve çıktı penceresindeki dosya boyutunu oluşturur ve yazdırır. Daha sonra için bu değerlere dikkat edin:

    ![CRC bilgileri](media/iot-accelerators-remote-monitoring-bulk-configuration-update/crc-info.png)

### <a name="upload-the-firmware-to-the-cloud"></a>Firmware'i buluta yükleyin

Yeni firmware dosyanızı bulutta barındırmak için Azure depolama hesabınızı kullanın.

1. Azure portalda depolama hesabınıza gidin. Hizmetler bölümünde **Blobs'ı**seçin. Firmware dosyalarınızı depolamak için **firmware** adı verilen genel bir kapsayıcı oluşturun:

    ![Klasör Oluştur](media/iot-accelerators-remote-monitoring-bulk-configuration-update/blob-folder.png)

1. Firmware dosyanızı kapsayıcıya yüklemek için **firmware** kapsayıcısını seçin ve **Yükle'yi**tıklatın.

1. **FirmwareOTA.ino.bin'i**seçin. Önceki bölümde bu dosyaya giden yolun tamamını not aldınız.

1. Firmware dosya yüklemetamamlandıktan sonra, dosya URL'sini not edin.

### <a name="build-and-upload-the-original-firmware-to-the-iot-devkit-device"></a>Orijinal firmware'i ioT DevKit aygıtına oluşturun ve yükleyin

1. VS Code'da **FirmwareOTA.ino** dosyasını `currentFirmwareVersion` açın `1.0.0`ve geri yi değiştirin:

    ![Sürüm 1.0.0](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Komut paletini açın, ardından **IoT Workbench**yazın ve seçin: Aygıt. Sonra **Aygıt Yükleme**seçin:

    ![Cihaz Yükleme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/device-upload.png)

1. VS Code kodu doğrular ve IoT DevKit cihazınıza yükler.

1. Yükleme bittiğinde, IoT DevKit aygıtı yeniden başlatılır. Yeniden başlatma tamamlandığında, IoT DevKit'in ekranı **FW sürümünü gösterir: 1.0.0**, ve yeni firmware için kontrol ediyor:

    ![ota-1](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-1.jpg)

## <a name="create-a-device-configuration"></a>Aygıt yapılandırması oluşturma

Aygıt yapılandırması, aygıtlarınızın istenilen durumunu belirtir. Genellikle, bir geliştirici yapılandırmayı Azure portalındaki **IoT aygıt yapılandırma** sayfasında [oluşturur.](../iot-hub/iot-hub-automatic-device-management.md#create-a-configuration) Aygıt yapılandırması, aygıtlarınızın istenilen durumunu ve bir dizi ölçüm kümesini belirten bir JSON belgesidir.

Aşağıdaki yapılandırmayı yerel makinenize **firmware-update.json** adı verilen dosya olarak kaydedin. `YOURSTRORAGEACCOUNTNAME`,, `YOURCHECKSUM`ve `YOURPACKAGESIZE` yer tutucuları daha önce not aldığınız değerlerle değiştirin:

```json
{
  "id": "firmware-update",
  "schemaVersion": null,
  "labels": {
    "Version": "1.0.1",
    "Devices": "IoT DevKit"
  },
  "content": {
    "deviceContent": {
      "properties.desired.firmware": {
        "fwVersion": "1.0.1",
        "fwPackageURI": "https://YOURSTRORAGEACCOUNTNAME.blob.core.windows.net/firmware/FirmwareOTA.ino.bin",
        "fwPackageCheckValue": "YOURCHECKSUM",
        "fwSize": YOURPACKAGESIZE
      }
    }
  },
  "targetCondition": "",
  "priority": 10,
  "systemMetrics": {
    "results": {
      "targetedCount": 0,
      "appliedCount": 0
    },
    "queries": {
      "targetedCount": "",
      "appliedCount": "select deviceId from devices where configurations.[[firmware-update]].status = 'Applied'"
    }
  },
  "metrics": {
    "results": {
      "Current": 1
    },
    "queries": {
      "Current": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Current' AND properties.reported.firmware.type='IoTDevKit'",
      "Downloading": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Downloading' AND properties.reported.firmware.type='IoTDevKit'",
      "Verifying": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Verifying' AND properties.reported.firmware.type='IoTDevKit'",
      "Applying": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Applying' AND properties.reported.firmware.type='IoTDevKit'",
      "Error": "SELECT deviceId FROM devices WHERE properties.reported.firmware.fwUpdateStatus='Error' AND properties.reported.firmware.type='IoTDevKit'"
    }
  }
}
```

Bu yapılandırma dosyasını aşağıdaki bölümde kullanırsınız.

## <a name="import-a-configuration"></a>Yapılandırma alma

Bu bölümde, aygıt yapılandırmasını paket olarak Uzaktan İzleme çözüm hızlandırıcısına aktarın. Genellikle, bir işleç bu görevi tamamlar.

1. Uzaktan İzleme web Kullanıcı Arama Sürümü'nde **Paketler** sayfasına gidin ve **+ Yeni Paket'e**tıklayın:

    ![Yeni paket](media/iot-accelerators-remote-monitoring-bulk-configuration-update/packagepage.png)

1. Yeni **Paket** panelinde, paket türü olarak **Aygıt yapılandırmasını** ve yapılandırma türü olarak **Firmware'i** seçin. Yerel makinenizde **firmware-update.json** dosyasını bulmak için **Gözat'ı** tıklatın ve sonra **Yükle'yi**tıklatın:

    ![Paketi yükleme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/uploadpackage.png)

1. Paketlerin listesi artık **firmware güncelleme** paketini içerir.

## <a name="deploy-the-configuration-to-your-devices"></a>Yapılandırmayı aygıtlarınıza dağıtma

Bu bölümde, aygıt yapılandırmasını IoT DevKit aygıtlarınıza uygulayan bir dağıtım oluşturur ve yürütür.

1. Uzaktan İzleme web Kullanıcı Arama Sürümü'nde **Dağıtımlar** sayfasına gidin ve **+ Yeni dağıtım'ı**tıklatın:

    ![Yeni dağıtım](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentpage.png)

1. Yeni **dağıtım** panelinde, aşağıdaki ayarlara sahip bir dağıtım oluşturun:

    |Seçenek|Değer|
    |---|---|
    |Adı|Firmware güncelleştirmesi dağıtma|
    |Paket türü|Cihaz Yapılandırması|
    |Yapılandırma türü|Üretici yazılımı|
    |Paket|firmware-update.json|
    |Cihaz Grubu|IoT DevKit cihazları|
    |Öncelik|10|

    ![Dağıtım oluşturma](media/iot-accelerators-remote-monitoring-bulk-configuration-update/newdeployment.png)

    **Uygula**’ya tıklayın. **Dağıtımlar** sayfasında aşağıdaki ölçümleri gösteren yeni bir dağıtım görürsünüz:

    * **Hedeflenen** aygıt grubundaki aygıt sayısını gösterir.
    * **Uygulanan** yapılandırma içeriği ile güncelleştirilen aygıt sayısını gösterir.
    * **Başarılı,** dağıtımda başarıyı bildiren aygıt sayısını gösterir.
    * **Başarısız,** dağıtımda hata bildiren aygıt sayısını gösterir.

## <a name="monitor-the-deployment"></a>Dağıtımı izleme

Birkaç dakika sonra, IoT DevKit yeni firmware bilgilerini alır ve cihaza indirmeye başlar:

![ota-2](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-2.jpg)

Ağınızın hızına bağlı olarak, indirme işlemi birkaç dakika kadar sürebilir. Firmware indirildikten sonra, aygıt dosya boyutunu ve CRC değerini doğrular. Doğrulama başarılı olursa MXChip ekranları **geçti.**

![ota-3](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-3.jpg)

Denetim başarılı olursa, aygıt yeniden başlatılır. Yeniden başlatma gerçekleşmeden önce **5'ten** **0'a** kadar geri sayım görürsünüz.

![ota-4](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-4.jpg)

Yeniden başlatmadan sonra, IoT DevKit bootloader firmware'i yeni sürüme yükseltir. Yükseltme birkaç saniye sürebilir. Bu aşamada cihazdaki RGB LED'i kırmızı ve ekran boştur.

![ota-5](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-5.jpg)

Yeniden başlatma tamamlandığında, IoT DevKit aygıtınız artık firmware'in 1.0.1 sürümünü çalıştırıyor.

![ota-6](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-6.jpg)

**Dağıtımlar** sayfasında, aygıtlarınızın güncellenirken durumunu görmek için dağıtıma tıklayın. Aygıt grubunuzun her aygıtının durumunu ve tanımladığınız özel ölçümleri görebilirsiniz.

![Dağıtım ayrıntıları](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentstatus.png)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, çözümünüze bağlı bir grup aygıtın firmware'ini nasıl güncelleştirdiğinizi gösterdi. Aygıtları güncellemek için çözümünüz otomatik aygıt yönetimini kullanır. Çözümünüzün temel IoT hub'ındaki otomatik aygıt yönetimi özelliği hakkında daha fazla bilgi edinmek için [Azure portalını kullanarak IoT aygıtlarını ölçekte yapılandır'a](../iot-hub/iot-hub-auto-device-config.md)bakın ve izleyin.