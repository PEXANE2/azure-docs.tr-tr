---
title: Asynchronous Konuşma Transkripsiyon (Önizleme) - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetini kullanarak eşzamanlı Konuşma Transkripsiyon'u nasıl kullanacağınızı öğrenin. Yalnızca Java için kullanılabilir.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: amishu
ms.openlocfilehash: 57543f4a3779145ce66259eec1abac195b63c7ba
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384305"
---
# <a name="asynchronous-conversation-transcription-preview"></a>Asynchronous Konuşma Transkripsiyon (Önizleme)

Bu makalede, asynchronous Konuşma Transkripsiyon **RemoteConversationTranscriptionClient** API kullanılarak gösterilmiştir. Konuşma Transkripsiyon'u asynchronous transkripsiyon `conversationId`yapmak üzere yapılandırıldıysanız ve `conversationId` **RemoteConversationTranscriptionClient** API'yi kullanarak bununla ilişkili transkripsiyonu elde edebilirsiniz.

## <a name="asynchronous-vs-real-time--asynchronous"></a>Asynchronous vs gerçek zamanlı + asynchronous

Eşzamanlı transkripsiyon ile, konuşma ses akışı, ancak gerçek zamanlı olarak döndürülen bir transkripsiyon gerekmez. Bunun yerine, ses gönderildikten `conversationId` sonra, asynchronous transkripsiyon durumu için sorgu `Conversation` için kullanın. Asynchronous transkripsiyon hazır olduğunda, bir `RemoteConversationTranscriptionResult`alırsınız .

Gerçek zamanlı artı asynchronous ile transkripsiyon gerçek zamanlı olarak olsun, ama aynı `conversationId` zamanda (asynchronous senaryoya benzer) ile sorgulayarak transkripsiyon olsun.

Asynchronous transkripsiyon gerçekleştirmek için iki adım gereklidir. İlk adım, yalnızca eşzamanlı veya gerçek zamanlı artı asynchronous seçerek, ses yüklemektir. İkinci adım transkripsiyon sonuçlarını elde etmektir.

## <a name="upload-the-audio"></a>Sesi yükleyin

Eşkron transkripsiyon işlemi yapılmadan önce, Sesi Microsoft Bilişsel Konuşma istemcisi SDK'yı (sürüm 1.8.0 veya üzeri) kullanarak Konuşma Transkripsiyon Hizmetine göndermeniz gerekir.

Bu örnek kod, yalnızca eş zamanlı mod için konuşma çeviricinin nasıl oluşturulacak olduğunu gösterir. Transkripsiyöre ses akışı sağlamak için, [Konuşma SDK ile Transcribe konuşmalarından](how-to-use-conversation-transcription-service.md)türetilen ses akış kodunu gerçek zamanlı olarak eklemeniz gerekir. Desteklenen platformları ve dilleri görmek için bu konunun **Sınırlamalar** bölümüne bakın.

```java
// Create the speech config object
// Substitute real information for "YourSubscriptionKey" and "Region"
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");
speechConfig.setProperty("ConversationTranscriptionInRoomAndOnline", "true");

// Set the property for asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);

// pick a conversation Id that is a GUID.
String conversationId = UUID.randomUUID().toString();

// Create a Conversation
Future<Conversation> conversationFuture = Conversation.createConversationAsync(speechConfig, conversationId);
Conversation conversation = conversationFuture.get();

// Create an audio stream from a wav file or from the default microphone if you want to stream live audio from the supported devices
// Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
PullAudioInputStreamCallback wavfilePullStreamCallback = Helper.OpenWavFile("16kHz16Bits8channelsOfRecordedPCMAudio.wav");
// Create an audio stream format assuming the file used above is 16kHz, 16 bits and 8 channel pcm wav file
AudioStreamFormat audioStreamFormat = AudioStreamFormat.getWaveFormatPCM((long)16000, (short)16,(short)8);
// Create an input stream
AudioInputStream audioStream = AudioInputStream.createPullStream(wavfilePullStreamCallback, audioStreamFormat);

// Create a conversation transcriber
ConversationTranscriber transcriber = new ConversationTranscriber(AudioConfig.fromStreamInput(audioStream));

// join a conversation
transcriber.joinConversationAsync(conversation);

// Add the event listener for the realtime events
transcriber.transcribed.addEventListener((o, e) -> {
    System.out.println("Conversation transcriber Recognized:" + e.toString());
});

transcriber.canceled.addEventListener((o, e) -> {
    System.out.println("Conversation transcriber canceled:" + e.toString());
    try {
        transcriber.stopTranscribingAsync().get();
    } catch (InterruptedException ex) {
        ex.printStackTrace();
    } catch (ExecutionException ex) {
        ex.printStackTrace();
    }
});

transcriber.sessionStopped.addEventListener((o, e) -> {
    System.out.println("Conversation transcriber stopped:" + e.toString());

    try {
        transcriber.stopTranscribingAsync().get();
    } catch (InterruptedException ex) {
        ex.printStackTrace();
    } catch (ExecutionException ex) {
        ex.printStackTrace();
    }
});

// start the transcription.
Future<?> future = transcriber.startTranscribingAsync();
...
```

