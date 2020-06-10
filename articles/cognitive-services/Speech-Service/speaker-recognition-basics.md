---
title: Konuşmacı Tanıma temelleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: "\"Konuşuyor\" sorusuna yanıt vermek için konuşma SDK 'sını nasıl kullanacağınızı öğrenin. Bu makalede, hem konuşmacı doğrulama hem de tanımlama ile çalışmaya yönelik yaygın tasarım desenleri hakkında bilgi edineceksiniz."
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/05/2020
ms.author: trbye
ms.openlocfilehash: b8c38847e3228c54091b84218a5d72c79e3e41a8
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632320"
---
# <a name="learn-the-basics-of-speaker-recognition"></a>Konuşmacı tanımanın temellerini öğrenin

Bu makalede, konuşma SDK 'sını kullanarak konuşmacı tanıma için temel tasarım düzenlerini öğrenirsiniz, örneğin:

* Metne bağımlı ve metinden bağımsız doğrulama
* Sesler grubu arasında bir ses örneği tanımlamak için konuşmacı tanımlaması
* Ses profillerini silme

Konuşma tanıma kavramlarının üst düzey bir görünümü için bkz. [genel bakış](speaker-recognition-overview.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](get-started.md).

> [!IMPORTANT]
> Konuşmacı tanıma özelliği şu anda *yalnızca* bölgede oluşturulan Azure konuşma kaynaklarında desteklenmektedir `westus` .

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir. Platformunuza bağlı olarak, aşağıdaki yönergeleri kullanın:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=dotnet&pivots=programming-language-csharp" target="_blank">.NET Framework<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=dotnetcore&pivots=programming-language-csharp" target="_blank">.NET Core<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=unity&pivots=programming-language-csharp" target="_blank">'Yi<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=uwps&pivots=programming-language-csharp" target="_blank">UWP<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=xaml&pivots=programming-language-csharp" target="_blank">Xamarin<span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="import-dependencies"></a>Bağımlılıkları içeri aktar

