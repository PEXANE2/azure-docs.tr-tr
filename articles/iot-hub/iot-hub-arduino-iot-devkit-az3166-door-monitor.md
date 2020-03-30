---
title: Azure İşlevlerini kullanarak kapı açıldığında e-posta gönderme
description: Bir kapının ne zaman açıldığını tespit etmek için manyetik sensörü izleyin ve e-posta bildirimi göndermek için Azure Fonksiyonları'nı kullanın.
author: liydu
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 03/19/2018
ms.author: liydu
ms.openlocfilehash: 6bebe8ac6b9869466938600d6267fd0062c84477
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75977294"
---
# <a name="door-monitor----using-azure-functions-and-sendgrid-send-email-when-a-door-is-opened"></a>Kapı Monitörü -- Azure İşlerini ve SendGrid'i kullanarak, kapı açıldığında e-posta gönderin           

MXChip IoT DevKit dahili manyetik sensör içerir. Bu projede, yakındaki güçlü bir manyetik alanın varlığını ya da yokluğunu tespit emzebilirsiniz -- bu durumda, küçük, kalıcı bir mıknatıstan geliyor.

## <a name="what-you-learn"></a>Öğrenecekleriniz

Bu projede şunları öğreniyorsunuz:
- Yakındaki bir mıknatısın hareketini algılamak için MXChip IoT DevKit'in manyetik sensörünü nasıl kullanacağız?
- E-posta adresinize bir bildirim göndermek için SendGrid hizmetini nasıl kullanacağınız.

> [!NOTE]
> Bu projenin pratik kullanımı için aşağıdaki görevleri gerçekleştirin:
> - Kapının kenarına bir mıknatıs monte edin.
> - DevKit'i mıknatısın yakınına kapı sıkışmasına monte edin. Kapıyı açmak veya kapatmak sensörü tetikler ve bu da olayla ilgili bir e-posta bildirimi almanıza neden olur.

## <a name="what-you-need"></a>Ne gerekiyor

[Başlarken Kılavuzunu](iot-hub-arduino-iot-devkit-az3166-get-started.md) bitirin:

* DevKit'inizin Wi-Fi'a bağlanmasını sağlar
* Geliştirme ortamını hazırlama

Etkin bir Azure aboneliği. Eğer yoksa, şu yöntemlerden biri ile kaydolabilirsiniz:

