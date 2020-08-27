---
services: cognitive-services
author: ralphe
manager: cpoulain
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: ralphe
ms.custom: devx-track-csharp
ms.openlocfilehash: dff5c7c7018945bb93ce492ee3205f3567dff1d7
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88925920"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp)

## <a name="add-sample-code"></a>Örnek kod ekleme

1. **Program.cs**açın ve içindeki tüm kodu aşağıdaki kodla değiştirin:

    ```csharp
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Audio;
    using Microsoft.CognitiveServices.Speech.Transcription;
    using System;
    using System.Threading.Tasks;
    
    namespace HelloWorld
    {
        class Program
        {
            static async Task Main(string[] args)
            {
                await CreateConversationAsync();
            }
    
            static async Task CreateConversationAsync()
            {
                // Replace these values with the details of your Cognitive Speech subscription
                string subscriptionKey = "YourSubscriptionKey";

                // Replace below with your region identifier from here: https://aka.ms/speech/sdkregion
                string region = "YourServiceRegion";
    
                // Sets source and target languages.
                // Replace with the languages of your choice, from list found here: https://aka.ms/speech/sttt-languages
                string fromLanguage = "en-US";
                string toLanguage = "de";
    
                // Set this to the display name you want for the conversation host
                string displayName = "The host";
    
                // Create the task completion source that will be used to wait until the user presses Ctrl + C
                var completionSource = new TaskCompletionSource<bool>();
    
                // Register to listen for Ctrl + C
                Console.CancelKeyPress += (s, e) =>
                {
                    completionSource.TrySetResult(true);
                    e.Cancel = true; // don't terminate the current process
                };
    
                // Create an instance of the speech translation config
                var config = SpeechTranslationConfig.FromSubscription(subscriptionKey, region);
                config.SpeechRecognitionLanguage = fromLanguage;
                config.AddTargetLanguage(toLanguage);
    
                // Create the conversation
                using (var conversation = await Conversation.CreateConversationAsync(config).ConfigureAwait(false))
                {
                    // Start the conversation so the host user and others can join
                    await conversation.StartConversationAsync().ConfigureAwait(false);
    
                    // Get the conversation ID. It will be up to your scenario to determine how this is shared with other participants.
                    string conversationId = conversation.ConversationId;
                    Console.WriteLine($"Created '{conversationId}' conversation");
    
                    // At this point, you can use the conversation object to manage the conversation. 
                    // For example, to mute everyone else in the room you can call this method:
                    await conversation.MuteAllParticipantsAsync().ConfigureAwait(false);
    
                    // Configure which audio source you want to use. If you are using a text only language, you 
                    // can use the other overload of the constructor that takes no arguments
                    var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
                    using (var conversationTranslator = new ConversationTranslator(audioConfig))
                    {
                        // You should connect all the event handlers you need at this point
                        conversationTranslator.SessionStarted += (s, e) =>
                        {
                            Console.WriteLine($"Session started: {e.SessionId}");
                        };
                        conversationTranslator.SessionStopped += (s, e) =>
                        {
                            Console.WriteLine($"Session stopped: {e.SessionId}");
                        };
                        conversationTranslator.Canceled += (s, e) =>
                        {
                            switch (e.Reason)
                            {
                                case CancellationReason.EndOfStream:
                                    Console.WriteLine($"End of audio reached");
                                    break;
    
                                case CancellationReason.Error:
                                    Console.WriteLine($"Canceled due to error. {e.ErrorCode}: {e.ErrorDetails}");
                                    break;
                            }
                        };
                        conversationTranslator.ConversationExpiration += (s, e) =>
                        {
                            Console.WriteLine($"Conversation will expire in {e.ExpirationTime.TotalMinutes} minutes");
                        };
                        conversationTranslator.ParticipantsChanged += (s, e) =>
                        {
                            Console.Write("The following participant(s) have ");
                            switch (e.Reason)
                            {
                                case ParticipantChangedReason.JoinedConversation:
                                    Console.Write("joined");
                                    break;
    
                                case ParticipantChangedReason.LeftConversation:
                                    Console.Write("left");
                                    break;
    
                                case ParticipantChangedReason.Updated:
                                    Console.Write("been updated");
                                    break;
                            }
    
                            Console.WriteLine(":");
    
                            foreach (var participant in e.Participants)
                            {
                                Console.WriteLine($"\t{participant.DisplayName}");
                            }
                        };
                        conversationTranslator.TextMessageReceived += (s, e) =>
                        {
                            Console.WriteLine($"Received an instant message from '{e.Result.ParticipantId}': '{e.Result.Text}'");
                            foreach (var entry in e.Result.Translations)
                            {
                                Console.WriteLine($"\tTranslated into '{entry.Key}': '{entry.Value}'");
                            }
                        };
                        conversationTranslator.Transcribed += (s, e) =>
                        {
                            Console.WriteLine($"Received a transcription from '{e.Result.ParticipantId}': '{e.Result.Text}'");
                            foreach (var entry in e.Result.Translations)
                            {
                                Console.WriteLine($"\tTranslated into '{entry.Key}': '{entry.Value}'");
                            }
                        };
                        conversationTranslator.Transcribing += (s, e) =>
                        {
                            Console.WriteLine($"Received a partial transcription from '{e.Result.ParticipantId}': '{e.Result.Text}'");
                            foreach (var entry in e.Result.Translations)
                            {
                                Console.WriteLine($"\tTranslated into '{entry.Key}': '{entry.Value}'");
                            }
                        };
    
                        // Enter the conversation to start receiving events
                        await conversationTranslator.JoinConversationAsync(conversation, displayName).ConfigureAwait(false);
    
                        // You can now send an instant message to all other participants in the room
                        await conversationTranslator.SendTextMessageAsync("The instant message to send").ConfigureAwait(false);
    
                        // If specified a speech-to-text language, you can start capturing audio
                        await conversationTranslator.StartTranscribingAsync().ConfigureAwait(false);
                        Console.WriteLine("Started transcribing. Press Ctrl + c to stop");
    
                        // At this point, you should start receiving transcriptions for what you are saying using the default microphone. Press Ctrl+c to stop audio capture
                        await completionSource.Task.ConfigureAwait(false);
    
                        // Stop audio capture
                        await conversationTranslator.StopTranscribingAsync().ConfigureAwait(false);
    
                        // Leave the conversation. After this you will no longer receive events
                        await conversationTranslator.LeaveConversationAsync().ConfigureAwait(false);
                    }
    
                    // End the conversation
                    await conversation.EndConversationAsync().ConfigureAwait(false);
    
                    // Delete the conversation. Any other participants that are still in the conversation will be removed
                    await conversation.DeleteConversationAsync().ConfigureAwait(false);
                }
            }
        }
    }
    ```