Bu makaledeki örnekleri çalıştırmak için, `using` betiğinizin en üstüne aşağıdaki deyimleri ekleyin.

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
```

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir oluşturmanız gerekir [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet) . Bu örnekte, bir [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet) abonelik anahtarı ve bölgesi kullanarak bir oluşturun. Ayrıca, bu makalenin geri kalanı için kullanabileceğiniz, farklı özelleştirmeler için değiştirdiğiniz bazı temel ortak kod oluşturabilirsiniz.

`westus`Hizmet için desteklenen tek bölge olduğu için bölgenin olarak ayarlandığını unutmayın.

```csharp
public class Program 
{
    static async Task Main(string[] args)
    {
        // replace with your own subscription key 
        string subscriptionKey = "YourSubscriptionKey";
        string region = "westus";
        var config = SpeechConfig.FromSubscription(subscriptionKey, region);
    }
}
```

## <a name="text-dependent-verification"></a>Metne bağımlı doğrulama

Konuşmacı Doğrulama, konuşmacının bilinen veya **kayıtlı** bir sesle eşleştiğini onaylama işlemidir. İlk adım, bir ses profilini **kaydetmek** ve bu sayede hizmetin gelecekteki sesli örnekleri karşılaştırmak için bir şey olması gerekir. Bu örnekte, profili, hem kayıt hem de doğrulama için kullanılmak üzere belirli bir geçiş ifadesi gerektiren **metne bağlı** bir strateji kullanarak kaydedetirsiniz. Desteklenen geçiş tümceciklerin listesi için [başvuru belgelerine](https://docs.microsoft.com/rest/api/speakerrecognition/) bakın.

`Program`Bir ses profili kaydetmek için Sınıfınıza aşağıdaki işlevi oluşturarak başlayın.

```csharp
public static async Task VerificationEnroll(SpeechConfig config, Dictionary<string, string> profileMapping)
{
    using (var client = new VoiceProfileClient(config))
    using (var profile = await client.CreateProfileAsync(VoiceProfileType.TextDependentVerification, "en-us"))
    {
        using (var audioInput = AudioConfig.FromDefaultMicrophoneInput())
        {
            Console.WriteLine($"Enrolling profile id {profile.Id}.");
            // give the profile a human-readable display name
            profileMapping.Add(profile.Id, "Your Name");

            VoiceProfileEnrollmentResult result = null;
            while (result is null || result.RemainingEnrollmentsCount > 0)
            {
                Console.WriteLine("Speak the passphrase, \"My voice is my passport, verify me.\"");
                result = await client.EnrollProfileAsync(profile, audioInput);
                Console.WriteLine($"Remaining enrollments needed: {result.RemainingEnrollmentsCount}");
                Console.WriteLine("");
            }
            
            if (result.Reason == ResultReason.EnrolledVoiceProfile)
            {
                await SpeakerVerify(config, profile, profileMapping);
            }
            else if (result.Reason == ResultReason.Canceled)
            {
                var cancellation = VoiceProfileEnrollmentCancellationDetails.FromResult(result);
                Console.WriteLine($"CANCELED {profile.Id}: ErrorCode={cancellation.ErrorCode} ErrorDetails={cancellation.ErrorDetails}");
            }
        }
    }
}
```

Bu işlevde, `await client.CreateProfileAsync()` aslında yeni ses profilini oluşturur. Oluşturulduktan sonra, `AudioConfig.FromDefaultMicrophoneInput()` varsayılan giriş cihazınızdan ses yakalamak için bu örnekte kullanarak, ses örneklerini nasıl girirsiniz belirlersiniz. Ardından, ses örneklerini, `while` kalan örnek sayısını izleyen ve kayıt için gereken bir döngüye kaydeder. Her yinelemede, `client.EnrollProfileAsync(profile, audioInput)` sizden geçiş tümceciğini mikrofona iletmeniz ve örneği ses profiline eklemeniz istenir.

Kayıt tamamlandıktan sonra, `await SpeakerVerify(config, profile, profileMapping)` Yeni oluşturduğunuz profile karşı doğrulama seçeneğini çağırın. Tanımlamak için başka bir işlev ekleyin `SpeakerVerify` .

```csharp
public static async Task SpeakerVerify(SpeechConfig config, VoiceProfile profile, Dictionary<string, string> profileMapping)
{
    var speakerRecognizer = new SpeakerRecognizer(config, AudioConfig.FromDefaultMicrophoneInput());
    var model = SpeakerVerificationModel.FromProfile(profile);

    Console.WriteLine("Speak the passphrase to verify: \"My voice is my passport, please verify me.\"");
    var result = await speakerRecognizer.RecognizeOnceAsync(model);
    Console.WriteLine($"Verified voice profile for speaker {profileMapping[result.ProfileId]}, score is {result.Score}");
}
```

Bu işlevde, `VoiceProfile` Yeni oluşturduğunuz nesneyi, için bir model başlatmak üzere geçitirsiniz. Ardından, `await speakerRecognizer.RecognizeOnceAsync(model)` geçiş tümceciğini yeniden konuşuyor, ancak bu kez ses profilinize göre doğrular ve 0.0-1,0 arasında bir benzerlik puanı döndürür. `result`Nesne, `Accept` `Reject` geçiş tümceciğinin eşleşip eşleşmediğine göre de veya döndürür.

Ardından, `Main()` işlevinizi, oluşturduğunuz yeni işlevleri çağırmak için değiştirin. Ayrıca, `Dictionary<string, string>` işlev çağrılarınız aracılığıyla başvuruya göre geçiş yapmak için bir oluşturmanız gerektiğini unutmayın. Bunun nedeni, hizmetin, bir kullanıcı tarafından okunabilen `VoiceProfile` ve yalnızca gizlilik amaçları için BIR kimlik numarası depolayan bir adı saklamasının nedenidir. İşlevinde, `VerificationEnroll` Bu sözlüğe yeni oluşturulan kimliğe sahip bir giriş ekleyin ve bir metin adı ile birlikte. İnsan tarafından okunabilen bir adı görüntülemesi gereken uygulama geliştirme senaryolarında, **Bu eşlemeyi bir yere depolamanız gerekir, hizmet tarafından depolanamıyor**.

```csharp
static async Task Main(string[] args)
{
    string subscriptionKey = "YourSubscriptionKey";
    string region = "westus";
    var config = SpeechConfig.FromSubscription(subscriptionKey, region);

    // persist profileMapping if you want to store a record of who the profile is
    var profileMapping = new Dictionary<string, string>();
    await VerificationEnroll(config, profileMapping);

    Console.ReadLine();
}
```

Betiği çalıştırın ve şu ana kadar bir süre için *My Voice* My, kayıt için üç kez parola ve doğrulama için bir ek zaman doğrulayın. Döndürülen sonuç, doğrulama için kendi özel eşiklerinizi oluşturmak için kullanabileceğiniz benzerlik puandır.

```shell
Enrolling profile id 87-2cef-4dff-995b-dcefb64e203f.
Speak the passphrase, "My voice is my passport, verify me."
Remaining enrollments needed: 2

