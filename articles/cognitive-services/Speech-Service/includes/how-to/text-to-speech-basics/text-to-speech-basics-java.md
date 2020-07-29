---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2020
ms.custom: devx-track-java
ms.author: trbye
ms.openlocfilehash: f83c2078358c7f35c5de45a5ac17d43750457562
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87374921"
---
## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir. Platformunuza bağlı olarak, aşağıdaki yönergeleri kullanın:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=jre&pivots=programming-language-java" target="_blank">Java çalışma zamanı<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=android&pivots=programming-language-java" target="_blank">Android<span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="import-dependencies"></a>Bağımlılıkları içeri aktar

Bu makaledeki örnekleri çalıştırmak için, betiğinizin en üstüne aşağıdaki içeri aktarma deyimlerini ekleyin.

```java
import com.microsoft.cognitiveservices.speech.AudioDataStream;
import com.microsoft.cognitiveservices.speech.SpeechConfig;
import com.microsoft.cognitiveservices.speech.SpeechSynthesizer;
import com.microsoft.cognitiveservices.speech.SpeechSynthesisOutputFormat;
import com.microsoft.cognitiveservices.speech.SpeechSynthesisResult;
import com.microsoft.cognitiveservices.speech.audio.AudioConfig;

import java.io.*;
import java.util.Scanner;
```

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir oluşturmanız gerekir [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable) . Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir.

> [!NOTE]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

Şunları başlatabilmeniz için birkaç yol vardır [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable) :

* Abonelik ile: bir anahtarı ve ilişkili bölgeyi geçirin.
* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

Bu örnekte, bir [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable) abonelik anahtarı ve bölgesi kullanarak bir oluşturun. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın. Ayrıca, bu makalenin geri kalanı için kullanabileceğiniz, farklı özelleştirmeler için değiştirdiğiniz bazı temel ortak kod oluşturabilirsiniz.

```java
public class Program 
{
    public static void main(String[] args) {
        SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    }
}
```

## <a name="synthesize-speech-to-a-file"></a>Konuşmayı bir dosyaya sentezleştirme

