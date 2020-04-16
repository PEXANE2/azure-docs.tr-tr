---
title: 'Öğretici: Sesler Konuşma SDK kullanarak bot etkinleştirmek - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, Microsoft Bot Framework'ü kullanarak bir Echo Bot oluşturur, Azure'a dağıtacak ve Bot-Framework Direct Line Speech kanalına kaydedeceksiniz. Ardından, Botunuzla konuşmanızı ve yanıt verdiğini duymanızı sağlayan windows için örnek bir istemci uygulaması yapılandırırsınız.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: trbye
ms.openlocfilehash: b2c119f6552773bce7bb93a503c22324278ac0bc
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399470"
---
# <a name="tutorial-voice-enable-your-bot-using-the-speech-sdk"></a>Öğretici: Konuşma SDK kullanarak botunuzu sesli etkinleştirin

Artık konuşma robotunun sesini kolayca etkinleştirmek için Konuşma hizmetinin gücünü kullanabilirsiniz.

Bu eğitimde, Microsoft Bot Framework'ü kullanarak bir Echo Bot oluşturur, Azure'a dağıtacak ve Bot-Framework Direct Line Speech kanalına kaydedeceksiniz. Ardından, Botunuzla konuşmanızı ve yanıt verdiğini duymanızı sağlayan windows için örnek bir istemci uygulaması yapılandırırsınız.

Bu öğretici, yolculuğuna Azure, Bot-Framework botları, Doğrudan Satır Konuşması veya Konuşma SDK ile yeni başlayan ve sınırlı kodlamaya sahip hızlı bir çalışma sistemi oluşturmak isteyen geliştiriciler için tasarlanmıştır. Bu hizmetlerle ilgili hiçbir deneyim veya aşinalık gerekmez.

Bu alıştırmanın sonunda, aşağıdaki gibi çalışacak bir sistem kurmuş olacaksınız:

1. Örnek istemci uygulaması Doğrudan Hat Konuşma kanalına ve Echo Bot'a bağlanmak üzere yapılandırılmıştır
1. Ses, düğmeye basarak varsayılan mikrofondan kaydedilir (veya özel anahtar kelime etkinleştirilirse sürekli olarak kaydedilir)
1. İsteğe bağlı olarak, özel anahtar kelime algılama olur, buluta ses akışı gating
1. Konuşma SDK'sını kullanarak, uygulama Doğrudan Satır Konuşması kanalına bağlanır ve ses akışı sağlar
1. İsteğe bağlı olarak, daha yüksek doğrulukanahtar kelime doğrulama hizmet olur
1. Ses konuşma tanıma hizmetine aktarılır ve metne aktarılır
1. Tanınan metin, Bot Framework Etkinliği olarak Echo-Bot'a aktarılır 
1. Yanıt metni Metinden Konuşmaya (TTS) hizmeti tarafından sese dönüştürüldü ve oynatma için istemci uygulamasına geri aktanındı

![diyagram etiketi](media/tutorial-voice-enable-your-bot-speech-sdk/diagram.png "Konuşma Kanalı akışı")

> [!NOTE]
> Bu öğreticideki adımlar ücretli bir hizmet gerektirmez. Yeni bir Azure kullanıcısı olarak, bu öğreticiyi tamamlamak için ücretsiz Azure deneme aboneliğinizden ve Konuşma hizmetinin ücretsiz katmanından alınan kredileri kullanabilirsiniz.

Bu öğreticinin kapsadığı şey şu:
> [!div class="checklist"]
> * Yeni Azure kaynakları oluşturun
> * Echo Bot örneğini bir Azure Uygulama Hizmetine oluşturun, test edin ve dağıtın
> * Botunuzu Doğrudan Hat Konuşma kanalıyla kaydedin
> * Echo Bot'unuzun izlesin ama Windows Voice Assistant İstemcisini oluşturun ve çalıştırın
> * Özel anahtar kelime etkinleştirme ekleme
> * Tanınan ve konuşulan konuşmanın dilini değiştirmeyi öğrenin

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için gerekenler şunlardır:

- Çalışan mikrofonve hoparlörlere (veya kulaklıklara) sahip bir Windows 10 bilgisayar
- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) veya üstü
- [.NET Framework Runtime 4.6.1](https://dotnet.microsoft.com/download) veya üzeri
- Bir Azure hesabı. [Ücretsiz kaydolun.](https://azure.microsoft.com/free/ai/)
- [Bir GitHub](https://github.com/) hesabı
- [Windows için Git](https://git-scm.com/download/win)

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bu eğitimde oluşturacağınız istemci uygulaması bir avuç Azure hizmeti kullanır. Botunuzdan gelen yanıtlar için gidiş dönüş süresini azaltmak için, bu hizmetlerin aynı Azure bölgesinde bulunduğundan emin olmak istersiniz. Bu bölümde, **Batı ABD** bölgesinde bir kaynak grubu oluşturursunuz. Bu kaynak grubu, Bot-Framework, Direct Line Speech kanalı ve Konuşma hizmeti için ayrı kaynaklar oluşturulurken kullanılacaktır.

1. <a href="https://ms.portal.azure.com/#create/Microsoft.ResourceGroup" target="_blank">Kaynak grubu oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
1. Bazı bilgiler sağlamanız istenir:
   * **Aboneliği** **Ücretsiz Deneme** olarak ayarlayın (varolan bir aboneliği de kullanabilirsiniz).
   * **Kaynak grubunuz**için bir ad girin. Biz **SpeechEchoBotTutorial-ResourceGroup**öneririz.
   * **Bölge** açılır tarafından, **Batı ABD'yi**seçin.
1. **Gözden geçir ve oluştur**’a tıklayın. **Validation'ı**okuyan bir afiş görmeniz gerekir.
1. **Oluştur'u**tıklatın. Kaynak grubunun oluşturulması birkaç dakika sürebilir.
1. Bu eğitimde daha sonra oluşturacağınız kaynaklarda olduğu gibi, kolay erişim için bu kaynak grubunu panonuza sabitlemek iyi bir fikirdir. Bu kaynak grubunu sabitlemek istiyorsanız, pano sağ üst kısmındaki pin simgesine tıklayın.

### <a name="choosing-an-azure-region"></a>Azure bölgesi seçme

Bu öğretici için farklı bir bölge kullanmak istiyorsanız, bu faktörler seçimlerinizi sınırlayabilir:

* Desteklenen bir [Azure bölgesi](regions.md#voice-assistants)kullandığınızdan emin olun.
* Doğrudan Satır Konuşma kanalı, standart ve sinirsel seslere sahip metin-konuşma hizmetini kullanır. Sinirsel sesler [belirli Azure bölgeleriyle sınırlıdır.](regions.md#standard-and-neural-voices)
* Ücretsiz deneme anahtarları belirli bir bölgeyle sınırlı olabilir.

Bölgeler hakkında daha fazla bilgi için [Azure konumları'na](https://azure.microsoft.com/global-infrastructure/locations/)bakın.

## <a name="create-resources"></a>Kaynak oluşturma

Desteklenen bir bölgede bir kaynak grubunuz olduğuna göre, bir sonraki adım, bu öğreticide kullanacağınız her hizmet için ayrı kaynaklar oluşturmaktır.

### <a name="create-a-speech-service-resource"></a>Konuşma hizmeti kaynağı oluşturma

Konuşma kaynağı oluşturmak için aşağıdaki yönergeleri izleyin:

1. <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Konuşma hizmeti kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
4. Bazı bilgiler sağlamanız istenir:
   * Kaynağınıza bir **Ad**verin. Biz **SpeechEchoBotTutorial-Speech** öneririz
   * **Abonelik** **için, Ücretsiz Deneme'nin** seçildiğinden emin olun.
   * **Konum**için **Batı ABD'yi**seçin.
   * **Fiyatlandırma katmanı**için **F0'yi**seçin. Bu serbest katmandır.
   * **Kaynak grubu için** **SpeechEchoBotTutorial-ResourceGroup'u**seçin.
5. Gerekli tüm bilgileri girdikten sonra **Oluştur'u**tıklatın. Kaynağınızı oluşturmak birkaç dakika sürebilir.
6. Bu eğitimin ilerleyen saatlerinde bu hizmet için abonelik anahtarlarına ihtiyacınız olacak. Bu tuşlara kaynağınızın **Genel Bakışı** (Anahtarları Yönet) veya **Anahtarlar'dan**istediğiniz zaman erişebilirsiniz.

Bu noktada, kaynak grubunuzun **(SpeechEchoBotTutorial-ResourceGroup)** bir Konuşma kaynağına sahip olup olmadığını denetleyin:

| Adı | Tür  | Konum |
|------|-------|----------|
| SpeechEchoBotTutorial-Konuşma | Bilişsel Hizmetler | Batı ABD |

### <a name="create-an-azure-app-service-plan"></a>Azure App Service planı oluşturma

Bir sonraki adım, bir Uygulama Hizmet Planı oluşturmaktır. App Service planı, bir web uygulamasının birlikte çalıştırılacağı işlem kaynakları kümesini tanımlar.

1. <a href="https://ms.portal.azure.com/#create/Microsoft.AppServicePlanCreate" target="_blank">Azure Uygulama Hizmeti planı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
4. Bazı bilgiler sağlamanız istenir:
   * **Aboneliği** **Ücretsiz Deneme** olarak ayarlayın (varolan bir aboneliği de kullanabilirsiniz).
   * **Kaynak grubu için** **SpeechEchoBotTutorial-ResourceGroup'u**seçin.
   * Kaynağınıza bir **Ad**verin. Biz **SpeechEchoBotTutorial-AppServicePlan öneririz**
   * **İşletim Sistemi**için **Windows'u**seçin.
   * **Bölge**için **Batı ABD'yi**seçin.
   * **Fiyatlandırma Katmanı**için **Standart S1'in** seçildiğinden emin olun. Bu varsayılan değer olmalıdır. Değilse, **İşletim Sistemini** yukarıda açıklandığı gibi **Windows** olarak ayarladığınızdan emin olun.
5. **Gözden geçir ve oluştur**’a tıklayın. **Validation'ı**okuyan bir afiş görmeniz gerekir.
6. **Oluştur'u**tıklatın. Kaynak grubunun oluşturulması birkaç dakika sürebilir.

Bu noktada, kaynak grubunuzun **(SpeechEchoBotTutorial-ResourceGroup)** iki kaynağı olduğundan denetleyin:

| Adı | Tür  | Konum |
|------|-------|----------|
| SpeechEchoBotTutorial-AppServicePlan | App Service Planı | Batı ABD |
| SpeechEchoBotTutorial-Konuşma | Bilişsel Hizmetler | Batı ABD |

## <a name="build-an-echo-bot"></a>Bir Echo Bot oluşturun

Şimdi bazı kaynaklar oluşturduk, bir bot oluşturalım. Echo Bot örneğiyle başlayacağız. Endişelenmeyin, örnek kod herhangi bir değişiklik olmadan kullanmanız için hazırdır. Botu Azure'a yerleştirdikten sonra bağlayabildiğimiz Doğrudan Satır Konuşması kanalıyla çalışacak şekilde yapılandırılmıştır.

> [!NOTE]
> Takip eden yönergelerin yanı sıra Echo Bot hakkında ek [bilgiler, örneğin ReadME on GitHub'da](https://github.com/microsoft/BotBuilder-Samples/blob/master/samples/csharp_dotnetcore/02.echo-bot/README.md)mevcuttur.

### <a name="run-the-bot-sample-on-your-machine"></a>Makinenizde bot örneğini çalıştırın

1. Numune deposunu klonla:

   ```bash
   git clone https://github.com/Microsoft/botbuilder-samples.git
   ```

2. Visual Studio’yu başlatın.
3. Araç çubuğundan **Dosya** > **Açık** > **Projesi/Çözümü'nü**seçin ve Echo Bot proje çözümünü açın:

   ```
   samples\csharp_dotnetcore\02.echo-bot\EchoBot.sln
   ```

4. Proje yüklendikten sonra, projeyi oluşturmak ve çalıştırmak için <kbd>F5</kbd> tuşuna basın.
5. Bir tarayıcı başlatmalısınız ve buna benzer bir ekran görürsünüz.
    > [!div class="mx-imgBorder"]
    > [![echobot-running-on-localhost](media/tutorial-voice-enable-your-bot-speech-sdk/echobot-running-on-localhost.png "EchoBot localhost üzerinde çalışıyor")](media/tutorial-voice-enable-your-bot-speech-sdk/echobot-running-on-localhost.png#lightbox)

### <a name="test-the-bot-sample-with-the-bot-framework-emulator"></a>Bot Çerçeve Emülatörü ile bot örneğini test edin

[Bot Framework Emülatörü,](https://github.com/microsoft/botframework-emulator) bot geliştiricilerinin botlarını bir tünelden yerel veya uzaktan test edip hata ayıklamalarına olanak tanıyan bir masaüstü uygulamasıdır. Emülatör giriş(ses olarak değil) olarak yazılan metni destekler. Bot metin ile yanıt verecektir. Metin girişi ve metin çıkışıyla birlikte Echo Bot'unuzun yerel olarak çalışmasını test etmek için Bot Framework Emülatör'ü kullanmak için aşağıdaki adımları izleyin. Botu Azure'a dağıttıktan sonra ses girişi ve ses çıkışıyla test edeceğiz.

1. Bot [Framework Emülatör](https://github.com/Microsoft/BotFramework-Emulator/releases/latest) sürümünü 4.3.0 veya daha büyük bir şekilde yükleyin
2. Bot Framework Emülatörü başlatın ve botunuzu açın:
   * **Dosya** -> **Açık Bot**.
3. Botunuzun URL'sini girin. Örneğin:

   ```
   http://localhost:3978/api/messages
   ```
   ve "Connect" tuşuna basın.
4. Bot hemen "Merhaba ve hoş geldiniz!" ile selamlıyorum gerekir. iletisi döndürmektedir. Herhangi bir kısa mesaj yazın ve botbir yanıt almak onaylayın.
5. Bu bir Echo Bot örneği ile iletişim alışverişi gibi görünebilir: [ ![bot-framework-emülatörü](media/tutorial-voice-enable-your-bot-speech-sdk/bot-framework-emulator.png "Bot Çerçeve emülatörü")](media/tutorial-voice-enable-your-bot-speech-sdk/bot-framework-emulator.png#lightbox)

## <a name="deploy-your-bot-to-an-azure-app-service"></a>Botunuzu bir Azure Uygulama Hizmetine dağıtma

Bir sonraki adım, Echo Bot'u Azure'a dağıtmaktır. Bir bot dağıtmak için birkaç yolu vardır, ancak bu öğretici biz Visual Studio doğrudan yayıncılık üzerinde durulacak.

> [!NOTE]
> Alternatif olarak, [Azure CLI](https://docs.microsoft.com/azure/bot-service/bot-builder-deploy-az-cli) ve dağıtım [şablonlarını](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/directline-speech/csharp_dotnetcore/02.echo-bot/DeploymentTemplates)kullanarak bir bot dağıtabilirsiniz.

1. Visual Studio'dan, Doğrudan Hat Konuşma kanalıyla kullanılmak üzere yapılandırılan Echo Bot'u açın:

   ```
   samples\csharp_dotnetcore\02.echo-bot\EchoBot.sln
   ```

1. Çözüm **Gezgini'nde,** **EchoBot** projesine sağ tıklayın ve **Yayımla'yı seçin...**
1. **Yayımlama hedefini seç** başlıklı yeni bir pencere açılır.
1. **Azure hizmetleri** gezintisinden **Uygulama Hizmeti'ni** seçin, **Yeni Oluştur'u**seçin ve ardından **Profil Oluştur'u**tıklatın.
1. Uygulama **Hizmeti Oluştur** penceresi göründüğünde:
   * **Hesap Ekle'yi**tıklatın ve Azure hesap kimlik bilgilerinizle oturum açın. Zaten oturum açmışsanız, açılan listeden istediğiniz hesabı seçin.
   * Uygulama **Adı**için, Bot'unuzun dünya çapında benzersiz bir ad girmeniz gerekir. Bu ad, benzersiz bir bot URL'si oluşturmak için kullanılır. Tarih ve saat de dahil olmak üzere varsayılan bir değer doldurulur (Örneğin: "EchoBot20190805125647"). Bu öğretici için varsayılan adı kullanabilirsiniz.
   * **Abonelik**için , **Ücretsiz Deneme** olarak ayarlayın
   * **Kaynak Grubu**için **SpeechEchoBotTutorial-ResourceGroup'u** seçin
   * **Hosting Planı** **için, SpeechEchoBotTutorial-AppServicePlan** seçin
   * **Uygulama Öngörüleri**için , **Yok** olarak bırakın
1. **Oluştur'u** tıklatın
1. Visual Studio'da şuna benzer bir başarı iletisi görmelisiniz:

   ```
   Publish Succeeded.
   Web App was published successfully https://EchoBot20190805125647.azurewebsites.net/
   ```

1. Varsayılan tarayıcınız "Botunuzun hazır olduğunu" yazan bir sayfa yı açmalı ve görüntülemelidir.
1. Bu noktada, Azure portalında Kaynak Grubu **SpeechEchoBotTutorial-ResourceGroup'unuzu** kontrol edin ve üç kaynak olduğunu onaylayın:

| Adı | Tür  | Konum |
|------|-------|----------|
| EchoBot20190805125647 | App Service | Batı ABD |
| SpeechEchoBotTutorial-AppServicePlan | App Service planı | Batı ABD |
| SpeechEchoBotTutorial-Konuşma | Bilişsel Hizmetler | Batı ABD |

## <a name="enable-web-sockets"></a>Web soketlerini etkinleştirme

Botunuzun web soketlerini kullanarak Doğrudan Hat Konuşması kanalıyla iletişim kurabilmesi için küçük bir yapılandırma değişikliği yapmanız gerekir. Web soketlerini etkinleştirmek için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com)gidin ve Uygulama Hizmetinizi bulun. Kaynak **EchoBot20190805125647** (benzersiz uygulama adı) benzer adlandırılmalıdır.
2. Azure **hizmetleri** gezintisinde, **Ayarlar**altında **Yapılandırma'yı**tıklatın.
3. Genel **ayarlar** sekmesini seçin.
4. **Web soketleri** için geçişi bulun ve **A'ya**ayarlayın.
5. **Kaydet**’e tıklayın.

> [!TIP]
> Hizmeti durdurmak veya yeniden başlatmak için Azure Uygulama Hizmeti sayfanızın üst kısmındaki denetimleri kullanabilirsiniz. Sorun giderme de kullanışlı olabilir.

## <a name="create-a-channel-registration"></a>Kanal kaydı oluşturma

Botunuzu barındırmak için bir Azure Uygulama Hizmeti oluşturduğunuza göre, bir sonraki adım bir **Bot Kanalı Kaydı**oluşturmaktır. Kanal kaydı oluşturmak, botunuzu Doğrudan Hat Konuşması kanalı da dahil olmak üzere Bot-Framework kanallarına kaydetmek için bir ön koşuldur.

> [!NOTE]
> Botların kanallardan nasıl yararlandığınız hakkında daha fazla bilgi edinmek istiyorsanız, [bir botu kanallara bağlayın](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels?view=azure-bot-service-4.0)bölümüne bakın.


1. <a href="https://ms.portal.azure.com/#create/Microsoft.BotServiceConnectivityGalleryPackage" target="_blank">Azure Bot Kanalları Kaydı Oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
2. Bazı bilgiler sağlamanız istenir:
   * **Bot kolu için** **SpeechEchoBotTutorial-BotRegistration**girin.
   * **Abonelik**için **Ücretsiz Deneme'yi**seçin.
   * **Kaynak grubu için** **SpeechEchoBotTutorial-ResourceGroup'u**seçin.
   * **Konum**için **Batı ABD'yi**seçin.
     * **Fiyatlandırma katmanı**için **F0'yi**seçin.
     * **Mesajlaşma bitiş noktası**için, sonunda eklenen `/api/messages` yolu olan web uygulamanızın URL'sini girin. Örneğin: genel olarak benzersiz Uygulama Adınız **EchoBot20190805125647**ise, mesajlaşma `https://EchoBot20190805125647.azurewebsites.net/api/messages/`bitiş noktanız: .
     * **Uygulama öngörüleri**için, bunu **Kapalı**olarak ayarlayabilirsiniz. Daha fazla bilgi için [Bot analitiği](https://docs.microsoft.com/azure/bot-service/bot-service-manage-analytics?view=azure-bot-service-4.0)bölümüne bakın.
     * **Otomatik Oluşturma Uygulama Kimliği ve parolayı**yoksay.
5. **Bot Kanalları Kayıt** bıçağının alt kısmında **Oluştur'u**tıklatın.

Bu noktada, Azure portalında Kaynak Grubu **SpeechEchoBotTutorial-ResourceGroup'unuzu** kontrol edin. Şimdi dört kaynak göstermelidir:

| Adı | Tür  | Konum |
|------|-------|----------|
| EchoBot20190805125647 | App Service | Batı ABD |
| SpeechEchoBotTutorial-AppServicePlan | App Service planı | Batı ABD |
| SpeechEchoBotTutorial-BotRegistration | Bot Kanalları Kayıt | global |
| SpeechEchoBotTutorial-Konuşma | Bilişsel Hizmetler | Batı ABD |

> [!IMPORTANT]
> Bot Kanalları Kayıt kaynağı, Batı ABD'yi seçmiş olsanız bile Küresel bölgeyi gösterir. Bu beklenen bir durumdur.

## <a name="register-the-direct-line-speech-channel"></a>Doğrudan Satır Konuşma kanalını kaydedin

Şimdi doğrudan hat konuşma kanalı ile bot kayıt zamanı. Bu kanal, yankı botunuzun yanında yer alan bir istemci uygulaması ile Speech SDK ile derlenen bir istemci uygulaması arasında bağlantı kurmak için kullanılan kanaldır.

1. [Azure portalında](https://portal.azure.com) **SpeechEchoBotTutorial-BotRegistration** kaynağınızı bulun ve açın.
1. Azure **hizmetleri** gezintisinden, **Kanallar'ı**seçin.
   * Daha **fazla kanal**arayın, doğrudan **satır konuşma**bulmak ve tıklatın.
   * **Doğrudan satır Konuşmasını Yapılandır**başlıklı sayfadaki metni gözden geçirin ve "Bilişsel hizmet hesabı" etiketli açılır menüyü genişletin.
   * Botunuzu konuşma aboneliği anahtarınızla ilişkilendirmek için menüden daha önce oluşturduğunuz konuşma kaynağını (örneğin, **SpeechEchoBotTutorial-Speech)** seçin.
   * **Kaydet**’e tıklayın.

1. Bot **yönetimi** gezintisinden **Ayarlar'ı**tıklatın.
   * **Akış Bitiş Noktasını Etkinleştir**etiketli kutuyu işaretleyin. Bu, botunuz ve Doğrudan Hat Konuşma kanalı arasında web soketleri üzerine inşa edilmiş bir iletişim protokolü etkinleştirmek için gereklidir.
   * **Kaydet**’e tıklayın.

> [!TIP]
> Daha fazla bilgi edinmek istiyorsanız, [bkz.](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0) Bu sayfa ek bilgiler ve bilinen sorunları içerir.

## <a name="run-the-windows-voice-assistant-client"></a>Windows Ses Yardımcısı İstemciyi Çalıştırma

Bu adımda, Windows Voice Assistant Client'ı çalıştırın. İstemci, Doğrudan Hat Konuşması kanalını kullanarak botunla iletişimi yönetmek için [Konuşma SDK'sını](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk) kullanan C# bir Windows Presentation Foundation (WPF) uygulamasıdır. Özel bir istemci uygulaması yazmadan önce botunuzun etkileşimini sağlamak ve test etmek için kullanın. Açık kaynak, böylece ya çalıştırılabilir indirebilir ve çalıştırabilirsiniz, ya da kendiniz inşa.

Windows Voice Assistant İstemci, botunuzla bağlantıyı yapılandırmanızı, metin konuşmasını görüntülemenizi, Bot-Framework etkinliklerini JSON biçiminde görüntülemenizi ve uyarlanabilir kartları görüntülemenizi sağlayan basit bir kullanıcı arabirimi içerir. Ayrıca özel anahtar kelimelerin kullanımını destekler. Bu istemciyi, botunuzla konuşmak ve sesli yanıt almak için kullanırsınız.

Devam etmeden önce mikrofonunuzun ve hoparlörlerinizin etkin ve çalışır olduğundan emin olun.

1. [Windows Voice Assistant İstemci](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/README.md)için GitHub deposuna gidin.
1. Ya orada verilen yönergeleri izleyin
   * çalıştırılabilir çalıştırılabilir içeren bir ZIP paketi indirmek veya
   * depoyu klonlayarak ve projeyi oluşturarak, yürütülebilir oluşturabilirsiniz.

1. İstemci uygulamasını başlatın ve yapılandırın.
1. **Yeniden Bağlan'ı** tıklatın ve iletiyi gördüğünüzden emin olun **Mikrofon düğmesine basın veya botunuzla konuşmaya başlamak için yazın.**
1. Hadi test edelim. Mikrofon düğmesini tıklatın ve İngilizce birkaç kelime konuşun. Siz konuşurken tanınan metin görünür. Konuşmayı bitirdiğinde, bot kendi sesiyle cevap verecektir ve "yankı" diyerek tanınan sözcükleri takip eder.
1. Botla iletişim kurmak için metni de kullanabilirsiniz. Alt takimetini yazman. 

### <a name="troubleshooting-errors-in-windows-voice-assistant-client"></a>Windows Voice Assistant İstemci'deki sorun giderme hataları

Ana uygulama pencerenizde bir hata iletisi alırsanız, hatayı tanımlamak ve sorun gidermek için bu tabloyu kullanın:

| Hata | Ne yapmanız gerekir? |
|-------|----------------------|
|Hata Kimlik Doğrulama Hatası: WebSocket Yükseltme kimlik doğrulama hatası (401) ile başarısız oldu. Doğru abonelik anahtarı (veya yetkilendirme belirteci) ve bölge adını denetleme| Uygulamanın Ayarlar sayfasında, Konuşma Aboneliği anahtarını ve bölgesini doğru girdiğinden emin olun.<br>Konuşma anahtarınızın ve anahtar bölgenizin doğru girildidiğinden emin olun. |
|Hata BağlantısıHata: Bağlantı uzak ana bilgisayar tarafından kapatıldı. Hata kodu: 1011. Hata ayrıntıları: İleti göndermeden önce bota bağlanamayız | ["Akış Bitiş Noktasını Etkinleştir"](#register-the-direct-line-speech-channel) kutusunu ve/veya Web [ **soketlerini** ](#enable-web-sockets) A'ya değdirdiğinizi emin olun.<br>Azure Uygulama Hizmetinizin çalışır durumda olduğundan emin olun. Bu ysa, Uygulama Hizmetinizi yeniden başlatmayı deneyin.|
|Hata BağlantısıHata: Bağlantı uzak ana bilgisayar tarafından kapatıldı. Hata kodu: 1011. Hata ayrıntıları: Yanıt durum kodu başarı göstermez: 500 (InternalServerError)| Robotunuz çıktısı Activity [Speak](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) alanında nöral bir ses belirlemektedir, ancak Konuşma aboneliği anahtarınızla ilişkili Azure bölgesi nöral sesleri desteklemez. [Bkz. Standart ve sinirsel sesler.](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices)|
|Hata BağlantısıHata: Bağlantı uzak ana bilgisayar tarafından kapatıldı. Hata kodu: 1000. Hata ayrıntıları: Aşılmış maksimum web soket bağlantısı boşta süresi(> 300000 ms)| Kanala bağlantı beş dakikadan fazla açık ve etkin olmadığında beklenen bir hatadır. |

Sorununuzun sorunu tabloda ele alınmadıysa, [bkz.](faq-voice-assistants.md)

### <a name="view-bot-activities"></a>Bot etkinliklerini görüntüleyin

Her bot **Etkinlik** iletileri gönderir ve alır. Windows Voice Assistant Client'ın **Etkinlik Günlüğü** penceresinde, istemcinin bottan aldığı her etkinlikle zaman damgalı günlükleri görürsünüz. İstemcinin [`DialogServiceConnector.SendActivityAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.sendactivityasync) yöntemi kullanarak bota gönderdiği etkinlikleri de görebilirsiniz. Bir günlük öğesi seçtiğinizde, ilişkili etkinliğin ayrıntılarını JSON olarak gösterir.

İstemcinin aldığı bir Etkinliğin örnek json'u aşağıda veda edebilirsiniz:

```json
{
    "attachments":[],
    "channelData":{
        "conversationalAiData":{
             "requestInfo":{
                 "interactionId":"8d5cb416-73c3-476b-95fd-9358cbfaebfa",
                 "version":"0.2"
             }
         }
    },
    "channelId":"directlinespeech",
    "conversation":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79",
        "isGroup":false
    },
    "entities":[],
    "from":{
        "id":"SpeechEchoBotTutorial-BotRegistration"
    },
    "id":"89841b4d-46ce-42de-9960-4fe4070c70cc",
    "inputHint":"acceptingInput",
    "recipient":{
        "id":"129ebffe-772b-47f0-9812-7c5bfd4aca79|0000"
    },
    "replyToId":"67c823b4-4c7a-4828-9d6e-0b84fd052869",
    "serviceUrl":"urn:botframework:websocket:directlinespeech",
    "speak":"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='en-US'><voice name='Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)'>Echo: Hello and welcome.</voice></speak>",
    "text":"Echo: Hello and welcome.",
    "timestamp":"2019-07-19T20:03:51.1939097Z",
    "type":"message"
}
```

JSON çıktısında döndürülenler hakkında daha fazla bilgi edinmek için [Etkinlik'teki alanlara](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md)bakın. Bu öğreticinin amacı için [Metin](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#text) ve [Konuşma](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) alanlarına odaklanabilirsiniz.

### <a name="view-client-source-code-for-calls-to-the-speech-sdk"></a>Konuşma SDK'ya yapılan aramalar için istemci kaynak kodunu görüntüleme

Windows Voice Assistant İstemci NuGet paketi [Microsoft.CognitiveServices.Speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/), Konuşma SDK içerir kullanır. Örnek kodu gözden geçirmeye başlamak için iyi bir yer, bu iki [`VoiceAssistantClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/MainWindow.xaml.cs)Konuşma SDK nesneleri oluşturur dosyada yöntem InitSpeechConnector() olduğunu:
- [`DialogServiceConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconfig)- Yapılandırma ayarları için (örn. konuşma abonelik anahtarı, anahtar bölgesi)
- [`DialogServiceConnector`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.-ctor)- Tanınan konuşma ve bot yanıtlarını işlemek için kanal bağlantısını ve istemci abonelik olaylarını yönetmek için.

## <a name="add-custom-keyword-activation"></a>Özel anahtar kelime etkinleştirme ekleme

Konuşma SDK özel anahtar kelime etkinleştirme destekler. Microsoft'un Yardımcısı için "Hey Cortana" benzer, sürekli seçtiğiniz bir anahtar kelime için dinleyecek bir uygulama yazabilirsiniz. Bir anahtar kelimenin tek sözcük veya çok sözcüklü bir tümcecik olabileceğini unutmayın.

> [!NOTE]
> Anahtar *kelime* terimi genellikle *uyandırma sözcüğü*yle birbirinin yerine kullanılır ve her ikisinin de Microsoft belgelerinde kullanıldığını görebilirsiniz.

Anahtar kelime algılama istemci uygulamasında yapılır. Bir anahtar kelime kullanıyorsanız, anahtar kelime algılanırsa ses yalnızca Doğrudan Satır Konuşması kanalına aktarılır. Doğrudan Satır Konuşması kanalı, seçtiğiniz anahtar kelimenin ses akışının başlangıcında olduğunu doğrulamak için bulutta daha karmaşık işleme yapan *anahtar kelime doğrulaması (KWV)* adı verilen bir bileşen içerir. Anahtar kelime doğrulama başarılı olursa, o zaman kanal bot ile iletişim kurar.

Bir anahtar kelime modeli oluşturmak için aşağıdaki adımları izleyin, Windows Voice Assistant Client'ı bu modeli kullanacak şekilde yapılandırın ve son olarak bunu botunuzla test edin.

1. Konuşma hizmetini [kullanarak özel bir anahtar kelime oluşturmak](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws)için bu yönergeleri izleyin.
2. Önceki adımda indirdiğiniz model dosyasının zip'ini açın. Anahtar kelimenizin adı verilmelidir. ". `kws.table`
3. Windows Voice Assistant İstemci'sinde **Ayarlar** menüsünü bulun (sağ üstteki vites simgesine bakın). **Model dosya yolunu** bulun ve `kws.table` 2.
4. **Etkin**etiketli kutuyu işaretlediğinden emin olun. Bu iletiyi onay kutusunun yanında görmelisiniz: "Bir sonraki bağlantıda anahtar kelimeyi dinleyecek". Yanlış dosya veya geçersiz bir yol sağladıysanız, bir hata iletisi görmeniz gerekir.
5. Konuşma **abonelik anahtarınızı**, **abonelik anahtar bölgenizi**girin ve ardından **Ayarlar** menüsünü kapatmak için **Tamam'ı** tıklatın.
6. **Yeniden Bağlan'ı**tıklatın. "Yeni konuşma başladı - yazın, mikrofon düğmesine basın veya anahtar kelimeyi söyleyin" yazan bir ileti görmelisiniz. Uygulama artık sürekli olarak dinliyor.
7. Anahtar kelimenizle başlayan tümcecikleri konuşun. Örneğin: "**{anahtar kelimeniz}**, saat ne zaman?". Anahtar kelimeyi söyledikten sonra duraklatmanız gerekmez. Bittiğinde, iki şey olur:
   * Konuştuğunuz şeyin transkripsiyonunun bir kısmını göreceksiniz.
   * Kısa bir süre sonra, botun yanıtını duyacaksınız
8. Botunuzun desteklediği üç giriş türünü denemeye devam edin:
   * Alt çubukta yazılı metin
   * Mikrofon simgesine basma ve konuşma
   * Anahtar kelimenizle başlayan bir tümcecik söyleme

### <a name="view-the-source-code-that-enables-keyword"></a>Anahtar kelimeyi etkinleştiren kaynak kodu görüntüleme

Windows Voice Assistant İstemci kaynak kodunda, anahtar kelime algılamayı etkinleştirmek için kullanılan kodu gözden geçirmek için bu dosyalara bir göz atın:

1. [`VoiceAssistantClient\Models.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/Models.cs)diskteki yerel bir dosyadan [`KeywordRecognitionModel.fromFile()`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/keywordrecognitionmodel?view=azure-node-latest#fromfile-string-)modeli anında almak için kullanılan Konuşma SDK yöntemine bir çağrı içerir.
1. [`VoiceAssistantClient\MainWindow.xaml.cs`](https://github.com/Azure-Samples/Cognitive-Services-Voice-Assistant/blob/master/clients/csharp-wpf/VoiceAssistantClient/MainWindow.xaml.cs)sürekli anahtar kelime algılamayı [`DialogServiceConnector.StartKeywordRecognitionAsync()`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.dialog.dialogserviceconnector.startkeywordrecognitionasync)etkinleştiren Konuşma SDK yöntemine çağrı içerir.

## <a name="optional-change-the-language-and-bot-voice"></a>(İsteğe bağlı) Dili ve bot sesini değiştirme

Oluşturduğunuz bot, varsayılan Amerikan İngilizcesi metin den konuşmaya sesiyle İngilizce olarak dinleyecek ve yanıt verecektir. Ancak, İngilizce veya varsayılan bir ses kullanmakla sınırlı değildir. Bu bölümde, robotunuzun dinleyip yanıt alacağı dili nasıl değiştireceğinizi öğreneceksiniz. Ayrıca, bu dil için farklı bir ses seçmeyi de öğreneceksiniz.

### <a name="change-the-language"></a>Dili değiştirme

[Konuşma-metin](language-support.md#speech-to-text) tablosunda belirtilen dillerden herhangi birini seçebilirsiniz. Aşağıdaki örnekte, dili Almanca olarak değiştireceğiz.

1. Windows Voice Assistant Client uygulamasını açın, ayarlar düğmesine tıklayın (sağ `de-de` üst vites simgesi) ve Dil alanına girin (bu [konuşma-metin](language-support.md#speech-to-text) tablosunda belirtilen Yerel değerdir). Bu, varsayılan geçersiz kılınan sözlü dili `en-us`tanıyacak şekilde ayarlar. Bu aynı zamanda Doğrudan Satır Konuşma kanalının Bot yanıtı için varsayılan bir Almanca ses kullanmasını da emreder.
2. Ayarlar sayfasını kapatın ve yankı botunuza yeni bir bağlantı kurmak için Yeniden Bağlan düğmesini tıklayın.
3. Mikrofon düğmesine tıklayın ve Almanca bir ifade söyleyin. Tanınan metni ve yankı botun varsayılan Almanca sesiyle yanıtlarını görürsünüz.

### <a name="change-the-default-bot-voice"></a>Varsayılan bot sesini değiştirme

Bot yanıtı basit metin yerine [Konuşma Sentezi Biçimlendirme Dili](speech-synthesis-markup.md) (SSML) şeklinde belirtirse metinden konuşmaya ses ve kontrol telaffuzunu seçmek yapılabilir. Yankı bot SSML kullanmaz, ama biz kolayca bunu yapmak için kod değiştirebilirsiniz. Aşağıdaki örnekte yankı bot cevap SSML eklemek, böyle Alman ses Stefan Apollo (bir erkek ses) varsayılan kadın sesi yerine kullanılacaktır. Diliniz için desteklenen [Standart Sesler](language-support.md#standard-voices) ve [Nöral Sesler](language-support.md#neural-voices) listesine bakın.

1. Açarak `samples\csharp_dotnetcore\02.echo-bot\echo-bot.cs`başlayalım.
2. Şu iki satırı bulun:
    ```csharp
    var replyText = $"Echo: {turnContext.Activity.Text}";
    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replyText), cancellationToken);
    ```
3. Bunları şu şekilde değiştirin:
    ```csharp
    var replyText = $"Echo: {turnContext.Activity.Text}";
    var replySpeak = @"<speak version='1.0' xmlns='https://www.w3.org/2001/10/synthesis' xml:lang='de-DE'>
                    <voice name='Microsoft Server Speech Text to Speech Voice (de-DE, Stefan, Apollo)'>" +
                    $"{replyText}" + "</voice></speak>";
    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replySpeak), cancellationToken);
    ```
4. Visual Studio'da çözümünüzü oluşturun ve yapı hatalarını düzeltin.

'MessageFactory.Text' yöntemindeki ikinci bağımsız değişken, bot yanıtında [Etkinlik konuşma alanını](https://github.com/Microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#speak) ayarlar. Yukarıdaki değişiklikle, varsayılan olmayan bir Almanca ses belirtmek için basit metinden SSML'ye değiştirildi.

### <a name="redeploy-your-bot"></a>Botunuzu yeniden dağıtın

Artık botta gerekli değişikliği yaptığınıza göre, bir sonraki adım bunu Azure Uygulama Hizmetinizde yeniden yayımlamak ve denemektir:

1. Çözüm Gezgini penceresinde, **EchoBot** projesine sağ tıklayın ve **Yayımla'yı**seçin.
2. Önceki dağıtım yapılandırmanız zaten varsayılan olarak yüklendi. **EchoBot20190805125647**yanında **Yayımla'yı** tıklatın - Web Dağıtım .
3. **Yayımla Başarılı** İletisi Visual Studio çıkış penceresinde görünür ve "Botunhazır!" mesajıyla bir web sayfası açılır.
4. Windows Voice Assistant Client uygulamasını açın, ayarlar düğmesine tıklayın (sağ üst vites `de-de` simgesi) ve Dil alanında hala var olduğundan emin olun.
5. Yeni dağıtılan botunuzla yeniden bağlantı kurmak, yeni dilde konuşmak ve yeni sesle bu dilde bot yanıtı nızı duymak için [Windows Voice Assistant Client'ı Çalıştır'daki](#run-the-windows-voice-assistant-client) yönergeleri izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu eğitimde dağıtılan yankı-bot'u kullanmaya devam etmeyecekseniz, Azure Kaynak grubu **SpeechEchoBotTutorial-ResourceGroup'u**silerek onu ve ilişkili tüm Azure kaynaklarını kaldırabilirsiniz.

1. Azure [portalından,](https://portal.azure.com) **Azure hizmetleri** gezintisinden **Kaynak Grupları'nı** tıklatın.
2. Adlı kaynak grubunu bulun: **SpeechEchoBotTutorial-ResourceGroup**. Üç noktayı tıklatın (...).
3. **Kaynak grubunu sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma SDK ile kendi istemci uygulamanızı oluşturun](quickstart-voice-assistant-csharp-uwp.md)

## <a name="see-also"></a>Ayrıca bkz.

* Bot yanıt süresi iyileştirmesini görmek için [yakınınızdaki](https://azure.microsoft.com/global-infrastructure/locations/) bir Azure bölgesine dağıtım
* Yüksek kaliteli [Nöral TTS seslerini destekleyen](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#standard-and-neural-voices) bir Azure bölgesine dağıtım
* Doğrudan Satır Konuşma kanalıyla ilişkili fiyatlandırma:
  * [Bot Servisi fiyatlandırması](https://azure.microsoft.com/pricing/details/bot-service/)
  * [Konuşma hizmeti](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)
* Kendi ses özellikli botunuzu oluşturma ve dağıtma:
  * Bir [Bot-Framework bot oluşturun.](https://dev.botframework.com/) [Doğrudan Hat Konuşma kanalına](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech?view=azure-bot-service-4.0) kaydedin ve ses için [botunuzu özelleştirin](https://docs.microsoft.com/azure/bot-service/directline-speech-bot?view=azure-bot-service-4.0)
  * Mevcut [Bot-Framework çözümlerini](https://microsoft.github.io/botframework-solutions/index)keşfedin: Sanal bir [asistan](https://microsoft.github.io/botframework-solutions/overview/virtual-assistant-solution/) oluşturun ve [Doğrudan Satır Konuşması'na genişletin](https://microsoft.github.io/botframework-solutions/clients-and-channels/tutorials/enable-speech/1-intro/)
