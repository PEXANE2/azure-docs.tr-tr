---
title: Kiracı modeli oluşturma (önizleme) - Konuşma Hizmeti
titleSuffix: Azure Cognitive Services
description: Kuruluşa özel terimler için en iyi konuşma tanımayı sunmak için Office 365 verilerinizi kullanan güvenli ve uyumlu bir kiracı modeli (Office 365 verileriyle Özel Konuşma) otomatik olarak oluşturun.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: erhopf
ms.openlocfilehash: a83ed5c9cec994c1bc4cadd5cf6208c159823658
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77469026"
---
# <a name="tutorial-create-a-tenant-model-preview"></a>Öğretici: Kiracı modeli oluşturma (önizleme)

Kiracı Modeli (Office 365 verileriyle Özel Konuşma), kuruluşunuzun Office 365 verilerinden otomatik olarak özel bir konuşma tanıma modeli oluşturan Office 365 kurumsal müşterileri için bir kabul hizmetidir. Model, teknik terimler, jargon ve kişilerin adları için güvenli ve uyumlu bir şekilde optimize edilmiştir.

> [!IMPORTANT]
> Kuruluşunuz Kiracı Modeli hizmetini kullanarak kaydoluyorsa, Konuşma Hizmeti kuruluşunuzun dil modeline erişebilir. Model, kuruluşunuzdaki herkes tarafından görülebilen Office 365 ortak grup e-postalarından ve belgelerinden oluşturulur. Kuruluşunuzun Office 365 yöneticisi, Office 365 yönetici portalından kuruluş genelindeki dil modelini açabilir veya kullanabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Microsoft 365 yönetici merkezini kullanarak Kiracı Modeline kaydolun
> * Konuşma abonelik anahtarı alın
> * Kiracı modeli oluşturma
> * Kiracı modelini dağıtma
> * Konuşma SDK ile kiracı modelinizi kullanın

## <a name="enroll-in-the-tenant-model-service"></a>Kiracı Modeli hizmetine kaydolun

Kiracı modelinizi dağıtmadan önce Kiracı Modeli hizmetine kaydolmanız gerekir. Kayıt Microsoft 365 yönetici merkezinde tamamlanır ve yalnızca Microsoft 365 yöneticiniz tarafından yapılabilir.

