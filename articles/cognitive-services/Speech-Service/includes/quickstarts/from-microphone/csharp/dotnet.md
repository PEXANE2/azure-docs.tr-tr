---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: dapine
ms.openlocfilehash: 4e960a2dee87db77647ba1596a9c22138c64d15c
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80671783"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure Konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

## <a name="open-your-project-in-visual-studio"></a>Görsel Stüdyo'da projenizi açın

İlk adım, visual studio'da projenizin açık olduğundan emin olmaktır.

1. Görsel **Stüdyo 2019'u**başlatın.
2. Projenizi yükleyin ve *Program.cs*açın.

## <a name="source-code"></a>Kaynak kod

*Program.cs* dosyasının içeriğini aşağıdaki C# koduyla değiştirin.

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

Artık uygulamanızı yeniden oluşturmaya ve Konuşma hizmetini kullanarak konuşma tanıma işlevini test etmeye hazırsınız.

1. **Kodu derle** - Visual Studio'nun menü çubuğundan **Build** > **Build Solution'ı**seçin.
2. **Uygulamanızı başlatın** - Menü çubuğundan **Hata** > **Ayıklama'yı** seçin veya <kbd>F5 tuşuna</kbd>basın.
3. **Tanımayı başlatın** - İngilizce bir ifade konuşmanızı ister. Konuşmanız Konuşma hizmetine gönderilir, metin olarak yazılır ve konsolda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](../footer.md)]
