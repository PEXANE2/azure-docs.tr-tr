---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 02/20/2020
ms.author: trbye
ms.openlocfilehash: b22ba1df6eeddfaf04d11e542acb4f2b8ab00d76
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83673127"
---
Bu hızlı başlangıçta, konuşma özellikli bir ürün oluşturmak veya bir konuşma [dökümü](../conversation-transcription-service.md) cihazı olarak kullanmak için Windows Için konuşma cihazları SDK 'sını nasıl kullanacağınızı öğreneceksiniz. Konuşma için yalnızca [Azure Kinect dk](https://azure.microsoft.com/services/kinect-dk/) desteklenir. Diğer konuşma için, bir Microphone dizi geometrisi sağlayan doğrusal mik dizileri kullanın.

Uygulama, konuşma SDK paketiyle oluşturulmuştur ve 64 bit Windows üzerinde Java IDE tutulma (v4). 64 bit Java 8 çalışma zamanı ortamında (JRE) çalışır.

Bu kılavuzda, bir konuşma hizmeti kaynağına sahip bir Azure bilişsel [Hizmetler](../get-started.md) hesabı gerekir. Bir hesabınız yoksa, abonelik anahtarı almak için [ücretsiz deneme sürümünü](https://azure.microsoft.com/try/cognitive-services/) kullanabilirsiniz.

[Örnek uygulamanın](https://aka.ms/sdsdk-download-JRE) kaynak kodu, konuşma cihazları SDK 'sına dahildir. [GitHub 'da da kullanılabilir](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

* İşletim sistemi: 64-bit Windows
* [Azure Kinect dk](https://azure.microsoft.com/services/kinect-dk/) gibi bir mikrofon dizisi
* [Tutulma Java IDE](https://www.eclipse.org/downloads/)
* Yalnızca [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) veya [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html) .
* [Microsoft Visual C++ Yeniden Dağıtılabilir](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](../get-started.md).
* Java için [konuşma cihazları SDK 'sının](https://aka.ms/sdsdk-download-JRE) en son sürümünü indirin ve. zip dosyasını çalışma dizininize ayıklayın.
   > [!NOTE]
   > Bu hızlı başlangıçta, uygulamanın C:\SDSDK\JRE-Sample-Release 'e ayıklanacağı varsayılmaktadır

Konuşma dökümü şu anda yalnızca "en-US" ve "zh-CN" için kullanılabilir ve "merkezileştirme" ve "eastaya" bölgelerinde mevcuttur. Konuşma dökümünü kullanmak için bu bölgelerden birinde bir konuşma anahtarınız olmalıdır.

Hedefleri kullanmayı planlıyorsanız, bir [Language Understanding hizmeti (LUA)](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription) aboneliğine ihtiyacınız olacaktır. Lua ve amaç tanıma hakkında daha fazla bilgi edinmek için bkz. [lusıs, C# ile konuşma amaçlarını tanıma](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-recognize-intents-from-speech-csharp). Bu uygulama için [Örnek BIR lusıs modeli](https://aka.ms/sdsdk-luis) vardır.

## <a name="create-and-configure-the-project"></a>Projeyi oluşturma ve yapılandırma

1. Eclipse’i başlatın.

1. **Çakışan Küreler IDE başlatıcısı**' nda, **çalışma alanı** alanına yeni bir çalışma alanı dizininin adını girin. Ardından **Başlat**’ı seçin.

   ![Eclipse Başlatıcısı ekran görüntüsü](../media/speech-devices-sdk/eclipse-launcher.png)

1. Çok geçmeden Eclipse IDE ana penceresi görüntülenir. Varsa, Hoş Geldiniz ekranını kapatın.

1. Çakışan Küreler menü çubuğunda **Dosya**  >  **Yeni**  >  **Java projesi**' ni seçerek yeni bir proje oluşturun. Kullanılabilir değilse **Proje** ' yi ve ardından **Java projesi**' ni seçin.

1. **Yeni Java proje** Sihirbazı başlatılır. Örnek projenin konumuna **gözatın** . **Son**' u seçin.

   ![Yeni Java Projesi sihirbazının ekran görüntüsü](../media/speech-devices-sdk/eclipse-new-java-project.png)

1. **Paket Gezgini**' nde projenize sağ tıklayın. **Configure**  >  Bağlam menüsünden**Maven projesine dönüştürmeyi** Yapılandır ' ı seçin. **Son**' u seçin.

   ![Paket gezgininin ekran görüntüsü](../media/speech-devices-sdk/eclipse-convert-to-maven.png)

1. pom.xml dosyasını açıp düzenleyin.

    Dosyanın sonunda, kapanış etiketinden önce, `</project>` `repositories` ve `dependencies` öğelerini, burada gösterildiği gibi oluşturun ve `version` geçerli sürümünüzle eşleştiğinden emin olun:
    ```xml
    <repositories>
         <repository>
             <id>maven-cognitiveservices-speech</id>
             <name>Microsoft Cognitive Services Speech Maven Repository</name>
             <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
         </repository>
    </repositories>
 
    <dependencies>
        <dependency>
             <groupId>com.microsoft.cognitiveservices.speech</groupId>
             <artifactId>client-sdk</artifactId>
             <version>1.12.0</version>
        </dependency>
    </dependencies>
   ```

1. **Windows-x64** Içeriğini Java proje konumuna kopyalayın, örneğin **C:\sdsdk\jre-Sample-Release**

1. `kws.table` `participants.properties` `Microsoft.CognitiveServices.Speech.extension.pma.dll` Proje klasörü **target\classes** Kopyala

## <a name="configure-the-sample-application"></a>Örnek uygulamayı yapılandırma

1. Konuşma abonelik anahtarınızı kaynak koda ekleyin. Amaç tanımayı denemek istiyorsanız, [Language Understanding hizmeti](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) abonelik anahtarınızı ve uygulama kimliğinizi da ekleyin.

   Konuşma ve LUSıS için, bilgileriniz şu şekilde olur `FunctionsList.java` :

   ```java
    // Subscription
    private static String SpeechSubscriptionKey = "<enter your subscription info here>";
    private static String SpeechRegion = "westus"; // You can change this if your speech region is different.
    private static String LuisSubscriptionKey = "<enter your subscription info here>";
    private static String LuisRegion = "westus2"; // you can change this, if you want to test the intent, and your LUIS region is different.
    private static String LuisAppId = "<enter your LUIS AppId>";
   ```

   Konuşma dökümünü kullanıyorsanız, konuşma anahtarınız ve bölge bilgilerinizin de şu şekilde olması gerekir `Cts.java` :

   ```java
    private static final String CTSKey = "<Conversation Transcription Service Key>";
    private static final String CTSRegion="<Conversation Transcription Service Region>";// Region may be "centralus" or "eastasia"
   ```

1. Default anahtar sözcüğü (anahtar sözcüğü) "bilgisayar" dır. "Makine" veya "yardımcı" gibi, belirtilen diğer anahtar sözcüklerden birini de deneyebilirsiniz. Bu alternatif anahtar sözcüklerin kaynak dosyaları, anahtar sözcük klasöründe konuşma cihazları SDK 'snda bulunur. Örneğin, `C:\SDSDK\JRE-Sample-Release\keyword\Computer` "bilgisayar" anahtar sözcüğü için kullanılan dosyaları içerir.

    > [!TIP]
    > Ayrıca, [özel bir anahtar sözcük oluşturabilirsiniz](../speech-devices-sdk-create-kws.md).

    Yeni bir anahtar sözcük kullanmak için ' de aşağıdaki satırı güncelleştirin `FunctionsList.java` ve anahtar sözcüğünü uygulamanıza kopyalayın. Örneğin, anahtar sözcük paketinden ' Machine ' anahtar sözcüğünü kullanmak için `machine.zip` :

   * `kws.table`Dosyayı ZIP paketinden proje klasörü **hedefi/sınıfları**içine kopyalayın.
   * `FunctionsList.java`Anahtar sözcük adıyla güncelleştirin:

     ```java
     private static final String Keyword = "Machine";
     ```

## <a name="run-the-sample-application-from-eclipse"></a>Örnek uygulamayı tutulma 'dan çalıştırma

1. Çakışan Küreler menü çubuğundan **Run**  >  **Çalıştır**  >  **Java uygulaması**' nı çalıştırın. Sonra **Functionslist** ve **Tamam**' ı seçin.

   ![Java uygulaması Seç ekranının ekran görüntüsü](../media/speech-devices-sdk/eclipse-run-sample.png)

1. Konuşma cihazları SDK örnek uygulaması başlatılır ve aşağıdaki seçenekleri görüntüler:

   ![Örnek konuşma cihazları SDK örnek uygulama ve Seçenekler](../media/speech-devices-sdk/java-sample-app-windows.png)

1. Yeni **konuşma dökümü** tanıtımı ' nı deneyin. **Oturum**başlatma ile bir başlangıç yapın  >  **Start**. Varsayılan olarak, herkes bir konudır. Ancak, katılımcının ses imzaları varsa `participants.properties` Proje klasörü **hedefi/sınıflarında**bir dosyaya yerleştirilebilir. Ses imzasını oluşturmak için, [konuşmalar (SDK)](../how-to-use-conversation-transcription-service.md)konusuna bakın.

   ![Demo konuşma dökümü uygulaması](../media/speech-devices-sdk/cts-sample-app-windows.png)

## <a name="create-and-run-a-standalone-application"></a>Tek başına uygulama oluşturma ve çalıştırma

1. **Paket Gezgini**' nde projenize sağ tıklayın. **Dışarı aktar**' ı seçin.

1. **Dışarı aktarma** penceresi görüntülenir. **Java** ' yı genişletin ve **Runiçin jar dosyasını** seçin ve ardından **İleri**' yi seçin.

   ![Dışarı aktarma penceresinin ekran görüntüsü](../media/speech-devices-sdk/eclipse-export-windows.png)

1. **ÇALıŞTıRıLABILIR jar dosya dışarı aktarma** penceresi görüntülenir. Uygulama için bir **dışarı aktarma hedefi** seçin ve ardından **son**' u seçin.

   ![Runfor JAR dosya dışarı aktarma ekran görüntüsü](../media/speech-devices-sdk/eclipse-export-jar-windows.png)

1. Lütfen `kws.table` `participants.properties` `unimic_runtime.dll` uygulama için bu dosyalar gerektiğinden,,, `pma.dll` ve `Microsoft.CognitiveServices.Speech.extension.pma.dll` yukarıda seçilen hedef klasöre yerleştirin.

1. Tek başına uygulamayı çalıştırmak için

   ```powershell
   java -jar SpeechDemo.jar
   ```
