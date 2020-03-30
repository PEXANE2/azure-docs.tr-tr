---
title: Azure İşlevleri ile bir Twitter iletisi alın | Microsoft Dokümanlar
description: Titremeyi algılamak için hareket sensörünü kullanın ve belirttiğiniz bir hashtag ile rastgele bir tweet bulmak için Azure Fonksiyonlarını kullanın
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/07/2018
ms.author: liydu
ms.openlocfilehash: dc4ff35ff04680e8635d54c25212c8ae639ae472
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60779908"
---
# <a name="shake-shake-for-a-tweet----retrieve-a-twitter-message-with-azure-functions"></a>Bir Tweet için Salla, Salla -- Azure İşleriyle Twitter mesajı alın

Bu projede, Azure İşlevlerini kullanarak bir olayı tetiklemek için hareket sensörünü nasıl kullanacağınızı öğrenirsiniz. Uygulama, Arduino çiziminizde yapılandırdığınız #hashtag rastgele bir tweet alır. Tweet DevKit ekranında görüntülenir.

## <a name="what-you-need"></a>Ne gerekiyor

[Başlarken Kılavuzunu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) bitirin:

* DevKit'inizin Wi-Fi'a bağlanmasını sağlar.
* Geliştirme ortamını hazırlayın.

Etkin bir Azure aboneliği. Eğer yoksa, şu yöntemlerden biri ile kaydolabilirsiniz:

* Ücretsiz [30 günlük deneme Sürümünü Microsoft Azure hesabını](https://azure.microsoft.com/free/) etkinleştirme
* MSDN veya Visual Studio abonesiyseniz [Azure kredinizi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) talep edin

## <a name="open-the-project-folder"></a>Proje klasörünü açma

Proje klasörünü açarak başlayın. 

### <a name="start-vs-code"></a>VS Kodunu Başlat

* DevKit'inizin bilgisayarınıza bağlı olduğundan emin olun.

* VS Code'u başlatın.

* DevKit'i bilgisayarınıza bağlayın.

   > [!NOTE]
   > VS Kodu başlatırken, Arduino IDE veya ilgili yönetim kurulu paketinin bulunamayan bir hata iletisi alabilirsiniz. Bu hata oluşursa, VS Kodunu kapatın ve Arduino IDE'yi yeniden başlatın. VS Kodu artık Arduino IDE yolunu doğru bir şekilde bulmalı.

### <a name="open-the-arduino-examples-folder"></a>Arduino Örnekleri klasörünü açma

Sol **taraftaki ARDUINO ÖRNEKLERİ** bölümünü genişletin, **MXCHIP AZ3166 > AzureIoT örneklerine**göz atın ve **ShakeShake'i**seçin. Proje klasörünü görüntüleyen yeni bir VS Kodu penceresi açılır. MXCHIP AZ3166 bölümünü göremiyorsanız, cihazınızın düzgün bir şekilde bağlı olduğundan emin olun ve Visual Studio Code'u yeniden başlatın.  
![mini çözüm örnekleri](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_examples.png)

Örnek projeyi komut paletinden de açabilirsiniz. Komut `Ctrl+Shift+P` paletini `Cmd+Shift+P`açmak için (macOS: ) tıklatın, **Arduino**yazın ve ardından **Arduino'yu**bulup seçin: Örnekler .

## <a name="provision-azure-services"></a>Azure hizmetlerini sağlama

Çözüm penceresinde, (macOS: `Ctrl+P` `Cmd+P`) üzerinden . `task cloud-provision`

VS Code terminalinde, etkileşimli bir komut satırı gerekli Azure hizmetlerini sağlama konusunda size yol göstermektedir:

![bulut hükmü](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-provision.png)

> [!NOTE]
> Azure'da oturum açmaya çalışırken sayfa yükleme durumunda [duruyorsa, IoT DevKit SSS'deki "giriş sayfası askıda" adımına](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure)bakın.
 
