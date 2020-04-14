---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: dapine
ms.openlocfilehash: 36fb26fbc658e00c98f118d9b14347bd8392c8ae
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275427"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp)

[!INCLUDE [Audio input format](~/articles/cognitive-services/speech-service/includes/audio-input-format-chart.md)]

## <a name="open-your-project-in-visual-studio"></a>Görsel Stüdyo'da projenizi açın

İlk adım, visual studio'da projenizin açık olduğundan emin olmaktır.

1. Görsel Stüdyo 2019'u başlatın.
2. Projenizi yükleyin `Program.cs`ve açın.
3. <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/whatstheweatherlike.wav" download="whatstheweatherlike" target="_blank">Whatstheweatherlike.wav'ı <span class="docon docon-download x-hidden-focus"></span> </a> indirin ve projenize ekleyin.
    - *Whatstheweatherlike.wav* dosyasını dosyanın `Program.cs` yanına kaydedin.
    - Çözüm **Gezgini'nden** projeye sağ tıklayın, **Varolan öğeyi > ekle'yi**seçin.
    - *Whatstheweatherlike.wav* dosyasını seçin ve **ardından Ekle** düğmesini seçin.
    - Yeni eklenen dosyaya sağ tıklayın, **Özellikler'i**seçin.
    - Kopyayı Her **Zaman Kopyalamak** **için Çıktı Dizini** olarak değiştirin.

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim. 'li bir `RecognizeSpeechAsync()`async yöntemi oluşturduğunuza dikkat edin.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

namespace HelloWorld
{
    class Program
    {
        static async Task Main()
        {
            await RecognizeSpeechAsync();
        }

        static async Task RecognizeSpeechAsync()
        {
        }
    }
}
```

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `SpeechRecognizer` nesneyi başlatmadan önce, abonelik anahtarınızı ve abonelik bölgenizi kullanan bir yapılandırma oluşturmanız gerekir. Yönteme `RecognizeSpeechAsync()` bu kodu ekleyin.

> [!NOTE]
> Bu örnek, `FromSubscription()` `SpeechConfig`'yi oluşturmak için yöntemi kullanır. Kullanılabilir yöntemlerin tam listesi için [SpeechConfig Class'a](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)bakın.
> Konuşma SDK dil için en-us kullanarak tanıma varsayılan olacaktır, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../../how-to-specify-source-language.md) bakın.

```csharp
// Replace with your own subscription key and region identifier from here: https://aka.ms/speech/sdkregion
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="create-an-audio-configuration"></a>Ses yapılandırması oluşturma

Şimdi, ses dosyanızı `AudioConfig` işaret eden bir nesne oluşturmanız gerekir. Bu nesne, yönetilmeyen kaynakların doğru şekilde serbest bırakılmasını sağlamak için kullanılan bir deyimin içinde oluşturulur. Bu kodu yönteme, `RecognizeSpeechAsync()` Konuşma yapılandırmanızın hemen altına ekleyin.

```csharp
using (var audioInput = AudioConfig.FromWavFileInput("whatstheweatherlike.wav"))
{
}
```

## <a name="initialize-a-speechrecognizer"></a>Bir SpeechRecognizer'ı Başlatma

Şimdi, nesneyi `SpeechRecognizer` daha önce `SpeechConfig` oluşturulan `AudioConfig` nesneleri ve nesneleri kullanarak oluşturalım. Bu nesne, yönetilmeyen kaynakların doğru şekilde serbest bırakılmasını sağlamak için bir using deyiminin içinde de oluşturulur. Bu `RecognizeSpeechAsync()` kodu, nesnenizi ```AudioConfig``` saran kullanarak ifadenin içine ekleyin.

```csharp
using (var recognizer = new SpeechRecognizer(config, audioInput))
{
}
```

## <a name="recognize-a-phrase"></a>Bir tümceciği tanıma

Nesneden, `SpeechRecognizer` `RecognizeOnceAsync()` yöntemi aramalısınız. Bu yöntem, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar.

Using deyiminin içine şu kodu ekleyin:

```csharp
Console.WriteLine("Recognizing first result...");
var result = await recognizer.RecognizeOnceAsync();
```

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hataları) görüntüleme

Tanıma sonucu Konuşma hizmeti tarafından döndürüldüğünde, bununla ilgili bir şey yapmak istersiniz. Basit tutacağız ve sonucu konsola yazdıracağız.

Aşağıdaki kullanarak deyimi `RecognizeOnceAsync()`içinde, bu kodu ekleyin:

```csharp
switch (result.Reason)
{
    case ResultReason.RecognizedSpeech:
        Console.WriteLine($"We recognized: {result.Text}");
        break;
    case ResultReason.NoMatch:
        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled:
        var cancellation = CancellationDetails.FromResult(result);
        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

        if (cancellation.Reason == CancellationReason.Error)
        {
            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
            Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
            Console.WriteLine($"CANCELED: Did you update the subscription info?");
        }
        break;
}
```

## <a name="check-your-code"></a>Kodunuzu kontrol edin

Bu noktada, kodunuz şu şekilde görünmelidir:

```csharp
//
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE.md file in the project root for full license information.
//

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

namespace HelloWorld
{
    class Program
    {
        static async Task Main()
        {
            await RecognizeSpeechAsync();
        }

        static async Task RecognizeSpeechAsync()
        {
            var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

            using (var audioInput = AudioConfig.FromWavFileInput("whatstheweatherlike.wav"))
            using (var recognizer = new SpeechRecognizer(config, audioInput))
            {
                Console.WriteLine("Recognizing first result...");
                var result = await recognizer.RecognizeOnceAsync();

                switch (result.Reason)
                {
                    case ResultReason.RecognizedSpeech:
                        Console.WriteLine($"We recognized: {result.Text}");
                        break;
                    case ResultReason.NoMatch:
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                        break;
                    case ResultReason.Canceled:
                        var cancellation = CancellationDetails.FromResult(result);
                        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");
                
                        if (cancellation.Reason == CancellationReason.Error)
                        {
                            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                            Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
                            Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        }
                        break;
                }
            }
        }
    }
}
```

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Artık uygulamanızı oluşturmaya ve Konuşma hizmetini kullanarak konuşma tanımamızı test etmeye hazırsınız.

1. Kodu derle: *Visual Studio'nun*menü çubuğundan **Yapı** > **Çözüm'ü**seçin.
2. Uygulamanızı başlatın: Menü çubuğundan **Hata** > **Ayıklama'yı** seçin veya **F5 tuşuna**basın.
3. Tanımayı başlat: Ses dosyanız Konuşma hizmetine gönderilir, metin olarak yazılır ve konsolda işlenir.

   ```console
   Recognizing first result...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
