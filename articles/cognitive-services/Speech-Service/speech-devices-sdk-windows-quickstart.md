---
title: "Hızlı Başlangıç: Windows-Konuşma hizmetinde konuşma cihazları SDK 'sını çalıştırma"
titleSuffix: Azure Cognitive Services
description: Windows konuşma cihazları SDK 'Sı ile çalışmaya başlama önkoşulları ve yönergeleri.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/10/2019
ms.author: erhopf
ms.openlocfilehash: ad90a6443cc1c94bcdb730e783b82dfdd4798676
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553023"
---
# <a name="quickstart-run-the-speech-devices-sdk-sample-app-on-windows"></a>Hızlı Başlangıç: Windows 'da konuşma cihazları SDK örnek uygulamasını çalıştırma

Bu hızlı başlangıçta, konuşma özellikli bir ürün oluşturmak veya bir konuşma [dökümü](conversation-transcription-service.md) cihazı olarak kullanmak için Windows Için konuşma cihazları SDK 'sını nasıl kullanacağınızı öğreneceksiniz. Şu anda yalnızca [Azure Kinect dk](https://azure.microsoft.com/services/kinect-dk/) destekleniyor.

Uygulama, konuşma SDK paketiyle oluşturulmuştur ve 64 bit Windows üzerinde Java IDE tutulma (v4). 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

Bu kılavuzda, bir konuşma Hizmetleri kaynağına sahip bir Azure bilişsel [Hizmetler](get-started.md) hesabı gerekir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

