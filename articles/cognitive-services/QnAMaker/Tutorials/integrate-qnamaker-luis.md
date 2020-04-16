---
title: LUIS ve QnAMaker - bot entegrasyonu
titleSuffix: Azure Cognitive Services
description: QnA Maker bilgi tabanınız büyüdükçe, tek bir yekpare küme olarak bunu sürdürmek zorlaşır. Bilgi tabanını daha küçük, mantıksal parçalara bölün.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 09/26/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b0d28c77966668f919cdf1265f8cc63b4931d5fd
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402715"
---
# <a name="use-a-bot-with-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Bilgi tabanınızı dağıtmak için QnA Maker ve LUIS ile bir bot kullanın
QnA Maker bilgi tabanınız büyüdükçe, tek bir yekpare küme olarak bunu sürdürmek zorlaşır. Bilgi tabanını daha küçük, mantıksal parçalara bölün.

QnA Maker'da birden çok bilgi tabanı oluşturmak kolay olsa da, gelen soruyu uygun bilgi tabanına yönlendirmek için bazı mantık gerekir. Bunu LUIS'i kullanarak yapabilirsiniz.

Bu makalede Bot Framework v3 SDK kullanır. Bu bilgilerin Bot Framework v4 SDK sürümüyle ilgileniyorsanız, [bkz.](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&tabs=csharp)

## <a name="architecture"></a>Mimari

