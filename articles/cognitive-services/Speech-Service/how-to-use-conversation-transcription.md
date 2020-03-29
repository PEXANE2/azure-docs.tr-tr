---
title: Gerçek zamanlı Konuşma Transkripsiyon (Önizleme) - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma SDK ile gerçek zamanlı Konuşma Transkripsiyon nasıl kullanılacağını öğrenin. C++, C#ve Java için kullanılabilir.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: weixu
ms.openlocfilehash: 64a9e11cec7164fb4421dd018238de9f0670382b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76263736"
---
# <a name="real-time-conversation-transcription-preview"></a>Gerçek zamanlı Konuşma Transkripsiyon (Önizleme)

Konuşma SDK'nın **ConversationTranscriber** API kullanarak Konuşma hizmetine ses akışı sağlayarak birden fazla katılımcı ekleme, kaldırma ve tanımlama `PullStream` `PushStream`yeteneği ile toplantı ve diğer konuşmaları transkripsiyonu yapmanızı sağlar. Bu konu, Konuşma SDK 'de (sürüm 1.8.0 veya daha sonra) Konuşma-metin kullanmayı bilmenizi gerektirir. Daha fazla bilgi için konuşma [hizmetleri nelerdir'e](overview.md)bakın.

## <a name="limitations"></a>Sınırlamalar

- ConversationTranscriber API, Windows, Linux ve Android'de C++, C#ve Java için desteklenir.
- Şu anda aşağıdaki bölgelerde "en-US" ve "zh-CN" dillerinde mevcuttur: _centralus_ ve _eastasia_.
- Oynatma başvuru akışına sahip 7 mikrofonlu dairesel çoklu mikrofon dizisi gerektirir. Mikrofon dizisi [belirtimimizi](https://aka.ms/sdsdk-microphone)karşılamalı.
- [Konuşma Cihazları SDK](speech-devices-sdk.md) uygun cihazlar ve Konuşma Transkripsiyon gösteren bir örnek uygulama sağlar.

## <a name="optional-sample-code-resources"></a>İsteğe bağlı örnek kod kaynakları

Konuşma Aygıtı SDK, 8 kanal kullanarak gerçek zamanlı ses çekimi için Java'da örnek kod sağlar.

- [ROOBO cihaz örnek kodu](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Android/Speech%20Devices%20SDK%20Starter%20App/example/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/sdsdkstarterapp/Conversation.java)
- [Azure Kinect Dev Kit örnek kodu](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK/blob/master/Samples/Windows_Linux/SampleDemo/src/com/microsoft/cognitiveservices/speech/samples/Cts.java)

## <a name="prerequisites"></a>Ön koşullar

Konuşma hizmeti aboneliği. Yoksa [Konuşma deneme aboneliği alabilirsiniz.](https://azure.microsoft.com/try/cognitive-services/)

## <a name="create-voice-signatures"></a>Ses imzaları oluşturma

İlk adım, verimli konuşmacı tanımlaması için konuşma katılımcıları için sesli imzalar oluşturmaktır.

### <a name="audio-input-requirements"></a>Ses giriş gereksinimleri

- Ses imzaları oluşturmak için giriş ses dalgası dosyası 16 bit örnekleri, 16 kHz örnek hızı ve tek bir kanal (mono) biçiminde olmalıdır.
- Her ses örneği için önerilen uzunluk otuz saniye ile iki dakika arasındadır.

### <a name="sample-code"></a>Örnek kod

Aşağıdaki örnek, C#'daki REST [API'sini kullanarak](https://aka.ms/cts/signaturegenservice) ses imzası oluşturmanın iki farklı yolunu gösterir. Gerçek bilgileri "YourSubscriptionKey", dalga dosya adınızı "speakerVoice.wav" ve bölgeniz `{region}` için "YourServiceRegion"_(centralus_ veya _eastasia)_ yerine almanız gerektiğini unutmayın.

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        // Replace with your own region
        var region = "YourServiceRegion";
        // Change the name of the wave file to match yours
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        // Add your subscription key to the header Ocp-Apim-Subscription-Key directly
        // Replace "YourSubscriptionKey" with your own subscription key
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        // Edit with your desired region for `{region}`
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric string or integer value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        // Replace with your own region
        var region = "YourServiceRegion";
        // Change the name of the wave file to match yours
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        // Add your subscription key to the header Ocp-Apim-Subscription-Key directly
        // Replace "YourSubscriptionKey" with your own subscription key
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        // Edit with your desired region for `{region}`
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric string or integer value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribe-conversations"></a>Konuşmaları transkripsiyonu

Aşağıdaki örnek kod, üç konuşmacı için konuşmaların gerçek zamanlı olarak nasıl yazıya aktarılabildiğini gösterir. Yukarıda gösterildiği gibi her hoparlör için sesli imzalar oluşturduğunuzu varsayar. SpeechConfig nesnesini oluştururken gerçek bilgileri "YourSubscriptionKey" ve "YourServiceRegion" yerine bulundurun.

Örnek kod vurguları şunlardır:

- `Conversation` Kullanarak oluşturulan toplantı `SpeechConfig` tanımlayıcısını kullanarak nesneden nesne oluşturma`Guid.NewGuid()`
- Bir `ConversationTranscriber` nesne oluşturma ve `JoinConversationAsync()` transkripsiyon başlatmak için konuşma katılmak
- İlgi çekici olayları kaydetme
- Konuşma nesnesini kullanarak konuşmaya katılımcı ekleme veya kaldırma
- Ses akışı
- Konuşma SDK sürüm 1.9.0 ve `int` itibaren `string` hem ve değer türleri ses imzası sürüm alanında desteklenir.

Transkripsiyon ve hoparlör tanımlayıcısı kayıtlı olaylarda geri gelir.

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Transcription;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region
        // Replace with your own subscription key and region
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        config.SetProperty("ConversationTranscriptionInRoomAndOnline", "true");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file or from the default microphone if you want to stream live audio from the supported devices
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            var meetingId = Guid.NewGuid().ToString();
            using (var conversation = await Conversation.CreateConversationAsync(config, meetingId).ConfigureAwait(false))
            {
                // Create a conversation transcriber using audio stream input
                using (var conversationTranscriber = new ConversationTranscriber    (audioInput))
                {
                    await conversationTranscriber.JoinConversationAsync(conversation);

                    // Subscribe to events
                    conversationTranscriber.Transcribing += (s, e) =>
                    {
                            Console.WriteLine($"TRANSCRIBING: Text={e.Result.Text}");
                    };

                    conversationTranscriber.Transcribed += (s, e) =>
                    {
                        if (e.Result.Reason == ResultReason.RecognizedSpeech)
                        {
                            Console.WriteLine($"TRANSCRIBED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                        }
                        else if (e.Result.Reason == ResultReason.NoMatch)
                        {
                            Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                        }
                    };

                    conversationTranscriber.Canceled += (s, e) =>
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

                    conversationTranscriber.SessionStarted += (s, e) =>
                    {
                        Console.WriteLine("\nSession started event.");
                    };

                    conversationTranscriber.SessionStopped += (s, e) =>
                    {
                        Console.WriteLine("\nSession stopped event.");
                        Console.WriteLine("\nStop recognition.");
                        stopTranscription.TrySetResult(0);
                    };

                    // Add participants to the conversation.
                    // Create voice signatures using REST API described in the earlier section in this document.
                    // Voice signature needs to be in the following format:
                    // { "Version": <Numeric string or integer value>, "Tag": "string", "Data": "string" }

                    var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                    var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                    var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                    await conversation.AddParticipantAsync(speakerA);
                    await conversation.AddParticipantAsync(speakerB);
                    await conversation.AddParticipantAsync(speakerC);

                    // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                    await conversationTranscriber.StartTranscribingAsync().ConfigureAwait(false);

                    // Waits for completion.
                    // Use Task.WaitAny to keep the task rooted.
                    Task.WaitAny(new[] { stopTranscription.Task });

                    // Stop transcribing the conversation.
                    await conversationTranscriber.StopTranscribingAsync().ConfigureAwait(false);
                 }
            }
       }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Zaman uyumsuz Konuşma Transkripsiyonu](how-to-async-conversation-transcription.md)
