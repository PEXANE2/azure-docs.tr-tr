---
title: Language Understanding bot Node. js v4
titleSuffix: Azure Cognitive Services
description: Node.js'yi kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bu sohbet robotu, bir robot çözümünü kısa sürede gerçekleştirmek için İnsan Kaynakları uygulamasını kullanır. Robot, Bot Framework 4 sürümü ve Azure Web uygulaması robotu ile geliştirilmiştir.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 08/30/2019
ms.author: diberry
ms.openlocfilehash: 8455a9c9ecff89643e090f1d763a44f97f5779f5
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2019
ms.locfileid: "70206889"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Öğretici: Node. js ' de Language Understanding etkin bir Web uygulaması bot kullanın 

Dil anlama (LUU) ile tümleştirilmiş bir sohbet bot oluşturmak için Node. js ' yi kullanın. Bot, Azure [Web App bot](https://docs.microsoft.com/azure/bot-service/) kaynağı ve [bot Framework sürüm](https://github.com/Microsoft/botbuilder-dotnet) v4 ile oluşturulmuştur.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Web uygulaması robotu oluşturma. Bu işlem sizin için yeni bir LUIS uygulaması oluşturur.
> * Web bot hizmeti tarafından oluşturulan bot projesini indirin
> * Robotu ve öykünücüyü bilgisayarınızda yerel olarak başlatma
> * Robotta konuşma sonuçlarını görüntüleme

## <a name="prerequisites"></a>Önkoşullar

* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-a-web-app-bot-resource"></a>Web uygulaması bot kaynağı oluşturma

