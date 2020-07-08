---
title: DevKit cihazını Azure IoT Central uygulamanıza bağlama | Microsoft Docs
description: Bir cihaz geliştiricisi olarak, IoT Tak ve Kullan (Önizleme) kullanarak bir Mxyonga IoT DevKit cihazını Azure IoT Central uygulamanıza bağlamayı öğrenin.
author: liydu
ms.author: liydu
ms.date: 12/03/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: jeffya
ms.openlocfilehash: bcf1dd2f89cf049d7da5b56170b2c13874c83ba4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81756796"
---
# <a name="connect-an-mxchip-iot-devkit-device-to-your-azure-iot-central-application"></a>Bir Mxyonga IoT DevKit cihazını Azure IoT Central uygulamanıza bağlama

*Bu makale, cihaz geliştiricileri için geçerlidir.*

Bu makalede bir Mxyonga IoT DevKit (DevKit) cihazının bir Azure IoT Central uygulamasına nasıl bağlanacağı gösterilmektedir. Cihaz, IoT Central bağlantısını yapılandırmak için DevKit cihazı için sertifikalı IoT Tak ve Kullan (Önizleme) modelini kullanır.

Bu nasıl yapılır makalesinde şunları yapabilirsiniz:

- IoT Central uygulamanızdan bağlantı ayrıntılarını alın.
- Cihazı hazırlayın ve IoT Central uygulamanıza bağlayın.
- IoT Central cihaz telemetri ve özelliklerini görüntüleyin.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdaki kaynaklar gereklidir:

- Bir [devkit cihazı](https://aka.ms/iot-devkit-purchase).
- Bir IoT Central uygulaması. [IoT Central uygulama oluşturma](./quick-deploy-iot-central.md)bölümündeki adımları izleyebilirsiniz.

## <a name="get-device-connection-details"></a>Cihaz bağlantısı ayrıntılarını al

1. Azure IoT Central uygulamanızda **cihaz şablonları** sekmesini seçin ve **+ Yeni**' yi seçin. **Önceden yapılandırılmış bir cihaz şablonu kullan**bölümünde **Mxyongaıot devkit**' i seçin.

    ![Mxyongaıot DevKit için cihaz şablonu](media/howto-connect-devkit/device-template.png)

1. Ileri 'yi seçin **: Özelleştir** ve sonra **Oluştur**.

1. **Cihazlar** sekmesini seçin. Cihazlar listesinde, **Mxyongaıot DevKit** ' i seçin ve şablondan yeni bir cihaz oluşturmak Için **+ Yeni** ' yi seçin.

    ![Yeni cihaz](media/howto-connect-devkit/new-device.png)

1. Açılır pencerede, **CIHAZ kimliğini** `SampleDevKit` ve **cihaz adını** olarak girin `MXChip IoT DevKit - Sample` . **Benzetimli** seçeneğinin kapalı olduğundan emin olun. Ardından **Oluştur**'u seçin.

    ![Cihaz KIMLIĞI ve adı](media/howto-connect-devkit/device-id-name.png)

1. Oluşturduğunuz cihazı seçin ve ardından **Bağlan**' ı seçin. **Kimlik kapsamını**, **cihaz kimliğini**ve **birincil anahtarı**bir yere unutmayın. Bu değerleri daha sonra bu nasıl yapılır makalesinde yapmanız gerekir.

    ![Cihaz bağlantı bilgileri](media/howto-connect-devkit/device-connection-info.png)

## <a name="prepare-the-device"></a>Cihazı hazırlama

1. GitHub 'dan DevKit cihazı için [önceden oluşturulmuş en son Azure IoT Central Tak ve kullan (Önizleme) bellenimini](https://github.com/Azure-Samples/mxchip-iot-devkit-pnp/raw/master/bin/iotc_devkit.bin) indirin.

1. DevKit cihazını bir USB kablosu kullanarak geliştirme makinenize bağlayın. Windows 'da, DevKit cihazında depolama ile eşleştirilmiş bir sürücüde dosya Gezgini penceresi açılır. Örneğin, sürücü **AZ3166 (D:)** olarak adlandırılabilir.

1. **İotc_devkit. bin** dosyasını sürücü penceresine sürükleyin. Kopyalama tamamlandığında, cihaz yeni üretici yazılımıyla yeniden başlatılır.

    > [!NOTE]
    > Ekranda **Wi-Fi**gibi hatalar görürseniz, bunun nedeni devkit 'In henüz WiFi 'ye bağlı olmaması olabilir.

1. DevKit üzerinde, **düğme b**tuşunu basılı tutarak **sıfırlama** düğmesini gönderin ve serbest bırakın ve sonra **B düğmesini**bırakın. Cihaz artık erişim noktası modunda. Onaylamak için, ekranda "IoT DevKit-AP" ve yapılandırma portalının IP adresi görüntülenir.

1. Bilgisayarınızda veya tabletinizde, cihazın ekranında gösterilen WiFi ağ adına bağlanın. WiFi ağı **az** ve ardından MAC adresi ile başlar. Bu ağa bağlandığınızda Internet erişiminiz yok demektir. Bu durum beklenir ve bu ağa yalnızca cihazı yapılandırırken kısa bir süre boyunca bağlanırsınız.

1. Web tarayıcınızı açın ve adresine gidin [http://192.168.0.1/](http://192.168.0.1/) . Aşağıdaki Web sayfası görüntülenir:

    ![Yapılandırma Kullanıcı arabirimi](media/howto-connect-devkit/config-ui.png)

    Web sayfasında, şunu girin:

    - WiFi ağınızın adı (SSID).
    - WiFi ağı parolanız.
    - Bağlantı ayrıntıları: daha önce bir nota yaptığınız **CIHAZ kimliği**, **kimlik kapsamı**ve **SAS birincil anahtarını** girin.

    > [!NOTE]
    > Şu anda IoT DevKit yalnızca 2,4 GHz Wi-Fi ' a bağlanabilir, donanım kısıtlamaları nedeniyle 5 GHz desteklenmez.

1. **Cihazı Yapılandır**' ı seçin, devkit cihazı yeniden başlatılır ve uygulamayı çalıştırır:

    ![Yeniden başlatma Kullanıcı arabirimi](media/howto-connect-devkit/reboot-ui.png)

    DevKit ekranında uygulamanın çalıştığı bir onay görüntülenir:

    ![DevKit çalışıyor](media/howto-connect-devkit/devkit-running.png)

DevKit öncelikle IoT Central uygulamasına yeni bir cihaz kaydeder ve sonra veri göndermeye başlar.

## <a name="view-the-telemetry"></a>Telemetriyi görüntüleme

Bu adımda, Azure IoT Central uygulamanızda Telemetriyi görüntüleyebilirsiniz.

IoT Central uygulamanızda, **cihazlar** sekmesini seçin, eklediğiniz cihazı seçin. **Genel bakış** sekmesinde, devkit cihazdan Telemetriyi görebilirsiniz:

![IoT Central cihaza genel bakış](media/howto-connect-devkit/mxchip-overview-page.png)

## <a name="review-the-code"></a>Kodu gözden geçirin

Kodu gözden geçirmek veya değiştirmek ve derlemek için [kod örneklerine](https://docs.microsoft.com/samples/azure-samples/mxchip-iot-devkit-pnp/sample/)gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bir cihaz geliştiricisiyseniz, önerilen bazı sonraki adımlar şunlardır:

- [Azure IoT Central cihaz bağlantısı](./concepts-get-connected.md) hakkında bilgi edinin
- [Azure CLI kullanarak cihaz bağlantısını izlemeyi](./howto-monitor-devices-azure-cli.md) öğrenin
