---
title: 'SDK C konuşmasını kullanarak konuşma niyetlerini nasıl tanır? #'
titleSuffix: Azure Cognitive Services
description: Bu kılavuzda, C# için Konuşma SDK'sını kullanarak konuşmaniyetlerini nasıl tanıyacağınızı öğrenirsiniz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: dapine
ms.openlocfilehash: 5d3c77c307739f9014010a592aa496a1cc83b333
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77120049"
---
# <a name="how-to-recognize-intents-from-speech-using-the-speech-sdk-for-c"></a>C için Konuşma SDK kullanarak konuşma niyetleri tanımak için nasıl #

Bilişsel Hizmetler [Konuşma SDK](speech-sdk.md) [dil anlama hizmeti (LUIS)](https://www.luis.ai/home) ile **niyet tanıma**sağlamak için entegre. Amaç, kullanıcının yapmak istediği herhangi bir şeydir: uçak rezervasyonu, hava durumuna bakma veya telefon etme. Kullanıcı kendisine hangi terim doğal geliyorsa onu kullanabilir. Luis, makine öğrenimini kullanarak kullanıcı isteklerini tanımladığınız niyetle eşler.

> [!NOTE]
> LUIS uygulaması tanımak istediğiniz amaçları ve varlıkları tanımlar. Konuşma hizmetini kullanan C# uygulamasından ayrıdır. Bu makalede "app" LUIS uygulaması anlamına gelirken "uygulama" da C# kodu anlamına gelmektedir.

Bu kılavuzda, cihazınızın mikrofonu aracılığıyla kullanıcı söylemlerinden niyet ler türeten bir C# konsolu uygulaması geliştirmek için Speech SDK'yı kullanırsınız. Şunları öğrenirsiniz:

> [!div class="checklist"]
>
> - Konuşma SDK'sı NuGet paketine başvuran bir Visual Studio projesi oluşturma
> - Konuşma yapılandırması oluşturun ve bir niyet tanıyın
> - LUIS app’iniz için modeli alma ve size gereken amaçları ekleme
> - Konuşma tanıma için dil belirtme
> - Dosyadan konuşma tanıma
> - Zaman uyumsuz, olay odaklı sürekli tanıma kullanma

## <a name="prerequisites"></a>Ön koşullar

Bu kılavuza başlamadan önce aşağıdaki öğelere sahip olduğundan emin olun:

- LUIS hesabı. [LUIS portalından](https://www.luis.ai/home) ücretsiz bir hesap alabilirsiniz.
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (herhangi bir baskı).

## <a name="luis-and-speech"></a>LUIS ve konuşma

LUIS, konuşmadaki amaçları tanımak için Konuşma hizmetiyle tümleştirilir. Konuşma hizmeti aboneliğine ihtiyacınız yoktur; LUIS yeterlidir.

LUIS üç tür anahtar kullanır:

| Anahtar türü  | Amaç                                               |
| --------- | ----------------------------------------------------- |
| Yazma | LUIS uygulamalarını programlı olarak oluşturmanıza ve değiştirmenize olanak tanır |
| Başlangıç   | LUIS uygulamanızı yalnızca metin kullanarak test etmenizi sağlar   |
| Uç Nokta  | Belirli bir LUIS uygulamasına erişimi yetkilendirme            |

Bu kılavuz için uç nokta anahtar türüne ihtiyacınız var. Bu kılavuz, [önceden oluşturulmuş Ev otomasyonu uygulamasını](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app) hızlı başlat'ı kullanarak oluşturabileceğiniz örnek Ev Otomasyonu LUIS uygulamasını kullanır. Kendi LUIS uygulamanızı oluşturduysanız, bunun yerine kullanabilirsiniz.

Bir LUIS uygulaması oluşturduğunuzda, LUIS otomatik olarak bir başlangıç anahtarı oluşturur, böylece uygulamayı metin sorgularını kullanarak test edebilirsiniz. Bu anahtar Konuşma hizmeti tümleştirmesini etkinleştirmez ve bu kılavuzla çalışmaz. Azure panosunda bir LUIS kaynağı oluşturun ve LUIS uygulamasına atayın. Bu kılavuz için ücretsiz abonelik katmanını kullanabilirsiniz.

Azure panosunda LUIS kaynağını oluşturduktan [sonra, LUIS portalına](https://www.luis.ai/home)giriş yapın, **Uygulamalarım** sayfasında uygulamanızı seçin ve ardından uygulamanın **Yönet** sayfasına geçin. Son olarak, kenar çubuğundaki **Anahtarlar ve Uç Noktaları'nı** seçin.

![LUIS portalı anahtarları ve uç nokta ayarları](media/sdk/luis-keys-endpoints-page.png)

**Anahtarlar ve Bitiş Noktası ayarları** sayfasında:

1. Kaynaklar ve **Anahtarlar** bölümüne gidin ve **kaynak ata'yı**seçin.
1. Uygulama iletişim **kutunuza bir anahtar atarken** aşağıdaki değişiklikleri yapın:

   - **Kiracı**altında, **Microsoft'u**seçin.
   - **Abonelik Adı**altında, kullanmak istediğiniz LUIS kaynağını içeren Azure aboneliğini seçin.
   - **Key'in**altında, uygulamayla birlikte kullanmak istediğiniz LUIS kaynağını seçin.

   Kısa süre içinde yeni abonelik sayfanın altındaki tabloda görüntülenir.

1. Panoya kopyalamak için anahtarın yanındaki simgeyi seçin. (İstediğiniz anahtarı kullanabilirsiniz.)

![LUIS app abonelik anahtarları](media/sdk/luis-keys-assigned.png)

## <a name="create-a-speech-project-in-visual-studio"></a>Visual Studio’da konuşma projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-the-code"></a>Kod ekleme

Ardından, projeye kod eklersiniz.

1. **Solution Explorer'dan**dosyayı **Program.cs.**

1. Dosyanın başındaki deyimbloğunu `using` aşağıdaki bildirimlerle değiştirin:

   [!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

1. Sağlanan `Main()` yöntemi aşağıdaki eşzamanlı eşdeğerle değiştirin:

   ```csharp
   public static async Task Main()
   {
       await RecognizeIntentAsync();
       Console.WriteLine("Please press Enter to continue.");
       Console.ReadLine();
   }
   ```

1. Burada gösterildiği gibi boş `RecognizeIntentAsync()`bir eşzamanlı yöntem oluşturun:

   ```csharp
   static async Task RecognizeIntentAsync()
   {
   }
   ```

1. Bu yeni yöntemin gövdesine şu kodu ekleyin:

   [!code-csharp[Intent recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

1. Bu yöntemdeki yer tutucuları aşağıda gösterildiği gibi LUIS abonelik anahtarınız, bölgeniz ve uygulama kimliğinizle değiştirin.

   | Yer tutucu | Şununla değiştir |
   | ----------- | ------------ |
   | `YourLanguageUnderstandingSubscriptionKey` | LUIS uç nokta anahtarınız. Yine, bu öğeyi "başlangıç anahtarı" değil, Azure panonunızdan almalısınız. Uygulamanızın **Keys ve Endpoints** sayfasında **(Yönet**altında) LUIS [portalında](https://www.luis.ai/home)bulabilirsiniz. |
   | `YourLanguageUnderstandingServiceRegion` | LUIS aboneliğinizin içinde bulunduğu bölgenin kısa tanımlayıcısı (örneğin, Batı ABD için `westus`). Bkz. [Bölgeler](regions.md). |
   | `YourLanguageUnderstandingAppId` | LUIS app kimliği. Uygulamanızın **Ayarlar** sayfasında [LUIS portalında](https://www.luis.ai/home)bulabilirsiniz. |

Yapılan bu değişikliklerle uygulamayı **(Control+Shift+B)** oluşturabilir**F5**ve çalıştırabilirsiniz. Size istendiğinde, bilgisayarınızın mikrofonuna "Işıkları kapatın" demeyi deneyin. Uygulama sonucu konsol penceresinde görüntüler.

Aşağıdaki bölümlerde kod açıklaması yer alır.

## <a name="create-an-intent-recognizer"></a>Amaç tanıyıcı oluşturma

İlk olarak, LUIS uç nokta anahtarınızdan ve bölgenizden bir konuşma yapılandırması oluşturmanız gerekir. Konuşma SDK'nın çeşitli yetenekleri için tanıyıcılar oluşturmak için konuşma yapılandırmalarını kullanabilirsiniz. Konuşma yapılandırmasının kullanmak istediğiniz aboneliği belirtmenin birden çok yolu vardır; burada, abonelik `FromSubscription`anahtarı nı ve bölgesini alan, kullanıyoruz.

> [!NOTE]
> Konuşma hizmeti aboneliğini değil, LUIS aboneliğinizin anahtarını ve bölgesini kullanın.

Daha sonra, `new IntentRecognizer(config)` kullanarak bir amaç tanıyıcı oluşturun. Yapılandırma hangi aboneliği kullanacağınızı zaten bildiğinden, tanıyanı oluştururken abonelik anahtarını ve bitiş noktasını yeniden belirtmeniz gerekmez.

## <a name="import-a-luis-model-and-add-intents"></a>LUIS modelini içeri aktarma ve amaç ekleme

Şimdi de, `LanguageUnderstandingModel.FromAppId()` kullanarak LUIS app’ten modeli içeri aktarın ve tanıyıcının `AddIntent()` yöntemi aracılığıyla tanınmasını istediğiniz LUIS amaçlarını ekleyin. Bu iki adım, kullanıcıların isteklerinde kullanabileceği sözcükleri belirterek konuşma tanımanın doğruluğunu güçlendirir. Uygulamanızda hepsini tanımanız gerekmiyorsa, uygulamanın tüm amaçlarını eklemeniz gerekmez.

Niyet eklemek için üç bağımsız değişken sağlamanız gerekir: LUIS modeli `model`(oluşturulmuş ve adlandırılmış), niyet adı ve bir niyet kimliği. Kimlik ve ad arasındaki fark aşağıda gösterilmiştir.

| `AddIntent()`&nbsp;Bağımsız değişken | Amaç |
| --------------------------- | ------- |
| `intentName` | LUIS app’te tanımlandığı şekliyle amacın adı. Bu değer, LUIS niyet adı ile tam olarak eşleşmelidir. |
| `intentID` | Konuşma SDK’sı tarafından tanınan amaca atanan kimlik. Bu değer ne isterseniz olabilir; LUIS uygulamasında tanımlandığı gibi niyet adına karşılık gelmek zorunda değildir. Örneğin, aynı kodla birden çok amaç işleniyorsa, bunlar için aynı kimliği kullanabilirsiniz. |

Ev Otomasyonu LUIS uygulamasının iki amacı vardır: biri cihazı açmak, diğeri de bir cihazı kapatmak için. Aşağıdaki satırlar bu amaçları tanıyıcıya ekler; `RecognizeIntentAsync()` yöntemindeki üç `AddIntent` satırını bu kodla değiştirin.

```csharp
recognizer.AddIntent(model, "HomeAutomation.TurnOff", "off");
recognizer.AddIntent(model, "HomeAutomation.TurnOn", "on");
```

Tek tek niyetler eklemek yerine, `AddAllIntents` bir modeldeki tüm amaçları tanıyankişiye eklemek için de yöntemi kullanabilirsiniz.

## <a name="start-recognition"></a>Tanımayı başlatma

Oluşturulan tanıyıcıyla ve eklenen amaçlarla tanıma başlayabilir. Konuşma SDK’sı hem tek seferlik hem de sürekli tanımayı destekler.

| Tanıma modu | Çağrılacak yöntemler | Sonuç |
| ---------------- | --------------- | ------ |
| Tek seferlik | `RecognizeOnceAsync()` | Tek konuşmadan sonra, tanınan amacı (varsa) döndürür. |
| Sürekli | `StartContinuousRecognitionAsync()`<br>`StopContinuousRecognitionAsync()` | Birden fazla söyleyiş tanır; sonuçlar kullanılabilir olduğunda olaylar `IntermediateResultReceived`(örneğin, ) yayır. |

Uygulama tek çekim modu kullanır `RecognizeOnceAsync()` ve böylece tanıma başlamak için çağırır. Sonuç, tanınan amaç hakkındaki bilgileri içeren `IntentRecognitionResult` nesnesidir. Aşağıdaki ifadeyi kullanarak LUIS JSON yanıtını ayıklarsınız:

```csharp
result.Properties.GetProperty(PropertyId.LanguageUnderstandingServiceResponse_JsonResult)
```

Uygulama JSON sonucunu ayrışturmuyor. Yalnızca konsol penceresinde JSON metnini görüntüler.

![Tek LUIS tanıma sonuçları](media/sdk/luis-results.png)

## <a name="specify-recognition-language"></a>Tanıma dilini belirtme

LUIS varsayılan olarak ABD İngilizcesindeki (`en-us`) amaçları tanır. Konuşma yapılandırmasının `SpeechRecognitionLanguage` özelliğine yerel ayar kodu atayarak başka dillerde de amaçları tanıyabilirsiniz. Örneğin, Almanca'daki niyetleri tanımak için tanıyıcı oluşturmadan önce uygulamamızı ekleyin. `config.SpeechRecognitionLanguage = "de-de";` Daha fazla bilgi için [LUIS dil desteğine](../LUIS/luis-language-support.md#languages-supported)bakın.

## <a name="continuous-recognition-from-a-file"></a>Dosyadan sürekli tanıma

Aşağıdaki kod, Konuşma SDK’sını kullanarak amaç tanımanın iki ek özelliğini gösterir. Önceden belirtildiği gibi, ilk özellik sonuçlar sağlandığında tanıyıcının olayları gösterdiği sürekli tanımadır. Ardından bu olaylar sağladığınız olay işleyicileri tarafından işlenebilir. Sürekli tanıma ile, tanıma `StartContinuousRecognitionAsync()` yerine tanıma başlatmak için `RecognizeOnceAsync()`tanıyan ın yöntemini çağırırsınız.

Diğer özellik, işlenecek konuşmayı içeren sesin WAV dosyasından okunmasıdır. Uygulama, niyet tanıyıcıoluştururken kullanılabilecek bir ses yapılandırması oluşturmayı içerir. Bu dosya 16 kHz örnekleme hızıyla tek kanallı (mono) olmalıdır.

Bu özellikleri denemek için `RecognizeIntentAsync()` yöntemin gövdesini silin veya yorumyapın ve yerine aşağıdaki kodu ekleyin.

[!code-csharp[Intent recognition by using events from a file](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

Daha önce olduğu gibi LUIS uç nokta anahtarınızı, bölgenizi ve app kimliğinizi dahil etmek, ayrıca Ev Otomasyonu amaçlarını eklemek için kodu düzeltin. Kaydedilmiş `whatstheweatherlike.wav` ses dosyanızın adını değiştirin. Ardından yapılayın, ses dosyasını yapı dizinine kopyalayın ve uygulamayı çalıştırın.

Örneğin, "Işıkları kapatın" diyorsanız, duraklatıyorsanız ve ardından kaydettiğiniz ses dosyasında "Işıkları açın" diyorsanız, konsol çıkışı aşağıdakilere benzer görünebilir:

![Ses dosyası LUIS tanıma sonuçları](media/sdk/luis-results-2.png)

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
**Örnekler/csharp/sharedcontent/console** klasöründe bu makaledeki kodu arayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı başlatma: Mikrofondan konuşmayı tanıma](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore)