1. Aynı dosyada, dizeyi `YourSubscriptionKey` bilişsel konuşma abonelik anahtarınızla değiştirin.

1. Dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin.

1. Menü çubuğundan **Dosya**  >  **Tümünü Kaydet**' i seçin.

## <a name="build-and-run-the-application-to-create-a-new-conversation"></a>Yeni bir konuşma oluşturmak için uygulamayı derleyin ve çalıştırın

1. Menü çubuğundan **Build**  >  uygulamayı derlemek için derleme**Build Solution** ' ı seçin. Kodun artık hatasız derlenmesi gerekir.

1. **Debug**  >  **HelloWorld** uygulamasını başlatmak için hata**ayıklamayı Başlat** ' ı seçin (veya **F5**tuşuna basın).

1. `Started transcribing`İletinin göründüğünü gördüğünüzde konuşmayı başlatabilirsiniz. Konuştuğunuz olarak görünmeler görüntülenir.
    - Konuşma kodunu diğer kişilerle paylaşırsanız ve konuşmaya katılırsanız, bu kişilerin de onların dökümünü görürsünüz.

1. Konuşmayı tamamladıktan sonra, ses yakalamayı durdurmak için <kbd>CTRL + C</kbd> tuşlarına basın ve konuşmayı sonlandırın.

## <a name="build-and-run-the-application-to-join-an-existing-conversation"></a>Mevcut bir konuşmaya katmak için uygulamayı derleyin ve çalıştırın