1. [Microsoft 365 yönetim merkezinde](https://admin.microsoft.com) oturum açın.

1. Sol bölmede **Ayarlar'ı**seçin, ardından iç içe açılan menüden **Ayarlar'ı** seçin ve ardından ana pencereden **Azure Konuşma Hizmetleri'ni** seçin.

   !["Hizmetler & eklentileri" bölmesi](media/tenant-language-model/tenant-language-model-enrollment.png)

1. Kuruluş **genelindeki dil modeli** onay kutusunu İzin ver'i seçin ve ardından **değişiklikleri kaydet'i**seçin.

   ![Azure Konuşma Hizmetleri bölmesi](media/tenant-language-model/tenant-language-model-enrollment-2.png)

Kiracı modeli örneğini kapatmak için:
1. Önceki adımları 1 ve 2'yi yineleyin.
1. Kuruluş **genelindeki dil modeli** onay kutusunu İzin ver ve ardından **değişiklikleri kaydet'i**seçin.

## <a name="get-a-speech-subscription-key"></a>Konuşma abonelik anahtarı alın

Kiracı modelinizi Konuşma SDK ile kullanmak için bir Konuşma kaynağına ve ilişkili abonelik anahtarına ihtiyacınız vardır.

1. [Azure portalında](https://aka.ms/azureportal)oturum açın.
1. **Kaynak oluştur**’u seçin.
1. **Arama** kutusunda Konuşma **yazın.**
1. Sonuç listesinde **Konuşma'yı**seçin ve ardından **Oluştur'u**seçin.
1. Kaynağınızı oluşturmak için ekrandaki yönergeleri izleyin. Şunlardan emin olun:
   * **Konumu** **eastus** veya **westus**olarak ayarlanır.
   * **Fiyatlandırma katmanı** **S0**olarak ayarlanır.
1. **Oluştur'u**seçin.

   Birkaç dakika sonra kaynağınız oluşturulur. Abonelik anahtarı kaynağınız için **Genel Bakış** bölümünde kullanılabilir.

## <a name="create-a-language-model"></a>Dil modeli oluşturma

Yöneticiniz kuruluşunuz için Kiracı Modeli'ni etkinleştirdikten sonra Office 365 verilerinizi temel alan bir dil modeli oluşturabilirsiniz.

1. [Konuşma Stüdyosu'nda](https://speech.microsoft.com/)oturum açın.
1. Sağ üstte **Ayarlar'ı** (vites simgesi) seçin ve ardından **Kiracı Modeli ayarlarını**seçin.

   !["Kiracı Model ayarları" bağlantısı](media/tenant-language-model/tenant-language-settings.png)

   Speech Studio, kiracı modeli oluşturmak için nitelikli olup olmadığınızı bilmenizi sağlayan bir ileti görüntüler.

   > [!NOTE]
   > Kuzey Amerika'daki Office 365 kurumsal müşterileri kiracı modeli (İngilizce) oluşturabilir. Müşteri Kilit Kutusu, Müşteri Anahtarı veya Office 365 Devlet müşterisiyseniz, bu özellik kullanılamaz. Müşteri Kilit Kutusu veya Müşteri Anahtarı müşterisi olup olmadığınızı belirlemek için bkz:
   > * [Müşteri Kasası](/microsoft-365/compliance/customer-lockbox-requests)
   > * [Müşteri Anahtarı](/microsoft-365/compliance/customer-key-overview)
   > * [Office 365 Hükümet](https://www.microsoft.com/microsoft-365/government)

1. **Kabul et**’i seçin.

   Kiracı modeliniz hazır olduğunda, başka talimatlar içeren bir onay e-posta iletisi alırsınız.

## <a name="deploy-your-tenant-model"></a>Kiracı modelinizi dağıtma

Kiracı model örneğiniz hazır olduğunda, aşağıdakileri yaparak dağıtın:

1. Onay e-posta iletinizde **Modeli Görüntüle** düğmesini seçin. Ya da [Speech Studio'da](https://speech.microsoft.com/)oturum açın.
1. Sağ üstte **Ayarlar'ı** (vites simgesi) seçin ve ardından **Kiracı Modeli ayarlarını**seçin.

   !["Kiracı Model ayarları" bağlantısı](media/tenant-language-model/tenant-language-settings.png)

1. **Dağıt**'ı seçin.

   Modeliniz dağıtıldığında, durum *Dağıtıldı*olarak değişir.

## <a name="use-your-tenant-model-with-the-speech-sdk"></a>Konuşma SDK ile kiracı modelinizi kullanın

Artık modelinizi dağıttığınıza göre, Konuşma SDK ile kullanabilirsiniz. Bu bölümde, Azure Etkin Dizin (Azure AD) kimlik doğrulaması kullanarak Konuşma Hizmeti'ni aramak için örnek kodu kullanırsınız.

C#'daki Konuşma SDK'sını aramak için kullanacağınız koda bakalım. Bu örnekte, kiracı modelinizi kullanarak konuşma tanıma gerçekleştirin. Bu kılavuz, platformunuzun zaten ayarlandığını varsayılır. Kurulum yardımına ihtiyacınız varsa, [bkz.](quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore)

Bu kodu projenize kopyalayın:

```csharp
namespace PrincetonSROnly.FrontEnd.Samples
{
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Net.Http;
    using System.Text;
    using System.Text.RegularExpressions;
    using System.Threading.Tasks;
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Audio;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json.Linq;

    // ServiceApplicationId is a fixed value. No need to change it.

    public class TenantLMSample
    {
        private const string EndpointUriArgName = "EndpointUri";
        private const string SubscriptionKeyArgName = "SubscriptionKey";
        private const string UsernameArgName = "Username";
        private const string PasswordArgName = "Password";
        private const string ClientApplicationId = "f87bc118-1576-4097-93c9-dbf8f45ef0dd";
        private const string ServiceApplicationId = "18301695-f99d-4cae-9618-6901d4bdc7be";

        public static async Task ContinuousRecognitionWithTenantLMAsync(Uri endpointUri, string subscriptionKey, string audioDirPath, string username, string password)
        {
            var config = SpeechConfig.FromEndpoint(endpointUri, subscriptionKey);

            // Passing client specific information for obtaining LM
            if (string.IsNullOrEmpty(username) || string.IsNullOrEmpty(password))
            {
                config.AuthorizationToken = await AcquireAuthTokenWithInteractiveLoginAsync().ConfigureAwait(false);
            }
            else
            {
                config.AuthorizationToken = await AcquireAuthTokenWithUsernamePasswordAsync(username, password).ConfigureAwait(false);
            }

            var stopRecognition = new TaskCompletionSource<int>();

            // Creates a speech recognizer using file as audio input.
            // Replace with your own audio file name.
            using (var audioInput = AudioConfig.FromWavFileInput(audioDirPath))
            {
                using (var recognizer = new SpeechRecognizer(config, audioInput))
                {
                    // Subscribes to events
                    recognizer.Recognizing += (s, e) =>
                    {
                        Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                    };

                    recognizer.Recognized += (s, e) =>
                    {
                        if (e.Result.Reason == ResultReason.RecognizedSpeech)
                        {
                            Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}");
                        }
                        else if (e.Result.Reason == ResultReason.NoMatch)
                        {
                            Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                        }
                    };

                    recognizer.Canceled += (s, e) =>
                    {
                        Console.WriteLine($"CANCELED: Reason={e.Reason}");
                        if (e.Reason == CancellationReason.Error)
                        {
                            Exception exp = new Exception(string.Format("Error Code: {0}\nError Details{1}\nIs your subscription information updated?", e.ErrorCode, e.ErrorDetails));
                            throw exp;
                        }

                        stopRecognition.TrySetResult(0);
                    };

                    recognizer.SessionStarted += (s, e) =>
                    {
                        Console.WriteLine("\n    Session started event.");
                    };

                    recognizer.SessionStopped += (s, e) =>
                    {
                        Console.WriteLine("\n    Session stopped event.");
                        Console.WriteLine("\nStop recognition.");
                        stopRecognition.TrySetResult(0);
                    };

                    // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
                    await recognizer.StartContinuousRecognitionAsync().ConfigureAwait(false);

                    // Waits for completion.
                    // Use Task.WaitAny to keep the task rooted.
                    Task.WaitAny(new[] { stopRecognition.Task });

                    // Stops recognition.
                    await recognizer.StopContinuousRecognitionAsync().ConfigureAwait(false);
                }
            }
        }

        public static void Main(string[] args)
        {
            var arguments = new Dictionary<string, string>();
            string inputArgNamePattern = "--";
            Regex regex = new Regex(inputArgNamePattern);
            if (args.Length > 0)
            {
                foreach (var arg in args)
                {
                    var userArgs = arg.Split("=");
                    arguments[regex.Replace(userArgs[0], string.Empty)] = userArgs[1];
                }
            }

            var endpointString = arguments.GetValueOrDefault(EndpointUriArgName, $"wss://westus.online.princeton.customspeech.ai/msgraphcustomspeech/conversation/v1");
            var endpointUri = new Uri(endpointString);

            if (!arguments.ContainsKey(SubscriptionKeyArgName))
            {
                Exception exp = new Exception("Subscription Key missing! Please pass in a Cognitive services subscription Key using --SubscriptionKey=\"your_subscription_key\"" +
                    "Find more information on creating a Cognitive services resource and accessing your Subscription key here: https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows");
                throw exp;
            }

            var subscriptionKey = arguments[SubscriptionKeyArgName];
            var username = arguments.GetValueOrDefault(UsernameArgName, null);
            var password = arguments.GetValueOrDefault(PasswordArgName, null);

            var audioDirPath = Path.Combine(Path.GetDirectoryName(System.Reflection.Assembly.GetExecutingAssembly().Location), "../../../AudioSamples/DictationBatman.wav");
            if (!File.Exists(audioDirPath))
            {
                Exception exp = new Exception(string.Format("Audio File does not exist at path: {0}", audioDirPath));
                throw exp;
            }

            ContinuousRecognitionWithTenantLMAsync(endpointUri, subscriptionKey, audioDirPath, username, password).GetAwaiter().GetResult();
        }

        private static async Task<string> AcquireAuthTokenWithUsernamePasswordAsync(string username, string password)
        {
            var tokenEndpoint = "https://login.microsoftonline.com/common/oauth2/token";
            var postBody = $"resource={ServiceApplicationId}&client_id={ClientApplicationId}&grant_type=password&username={username}&password={password}";
            var stringContent = new StringContent(postBody, Encoding.UTF8, "application/x-www-form-urlencoded");
            using (HttpClient httpClient = new HttpClient())
            {
                var response = await httpClient.PostAsync(tokenEndpoint, stringContent).ConfigureAwait(false);

                if (response.IsSuccessStatusCode)
                {
                    var result = await response.Content.ReadAsStringAsync().ConfigureAwait(false);

                    JObject jobject = JObject.Parse(result);
                    return jobject["access_token"].Value<string>();
                }
                else
                {
                    throw new Exception($"Requesting token from {tokenEndpoint} failed with status code {response.StatusCode}: {await response.Content.ReadAsStringAsync().ConfigureAwait(false)}");
                }
            }
        }

        private static async Task<string> AcquireAuthTokenWithInteractiveLoginAsync()
        {
            var authContext = new AuthenticationContext("https://login.windows.net/microsoft.onmicrosoft.com");
            var deviceCodeResult = await authContext.AcquireDeviceCodeAsync(ServiceApplicationId, ClientApplicationId).ConfigureAwait(false);

            Console.WriteLine(deviceCodeResult.Message);

            var authResult = await authContext.AcquireTokenByDeviceCodeAsync(deviceCodeResult).ConfigureAwait(false);

            return authResult.AccessToken;
        }
    }
}
```

Ardından, projeyi komut satırından yeniden oluşturmanız ve çalıştırmanız gerekir. Komutu çalıştırmadan önce, aşağıdakileri yaparak birkaç parametreyi güncelleştirin:

1. Geçerli `<Username>` `<Password>` bir kiracı kullanıcı için değerleri değiştirin ve değiştirin.
1. Konuşma `<Subscription-Key>` kaynağınız için abonelik anahtarıyla değiştirin. Bu değer, [Azure portalındaki](https://aka.ms/azureportal)Konuşma kaynağınız için **Genel Bakış** bölümünde kullanılabilir.
1. Aşağıdaki `<Endpoint-Uri>` bitiş noktasıyla değiştirin. Konuşma kaynağınızın `{your region}` oluşturulduğu bölgeyle değiştirdiğinizden emin olun. Bu bölgeler `westus`desteklenir: `westus2`, `eastus`, ve . Bölge bilgileriniz, [Azure portalındaki](https://aka.ms/azureportal)Konuşma kaynağınızın **Genel Bakış** bölümünde mevcuttur.
   ```
   "wss://{your region}.online.princeton.customspeech.ai/msgraphcustomspeech/conversation/v1".
   ```
1. Şu komutu çalıştırın:

   ```bash
   dotnet TenantLMSample.dll --Username=<Username> --Password=<Password> --SubscriptionKey=<Subscription-Key> --EndpointUri=<Endpoint-Uri>
   ```

Bu eğitimde, özel bir konuşma tanıma modeli oluşturmak, dağıtmak ve Konuşma SDK ile kullanmak için Office 365 verilerini nasıl kullanacağınızı öğrendiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma Stüdyosu](https://speech.microsoft.com/)
* [Konuşma SDK'sı](speech-sdk.md)