## <a name="modify-the-hashtag"></a>#hashtag değiştirin

Açın `ShakeShake.ino` ve bu kod satırına bakın:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

Kıvırcık `iot` ayraçların içindeki dizeyi tercih ettiğiniz hashtag ile değiştirin. DevKit daha sonra bu adımda belirttiğiniz hashtag'i içeren rastgele bir tweet alır.

## <a name="deploy-azure-functions"></a>Azure İşlevleri’ni dağıtma

Azure `Ctrl+P` İşlevler `Cmd+P`kodunu `task cloud-deploy` dağıtmaya başlamak için çalıştırmak için (macOS: ) kullanın:

![bulut dağıtma](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-deploy.png)

> [!NOTE]
> Bazen Azure İşlevi düzgün çalışmayabilir. Bu sorunu oluştuğunda gidermek [için, IoT DevKit SSS'nin "derleme hatası" bölümünü](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#compilation-error-for-azure-function)kontrol edin.

## <a name="build-and-upload-the-device-code"></a>Aygıt kodunu oluşturma ve yükleme

Ardından, aygıt kodunu oluşturun ve yükleyin.

### <a name="windows"></a>Windows

1. Çalıştırmak `Ctrl+P` `task device-upload`için kullanın.

2. Terminal, yapılandırma moduna girmenizi ister. Bunu yapmak için:

   * Basılı düğme A

   * Sıfırlama düğmesine basın ve bırakın.

3. Ekranda DevKit Kimliği ve 'Yapılandırma' görüntülenir.

### <a name="macos"></a>macOS

1. DevKit'i yapılandırma moduna koyun:

   A düğmesini basılı tutun, ardından sıfırlama düğmesine basın ve bırakın. Ekranda 'Yapılandırma' görüntülenir.

2. Adımdan `task device-upload` alınan bağlantı dizesini ayarlamak için `task cloud-provision` çalıştırmayı kullanın. `Cmd+P`

### <a name="verify-upload-and-run"></a>Doğrula, yükle ve çalıştır

Şimdi bağlantı dizesi ayarlanır, uygulamayı doğrular ve yükler, sonra çalıştırır. 

1. VS Code, Arduino çizimini doğrulamaya ve DevKit'inize yüklemeye başlar:

   ![cihaz yükleme](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)

2. DevKit yeniden başlatılır ve kodu çalıştırmaya başlar.

"Hata: AZ3166: Bilinmeyen paket" hata iletisi alabilirsiniz. Bu hata, yönetim kurulu paket dizini doğru şekilde yenilenmediğinde oluşur. Bu sorunu gidermek [için, IoT DevKit SSS'deki "bilinmeyen paket" hatasını kontrol edin.](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development)

## <a name="test-the-project"></a>Projeyi test edin

Uygulama nın başlatılmasından sonra A düğmesini tıklatın ve bırakın, ardından DevKit kartını hafifçe sallayın. Bu eylem, daha önce belirttiğiniz hashtag'i içeren rasgele bir tweet alır. Birkaç saniye içinde, DevKit ekranınızda bir tweet görüntülenir:

### <a name="arduino-application-initializing"></a>Arduino uygulaması nın başlatılması...

![Arduino-uygulama-başbaşlatma](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-1.png)

### <a name="press-a-to-shake"></a>Sallamak için A tuşuna basın...

![Basın-A-to-shake](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-2.png)

### <a name="ready-to-shake"></a>Sallanmaya hazır...

![Sallamaya hazır](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-3.png)

### <a name="processing"></a>Işleme...

![İşleniyor](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-4.png)

### <a name="press-b-to-read"></a>Okumak için B tuşuna basın...

![Okumaya basın](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-5.png)

### <a name="display-a-random-tweet"></a>Rastgele bir tweet görüntüleyin...

![Rastgele tweet görüntüle](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-6.png)

- A tuşuna tekrar basın, ardından yeni bir tweet için sallayın.
- Tweetin geri kalanında gezinmek için B düğmesine basın.

## <a name="how-it-works"></a>Nasıl çalışır?

![şema](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/diagram.png)

Arduino çizimi, bir olayı Azure IoT Hub'ına gönderir. Bu olay Azure İşlevler uygulamasını tetikler. Azure İşlevleri uygulaması, Twitter'ın API'sine bağlanma ve bir tweet alma mantığını içerir. Daha sonra tweet metnini C2D (Buluttan cihaza) iletisine sarar ve aygıta geri gönderir.

## <a name="optional-use-your-own-twitter-bearer-token"></a>İsteğe bağlı: Kendi Twitter taşıyıcı belirteci kullanın

Test amacıyla, bu örnek proje önceden yapılandırılmış bir Twitter taşıyıcı belirteci kullanır. Ancak, her Twitter hesabı için bir [oran sınırı](https://dev.twitter.com/rest/reference/get/search/tweets) vardır. Kendi belirtecinizi kullanmayı düşünüyorsanız aşağıdaki adımları izleyin:

1. Yeni bir Twitter uygulaması kaydetmek için [Twitter Developer portalına](https://dev.twitter.com/) gidin.

2. Uygulamanızın [Tüketici Anahtarı ve Tüketici Sırları'nı alın.](https://support.yapsody.com/hc/en-us/articles/360003291573-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-)

3. Bu iki anahtardan bir Twitter taşıyıcı belirteci oluşturmak için [bazı yardımcı program](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/) kullanın.

4. Azure [portalı](https://portal.azure.com/){:target="_blank"}'de **Kaynak Grubuna** girin ve "Sallama, Sallama" projeniz için Azure İşi'ni (Tür: Uygulama Hizmeti) bulun. Adı her zaman 'shake ...' içerir Dize.

   ![azure fonksiyonu](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function.png)

5. Kendi belirteç ile `run.csx` **shakeshake-cs > Fonksiyonlar** içinde için kodu güncelleştirin:

   ```csharp
   string authHeader = "Bearer " + "[your own token]";
   ```
  
   ![twitter-belirteç](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/twitter-token.png)

6. Dosyayı kaydedin ve **Çalıştır'ı**tıklatın.

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunları giderme veya geri bildirim sağlama. 

### <a name="problems"></a>Sorun

Her adım başarıyla çalışırken ekranın 'Tweet Yok' görüntülediğini görebileceğiniz bir sorun. Bu durum normalde örneği ilk kez dağıtTığınız ve çalıştırdığınızda gerçekleşir, çünkü işlev uygulaması uygulamayı başlatmak için birkaç saniyeden bir dakikaya kadar herhangi bir yerde gerektirir. 

Veya, kodu çalıştırırken, uygulamanın yeniden başlatılmasına neden olan bazı blips vardır. Bu durum gerçekleştiğinde, aygıt uygulaması tweet'i almak için bir zaman ara sı alabilir. Bu durumda, sorunu çözmek için aşağıdaki yöntemlerden birini veya her ikisini deneyebilirsiniz:

1. Cihaz uygulamasını yeniden çalıştırmak için DevKit'teki sıfırlama düğmesini tıklatın.

2. Azure [portalında,](https://portal.azure.com/)oluşturduğunuz Azure İşlevleri uygulamasını bulun ve yeniden başlatın:

   ![azure-fonksiyon-yeniden başlatma](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function-restart.png)

### <a name="feedback"></a>Geri Bildirim

Başka sorunlarla karşılaşırsanız, [IoT DevKit SSS'ye](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) bakın veya aşağıdaki kanalları kullanarak bizimle iletişime geçin:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bir DevKit aygıtını Azure IoT Uzaktan İzleme çözüm hızlandırıcınıza nasıl bağlayıp bir tweet aldığınızı öğrendiğinize göre, önerilen sonraki adımlar şunlardır:

* [Azure IoT Uzaktan İzleme çözüm hızlandırıcıya genel bakış](https://docs.microsoft.com/azure/iot-suite/)