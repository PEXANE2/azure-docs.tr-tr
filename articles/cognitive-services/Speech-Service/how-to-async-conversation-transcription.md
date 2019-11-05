---
title: Zaman uyumsuz konuşma dökümü (Önizleme)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetini kullanarak zaman uyumsuz konuşma dökümünü nasıl kullanacağınızı öğrenin. Yalnızca Java için kullanılabilir.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: amishu
ms.openlocfilehash: 433ea2778d99de39991565b2529e2f8eab4e13df
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506983"
---
# <a name="asynchronous-conversation-transcription-preview"></a>Zaman uyumsuz konuşma dökümü (Önizleme)

Bu makalede, **Remotekonuşmayı Tiontranscriptionclient** API 'si kullanılarak zaman uyumsuz görüşme dökümü gösterilmektedir. Zaman uyumsuz dökümü yapmak ve bir `conversationId`sağlamak için konuşma dökümünü yapılandırdıysanız, **Remotekonuşmayı Tiontranscriptionclient** API 'sini kullanarak bu `conversationId` ilişkili dökümü elde edebilirsiniz.

## <a name="asynchronous-vs-real-time--asynchronous"></a>Zaman uyumsuz ile gerçek zamanlı + zaman uyumsuz

Zaman uyumsuz döküm sayesinde, konuşma sesini akışa alırsınız ancak gerçek zamanlı olarak geri döndürülen bir döküm gerekmez. Bunun yerine, ses gönderildikten sonra, zaman uyumsuz döküm durumunu sorgulamak için `ConversationTranscriber` `conversationId` kullanın. Zaman uyumsuz döküm hazırlandıktan sonra bir `RemoteConversationTranscriptionResult`alırsınız.

Gerçek zamanlı ve zaman uyumsuz olarak, dökümü gerçek zamanlı olarak alır, ancak `conversationId` (zaman uyumsuz senaryoya benzer şekilde) ile sorgulama yaparak da bir zaman alıcı elde edersiniz.

Zaman uyumsuz dökümü gerçekleştirmek için iki adım gerekir. İlk adım, yalnızca zaman uyumsuz veya gerçek zamanlı ve zaman uyumsuz olarak seçerek sesi karşıya yüklemedir. İkinci adım, döküm sonuçlarını almak için kullanılır.

## <a name="upload-the-audio"></a>Sesi karşıya yükle

Zaman uyumsuz döküm gerçekleştirilmeden önce, Microsoft bilişsel konuşma istemci SDK 'sını (sürüm 1.8.0 veya üzeri) kullanarak konuşmayı bir şekilde göndermeniz gerekir.

Bu örnek kod, yalnızca zaman uyumsuz mod için görüşme dökümünü oluşturmayı gösterir. Bu şekilde ses akışını sağlamak için [konuşma SDK 'sı ile gerçek zamanlı olarak konuşmadan](how-to-use-conversation-transcription-service.md)elde edilen ses akışı kodunu eklemeniz gerekir. Desteklenen platformlar ve diller API 'Lerini görmek için ilgili konunun **sınırlamalar** bölümüne bakın.

```java
// Create the speech config object
// Substitute real information for "YourSubscriptionKey" and "Region"
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");
speech_config.setProperty("ConversationTranscriptionInRoomAndOnline", "true");

// Set the property for asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "Async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);

// Do rest of the things as explained in how to use Conversation Transcription

// pick a conversation Id that is a GUID.
Conversation conversation = new Conversation(speechConfig, conversationId);

// Create a conversation transcriber
ConversationTranscriber transcriber = new ConversationTranscriber(AudioConfig.fromDefaultMicrophoneInput());

// join a conversation
transcriber.joinConversationAsync(conversation).get();

// stream audio as shown in “Transcribe conversations in real time”
...
```

Gerçek zamanlı _artı_ zaman uyumsuz isterseniz, ilgili kod satırlarının açıklamasını aşağıdaki gibi kodlayın:

```java
// Set the property for asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "Async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);
```

## <a name="get-transcription-results"></a>Dökümü alma sonuçları

Bu adım zaman uyumsuz döküm sonuçlarını alır, ancak gerekli olan gerçek zamanlı işlemleri başka bir yerde yerine getirir. Daha fazla bilgi için bkz. [KONUŞMAYı SDK ile gerçek zamanlı olarak konuşmaları dönüştürme](how-to-use-conversation-transcription-service.md).

Burada gösterilen kod için, Windows, Linux ve Android (API düzeyi 26 veya üzeri) üzerinde yalnızca Java (1,8 veya üzeri) için desteklenen **remoteconversation-Client-SDK sürüm 1.0.0**gerekir.

### <a name="obtaining-the-client-sdk"></a>İstemci SDK 'sını alma

Pod. XML dosyanızı aşağıdaki gibi düzenleyerek **remoteconversation-Client-SDK** elde edebilirsiniz.

- Dosyanın sonunda, kapatma etiketiyle `</project>`önce, konuşma SDK 'Sı için Maven deposuna başvuru içeren bir `repositories` öğesi oluşturun:

  ```xml
  <repositories>
    <repository>
      <id>maven-cognitiveservices-speech</id>
      <name>Microsoft Cognitive Services Speech Maven Repository</name>
      <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
    </repository>
  </repositories>
  ```

- Ayrıca, bir bağımlılık olarak remoteconversation-Client-SDK 1.0.0 ile bir `dependencies` öğesi ekleyin:

  ```xml
  <dependencies>
    <dependency>
      <groupId>com.microsoft.cognitiveservices.speech.remoteconversation</groupId>
      <artifactId>remoteconversation-client-sdk</artifactId>
      <version>1.0.0</version>
    </dependency>
  </dependencies>
  ```

- Değişiklikleri Kaydet

### <a name="sample-transcription-code"></a>Örnek dökümü kodu

`conversationId`aldıktan sonra, zaman uyumsuz döküm durumunu sorgulamak için istemcide bir uzak işlem nesnesi **Remotetalk Tiontranscriptionoperation** oluşturun. **Remotetalk Tiontranscriptionoperation** , [Poller](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/util/polling/Poller.java)'tan genişletilir. Poller tamamlandıktan sonra, Poller 'e abone olarak ve nesneyi sorgularken **Remotetalk Tiontranscriptionresult** alın. Bu kodda, sonuç içeriğini sistem çıktısına yazdırdık.

```java
// Create the speech config object
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");

// Create a remote Conversation Transcription client
RemoteConversationTranscriptionClient client = new RemoteConversationTranscriptionClient(speechConfig);

// Create a remote Conversation Transcription operation
RemoteConversationTranscriptionOperation operation = new RemoteConversationTranscriptionOperation(conversationId, client);

// Operation identifier now be the value of `conversationId`
System.out.println("Operation Identifier:" + operation.getId());

// Get the observer (which is a Flux) and subscribe to it for the response
operation.getObserver()
                .subscribe(
                        pollResponse -> {
                            System.out.println("Poll response status : " + pollResponse.getStatus());
                            if(pollResponse.getValue().getConversationTranscriptionResults() != null) {
                                for (int i = 0; i < pollResponse.getValue().getConversationTranscriptionResults().size(); i++) {
                                    ConversationTranscriptionResult result = pollResponse.getValue().getConversationTranscriptionResults().get(i);
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
                );
// Block on the operation till it is finished
operation.block();

System.out.println("Operation finished");
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklerimizi keşfet](https://aka.ms/csspeech/samples)
