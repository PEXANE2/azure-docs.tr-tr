---
title: Azure IoT Central'a Azure IoT Edge aygıtı ekleme | Microsoft Dokümanlar
description: Operatör olarak, Azure IoT Merkezi uygulamanıza bir Azure IoT Edge aygıtı ekleyin
author: rangv
ms.author: rangv
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: bf74784998de6bbad6310c48c24d6353512bff13
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77027737"
---
# <a name="tutorial-add-an-azure-iot-edge-device-to-your-azure-iot-central-application"></a>Öğretici: Azure IoT Merkezi uygulamanıza Azure IoT Edge aygıtı ekleme

Bu öğretici, Azure IoT Merkezi uygulamanıza nasıl bir Azure IoT Edge aygıtını yapılandırabileceğinizi ve ekleyeceğiniz gösterilmektedir. Öğretici, Bir IoT Edge aygıtını simüle etmek için Azure Marketplace'ten ioT Edge özellikli bir Linux sanal makinesini (VM) kullanır. IoT Edge cihazı, simüle edilmiş çevresel telemetri üreten bir modül kullanır. IoT Central uygulamanızdaki bir panoda telemetriyi görüntüleyebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * IoT Edge aygıtı için aygıt şablonu oluşturma
> * IoT Central'da bir IoT Edge cihazı oluşturma
> * Simüle edilmiş bir IoT Edge aygıtını Linux VM'ye dağıtma

## <a name="prerequisites"></a>Ön koşullar

**Özel uygulama > Özel uygulama** şablonu kullanarak bir IoT Central uygulaması oluşturmak için Azure [IoT Central uygulamasını](./quick-deploy-iot-central.md) hızlı bir şekilde oluşturun'ı tamamlayın.

Bu öğreticideki adımları tamamlamak için etkin bir Azure aboneliğine ihtiyacınız var.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

GitHub'dan IoT Edge bildirim dosyasını indirin. Aşağıdaki linke sağ tıklayın ve sonra **Save linkini**seçin : [EnvironmentalSensorManifest.json](https://raw.githubusercontent.com/Azure-Samples/iot-central-docs-samples/master/iotedge/EnvironmentalSensorManifest.json)

## <a name="create-device-template"></a>Aygıt şablonu oluşturma

Bu bölümde, IoT Merkezi uygulamanıza bağlanan bir IoT Edge aygıtı için bir aygıt şablonu oluşturursunuz. Başlamak için bir IoT Edge bildirimi içe aktarır sınız ve ardından şablonu telemetri tanımları ve görünümleri eklemek için değiştirirsiniz:

### <a name="import-manifest-to-create-template"></a>Şablon oluşturmak için manifesto alma

IoT Edge bildiriminden bir aygıt şablonu oluşturmak için:

1. IoT Merkezi uygulamanızda **Aygıt şablonlarına** gidin ve **+ Yeni'yi**seçin.

1. Şablon **türünü seç** sayfasında **Azure IoT Edge** döşemesini seçin. Sonra **İleri'yi seçin: Özelleştir.**

1. Azure **IoT Edge dağıtım bildirimi yükle** sayfasında, daha önce indirdiğiniz **EnvironmentalSensorManifest.json'ı** yüklemek için **Gözat'ı** seçin. Sonra **Sonraki seçin: Gözden geçirin.**

1. Gözden **Geçir** sayfasında **Oluştur'u**seçin.

1. Şablon oluşturulduğunda, adını *Environmental Sensor Edge Device*olarak değiştirin.

1. Bildirimde tanımlanan iki özelliği görüntülemek için **Simüle Edilen Sıcaklık Sensörü** modülündeki **Yönet** arabirimini seçin:

![IoT Edge bildiriminden oluşturulan aygıt şablonu](./media/tutorial-add-edge-as-leaf-device/imported-manifest.png)

### <a name="add-telemetry-to-manifest"></a>Tezahür etmek için telemetri ekleme

IoT Edge manifestosu, bir modülün gönderdiği telemetriyi tanımlamaz. Telemetri tanımlarını aygıt şablonuna eklemeniz gerekir. **Simüle TemperatureSensor** modülü aşağıdaki JSON gibi görünen telemetri mesajları gönderir:

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

Telemetri tanımlarını aygıt şablonuna eklemek için:

1. Çevre Sensörü Kenar **Aygıtı** şablonundaki **Yönet** arabirimini seçin.

1. Select **+ Ekle özelliği**. *Makineyi* **Görüntü adı** olarak girin ve **Yetenek türünün** **Telemetri**olduğundan emin olun.

