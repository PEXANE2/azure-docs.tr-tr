---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.custom: devx-track-csharp
ms.openlocfilehash: 63fc699cfd2f02a322f71324519b2f0b5b22234b
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88926476"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure konuşma kaynağı oluşturma <span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio 'da açın

İlk adım, projenizin Visual Studio 'da açık olduğundan emin olmak.

1. **Visual Studio 2019**' i başlatın.
2. Projenizi yükleyin ve *program.cs*açın.

## <a name="source-code"></a>Kaynak kod

*Program.cs* dosyasının Içeriğini aşağıdaki C# kodu ile değiştirin.

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace Speech.Recognition
{
    class Program
    {
        static async Task Main()
        {
            await RecognizeSpeechAsync();

            Console.WriteLine("Please press any key to continue...");
            Console.ReadLine();
        }

        static async Task RecognizeSpeechAsync()
        {
            var config =
                SpeechConfig.FromSubscription(
                    "YourSubscriptionKey",
                    "YourServiceRegion");

            using var recognizer = new SpeechRecognizer(config);
            
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
```

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Kod açıklaması

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Uygulama oluşturma ve çalıştırma

Artık uygulamanızı yeniden oluşturmaya ve konuşma tanıma işlevini konuşma hizmetini kullanarak test etmeye hazır olursunuz.

1. **Kodu derleyin** -Visual Studio menü **çubuğundan derleme**  >  **Build Solution**' ı seçin.
2. **Uygulamanızı başlatın** -menü çubuğundan hata **Debug**  >  **ayıklamayı Başlat hata** Ayıkla ' yı seçin veya <kbd>F5</kbd>tuşuna basın.
3. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
