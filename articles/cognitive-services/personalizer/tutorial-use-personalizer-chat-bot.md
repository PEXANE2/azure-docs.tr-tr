---
title: Sohbet bot 'ta kişiselleştirici kullanma-kişiselleştirici
description: Eylemler (özelliklerle birlikte) ve bağlam özellikleriyle bir kullanıcıya doğru içerik sağlamak için bir kişiselleştirici döngüsüyle bir C# .NET Chat bot 'ı özelleştirin.
ms.topic: tutorial
ms.date: 07/17/2020
ms.author: diberry
ms.openlocfilehash: d269dbf95356d1d8d0fb7c2f27e0828d016d0ac6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101718"
---
# <a name="tutorial-use-personalizer-in-net-chat-bot"></a>Öğretici: .NET Chat bot 'ta kişiselleştirici kullanma

Bir kullanıcıya doğru içerik sağlamak için bir kişiselleştirici döngüsüyle C# .NET Chat bot kullanın. Bu sohbet bot, kullanıcıya belirli bir kahve veya çay önerisinde bulunur. Kullanıcı bu öneriyi kabul edebilir veya reddedebilir. Bu, sonraki öneriyi daha uygun hale getirmenize yardımcı olmak için kişiselleştirici bilgiler sağlar.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Azure kaynakları ayarlama
> * Bot 'ı yapılandırma ve çalıştırma
> * Bot öykünücüsünü kullanarak bot ile etkileşim kurma
> * Bot 'ın nasıl ve nasıl kişiselleştirici kullandığını anlayın


## <a name="how-does-the-chat-bot-work"></a>Sohbet bot nasıl çalışır?

Sohbet bot, genellikle bir kullanıcı ile birlikte geri ve ileri bir konuşmadan gerçekleştirilir. Bu özel sohbet bot, kullanıcıyı sunmak için en iyi eylemi (kahve veya Tea) seçmek üzere kişiselleştirici kullanır. Kişiselleştirici, bu seçimi yapmak için pekiştirmeye dayalı Learning kullanır.

