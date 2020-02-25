---
title: Toplu-Azure 'da uzaktan Izlemeye bağlı cihazları yönetme | Microsoft Docs
description: Bu öğreticide, uzaktan Izleme çözümüne bağlı cihazların toplu olarak nasıl yönetileceğini öğreneceksiniz.
author: Philmea
manager: philmea
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: tutorial
ms.date: 11/29/2018
ms.author: philmea
ms.openlocfilehash: eaca93ac8a4e8c660be9618aefb27921a4e0a2eb
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565587"
---
# <a name="tutorial-manage-your-connected-devices-in-bulk"></a>Öğretici: bağlı cihazlarınızı toplu olarak yönetme

Bu öğreticide, bağlı cihazlarınızın yapılandırmasını toplu olarak yönetmek için uzaktan Izleme çözüm Hızlandırıcısını kullanırsınız.

Contoso 'da bir operatör olarak, yeni bir bellenim sürümü olan bir cihaz grubu yapılandırmanız gerekir. Her cihazdaki bellenimini tek tek güncellemek istemezsiniz. Bir cihaz grubundaki bellenimi güncelleştirmek için, uzaktan Izleme çözüm hızlandırıcısında cihaz gruplarını ve otomatik cihaz yönetimini kullanabilirsiniz. Cihaz grubuna eklediğiniz herhangi bir cihaz, cihaz çevrimiçi duruma geldiğinde en son bellenim yazılımını alır.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Cihaz grubu oluşturma
> * Bellenimi hazırlayın ve barındırın
> * Azure portal cihaz yapılandırması oluşturma
> * Bir cihaz yapılandırmasını uzaktan Izleme çözümünüze aktarma
> * Yapılandırmayı cihaz grubundaki cihazlara dağıtın
> * Dağıtımı izleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

<!--
If this is going to be a tutorial - we need to split this include into two so that we can accommodate the additional prerequisites:

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]
-->

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi takip etmek için Azure aboneliğinizde Uzaktan İzleme çözümü hızlandırıcısının dağıtılmış örneğine sahip olmanız gerekir.

Uzaktan İzleme çözümü hızlandırıcısını henüz dağıtmadıysanız [Bulut tabanlı uzaktan izleme çözümü dağıtma](quickstart-remote-monitoring-deploy.md) hızlı başlangıcını tamamlamanız gerekir.

Üretici yazılımı dosyalarınızı barındırmak için bir Azure depolama hesabınızın olması gerekir. Aboneliğinizde var olan bir depolama hesabı kullanabilir veya [Yeni bir depolama hesabı oluşturabilirsiniz](../storage/common/storage-account-create.md) .

Öğretici bir [IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) cihazını örnek bir cihaz olarak kullanır.

Yerel makinenizde aşağıdaki yazılımların yüklü olması gerekir:

