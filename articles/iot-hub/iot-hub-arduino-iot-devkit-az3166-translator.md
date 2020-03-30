---
title: Çevirmen w/Azure Fonksiyonları, Bilişsel Hizmetler, IoT DevKit yapın
description: Sesli mesaj almak için IoT DevKit'teki mikrofonu kullanın ve ardından bunları İngilizce'de çevrilmiş metne işlemek için Azure Bilişsel Hizmetleri'ni kullanın
author: liydu
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 12/19/2018
ms.author: liydu
ms.openlocfilehash: 8e840a1ae7161ea3e7b370889a1f0fb648ca120e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73953350"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>IoT DevKit AZ3166'yı Azure Fonksiyonları ve Bilişsel Hizmetler ile kullanarak dil çevirmeni yapmayı kullanın

Bu makalede, [Azure Bilişsel Hizmetleri'ni](https://azure.microsoft.com/services/cognitive-services/)kullanarak IoT DevKit'i dil çevirmeni olarak nasıl yapacağınızı öğreneceksiniz. Sesinizi kaydeder ve DevKit ekranında gösterilen İngilizce metne çevirir.

[MXChip IoT DevKit,](https://aka.ms/iot-devkit) zengin çevre birimleri ve sensörlere sahip hepsi bir aide Arduino uyumlu bir tahtadır. Visual Studio Code'daki [Azure IoT Aygıt Çalışma Tezgahı](https://aka.ms/iot-workbench) veya [Azure IoT Araçları](https://aka.ms/azure-iot-tools) uzantı paketini kullanarak bunun için geliştirebilirsiniz. [Proje kataloğu,](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) IoT çözümlerinin prototipini denemenize yardımcı olacak örnek uygulamalar içerir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticideki adımları tamamlamak için önce aşağıdaki görevleri yapın:

* [IoT DevKit AZ3166'daki adımları buluttaki Azure IoT Hub'ına](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started)izleyerek DevKit'inizi hazırlayın.

## <a name="create-azure-cognitive-service"></a>Azure Bilişsel Hizmet Oluşturma

1. Azure portalında kaynak **oluştur'u** ve **Konuşma**aramasını tıklatın. Konuşma Hizmeti oluşturmak için formu doldurun.
  ![Konuşma hizmeti](media/iot-hub-arduino-iot-devkit-az3166-translator/speech-service.png)

1. Az önce oluşturduğunuz Konuşma hizmetine gidin, DevKit'e erişen **Key1'i** kopyalamak ve not almak için **Tuşlar** bölümüne tıklayın.
  ![Kopya anahtarları](media/iot-hub-arduino-iot-devkit-az3166-translator/copy-keys.png)

## <a name="open-sample-project"></a>Açık örnek proje

1. IoT DevKit'inizin bilgisayarınıza **bağlı olmadığından** emin olun. Önce VS Kodu'nu başlatın ve ardından DevKit'i bilgisayarınıza bağlayın.

1. Komut `F1` paletini açmak, Azure **IoT Aygıt Çalışma Tezgahı**yazın ve seçin: Örnekleri Aç... seçeneğini tıklatın. Ardından tahta olarak **IoT DevKit'i** seçin.

1. IoT Çalışma Tezgahı Örnekleri sayfasında **DevKit Translator'ı** bulun ve **Örnek Aç'ı**tıklatın. Ardından örnek kodu indirmek için varsayılan yolu seçer.
  ![Açık örnek](media/iot-hub-arduino-iot-devkit-az3166-translator/open-sample.png)

## <a name="use-speech-service-with-azure-functions"></a>Konuşma Hizmetini Azure İşleriyle Kullanma

1. VS Kodu'nda `F1`Azure IoT Aygıt Çalışma Tezgahı yazın ve **seçin: Azure Hizmetlerini Sağlama...'ı**tıklatın. ![Azure hizmetlerini sağlama](media/iot-hub-arduino-iot-devkit-az3166-translator/provision.png)

1. Azure IoT Hub ve Azure İşlevlerini sağlamayı bitirmek için gereken adımları izleyin.
   ![Sağlama adımları](media/iot-hub-arduino-iot-devkit-az3166-translator/provision-steps.png)

   Oluşturduğunuz Azure IoT Hub aygıt adını not alın.

1. Aşağıdaki `Functions\DevKitTranslatorFunction.cs` kod satırlarını, belirttiğiniz aygıt adı ve Konuşma Hizmeti anahtarıyla açın ve güncelleyin.
   ```csharp
   // Subscription Key of Speech Service
   const string speechSubscriptionKey = "";

   // Region of the speech service, see https://docs.microsoft.com/azure/cognitive-services/speech-service/regions for more details.
   const string speechServiceRegion = "";

   // Device ID
   const string deviceName = "";
   ```

1. Azure `F1`IoT Aygıt Çalışma Tezgahı yazın ve **seçin: Azure'a dağıt...** seçeneğini tıklatın. VS Kodu yeniden dağıtım için onay **isterse, Evet'i**tıklatın.
   ![Uyarı dağıtma](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-warning.png)

1. Dağıtımın başarılı olduğundan emin olun.
   ![Başarıyı dağıtın](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-success.png)

1. Azure portalında, **Fonksiyonlar Uygulamaları** bölümüne gidin ve yeni oluşturulan Azure İşlevi uygulamasını bulun. Url'yi kopyalamak için `devkit_translator` **</> İşlev URL'sini** bul'u tıklatın.
   ![Kopyalama işlevi URL'si](media/iot-hub-arduino-iot-devkit-az3166-translator/get-function-url.png)

1. URL'yi dosyaya `azure_config.h` yapıştırın.
   ![Azure config](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-config.png)

   > [!NOTE]
   > İşlev uygulaması düzgün çalışmıyorsa, çözmek için bu [SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function) bölümünü kontrol edin.

## <a name="build-and-upload-device-code"></a>Aygıt kodu oluşturma ve yükleme

1. DevKit'i **yapılandırma moduna** şu şekilde geçirin:
   * **A**düğmesini basılı tutun.
   * **Sıfırla** düğmesine basın ve bırakın.

   Ekranın DevKit Kimliğini ve **Yapılandırmasını**görüntülediğini göreceksiniz.

   ![DevKit yapılandırma modu](media/iot-hub-arduino-iot-devkit-az3166-translator/devkit-configuration-mode.png)

1. Azure `F1`IoT Aygıt Çalışma Tezgahı yazın ve **seçin: Aygıt Ayarlarını Yapılandır... > Config Device Bağlantı Dizesi'ni**tıklatın. DevKit'e yapılandırmak için **IoT Hub Aygıt Bağlantı Dizesini seçin.**
   ![Bağlantı dizelerini yapılandırma](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string.png)

1. Bildirim başarılı bir şekilde yapıldıktan sonra bildirimi görürsünüz.
   ![Bağlantı dize başarısını yapılandırma](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string-success.png)

1. Yeniden `F1` tıklatın, **Azure IoT Aygıt Çalışma Tezgahı**yazın ve seçin: Aygıt Kodunu Yükle. Kodu derlemeye ve DevKit'e yüklemeye başlar.
   ![Cihaz yükleme](media/iot-hub-arduino-iot-devkit-az3166-translator/device-upload.png)

## <a name="test-the-project"></a>Projeyi test edin

Uygulama nın başlatılmasından sonra DevKit ekranındaki yönergeleri izleyin. Varsayılan kaynak dili Çincedir.

Çeviri için başka bir dil seçmek için:

1. Kurulum moduna girmek için A düğmesine basın.

2. Desteklenen tüm kaynak dilleri kaydırmak için B düğmesine basın.

3. Kaynak dil seçiminizi onaylamak için A düğmesine basın.

4. Konuşurken B tuşuna basın ve basılı tutun, ardından çeviriyi başlatmak için B düğmesini bırakın.

5. İngilizce olarak çevrilen metin ekranda gösterir.

![Seçili dile kaydırma](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Çeviri sonucu](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

Çeviri sonuç ekranında şunları yapabilirsiniz:

- Kaydırma krip kaynak dili seçmek için A ve B tuşlarına basın.

- Konuşmak için B düğmesine basın. Sesi göndermek ve çeviri metnini almak için B düğmesini bırakın.

## <a name="how-it-works"></a>Nasıl çalışır?

![mini-çözüm-ses-tweet-diyagramı](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

IoT DevKit sesinizi kaydeder ve Azure İşlevlerini tetiklemek için bir HTTP isteği gönderir. Azure Fonksiyonları, çeviri yapmak için bilişsel hizmet konuşma çevirmeni API'yi çağırır. Azure İşlevler çeviri metnini aldıktan sonra aygıta c2D iletisi gönderir. Daha sonra çeviri ekranda görüntülenir.

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunlarla karşılaşırsanız, [IoT DevKit SSS'ye](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) bakın veya aşağıdaki kanalları kullanarak bize ulaşın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Azure İşlevleri ve Bilişsel Hizmetler'i kullanarak IoT DevKit'i çevirmen olarak kullanmayı öğrendiniz. Bu nasıl-to-to, nasıl öğrendim:

> [!div class="checklist"]
> * Bulut hükümlerini otomatikleştirmek için Visual Studio Code görevini kullanma
> * Azure IoT aygıt bağlantı dizelerini yapılandırma
> * Azure İşi'ni dağıtma
> * Sesli mesaj çevirisini test edin

Öğrenmek için diğer öğreticilere ilerleyin:

> [!div class="nextstepaction"]
> [IoT DevKit AZ3166'yı Azure IoT Uzaktan İzleme çözüm hızlandırıcısına bağlayın](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring)