Sohbet bot 'ın, bilgisayarı açan konuşmayı yönetmesi gerekir. Sohbet botu, bot mimarisini ve iletişimini yönetmek için [bot Framework 'ü](https://github.com/microsoft/botframework-sdk) kullanır ve doğal dilin Kullanıcı amacını anlamak için bilişsel hizmeti [Language Understanding](../LUIS/index.yml) (Luis) kullanır.

Sohbet bot, istekleri yanıtlamak için kullanılabilen belirli bir yolu olan bir Web sitesidir `http://localhost:3978/api/messages` . Yerel olarak bir bot geliştirirken, çalışan sohbet bot ile görsel olarak etkileşimde bulunmak için bot öykünücüsünü kullanabilirsiniz.

### <a name="user-interactions-with-the-bot"></a>Bot ile kullanıcı etkileşimleri

Bu, metin sorguları girmenize izin veren basit bir sohbet bot ' dır.

|Kullanıcı metin giriyor|Bot metinle yanıt veriyor|Komut bot 'un yanıt metnini belirlemesi için açıklaması|
|--|--|--|
|Metin girilmedi-bot konuşmayı başlatır.|`This is a simple chatbot example that illustrates how to use Personalizer. The bot learns what coffee or tea order is preferred by customers given some context information (such as weather, temperature, and day of the week) and information about the user.`<br>`To use the bot, just follow the prompts. To try out a new imaginary context, type “Reset” and a new one will be randomly generated.`<br>`Welcome to the coffee bot, please tell me if you want to see the menu or get a coffee or tea suggestion for today. Once I’ve given you a suggestion, you can reply with ‘like’ or ‘don’t like’. It’s Tuesday today and the weather is Snowy.`|Bot, görüşmeyi eğitici bir metinle başlatır ve içeriğin ne olduğunu, ne olduğunu bilmenizi sağlar: `Tuesday` , `Snowy` .|
|`Show menu`|`Here is our menu: Coffee: Cappuccino Espresso Latte Macchiato Mocha Tea: GreenTea Rooibos`|Lusıs kullanarak sorgu amacını belirleyin, sonra kahve ve çay öğelerinin menü seçimlerini görüntüleyin. Eylemlerin özellikleri şunlardır |
|`What do you suggest`|`How about Latte?`|LUSıS kullanarak sorgu amacını belirleyip, **Derecelendirme API 'sini**çağırın ve bir soru olarak en iyi seçimi görüntüleyin `How about {response.RewardActionId}?` . Ayrıca, çizim amaçları için JSON çağrısını ve yanıtını görüntüler.|
|`I like it`|`That’s great! I’ll keep learning your preferences over time.`<br>`Would you like to get a new suggestion or reset the simulated context to a new day?`|LUSıS kullanarak sorgu amacını belirleme, sonra yeniden **API** 'yi çağırma `1` , çizim amacıyla JSON çağrısını ve yanıtını görüntüler.|
|`I don't like it`|`Oh well, maybe I’ll guess better next time.`<br>`Would you like to get a new suggestion or reset the simulated context to a new day?`|LUSıS kullanarak sorgu amacını belirleme, sonra yeniden **API** 'yi çağırma `0` , çizim amacıyla JSON çağrısını ve yanıtını görüntüler.|
|`Reset`|Yönerge metni döndürür.|LUSıS kullanarak sorgu amacını belirleme, ardından yönerge metnini görüntüleme ve bağlamı sıfırlama.|


### <a name="personalizer-in-this-bot"></a>Bu bot 'ta kişiselleştirici

Bu sohbet botu, bir eylem _listesine (bazı_ içerik türleri) ve bağlam özelliklerine göre en üstteki eylemi (belirli bir kahve veya Tea) seçmek Için kişiselleştirici kullanır.

Bot, eylem listesini, bağlam özellikleriyle birlikte kişiselleştirici döngüsüne gönderir. Kişiselleştirici, bot 'unuzun görüntüleyeceği en iyi eylemi döndürür.

Bu öğreticide, **Eylemler** kahve ve Tea türleridir:

|Ünüzü|Çay|
|--|--|
|Cappuccino<br>Espresso<br>Latte<br>Mocha|GreenTea<br>Rooiboş|

**Derecelendirme API 'si:** Yardım eden eylemleriniz hakkında bilgi edinmek için, bot her bir derecelendirme API isteği ile aşağıdakileri gönderir:

* _Özelliklerle ilgili_ eylemler
* Bağlam özellikleri

Modelin bir **özelliği** , sohbet bot Kullanıcı tabanınızın üyeleri genelinde toplanabilecek eylem veya içerikle ilgili bilgiler. Bir özellik ayrı ayrı (Kullanıcı KIMLIĞI gibi) veya yüksek oranda özel (günün tam saati gibi) _değildir_ .

Özellikler, işlemleri modeldeki geçerli içeriğe hizalamak için kullanılır. Model, kişisel kararlar alma kararı veren eylemler, bağlam ve özellikleri hakkında Kişiselleştiriciye ait geçmiş bilgisinin bir gösterimidir.

Özellikler dahil olmak üzere model, Azure portal **model güncelleştirme sıklığı** ayarı temelinde bir zamanlamaya göre güncelleştirilir.

Özellikler, teknik mimarinizdeki herhangi bir şema veya modele uygulayabileceğiniz planlama ve tasarıma sahip olacak şekilde seçilmelidir. Özellik değerleri, iş mantığı veya üçüncü taraf sistemlerle ayarlanabilir.

> [!CAUTION]
> Bu uygulamadaki özellikler, tanıtım içindir ve kullanım örneği için Web uygulamanızda kullanılacak en iyi özellikler olabilir.

#### <a name="action-features"></a>Eylem özellikleri

Her eylem (içerik öğesi), kahve veya çay öğesini ayırt etmenize yardımcı olmak için özelliklere sahiptir.

Özellikler, Azure portal döngü yapılandırmasının bir parçası olarak yapılandırılmamıştır. Bunun yerine, her derecelendirme API çağrısıyla bir JSON nesnesi olarak gönderilirler. Bu, eylemlerin ve özelliklerinin zaman içinde büyümesini, değiştirilmesini ve daralmasına olanak tanır. bu sayede, Kişiselleştiriciye eğilimleri takip edebilirsiniz.

Kahve ve çay 'nın özellikleri şunlardır:

* Kenya ve Brezilya gibi kahve çekirdeklere kaynak konumu
* Kahve veya çay Organic mi?
* Açık ya da koyu kahve

Kahve, yukarıdaki listede üç özelliğe sahip olsa da, çay 'nın yalnızca bir tane vardır. Yalnızca, eylemi anlamlı hale getirmek için özellikleri Kişiselleştiriciye geçirin. Eyleme uygulanmazsa, özellik için boş bir değer geçirmeyin.

#### <a name="context-features"></a>Bağlam özellikleri

Bağlam özellikleri, görüntü cihazı, Kullanıcı, konum ve kullanım çalışmanıza uygun diğer özellikler gibi ortamın bağlamını anlamak için Kişiselleştiriciye yardımcı olur.

Bu sohbet bot bağlamı şunları içerir:

* Hava durumu türü (kar y, Rainy, Sunny)
* Haftanın günü

Bu sohbet bot 'ta Özellik seçimi rasgeleleştirilmiştir. Gerçek bir bot 'ta, bağlam özelliklerinizin gerçek verilerini kullanın.

### <a name="design-considerations-for-this-bot"></a>Bu bot için tasarım konuları

Bu konuşmayı öğrenmek için birkaç uyarı vardır:
* **Bot etkileşimi**: bir basit kullanım durumunda derecelendirme ve yeniden ödül gösteren konuşma çok basittir. Bu, bot Framework SDK 'sının veya öykünücüsünün tüm işlevlerini gösterir.
* **Kişiselleştirici**: kullanım benzetimi için özellikler rastgele seçilir. Bir üretim kişiselleştirici senaryosunda özellikleri rastgele kullanmayın.
* **Language Understanding (lusıs)**: lusıs modelinin birkaç örneği yalnızca bu örneğe yöneliktir. Üretim Ludo uygulamanızda daha az sayıda örnek kullanmayın.


## <a name="install-required-software"></a>Gerekli yazılımları yükler
- [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/). İndirilebilir örnekler deposu .NET Core CLI kullanmayı tercih ediyorsanız yönergeleri içerir.
- [Microsoft bot Framework öykünücüsü](https://aka.ms/botframeworkemulator) , bot geliştiricilerinin yerel olarak botları sınaymasına ve hata ayıklamasına izin veren bir masaüstü uygulamasıdır ve bir tünel üzerinden uzaktan çalışır.

## <a name="download-the-sample-code-of-the-chat-bot"></a>Sohbet bot 'ın örnek kodunu indirin

Sohbet botu, kişiselleştirici örnekleri deposunda bulunur. Depoyu kopyalayın veya [indirin](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/archive/master.zip) , ardından `/samples/ChatbotExample` Visual Studio 2019 ile dizinde örneği açın.

Depoyu kopyalamak için, bir bash kabuğu 'nda (Terminal) aşağıdaki git komutunu kullanın.

```bash
git clone https://github.com/Azure-Samples/cognitive-services-personalizer-samples.git
```

## <a name="create-and-configure-personalizer-and-luis-resources"></a>Kişiselleştirici ve LUSıS kaynakları oluşturma ve yapılandırma

### <a name="create-azure-resources"></a>Azure kaynakları oluşturma

Bu sohbet bot 'ı kullanmak için, kişiselleştirici ve Language Understanding (LUU) için Azure kaynakları oluşturmanız gerekir.

* [Lusıs kaynakları oluşturun](../luis/luis-how-to-azure-subscription.md#create-luis-resources-in-azure-portal). Hem yazma hem de tahmin kaynaklarına ihtiyacınız olduğundan, oluşturma adımında **her ikisini de** seçin.
* Bir [kişiselleştirici kaynağı oluşturun](how-to-create-resource.md) ve sonra anahtarı ve uç noktayı Azure Portal kopyalayın. Bu değerleri `appsettings.json` .net projesinin dosyasında ayarlamanız gerekir.

### <a name="create-luis-app"></a>LUSıS uygulaması oluşturma

LUO 'ya yeni giriş yaparsanız, [oturum açmanız](https://www.luis.ai) ve hesabınızı hemen geçirmeniz gerekir. Yeni kaynak oluşturmanız gerekmez, bunun yerine Bu öğreticinin önceki bölümünde oluşturduğunuz kaynakları seçin.

1. Yeni bir Luo uygulaması oluşturmak için, [Luu portalında](https://www.luis.ai), aboneliğinizi ve yazma kaynağınızı seçin.
1. Yine de aynı sayfada, **konuşma için + yeni uygulama**' yı seçin ve ardından **JSON olarak içeri aktarın**.
1. Açılır iletişim kutusunda **Dosya Seç** ' i seçin ve `/samples/ChatbotExample/CognitiveModels/coffeebot.json` dosyayı seçin. Adı girin `Personalizer Coffee bot` .
1. LUı portalının sağ üst köşesindeki **eğitme** düğmesini seçin.
1. Uygulamayı tahmin çalışma zamanı için **Üretim yuvasında** yayımlamak üzere **Yayımla** düğmesini seçin.
1. **Yönet**' i ve ardından **Ayarlar**' ı seçin. **Uygulama kimliği**değerini kopyalayın. Bu değeri `appsettings.json` .net projesinin dosyasında ayarlamanız gerekir.
1. Hala **Yönet** bölümünde **Azure kaynakları**' nı seçin. Bu, uygulamadaki ilişkili kaynakları görüntüler.
1. **Tahmin kaynağı Ekle**' yi seçin. Açılır iletişim kutusunda, aboneliğinizi ve Bu öğreticinin önceki bir bölümünde oluşturulan tahmin kaynağını seçin ve **bitti**' yi seçin.
1. **Birincil anahtar** ve **uç nokta URL 'si**değerlerini kopyalayın. Bu değerleri `appsettings.json` .net projesinin dosyasında ayarlamanız gerekir.

### <a name="configure-bot-with-appsettingsjson-file"></a>Dosya üzerinde appsettings.jsbot 'ı yapılandırma

1. Visual Studio 2019 ile sohbet bot çözüm dosyasını açın `ChatbotSamples.sln` .
1. `appsettings.json`Projenin kök dizininde açın.
1. Bu öğreticinin önceki bölümünde kopyalanmış olan beş ayarı tümüyle ayarlayın.

    ```json
    {
      "PersonalizerChatbot": {
        "LuisAppId": "",
        "LuisAPIKey": "",
        "LuisServiceEndpoint": "",
        "PersonalizerServiceEndpoint": "",
        "PersonalizerAPIKey": ""
      }
    }
    ```

### <a name="build-and-run-the-bot"></a>Bot oluşturma ve çalıştırma

Yapılandırmasını yaptıktan sonra `appsettings.json` , sohbet bot 'ı oluşturmaya ve çalıştırmaya hazırlanın. Bunu yaptığınızda, çalışan Web sitesi için bir tarayıcı açılır `http://localhost:3978` .

:::image type="content" source="media/tutorial-chat-bot/chat-bot-web-site.png" alt-text="Sohbet Bot Web sitesini görüntüleyen tarayıcının ekran görüntüsü.":::

Web sitesini çalışır durumda tutun çünkü öğretici, bot 'ın ne yaptığını açıklıyor, bu sayede bot ile etkileşim kurabilirsiniz.


## <a name="set-up-the-bot-emulator"></a>Bot öykünücüsünü ayarlama

1. Bot öykünücüsünü açın ve bot 'ı **Aç**' ı seçin.

    :::image type="content" source="media/tutorial-chat-bot/bot-emulator-startup.png" alt-text="Bot öykünücüsü başlangıç ekranının ekran görüntüsü.":::


1. Aşağıdaki **bot URL 'si** ile bot 'ı yapılandırın ve **Bağlan**'ı seçin:

    `http://localhost:3978/api/messages`

    :::image type="content" source="media/tutorial-chat-bot/bot-emulator-open-bot-settings.png" alt-text="Bot öykünücüsü açık bot ayarlarını ekran görüntüsü.":::

    Öykünücü, sohbet bot 'a bağlanır ve yerel geliştirme için yardımcı olan günlük ve hata ayıklama bilgileri ile birlikte yönerge metnini görüntüler.

    :::image type="content" source="media/tutorial-chat-bot/bot-emulator-bot-conversation-first-turn.png" alt-text="İlk konuşmayı açmak için bot öykünücüsünün ekran görüntüsü.":::

## <a name="use-the-bot-in-the-bot-emulator"></a>Bot öykünücüsünde bot 'ı kullanma

1. Yazarak menüyü görmeyi isteyin `I would like to see the menu` . Sohbet bot öğeleri görüntüler.
1. Bir öğeyi öykünücüyü girerek bir öğe önermesine olanak sağlamak için `Please suggest a drink for me.` , tam JSON 'u görebilmeniz için, sohbet penceresinde derecelendirme isteğini ve yanıtı görüntüler. Ve bot, şunun gibi bir öneri sağlar`How about Latte?`
1. Bu, kişiselleştirici tarafından kullanılan en üst dereceli seçimi kabul ettiğiniz anlamına gelir `I like it.` . öykünücü, sohbet penceresinde yeniden ödül isteği 1 ' i ve yanıt olarak görüntüler, böylece tam JSON 'yi görebilirsiniz. `That’s great! I’ll keep learning your preferences over time.`Ve bot ile yanıt verir`Would you like to get a new suggestion or reset the simulated context to a new day?`

    `no`' İ seçimle karşılaşırsanız, 0 ' ın ödül puanı kişiselleştiriciye gönderilir.


## <a name="understand-the-net-code-using-personalizer"></a>Kişiselleştirici kullanarak .NET kodunu anlayın

.NET çözümü basit bir bot Framework sohbet bot. Kişiselleştirici ile ilgili kod şu klasörleridir:
* `/samples/ChatbotExample/Bots`
    * `PersonalizerChatbot.cs`bot ve kişiselleştirici arasındaki etkileşimin dosyası
* `/samples/ChatbotExample/ReinforcementLearning`-kişiselleştirici modeli için eylemleri ve özellikleri yönetir
* `/samples/ChatbotExample/Model`-kişiselleştirici eylemleri ve özellikleri için ve Lua amaçları için dosyalar

### <a name="personalizerchatbotcs---working-with-personalizer"></a>PersonalizerChatbot.cs-kişiselleştirici ile çalışma

`PersonalizerChatbot`Sınıfı öğesinden türetilir `Microsoft.Bot.Builder.ActivityHandler` . Konuşma akışını yönetmek için üç özellik ve yöntem içerir.

> [!CAUTION]
> Bu öğreticiden kod kopyalamayın. [Kişiselleştirici örnekleri deposundaki](https://github.com/Azure-Samples/cognitive-services-personalizer-samples)örnek kodu kullanın.

```csharp
public class PersonalizerChatbot : ActivityHandler
{

    private readonly LuisRecognizer _luisRecognizer;
    private readonly PersonalizerClient _personalizerClient;

    private readonly RLContextManager _rlFeaturesManager;

    public PersonalizerChatbot(LuisRecognizer luisRecognizer, RLContextManager rlContextManager, PersonalizerClient personalizerClient)
            {
                _luisRecognizer = luisRecognizer;
                _rlFeaturesManager = rlContextManager;
                _personalizerClient = personalizerClient;
            }
    }

    public override async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken = default(CancellationToken))
    {
        await base.OnTurnAsync(turnContext, cancellationToken);

        if (turnContext.Activity.Type == ActivityTypes.Message)
        {
            // Check LUIS model
            var recognizerResult = await _luisRecognizer.RecognizeAsync(turnContext, cancellationToken);
            var topIntent = recognizerResult?.GetTopScoringIntent();
            if (topIntent != null && topIntent.HasValue && topIntent.Value.intent != "None")
            {
                Intents intent = (Intents)Enum.Parse(typeof(Intents), topIntent.Value.intent);
                switch (intent)
                {
                    case Intents.ShowMenu:
                        await turnContext.SendActivityAsync($"Here is our menu: \n Coffee: {CoffeesMethods.DisplayCoffees()}\n Tea: {TeaMethods.DisplayTeas()}", cancellationToken: cancellationToken);
                        break;
                    case Intents.ChooseRank:
                        // Here we generate the event ID for this Rank.
                        var response = await ChooseRankAsync(turnContext, _rlFeaturesManager.GenerateEventId(), cancellationToken);
                        _rlFeaturesManager.CurrentPreference = response.Ranking;
                        await turnContext.SendActivityAsync($"How about {response.RewardActionId}?", cancellationToken: cancellationToken);
                        break;
                    case Intents.RewardLike:
                        if (!string.IsNullOrEmpty(_rlFeaturesManager.CurrentEventId))
                        {
                            await RewardAsync(turnContext, _rlFeaturesManager.CurrentEventId, 1, cancellationToken);
                            await turnContext.SendActivityAsync($"That's great! I'll keep learning your preferences over time.", cancellationToken: cancellationToken);
                            await SendByebyeMessageAsync(turnContext, cancellationToken);
                        }
                        else
                        {
                            await turnContext.SendActivityAsync($"Not sure what you like. Did you ask for a suggestion?", cancellationToken: cancellationToken);
                        }

                        break;
                    case Intents.RewardDislike:
                        if (!string.IsNullOrEmpty(_rlFeaturesManager.CurrentEventId))
                        {
                            await RewardAsync(turnContext, _rlFeaturesManager.CurrentEventId, 0, cancellationToken);
                            await turnContext.SendActivityAsync($"Oh well, maybe I'll guess better next time.", cancellationToken: cancellationToken);
                            await SendByebyeMessageAsync(turnContext, cancellationToken);
                        }
                        else
                        {
                            await turnContext.SendActivityAsync($"Not sure what you dislike. Did you ask for a suggestion?", cancellationToken: cancellationToken);
                        }

                        break;
                    case Intents.Reset:
                        _rlFeaturesManager.GenerateRLFeatures();
                        await SendResetMessageAsync(turnContext, cancellationToken);
                        break;
                    default:
                        break;
                }
            }
            else
            {
                var msg = @"Could not match your message with any of the following LUIS intents:
                        'ShowMenu'
                        'ChooseRank'
                        'RewardLike'
                        'RewardDislike'.
                        Try typing 'Show me the menu','What do you suggest','I like it','I don't like it'.";
                await turnContext.SendActivityAsync(msg);
            }
        }
        else if (turnContext.Activity.Type == ActivityTypes.ConversationUpdate)
        {
            // Generate a new weekday and weather condition
            // These will act as the context features when we call rank with Personalizer
            _rlFeaturesManager.GenerateRLFeatures();

            // Send a welcome message to the user and tell them what actions they may perform to use this bot
            await SendWelcomeMessageAsync(turnContext, cancellationToken);
        }
        else
        {
            await turnContext.SendActivityAsync($"{turnContext.Activity.Type} event detected", cancellationToken: cancellationToken);
        }
    }

    // code removed for brevity, full sample code available for download
    private async Task SendWelcomeMessageAsync(ITurnContext turnContext, CancellationToken cancellationToken)
    private async Task SendResetMessageAsync(ITurnContext turnContext, CancellationToken cancellationToken)
    private async Task SendByebyeMessageAsync(ITurnContext turnContext, CancellationToken cancellationToken)
    private async Task<RankResponse> ChooseRankAsync(ITurnContext turnContext, string eventId, CancellationToken cancellationToken)
    private async Task RewardAsync(ITurnContext turnContext, string eventId, double reward, CancellationToken cancellationToken)
}
```

`Send`Bot ve lusıs ile Yönet görüşmesini ön eki olan Yöntemler. Yöntemler `ChooseRankAsync` ve `RewardAsync` kişiselleştirici ile etkileşime geçin.

#### <a name="calling-rank-api-and-display-results"></a>Derecelendirme API 'sini çağırma ve sonuçları görüntüleme

Yöntemi, `ChooseRankAsync` eylemleri Özellikler ve bağlam özellikleriyle toplayarak, kişiselleştirici 'In DERECELENDIRME API 'sine göndermek IÇIN JSON verilerini oluşturur.

```csharp
private async Task<RankResponse> ChooseRankAsync(ITurnContext turnContext, string eventId, CancellationToken cancellationToken)
{
    IList<object> contextFeature = new List<object>
    {
        new { weather = _rlFeaturesManager.RLFeatures.Weather.ToString() },
        new { dayofweek = _rlFeaturesManager.RLFeatures.DayOfWeek.ToString() },
    };

    Random rand = new Random(DateTime.UtcNow.Millisecond);
    IList<RankableAction> actions = new List<RankableAction>();
    var coffees = Enum.GetValues(typeof(Coffees));
    var beansOrigin = Enum.GetValues(typeof(CoffeeBeansOrigin));
    var organic = Enum.GetValues(typeof(Organic));
    var roast = Enum.GetValues(typeof(CoffeeRoast));
    var teas = Enum.GetValues(typeof(Teas));

    foreach (var coffee in coffees)
    {
        actions.Add(new RankableAction
        {
            Id = coffee.ToString(),
            Features =
            new List<object>()
            {
                new { BeansOrigin = beansOrigin.GetValue(rand.Next(0, beansOrigin.Length)).ToString() },
                new { Organic = organic.GetValue(rand.Next(0, organic.Length)).ToString() },
                new { Roast = roast.GetValue(rand.Next(0, roast.Length)).ToString() },
            },
        });
    }

    foreach (var tea in teas)
    {
        actions.Add(new RankableAction
        {
            Id = tea.ToString(),
            Features =
            new List<object>()
            {
                new { Organic = organic.GetValue(rand.Next(0, organic.Length)).ToString() },
            },
        });
    }

    // Sending a rank request to Personalizer
    // Here we are asking Personalizer to decide which drink the user is most likely to want
    // based on the current context features (weather, day of the week generated in RLContextManager)
    // and the features of the drinks themselves
    var request = new RankRequest(actions, contextFeature, null, eventId);
    await turnContext.SendActivityAsync(
        "===== DEBUG MESSAGE CALL TO RANK =====\n" +
        "This is what is getting sent to Rank:\n" +
        $"{JsonConvert.SerializeObject(request, Formatting.Indented)}\n",
        cancellationToken: cancellationToken);
    var response = await _personalizerClient.RankAsync(request, cancellationToken);
    await turnContext.SendActivityAsync(
        $"===== DEBUG MESSAGE RETURN FROM RANK =====\n" +
        "This is what Rank returned:\n" +
        $"{JsonConvert.SerializeObject(response, Formatting.Indented)}\n",
        cancellationToken: cancellationToken);
    return response;
}
```

#### <a name="calling-reward-api-and-display-results"></a>Yeniden API çağırma ve sonuçları görüntüleme

Yöntemi, `RewardAsync` puanı belirleyerek, kişiselleştirici 'Nin ödül API 'sine göndermek IÇIN JSON verilerini oluşturur. Puan, Kullanıcı metninde tanımlanan ve yönteminden gönderilen LUSıS hedefini belirler `OnTurnAsync` .

```csharp
private async Task RewardAsync(ITurnContext turnContext, string eventId, double reward, CancellationToken cancellationToken)
{
    await turnContext.SendActivityAsync(
        "===== DEBUG MESSAGE CALL REWARD =====\n" +
        "Calling Reward:\n" +
        $"eventId = {eventId}, reward = {reward}\n",
        cancellationToken: cancellationToken);

    // Sending a reward request to Personalizer
    // Here we are responding to the drink ranking Personalizer provided us
    // If the user liked the highest ranked drink, we give a high reward (1)
    // If they did not, we give a low reward (0)
    await _personalizerClient.RewardAsync(eventId, new RewardRequest(reward), cancellationToken);
}
```

## <a name="design-considerations-for-a-bot"></a>Bir bot için tasarım konuları

Bu örnek, bir bot 'ta Kişiselleştiriciye ilişkin basit bir uçtan uca çözümü göstermek için tasarlanmıştır. Kullanım durumu daha karmaşık olabilir.

Bir üretim bot 'ta kişiselleştirici kullanmak istiyorsanız, aşağıdakileri planlayın:
* _Her_ bir derecelendirilmiş seçime Ihtiyaç duyduğunuzda kişiselleştiriciye gerçek zamanlı erişim. Rank API toplu olarak veya önbelleğe alınamaz.  Yeniden çağırma çağrısı geciktirilebilir veya ayrı bir işleme ayrılabilir ve zaman aşımına uğramaz, olay için varsayılan bir ödül değeri ayarlanır.
* Kullanım için büyük/küçük harf temelli hesaplama: Bu örnek, sıfır ve diğeri arasında aralığa sahip olmayan ve bir puan için negatif değer olmayan iki tane gösterdi. Sisteminizin daha ayrıntılı Puanlama yapması gerekiyor.
* Bot kanalları: Bu örnek tek bir kanal kullanır, ancak birden fazla kanal veya tek bir kanalda robotların çeşitlemelerini kullanarak, kişiselleştirici modelinin bağlam özelliklerinin bir parçası olarak değerlendirilmesi gerekebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticiyi tamamladığınızda, aşağıdaki kaynakları temizleyin:

* Örnek proje dizininizi silin.
* Azure portal, kişiselleştirici ve LUKAYNAK kaynağınızı silin.

## <a name="next-steps"></a>Sonraki adımlar
* [Kişiselleştirme nasıl çalışır?](how-personalizer-works.md)
* [Özellikler](concepts-features.md): eylemler ve bağlamla kullanılan özellikler hakkında kavramları öğrenin
* [Re,](concept-rewards.md)ödüller: yeniden temelsiz hesaplamayı öğrenin
