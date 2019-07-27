---
title: Konuşma SDK 'Sı konuşma hizmeti ile çok katılımcının konuşmalar
titleSuffix: Azure Cognitive Services
description: Konuşma ile konuşmayı, konuşma SDK 'sını kullanarak nasıl kullanacağınızı öğrenin. , C#Ve C++Java için kullanılabilir.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: jhakulin
ms.openlocfilehash: 8c4ecc017d058900297f2220173e064700e7051b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559458"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile çok katılımcı konuşmaları

Konuşma SDK 'sının konuşma **API 'si** , veya `PullStream` `PushStream`kullanarak konuşma Hizmetleri için ses akışı yaparak katılımcıları ekleme, kaldırma ve tanımanıza olanak sağlayan toplantılar/konuşmalar sağlar.

## <a name="limitations"></a>Sınırlamalar

* Konuşma dökümü Windows, Linux ve Android C++'de C#,, ve Java için desteklenir.
* ROOBO DevKit, konuşmacı kimliği için verimli bir şekilde kullanılabilecek dairesel çok mikrofonli bir dizi sağlayan konuşma dökümlerini oluşturmaya yönelik desteklenen donanım ortamıdır. [Daha fazla bilgi için bkz. konuşma CIHAZLARı SDK](speech-devices-sdk.md).
* Konuşma dökümü için konuşma SDK 'Sı desteği, 16 bit 16 kHz PCM ses içeren sekiz kanallı ses çekme ve gönderme modu akışlarının kullanımıyla sınırlandırılmıştır.
* Şu bölgelerde "en-US" ve "zh-CN" dillerinde konuşma dökümü şu anda kullanılabilir: merkezileştirme ve eastasıu.

## <a name="prerequisites"></a>Önkoşullar

* [Konuşma SDK 'Sı ile konuşmayı metne nasıl kullanacağınızı öğrenin.](quickstart-csharp-dotnet-windows.md)
* [Konuşma deneme aboneliğinizi alın.](https://azure.microsoft.com/try/cognitive-services/)
* Konuşma SDK sürümü 1.5.1 veya üzeri gereklidir.

## <a name="create-voice-signatures-for-participants"></a>Katılımcılar için ses imzaları oluşturma

İlk adım, konuşma katılımcıları için ses imzaları oluşturmaktır. Verimli konuşmacı tanımlaması için ses imzaları oluşturma gerekir.

### <a name="requirements-for-input-wave-file"></a>Giriş dalga dosyası gereksinimleri

* Ses imzaları oluşturmak için giriş sesi dalga dosyası 16 bit örneklerde, 16 kHz örnek hızında ve tek kanallı (mono) biçimde olmalıdır.
* Her ses örneği için önerilen uzunluk 30 saniye ile iki dakika arasındadır.

Aşağıdaki örnek, [REST API kullanarak](https://aka.ms/cts/signaturegenservice) sesli imza oluşturmanın iki farklı yolunu göstermektedir C#:

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribing-conversations"></a>Bing konuşmaları

Birden çok katılımcı ile konuşmalar yapmak için `ConversationTranscriber` , veya `PushAudioInputStream`kullanarak `PullAudioInputStream` konuşma oturumu için oluşturulan `AudioConfig` nesneyle ilişkili nesne ve akış sesi oluşturun.

Adında `MyConversationTranscriber`bir konuşma tiontranber sınıfınız olduğunu varsayalım. Kodunuz şöyle görünebilir:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Conversation;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and region.
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file.
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            // Creates a conversation transcriber using audio stream input.
            using (var transcriber = new ConversationTranscriber(config, audioInput))
            {
                // Subscribes to events.
                transcriber.Recognizing += (s, e) =>
                {
                    Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                };

                transcriber.Recognized += (s, e) =>
                {
                    if (e.Result.Reason == ResultReason.RecognizedSpeech)
                    {
                        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                    }
                    else if (e.Result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                };

                transcriber.Canceled += (s, e) =>
                {
                    Console.WriteLine($"CANCELED: Reason={e.Reason}");

                    if (e.Reason == CancellationReason.Error)
                    {
                        Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                        Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                        Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        stopTranscription.TrySetResult(0);
                    }
                };

                transcriber.SessionStarted += (s, e) =>
                {
                    Console.WriteLine("\nSession started event.");
                };

                transcriber.SessionStopped += (s, e) =>
                {
                    Console.WriteLine("\nSession stopped event.");
                    Console.WriteLine("\nStop recognition.");
                    stopTranscription.TrySetResult(0);
                };

                // Sets a conversation Id.
                transcriber.ConversationId = "AConversationFromTeams";

                // Add participants to the conversation.
                // Create voice signatures using REST API described in the earlier section in this document.
                // Voice signature needs to be in the following format:
                // { "Version": <Numeric value>, "Tag": "string", "Data": "string" }

                var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                transcriber.AddParticipant(speakerA);
                transcriber.AddParticipant(speakerB);
                transcriber.AddParticipant(speakerC);

                // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                await transcriber.StartTranscribingAsync().ConfigureAwait(false);

                // Waits for completion.
                // Use Task.WaitAny to keep the task rooted.
                Task.WaitAny(new[] { stopTranscription.Task });

                // Stop transcribing the conversation.
                await transcriber.StopTranscribingAsync().ConfigureAwait(false);

                // Ends the conversation.
                await transcriber.EndConversationAsync().ConfigureAwait(false);
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklerimizi keşfet](https://aka.ms/csspeech/samples)