Gerçek zamanlı _artı_ eşzamanlı istiyorsanız, uygun kod satırlarını aşağıdaki gibi yorumlayın ve yorumsunuz:

```java
// Set the property for asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);
```

## <a name="get-transcription-results"></a>Transkripsiyon sonuçlarını alın

Bu adım, eşzamanlı transkripsiyon sonuçlarını alır, ancak gerekli olabilecek herhangi bir gerçek zamanlı işlemin başka bir yerde yapıldığını varsayar. Daha fazla bilgi için, [Konuşma SDK ile gerçek zamanlı olarak transkripsiyonu konuşmaları](how-to-use-conversation-transcription-service.md)bakın.

Burada gösterilen kod için, yalnızca Windows, Linux ve Android'de (yalnızca API düzeyi 26 veya üzeri) Java (1.8.0 veya üzeri) için desteklenen **uzaktan konuşma sürümü 1.8.0'a**ihtiyacınız vardır.

### <a name="obtaining-the-client-sdk"></a>İstemci SDK'nın elde edilmesi

Pom.xml dosyanızı aşağıdaki gibi düzenleyerek **uzaktan konuşma** elde edebilirsiniz.

1. Dosyanın sonunda, kapanış etiketinden `</project>`önce, `repositories` Konuşma SDK için Maven deposuna atıfta bulunulan bir öğe oluşturun:

   ```xml
   <repositories>
     <repository>
       <id>maven-cognitiveservices-speech</id>
       <name>Microsoft Cognitive Services Speech Maven Repository</name>
       <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
     </repository>
   </repositories>
   ```

2. Ayrıca, `dependencies` uzaktan konuşma istemcisi-sdk 1.8.0 bağımlılık olarak bir öğe ekleyin:

   ```xml
   <dependencies>
     <dependency>
       <groupId>com.microsoft.cognitiveservices.speech.remoteconversation</groupId>
       <artifactId>remote-conversation</artifactId>
       <version>1.8.0</version>
     </dependency>
   </dependencies>
   ```

3. Değişiklikleri kaydetme

### <a name="sample-transcription-code"></a>Örnek transkripsiyon kodu

Sonra `conversationId`, asynchronous transkripsiyon durumunu sorgulamak için istemci uygulamasında uzaktan konuşma transkripsiyon **istemciremoteConversationTranscriptionClient** oluşturun. [Bir PollerFlux](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/util/polling/PollerFlux.java) nesnesi almak için **RemoteConversationTranscriptionClient** **getTranscriptionOperation** yöntemini kullanın. PollerFlux nesne uzak çalışma durumu **RemoteConversationTranscriptionOperation** ve nihai sonuç **RemoteConversationTranscriptionResult**hakkında bilgi olacaktır. İşlem tamamlandıktan sonra, [bir SyncPoller'da](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/util/polling/SyncPoller.java) **getFinalResult'ı** arayarak **RemoteConversationTranscriptionResult'ı** alın. Bu kodda biz sadece sistem çıktısı için sonuç içeriğini yazdırmak.

```java
// Create the speech config object
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");

// Create a remote Conversation Transcription client
RemoteConversationTranscriptionClient client = new RemoteConversationTranscriptionClient(speechConfig);

// Get the PollerFlux for the remote operation
PollerFlux<RemoteConversationTranscriptionOperation, RemoteConversationTranscriptionResult> remoteTranscriptionOperation = client.getTranscriptionOperation(conversationId);

// Subscribe to PollerFlux to get the remote operation status
remoteTranscriptionOperation.subscribe(
        pollResponse -> {
            System.out.println("Poll response status : " + pollResponse.getStatus());
            System.out.println("Poll response status : " + pollResponse.getValue().getServiceStatus());
        }
);

// Obtain the blocking operation using getSyncPoller
SyncPoller<RemoteConversationTranscriptionOperation, RemoteConversationTranscriptionResult> blockingOperation =  remoteTranscriptionOperation.getSyncPoller();

// Wait for the operation to finish
blockingOperation.waitForCompletion();

// Get the final result response
RemoteConversationTranscriptionResult resultResponse = blockingOperation.getFinalResult();

// Print the result
if(resultResponse != null) {
    if(resultResponse.getConversationTranscriptionResults() != null) {
        for (int i = 0; i < resultResponse.getConversationTranscriptionResults().size(); i++) {
            ConversationTranscriptionResult result = resultResponse.getConversationTranscriptionResults().get(i);
            System.out.println(result.getProperties().getProperty(PropertyId.SpeechServiceResponse_JsonResult.name()));
            System.out.println(result.getProperties().getProperty(PropertyId.SpeechServiceResponse_JsonResult));
            System.out.println(result.getOffset());
            System.out.println(result.getDuration());
            System.out.println(result.getUserId());
            System.out.println(result.getReason());
            System.out.println(result.getResultId());
            System.out.println(result.getText());
            System.out.println(result.toString());
        }
    }
}

System.out.println("Operation finished");
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneklerimizi GitHub'da keşfedin](https://aka.ms/csspeech/samples)