1. [Azure portalda](https://portal.azure.com) **Yeni kaynak oluştur**'u seçin.

1. Arama kutusunda **Web Uygulaması Robotu**'nu arayın ve bunu seçin. **Oluştur**’u seçin.

1. **Robot Hizmeti**'nde gerekli bilgileri sağlayın:

    |Ayar|Amaç|Önerilen ayar|
    |--|--|--|
    |Robot adı|Kaynak adı|`luis-nodejs-bot-` + `<your-name>`, örneğin `luis-nodejs-bot-johnsmith`|
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
    |SDK sürümü|Robot Framework sürümü|**SDK v4**|
    |SDK dili|Robotun programlama dili|**Node.js**|
    |Hecesi|Robot türü|**Temel robot**|
    
1. **Oluştur**’u seçin. Robot hizmetini oluşturur ve Azure'a dağıtır. Bu işlemin bir parçası olarak `luis-nodejs-bot-XXXX` adlı bir LUIS uygulaması oluşturulur. Bu ad/Azure bot hizmeti uygulama adını temel alır.

    [![Web uygulaması bot oluştur](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Devam etmeden önce bot hizmeti oluşturuluncaya kadar bekleyin.

## <a name="the-bot-has-a-language-understanding-model"></a>Bot 'ın bir Language Understanding modeli vardır

Bot hizmeti oluşturma işlemi, amaçlar ve örnek dıklarla yeni bir LUO uygulaması da oluşturur. Robot, yeni LUIS uygulamasına şu amaçlar için amaç eşlemesi sağlar: 

|Temel robot LUIS amaçları|örnek konuşma|
|--|--|
|Kitap kolu|`Travel to Paris`|
|İptal|`bye`|
|Yok.|Uygulamanın etki alanı dışındaki her şey.|

## <a name="test-the-bot-in-web-chat"></a>Bot Web Chat test edin.

1. Yeni bot için Azure portal devam ederken, **Web sohbetinde test**' i seçin. 
1. **Iletinizi yazın** metin kutusuna metni `hello`girin. Bot, bot Framework ile ilgili bilgilerle yanıt verir, Ayrıca, Paris 'e bir uçuş sağlamak gibi belirli bir LUL modeli için örnek sorgular. 

    ![Azure portal ekran görüntüsü, ' Merhaba ' metnini girin.](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    Botunuzu hızlı bir şekilde test etmek için test işlevselliğini kullanabilirsiniz. Hata ayıklama da dahil olmak üzere daha kapsamlı bir test için, bot kodunu indirin ve Visual Studio 'Yu kullanın. 

## <a name="download-the-web-app-bot-source-code"></a>Web uygulaması bot kaynak kodunu indirin
Web uygulaması robot kodunu geliştirmek için, yerel bilgisayarınızda kodu indirin ve kullanın. 

1. Azure portalda, **Robot yönetimi** bölümünden **Derle**'yi seçin. 

1. **Robot kaynak kodunu indir**'i seçin. 

    [![Temel bot için Web uygulaması bot kaynak kodunu indirin](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Açılan iletişim kutusu **indirilen ZIP dosyasında uygulama ayarlarını dahil**Istediğinde, **Evet**' i seçin.

1. Kaynak kodu .zip dosyasına sıkıştırılmışsa, bir iletide kodu indirme bağlantısı sağlanır. Bağlantıyı seçin. 

1. Bu .zip dosyasını yerel bilgisayarınıza kaydedin ve dosyaları ayıklayın. Projeyi Visual Studio ile açın. 

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Lua 'ya ve yanıtı almaya yönelik kodu gözden geçirin

1. **İletişim kutusu-> luisHelper. js** dosyasını açın. Robota girilen kullanıcı konuşmasının LUIS'e gönderildiği yer budur. LUSıS yanıtı, yönteminden bir **Bookdetails** JSON nesnesi olarak döndürülür. Kendi bot uygulamanızı oluşturduğunuzda, LUSıS 'den ayrıntıları döndürmek için kendi nesneniz da oluşturmanız gerekir. 

    ```nodejs
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { LuisRecognizer } = require('botbuilder-ai');
    
    class LuisHelper {
        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {*} logger
         * @param {TurnContext} context
         */
        static async executeLuisQuery(logger, context) {
            const bookingDetails = {};
    
            try {
                const recognizer = new LuisRecognizer({
                    applicationId: process.env.LuisAppId,
                    endpointKey: process.env.LuisAPIKey,
                    endpoint: `https://${ process.env.LuisAPIHostName }`
                }, {}, true);
    
                const recognizerResult = await recognizer.recognize(context);
    
                const intent = LuisRecognizer.topIntent(recognizerResult);
    
                bookingDetails.intent = intent;
    
                if (intent === 'Book_flight') {
                    // We need to get the result from the LUIS JSON which at every level returns an array
    
                    bookingDetails.destination = LuisHelper.parseCompositeEntity(recognizerResult, 'To', 'Airport');
                    bookingDetails.origin = LuisHelper.parseCompositeEntity(recognizerResult, 'From', 'Airport');
    
                    // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                    // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                    bookingDetails.travelDate = LuisHelper.parseDatetimeEntity(recognizerResult);
                }
            } catch (err) {
                logger.warn(`LUIS Exception: ${ err } Check your LUIS configuration`);
            }
            return bookingDetails;
        }
    
        static parseCompositeEntity(result, compositeName, entityName) {
            const compositeEntity = result.entities[compositeName];
            if (!compositeEntity || !compositeEntity[0]) return undefined;
    
            const entity = compositeEntity[0][entityName];
            if (!entity || !entity[0]) return undefined;
    
            const entityValue = entity[0][0];
            return entityValue;
        }
    
        static parseDatetimeEntity(result) {
            const datetimeEntity = result.entities['datetime'];
            if (!datetimeEntity || !datetimeEntity[0]) return undefined;
    
            const timex = datetimeEntity[0]['timex'];
            if (!timex || !timex[0]) return undefined;
    
            const datetime = timex[0].split('T')[0];
            return datetime;
        }
    }
    
    module.exports.LuisHelper = LuisHelper;
    ```

1. İletişim akışını yönetmek için BookingDetails nesnesinin nasıl kullanıldığını anlamak için **iletişim kutularını açın-> bookingDialog. js** . Seyahat ayrıntılarına adım sorulur, ardından tüm kayıt onaylanır ve son olarak kullanıcıya tekrar yinelenir. 

    ```nodejs
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { TimexProperty } = require('@microsoft/recognizers-text-data-types-timex-expression');
    const { ConfirmPrompt, TextPrompt, WaterfallDialog } = require('botbuilder-dialogs');
    const { CancelAndHelpDialog } = require('./cancelAndHelpDialog');
    const { DateResolverDialog } = require('./dateResolverDialog');
    
    const CONFIRM_PROMPT = 'confirmPrompt';
    const DATE_RESOLVER_DIALOG = 'dateResolverDialog';
    const TEXT_PROMPT = 'textPrompt';
    const WATERFALL_DIALOG = 'waterfallDialog';
    
    class BookingDialog extends CancelAndHelpDialog {
        constructor(id) {
            super(id || 'bookingDialog');
    
            this.addDialog(new TextPrompt(TEXT_PROMPT))
                .addDialog(new ConfirmPrompt(CONFIRM_PROMPT))
                .addDialog(new DateResolverDialog(DATE_RESOLVER_DIALOG))
                .addDialog(new WaterfallDialog(WATERFALL_DIALOG, [
                    this.destinationStep.bind(this),
                    this.originStep.bind(this),
                    this.travelDateStep.bind(this),
                    this.confirmStep.bind(this),
                    this.finalStep.bind(this)
                ]));
    
            this.initialDialogId = WATERFALL_DIALOG;
        }
    
        /**
         * If a destination city has not been provided, prompt for one.
         */
        async destinationStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            if (!bookingDetails.destination) {
                return await stepContext.prompt(TEXT_PROMPT, { prompt: 'To what city would you like to travel?' });
            } else {
                return await stepContext.next(bookingDetails.destination);
            }
        }
    
        /**
         * If an origin city has not been provided, prompt for one.
         */
        async originStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the response to the previous step's prompt
            bookingDetails.destination = stepContext.result;
            if (!bookingDetails.origin) {
                return await stepContext.prompt(TEXT_PROMPT, { prompt: 'From what city will you be travelling?' });
            } else {
                return await stepContext.next(bookingDetails.origin);
            }
        }
    
        /**
         * If a travel date has not been provided, prompt for one.
         * This will use the DATE_RESOLVER_DIALOG.
         */
        async travelDateStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the results of the previous step
            bookingDetails.origin = stepContext.result;
            if (!bookingDetails.travelDate || this.isAmbiguous(bookingDetails.travelDate)) {
                return await stepContext.beginDialog(DATE_RESOLVER_DIALOG, { date: bookingDetails.travelDate });
            } else {
                return await stepContext.next(bookingDetails.travelDate);
            }
        }
    
        /**
         * Confirm the information the user has provided.
         */
        async confirmStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the results of the previous step
            bookingDetails.travelDate = stepContext.result;
            const msg = `Please confirm, I have you traveling to: ${ bookingDetails.destination } from: ${ bookingDetails.origin } on: ${ bookingDetails.travelDate }.`;
    
            // Offer a YES/NO prompt.
            return await stepContext.prompt(CONFIRM_PROMPT, { prompt: msg });
        }
    
        /**
         * Complete the interaction and end the dialog.
         */
        async finalStep(stepContext) {
            if (stepContext.result === true) {
                const bookingDetails = stepContext.options;
    
                return await stepContext.endDialog(bookingDetails);
            } else {
                return await stepContext.endDialog();
            }
        }
    
        isAmbiguous(timex) {
            const timexPropery = new TimexProperty(timex);
            return !timexPropery.types.has('definite');
        }
    }
    
    module.exports.BookingDialog = BookingDialog;
    ```


## <a name="install-dependencies-and-start-the-bot-code-in-visual-studio"></a>Visual Studio 'da bağımlılıkları yükleyip bot kodunu başlatma

1. VSCode 'da, tümleşik terminalden komutuyla `npm install`bağımlılıkları yükler.
1. Ayrıca, tümleşik terminalde komutuyla `npm start`bot 'ı başlatın. 


## <a name="use-the-bot-emulator-to-test-the-bot"></a>Bot öykünücüsünü test etmek için kullanın

1. Bot öykünücüsünü başlatın ve **bot 'ı aç**' ı seçin.
1. **Bir bot** açılır penceresi açın iletişim kutusunda, gıbı bot URL `http://localhost:3978/api/messages`'nizi girin. `/api/messages` Yol, bot 'ın Web adresidir.
1. İndirdiğiniz bot kodunun kökündeki **. env** dosyasında bulunan **Microsoft uygulama kimliği** ve **Microsoft uygulama parolasını**girin.

    İsteğe bağlı olarak, yeni bir bot yapılandırması oluşturabilir ve bu dosyayı `MicrosoftAppId` ve `MicrosoftAppPassword` Visual Studio projesindeki **. env** dosyasından bot için kopyalayabilirsiniz. Bot yapılandırma dosyasının adı, bot adı ile aynı olmalıdır. 

    ```json
    {
        "name": "<bot name>",
        "description": "<bot description>",
        "services": [
            {
                "type": "endpoint",
                "appId": "<appId from .env>",
                "appPassword": "<appPassword from .env>",
                "endpoint": "http://localhost:3978/api/messages",
                "id": "<don't change this value>",
                "name": "http://localhost:3978/api/messages"
            }
        ],
        "padlock": "",
        "version": "2.0",
        "overrides": null,
        "path": "<local path to .bot file>"
    }
    ```

1. Bot öykünücüsünde, `Hello` **Web sohbetinde testte**aldığınız gibi temel bot için aynı yanıtı girin ve alın.

    [![Öykünücüde temel bot yanıtı](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)


## <a name="ask-bot-a-question-for-the-book-flight-intent"></a>Kitap uçuş amacı için bir soru sorun

1. Bot öykünücüsünde aşağıdaki utterance girerek bir uçuş yapın: 

    ```console
    Book a flight from Paris to Berlin on March 22, 2020
    ```

    Bot öykünücüsü şunları onaylamanızı ister. 

1. **Evet**' i seçin. Bot, eylemlerinin bir özeti ile yanıt verir. 
1. Bot öykünücüsünün günlüğünden, dahil `Luis Trace`edilen satırı seçin. Bu, deterance 'in amacı ve varlıkları için LUSıS 'den gelen JSON yanıtını görüntüler.

    [![Öykünücüde temel bot yanıtı](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)


[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

Konuşma botları ile daha fazla [örnek](https://github.com/microsoft/botframework-solutions) görüntüleyin. 

> [!div class="nextstepaction"]
> [Özel konu etki alanı ile Language Understanding uygulaması oluşturma](luis-quickstart-intents-only.md)