* Ücretsiz [30 günlük deneme Microsoft Azure hesabını](https://azure.microsoft.com/free/)etkinleştirin.
* MSDN veya Visual Studio abonesiyseniz [Azure kredinizi](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) talep edin.

## <a name="deploy-the-sendgrid-service-in-azure"></a>SendGrid hizmetini Azure'da dağıtma

[SendGrid](https://sendgrid.com/) bulut tabanlı bir e-posta dağıtım platformudur. Bu hizmet e-posta bildirimleri göndermek için kullanılacaktır.

> [!NOTE]
> Bir SendGrid hizmetini zaten dağıttıysanız, doğrudan [Azure'daki IoT Hub'ına geçebilirsiniz.](#deploy-iot-hub-in-azure)

### <a name="sendgrid-deployment"></a>SendGrid Dağıtımı

Azure hizmetlerini sağlamak **için Azure'a Dağıt** düğmesini kullanın. Bu düğme, açık kaynak projelerinizi Microsoft Azure'a hızlı ve kolay bir şekilde dağıtımını sağlar.

Aşağıdaki **Azure'a Dağıt** düğmesini tıklatın. 

[![Azure'a Dağıt](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2FSendGridDeploy%2Fazuredeploy.json)

Azure hesabınızda zaten oturum açmadıysanız, şimdi oturum açın. 

Şimdi SendGrid kayıt formunu görüyorsunuz.

![SendGrid Dağıtımı](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-deploy.png)

Kayıt formunu doldurun:

   * **Kaynak grubu**: SendGrid hizmetini barındıracak veya varolan bir hizmeti kullanmak için bir kaynak grubu oluşturun. Bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını](../azure-resource-manager/management/manage-resource-groups-portal.md)kullanma .

   * **Adı**: SendGrid hizmetinizin adı. Sahip olabileceğiniz diğer hizmetlerden farklı olarak benzersiz bir ad seçin.

   * **Şifre**: Hizmet, bu projede hiçbir şey için kullanılmayacak bir parola gerektirir.

   * **E-posta**: SendGrid hizmeti bu e-posta adresine doğrulama gönderecektir.

Bu uygulamayı gelecekte bulmayı kolaylaştırmak için **panoya Pin** seçeneğini işaretleyin ve oturum açma formunu göndermek için **Satın Al'ı** tıklatın.
 
### <a name="sendgrid-api-key-creation"></a>SendGrid API Anahtar oluşturma

Dağıtım tamamlandıktan sonra tıklatın ve ardından **Yönet** düğmesini tıklatın. SendGrid hesap sayfanız, e-posta adresinizi doğrulamanız gereken yerde görüntülenir.

![SendGrid Yönet](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-manage.png)

SendGrid sayfasında Ayarlar > **API Tuşları'nı** >  **tıklatın****API Anahtarı Oluşturun.**

![Sendgrid Önce API Oluştur](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-first.png)

**API Anahtar Oluştur** **sayfasında, API Anahtar Adını** girdirin ve & **Görüntüle'yi Oluştur'u**tıklatın.

![Sendgrid OLUŞTURMA API İkinci](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/sendgrid-create-api-second.png)

API anahtarınız yalnızca bir kez görüntülenir. Bir sonraki adımda kullanıldığı ndan emin olun ve güvenli bir şekilde saklayın.

## <a name="deploy-iot-hub-in-azure"></a>Azure'da IoT Hub'ı dağıtma

Aşağıdaki adımlar, diğer Azure IoT ile ilgili hizmetleri sağlar ve bu proje için Azure Işlevlerini dağıtacaktır.

Aşağıdaki **Azure'a Dağıt** düğmesini tıklatın. 

[![Azure'a Dağıt](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FVSChina%2Fdevkit-door-monitor%2Fmaster%2Fazuredeploy.json)

Kayıt formu görüntülenir.

![IoTHub Dağıtım](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/iot-hub-deploy.png)

Kayıt formundaki alanları doldurun.

   * **Kaynak grubu**: SendGrid hizmetini barındıracak veya varolan bir hizmeti kullanmak için bir kaynak grubu oluşturun. Bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını](../azure-resource-manager/management/manage-resource-groups-portal.md)kullanma .

   * **Iot Hub Adı**: IoT hub'ınızın adı. Sahip olabileceğiniz diğer hizmetlerden farklı olarak benzersiz bir ad seçin.

   * **Iot Hub Sku**: F1 (abonelik başına bir ile sınırlıdır) ücretsizdir. [Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/iot-hub/)daha fazla fiyatlandırma bilgisi görebilirsiniz.

   * **E-posta Gönderen**: Bu alan, SendGrid hizmetini kurarken kullandığınız e-posta adresi olmalıdır.

Bu uygulamayı gelecekte bulmayı kolaylaştırmak için **panoya Pin** seçeneğini işaretleyin ve ardından bir sonraki adıma devam etmeye hazır olduğunuzda **Satın Al'ı** tıklatın.
 
## <a name="build-and-upload-the-code"></a>Kodu oluşturma ve yükleme

Ardından, örnek kodu VS Kodu'na yükleyin ve gerekli Azure hizmetlerini verin.

### <a name="start-vs-code"></a>VS Kodunu Başlat

- DevKit'inizin bilgisayarınıza bağlı **olmadığından** emin olun.
- VS Code'u başlatın.
- DevKit'i bilgisayarınıza bağlayın.

> [!NOTE]
> VS Kodunu başlattığınızda, Arduino IDE veya ilgili yönetim kurulu paketini bulamadığını belirten bir hata iletisi alabilirsiniz. Bu hatayı alırsanız, VS Kodunu kapatın, Arduino IDE'yi yeniden başlatın ve VS Kodu Arduino IDE yolunu doğru şekilde bulunmalıdır.

### <a name="open-arduino-examples-folder"></a>Arduino Örnekleri klasörünü aç

Sol taraftaki **ARDUINO ÖRNEKLERİ** bölümünü genişletin, **AzureIoT'> MXCHIP AZ3166 örneklerine**göz atın ve **DoorMonitor'u**seçin. Bu eylem, içinde bir proje klasörü bulunan yeni bir VS Kodu penceresi açar.

![mini çözüm örnekleri](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/vscode-examples.png)

Örnek uygulamayı komut paletinden de açabilirsiniz. Komut `Ctrl+Shift+P` paletini `Cmd+Shift+P`açmak için (macOS: ) kullanın, **Arduino**yazın ve ardından **Arduino'yu**bulup seçin: Örnekler .

### <a name="provision-azure-services"></a>Azure hizmetlerini sağlama

Çözüm penceresinde, bulut sağlama görevini çalıştırın:
- Türü `Ctrl+P` (macOS: `Cmd+P`).
- Sağlanan `task cloud-provision` metin kutusuna girin.

VS Code terminalinde, etkileşimli komut satırı, gerekli Azure hizmetlerini sağlama konusunda size yol sunar. Azure'da Daha önce [Dağıtı IoT](#deploy-iot-hub-in-azure)Hub'ında uyguladığınız istenilen listeden aynı öğelerin tümünü seçin.

![Bulut Sağlama](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/cloud-provision.png)

> [!NOTE]
> Azure'da oturum açmaya çalışırken sayfa yükleme durumunda asılıysa, bu sorunu gidermek için [IoT DevKit SSS'nin "oturum açarken sayfa askıda kalır" bölümüne](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#page-hangs-when-log-in-azure) bakın. 

### <a name="build-and-upload-the-device-code"></a>Aygıt kodunu oluşturma ve yükleme

Ardından, aygıtın kodunu yükleyin.

#### <a name="windows"></a>Windows

1. Çalıştırmak `Ctrl+P` `task device-upload`için kullanın.

2. Terminal, yapılandırma moduna girmenizi ister. Bunu yapmak için A düğmesini basılı tutun, ardından sıfırlama düğmesine basın ve bırakın. Ekran DevKit kimlik numarasını ve *Yapılandırma*sözcüciyi görüntüler.

#### <a name="macos"></a>macOS

1. DevKit'i yapılandırma moduna koyun: A düğmesini basılı tutun, ardından sıfırlama düğmesine basın ve bırakın. Ekranda 'Yapılandırma' görüntülenir.

2. Çalıştırmak `Cmd+P` `task device-upload`için tıklatın.

#### <a name="verify-upload-and-run-the-sample-app"></a>Örnek uygulamayı doğrulayın, yükleyin ve çalıştırın

[Provision Azure hizmetleri](#provision-azure-services) adımından alınan bağlantı dizesi şimdi ayarlandı. 

VS Code daha sonra Arduino çizimini doğrulamaya ve DevKit'e yüklemeye başlar.

![cihaz yükleme](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/device-upload.png)

DevKit yeniden başlatılır ve kodu çalıştırmaya başlar.

> [!NOTE]
> Bazen bir "Hata: AZ3166: Bilinmeyen paket" hata iletisi alabilirsiniz. Bu hata, yönetim kurulu paket dizini doğru şekilde yenilenmediğinde oluşur. Bu hatayı gidermek için [IoT DevKit SSS](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development)geliştirme bölümüne bakın.

## <a name="test-the-project"></a>Projeyi test edin

Program ilk olarak DevKit kararlı bir manyetik alanın varlığında olduğunda başlar.

Başlatmadan sonra `Door closed` ekranda görüntülenir. Manyetik alanda bir değişiklik olduğunda, durum `Door opened`. Kapı durumu her değiştiğinde, bir e-posta bildirimi alırsınız. (Bu e-posta iletilerinin alınması beş dakika kadar sürebilir.)

![Sensöre yakın mıknatıslar: Kapı Kapalı](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-closed.jpg "Sensöre yakın mıknatıslar: Kapı Kapalı")

![Mıknatıs sensörden uzaklaştı: Kapı Açıldı](media/iot-hub-arduino-iot-devkit-az3166-door-monitor/test-door-opened.jpg "Mıknatıs sensörden uzaklaştı: Kapı Açıldı")

## <a name="problems-and-feedback"></a>Sorunlar ve geri bildirim

Sorunlarla karşılaşırsanız, [IoT DevKit SSS'ye](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) bakın veya aşağıdaki kanalları kullanarak bağlanın:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Sonraki adımlar

Bir DevKit cihazını Azure IoT Uzaktan İzleme çözüm hızlandırıcınıza nasıl bağlayabileceğinizi öğrendiniz ve e-posta göndermek için SendGrid hizmetini kullandınız. Önerilen sonraki adımlar şunlardır:

* [Azure IoT Uzaktan İzleme çözüm hızlandırıcıya genel bakış](https://docs.microsoft.com/azure/iot-suite/)
* [Bir MXChip IoT DevKit cihazını Azure IoT Merkezi uygulamanıza bağlayın](/azure/iot-central/core/howto-connect-devkit)