[Örnek uygulamanın](https://aka.ms/sdsdk-download-JRE) kaynak kodu, konuşma cihazları SDK 'sına dahildir. Ayrıca [github'da](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim Sistemi: 64 bit Windows
* [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)
* [Tutulma Java IDE](https://www.eclipse.org/downloads/)
* Yalnızca [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html) .
* [Microsoft Visual C++ Yeniden Dağıtılabilir](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
* Java için [konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download-JRE) en son sürümünü indirin ve. zip dosyasını çalışma dizininize ayıklayın.
   > [!NOTE]
   > JRE-Sample-Release. zip dosyası JRE örnek uygulamasını içerir ve bu hızlı başlangıç, uygulamanın C:\SDSDK\JRE-Sample-Release konumuna ayıklandığını varsayar

Konuşma dökümü şu anda yalnızca "en-US" ve "zh-CN" için kullanılabilir ve "merkezileştirme" ve "eastaya" bölgelerinde mevcuttur. Konuşma dökümünü kullanmak için bu bölgelerden birinde bir konuşma anahtarınız olmalıdır.

Hedefleri kullanmayı planlıyorsanız, bir [Language Understanding hizmeti (LUA)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) aboneliğine ihtiyacınız olacaktır. Lua ve amaç tanıma hakkında daha fazla bilgi edinmek için bkz. [lusıs C#Ile konuşma amaçlarını tanıma ](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp). Bu uygulama için [Örnek BIR lusıs modeli](https://aka.ms/sdsdk-luis) vardır.

## <a name="create-and-configure-the-project"></a>Projeyi oluşturma ve yapılandırma

1. Eclipse’i başlatın.

1. **Çakışan Küreler IDE başlatıcısı**' nda, **çalışma alanı** alanına yeni bir çalışma alanı dizininin adını girin. Ardından **Başlat**’ı seçin.

   ![Eclipse Başlatıcısı ekran görüntüsü](media/speech-devices-sdk/eclipse-launcher.png)

1. Çok geçmeden Eclipse IDE ana penceresi görüntülenir. Varsa, Hoş Geldiniz ekranını kapatın.

1. Çakışan Küreler menü çubuğunda **Dosya** > **Yeni** > **Java projesi**' ni seçerek yeni bir proje oluşturun. Kullanılabilir değilse **Proje** ' yi ve ardından **Java projesi**' ni seçin.

1. **Yeni Java proje** Sihirbazı başlatılır. Örnek projenin konumuna gözatın. **Son**’u seçin.

   ![Yeni Java Projesi sihirbazının ekran görüntüsü](media/speech-devices-sdk/eclipse-new-java-project.png)

1. **Paket Gezgini**' nde projenize sağ tıklayın. Bağlam menüsünden **Yapılandır** > **Maven Projesine Dönüştür**’ü seçin. **Son**’u seçin.

   ![Paket gezgininin ekran görüntüsü](media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. Proje klasörü **target\classes** Kopyala `kws.table` `participants.properties` `Microsoft.CognitiveServices.Speech.extension.pma.dll`

## <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Konuşma abonelik anahtarınızı kaynak koda ekleyin. Amaç tanıma denemek istiyorsanız, ayrıca ekleyin, [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarı ve uygulama kimliği

   Konuşma ve lusıs için, bilgileriniz `FunctionsList.java`şu şekilde olur:

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

    Konuşma dökümünü kullanıyorsanız, konuşma anahtarınız ve bölge bilgilerinizin de şu şekilde `Cts.java`olması gerekir:

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
    ```

1. Varsayılan Uyandırma sözcüğünü (anahtar) "Bilgisayar" dir. Sağlanan diğer birini de deneyebilirsiniz "Machine" veya "Yardımcısı" gibi sözcükleri Uyandırma. Bu alternatif Uyandırma sözcükler için kaynak dosyaları konuşma cihazları SDK'da anahtar sözcüğü klasörü arasındadır. Örneğin, `C:\SDSDK\JRE-Sample-Release\keyword\Computer` "bilgisayar" uyandırma sözcüğü için kullanılan dosyaları içerir.

   > [!TIP]
   > Ayrıca [özel Uyandırma word oluşturmak](speech-devices-sdk-create-kws.md).

    Yeni bir uyandırma sözcüğü kullanmak için ' de `FunctionsList.java`aşağıdaki iki satırı güncelleştirin ve uyandırma Word paketini uygulamanıza kopyalayın. Örneğin, uyanma Word paketinden `kws-machine.zip`' Machine ' uyandırma sözcüğünü kullanmak için:

   * Uyandırma sözcüğü paketini proje klasörü **hedefi/sınıfları**içine kopyalayın.

   * `FunctionsList.java` Anahtar sözcüğüyle ve paket adıyla güncelleştirin:

     ```java
     private static final String Keyword = "Machine";
     private static final String KeywordModel = "kws-machine.zip" // set your own keyword package name.
     ```

## <a name="run-the-sample-application-from-eclipse"></a>Örnek uygulamayı tutulma 'dan çalıştırma

1. Çakışan Küreler menü çubuğundan**Çalıştır** > **Java uygulaması**' nı **çalıştırın** > . Sonra **Functionslist** ve **Tamam**' ı seçin.

   ![Java uygulaması Seç ekranının ekran görüntüsü](media/speech-devices-sdk/eclipse-run-sample.png)

1. Konuşma cihaz SDK'sı örnek bir uygulama başlar ve şu seçeneklerini gösterir:

   ![Örnek konuşma cihaz SDK'sı örnek uygulama ve seçenekleri](media/speech-devices-sdk/java-sample-app-windows.png)

1. Yeni **konuşma dökümü** tanıtımı ' nı deneyin. **Oturum** > **başlatma**ile bir başlangıç yapın. Varsayılan olarak, herkes bir konudır. Ancak, katılımcının ses imzaları varsa proje klasörü `participants.properties` **hedefi/sınıflarında**bir dosyaya yerleştirilebilir. Ses imzasını oluşturmak için, [konuşmalar (SDK)](how-to-use-conversation-transcription-service.md)konusuna bakın.

   ![Demo konuşma dökümü uygulaması](media/speech-devices-sdk/cts-sample-app-windows.png)

## <a name="create-and-run-a-standalone-application"></a>Tek başına uygulama oluşturma ve çalıştırma

1. **Paket Gezgini**' nde projenize sağ tıklayın. Seçin **dışarı**. 

1. **Dışarı aktarma** penceresi görüntülenir. **Java** ' yı genişletin ve **Runiçin jar dosyasını** seçin ve ardından **İleri**' yi seçin.

   ![Dışarı aktarma penceresinin ekran görüntüsü](media/speech-devices-sdk/eclipse-export-windows.png) 

1. **ÇALıŞTıRıLABILIR jar dosya dışarı aktarma** penceresi görüntülenir. Uygulama için bir **dışarı aktarma hedefi** seçin ve ardından **son**' u seçin.
 
   ![Runfor JAR dosya dışarı aktarma ekran görüntüsü](media/speech-devices-sdk/eclipse-export-jar-windows.png)

1. Lütfen uygulama `kws.table`için bu `unimic_runtime.dll`dosyalar `pma.dll` gerektiğinden `Microsoft.CognitiveServices.Speech.extension.pma.dll` ,,, ve yukarıda seçilen hedef klasöre yerleştirin `participants.properties`.

1. Tek başına uygulamayı çalıştırmak için

     ```powershell
     java -jar SpeechDemo.jar
     ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm notlarını gözden geçirin](devices-sdk-release-notes.md)
