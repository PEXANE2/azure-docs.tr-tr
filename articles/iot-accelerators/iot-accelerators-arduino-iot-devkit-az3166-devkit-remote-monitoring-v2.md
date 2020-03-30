---
title: IoT DevKit'i Uzaktan İzleme çözümüne bağlayın - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılabilir kılavuzunda, IoT DevKit AZ3166 cihazındaki sensörlerden telemetriyi izleme ve görselleştirme için Uzaktan İzleme çözüm hızlandırıcısına nasıl göndereceğinizi öğrenirsiniz.
author: isabelcabezasm
manager: ''
ms.service: iot-accelerators
services: iot-accelerators
ms.devlang: c
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: isacabe
ms.openlocfilehash: 6e9f9c89cf2e5e40d37a1532e688490aae294181
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73888858"
---
# <a name="connect-an-iot-devkit-device-to-the-remote-monitoring-solution-accelerator"></a>Bir IoT DevKit cihazını Uzaktan İzleme çözüm hızlandırıcısına bağlayın

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu nasıl yapıl› kılavuz, IoT DevKit aygıtınızda örnek bir uygulamayı nasıl çalıştırabileceğinizi gösterir. Örnek kod, DevKit cihazındaki sensörlerden çözüm hızlandırıcınıza telemetri gönderir.

[MXChip IoT DevKit,](https://aka.ms/iot-devkit) zengin çevre birimleri ve sensörlere sahip hepsi bir aide Arduino uyumlu bir tahtadır. Visual Studio Code'daki [Azure IoT Aygıt Çalışma Tezgahı](https://aka.ms/iot-workbench) veya [Azure IoT Araçları](https://aka.ms/azure-iot-tools) uzantı paketini kullanarak bunun için geliştirebilirsiniz. [Proje kataloğu,](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) IoT çözümlerinin prototipini denemenize yardımcı olacak örnek uygulamalar içerir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticideki adımları tamamlamak için önce aşağıdaki görevleri yapın:

* [IoT DevKit AZ3166'daki adımları buluttaki Azure IoT Hub'ına](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started)izleyerek DevKit'inizi hazırlayın.

## <a name="open-sample-project"></a>Açık örnek proje

UZAKTAN İzleme örneğini VS Kodu'nda açmak için:

1. IoT DevKit'inizin bilgisayarınıza gitmediğinden emin olun. Önce VS Kodu'nu başlatın ve ardından DevKit'i bilgisayarınıza bağlayın.

1. Komut `F1` paletini açmak, Azure **IoT Aygıt Çalışma Tezgahı**yazın ve seçin: Örnekleri Aç... seçeneğini tıklatın. Ardından tahta olarak **IoT DevKit'i** seçin.

1. **Uzaktan İzleme'yi** bulun ve **Örnek Aç'ı**tıklatın. Proje klasörünü gösteren yeni bir VS Kodu penceresi açılır:

   ![IoT Workbench, Uzaktan İzleme örneğini seçin](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-example.png)

## <a name="configure-the-device"></a>Aygıtı yapılandırma

DevKit aygıtınızda IoT Hub aygıt bağlantı dizesini yapılandırmak için:

1. IoT DevKit'i **Yapılandırma moduna**geçin:

    * **A**düğmesini basılı tutun.
    * **Sıfırla** düğmesine basın ve bırakın.

1. Ekran DevKit Kimliğini görüntüler `Configuration`ve .

    ![IoT DevKit Yapılandırma Modu](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/devkit-configuration-mode.png)

1. Komut paletini açmak için **F1** tuşuna basın, Azure IoT Aygıt Çalışma Tezgahı yazın ve **seçin: Aygıt Ayarlarını Yapılandır... > Config Aygıt Bağlantı Dizesi.**

1. Daha önce kopyaladığınız bağlantı dizesini yapıştırın ve aygıtı yapılandırmak için **Enter** tuşuna basın.

## <a name="build-the-code"></a>Kodu oluşturma

Aygıt kodunu oluşturmak ve yüklemek için:

1. Komut `F1` paletini açmak için basın, **Azure IoT Aygıt Çalışma Tezgahı**yazın ve seçin: Aygıt Kodunu Yükleyin:

1. VS Code kodu DevKit cihazınıza derler ve yükler:

    ![IoT Çalışma Tezgahı: Cihaz - > Yüklendi](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-workbench-device-uploaded.png)

1. DevKit aygıtı yüklediğiniz kodu yeniden başlatır ve çalıştırın.

## <a name="test-the-sample"></a>Örneği test edin

DevKit cihazına yüklediğiniz örnek uygulamanın çalıştığını doğrulamak için aşağıdaki adımları tamamlayın:

### <a name="view-the-telemetry-sent-to-remote-monitoring-solution"></a>Uzaktan İzleme çözümüne gönderilen telemetriyi görüntüleme

Örnek uygulama çalıştığında, DevKit cihazı sensör verilerinden Wi-Fi üzerinden çözüm hızlandırıcınıza telemetri gönderir. Telemetriyi görmek için:

1. Çözüm panonuza gidin ve **Aygıt Gezgini'ni**tıklatın.

1. DevKit cihazınızın cihaz adını tıklayın. sağ sekmesinde, DevKit'ten telemetriyi gerçek zamanlı olarak görebilirsiniz:

    ![Azure IoT Paketi'ndeki sensör verileri](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-dashboard.png)

### <a name="control-the-devkit-device"></a>DevKit cihazını kontrol edin

Uzaktan İzleme çözüm hızlandırıcısı cihazınızı uzaktan kontrol etmenizi sağlar. Örnek kod, **Aygıt Gezgini** sayfasında aygıtı seçtiğinizde **Yöntem** bölümünde görebileceğiniz üç yöntem uygular:

![IoT DevKit Yöntemleri](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-methods.png)

DevKit LED'lerden birinin rengini değiştirmek için **LedColor** yöntemini kullanın:

1. Cihaz listesinden aygıt adını seçin ve **İşler'e**tıklayın:

    ![İş Oluşturma](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-job.png)

1. İşleri aşağıdaki değerleri kullanarak yapılandırın ve **Uygula'yı**tıklatın:

   * İş'i seçin: **Çalıştırma yöntemi**
   * Yöntem adı: **LedColor**
   * İş Adı: **ChangeLedColor**

     ![İş ayarları](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/iot-suite-change-color.png)

1. Birkaç saniye sonra, DevKit'inizdeki RGB LED'inin rengi (A düğmesinin altında) değişir:

    ![IoT DevKit kırmızı led](media/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoringv2/azure-iot-suite-devkit-led.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticilere devam etmeyi planlıyorsanız Uzaktan İzleme çözümü hızlandırıcısı dağıtımını bırakın.

Çözüm hızlandırıcısına artık ihtiyacınız yoksa, çözüm seçerek ve Ardından Çözüm Sil'i tıklatarak, Verilen Çözümler sayfasından silin:

![Çözümü sil](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Herhangi bir sorunla karşılaştıysanız, [IoT DevKit SSS'lerine](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) bakın veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bir DevKit cihazını Uzaktan İzleme çözüm hızlandırıcınıza nasıl bağlayabileceğinizi öğrendiğinize göre, önerilen sonraki bazı adımlar şunlardır:

* [Azure IoT çözüm hızlandırıcılarına genel bakış](https://docs.microsoft.com/azure/iot-accelerators/)
* [Kullanıcı arabirimini özelleştirme](iot-accelerators-remote-monitoring-customize.md)
* [IoT DevKit'i Azure IoT Merkezi uygulamanıza bağlayın](../iot-central/core/howto-connect-devkit.md)