---
title: 'Quickstart: Ses dosyası, C# (.NET) içine synthesize konuşma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 4ccc68b38d98c332435e252877d258c8591aab8a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78925819"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Görsel Stüdyo'da projenizi açın

İlk adım, visual studio'da projenizin açık olduğundan emin olmaktır.

1. Görsel Stüdyo 2019'u başlatın.
2. Projenizi yükleyin `Program.cs`ve açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim. 'li bir `SynthesisToAudioFileAsync()`async yöntemi oluşturduğunuza dikkat edin.

````C#

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task SynthesisToAudioFileAsync()
        {
        }

        static void Main()
        {
            SynthesisToAudioFileAsync().Wait();
        }
    }
}

````

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `SpeechSynthesizer` nesneyi başlatmadan önce, abonelik anahtarınızı ve abonelik bölgenizi kullanan bir yapılandırma oluşturmanız gerekir. Yönteme `SynthesisToAudioFileAsync()` bu kodu ekleyin.

````C#
// Replace with your own subscription key and region identifier from here: https://aka.ms/speech/sdkregion
// The default language is "en-us".
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
````

## <a name="create-an-audio-configuration"></a>Ses yapılandırması oluşturma

Şimdi, ses dosyanızı ````AudioConfig```` işaret eden bir nesne oluşturmanız gerekir. Bu nesne, yönetilmeyen kaynakların doğru şekilde serbest bırakılmasını sağlamak için kullanılan bir deyimin içinde oluşturulur. Bu kodu yönteme, `SynthesisToAudioFileAsync()` Konuşma yapılandırmanızın hemen altına ekleyin.

````C#
var fileName = "helloworld.wav";
using (var fileOutput = AudioConfig.FromWavFileOutput(fileName))
{
}
````

## <a name="initialize-a-speechsynthesizer"></a>Bir SpeechSynthesizer'ı başlatma

Şimdi, nesneyi `SpeechSynthesizer` daha önce `SpeechConfig` oluşturulan `AudioConfig` nesneleri ve nesneleri kullanarak oluşturalım. Bu nesne, yönetilmeyen kaynakların doğru şekilde serbest bırakılmasını sağlamak için bir using deyiminin içinde de oluşturulur. Bu `SynthesisToAudioFileAsync()` kodu, nesnenizi ````AudioConfig```` saran kullanarak ifadenin içine ekleyin.

````C#
using (var synthesizer = new SpeechSynthesizer(config, fileOutput))
{
}
````

## <a name="synthesize-text-using-speaktextasync"></a>SpeakTextAsync kullanarak metni sentezleme

Nesneden, `SpeechSynthesizer` `SpeakTextAsync()` yöntemi aramalısınız. Bu yöntem, metninizi sese dönüştüren Konuşma hizmetine gönderir. Açıkça `SpeechSynthesizer` belirtilmemişse `config.VoiceName` varsayılan sesi kullanır.

Using deyiminin içine şu kodu ekleyin:
````C#
var text = "Hello world!";
var result = await synthesizer.SpeakTextAsync(text);
````

## <a name="check-for-errors"></a>Hataları denetleme

Konuşma hizmeti tarafından sentez sonucu döndürüldüğünde, metninizin başarıyla sentezlendiğinden emin olmalısınız.

Aşağıdaki kullanarak deyimi `SpeakTextAsync()`içinde, bu kodu ekleyin:
````C#
if (result.Reason == ResultReason.SynthesizingAudioCompleted)
{
    Console.WriteLine($"Speech synthesized to [{fileName}] for text [{text}]");
}
else if (result.Reason == ResultReason.Canceled)
{
    var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
    Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

    if (cancellation.Reason == CancellationReason.Error)
    {
        Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
        Console.WriteLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
        Console.WriteLine($"CANCELED: Did you update the subscription info?");
    }
}
````

## <a name="check-your-code"></a>Kodunuzu kontrol edin

Bu noktada, kodunuz şu şekilde görünmelidir:

````C#
//
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE.md file in the project root for full license information.
//

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task SynthesisToAudioFileAsync()
        {
            var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

            var fileName = "helloworld.wav";
            using (var fileOutput = AudioConfig.FromWavFileOutput(fileName))
            {
                using (var synthesizer = new SpeechSynthesizer(config, fileOutput))
                {
                    var text = "Hello world!";
                    var result = await synthesizer.SpeakTextAsync(text);

                    if (result.Reason == ResultReason.SynthesizingAudioCompleted)
                    {
                        Console.WriteLine($"Speech synthesized to [{fileName}] for text [{text}]");
                    }
                    else if (result.Reason == ResultReason.Canceled)
                    {
                        var cancellation = SpeechSynthesisCancellationDetails.FromResult(result);
                        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                        if (cancellation.Reason == CancellationReason.Error)
                        {
                            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                            Console.WriteLine($"CANCELED: ErrorDetails=[{cancellation.ErrorDetails}]");
                            Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        }
                    }
                }
            }
        }

        static void Main()
        {
            SynthesisToAudioFileAsync().Wait();
        }
    }
}
````

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Artık uygulamanızı oluşturmaya ve Konuşma hizmetini kullanarak konuşma sentezimizi test etmeye hazırsınız.

1. **Kodu derle** - Visual Studio'nun menü çubuğundan **Build** > **Build Solution'ı**seçin.
2. **Uygulamanızı başlatın** - Menü çubuğundan **Hata** > **Ayıklama'yı** seçin veya **F5 tuşuna**basın.
3. **Sentezi başlat** - Metniniz konuşmaya dönüştürülür ve belirtilen ses verilerine kaydedilir.

   ```text
   Speech synthesized to [helloworld.wav] for text [Hello world!]
   ```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Ses Oluşturma](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini kaydetme](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
