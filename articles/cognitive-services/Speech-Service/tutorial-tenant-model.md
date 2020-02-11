---
title: Kiracı modeli oluşturma (Önizleme)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: ', Kuruluşa özgü şartlar için en iyi konuşma tanımayı sunmak üzere Office 365 verilerinizi kullanan güvenli, uyumlu bir kiracı modelini (Office 365 verileriyle Özel Konuşma Tanıma) otomatik olarak oluşturun.'
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: erhopf
ms.openlocfilehash: 54e2516211495f7f044f716c518291ca8008f92d
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77118894"
---
# <a name="tutorial-create-a-tenant-model-preview"></a>Öğretici: kiracı modeli oluşturma (Önizleme)

Kiracı modeli (Office 365 verileriyle Özel Konuşma Tanıma), kuruluşunuzun Office 365 verilerinden otomatik olarak özel bir konuşma tanıma modeli üreten Office 365 kurumsal müşterilerine yönelik bir katılım hizmetidir. Model, teknik koşullar, jargon ve kişilerin adları için en iyi duruma getirilmiştir, güvenli ve uyumlu bir şekilde yapılır.

> [!IMPORTANT]
> Kuruluşunuz kiracı modeli hizmetini kullanarak kaydolur, konuşma hizmeti kuruluşunuzun dil modeline erişebilir. Model, kuruluşunuzdaki herkes tarafından görülemeyen Office 365 Genel Grup e-postalarından ve belgelerinden oluşturulur. Kuruluşunuzun Office 365 Yöneticisi, Office 365 Yönetim portalından kuruluş genelindeki dil modelinin kullanımını açabilir veya kapatabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Microsoft 365 Yönetim merkezini kullanarak kiracı modeline kaydolma
> * Konuşma aboneliği anahtarı al
> * Kiracı modeli oluşturma
> * Kiracı modeli dağıtma
> * Konuşma SDK 'sını kullanarak kiracı modelinizi kullanın

## <a name="enroll-in-the-tenant-model-service"></a>Kiracı model hizmetine kaydolma

Kiracı modelinizi dağıtabilmeniz için önce kiracı modeli hizmetine kaydolmuş olmanız gerekir. Kayıt, Microsoft 365 Yönetim merkezinde tamamlanır ve yalnızca Microsoft 365 yöneticiniz tarafından yapılabilir.

