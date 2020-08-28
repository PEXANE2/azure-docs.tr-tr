---
title: Azure Işlevleri ile Twitter iletisi alma | Microsoft Docs
description: Gerçekleşmesi algılamak için hareket algılayıcısını kullanın ve belirttiğiniz diyez etiketiyle rastgele bir tweet bulmak için Azure Işlevlerini kullanın
author: liydu
manager: jeffya
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/07/2018
ms.author: liydu
ms.custom: devx-track-csharp
ms.openlocfilehash: f0aa71b34818cf373d1bb58531ee5c68c8d3d5ec
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89004332"
---
# <a name="shake-shake-for-a-tweet----retrieve-a-twitter-message-with-azure-functions"></a>Tweet için sallayın, sallayın, Azure Işlevleri ile Twitter iletisi alın

Bu projede, Azure Işlevleri 'ni kullanarak bir olayı tetiklemek için hareket algılayıcıyı nasıl kullanacağınızı öğrenirsiniz. Uygulama, Arduino taslacda yapılandırdığınız #hashtag rastgele bir tweet alır. Tweet, DevKit ekranında görüntülenir.

## <a name="what-you-need"></a>Gerekenler

[Başlarken Kılavuzunu](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) şu şekilde sona erdirin:

* DevKit 'in Wi-Fi ' a bağlanmasını sağlar.
* Geliştirme ortamını hazırlayın.

Etkin bir Azure aboneliği. Bir tane yoksa, aşağıdaki yöntemlerden birini kullanarak kaydedebilirsiniz:

