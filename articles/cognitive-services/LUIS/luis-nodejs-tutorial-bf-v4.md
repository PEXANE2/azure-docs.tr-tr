---
title: 'Öğretici: Language Understanding bot Node.js v4'
description: Node.js kullanarak, bu öğreticide dil anlama (LUU) ile tümleştirilmiş bir sohbet bot oluşturun. Bu sohbet robotu, bir robot çözümünü kısa sürede gerçekleştirmek için İnsan Kaynakları uygulamasını kullanır. Robot, Bot Framework sürümü 4 ve Azure Web uygulaması robotu ile geliştirilmiştir.
ms.topic: tutorial
ms.date: 06/23/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 522502a043778d6f482c1756506063dfa6515ca3
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405033"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Öğretici: Node.js Language Understanding ile etkin bir Web uygulaması bot kullanın

Dil anlama (LUU) ile tümleştirilmiş bir sohbet bot oluşturmak için Node.js kullanın. Bot, Azure [Web App bot](https://docs.microsoft.com/azure/bot-service/) kaynağı ve [bot Framework sürüm](https://github.com/Microsoft/botbuilder-dotnet) v4 ile oluşturulmuştur.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Web uygulaması robotu oluşturma. Bu işlem sizin için yeni bir LUIS uygulaması oluşturur.
> * Web bot hizmeti tarafından oluşturulan bot projesini indirin
> * Robotu ve öykünücüyü bilgisayarınızda yerel olarak başlatma
> * Robotta konuşma sonuçlarını görüntüleme

## <a name="prerequisites"></a>Önkoşullar

* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

## <a name="create-a-web-app-bot-resource"></a>Web uygulaması bot kaynağı oluşturma

1. [Azure portalda](https://portal.azure.com)**Yeni kaynak oluştur**'u seçin.

1. Arama kutusunda **Web Uygulaması Robotu**'nu arayın ve bunu seçin. **Oluştur**’u seçin.

1. **Robot Hizmeti**'nde gerekli bilgileri sağlayın:

    |Ayar|Amaç|Önerilen ayar|
    |--|--|--|
    |Bot tutamacı|Kaynak adı|`luis-nodejs-bot-` + `<your-name>`, örneğin `luis-nodejs-bot-johnsmith`|
    |Abonelik|Robotun oluşturulacağı abonelik.|Birincil aboneliğiniz.
    |Kaynak grubu|Azure kaynaklarının mantıksal grubu|Bu robotla kullanılan tüm kaynakları depolamak için yeni bir grup oluşturun ve grubu `luis-nodejs-bot-resource-group` olarak adlandırın.|
    |Konum|Azure bölgesi - LUIS yazma veya yayımlama bölgesi ile aynı olması gerekmez.|`westus`|
    |Fiyatlandırma katmanı|Hizmet isteği sınırları ve faturalandırma için kullanılır.|`F0` ücretsiz katmanı gösterir.
    |Uygulama adı|Ad, robotunuz buluta dağıtıldığında alt etki alanı olarak kullanılır (örneğin humanresourcesbot.azurewebsites.net).|`luis-nodejs-bot-` + `<your-name>`, örneğin `luis-nodejs-bot-johnsmith`|
    |Robot şablonu|Bot Framework ayarları - sonraki tabloya bakın|
    |LUIS Uygulaması konumu|LUIS kaynak bölgesi ile aynı olmalıdır|`westus`|
    |Uygulama hizmeti planı/konumu|Belirtilen varsayılan değerden değiştirmeyin.|
    |Application Insights|Belirtilen varsayılan değerden değiştirmeyin.|
    |Microsoft uygulama KIMLIĞI ve parolası|Belirtilen varsayılan değerden değiştirmeyin.|

1. **Bot şablonunda**, aşağıdakileri seçin ve ardından bu ayarlar altındaki **Seç** düğmesini seçin:

    |Ayar|Amaç|Seçim|
    |--|--|--|
    |SDK dili|Robotun programlama dili|**Node.js**|
    |Hecesi|Robot türü|**Temel robot**|

1. **Oluştur**’u seçin. Robot hizmetini oluşturur ve Azure'a dağıtır. Bu işlemin bir parçası olarak `luis-nodejs-bot-XXXX` adlı bir LUIS uygulaması oluşturulur. Bu ad/Azure bot hizmeti uygulama adını temel alır.

    > [!div class="mx-imgBorder"]
    > [![Web uygulaması robotu oluşturma](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Devam etmeden önce bot hizmeti oluşturuluncaya kadar bekleyin.

1. `Go to resource`Web uygulaması bot sayfanıza gitmek için bildirimde ' u seçin.

## <a name="the-bot-has-a-language-understanding-model"></a>Bot 'ın bir Language Understanding modeli vardır

Bot hizmeti oluşturma işlemi, amaçlar ve örnek dıklarla yeni bir LUO uygulaması da oluşturur. Robot, yeni LUIS uygulamasına şu amaçlar için amaç eşlemesi sağlar:

|Temel robot LUIS amaçları|örnek konuşma|
|--|--|
|Kitap kolu|`Travel to Paris`|
|İptal|`bye`|
|Gethava durumu|`what's the weather like?`|
|Yok|Uygulamanın etki alanı dışındaki her şey.|

## <a name="test-the-bot-in-web-chat"></a>Web sohbetinde bot 'ı test etme

1. Yeni bot için Azure portal devam ederken, **Web sohbetinde test**' i seçin.
1. **Iletinizi yazın** metin kutusuna metni girin `Book a flight from Seattle to Berlin tomorrow` . Bot, bir uçuş sağlamak istediğiniz doğrulama ile yanıt verir.

    ![Azure portal ekran görüntüsü, ' Merhaba ' metnini girin.](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    Botunuzu hızlı bir şekilde test etmek için test işlevselliğini kullanabilirsiniz. Hata ayıklama da dahil olmak üzere daha kapsamlı bir test için, bot kodunu indirin ve Visual Studio Code kullanın.

## <a name="download-the-web-app-bot-source-code"></a>Web uygulaması bot kaynak kodunu indirin
Web uygulaması robot kodunu geliştirmek için, yerel bilgisayarınızda kodu indirin ve kullanın.

1. Azure portalda, **Robot yönetimi** bölümünden **Derle**'yi seçin.

1. **Robot kaynak kodunu indir**'i seçin.

    [![Temel bot için Web uygulaması bot kaynak kodunu indirin](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Açılan iletişim kutusu **indirilen ZIP dosyasında uygulama ayarlarını dahil**Istediğinde, **Evet**' i seçin. Bu, LUSıS ayarlarını sağlar.

1. Kaynak kodu .zip dosyasına sıkıştırılmışsa, bir iletide kodu indirme bağlantısı sağlanır. Bağlantıyı seçin.

1. Bu .zip dosyasını yerel bilgisayarınıza kaydedin ve dosyaları ayıklayın. Proje klasörünü Visual Studio Code açın.

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Lua 'ya ve yanıtı almaya yönelik kodu gözden geçirin

1. Kullanıcı iletişim kutusunu LUSıS tahmin uç noktasına göndermek için **iletişim > flightBookingRecognizer.js** dosyasını açın. Robota girilen kullanıcı konuşmasının LUIS'e gönderildiği yer budur. LU, **executeLuisQuery** yönteminden yanıt döndürülür.

    ```javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.

    const { LuisRecognizer } = require('botbuilder-ai');

    class FlightBookingRecognizer {
        constructor(config) {
            const luisIsConfigured = config && config.applicationId && config.endpointKey && config.endpoint;
            if (luisIsConfigured) {
                // Set the recognizer options depending on which endpoint version you want to use e.g v2 or v3.
                // More details can be found in https://docs.microsoft.com/en-gb/azure/cognitive-services/luis/luis-migration-api-v3
                const recognizerOptions = {
                    apiVersion: 'v3'
                };

                this.recognizer = new LuisRecognizer(config, recognizerOptions);
            }
        }

        get isConfigured() {
            return (this.recognizer !== undefined);
        }

        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {TurnContext} context
         */
        async executeLuisQuery(context) {
            return await this.recognizer.recognize(context);
        }

        getFromEntities(result) {
            let fromValue, fromAirportValue;
            if (result.entities.$instance.From) {
                fromValue = result.entities.$instance.From[0].text;
            }
            if (fromValue && result.entities.From[0].Airport) {
                fromAirportValue = result.entities.From[0].Airport[0][0];
            }

            return { from: fromValue, airport: fromAirportValue };
        }

        getToEntities(result) {
            let toValue, toAirportValue;
            if (result.entities.$instance.To) {
                toValue = result.entities.$instance.To[0].text;
            }
            if (toValue && result.entities.To[0].Airport) {
                toAirportValue = result.entities.To[0].Airport[0][0];
            }

            return { to: toValue, airport: toAirportValue };
        }

        /**
         * This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
         * TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
         */
        getTravelDate(result) {
            const datetimeEntity = result.entities.datetime;
            if (!datetimeEntity || !datetimeEntity[0]) return undefined;

            const timex = datetimeEntity[0].timex;
            if (!timex || !timex[0]) return undefined;

            const datetime = timex[0].split('T')[0];
            return datetime;
        }
    }

    module.exports.FlightBookingRecognizer = FlightBookingRecognizer;
    ```

1. **İletişim kutuları-> MainDialog** , söylenişi 'i yakalar ve actstep yönteminde executeLuisQuery 'e gönderir.

    ```javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.

    const { TimexProperty } = require('@microsoft/recognizers-text-data-types-timex-expression');
    const { MessageFactory, InputHints } = require('botbuilder');
    const { LuisRecognizer } = require('botbuilder-ai');
    const { ComponentDialog, DialogSet, DialogTurnStatus, TextPrompt, WaterfallDialog } = require('botbuilder-dialogs');

    const MAIN_WATERFALL_DIALOG = 'mainWaterfallDialog';

    class MainDialog extends ComponentDialog {
        constructor(luisRecognizer, bookingDialog) {
            super('MainDialog');

            if (!luisRecognizer) throw new Error('[MainDialog]: Missing parameter \'luisRecognizer\' is required');
            this.luisRecognizer = luisRecognizer;

            if (!bookingDialog) throw new Error('[MainDialog]: Missing parameter \'bookingDialog\' is required');

            // Define the main dialog and its related components.
            // This is a sample "book a flight" dialog.
            this.addDialog(new TextPrompt('TextPrompt'))
                .addDialog(bookingDialog)
                .addDialog(new WaterfallDialog(MAIN_WATERFALL_DIALOG, [
                    this.introStep.bind(this),
                    this.actStep.bind(this),
                    this.finalStep.bind(this)
                ]));

            this.initialDialogId = MAIN_WATERFALL_DIALOG;
        }

        /**
         * The run method handles the incoming activity (in the form of a TurnContext) and passes it through the dialog system.
         * If no dialog is active, it will start the default dialog.
         * @param {*} turnContext
         * @param {*} accessor
         */
        async run(turnContext, accessor) {
            const dialogSet = new DialogSet(accessor);
            dialogSet.add(this);

            const dialogContext = await dialogSet.createContext(turnContext);
            const results = await dialogContext.continueDialog();
            if (results.status === DialogTurnStatus.empty) {
                await dialogContext.beginDialog(this.id);
            }
        }

        /**
         * First step in the waterfall dialog. Prompts the user for a command.
         * Currently, this expects a booking request, like "book me a flight from Paris to Berlin on march 22"
         * Note that the sample LUIS model will only recognize Paris, Berlin, New York and London as airport cities.
         */
        async introStep(stepContext) {
            if (!this.luisRecognizer.isConfigured) {
                const messageText = 'NOTE: LUIS is not configured. To enable all capabilities, add `LuisAppId`, `LuisAPIKey` and `LuisAPIHostName` to the .env file.';
                await stepContext.context.sendActivity(messageText, null, InputHints.IgnoringInput);
                return await stepContext.next();
            }

            const messageText = stepContext.options.restartMsg ? stepContext.options.restartMsg : 'What can I help you with today?\nSay something like "Book a flight from Paris to Berlin on March 22, 2020"';
            const promptMessage = MessageFactory.text(messageText, messageText, InputHints.ExpectingInput);
            return await stepContext.prompt('TextPrompt', { prompt: promptMessage });
        }

        /**
         * Second step in the waterfall.  This will use LUIS to attempt to extract the origin, destination and travel dates.
         * Then, it hands off to the bookingDialog child dialog to collect any remaining details.
         */
        async actStep(stepContext) {
            const bookingDetails = {};

            if (!this.luisRecognizer.isConfigured) {
                // LUIS is not configured, we just run the BookingDialog path.
                return await stepContext.beginDialog('bookingDialog', bookingDetails);
            }

            // Call LUIS and gather any potential booking details. (Note the TurnContext has the response to the prompt)
            const luisResult = await this.luisRecognizer.executeLuisQuery(stepContext.context);
            switch (LuisRecognizer.topIntent(luisResult)) {
            case 'BookFlight': {
                // Extract the values for the composite entities from the LUIS result.
                const fromEntities = this.luisRecognizer.getFromEntities(luisResult);
                const toEntities = this.luisRecognizer.getToEntities(luisResult);

                // Show a warning for Origin and Destination if we can't resolve them.
                await this.showWarningForUnsupportedCities(stepContext.context, fromEntities, toEntities);

                // Initialize BookingDetails with any entities we may have found in the response.
                bookingDetails.destination = toEntities.airport;
                bookingDetails.origin = fromEntities.airport;
                bookingDetails.travelDate = this.luisRecognizer.getTravelDate(luisResult);
                console.log('LUIS extracted these booking details:', JSON.stringify(bookingDetails));

                // Run the BookingDialog passing in whatever details we have from the LUIS call, it will fill out the remainder.
                return await stepContext.beginDialog('bookingDialog', bookingDetails);
            }

            case 'GetWeather': {
                // We haven't implemented the GetWeatherDialog so we just display a TODO message.
                const getWeatherMessageText = 'TODO: get weather flow here';
                await stepContext.context.sendActivity(getWeatherMessageText, getWeatherMessageText, InputHints.IgnoringInput);
                break;
            }

            default: {
                // Catch all for unhandled intents
                const didntUnderstandMessageText = `Sorry, I didn't get that. Please try asking in a different way (intent was ${ LuisRecognizer.topIntent(luisResult) })`;
                await stepContext.context.sendActivity(didntUnderstandMessageText, didntUnderstandMessageText, InputHints.IgnoringInput);
            }
            }

            return await stepContext.next();
        }

        /**
         * Shows a warning if the requested From or To cities are recognized as entities but they are not in the Airport entity list.
         * In some cases LUIS will recognize the From and To composite entities as a valid cities but the From and To Airport values
         * will be empty if those entity values can't be mapped to a canonical item in the Airport.
         */
        async showWarningForUnsupportedCities(context, fromEntities, toEntities) {
            const unsupportedCities = [];
            if (fromEntities.from && !fromEntities.airport) {
                unsupportedCities.push(fromEntities.from);
            }

            if (toEntities.to && !toEntities.airport) {
                unsupportedCities.push(toEntities.to);
            }

            if (unsupportedCities.length) {
                const messageText = `Sorry but the following airports are not supported: ${ unsupportedCities.join(', ') }`;
                await context.sendActivity(messageText, messageText, InputHints.IgnoringInput);
            }
        }

        /**
         * This is the final step in the main waterfall dialog.
         * It wraps up the sample "book a flight" interaction with a simple confirmation.
         */
        async finalStep(stepContext) {
            // If the child dialog ("bookingDialog") was cancelled or the user failed to confirm, the Result here will be null.
            if (stepContext.result) {
                const result = stepContext.result;
                // Now we have all the booking details.

                // This is where calls to the booking AOU service or database would go.

                // If the call to the booking service was successful tell the user.
                const timeProperty = new TimexProperty(result.travelDate);
                const travelDateMsg = timeProperty.toNaturalLanguage(new Date(Date.now()));
                const msg = `I have you booked to ${ result.destination } from ${ result.origin } on ${ travelDateMsg }.`;
                await stepContext.context.sendActivity(msg, msg, InputHints.IgnoringInput);
            }

            // Restart the main dialog with a different message the second time around
            return await stepContext.replaceDialog(this.initialDialogId, { restartMsg: 'What else can I do for you?' });
        }
    }

    module.exports.MainDialog = MainDialog;
    ```

<a name="ask-bot-a-question-for-the-book-flight-intent"></a>

## <a name="start-the-bot-code"></a>Bot kodunu başlatma

1. Bir Windows konsolu veya Linux veya macOS terminali açın.

1. Bot kodundaki dizine geçin ve dotenv bağımlılığını yüklemek için aşağıdaki komutu girin:

    ```console
    npm install dotenv --save
    ```

1. Bot 'ı başlatmak için aşağıdaki komutu girin:

    ```console
    node index.js
    ```

Web uygulaması robotunun `http://localhost:3978/` adresindeki sitesi ile bir tarayıcı penceresi açılır. Bir giriş sayfası, bot 'unuzla ilgili bilgileri görüntüler.

![Bir giriş sayfası, bot 'unuzla ilgili bilgileri görüntüler.](./media/bfv4-csharp/running-bot-web-home-page-success.png)

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Bot öykünücüsünü test etmek için kullanın

Kitap uçuş amacı için bir soru sorun.

1. Bot öykünücüsünü başlatın ve **bot 'ı aç**' ı seçin.
1. **Bir bot** açılır penceresi açın iletişim kutusunda, gıbı bot URL 'nizi girin `http://localhost:3978/api/messages` . `/api/messages`Yol, bot 'ın Web adresidir.
1. İndirdiğiniz bot kodunun kökündeki **. env** dosyasında bulunan **Microsoft uygulama kimliği** ve **Microsoft uygulama parolasını**girin.

1. Bot öykünücüsünde, `Book a flight from Seattle to Berlin tomorrow` **Web sohbetinde testte**aldığınız gibi temel bot için aynı yanıtı girin ve alın.

    [![Öykünücüde temel bot yanıtı](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)

1. **Evet**’i seçin. Bot, eylemlerinin bir özeti ile yanıt verir.
1. Bot öykünücüsünün günlüğünden, dahil edilen satırı seçin `<- trace LuisV3 Trace` . Bu, deterance 'in amacı ve varlıkları için LUSıS 'den gelen JSON yanıtını görüntüler.

    [![Öykünücüde temel bot yanıtı](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)

[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

Konuşma botları ile daha fazla [örnek](https://github.com/microsoft/botframework-solutions) görüntüleyin.

> [!div class="nextstepaction"]
> [Özel konu etki alanı ile Language Understanding uygulaması oluşturma](luis-quickstart-intents-only.md)
