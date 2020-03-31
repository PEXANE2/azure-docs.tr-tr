---
title: MXChip IoT DevKit kullanıcı LED'ini kontrol etmek için Azure aygıt ikizlerini kullanın | Microsoft Dokümanlar
description: Bu eğitimde, DevKit durumlarını nasıl izleyeceğinizi ve Azure IoT Hub aygıt ikizleri ile kullanıcı LED'ini nasıl denetleyebilirsiniz öğrenin.
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/04/2018
ms.author: liydu
ms.openlocfilehash: deb1ea8c7b41ad48bddebfbed1b15c667ee0071a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73483941"
---
# <a name="mxchip-iot-devkit"></a>MXChip IoT DevKit

Bu örneği, MXChip IoT DevKit WiFi bilgilerini ve sensör durumlarını izlemek ve Azure IoT Hub aygıt ikizlerini kullanarak kullanıcı LED'inin rengini denetlemek için kullanabilirsiniz.

## <a name="what-you-learn"></a>Öğrenecekleriniz

- Nasıl MXChip IoT DevKit sensör durumları izlemek için.

- DevKit'in RGB LED'inin rengini kontrol etmek için Azure aygıt ikizleri nasıl kullanılır?

## <a name="what-you-need"></a>Ne gerekiyor

- [Başlangıç Kılavuzu'nu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started)izleyerek geliştirme ortamınızı ayarlayın.

- GitBash terminal pencerenizden (veya diğer Git komut satırı arabiriminden) aşağıdaki komutları yazın:

   ```bash
   git clone https://github.com/DevKitExamples/DevKitState.git
   cd DevKitState
   code .
   ```

## <a name="provision-azure-services"></a>Azure Hizmetlerini Sağlama

1. Visual Studio Code'da **Görevler** açılır menüsüne tıklayın ve **Görev Çalıştır'ı seçin...**  -  **bulut hükmü**.

2. İlerlemeniz **Karşılama** panelinin **TERMINAL** sekmesi altında görüntülenir.

3. İleti ile istendiğinde *hangi aboneliği seçmek istiyorsunuz,* bir abonelik seçin.

4. Bir kaynak grubu seçin veya seçin. 
 
   > [!NOTE]
   > Zaten ücretsiz bir IoT Hub'ınvarsa, bu adımı atlayabilirsiniz.

5. İleti ile *istendiğinde, bir IoT hub'ı seçmek istediğiniz ioT hub'ını seçin*veya oluşturun.

6. Fonksiyon uygulamasına benzer bir *şey: fonksiyon uygulama adı: xxx*, görüntülenir. İşlev uygulama adını yazın; daha sonraki bir adımda kullanılacaktır.

7. İleti *Kaynak Yöneticisi şablon dağıtımı: Bitti* görüntülenirken belirtilen Azure Kaynak Yöneticisi şablon dağıtımının tamamlanmasını bekleyin.

## <a name="deploy-function-app"></a>Fonksiyon Uygulamasını Dağıt

1. Visual Studio Code'da **Görevler** açılır menüsüne tıklayın ve **Görev Çalıştır'ı seçin...**  -  **bulut dağıtma.**

2. İşlev uygulama kodu yükleme işleminin tamamlanmasını bekleyin; mesaj *işlevi uygulaması dağıtır: Yapılır* görüntülenir.

## <a name="configure-iot-hub-device-connection-string-in-devkit"></a>DevKit'te IoT Hub Aygıt Bağlantı Dizelerini Yapılandırma

1. MXChip IoT DevKit'inizi bilgisayarınıza bağlayın.

2. Visual Studio Code'da **Görevler** açılır menüsüne tıklayın ve **Görev Çalıştır'ı seçin...**  -  **config-cihaz-bağlantı**

3. MXChip IoT DevKit'te **A**tuşuna basın ve basılı tutun, **Sıfırla** düğmesine basın ve DekKit'in yapılandırma moduna girmesini sağlamak için **A** düğmesini bırakın.

4. Bağlantı dize yapılandırma işleminin tamamlanmasını bekleyin.

## <a name="upload-arduino-code-to-devkit"></a>Arduino Kodunu DevKit'e Yükleyin

Bilgisayarınıza bağlı MXChip IoT DevKit'iniz ile:

1. Visual Studio Code'da **Görevler** açılır menüsüne tıklayın ve **Çalıştır Yap Görev'i seçin...** Arduino çizimi derlenir ve DevKit'e yüklenir.

2. Çizim başarıyla yüklendiğinde, Bir *Yapı & Yükleme Çizimi: başarı* iletisi görüntülenir.

## <a name="monitor-devkit-state-in-browser"></a>DevKit Durumunu Tarayıcıda İzleyin

1. Web tarayıcısında, gereksinim `DevKitState\web\index.html` duyduğunuz adım sırasında oluşturulan dosyayı açın.

2. Aşağıdaki Web sayfası görüntülenir:![İşlev uygulama adını belirtin.](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state-function-app-name.png)

3. Daha önce yazdığınız işlev uygulama adını girdin.

4. **Bağlan** düğmesini tıklatın

5. Birkaç saniye içinde sayfa, DevKit'in WiFi bağlantı durumunu ve yerleşik sensörlerin her birinin durumunu yeniler ve görüntüler.

## <a name="control-the-devkits-user-led"></a>DevKit'in Kullanıcı LED'ini kontrol edin

1. Web sayfası çizimindeki kullanıcı LED grafiğini tıklatın.

2. Birkaç saniye içinde ekran yenilenir ve kullanıcı LED'inin geçerli renk durumunu gösterir.

3. RGB kaydırıcı denetimleri çeşitli yerlerde tıklayarak RGB LED renk değerini değiştirmeyi deneyin.

## <a name="example-operation"></a>Örnek işlem

![Örnek test yordamı](media/iot-hub-arduino-iot-devkit-az3166-devkit-state/devkit-state.gif)

> [!NOTE]
> Aygıt ikizinin ham verilerini Azure portalında görebilirsiniz: IoT Hub -\> IoT cihazları -\> * \<cihazınız\> *  - \> Aygıt İkiz.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl yapılacağını öğrendiniz:
- Bir MXChip IoT DevKit cihazını Azure IoT Uzaktan İzleme çözüm hızlandırıcınıza bağlayın.
- DevKit'in RGB LED'inin rengini algılamak ve kontrol etmek için Azure IoT aygıt ikizleri işlevini kullanın.

Önerilen sonraki adımlar şunlardır:

* [Azure IoT Uzaktan İzleme çözüm hızlandırıcıya genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Bir MXChip IoT DevKit cihazını Azure IoT Merkezi uygulamanıza bağlayın](/azure/iot-central/core/howto-connect-devkit)