![Dil Anlayışı ile QnA Maker mimarisigösteren grafik](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

Önceki grafik QnA Maker ilk bir LUIS modeli gelen soru niyetalır gösterir. Daha sonra, QnA Maker doğru QnA Maker bilgi tabanına soru yönlendirmek için bu niyeti kullanır.

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

1. [LUIS](https://www.luis.ai/) portalında oturum açın.
1. [Bir uygulama oluşturun.](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app)
1. Her QnA Maker bilgi tabanı için [bir niyet ekleyin.](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) Örnek söyleyinçler QnA Maker bilgi üslerindeki sorulara karşılık vermelidir.
1. [LUIS uygulamasını eğitin](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) ve [LUIS uygulamasını yayınlayın.](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp)
1. **Yönet** bölümünde, LUIS uygulama kimliğinizi, LUIS uç noktası anahtarınızı ve [özel alan adınızı](../../cognitive-services-custom-subdomains.md)not edin. Bu değerlere daha sonra ihtiyacınız olacaktır.

## <a name="create-qna-maker-knowledge-bases"></a>QnA Maker bilgi tabanları oluşturun

1. [QnA Maker'da](https://qnamaker.ai)oturum açın.
1. LUIS uygulamasında her amaç için bir bilgi tabanı [oluşturun.](https://www.qnamaker.ai/Create)
1. Bilgi tabanlarını test edin ve yayınlayın. Her birini yayımladığınızda, kimliği, kaynak adını _(.azurewebsites.net/qnamaker'dan_önceki özel alt etki alanı) ve yetkilendirme bitiş noktası anahtarına dikkat edin. Bu değerlere daha sonra ihtiyacınız olacaktır.

    Bu makalede, bilgi tabanlarının tümü aynı Azure QnA Maker aboneliğinde oluşturulmuştur varsayar.

    ![QnA Maker HTTP isteğinin ekran görüntüsü](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

## <a name="web-app-bot"></a>Web uygulaması botu

1. Otomatik olarak bir LUIS uygulaması içeren [Azure Bot Hizmeti ile bir "Temel" bot oluşturun.](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0) C# programlama dilini seçin.

1. Azure portalında web uygulaması botu oluşturduktan sonra web uygulaması botu'nu seçin.
1. Web uygulaması bot hizmeti navigasyon, **Uygulama Ayarları**seçin. Ardından kullanılabilir ayarların **Uygulama ayarları** bölümüne aşağı kaydırın.
1. **LuisAppId'i** önceki bölümde oluşturulan LUIS uygulamasının değeriyle değiştirin. Ardından **Kaydet'i**seçin.


## <a name="change-the-code-in-the-basicluisdialogcs-file"></a>BasicLuisDialog.cs dosyasındaki kodu değiştirme
1. Azure portalında web uygulaması bot navigasyonbot Bot **Yönetimi** bölümünden, **Oluştur'u**seçin.
2. **Çevrimiçi kod düzenleyicisini aç'ı**seçin. Çevrimiçi düzenleme ortamıyla yeni bir tarayıcı sekmesi açılır.
3. **WWWROOT** bölümünde, İletişim **dizini** seçin ve ardından **BasicLuisDialog.cs**açın.
4. **BasicLuisDialog.cs** dosyasının üst bölümüne bağımlılıklar ekleyin:

    ```csharp
    using System;
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Bot.Builder.Luis;
    using Microsoft.Bot.Builder.Luis.Models;
    using Newtonsoft.Json;
    using System.Text;
    ```

5. QnA Maker yanıtını deserialize etmek için aşağıdaki sınıfları ekleyin:

    ```csharp
    public class Metadata
    {
        public string name { get; set; }
        public string value { get; set; }
    }

    public class Answer
    {
        public IList<string> questions { get; set; }
        public string answer { get; set; }
        public double score { get; set; }
        public int id { get; set; }
        public string source { get; set; }
        public IList<object> keywords { get; set; }
        public IList<Metadata> metadata { get; set; }
    }

    public class QnAAnswer
    {
        public IList<Answer> answers { get; set; }
    }
    ```


6. QnA Maker hizmetine http isteğinde bulunmak için aşağıdaki sınıfı ekleyin. **Yetkilendirme** üstbilgisinin değerinin sözcüğü `EndpointKey`izleyen bir boşluk içeren sözcüğü içerdiğine dikkat edin. JSON sonucu önceki sınıflara deserialized ve ilk yanıt döndürülür.

    ```csharp
    [Serializable]
    public class QnAMakerService
    {
        private string qnaServiceResourceName;
        private string knowledgeBaseId;
        private string endpointKey;

        public QnAMakerService(string resourceName, string kbId, string endpointkey)
        {
            qnaServiceResourceName = resourceName;
            knowledgeBaseId = kbId;
            endpointKey = endpointkey;

        }
        async Task<string> Post(string uri, string body)
        {
            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(body, Encoding.UTF8, "application/json");
                request.Headers.Add("Authorization", "EndpointKey " + endpointKey);

                var response = await client.SendAsync(request);
                return  await response.Content.ReadAsStringAsync();
            }
        }
        public async Task<string> GetAnswer(string question)
        {
            string uri = qnaServiceResourceName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer";
            string questionJSON = "{\"question\": \"" + question.Replace("\"","'") +  "\"}";

            var response = await Post(uri, questionJSON);

            var answers = JsonConvert.DeserializeObject<QnAAnswer>(response);
            if (answers.answers.Count > 0)
            {
                return answers.answers[0].answer;
            }
            else
            {
                return "No good match found.";
            }
        }
    }
    ```


7. Sınıfı `BasicLuisDialog` değiştirin. Her LUIS niyet **LuisIntent**ile dekore edilmiş bir yöntem olmalıdır. Dekorasyon için parametre gerçek LUIS niyet adıdır. Dekore edilmiş yöntem adı okunabilirlik ve bakım için LUIS niyet adı _olmalıdır,_ ancak tasarım veya çalışma zamanında aynı olması gerekmemektedir.

    ```csharp
    [Serializable]
    public class BasicLuisDialog : LuisDialog<object>
    {
        // LUIS Settings
        static string LUIS_appId = "<LUIS APP ID>";
        static string LUIS_apiKey = "<LUIS API KEY>";
        static string LUIS_hostRegion = "westus.api.cognitive.microsoft.com";

        // QnA Maker global settings
        // assumes all knowledge bases are created with same Azure service
        static string qnamaker_endpointKey = "<QnA Maker endpoint KEY>";
        static string qnamaker_resourceName = "my-qnamaker-s0-s";

        // QnA Maker Human Resources Knowledge base
        static string HR_kbID = "<QnA Maker KNOWLEDGE BASE ID>";

        // QnA Maker Finance Knowledge base
        static string Finance_kbID = "<QnA Maker KNOWLEDGE BASE ID>";

        // Instantiate the knowledge bases
        public QnAMakerService hrQnAService = new QnAMakerService("https://" + qnamaker_resourceName + ".azurewebsites.net", HR_kbID, qnamaker_endpointKey);
        public QnAMakerService financeQnAService = new QnAMakerService("https://" + qnamaker_resourceName + ".azurewebsites.net", Finance_kbID, qnamaker_endpointKey);

        public BasicLuisDialog() : base(new LuisService(new LuisModelAttribute(
            LUIS_appId,
            LUIS_apiKey,
            domain: LUIS_hostRegion)))
        {
        }

        [LuisIntent("None")]
        public async Task NoneIntent(IDialogContext context, LuisResult result)
        {
            HttpClient client = new HttpClient();
            await this.ShowLuisResult(context, result);
        }

        // HR Intent
        [LuisIntent("HR")]
        public async Task HumanResourcesIntent(IDialogContext context, LuisResult result)
        {
            // Ask the HR knowledge base
            var qnaMakerAnswer = await hrQnAService.GetAnswer(result.Query);
            await context.PostAsync($"{qnaMakerAnswer}");
            context.Wait(MessageReceived);
        }

        // Finance intent
        [LuisIntent("Finance")]
        public async Task FinanceIntent(IDialogContext context, LuisResult result)
        {
            // Ask the finance knowledge base
            var qnaMakerAnswer = await financeQnAService.GetAnswer(result.Query);
            await context.PostAsync($"{qnaMakerAnswer}");
            context.Wait(MessageReceived);
        }
        private async Task ShowLuisResult(IDialogContext context, LuisResult result)
        {
            await context.PostAsync($"You have reached {result.Intents[0].Intent}. You said: {result.Query}");
            context.Wait(MessageReceived);
        }
    }
    ```


## <a name="build-the-bot"></a>Botu oluşturun
1. Kod düzenleyicisinde **build.cmd'ye**sağ tıklayın ve **Konsoldan Çalıştır'ı**seçin.

    ![Kod düzenleyicisinde Konsoldan Çalıştır seçeneğinin ekran görüntüsü](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. Kod görünümü, yapının ilerlemesini ve sonuçlarını gösteren bir terminal penceresiyle değiştirilir.

    ![Konsol yapısının ekran görüntüsü](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Botu test edin
Azure portalında, botu test etmek için **Web Sohbetinde Sınama'yı** seçin. Yanıtı ilgili bilgi tabanından almak için farklı niyetlerden iletiler yazın.

![Web sohbet testi ekran görüntüsü](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi tabanınızı Power Virtual Agents'te bir ajanla bütünleştirin](integrate-with-power-virtual-assistant-fallback-topic.md)