1. [Microsoft 365 Yönetim merkezinde](https://admin.microsoft.com)oturum açın.

1. Sol bölmede **Ayarlar**' ı seçin, ardından iç Içe menüsünden **Ayarlar** ' ı seçin ve ardından Ana penceredeki **Azure konuşma Hizmetleri** ' ni seçin.

   !["Hizmetler & eklentileri" bölmesi](media/tenant-language-model/tenant-language-model-enrollment.png)

1. **Kuruluş genelinde dil modeline Izin ver** onay kutusunu seçin ve ardından **Değişiklikleri Kaydet**' i seçin.

   ![Azure konuşma Hizmetleri bölmesi](media/tenant-language-model/tenant-language-model-enrollment-2.png)

Kiracı model örneğini devre dışı bırakmak için:
1. Yukarıdaki 1. ve 2. adımları yineleyin.
1. **Kuruluş genelinde dil modeline Izin ver** onay kutusunu temizleyin ve ardından **Değişiklikleri Kaydet**' i seçin.

## <a name="get-a-speech-subscription-key"></a>Konuşma aboneliği anahtarı al

Kiracı modelinizi konuşma SDK 'Sı ile kullanmak için bir konuşma kaynağına ve onunla ilişkili abonelik anahtarına ihtiyacınız vardır.

1. [Azure Portal](https://aka.ms/azureportal)’ında oturum açın.
1. **Kaynak oluştur**' u seçin.
1. **Arama** kutusuna **konuşma**yazın.
1. Sonuçlar listesinde **konuşma**' yı seçin ve ardından **Oluştur**' u seçin.
1. Kaynağı oluşturmak için ekrandaki yönergeleri izleyin. Şunları yaptığınızdan emin olun:
   * **Konum** **eastus** veya **westus**olarak ayarlanır.
   * **Fiyatlandırma katmanı** **S0**olarak ayarlanır.
1. **Oluştur**'u seçin.

   Birkaç dakika sonra kaynağınız oluşturulur. Abonelik anahtarı, kaynağınızın **genel bakış** bölümünde bulunabilir.

## <a name="create-a-language-model"></a>Dil modeli oluşturma

Yöneticiniz kuruluşunuz için kiracı modelini etkinleştirdikten sonra, Office 365 verilerinizi temel alan bir dil modeli oluşturabilirsiniz.

1. [Konuşma Studio](https://speech.microsoft.com/)'da oturum açın.
1. Sağ üst köşedeki **Ayarlar** ' ı (dişli simgesi) seçin ve ardından **kiracı modeli ayarları**' nı seçin.

   !["Kiracı model ayarları" bağlantısı](media/tenant-language-model/tenant-language-settings.png)

   Konuşma Studio, kiracı modeli oluşturmak için uygun olup olmadığını bilmenizi sağlayan bir ileti görüntüler.

   > [!NOTE]
   > Kuzey Amerika 'de Office 365 Kurumsal müşterileri bir kiracı modeli (Ingilizce) oluşturmaya uygundur. Müşteri Kasası, müşteri anahtarı veya Office 365 Kamu müşterisiyseniz bu özellik kullanılamaz. Müşteri Kasası veya müşteri anahtarı müşterisi olup olmadığınızı öğrenmek için bkz.:
   > * [Müşteri Kasası](https://docs.microsoft.com/office365/securitycompliance/controlling-your-data-using-customer-key#FastTrack)
   > * [Müşteri anahtarı](https://docs.microsoft.com/microsoft-365/compliance/customer-lockbox-requests)
   > * [Office 365 Kamu](https://www.microsoft.com/microsoft-365/government)

1. **Kabul et**’i seçin.

   Kiracı modeliniz hazırlandığınızda, daha fazla yönerge içeren bir onay e-posta iletisi alırsınız.

## <a name="deploy-your-tenant-model"></a>Kiracı modelinizi dağıtın

Kiracı modeli örneğiniz hazırlandığınızda, aşağıdakileri yaparak dağıtın:

1. Onay e-posta iletinizde **modeli görüntüle** düğmesini seçin. Veya [konuşma Studio](https://speech.microsoft.com/)'da oturum açın.
1. Sağ üst köşedeki **Ayarlar** ' ı (dişli simgesi) seçin ve ardından **kiracı modeli ayarları**' nı seçin.

   !["Kiracı model ayarları" bağlantısı](media/tenant-language-model/tenant-language-settings.png)

1. **Dağıt**'ı seçin.

   Modeliniz dağıtıldığında, durum *dağıtıldı*olarak değişir.

## <a name="use-your-tenant-model-with-the-speech-sdk"></a>Konuşma SDK 'sını kullanarak kiracı modelinizi kullanın

Modelinizi dağıttığınıza göre, artık bunu konuşma SDK 'Sı ile kullanabilirsiniz. Bu bölümde, Azure Active Directory (Azure AD) kimlik doğrulaması kullanarak konuşma hizmetini çağırmak için örnek kod kullanırsınız.

' De C#konuşma SDK 'sını çağırmak için kullanacağınız koda göz atalım. Bu örnekte, kiracı modelinizi kullanarak konuşma tanımayı gerçekleştirirsiniz. Bu kılavuz, platformunuzun zaten ayarlandığı varsayılır. Kurulum Yardımı 'na ihtiyacınız varsa bkz. [hızlı başlangıç: konuşmayı tanıma C# , (.NET Core)](quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore).

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

Ardından, komut satırından projeyi yeniden oluşturup çalıştırmanız gerekir. Komutu çalıştırmadan önce, aşağıdakileri yaparak birkaç parametreyi güncelleştirin:

1. `<Username>` ve `<Password>` değerlerini geçerli bir kiracı kullanıcısının değerleriyle değiştirin.
1. `<Subscription-Key>`, konuşma kaynağınızın abonelik anahtarıyla değiştirin. Bu değer, [Azure Portal](https://aka.ms/azureportal)konuşma kaynağınızın **genel bakış** bölümünde bulunur.
1. `<Endpoint-Uri>` aşağıdaki uç noktayla değiştirin. `{your region}`, konuşma kaynağınızın oluşturulduğu bölge ile değiştirdiğinizden emin olun. Bu bölgeler desteklenir: `westus`, `westus2`ve `eastus`. Bölge bilgileriniz, [Azure Portal](https://aka.ms/azureportal)konuşma kaynağınızın **genel bakış** bölümünde bulunabilir.
   ```
   "wss://{your region}.online.princeton.customspeech.ai/msgraphcustomspeech/conversation/v1".
   ```
1. Şu komutu çalıştırın:

   ```bash
   dotnet TenantLMSample.dll --Username=<Username> --Password=<Password> --SubscriptionKey=<Subscription-Key> --EndpointUri=<Endpoint-Uri>
   ```

Bu öğreticide, Office 365 verilerini kullanarak özel bir konuşma tanıma modeli oluşturmayı, dağıtmayı ve konuşma SDK 'Sı ile kullanmayı öğrendiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma Studio](https://speech.microsoft.com/)
* [Konuşma SDK'sı](speech-sdk.md)
