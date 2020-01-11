---
title: 'Öğretici: Application Insights, C# -Lua'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, Application Insights telemetri veri depolama alanına bot ve Language Understanding bilgileri eklenmektedir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: diberry
ms.openlocfilehash: b9c47685253e2a70c7b5e947debaac6f5f3264b2
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75888303"
---
# <a name="tutorial-add-luis-results-to-application-insights-from-a-bot-in-c"></a>Öğretici: içindeki bir bot 'tan Application Insights LUSıS sonuçları eklemeC#

Bu öğreticide, [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetri veri depolama alanına bot ve Language Understanding bilgileri eklenmektedir. Bu verilere sahip olduktan sonra, kusto Language veya Power BI ile sorgulayabilirsiniz, toplayabilirsiniz ve amaçlarını ve gerçek zamanlı olarak yapılan varlıkları analiz etmek, toplamak ve raporlamak için kullanabilirsiniz. Bu analiz, eklediğinizde veya amaç ve varlıkları LUIS uygulamanızı düzenlemek, belirlemenize yardımcı olur.

[!INCLUDE [Waiting for Bot refresh](./includes/wait-bot-upgrade.md)]

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Application Insights içinde verileri anlamak için bot ve dil yakalayın
> * Language Understanding verileri için sorgu Application Insights

## <a name="prerequisites"></a>Ön koşullar

* Application Insights etkinken oluşturulmuş bir Azure bot hizmet bot.
* Önceki bot **[öğreticiden](luis-csharp-tutorial-bf-v4.md)** bot kodu indirildi. 
* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Bu öğreticideki tüm kod, [Azure-Samples Language Understanding GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-csharp-bot-johnsmith-src-telemetry)bulunur. 

## <a name="add-application-insights-to-web-app-bot-project"></a>Web uygulaması bot projesine Application Insights ekleyin

Şu anda bu web app botu kullanılan Application Insights hizmeti için robot genel durumu telemetri toplar. LUSıS bilgileri toplanmaz. 

LUSıS bilgilerini yakalamak için, Web uygulaması bot 'ın **[Microsoft. ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** NuGet paketinin yüklü ve yapılandırılmış olması gerekir.  

1. Visual Studio 'dan çözüme bağımlılığı ekleyin. **Çözüm Gezgini**, proje adına sağ tıklayın ve **NuGet Paketlerini Yönet...** seçeneğini belirleyin. NuGet Paket Yöneticisi yüklü paketlerin bir listesini gösterir. 
1. **Araştır** ' ı seçin ve ardından **Microsoft. ApplicationInsights**'ı arayın.
1. Paketi yükler. 

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Yakalama ve LUIS sorgu sonuçları Application Insights'a gönderme

1. `LuisHelper.cs` dosyasını açın ve içeriğini aşağıdaki kodla değiştirin. **Logtoapplicationınsights** yöntemi, bot ve lusıs verilerini yakalar ve bunu `LUIS`adlı bir izleme olayı olarak Application Insights gönderir.

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.AI.Luis;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    using Microsoft.ApplicationInsights;
    using System.Collections.Generic;
    
    namespace Microsoft.BotBuilderSamples
    {
        public static class LuisHelper
        {
            public static async Task<BookingDetails> ExecuteLuisQuery(IConfiguration configuration, ILogger logger, ITurnContext turnContext, CancellationToken cancellationToken)
            {
                var bookingDetails = new BookingDetails();
    
                try
                {
                    // Create the LUIS settings from configuration.
                    var luisApplication = new LuisApplication(
                        configuration["LuisAppId"],
                        configuration["LuisAPIKey"],
                        "https://" + configuration["LuisAPIHostName"]
                    );
    
                    var recognizer = new LuisRecognizer(luisApplication);
    
                    // The actual call to LUIS
                    var recognizerResult = await recognizer.RecognizeAsync(turnContext, cancellationToken);
    
                    LuisHelper.LogToApplicationInsights(configuration, turnContext, recognizerResult);
    
                    var (intent, score) = recognizerResult.GetTopScoringIntent();
                    if (intent == "Book_flight")
                    {
                        // We need to get the result from the LUIS JSON which at every level returns an array.
                        bookingDetails.Destination = recognizerResult.Entities["To"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
                        bookingDetails.Origin = recognizerResult.Entities["From"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
    
                        // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                        // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                        bookingDetails.TravelDate = recognizerResult.Entities["datetime"]?.FirstOrDefault()?["timex"]?.FirstOrDefault()?.ToString().Split('T')[0];
                    }
                }
                catch (Exception e)
                {
                    logger.LogWarning($"LUIS Exception: {e.Message} Check your LUIS configuration.");
                }
    
                return bookingDetails;
            }
            public static void LogToApplicationInsights(IConfiguration configuration, ITurnContext turnContext, RecognizerResult result)
            {
                // Create Application Insights object
                TelemetryClient telemetry = new TelemetryClient();
    
                // Set Application Insights Instrumentation Key from App Settings
                telemetry.InstrumentationKey = configuration["BotDevAppInsightsKey"];
    
                // Collect information to send to Application Insights
                Dictionary<string, string> logProperties = new Dictionary<string, string>();
    
                logProperties.Add("BotConversation", turnContext.Activity.Conversation.Name);
                logProperties.Add("Bot_userId", turnContext.Activity.Conversation.Id);
    
                logProperties.Add("LUIS_query", result.Text);
                logProperties.Add("LUIS_topScoringIntent_Name", result.GetTopScoringIntent().intent);
                logProperties.Add("LUIS_topScoringIntentScore", result.GetTopScoringIntent().score.ToString());
    
    
                // Add entities to collected information
                int i = 1;
                if (result.Entities.Count > 0)
                {
                    foreach (var item in result.Entities)
                    {
                        logProperties.Add("LUIS_entities_" + i++ + "_" + item.Key, item.Value.ToString());
                    }
                }
    
                // Send to Application Insights
                telemetry.TrackTrace("LUIS", ApplicationInsights.DataContracts.SeverityLevel.Information, logProperties);
    
            }
        }
    }
    ```

## <a name="add-application-insights-instrumentation-key"></a>Application Insights izleme anahtarı Ekle 

Application Insights 'a veri eklemek için, izleme anahtarına ihtiyacınız vardır.

1. Bir tarayıcıda [Azure Portal](https://portal.azure.com), bot 'ın **Application Insights** kaynağını bulun. Adında, bot adının büyük bir kısmında, adın sonunda `luis-csharp-bot-johnsmithxqowom`gibi rastgele karakterler olacaktır. 
1. Application Insights kaynağında **genel bakış** sayfasında, **izleme anahtarını**kopyalayın.
1. Visual Studio 'da, bot projesinin kökündeki **appSettings. JSON** dosyasını açın. Bu dosya, tüm ortam değişkenlerinizi içerir.
1. İzleme anahtarınızın değeri ile `BotDevAppInsightsKey` yeni bir değişken ekleyin. İçindeki değer tırnak içinde olmalıdır. 

## <a name="build-and-start-the-bot"></a>Bot oluşturma ve başlatma

1. Visual Studio 'da, bot 'ı derleyin ve çalıştırın. 
1. Bot öykünücüsünü başlatın ve bot 'ı açın. Bu [adım](luis-csharp-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) , önceki öğreticide sunulmaktadır.

1. Bot 'a soru sorun. Bu [adım](luis-csharp-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) , önceki öğreticide sunulmaktadır.

## <a name="view-luis-entries-in-application-insights"></a>Application ınsights'ta görünümü LUIS girişleri

LUIS girişlerini görmek için Application ınsights'ı açın. Verilerin Application Insights görünmesi birkaç dakika sürebilir.

1. [Azure Portal](https://portal.azure.com), bot 'ın Application Insights kaynağını açın. 
1. Kaynak açıldığında **Ara** ' yı seçin ve son **30 dakika** içinde, **izleme**olay türü ile tüm verileri ara ' yı seçin. **Lusıs**adlı izlemeyi seçin. 
1. Bot ve LUSıS bilgileri **özel özellikler**altında bulunabilir. 

    ![Application Insights ' de depolanan LUSıS özel özelliklerini gözden geçirin](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-csharp.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Amacını ve score utterance için Application Insights sorgu
Application Insights, verileri [kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use) diliyle sorgulama yanı sıra [Power BI](https://powerbi.microsoft.com)dışa aktarma gücü sağlar. 

1. **Günlük (Analiz)** öğesini seçin. Bir sorgu penceresi üstündeki ve bir veri tablosu penceresi altındaki yeni bir pencere açılır. Veritabanları önce kullandıysanız, bu düzenleme tanıdık gelir. Sorgu, önceki Filtrelenen verileri temsil eder. **Customdimensions** sütununda bot ve lusıs bilgileri bulunur.
1. En üst amacı, puanı ve utterance 'i almak için, sorgu penceresinde son satırın (`|top...` satırı) hemen üstüne aşağıdakini ekleyin:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent_Name)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

1. Sorguyu çalıştırın. Yeni sütunlar topIntent, Puanlama ve utterance kullanılabilir. Sıralamak için Topamaç sütununu seçin.

[Kusto sorgu dili](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) hakkında daha fazla bilgi edinin veya [verileri Power BI dışarı aktarın](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin

Daha fazla bilgi edinin [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Sonraki adımlar

Application ınsights veri eklemek isteyebileceğiniz diğer bilgileri içeren uygulama kimliği, sürüm kimliği, son model değişikliği tarihi, son tarih eğitmek, son yayımlama tarihi. Bu değerler, uç nokta URL 'sinden (uygulama KIMLIĞI ve sürüm KIMLIĞI) alınabilir veya bir yazma API çağrısından, daha sonra Web uygulaması bot ayarlarında ayarlanır ve buradan çekilir.  

Birden fazla LUIS uygulaması için aynı uç nokta aboneliği kullanıyorsanız, abonelik kimliği ve paylaşılan anahtar olduğunu belirten bir özellik içermelidir.

> [!div class="nextstepaction"]
> [Örnek konuşma hakkında daha fazla bilgi edinin](luis-how-to-add-example-utterances.md)