Speak the passphrase, "My voice is my passport, verify me."
Remaining enrollments needed: 1

Speak the passphrase, "My voice is my passport, verify me."
Remaining enrollments needed: 0

Speak the passphrase to verify: "My voice is my passport, verify me."
Verified voice profile for speaker Your Name, score is 0.915581
```

## <a name="text-independent-verification"></a>Metnin bağımsız doğrulaması

**Metne bağımlı** doğrulamanın aksine, **metinden bağımsız** doğrulama:

* Belirli bir geçiş ifadesinin söylanmasına gerek yoktur, her şey söylenebilir
* Üç ses *örneği gerektirmez, ancak toplam* ses için 20 saniye gerekir

`VerificationEnroll` **Metin bağımsız** doğrulamasına geçmek için işlevinizde birkaç basit değişiklik yapın. İlk olarak, doğrulama türünü olarak değiştirirsiniz `VoiceProfileType.TextIndependentVerification` . Sonra, `while` `result.RemainingEnrollmentsSpeechLength` 20 saniyelik ses yakalanana kadar konuşmanıza devam edecek olan döngüyü izlemek üzere değiştirin.

```csharp
public static async Task VerificationEnroll(SpeechConfig config, Dictionary<string, string> profileMapping)
{
    using (var client = new VoiceProfileClient(config))
    using (var profile = await client.CreateProfileAsync(VoiceProfileType.TextIndependentVerification, "en-us"))
    {
        using (var audioInput = AudioConfig.FromDefaultMicrophoneInput())
        {
            Console.WriteLine($"Enrolling profile id {profile.Id}.");
            // give the profile a human-readable display name
            profileMapping.Add(profile.Id, "Your Name");

            VoiceProfileEnrollmentResult result = null;
            while (result is null || result.RemainingEnrollmentsSpeechLength > TimeSpan.Zero)
            {
                Console.WriteLine("Continue speaking to add to the profile enrollment sample.");
                result = await client.EnrollProfileAsync(profile, audioInput);
                Console.WriteLine($"Remaining enrollment audio time needed: {result.RemainingEnrollmentsSpeechLength}");
                Console.WriteLine("");
            }
            
            if (result.Reason == ResultReason.EnrolledVoiceProfile)
            {
                await SpeakerVerify(config, profile, profileMapping);
            }
            else if (result.Reason == ResultReason.Canceled)
            {
                var cancellation = VoiceProfileEnrollmentCancellationDetails.FromResult(result);
                Console.WriteLine($"CANCELED {profile.Id}: ErrorCode={cancellation.ErrorCode} ErrorDetails={cancellation.ErrorDetails}");
            }
        }
    }
}
```

Programı yeniden çalıştırın ve bir geçiş ifadesi gerekli olmadığından doğrulama aşamasında her şeyi konuşun. Yine, benzerlik puanı döndürülür.

```shell
Enrolling profile id 4tt87d4-f2d3-44ae-b5b4-f1a8d4036ee9.
Continue speaking to add to the profile enrollment sample.
Remaining enrollment audio time needed: 00:00:15.3200000