* [Visual Studio Code (vs Code)](https://code.visualstudio.com/).
* [Azure IoT çalışma ekranı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) vs Code uzantısı.

Başlamadan önce:

* [IoT DevKit cihazındaki önyükleme yükleyicinin sürüm 1.4.0 veya daha yüksek](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/)olduğundan emin olun.
* IoT DevKit SDK 'sının önyükleme yükleyicisinden aynı sürümde olduğundan emin olun. IoT DevKit SDK 'sını, VS Code Azure IoT çalışma ekranı kullanarak güncelleştirebilirsiniz. Komut paletini açın ve **Arduino: Board Manager**girin. Daha fazla bilgi için bkz. [geliştirme ortamını hazırlama](../iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started.md#prepare-the-development-environment).

Ayrıca, en az bir IoT DevKit cihazını uzaktan Izleme çözüm hızlandırıcısına bağlamanız gerekir. IoT DevKit cihazı bağlanamadıysanız, bkz. [Mxyongaıot DEVKIT AZ3166 'ı IoT uzaktan izleme çözüm hızlandırıcısına bağlama](iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringV2.md).

## <a name="navigate-to-the-dashboard"></a>Panoya gidin

Uzaktan İzleme çözümü panosunu tarayıcınızda görüntülemek için önce [Microsoft Azure IoT Çözüm Hızlandırıcıları](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasına gidin. Azure aboneliği kimlik bilgilerinizi kullanarak oturum açmanız istenebilir.

Ardından **Hızlı başlangıç** ile dağıttığınız Uzaktan İzleme çözümü hızlandırıcısına ait kutucukta bulunan [Başlat](quickstart-remote-monitoring-deploy.md) öğesine tıklayın.

## <a name="create-a-device-group"></a>Cihaz grubu oluşturma

Bir cihaz grubundaki bellenimi otomatik olarak güncelleştirmek için, cihazların uzaktan Izleme çözümünüzdeki bir cihaz grubunun üyesi olması gerekir:

1. **Cihazlar** sayfasında, çözüm hızlandırıcısına bağladığınız tüm **IoT devkit** cihazlarını seçin. Ardından **Jobs** (İşler) öğesine tıklayın.

1. **İşler** panelinde, **Etiketler**' i seçin, Iş adını **adddevkittag**olarak ayarlayın ve ardından **Y**değeri olan **isdevkitcihazı** adlı bir metin etiketi ekleyin. Sonra **Uygula**' ya tıklayın.

1. Artık bir cihaz grubu oluşturmak için etiket değerlerini kullanabilirsiniz. **Cihazlar** sayfasında, **cihaz gruplarını yönet**' e tıklayın.

1. **Idevkitdevice** adını ve koşulundaki **Y** değerini kullanan bir metin filtresi oluşturun. Cihaz grubunu **IoT DevKit cihazları**olarak kaydedin.

Bu öğreticide daha sonra, tüm üyelerin bellenimini güncelleştiren bir cihaz yapılandırması uygulamak için bu cihaz grubunu kullanırsınız.

## <a name="prepare-and-host-the-firmware"></a>Bellenimi hazırlayın ve barındırın

[Azure IoT çalışma ekranı](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-iot-workbench) vs Code uzantısı, üretici yazılımı güncelleştirmesi için örnek cihaz kodunu içerir.

### <a name="open-the-firmware-ota-sample-in-vs-code"></a>Bellenim OTA örneğini VS Code ' de açın

1. IoT DevKit 'in bilgisayarınıza bağlı olmadığından emin olun. VS Code başlatın ve ardından DevKit 'i bilgisayarınıza bağlayın.

1. **F1** tuşuna basarak komut paletini açın, yazıp **IoT çalışma ekranı: örnekler**' i seçin. Ardından, pano olarak **IoT DevKit** ' i seçin.

1. **Bellenim OTA** ' yı bulun ve **örnek aç**' a tıklayın. Yeni bir VS Code penceresi açılır ve **firmware_ota** proje klasörünü gösterir:

    ![IoT çalışma ekranı, bellenim OTA örneğini seçin](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-firmware-example.png)

### <a name="build-the-new-firmware"></a>Yeni üretici yazılımını oluşturun

Cihaz belleniminin ilk sürümü 1.0.0 'dir. Yeni üretici yazılımının daha yüksek bir sürüm numarası olmalıdır.

1. VS Code **Firmwareota. ino** dosyasını açın ve `currentFirmwareVersion` `1.0.0` ' den `1.0.1`' e değiştirin:

    ![Bellenim sürümünü değiştir](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Komut paletini açın, yazıp **IoT çalışma ekranı: cihaz**' ı seçin. Kodu derlemek için **cihaz derle** 'yi seçin:

    ![Cihaz derleme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-compile.png)

    VS Code derlenen dosyayı projedeki `.build` klasörüne kaydeder. Ayarlarınıza bağlı olarak, VS Code gezgin görünümündeki `.build` klasörü gizleyebilir.

### <a name="generate-the-crc-value-and-calculate-the-firmware-file-size"></a>CRC değeri oluştur ve üretici yazılımı dosya boyutunu hesapla

1. Komut paletini açın, yazıp **IoT çalışma ekranı: cihaz**' ı seçin. Sonra **CRC oluştur**' u seçin:

    ![CRC oluştur](media/iot-accelerators-remote-monitoring-bulk-configuration-update/iot-workbench-device-crc.png)

1. VS Code, CRC değeri, bellenim dosya adı ve yolu ve çıkış penceresinde dosya boyutunu oluşturur ve yazdırır. Daha sonra bu değerleri bir yere göz önünde oluşturun:

    ![CRC bilgisi](media/iot-accelerators-remote-monitoring-bulk-configuration-update/crc-info.png)

### <a name="upload-the-firmware-to-the-cloud"></a>Üretici yazılımını buluta yükleyin

Yeni bellenim dosyanızı bulutta barındırmak için Azure depolama hesabınızı kullanın.

1. Azure portalda depolama hesabınıza gidin. Hizmetler bölümünde **Bloblar**' ı seçin. Bellenim dosyalarınızı depolamak için **bellenim** adlı ortak bir kapsayıcı oluşturun:

    ![Klasör Oluştur](media/iot-accelerators-remote-monitoring-bulk-configuration-update/blob-folder.png)

1. Üretici yazılımı dosyanızı kapsayıcıya yüklemek için **bellenim** kapsayıcısını seçin ve **karşıya yükle**' ye tıklayın.

1. **Firmwareota. ino. bin**' i seçin. Önceki bölümde bu dosyanın tam yolunu bir yere görürsünüz.

1. Üretici yazılımı dosyasının karşıya yüklenmesi tamamlandıktan sonra, dosya URL 'sini bir yere getirin.

### <a name="build-and-upload-the-original-firmware-to-the-iot-devkit-device"></a>Özgün üretici yazılımını IoT DevKit cihazına derleyin ve karşıya yükleyin

1. VS Code **Firmwareota. ino** dosyasını açın ve `currentFirmwareVersion` geri `1.0.0`olarak değiştirin:

    ![Sürüm 1.0.0](media/iot-accelerators-remote-monitoring-bulk-configuration-update/version-1-0-1.png)

1. Komut paletini açın, yazıp **IoT çalışma ekranı: cihaz**' ı seçin. Sonra **cihaz karşıya yükleme**' yi seçin:

    ![Cihaz karşıya yükleme](media/iot-accelerators-remote-monitoring-bulk-configuration-update/device-upload.png)

1. VS Code, IoT DevKit cihazınıza kodu doğrular ve yükler.

1. Karşıya yükleme tamamlandığında IoT DevKit cihazı yeniden başlatılır. Yeniden başlatma tamamlandığında IoT DevKit ekranı, **fw sürümü: 1.0.0**gösterir ve yeni üretici yazılımı olup olmadığını denetler:

    ![OTA-1](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-1.jpg)

## <a name="create-a-device-configuration"></a>Cihaz yapılandırması oluşturma

Cihaz yapılandırması, cihazlarınızın istenen durumunu belirtir. Genellikle, geliştirici Azure portal, yapılandırmayı **IoT cihaz yapılandırma** sayfasında [oluşturur](../iot-hub/iot-hub-automatic-device-management.md#create-a-configuration) . Cihaz yapılandırması, cihazlarınızın istenen durumunu ve bir dizi ölçümü belirten bir JSON belgesidir.

Aşağıdaki yapılandırmayı yerel makinenizde **bellenim-Update. JSON** adlı dosya olarak kaydedin. `YOURSTRORAGEACCOUNTNAME`, `YOURCHECKSUM`ve `YOURPACKAGESIZE` yer tutucuları, daha önce bir nota yaptığınız değerlerle değiştirin:

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

Bu yapılandırma dosyasını aşağıdaki bölümde kullanın.

## <a name="import-a-configuration"></a>Yapılandırma içeri aktar

Bu bölümde, cihaz yapılandırmasını uzaktan Izleme çözüm hızlandırıcısına bir paket olarak içeri aktarırsınız. Genellikle, bir operatör bu görevi tamamlar.

1. Uzaktan Izleme Web Kullanıcı arabiriminde, **paketler** sayfasına gidin ve **+ yeni paket**' e tıklayın:

    ![Yeni paket](media/iot-accelerators-remote-monitoring-bulk-configuration-update/packagepage.png)

1. **Yeni paket** panelinde, yapılandırma türü olarak paket türü ve **üretici yazılımı** olarak **cihaz yapılandırması** ' nı seçin. Yerel makinenizde **bellenim-Update. JSON** dosyasını bulmak Için, **Araştır** ' a tıklayın ve ardından **karşıya yükle**' ye tıklayın:

    ![Paketi karşıya yükle](media/iot-accelerators-remote-monitoring-bulk-configuration-update/uploadpackage.png)

1. Paket listesi artık **bellenim-güncelleştirme** paketini içerir.

## <a name="deploy-the-configuration-to-your-devices"></a>Yapılandırmayı cihazlarınıza dağıtın

Bu bölümde, IoT DevKit cihazlarınıza cihaz yapılandırmasını uygulayan bir dağıtım oluşturup yürütmeniz gerekir.

1. Uzaktan Izleme Web Kullanıcı arabiriminde **dağıtımlar** sayfasına gidin ve **+ yeni dağıtım**' ye tıklayın:

    ![Yeni dağıtım](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentpage.png)

1. **Yeni dağıtım** panelinde, aşağıdaki ayarlarla bir dağıtım oluşturun:

    |Seçenek|Değer|
    |---|---|
    |Adı|Üretici yazılımı güncelleştirmesini dağıtma|
    |Paket türü|Cihaz yapılandırması|
    |Yapılandırma türü|Üretici yazılımı|
    |Paket|bellenim-Update. JSON|
    |Cihaz grubu|IoT DevKit cihazları|
    |Öncelik|10|

    ![Dağıtım oluşturma](media/iot-accelerators-remote-monitoring-bulk-configuration-update/newdeployment.png)

    **Apply (Uygula)** düğmesine tıklayın. **Dağıtımlar** sayfasında, aşağıdaki ölçümleri gösteren yeni bir dağıtım görürsünüz:

    * **Hedeflenen** cihaz grubundaki cihazların sayısını gösterir.
    * **Uygulanan** , yapılandırma içeriğiyle güncelleştirilmiş cihazların sayısını gösterir.
    * **Başarılı** , dağıtımda başarılı rapor veren cihazların sayısını gösterir.
    * **Başarısız** olan dağıtımda, dağıtımdaki cihazların sayısını gösterir.

## <a name="monitor-the-deployment"></a>Dağıtımı izleme

Birkaç dakika sonra IoT DevKit yeni bellenim bilgilerini alır ve cihaza indirmeye başlar:

![OTA-2](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-2.jpg)

Ağınızın hızına bağlı olarak, indirme birkaç dakika sürebilir. Üretici yazılımı indirildikten sonra cihaz dosya boyutunu ve CRC değerini doğrular. Doğrulama başarılı olursa, Mxyongasındaki ekran **geçti** görüntülenir.

![OTA-3](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-3.jpg)

Denetim başarılı olursa cihaz yeniden başlatılır. Yeniden başlatma işlemi yapılmadan önce **5** ile **0** arasında bir geri sayım görürsünüz.

![OTA-4](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-4.jpg)

Yeniden başlatmanın ardından IoT DevKit önyükleme yükleyicisi, bellenimi yeni sürüme yükseltir. Yükseltme birkaç saniye sürebilir. Bu aşamada, cihazdaki RGB Kırmızı ve ekran boştur.

![OTA-5](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-5.jpg)

Yeniden başlatma tamamlandığında, IoT DevKit cihazınız artık bellenimin 1.0.1 sürümünü çalıştırıyor.

![OTA-6](media/iot-accelerators-remote-monitoring-bulk-configuration-update/ota-6.jpg)

**Dağıtımlar** sayfasında, güncelleştirdikleri şekilde cihazların durumunu görmek için bir dağıtıma tıklayın. Cihaz grubunuzda her bir cihazın durumunu ve tanımladığınız özel ölçümleri görebilirsiniz.

![Dağıtım ayrıntıları](media/iot-accelerators-remote-monitoring-bulk-configuration-update/deploymentstatus.png)

[!INCLUDE [iot-accelerators-tutorial-cleanup](../../includes/iot-accelerators-tutorial-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, çözümünüze bağlı bir cihaz grubunun bellenimini güncelleştirme konusu gösterilmektedir. Cihazları güncelleştirmek için çözümünüz otomatik cihaz yönetimini kullanır. Çözümünüzün temel alınan IoT Hub 'ında otomatik cihaz yönetimi özelliği hakkında daha fazla bilgi edinmek için bkz. [Azure Portal kullanarak IoT cihazlarını ölçeklendirerek yapılandırma ve izleme](../iot-hub/iot-hub-auto-device-config.md).