1. **Şema** türü olarak Nesne'yi seçin ve sonra **Tanımla'yı**seçin. Nesne tanımı sayfasında, **Çift** türüne öznitelikleri olarak *sıcaklık* ve *basınç* ekleyin ve sonra **Uygula'yı**seçin.

1. Select **+ Ekle özelliği**. **Ortama Görüntü adı** *olarak* girin ve **Yetenek türünün** **Telemetri**olduğundan emin olun.

1. **Şema** türü olarak Nesne'yi seçin ve sonra **Tanımla'yı**seçin. Nesne tanımı sayfasında, **Çift** türüne göre *sıcaklık* ve *nem* ekleyin ve sonra **Uygula'yı**seçin.

1. Select **+ Ekle özelliği**. **Görüntü adı** olarak *oluşturulan zamanı* girin ve Yetenek **türünün** **Telemetri**olduğundan emin olun.

1. Şema türü olarak **DateTime'ı** seçin.

1. Şablonu güncelleştirmek için **Kaydet'i** seçin.

**Yönet** arabirimi artık **makine,** **ortam**ve **zamanOluşturulan** telemetri türlerini içerir:

![Makine ve ortam telemetri seçimi tipleriyle arayüz](./media/tutorial-add-edge-as-leaf-device/manage-interface.png)

### <a name="add-views-to-template"></a>Şablona görünüm ekleme

Aygıt şablonu henüz bir operatörün IoT Edge aygıtından telemetriyi görmesini sağlayan bir görünüme sahip değil. Aygıt şablonuna görünüm eklemek için:

1. Çevre Sensörü **Kenar Aygıtı** şablonundaki **Görünümler'i** seçin.

1. Yeni **bir görünüm** sayfası eklemek için **Seç'te aygıt döşemesini Görselleştirme'yi** seçin.

1. Görünüm adını *IoT Edge aygıt telemetrisini görüntüle olarak*değiştirin.

1. **Ortam** ve **makine** telemetri türlerini seçin. Ardından **döşeme ekle'yi**seçin.

1. **IoT Edge aygıtını görüntülemek** için **Kaydet'i** seçin.

![Telemetri görünümünde aygıt şablonu](./media/tutorial-add-edge-as-leaf-device/template-telemetry-view.png)

### <a name="publish-the-template"></a>Şablonu yayımlama

**Çevre Sensörü Kenar Aygıtı** şablonu kullanan bir aygıt eklemeden önce şablonu yayımlamanız gerekir.

**Çevre Sensörü Kenar Aygıtı** şablonuna gidin ve **Yayımla'yı**seçin. Ardından şablonu yayımlamak için **Yayımla'yı** seçin:

![Aygıt şablonu yayımlama](./media/tutorial-add-edge-as-leaf-device/publish-template.png)

## <a name="add-iot-edge-device"></a>IoT Edge cihazı ekle

Artık Çevre Sensörü **Kenar Cihazı** şablonunu yayınladınız, IoT Merkezi uygulamanıza bir cihaz ekleyebilirsiniz:

1. IoT Merkezi uygulamanızda, **Cihazlar** sayfasına gidin ve kullanılabilir şablonlar listesinde **Çevre Sensörü Kenar Aygıtı'nı** seçin.

1. Şablondan yeni bir aygıt eklemek için seçin. **+** Yeni **aygıt oluştur** sayfasında **Oluştur'u**seçin.

Şimdi **kayıtlı**durumu ile yeni bir cihaz var:

![Aygıt şablonu yayımlama](./media/tutorial-add-edge-as-leaf-device/new-device.png)

### <a name="get-the-device-credentials"></a>Aygıt kimlik bilgilerini alma

Bu öğreticinin ilerleyen saatlerinde IoT Edge aygıtını dağıttığınızda, aygıtın IoT Merkezi uygulamanıza bağlanmasına izin veren kimlik bilgilerine ihtiyacınız vardır. Aygıt kimlik bilgilerini alma:

1. **Aygıt** sayfasında, oluşturduğunuz aygıtı seçin.

1. **Bağlan**’ı seçin.

1. Aygıt **bağlantı** sayfasında, **Kimlik Kapsamı,** **Aygıt Kimliği**ve Birincil **Anahtar'a**not edin. Bu değerleri daha sonra kullanırsınız.

1. **Kapat**'ı seçin.

IoT Central uygulamanızı, bir IoT Edge aygıtının bağlanmasını sağlayacak şekilde yapılandırmayı tamamladınız.

## <a name="deploy-an-iot-edge-device"></a>Bir IoT Edge aygıtı dağıtma