Continue speaking to add to the profile enrollment sample.
Remaining enrollment audio time needed: 00:00:09.8100008

Continue speaking to add to the profile enrollment sample.
Remaining enrollment audio time needed: 00:00:05.1900000

Continue speaking to add to the profile enrollment sample.
Remaining enrollment audio time needed: 00:00:00.8700000

Continue speaking to add to the profile enrollment sample.
Remaining enrollment audio time needed: 00:00:00

Speak the passphrase to verify: "My voice is my passport, please verify me."
Verified voice profile for speaker Your Name, score is 0.849409
```

## <a name="speaker-identification"></a>Konuşmacı belirleme

Konuşmacı kimliği, belirli bir kayıtlı ses grubundan **kimin** konuşduğunu tespit etmek için kullanılır. Bu işlem, tek bir profile karşı doğrulama yerine, aynı anda birden çok ses profiline karşı, ana fark ile **metin bağımsız doğrulamaya**çok benzer.

`IdentificationEnroll`Birden çok ses profilini kaydetmek için bir işlev oluşturun. Her profil için kayıt işlemi, **metin bağımsız doğrulama**için kayıt işlemiyle aynıdır ve her profil için 20 saniyelik ses gerektirir. Bu işlev, dizelerin listesini kabul eder `profileNames` ve listedeki her ad için yeni bir ses profili oluşturur. İşlevi `VoiceProfile` bir konuşmacı tanımlamak için bir sonraki işlevde kullandığınız nesne listesini döndürür.

```csharp
public static async Task<List<VoiceProfile>> IdentificationEnroll(SpeechConfig config, List<string> profileNames, Dictionary<string, string> profileMapping)
{
    List<VoiceProfile> voiceProfiles = new List<VoiceProfile>();
    using (var client = new VoiceProfileClient(config))
    {
        foreach (string name in profileNames)
        {
            using (var audioInput = AudioConfig.FromDefaultMicrophoneInput())
            {
                var profile = await client.CreateProfileAsync(VoiceProfileType.TextIndependentIdentification, "en-us");
                Console.WriteLine($"Creating voice profile for {name}.");
                profileMapping.Add(profile.Id, name);

                VoiceProfileEnrollmentResult result = null;
                while (result is null || result.RemainingEnrollmentsSpeechLength > TimeSpan.Zero)
                {
                    Console.WriteLine($"Continue speaking to add to the profile enrollment sample for {name}.");
                    result = await client.EnrollProfileAsync(profile, audioInput);
                    Console.WriteLine($"Remaining enrollment audio time needed: {result.RemainingEnrollmentsSpeechLength}");
                    Console.WriteLine("");
                }
                voiceProfiles.Add(profile);
            }
        }
    }
    return voiceProfiles;
}
```

`SpeakerIdentification`Bir kimlik isteği göndermek için aşağıdaki işlevi oluşturun. Bu işlevdeki bir **Konuşmacı doğrulama** isteğiyle karşılaştırılan ana fark, `SpeakerIdentificationModel.FromProfiles()` bir nesne listesini kabul eden öğesinin kullanımı olur `VoiceProfile` . 

```csharp
public static async Task SpeakerIdentification(SpeechConfig config, List<VoiceProfile> voiceProfiles, Dictionary<string, string> profileMapping) 
{
    var speakerRecognizer = new SpeakerRecognizer(config, AudioConfig.FromDefaultMicrophoneInput());
    var model = SpeakerIdentificationModel.FromProfiles(voiceProfiles);

    Console.WriteLine("Speak some text to identify who it is from your list of enrolled speakers.");
    var result = await speakerRecognizer.RecognizeOnceAsync(model);
    Console.WriteLine($"The most similiar voice profile is {profileMapping[result.ProfileId]} with similiarity score {result.Score}");
}
```

`Main()`İşlevinizi aşağıdaki şekilde değiştirin. İşlevinizle geçirdiğiniz dizelerin bir listesini oluşturursunuz `profileNames` `IdentificationEnroll()` . Böylece, bu listedeki her bir ad için yeni bir ses profili oluşturmanız istenir. böylece, arkadaşlar veya iş arkadaşları için ek profiller oluşturmak üzere daha fazla ad ekleyebilirsiniz.

```csharp
static async Task Main(string[] args)
{
    // replace with your own subscription key 
    string subscriptionKey = "YourSubscriptionKey";
    string region = "westus";
    var config = SpeechConfig.FromSubscription(subscriptionKey, region);

    // persist profileMapping if you want to store a record of who the profile is
    var profileMapping = new Dictionary<string, string>();
    var profileNames = new List<string>() { "Your name", "A friend's name" };
    
    var enrolledProfiles = await IdentificationEnroll(config, profileNames, profileMapping);
    await SpeakerIdentification(config, enrolledProfiles, profileMapping);

    foreach (var profile in enrolledProfiles)
    {
        profile.Dispose();
    }
    Console.ReadLine();
}
```

Betiği çalıştırın ve ilk profil için sesli örnekleri kaydetmek üzere konuşmak isteyip istemediğiniz sorulur. Kayıt tamamlandıktan sonra, listedeki her ad için bu işlemi tekrarlamanız istenir `profileNames` . Her kayıt bittikten sonra, **herhangi** bir kişiye konuştuğunuz istenir ve hizmet bu kişiyi kayıtlı ses profilleriniz arasından tanımaya çalışacaktır.

Bu örnek yalnızca en yakın eşleşmeyi döndürür ve benzerlik puanı olur, ancak işlevinize ekleyerek en iyi beş benzerlik puanlarını içeren tam yanıtı elde edebilirsiniz `string json = result.Properties.GetProperty(PropertyId.SpeechServiceResponse_JsonResult)` `SpeakerIdentification` .

## <a name="changing-audio-input-type"></a>Ses giriş türünü değiştirme

Bu makaledeki örneklerde, ses örnekleri için varsayılan cihaz mikrofonu giriş olarak kullanılır. Ancak, mikrofon girişi yerine ses dosyalarını kullanmanız gereken senaryolarda, `AudioConfig.FromDefaultMicrophoneInput()` `AudioConfig.FromWavFileInput(path/to/your/file.wav)` bir dosya girdisine geçiş yapmak için herhangi bir örneğini ' a değiştirmeniz yeterlidir. Ayrıca, örneğin, kayıt ve dosyalar için bir mikrofon kullanarak, örneğin, doğrulama için bir mikrofon kullanarak karma girdilere sahip olabilirsiniz.

## <a name="deleting-voice-profile-enrollments"></a>Ses profili kayıtlarını silme

Kayıtlı bir profili silmek için `DeleteProfileAsync()` nesnesi üzerindeki işlevini kullanın `VoiceProfileClient` . Aşağıdaki örnek işlev, bilinen bir ses profili KIMLIĞINDEN bir ses profilinin nasıl silineceğini gösterir.

```csharp
public static async Task DeleteProfile(SpeechConfig config, string profileId) 
{
    using (var client = new VoiceProfileClient(config))
    {
        var profile = new VoiceProfile(profileId);
        await client.DeleteProfileAsync(profile);
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Sınıflar ve işlevler hakkında ayrıntılı bilgi için konuşmacı tanıma [başvurusu belgelerine](https://docs.microsoft.com/rest/api/speakerrecognition/) bakın.

* GitHub 'da [C#](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/csharp/dotnet/speaker-recognition) ve [C++](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows/speaker-recognition) örneklerine bakın.

