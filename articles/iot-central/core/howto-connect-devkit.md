---
title: DevKit cihazını Azure IoT Merkezi uygulamanıza bağlayın | Microsoft Dokümanlar
description: Bir aygıt geliştiricisi olarak, IoT Tak ve Çalıştır (önizleme) kullanarak Bir MXChip IoT DevKit cihazını Azure IoT Merkezi uygulamanıza nasıl bağlayabileceğinizi öğrenin.
author: liydu
ms.author: liydu
ms.date: 12/03/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: jeffya
ms.openlocfilehash: bcf1dd2f89cf049d7da5b56170b2c13874c83ba4
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81756796"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Bir MXChip IoT DevKit cihazını Azure IoT Merkezi uygulamanıza bağlayın

*Bu makale, aygıt geliştiricileri için geçerlidir.*

Bu makalede, bir MXChip IoT DevKit (DevKit) aygıtını Azure IoT Merkezi uygulamasına nasıl bağlayabileceğiniz gösterilmektedir. Cihaz, IoT Central bağlantısını yapılandırmak için DevKit aygıtı için onaylı IoT Tak ve Çalıştır (önizleme) modelini kullanır.

Bu nasıl-nasıl makalede, sen:

- IoT Central uygulamanızdan bağlantı bilgilerini alın.
- Cihazı hazırlayın ve IoT Central uygulamanıza bağlayın.
- IoT Central'daki cihazdan telemetri ve özellikleri görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdaki kaynaklar gereklidir:

- DevKit [cihazı.](https://aka.ms/iot-devkit-purchase)
- Bir IoT Merkezi uygulaması. [Bir IoT Merkezi uygulaması oluştur'daki](./quick-deploy-iot-central.md)adımları izleyebilirsiniz.

## <a name="get-device-connection-details"></a>Cihaz bağlantısı ayrıntılarını alma

1. Azure IoT Merkezi uygulamanızda **Aygıt Şablonları** sekmesini seçin ve **+ Yeni'yi**seçin. Bölümde **Önceden yapılandırılmış bir aygıt şablonu kullanın**, **MXChip IoT DevKit'i**seçin.

    ![MXChip IoT DevKit için cihaz şablonu](media/howto-connect-devkit/device-template.png)

1. **Sonrakini Seçin: Özelleştir** in ve sonra **Oluştur.**

1. **Cihazlar** sekmesini seçin. Aygıtlar listesinde, Şablondan yeni bir aygıt oluşturmak için **MXChip IoT DevKit'i** seçin ve **+ Yeni'yi** seçin.

    ![Yeni cihaz](media/howto-connect-devkit/new-device.png)

1. Açılan pencereye **Aygıt Kimliğini** ve Aygıt `SampleDevKit` **Adı'nı** ' olarak `MXChip IoT DevKit - Sample`girin. **Benzetim** seçeneğinin kapalı olduğundan emin olun. Ardından **Oluştur**’u seçin.

    ![Cihaz kimliği ve adı](media/howto-connect-devkit/device-id-name.png)

1. Oluşturduğunuz aygıtı seçin ve sonra **Bağlan'ı**seçin. **Kimlik Kapsamı,** **Aygıt Kimliği**ve Birincil **anahtarı**not edin. Bu nasıl yapılsa makalede bu değerlere daha sonra ihtiyacınız var.

    ![Cihaz bağlantı bilgileri](media/howto-connect-devkit/device-connection-info.png)

## <a name="prepare-the-device"></a>Cihazı hazırlayın

1. DevKit aygıtı için önceden oluşturulmuş en son [Azure IoT Merkezi Tak ve Çalıştır (önizleme) firmware'ini](https://github.com/Azure-Samples/mxchip-iot-devkit-pnp/raw/master/bin/iotc_devkit.bin) GitHub'dan indirin.

1. DevKit cihazını usb kablosu kullanarak geliştirme makinenize bağlayın. Windows'da, DevKit aygıtındaki depolama alanına eşlenen bir sürücüde bir dosya gezgini penceresi açılır. Örneğin, sürücü **AZ3166 (D:)** olarak adlandırılabilir.

1. **iotc_devkit.bin** dosyasını sürücü penceresine sürükleyin. Kopyalama tamamlandığında, aygıt yeni firmware ile yeniden başlatılır.

    > [!NOTE]
    > **Wi-Fi yok**gibi ekranda hatalar görürseniz, bunun nedeni DevKit'in henüz WiFi'a bağlanmamış olmasıdır.

1. DevKit'te B **tuşuna**basılı tutun, **Sıfırla** düğmesine basın ve bırakın ve ardından **B düğmesini**bırakın. Aygıt artık erişim noktası modunda. Doğrulamak için, ekran "IoT DevKit - AP" ve yapılandırma portalı IP adresini görüntüler.

1. Bilgisayarınızda veya tabletinizde, aygıtın ekranında gösterilen WiFi ağ adına bağlanın. WiFi ağı AZ ile **başlar-** ardından MAC adresi. Bu ağa bağlandığınızda, internet erişiminiz yoktur. Bu durum bekleniyor ve aygıtı yapılandırırken bu ağa yalnızca kısa bir süre için bağlanırsınız.

1. Web tarayıcınızı açın [http://192.168.0.1/](http://192.168.0.1/)ve ' ye gidin. Aşağıdaki web sayfası görüntülenir:

    ![Config UI](media/howto-connect-devkit/config-ui.png)

    Web sayfasına girin:

    - WiFi ağınızın (SSID) adı.
    - WiFi ağ şifreniz.
    - Bağlantı ayrıntıları: **Aygıt Kimliği,** **Kimlik Kapsamı**ve Daha önce not aldığınız **SAS Primary Key'i** girin.

    > [!NOTE]
    > Şu anda, IoT DevKit sadece 2.4 GHz Wi-Fi bağlanabilir, 5 GHz donanım kısıtlamaları nedeniyle desteklenmez.

1. **Aygıtı Yapılandırmayı**seçin, DevKit aygıtı uygulamayı yeniden başlatır ve çalıştırın:

    ![Yeniden Başlatma UI](media/howto-connect-devkit/reboot-ui.png)

    DevKit ekranı, uygulamanın çalıştığını niçin onayladığını gösterir:

    ![DevKit çalışıyor](media/howto-connect-devkit/devkit-running.png)

DevKit önce IoT Central uygulamasına yeni bir aygıt kaydeder ve ardından veri göndermeye başlar.

## <a name="view-the-telemetry"></a>Telemetriyi görüntüle

Bu adımda, Azure IoT Central uygulamanızda telemetriyi görüntüleyebilirsiniz.

IoT Central **uygulamanızda, Cihazlar** sekmesini seçin, eklediğiniz aygıtı seçin. Genel **Bakış** sekmesinde, DevKit aygıtından telemetriyi görebilirsiniz:

![IoT Merkezi cihaza genel bakış](media/howto-connect-devkit/mxchip-overview-page.png)

## <a name="review-the-code"></a>Kodu gözden geçirin

Kodu gözden geçirmek veya değiştirmek ve derlemek için [Kod Örnekleri'ne](https://docs.microsoft.com/samples/azure-samples/mxchip-iot-devkit-pnp/sample/)gidin.

## <a name="next-steps"></a>Sonraki adımlar

Aygıt geliştiricisiyseniz, önerilen bazı sonraki adımlar şunlardır:

- Azure [IoT Central'da Aygıt bağlantısı](./concepts-get-connected.md) hakkında bilgi edinin
- [Azure CLI'yi kullanarak aygıt bağlantısını](./howto-monitor-devices-azure-cli.md) nasıl izleyeceğinizi öğrenin
