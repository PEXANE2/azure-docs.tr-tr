---
title: Azure IoT Central Azure IoT Edge bir cihaz ekleme | Microsoft Docs
description: Bir operatör olarak, Azure IoT Central uygulamanıza Azure IoT Edge bir cihaz ekleyin
author: rangv
ms.author: rangv
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: c60cf4b90b089d271c0ccd91031420efe9017b1e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81758165"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application"></a>Öğretici: Azure IoT Central uygulamanıza Azure IoT Edge bir cihaz ekleme

*Bu makale, çözüm oluşturucular ve cihaz geliştiricileri için geçerlidir.*

Bu öğreticide, Azure IoT Central uygulamanıza bir Azure IoT Edge cihazının nasıl yapılandırılacağı ve ekleneceği gösterilmektedir. Öğretici, bir IoT Edge cihazının benzetimini yapmak için Azure Marketi 'nden IoT Edge özellikli bir Linux sanal makinesi (VM) kullanır. IoT Edge cihaz, sanal çevresel telemetri üreten bir modül kullanır. Telemetri IoT Central uygulamanızdaki bir panoda görüntülenir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * IoT Edge cihaz için cihaz şablonu oluşturma
> * IoT Central IoT Edge cihaz oluşturma
> * Bir Linux sanal makinesine sanal bir IoT Edge cihazı dağıtma

## <a name="prerequisites"></a>Ön koşullar

Özel uygulama **> özel uygulama** şablonunu kullanarak bir IoT Central uygulaması oluşturmak için [Azure IoT Central uygulaması oluşturma](./quick-deploy-iot-central.md) hızlı başlangıcı ' nı doldurun.

Bu öğreticideki adımları tamamlayabilmeniz için etkin bir Azure aboneliğine ihtiyacınız vardır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