Bu eğitimde, bir IoT Edge aygıtını simüle etmek için Azure'da oluşturulan Azure IoT Edge özellikli bir Linux VM'si kullanırsınız. IoT Edge özellikli VM'yi oluşturmak için:

1. Azure Marketi'nde [Ubuntu'da Azure IoT Edge'e](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu?tab=Overview) gidin. Sonra **şimdi al'ı**seçin.

1. Azure sayfasında **bu uygulamayı oluştur'da** **Devam et'i**seçin. Bu bağlantı sizi Azure portalına götürür ve Azure aboneliğinizde oturum açmanız gerekebilir.

1. Azure **portalındaki Ubuntu sayfasındaki Azure IoT Edge'de** **Oluştur'u**seçin.

1. Temel **bilgiler sayfasına > sanal bir makine oluştur:**

    - Azure aboneliğinizi seçin.
    - **iot-edge aygıtları**adlı yeni bir kaynak grubu oluşturun.
    - Sanal makine adını kullanın: **iotedgevm**.
    - Size en yakın bölgeyi seçin.
    - Kimlik doğrulama türünü **Parola**olarak ayarlayın.
    - Bir kullanıcı adı ve parola seçin.
    - Diğer seçenekleri varsayılan değerlerine bırakabilirsiniz.
    - **İncele ve oluştur**’u seçin.

1. Doğrulama tamamlandığında **Oluştur'u**seçin.

Birkaç dakika sonra, dağıtım tamamlandığında **kaynağa git'i**seçin.

### <a name="provision-vm-as-an-iot-edge-device"></a>IoT Edge cihazı olarak VM sağlama 

VM'yi IoT Edge aygıtı olarak sağlamak için:

1. Destek **+ sorun giderme** bölümünde **Seri konsolu'nu**seçin.

1. İstemi görmek `login:` için **Enter** tuşuna basın. Oturum etmek için kullanıcı adınızı ve şifrenizi girin.

1. IoT Edge çalışma zamanı sürümünü denetlemek için aşağıdaki komutu çalıştırın. Yazma sırasında, sürüm 1.0.8' dir:

    ```bash
    sudo iotedge --version
    ```

1. IoT Edge config.yaml dosyasını açmak için `nano` düzenleyiciyi kullanın:

    ```bash
    sudo nano /etc/iotedge/config.yaml
    ```

1. Görene `# Manual provisioning configuration`kadar aşağı kaydırın. Aşağıdaki snippet gösterildiği gibi sonraki üç satır dışarı yorum:

    ```yaml
    # Manual provisioning configuration
    #provisioning:
    #  source: "manual"
    #  device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
    ```

1. Görene `# DPS symmetric key provisioning configuration`kadar aşağı kaydırın. Aşağıdaki parçacıkta gösterildiği gibi sonraki sekiz satırı açıklama:

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

1. Daha `{scope_id}` önce not aldığınız için **Kimlik Kapsamı** ile değiştirin.

1. Daha `{registration_id}` önce not aldığınız **Aygıt Kimliği** ile değiştirin.

1. Daha `{symmetric_key}` önce not aldığınız **Birincil anahtarla** değiştirin.

1. Değişiklikleri kaydedin (**Ctrl-O**) ve çıkış `nano` (**Ctrl-X**) düzenleyici.

1. IoT Edge daemon'u yeniden başlatmak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo systemctl restart iotedge
    ```

1. IoT Edge modüllerinin durumunu kontrol etmek için aşağıdaki komutu çalıştırın:

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

## <a name="view-the-telemetry"></a>Telemetriyi görüntüle

Simüle Edilmiş IoT Edge aygıtı şu anda VM'de çalışıyor. IoT Merkezi uygulamanızda, aygıt durumu artık **Cihazlar** sayfasında **verilmiştir:**

![Tedarik edilen cihaz](./media/tutorial-add-edge-as-leaf-device/provisioned-device.png)

Telemetriyi **View IoT Edge aygıt telemetri** sayfasında görebilirsiniz:

![Cihaz telemetrisi](./media/tutorial-add-edge-as-leaf-device/device-telemetry-view.png)

**Modüller** sayfası IoT Edge modüllerinin durumunu gösterir:

![Cihaz telemetrisi](./media/tutorial-add-edge-as-leaf-device/edge-module-status.png)

## <a name="next-steps"></a>Sonraki adımlar

IoT Central'daki IoT Edge cihazlarıyla çalışmayı ve yönetmeyi öğrendiğiniz için önerilen bir sonraki adım şu:

<!-- Next how-tos in the sequence -->

> [!div class="nextstepaction"]
> [Saydam ağ geçidini yapılandırma](../../iot-edge/how-to-create-transparent-gateway.md)
