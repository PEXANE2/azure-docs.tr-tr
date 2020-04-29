---
title: LUSıS ve QnAMaker-bot tümleştirmesi
titleSuffix: Azure Cognitive Services
description: Soru-Cevap Oluşturma bilgi tabanınız daha büyük bir şekilde büyüdükçe, tek bir monoc kümesi olarak korumak zor olur. Bilgi bankasını daha küçük, mantıksal parçalara ayırın.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81402715"
---
# <a name="use-a-bot-with-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Bilgi tabanınızı dağıtmak için Soru-Cevap Oluşturma ve LUO ile bir bot kullanın
Soru-Cevap Oluşturma bilgi tabanınız daha büyük bir şekilde büyüdükçe, tek bir monoc kümesi olarak korumak zor olur. Bilgi bankasını daha küçük, mantıksal parçalara ayırın.

Soru-Cevap Oluşturma ' de birden fazla bilgi tabanı oluşturmak basit olsa da, gelen soruyu ilgili Bilgi Bankası 'na yönlendirmek için bir mantığın olması gerekir. Bunu LUSıS kullanarak yapabilirsiniz.

Bu makalede, bot Framework v3 SDK kullanılmaktadır. Bu bilgilerin bot Framework v4 SDK sürümü ile ilgileniyorsanız, bkz. [birden çok Lune ve QnA modellerini kullanma](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&tabs=csharp).

## <a name="architecture"></a>Mimari

![Language Understanding ile Soru-Cevap Oluşturma mimarisini gösteren grafik](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

Yukarıdaki grafik, Soru-Cevap Oluşturma ilk olarak bir LUSıS modelinden gelen sorunun amacını alır. Daha sonra, Soru-Cevap Oluşturma soruyu doğru Soru-Cevap Oluşturma bilgi tabanına yönlendirmek için bu amacı kullanır.

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

1. [Lui](https://www.luis.ai/) portalında oturum açın.
1. [Uygulama oluşturun](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app).
1. Her Soru-Cevap Oluşturma Bilgi Bankası için [bir amaç ekleyin](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) . Örnek, Soru-Cevap Oluşturma bilgi bankasındaki sorulara karşılık gelmelidir.
1. [Lusıs uygulamasını eğitme](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) ve [lusıs uygulamasını yayımlama](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp).
1. **Yönet** bölümünde, lusıs uygulama KIMLIĞINIZI, lusıs bitiş noktası anahtarınızı ve [özel etki alanı adını](../../cognitive-services-custom-subdomains.md)unutmayın. Bu değerlere daha sonra ihtiyacınız olacak.

## <a name="create-qna-maker-knowledge-bases"></a>Soru-Cevap Oluşturma bilgi tabanları oluşturun

1. [Soru-cevap oluşturma](https://qnamaker.ai)oturum açın.
1. LUSıS uygulamasındaki her amaç için bir Bilgi Bankası [oluşturun](https://www.qnamaker.ai/Create) .
1. Bilgi temellerini test edin ve yayımlayın. Her birini yayımladığınızda KIMLIĞI, kaynak adını ( _. azurewebsites.net/qnamaker_öncesi özel alt etki alanı) ve yetkilendirme uç noktası anahtarını unutmayın. Bu değerlere daha sonra ihtiyacınız olacak.

    Bu makalede, bilgi temellerin hepsi aynı Azure Soru-Cevap Oluşturma aboneliğinde oluşturulan varsayılmaktadır.

    ![HTTP isteğinin Soru-Cevap Oluşturma ekran görüntüsü](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

## <a name="web-app-bot"></a>Web uygulaması botu

1. Azure bot hizmeti ile bir LUO uygulamasını otomatik olarak içeren [bir "temel" bot oluşturun](https://docs.microsoft.com/azure/bot-service/bot-service-quickstart?view=azure-bot-service-4.0). C# programlama dilini seçin.

1. Web uygulaması bot ' ı oluşturduktan sonra, Azure portal web uygulaması bot ' ı seçin.
1. Web uygulaması bot hizmeti gezinmede **uygulama ayarları**' nı seçin. Ardından, kullanılabilir ayarların **uygulama ayarları** bölümüne gidin.
1. **Luisappıd** 'yi, önceki bölümde oluşturulan Luo uygulamasının değerine değiştirin. Sonra **Kaydet**' i seçin.


## <a name="change-the-code-in-the-basicluisdialogcs-file"></a>BasicLuisDialog.cs dosyasındaki kodu değiştirme
1. Azure portal web uygulaması bot 'ın **bot Management** bölümünden **Oluştur**' u seçin.
2. **Çevrimiçi Kod Düzenleyiciyi Aç**' ı seçin. Çevrimiçi düzen ortamı ile yeni bir tarayıcı sekmesi açılır.
3. **Wwwroot** bölümünde, **iletişim kutuları** dizinini seçip **BasicLuisDialog.cs**açın.
4. **BasicLuisDialog.cs** dosyasının en üstüne bağımlılıklar ekleyin:

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

5. Soru-Cevap Oluşturma yanıtının serisini kaldırmak için aşağıdaki sınıfları ekleyin:

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


6. Soru-Cevap Oluşturma hizmetine bir HTTP isteği oluşturmak için aşağıdaki sınıfı ekleyin. **Yetkilendirme** üstbilgisinin değeri, sözcükten sonra bir boşluk olacak şekilde sözcüğünü `EndpointKey`içerdiğine dikkat edin. JSON sonucunun önceki sınıflarda serisi kaldırıldı ve ilk yanıt döndürülür.

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


7. `BasicLuisDialog` Sınıfı değiştirin. Her bir LUIN amacı, **Luisamaç**ile donatılmış bir yönteme sahip olmalıdır. Dekorasyonu parametresi gerçek LUO amaç adıdır. Donatılmış Yöntem adı, okunabilirlik ve bakım için LUıN amaç _adı olmalıdır,_ ancak tasarım veya çalışma zamanında aynı olmak zorunda değildir.

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


## <a name="build-the-bot"></a>Bot 'ı oluşturma
1. Kod Düzenleyicisi 'nde **Build. cmd**' ye sağ tıklayın ve **konsolundan Çalıştır**' ı seçin.

    ![Kod düzenleyicisinde konsoldan çalıştır seçeneğinin ekran görüntüsü](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. Kod görünümü, oluşturma işleminin ilerleme durumunu ve sonuçlarını gösteren bir Terminal penceresiyle değiştirilmiştir.

    ![Konsol derlemesinin ekran görüntüsü](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Bot 'ı test etme
Azure portal, bot 'ı test etmek için **Web sohbetinde test '** i seçin. Karşılık gelen bilgi bankasından yanıt almak için farklı amaçlardan iletiler yazın.

![Web sohbet testinin ekran görüntüsü](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power Virtual aracılarında bilgi Bankalarınızı bir aracıyla tümleştirin](integrate-with-power-virtual-assistant-fallback-topic.md)