Sonra, [`SpeechSynthesizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechsynthesizer?view=azure-java-stable) metin okuma dönüştürmeleri yürüten ve hoparlörlerde, dosyalarda veya diğer çıkış akışlarına giden çıktıları yürüten bir nesne oluşturun. , [`SpeechSynthesizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechsynthesizer?view=azure-java-stable) [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable) Önceki adımda oluşturulan nesneyi params olarak kabul eder ve [`AudioConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-java-stable) Çıkış sonuçlarının nasıl işleneceğini belirten bir nesnedir.

Başlamak için, `AudioConfig` `.wav` statik işlevi kullanarak çıktıyı bir dosyaya otomatik olarak yazmak üzere bir oluşturun `fromWavFileOutput()` .

```java
public static void main(String[] args) {
    SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    AudioConfig audioConfig = AudioConfig.fromWavFileOutput("path/to/write/file.wav");
}
```

Ardından, `SpeechSynthesizer` `speechConfig` nesne ve nesneyi params olarak geçirerek bir örnek oluşturun `audioConfig` . Daha sonra, konuşma birleştirmenin yürütülmesi ve bir dosyaya yazılması, `SpeakText()` bir metin dizesiyle çalışırken basittir.

```java
public static void main(String[] args) {
    SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    AudioConfig audioConfig = AudioConfig.fromWavFileOutput("path/to/write/file.wav");

    SpeechSynthesizer synthesizer = new SpeechSynthesizer(speechConfig, audioConfig);
    synthesizer.SpeakText("A simple test to write to a file.");
}
```

Programı çalıştırın ve belirttiğiniz konuma bir sentezlenmiş `.wav` dosya yazılır. Bu, en temel kullanımın iyi bir örneğidir, ancak sonraki bir deyişle, özel senaryolarla çalışmaya yönelik bellek içi akış olarak çıktıyı özelleştirmeye ve çıkış yanıtını işlemeye baktığınızın ardından.

## <a name="synthesize-to-speaker-output"></a>Konuşmacı çıktısına sentezleştirme

Bazı durumlarda, doğrudan sentezlenmiş konuşmayı doğrudan bir konuşmacının çıktısını almak isteyebilirsiniz. Bunu yapmak için `AudioConfig` static işlevini kullanarak örneğini oluşturun `fromDefaultSpeakerOutput()` . Bu, geçerli etkin çıkış cihazına çıkış verir.

```java
public static void main(String[] args) {
    SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    AudioConfig audioConfig = AudioConfig.fromDefaultSpeakerOutput();

    SpeechSynthesizer synthesizer = new SpeechSynthesizer(speechConfig, audioConfig);
    synthesizer.SpeakText("Synthesizing directly to speaker output.");
}
```

## <a name="get-result-as-an-in-memory-stream"></a>Bellek içi akış olarak sonuç alın

Konuşma uygulaması geliştirmede birçok senaryo için, büyük olasılıkla bir dosyaya doğrudan yazmak yerine, sonuçta elde edilen ses verilerinin bellek içi akış olarak olması gerekir. Bu, aşağıdakiler dahil olmak üzere özel davranış oluşturmanıza izin verir:

* Özel aşağı akış Hizmetleri için arama yapabilecek bir akış olarak ortaya çıkan bayt dizisinin soyut olması.
* Sonucu diğer API 'leri veya hizmetleriyle tümleştirin.
* Ses verilerini değiştirin, özel üstbilgiler yazın `.wav` vb.

Önceki örnekte bu değişikliği yapmak basittir. İlk `AudioConfig` olarak, daha fazla denetim için çıkış davranışını bu noktadan el ile yöneteceği için bloğu kaldırın. Sonra `null` `AudioConfig` Oluşturucu içinde öğesine geçirin `SpeechSynthesizer` . 

> [!NOTE]
> `null` `AudioConfig` Yukarıdaki konuşmacı çıktısı örneğinde olduğu gibi değil, için geçirme, geçerli etkin çıkış cihazında varsayılan olarak sesi oynamaz.

Bu kez, sonucu bir [`SpeechSynthesisResult`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechsynthesisresult?view=azure-java-stable) değişkene kaydedersiniz. `SpeechSynthesisResult.getAudioData()`İşlevi, `byte []` Çıkış verilerinden bir döndürür. Bu ile el ile çalışabilirsiniz `byte []` veya [`AudioDataStream`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audiodatastream?view=azure-java-stable) bellek içi akışı yönetmek için sınıfını kullanabilirsiniz. Bu örnekte, `AudioDataStream.fromResult()` sonuçtan bir akış almak için static işlevini kullanırsınız.

```java
public static void main(String[] args) {
    SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    SpeechSynthesizer synthesizer = new SpeechSynthesizer(speechConfig, null);
    
    SpeechSynthesisResult result = synthesizer.SpeakText("Getting the response as an in-memory stream.");
    AudioDataStream stream = AudioDataStream.fromResult(result);
    System.out.print(stream.getStatus());
}
```

Buradan, sonuçta elde edilen nesneyi kullanarak özel davranışları uygulayabilirsiniz `stream` .

## <a name="customize-audio-format"></a>Ses biçimini Özelleştir

Aşağıdaki bölümde aşağıdakiler dahil olmak üzere ses çıkış özniteliklerinin nasıl özelleştirileceği gösterilmektedir:

* Ses dosyası türü
* Örnek hız
* Bit derinliği

Ses biçimini değiştirmek için `setSpeechSynthesisOutputFormat()` nesnesi üzerinde işlevini kullanın `SpeechConfig` . Bu işlev `enum` [`SpeechSynthesisOutputFormat`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-java-stable) , çıkış biçimini seçmek için kullandığınız türünde bir tür bekliyor. Kullanılabilir [Ses biçimlerinin listesi](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-dotnet) için başvuru belgelerine bakın.

Gereksinimlerinize bağlı olarak farklı dosya türleri için çeşitli seçenekler vardır. Tanımına göre, gibi ham biçimlerin `Raw24Khz16BitMonoPcm` Ses üst bilgilerini içermediğini unutmayın. Ham biçimleri yalnızca aşağı akış uygulamanızın bir ham Bitstream kodunu çözemediğini veya bit derinliğine, örnek hızına, kanal sayısına ve vb. göre el ile üstbilgileri oluşturmayı planlıyorsanız kullanın.

Bu örnekte, `Riff24Khz16BitMonoPcm` nesnesi üzerinde ayarını yaparak Yüksek uygunluğa sahip bir biçim belirtirsiniz `SpeechSynthesisOutputFormat` `SpeechConfig` . Önceki bölümdeki örneğe benzer şekilde, [`AudioDataStream`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audiodatastream?view=azure-java-stable) sonucun bellek içi akışını elde etmek ve ardından bir dosyaya yazmak için öğesini kullanırsınız.

```java
public static void main(String[] args) {
    SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

    // set the output format
    speechConfig.setSpeechSynthesisOutputFormat(SpeechSynthesisOutputFormat.Riff24Khz16BitMonoPcm);

    SpeechSynthesizer synthesizer = new SpeechSynthesizer(speechConfig, null);
    SpeechSynthesisResult result = synthesizer.SpeakText("Customizing audio output format.");
    AudioDataStream stream = AudioDataStream.fromResult(result);
    stream.saveToWavFile("path/to/write/file.wav");
}
```

Programınızı yeniden çalıştırmak, `.wav` belirtilen yola bir dosya yazacak.

## <a name="use-ssml-to-customize-speech-characteristics"></a>SSML 'yi kullanarak konuşma özelliklerini özelleştirme

Konuşma sen, biçimlendirme dili (SSML), isteklerinizi bir XML şemasından göndererek, metin okuma çıktılarından oluşan sıklık, Söyleniş, konuşma oranı, hacim ve daha fazlasını ayarlamanıza olanak sağlar. Bu bölümde, birkaç pratik kullanım örneği gösterilmektedir, ancak daha ayrıntılı bir kılavuz için bkz. [SSML nasıl yapılır makalesi](../../../speech-synthesis-markup.md).

Özelleştirme için SSML 'yi kullanmaya başlamak için, sese geçiş yapan basit bir değişiklik yaparsınız.
İlk olarak, bu örnekte, kök proje dizininizde SSML yapılandırması için yeni bir XML dosyası oluşturun `ssml.xml` . Kök öğesi her zaman `<speak>` ve metni bir öğe içinde sarmalama, `<voice>` param kullanarak sesi değiştirmenize olanak sağlar `name` . Bu örnek, sesi erkek Ingilizce (UK) sesine dönüştürür. Bu sesin, **sinir** sesinden farklı fiyatlandırmaya ve kullanılabilirliğine sahip **Standart** bir ses olduğunu unutmayın. Desteklenen **Standart** seslerin [tam listesine](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#standard-voices) bakın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    When you're on the motorway, it's a good idea to use a sat-nav.
  </voice>
</speak>
```

Ardından, XML dosyanıza başvurmak için konuşma sensıs isteğini değiştirmeniz gerekir. İstek genellikle aynıdır, ancak işlevini kullanmak yerine `SpeakText()` kullanın `SpeakSsml()` . Bu işlev bir XML dizesi bekliyor, bu nedenle ilk olarak bir XML dosyası yüklemek için bir işlev oluşturun ve bunu bir dize olarak döndürün.

```java
private static String xmlToString(String filePath) {
    File file = new File(filePath);
    StringBuilder fileContents = new StringBuilder((int)file.length());

    try (Scanner scanner = new Scanner(file)) {
        while(scanner.hasNextLine()) {
            fileContents.append(scanner.nextLine() + System.lineSeparator());
        }
        return fileContents.toString().trim();
    } catch (FileNotFoundException ex) {
        return "File not found.";
    }
}
```

Buradan, sonuç nesnesi önceki örneklerle tamamen aynıdır.

```java
public static void main(String[] args) {
    SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    SpeechSynthesizer synthesizer = new SpeechSynthesizer(speechConfig, null);

    String ssml = xmlToString("ssml.xml");
    SpeechSynthesisResult result = synthesizer.SpeakSsml(ssml);
    AudioDataStream stream = AudioDataStream.fromResult(result);
    stream.saveToWavFile("path/to/write/file.wav");
}
```

Çıktı işe yarar, ancak daha doğal bir şekilde ses duymayı sağlayacak birkaç basit ek değişiklik vardır. Genel konuşma hızı çok hızlıdır, bu nedenle bir `<prosody>` etiket ekleyeceğiz ve varsayılan orandaki **%90** ' a kadar olan hızı azaltabilirsiniz. Ayrıca, tümcede virgülden sonraki duraklama, çok kısa ve doğal olmayan bir soun. Bu sorunu onarmak için `<break>` konuşmayı geciktirmek üzere bir etiket ekleyin ve zaman param ' ı **200ms**olarak ayarlayın. Bu özelleştirmelerin çıktıyı nasıl etkilediğine bakmak için senmu yeniden çalıştırın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    <prosody rate="0.9">
      When you're on the motorway,<break time="200ms"/> it's a good idea to use a sat-nav.
    </prosody>
  </voice>
</speak>
```

## <a name="neural-voices"></a>Sinir sesleri

Sinir sesleri, derin sinir Networks tarafından desteklenen konuşma senime algoritmalardır. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

Bir sinir sesinize geçiş yapmak için, `name` [sinir Voice seçeneklerinden](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices)birini değiştirin. Ardından, için bir XML ad alanı ekleyin `mstts` ve metninizi etikete sarın `<mstts:express-as>` . `style`Konuşma stilini özelleştirmek için param 'ı kullanın. Bu örnek kullanır `cheerful` , ancak `customerservice` `chat` konuşma stilindeki farkı görmek için veya olarak ayarlamayı deneyin.

> [!IMPORTANT]
> Sinir sesleri **yalnızca** *Doğu ABD*, *Güney Doğu Asya*ve *Batı Avrupa* bölgelerinde oluşturulan konuşma kaynakları için desteklenir.

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="en-US-AriaNeural">
    <mstts:express-as style="cheerful">
      This is awesome!
    </mstts:express-as>
  </voice>
</speak>
```