IoT Edge bildirim dosyasını GitHub 'dan indirin. Aşağıdaki bağlantıya sağ tıklayıp **Bağlantıyı farklı kaydet**' i seçin: [environmentalsensormanifest. JSON](https://raw.githubusercontent.com/Azure-Samples/iot-central-docs-samples/master/iotedge/EnvironmentalSensorManifest.json)

## <a name="create-device-template"></a>Cihaz şablonu oluştur

Bu bölümde, IoT Central uygulamanıza bağlanan IoT Edge bir cihaz için bir cihaz şablonu oluşturacaksınız. Başlamak için bir IoT Edge bildirimi içeri aktarır ve sonra telemetri tanımları ve görünümleri eklemek için şablonu değiştirirsiniz:

### <a name="import-manifest-to-create-template"></a>Şablon oluşturmak için bildirimi içeri aktar

IoT Edge bildiriminden bir cihaz şablonu oluşturmak için:

1. IoT Central uygulamanızda **cihaz şablonları** ' na gidin ve **+ Yeni**' yi seçin.

1. **Şablon türü seç** sayfasında **Azure IoT Edge** kutucuğunu seçin. Ardından **İleri: Özelleştir**' i seçin.

1. **Azure IoT Edge dağıtım bildirimini karşıya** Yükle sayfasında, daha önce Indirdiğiniz **Environmentalsensormanifest. JSON** ' ı karşıya yüklemek için **Araştır** ' ı seçin. Sonra Ileri ' yi seçin **: İnceleme**.

1. **İnceleme** sayfasında **Oluştur**' u seçin.

1. Şablon oluşturulduğunda, adını *çevresel algılayıcı Edge cihazı*olarak değiştirin.

1. Bildirimde tanımlanan iki özelliği görüntülemek için **SimulatedTemperatureSensor** modülündeki **Yönet** arabirimini seçin:

![IoT Edge bildiriminden oluşturulan cihaz şablonu](./media/tutorial-add-edge-as-leaf-device/imported-manifest.png)

### <a name="add-telemetry-to-manifest"></a>Bildirime telemetri Ekle

IoT Edge bildirimi, bir modülün gönderdiği Telemetriyi tanımlamaz. Telemetri tanımlarını cihaz şablonuna eklemeniz gerekir. **SimulatedTemperatureSensor** modülü aşağıdaki JSON gibi görünen telemetri iletilerini gönderir:

```json
{
    "machine": {
        "temperature": 75.0,
        "pressure": 40.2
    },
    "ambient": {
        "temperature": 23.0,
        "humidity": 30.0
    },
    "timeCreated": ""
}
```

Telemetri tanımlarını cihaz şablonuna eklemek için:

1. **Çevresel algılayıcı Edge cihaz** şablonunda **Yönet** arabirimini seçin.

1. **+ Özellik Ekle**' yi seçin. **Görüntü adı** olarak *makine* girin ve **yetenek türünün** **telemetri**olduğundan emin olun.

1. Şema türü olarak **nesne** ' yi seçin ve ardından **Tanımla**' yı seçin. Nesne tanımı sayfasında, **Double** türünden öznitelik olarak *sıcaklık* ve *basınç* ekleyin ve ardından **Uygula**' yı seçin.

1. **+ Özellik Ekle**' yi seçin. **Görüntü adı** olarak *ortam* girin ve **yetenek türünün** **telemetri**olduğundan emin olun.

1. Şema türü olarak **nesne** ' yi seçin ve ardından **Tanımla**' yı seçin. Nesne tanımı sayfasında, **Double** türü öznitelikleri olarak *sıcaklık* ve *nem* ekleyin ve ardından **Uygula**' yı seçin.

1. **+ Özellik Ekle**' yi seçin. **Görünen ad** olarak *TimeCreated* yazın ve **yetenek türünün** **telemetri**olduğundan emin olun.

1. Şema türü olarak **DateTime** öğesini seçin.

1. Şablonu güncelleştirmek için **Kaydet** ' i seçin.

**Manage** Interface artık **Machine**, **Ambient**ve **TimeCreated** telemetri türlerini içerir:

![Makine ve ortam telemetri türleri ile arabirim](./media/tutorial-add-edge-as-leaf-device/manage-interface.png)

### <a name="add-views-to-template"></a>Şablona görünümler ekleme

Cihaz şablonunda, bir işlecin IoT Edge cihazdan Telemetriyi görmesini sağlayan bir görünüm yoktur. Cihaz şablonuna bir görünüm eklemek için:

1. **Ortam algılayıcı uç cihaz** şablonunda **Görünümler** ' i seçin.

1. **Yeni bir görünüm eklemek Için seçin** sayfasında, cihaz kutucuğunu **görselleştirmeyi** seçin.

1. Görünüm adını *IoT Edge cihaz telemetrisini görüntülemek*için değiştirin.

1. **Çevresel** ve **makine** telemetri türlerini seçin. Ardından **kutucuk Ekle**' yi seçin.

1. **Görünüm IoT Edge cihaz telemetrisi** görünümünü kaydetmek için **Kaydet** ' i seçin.

![Telemetri görünümü ile cihaz şablonu](./media/tutorial-add-edge-as-leaf-device/template-telemetry-view.png)

### <a name="publish-the-template"></a>Şablonu yayımlama

**Ortam algılayıcı Edge cihaz** şablonunu kullanan bir cihaz ekleyebilmeniz için önce şablonu yayımlamanız gerekir.

**Ortam algılayıcı Edge cihaz** şablonuna gidin ve **Yayımla**' yı seçin. Şablonu yayımlamak için **Yayımla** ' yı seçin:

![Cihaz şablonunu yayımlama](./media/tutorial-add-edge-as-leaf-device/publish-template.png)

## <a name="add-iot-edge-device"></a>IoT Edge cihaz ekle

Artık **çevresel algılayıcı Edge cihaz** şablonunu yayımladınız, IoT Central uygulamanıza bir cihaz ekleyebilirsiniz:

1. IoT Central uygulamanızda, **cihazlar** sayfasına gidin ve kullanılabilir şablonlar listesinden **çevresel algılayıcı kenar cihazı** ' nı seçin.

1. Şablondan **+** yeni bir cihaz eklemek için seçin. **Yeni cihaz oluştur** sayfasında **Oluştur**' u seçin.

Artık **kayıtlı**durumda olan yeni bir cihazınız var:

![Cihaz şablonunu yayımlama](./media/tutorial-add-edge-as-leaf-device/new-device.png)

### <a name="get-the-device-credentials"></a>Cihaz kimlik bilgilerini al

IoT Edge cihazını Bu öğreticide daha sonra dağıtırken, cihazın IoT Central uygulamanıza bağlanmasına izin veren kimlik bilgileri gerekir. Cihaz kimlik bilgilerini al:

1. **Cihaz** sayfasında, oluşturduğunuz aygıtı seçin.

1. **Bağlan**’ı seçin.

1. **Cihaz bağlantısı** sayfasında, **kimlik KAPSAMı**, **cihaz kimliği**ve **birincil anahtarı**bir yere unutmayın. Bu değerleri daha sonra kullanırsınız.

1. **Kapat**'ı seçin.

Artık, bir IoT Edge cihazının bağlanmasını sağlamak için IoT Central uygulamanızı yapılandırmayı tamamladınız.

## <a name="deploy-an-iot-edge-device"></a>IoT Edge cihazı dağıtma

Bu öğreticide, bir IoT Edge cihazının benzetimini yapmak için Azure 'da oluşturulan Azure IoT Edge özellikli bir Linux VM kullanırsınız. IoT Edge özellikli VM oluşturmak için:

1. Azure Marketi 'nde [Ubuntu üzerinde Azure IoT Edge](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) gidin. Ardından **Şimdi al**' ı seçin.

1. **Bu uygulamayı Azure 'Da oluştur** sayfasında **devam**' ı seçin. Bu bağlantı sizi Azure aboneliğinizde oturum açmanız gerekebilecek Azure portal yönlendirir.

1. Azure portal **Ubuntu sayfasındaki Azure IoT Edge** **Oluştur**' u seçin.

1. **Sanal makine oluşturma > temel bilgiler** sayfasında:

    - Azure aboneliğinizi seçin.
    - **IoT-Edge-Devices**adlı yeni bir kaynak grubu oluşturun.
    - Sanal makine adını kullan: **ıotedgevm**.
    - Size en yakın bölgeyi seçin.
    - Kimlik doğrulama türünü **parola**olarak ayarlayın.
    - Bir Kullanıcı adı ve parola seçin.
    - Diğer seçenekleri varsayılan değerlerine bırakabilirsiniz.
    - **İncele ve oluştur**’u seçin.

1. Doğrulama tamamlandığında **Oluştur**' u seçin.

Birkaç dakika sonra, dağıtım tamamlandığında **Kaynağa Git**' i seçin.

### <a name="provision-vm-as-an-iot-edge-device"></a>IoT Edge cihaz olarak VM sağlama 

VM 'yi bir IoT Edge cihaz olarak sağlamak için:

1. **Destek + sorun giderme** bölümünde **seri konsol**' yi seçin.

1. İstemi görmek için ENTER tuşuna basın. **Enter** `login:` Oturum açmak için Kullanıcı adınızı ve parolanızı girin.

1. IoT Edge çalışma zamanı sürümünü denetlemek için aşağıdaki komutu çalıştırın. Yazma sırasında sürüm 1.0.8 ' dir:

    ```bash
    sudo iotedge --version
    ```

1. IoT Edge config `nano` . YAML dosyasını açmak için düzenleyiciyi kullanın:

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

1. Görene kadar aşağı kaydırın `# Manual provisioning configuration`. Aşağıdaki kod parçacığında gösterildiği gibi sonraki üç satırı Açıklama:

    ```yaml
    # Manual provisioning configuration
    #provisioning:
    #  source: "manual"
    #  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
    ```

1. Görene kadar aşağı kaydırın `# DPS symmetric key provisioning configuration`. Aşağıdaki kod parçacığında gösterildiği gibi sonraki sekiz satırın açıklamasını kaldırın:

    ```yaml
    # DPS symmetric key provisioning configuration
    provisioning:
      source: "dps"
      global_endpoint: "https://global.azure-devices-provisioning.net"
      scope_id: "{scope_id}"
      attestation:
        method: "symmetric_key"
        registration_id: "{registration_id}"
        symmetric_key: "{symmetric_key}"
    ```

1. Daha `{scope_id}` önce bir örneği yaptığınız Için **kimlik kapsamıyla** değiştirin.

1. Daha `{registration_id}` önce bir nota YAPTıĞıNıZ **cihaz kimliğiyle** değiştirin.

1. Daha `{symmetric_key}` önce bir notunuz yaptığınız **birincil anahtarla** değiştirin.

1. Değişiklikleri kaydedin (**CTRL-O**) ve Exit (**CTRL-X**) `nano` .

1. IoT Edge Daemon 'u yeniden başlatmak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo systemctl restart iotedge
    ```

1. IoT Edge modüllerinin durumunu denetlemek için şu komutu çalıştırın:

    ```bash
    iotedge list
    ```

    Çıktı aşağıdaki gibi görünür:

    ```bash
    NAME                        STATUS           DESCRIPTION      CONFIG
    SimulatedTemperatureSensor  running          Up 20 seconds    mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0
    edgeAgent                   running          Up 27 seconds    mcr.microsoft.com/azureiotedge-agent:1.0
    edgeHub                     running          Up 22 seconds    mcr.microsoft.com/azureiotedge-hub:1.0
    ```

## <a name="view-the-telemetry"></a>Telemetriyi görüntüleme

Sanal IoT Edge cihaz artık VM 'de çalışıyor. IoT Central uygulamanızda cihaz durumu artık **cihazlar** sayfasında **sağlanıyor** :

![Sağlanan cihaz](./media/tutorial-add-edge-as-leaf-device/provisioned-device.png)

Telemetriyi **görüntüleme IoT Edge cihaz telemetrisi** sayfasında görebilirsiniz:

![Cihaz telemetrisi](./media/tutorial-add-edge-as-leaf-device/device-telemetry-view.png)

**Modüller** sayfası IoT Edge modüllerinin durumunu gösterir:

![Cihaz telemetrisi](./media/tutorial-add-edge-as-leaf-device/edge-module-status.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz geliştiricisi olarak, artık IoT Central IoT Edge cihazlarıyla nasıl çalışabileceğinizi öğrendiğinize göre, önerilen bir sonraki adım okunmalıdır:

<!-- Next how-tos in the sequence -->

> [!div class="nextstepaction"]
> [Azure IoT Central 'e bağlanın](./concepts-get-connected.md)