* [30 günlük ücretsiz deneme Microsoft Azure hesabını](https://azure.microsoft.com/free/) etkinleştirin
* MSDN veya Visual Studio abonesi olduğunuzda [Azure kredinizi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) talep edin

## <a name="open-the-project-folder"></a>Proje klasörünü açın

Proje klasörünü açarak başlayın. 

### <a name="start-vs-code"></a>VS Code Başlat

* DevKit 'in bilgisayarınıza bağlı olduğundan emin olun.

* VS Code'u başlatın.

* DevKit 'i bilgisayarınıza bağlayın.

   > [!NOTE]
   > VS Code başlatırken, Arduino IDE veya ilgili Pano paketinin bulunamadığını belirten bir hata iletisi alabilirsiniz. Bu hata oluşursa VS Code kapatın ve Arduino IDE 'yi yeniden başlatın. VS Code artık Arduino IDE yolunu doğru bir şekilde bulmalıdır.

### <a name="open-the-arduino-examples-folder"></a>Arduino örnekleri klasörünü açın

Sol taraftaki **Arduino örnekleri** bölümünü GENIŞLETIN, **mxyongaAZ3166 > AzureIoT için örneklere**gidin ve **ShakeShake**' yi seçin. Proje klasörünü görüntüleyen yeni bir VS Code penceresi açılır. MXYONGAAZ3166 bölümünü göremiyorsanız, cihazınızın doğru bir şekilde bağlı olduğundan emin olun ve Visual Studio Code yeniden başlatın.  
![mini çözüm-örnekler](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/vscode_examples.png)

Örnek projeyi komut paletinden da açabilirsiniz. `Ctrl+Shift+P`Komut paletini açmak için (MacOS:) seçeneğine tıklayın `Cmd+Shift+P` , **Arduino**yazın ve ardından **Arduino: örnekleri**bulun ve seçin.

## <a name="provision-azure-services"></a>Azure hizmetlerini sağlama

Çözüm penceresinde, öğesini `Ctrl+P` girerek (MacOS:) görevini çalıştırın `Cmd+P` `task cloud-provision` .

VS Code terminalinde, etkileşimli bir komut satırı gerekli Azure hizmetlerini sağlama sırasında size rehberlik eder:

![bulut sağlama](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-provision.png)

> [!NOTE]
> Azure 'da oturum açmaya çalışırken sayfa yükleme durumunda askıda kalırsa, [IoT DevKit SSS içindeki "oturum açma sayfası askıda kalıyor" adımına](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure)bakın.
 
## <a name="modify-the-hashtag"></a>#Hashtag değiştirin

`ShakeShake.ino`Bu kod satırını açın ve bulun:

```cpp
static const char* iot_event = "{\"topic\":\"iot\"}";
```

`iot`Küme ayraçları içindeki dizeyi, tercih ettiğiniz diyez etiketiyle değiştirin. DevKit daha sonra bu adımda belirttiğiniz diyez etiketini içeren bir rastgele Tweet alır.

## <a name="deploy-azure-functions"></a>Azure İşlevleri’ni dağıtma

`Ctrl+P` `Cmd+P` `task cloud-deploy` Azure işlevleri kodunu dağıtmaya başlamak üzere çalıştırmak için (MacOS:) kullanın:

![bulut-dağıtım](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/cloud-deploy.png)

> [!NOTE]
> Azure Işlevi bazen düzgün çalışmayabilir. Bu sorunu oluştuğunda gidermek için [IoT DevKit SSS konusunun "derleme hatası" bölümüne](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#compilation-error-for-azure-function)bakın.

## <a name="build-and-upload-the-device-code"></a>Cihaz kodunu derleyin ve karşıya yükleyin

Sonra, cihaz kodunu derleyin ve karşıya yükleyin.

### <a name="windows"></a>Windows

1. `Ctrl+P`Çalıştırmak için kullanın `task device-upload` .

2. Terminal sizden yapılandırma moduna girmenizi ister. Bunun için:

   * Düğmeyi basılı tutarak A

   * Sıfırla düğmesini gönderin ve serbest bırakın.

3. Bu ekranda DevKit KIMLIĞI ve ' Configuration ' görüntülenir.

### <a name="macos"></a>Mac OS

1. DevKit 'ı yapılandırma moduna alın:

   Düğme A tuşunu basılı tutarak sıfırlama düğmesini gönderin ve serbest bırakın. Ekran ' yapılandırma ' görüntüler.

2. `Cmd+P` `task device-upload` Adımdan alınan bağlantı dizesini ayarlamak için öğesini kullanın `task cloud-provision` .

### <a name="verify-upload-and-run"></a>Doğrulayın, karşıya yükleyin ve çalıştırın

Artık bağlantı dizesi ayarlandı, uygulamayı doğrular ve karşıya yükler, ardından onu çalıştırır. 

1. VS Code, Arduino taslağını DevKit 'e doğrulamaya ve karşıya yüklemeye başlar:

   ![cihaz-karşıya yükle](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/device-upload.png)

2. DevKit yeniden başlatılır ve kodu çalıştırmaya başlar.

"Hata: AZ3166: bilinmeyen paket" hata iletisi alabilirsiniz. Pano paket dizini doğru yenilenmediğinde bu hata oluşur. Bu sorunu çözmek için [IoT DevKit SSS içindeki "bilinmeyen paket" hatasını](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development)denetleyin.

## <a name="test-the-project"></a>Projeyi test etme

Uygulama başlatma sonrasında A düğmesine tıklayın ve sonra DevKit panosunu yavaşça sallayın. Bu eylem, daha önce belirttiğiniz diyez etiketini içeren bir rastgele Tweet alır. Birkaç saniye içinde, DevKit ekranınızda bir tweet görüntülenir:

### <a name="arduino-application-initializing"></a>Arduino uygulaması başlatılıyor...

![Arduino-uygulama-başlatılıyor](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-1.png)

### <a name="press-a-to-shake"></a>Sallanması için A 'ya basın...

![A-salya 'ya kadar basın](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-2.png)

### <a name="ready-to-shake"></a>Ayrılmaya hazırlanıyor...

![Salmaya hazırlanma](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-3.png)

### <a name="processing"></a>İşleniyor...

![İşleniyor](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-4.png)

### <a name="press-b-to-read"></a>Okumak için B tuşuna basın...

![-B-okuma tuşlarına basın](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-5.png)

### <a name="display-a-random-tweet"></a>Rastgele bir tweet görüntüle...

![Görüntü-a-Random-Tweet](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/result-6.png)

- Düğmeye bir kez daha basın ve ardından yeni bir Tweet için sallayın.
- Tweet geri kalanında gezinmek için düğme B 'ye basın.

## <a name="how-it-works"></a>Nasıl çalışır?

![şema](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/diagram.png)

Arduino taslağı Azure IoT Hub bir olay gönderir. Bu olay, Azure Işlevleri uygulamasını tetikler. Azure Işlevleri uygulaması, Twitter API 'sine bağlanma ve bir tweet alma mantığını içerir. Daha sonra Tweet metnini bir C2D (buluttan cihaza) iletisine sarmalar ve cihaza geri gönderir.

## <a name="optional-use-your-own-twitter-bearer-token"></a>İsteğe bağlı: kendi Twitter taşıyıcı belirtecinizi kullanın

Bu örnek proje, test amacıyla önceden yapılandırılmış bir Twitter taşıyıcı belirteci kullanır. Ancak, her Twitter hesabı için bir [hız sınırı](https://dev.twitter.com/rest/reference/get/search/tweets) vardır. Kendi belirtecinizi kullanmayı düşünmek istiyorsanız aşağıdaki adımları izleyin:

1. Yeni bir Twitter uygulamasını kaydetmek için [Twitter geliştirici portalı](https://dev.twitter.com/) ' na gidin.

2. Uygulamanızın [Tüketici anahtar ve tüketici gizli dizilerini alın](https://support.yapsody.com/hc/en-us/articles/360003291573-How-do-I-get-a-Twitter-Consumer-Key-and-Consumer-Secret-key-) .

3. Bu iki anahtardan bir Twitter taşıyıcı belirteci oluşturmak için [bazı yardımcı programları](https://gearside.com/nebula/utilities/twitter-bearer-token-generator/) kullanın.

4. {: Target = "_blank"} [Azure Portal](https://portal.azure.com/), **kaynak grubuna** ulaşın ve "sallama, sallama" projeniz Için Azure işlevini (tür: App Service) bulun. Ad her zaman ' Sal... ' içerir dizisinde.

   ![Azure-işlevi](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function.png)

5. `run.csx` **Shakeshake-CS > işlevleri** içinde kendi belirtecinizle ilgili kodu güncelleştirin:

   ```csharp
   string authHeader = "Bearer " + "[your own token]";
   ```
  
   ![Twitter-belirteç](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/twitter-token.png)

6. Dosyayı kaydedin ve **Çalıştır**' a tıklayın.

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunları giderme veya geri bildirim sağlama. 

### <a name="problems"></a>Sorunlar

Her adım başarıyla çalıştırıldığında ekranda ' fazla yer yok ' ekranında görüntülenen bir sorun. Bu durum normalde, örneği ilk kez dağıtıp çalıştırdığınızda gerçekleşir çünkü işlev uygulaması, uygulamayı soğuk olarak başlatmak için bir dakika kadar birkaç saniyeden fazla bir süre bekleyin. 

Ya da kodu çalıştırırken, uygulamanın yeniden başlatılmasına neden olan bazı sinyalleri 'ler vardır. Bu durum gerçekleştiğinde, cihaz uygulaması Tweet getirme zaman aşımını alabilir. Bu durumda, sorunu gidermek için bu yöntemlerin birini veya her ikisini de deneyebilirsiniz:

1. Cihaz uygulamasını yeniden çalıştırmak için DevKit üzerindeki sıfırla düğmesine tıklayın.

2. [Azure Portal](https://portal.azure.com/)oluşturduğunuz Azure işlevleri uygulamasını bulun ve yeniden başlatın:

   ![Azure-Function-yeniden Başlat](media/iot-hub-arduino-iot-devkit-az3166-retrieve-twitter-message/azure-function-restart.png)

Başka sorunlarla karşılaşırsanız, [IoT DevKit hakkında SSS bölümüne](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) bakın veya aşağıdaki kanalları kullanarak bizimle iletişime geçin:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Artık bir DevKit cihazını Azure IoT uzaktan Izleme çözüm hızlandırıcısına bağlamayı ve bir tweet almanızı öğrendiğinize göre, önerilen sonraki adımlar aşağıda verilmiştir:

* [Azure IoT uzaktan Izleme çözüm hızlandırıcısının genel bakış](https://docs.microsoft.com/azure/iot-suite/)