1. Aşağıdaki işlevi kopyalayıp **program.cs**yapıştırın:

    ```csharp
    static async Task JoinConversationAsync(string conversationId)
    {
        // Set this to the display name you want for the participant
        string displayName = "participant";

        // Set the speech to text, or text language you want to use
        string language = "en-US";

        // Create the task completion source that will be used to wait until the user presses Ctrl + c
        var completionSource = new TaskCompletionSource<bool>();

        // Register to listen for Ctrl+C
        Console.CancelKeyPress += (s, e) =>
        {
            completionSource.TrySetResult(true);
            e.Cancel = true; // don't terminate the current process
        };

        // As a participant, you don't need to specify any subscription key, or region. You can directly create
        // the conversation translator object
        var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
        using (var conversationTranslator = new ConversationTranslator(audioConfig))
        {
            // Register for any events you are interested here. For now let's just register for
            // transcription, and instant message events
            conversationTranslator.TextMessageReceived += (s, e) =>
            {
                Console.WriteLine($"Received an instant message from '{e.Result.ParticipantId}': '{e.Result.Text}'");
                foreach (var entry in e.Result.Translations)
                {
                    Console.WriteLine($"\tTranslated into '{entry.Key}': '{entry.Value}'");
                }
            };
            conversationTranslator.Transcribed += (s, e) =>
            {
                Console.WriteLine($"Received a transcription from '{e.Result.ParticipantId}': '{e.Result.Text}'");
                foreach (var entry in e.Result.Translations)
                {
                    Console.WriteLine($"\tTranslated into '{entry.Key}': '{entry.Value}'");
                }
            };
            conversationTranslator.Transcribing += (s, e) =>
            {
                Console.WriteLine($"Received a partial transcription from '{e.Result.ParticipantId}': '{e.Result.Text}'");
                foreach (var entry in e.Result.Translations)
                {
                    Console.WriteLine($"\tTranslated into '{entry.Key}': '{entry.Value}'");
                }
            };
    
            // To start receiving events, you will need to join the conversation
            await conversationTranslator.JoinConversationAsync(conversationId, displayName, language).ConfigureAwait(false);

            // You can now send an instant message
            await conversationTranslator.SendTextMessageAsync("Message from participant").ConfigureAwait(false);

            // Start capturing audio if you specified a speech-to-text language
            await conversationTranslator.StartTranscribingAsync().ConfigureAwait(false);
            Console.WriteLine("Started transcribing. Press Ctrl-C to stop");

            // At this point, you should start receiving transcriptions for what you are saying using
            // the default microphone. Press Ctrl+C to stop audio capture
            await completionSource.Task.ConfigureAwait(false);

            // Stop audio capture
            await conversationTranslator.StopTranscribingAsync().ConfigureAwait(false);

            // Leave the conversation. You will stop receiving events after this
            await conversationTranslator.LeaveConversationAsync().ConfigureAwait(false);
        }
    }
    ```

2. `CreateConversationAsync();` `public static async Task Main(string[] args)` İşlevinizdeki ile değiştirin:

    ```csharp
    // Set this to the conversation you want to join
    JoinConversationAsync("YourConversationId");
    ```

[!INCLUDE [create-from-web](../create-from-web.md)]

4. Visual Studio 'ya geri dönün ve dizeyi, `YourConversationId` önceki adımda oluşturduğunuz konuşma kimliğiyle değiştirin.

5. Menü çubuğundan **Build**  >  uygulamayı derlemek için derleme**Build Solution** ' ı seçin. Kodun artık hatasız derlenmesi gerekir.

6. **Debug**  >  **HelloWorld** uygulamasını başlatmak için hata**ayıklamayı Başlat** ' ı seçin (veya **F5**tuşuna basın).

7. `Started transcribing`İletinin göründüğünü gördüğünüzde konuşmayı başlatabilirsiniz. Konuştuğunuz olarak görünmeler görüntülenir.
    - Tarayıcınıza geri giderseniz, yaptığınız gibi, yaptığınız gibi görünmeler burada görünür.

8. Konuşmayı tamamladıktan sonra, ses yakalamayı durdurmak için <kbd>CTRL + C</kbd> tuşlarına basın ve konuşmayı sonlandırın.

9. Tarayıcınıza geri dönün ve sağ üst köşedeki Çıkış düğmesini kullanarak konuşmadan çıkın.

## <a name="next-steps"></a>Sonraki Adımlar

[!INCLUDE [footer](./footer.md)]
