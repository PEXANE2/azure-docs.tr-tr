---
title: MXChip IoT DevKit'i Azure IoT Hub Uzaktan İzleme'ye bağlayın
description: Bu eğitimde, IoT DevKit AZ3166'daki sensörlerin durumunu Azure IoT Uzaktan İzleme çözüm hızlandırıcısına nasıl göndereceğinizi öğrenin.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 02/02/2018
ms.author: liydu
ms.openlocfilehash: 598e361949b000724645c841910b1682a7bbb1a3
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258465"
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-remote-monitoring-solution-accelerator"></a>MXChip IoT DevKit'i Azure IoT Uzaktan İzleme çözüm hızlandırıcısına bağlayın

Bu eğitimde, Azure IoT Uzaktan İzleme çözüm hızlandırıcınıza sensör verileri göndermek için DevKit'inizde örnek bir uygulamayı nasıl çalıştırabileceğinizi öğrenirsiniz.

[MXChip IoT DevKit,](https://aka.ms/iot-devkit) zengin çevre birimleri ve sensörlere sahip hepsi bir aide Arduino uyumlu bir tahtadır. Bunun için [Arduino için Visual Studio Code uzantısı](https://aka.ms/arduino)kullanarak geliştirebilirsiniz. Microsoft Azure hizmetlerinden yararlanan Nesnelerin İnterneti (IoT) çözümlerinin prototipine rehberlik eden büyüyen bir [proje kataloğu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) yla birlikte gelir.

## <a name="what-you-need"></a>Ne gerekiyor

[Başlarken Kılavuzunu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) bitirin:

* DevKit'inizin Wi-Fi'a bağlanmasını sağlar
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Eğer bir tane yoksa, şu iki yöntemden biri ile kaydolabilirsiniz:

* Ücretsiz [30 günlük deneme Sürümünü Microsoft Azure hesabını](https://azure.microsoft.com/free/) etkinleştirme

* MSDN veya Visual Studio abonesiyseniz [Azure kredinizi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) talep edin

## <a name="create-an-azure-iot-remote-monitoring-solution-accelerator"></a>Azure IoT Uzaktan İzleme çözüm hızlandırıcısı oluşturma

1. Azure [IoT çözüm hızlandırıcıları sitesine](https://www.azureiotsolutions.com/) gidin ve **yeni bir çözüm oluştur'u**tıklatın.

   ![Azure IoT çözüm hızlandırıcı türünü seçin](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-solution-types.png)

   > [!WARNING]
   > Varsayılan olarak, bu örnek bir IoT Uzaktan İzleme çözüm hızlandırıcısı oluşturduktan sonra bir S2 IoT Hub'ı oluşturur. Bu IoT hub'ı çok sayıda aygıtla birlikte kullanılmamışsa, s2'den S1'e düşürmenizi ve ilgili IoT Hub'ının artık ihtiyacınız olmadığında silinmesi için IoT Uzaktan İzleme çözüm hızlandırıcısını silmenizi öneririz. 

2. **Uzaktan izleme'yi**seçin.

3. Bir çözüm adı girin, bir abonelik ve bölge seçin ve ardından **çözüm oluştur'u**tıklatın. Çözümün sağlanması biraz zaman alabilir.
  
   ![Çözüm oluşturma](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution.png)

4. Bitişleri sağlamadan sonra **Başlat'ı**tıklatın. Bazı simüle aygıtlar sağlama işlemi sırasında çözüm için oluşturulur. Bunları kontrol etmek için **AYGıTLAR'ı** tıklatın.

   ![Pano](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-solution-created.png)
  
   ![Konsol](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-console.png)

5. **ALET EKLE'yi**tıklatın.

6. **Özel Aygıt**için **Yeni Ekle'yi** tıklatın.
  
   ![Yeni cihaz ekleme](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-add-new-device.png)

7. **Kendi Aygıt Kimliğimi tanımlayayım, gireyim'** `AZ3166`i ve sonra **Oluştur'u**tıklatın.
  
   ![Kimlikle aygıt oluşturma](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/azure-iot-suite-new-device-configuration.png)

8. **IoT Hub Hostname'ye**not yapın ve **Bitti'yi**tıklatın.

## <a name="open-the-remotemonitoring-sample"></a>Uzaktan İzleme örneğini açma

1. Bağlıysa DevKit'in bilgisayarınızdan bağlantısını kesin.

2. VS Code'u başlatın.

3. DevKit'i bilgisayarınıza bağlayın. VS Code DevKit'inizi otomatik olarak algılar ve aşağıdaki sayfaları açar:

   * DevKit giriş sayfası.
   * Arduino Örnekleri: DevKit ile başlamak için uygulamalı örnekler.

4. Sol **taraftaki ARDUINO ÖRNEKLERİ** bölümünü genişletin, **AzureIoT'> MXCHIP AZ3166 örneklerine**göz atın ve **RemoteMonitoring'i**seçin. Içinde proje klasörü bulunan yeni bir VS Kodu penceresi açar.

   > [!NOTE]
   > Bölmeyi kapatırsanız, yeniden açabilirsiniz. Komut `Ctrl+Shift+P` paletini `Cmd+Shift+P`açmak için (macOS: ) kullanın, **Arduino**yazın ve ardından **Arduino'yu**bulup seçin: Örnekler .

## <a name="provision-required-azure-services"></a>Gerekli Azure hizmetlerinin sağlanması

Çözüm penceresinde, sağlanan metin `Ctrl+P` kutusuna girerek `Cmd+P` `task cloud-provision` görevinizi (macOS: ) çalıştırın.

VS Code terminalinde, etkileşimli komut satırı, gerekli Azure hizmetlerini sağlama konusunda size yol sunar.

![Azure kaynaklarını sağlama](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/provision.png)

## <a name="build-and-upload-the-device-code"></a>Aygıt kodunu oluşturma ve yükleme

1. Kullanın `Ctrl+P` (macOS: `Cmd + P`) ve tür **görev config-device-connection**.

2. Terminal, adımdan aldığı bir bağlantı dizesini kullanmak `task cloud-provision` isteyip istemediğinizi sorar. Ayrıca 'Yeni Oluştur...' seçeneğini tıklayarak kendi cihaz bağlantı dizenizi de girdiniz.

3. Terminal, yapılandırma moduna girmenizi ister. Bunu yapmak için A düğmesini basılı tutun, ardından sıfırlama düğmesine basın ve bırakın. Ekranda DevKit Kimliği ve 'Yapılandırma' görüntülenir.

   ![Giriş bağlantı dizesi](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/config-device-connection.png)

4. Bittikten sonra `task config-device-connection` `F1` VS Kodu komutlarını yüklemek `Arduino: Upload`için tıklatın ve seçin. VS Code, Arduino çizimini doğrulamaya ve yüklemeye başlar.
  
   ![Arduino çiziminin doğrulanması ve yüklenmesi](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/arduino-upload.png)

DevKit yeniden başlatılır ve kodu çalıştırmaya başlar.

## <a name="test-the-project"></a>Projeyi test edin

Örnek uygulama çalıştığında DevKit, Wi-Fi üzerinden sensör verilerini Azure IoT Uzaktan İzleme çözüm hızlandırıcınıza gönderir. Sonucu görmek için aşağıdaki adımları izleyin:

1. Azure IoT Uzaktan İzleme çözüm hızlandırıcınıza gidin ve **PANO'ya**tıklayın.

2. Uzaktan İzleme çözüm konsolunda DevKit sensör durumunuzu göreceksiniz.

   ![Azure IoT Uzaktan İzleme çözüm hızlandırıcısındaki sensör verileri](media/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring/sensor-status.png)

## <a name="change-device-id"></a>Aygıt kimliğini değiştirme

Koddaki özelleştirilmiş bir aygıt kimliğiyle kodkodlu **AZ3166'yı** değiştirmek istiyorsanız, uzaktan [izleme örneğinde](../iot-accelerators/iot-accelerators-arduino-iot-devkit-az3166-devkit-remote-monitoring-v2.md)görüntülenen kod satırını değiştirin.

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunlarla karşılaşırsanız, [IoT geliştirici kiti SSS'ye](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) bakın veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bir DevKit cihazını Azure IoT Uzaktan İzleme çözüm hızlandırıcınıza nasıl bağlayıp sensör verilerini görselleştireceğimize göre, önerilen sonraki adımlar şunlardır:

* [Azure IoT çözüm hızlandırıcılarına genel bakış](https://docs.microsoft.com/azure/iot-suite/)

* [Bir MXChip IoT DevKit cihazını Azure IoT Merkezi uygulamanıza bağlayın](/azure/iot-central/core/howto-connect-devkit)

* [IoT geliştirici kiti](https://microsoft.github.io/azure-iot-developer-kit/) 
