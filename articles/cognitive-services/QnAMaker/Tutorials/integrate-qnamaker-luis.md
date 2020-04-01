---
title: LUIS ve QnAMaker - Bot Entegrasyonu
titleSuffix: Azure Cognitive Services
description: QnA Maker bilgi tabanınız büyüdükçe, bunu tek bir yekpare küme olarak korumak zorlaşır ve bilgi tabanını daha küçük mantıksal parçalara bölmeye ihtiyaç vardır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 09/26/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: c01f5f41e61cd65855789bb753a7a297fe475885
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396338"
---
# <a name="use-bot-with-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Bilgi tabanınızı dağıtmak için QnA Maker ve LUIS ile bot kullanın
QnA Maker bilgi tabanınız büyüdükçe, bunu tek bir yekpare küme olarak korumak zorlaşır ve bilgi tabanını daha küçük mantıksal parçalara bölmeye ihtiyaç vardır.

QnA Maker'da birden çok bilgi tabanı oluşturmak kolay olsa da, gelen soruyu uygun bilgi tabanına yönlendirmek için bir mantık gerekir. Bunu LUIS'i kullanarak yapabilirsiniz.

Bu makalede Bot Framework v3 SDK kullanır. Bu bilgilerin Bot Framework v4 SDK sürümü ile ilgileniyorsanız, bu [Bot Framework makaleye](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&tabs=csharp)bakın.

## <a name="architecture"></a>Mimari

![Dil Anlayışı mimarisi ile QnA Maker](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

Yukarıdaki senaryoda, QnA Maker ilk bir LUIS modeli gelen soru niyetalır ve daha sonra doğru QnA Maker bilgi tabanına yönlendirmek için kullanabilirsiniz.

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

1. [LUIS](https://www.luis.ai/) portalında oturum açın.
1. [Bir uygulama oluşturun.](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app)
1. Her QnA Maker bilgi tabanı için [bir niyet ekleyin.](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) Örnek söyleyinçler QnA Maker bilgi üslerindeki sorulara karşılık vermelidir.
1. [LUIS uygulamasını eğitin](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) ve [LUIS Uygulamanızı yayınlayın.](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp)
1. **Yönet** bölümünde, LUIS uygulama kimliğinizi, LUIS uç noktası anahtarınızı ve [özel alan adınızı](../../cognitive-services-custom-subdomains.md)not edin. Bu değerlere daha sonra ihtiyacınız olacaktır.

## <a name="create-qna-maker-knowledge-bases"></a>QnA Maker bilgi tabanları oluşturun

1. [QnA Maker'da](https://qnamaker.ai)oturum açın.
1. LUIS uygulamasındaher amaç için bir bilgi tabanı [oluşturun.](https://www.qnamaker.ai/Create)
1. Bilgi tabanlarını test edin ve yayınlayın. Her KB'yi yayımladığınızda, KB Kimliğini, kaynak adını _(azurewebsites.net/qnamaker_önce özel alt etki alanı) ve yetkilendirme bitiş noktası anahtarını not edin. Bu değerlere daha sonra ihtiyacınız olacaktır.

    Bu makalede, KB'lerin tümü aynı Azure QnA Maker aboneliğinde oluşturulmuştur varsayar.

    ![QnA Maker HTTP isteği](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

## <a name="web-app-bot"></a>Web uygulaması Bot

1. Otomatik olarak bir LUIS uygulaması içeren [bir "Temel" Web App botu oluşturun.](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0) C# programlama dilini seçin.

1. Web uygulaması botu oluşturulduktan sonra Azure portalında web uygulaması botu'nu seçin.
1. Web uygulaması bot hizmeti gezintisinde **Uygulama Ayarları'nı** seçin ve ardından kullanılabilir ayarların **Uygulama ayarları** bölümüne gidin.
1. **LuisAppId'i** önceki bölümde oluşturulan LUIS uygulamasının değeriyle değiştirin ve ardından **Kaydet'i**seçin.


## <a name="change-code-in-basicluisdialogcs"></a>BasicLuisDialog.cs'da kodu değiştirme
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


6. QnA Maker hizmetine http isteğinde bulunmak için aşağıdaki sınıfı ekleyin. **Yetkilendirme** üstbilgisinin değerinin sözcüğü `EndpointKey` izleyen bir boşlukla birlikte sözcüğü içerdiğine dikkat edin. JSON sonucu önceki sınıflara deserialized ve ilk yanıt döndürülür.

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


7. BasicLuisDialog sınıfını değiştirin. Her LUIS niyet **LuisIntent**ile dekore edilmiş bir yöntem olmalıdır. Dekorasyon için parametre gerçek LUIS niyet adıdır. Dekore edilmiş yöntem adı okunabilirlik ve bakım için LUIS niyet adı _olmalıdır,_ ancak tasarım veya çalışma zamanında aynı olması gerekmemektedir.

    ```csharp
    [Serializable]
    public class BasicLuisDialog : LuisDialog<object>
    {
        // LUIS Settings
        static string LUIS_appId = "<LUIS APP ID>";
        static string LUIS_apiKey = "<LUIS API KEY>";
        static string LUIS_hostRegion = "westus.api.cognitive.microsoft.com";

        // QnA Maker global settings
        // assumes all KBs are created with same Azure service
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
1. Kod düzenleyicisinde, sağ tıklayın `build.cmd` ve **Konsoldan Çalıştır'ı**seçin.

    ![konsoldan çalıştırın](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. Kod görünümü, yapının ilerlemesini ve sonuçlarını gösteren bir terminal penceresi ile değiştirilir.

    ![konsol yapısı](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Botu test edin
Azure portalında, botu test etmek için **Web Sohbetinde Sınama'yı** seçin. Yanıtı ilgili bilgi tabanından almak için farklı niyetlerden iletiler yazın.

![web sohbet testi](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi tabanınızı Power Virtual Agent ile tümleştirin](integrate-with-power-virtual-assistant-fallback-topic.md)
