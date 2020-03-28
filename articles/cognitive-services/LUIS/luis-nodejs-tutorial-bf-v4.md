---
title: 'Öğretici: Dil Anlama Bot Node.js v4'
description: Node.js kullanarak, bu öğreticide dil bilgisi (LUIS) ile entegre edilmiş bir sohbet botu oluşturun. Bu sohbet robotu, bir robot çözümünü kısa sürede gerçekleştirmek için İnsan Kaynakları uygulamasını kullanır. Robot, Bot Framework sürümü 4 ve Azure Web uygulaması robotu ile geliştirilmiştir.
ms.topic: tutorial
ms.date: 02/03/2020
ms.openlocfilehash: 3ce12176957412a5599ced8b043f553969194efb
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76987858"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Öğretici: Düğüm.js Dil Anlayışı ile etkin bir Web App Bot kullanın

Dil anlama (LUIS) ile entegre bir sohbet botu oluşturmak için Node.js kullanın. Bot, Azure Web [uygulaması bot](https://docs.microsoft.com/azure/bot-service/) kaynağı ve [Bot Framework sürümü](https://github.com/Microsoft/botbuilder-dotnet) V4 ile oluşturulmuştur.

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Web uygulaması robotu oluşturma. Bu işlem sizin için yeni bir LUIS uygulaması oluşturur.
> * Web bot hizmeti tarafından oluşturulan bot projesini indirin
> * Robotu ve öykünücüyü bilgisayarınızda yerel olarak başlatma
> * Robotta konuşma sonuçlarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-a-web-app-bot-resource"></a>Bir web uygulaması bot kaynağı oluşturma

1. [Azure portalda](https://portal.azure.com)**Yeni kaynak oluştur**'u seçin.

1. Arama kutusunda **Web Uygulaması Robotu**'nu arayın ve bunu seçin. **Oluştur'u**seçin.

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
    |Uygulama hizmet planı/Konum|Sağlanan varsayılan değerden değiştirmeyin.|
    |Application Insights|Sağlanan varsayılan değerden değiştirmeyin.|
    |Microsoft Uygulama Kimliği ve parola|Sağlanan varsayılan değerden değiştirmeyin.|

1. Bot **şablonunda**aşağıdakileri seçin ve ardından aşağıdaki ayarların altındaki **Seç** düğmesini seçin:

    |Ayar|Amaç|Seçim|
    |--|--|--|
    |SDK sürümü|Robot Framework sürümü|**SDK v4**|
    |SDK dili|Robotun programlama dili|**Node.js**|
    |Bot|Robot türü|**Temel robot**|

1. **Oluştur'u**seçin. Robot hizmetini oluşturur ve Azure'a dağıtır. Bu işlemin bir parçası olarak `luis-nodejs-bot-XXXX` adlı bir LUIS uygulaması oluşturulur. Bu ad , /Azure Bot Hizmeti uygulama adını temel alıntır.

    > [!div class="mx-imgBorder"]
    > [![Web uygulaması robotu oluşturma](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Devam etmeden önce bot hizmeti oluşturulana kadar bekleyin.

## <a name="the-bot-has-a-language-understanding-model"></a>Botun Dil Anlama modeli vardır

Bot hizmeti oluşturma işlemi de niyet ve örnek söyleyiş ile yeni bir LUIS uygulaması oluşturur. Robot, yeni LUIS uygulamasına şu amaçlar için amaç eşlemesi sağlar:

|Temel robot LUIS amaçları|örnek konuşma|
|--|--|
|Kitap uçuşu|`Travel to Paris`|
|İptal|`bye`|
|GetWeather|`what's the weather like?`|
|None|Uygulamanın etki alanı dışındaki her şey.|

## <a name="test-the-bot-in-web-chat"></a>Web Sohbet'te botu test edin

1. Yeni bot için Azure portalında yken, **Web Sohbetinde Test'i**seçin.
1. **İletimetin kutusunu yazın,** metni `Book a flight from Seattle to Berlin tomorrow`girin. Bot, bir uçuş rezervasyonu yapmak istediğinizi doğrulayan bir yanıt verir.

    ![Azure portalının ekran görüntüsüne 'merhaba' metnini girin.](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    Botunuzu hızlı bir şekilde test etmek için test işlevini kullanabilirsiniz. Hata ayıklama da dahil olmak üzere daha eksiksiz test için, bot kodunu indirin ve Visual Studio'yu kullanın.

## <a name="download-the-web-app-bot-source-code"></a>Web uygulaması bot kaynak kodunu indirin
Web uygulaması robot kodunu geliştirmek için, yerel bilgisayarınızda kodu indirin ve kullanın.

1. Azure portalda, **Robot yönetimi** bölümünden **Derle**'yi seçin.

1. **Robot kaynak kodunu indir**'i seçin.

    [![Temel bot için web uygulaması bot kaynak kodu indirin](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Açılan iletişim kutusu sorduğunda **Yes** **indirilen zip dosyasına uygulama ayarlarını ekle?** Bu, LUIS ayarlarını sağlar.

1. Kaynak kodu .zip dosyasına sıkıştırılmışsa, bir iletide kodu indirme bağlantısı sağlanır. Bağlantıyı seçin.

1. Bu .zip dosyasını yerel bilgisayarınıza kaydedin ve dosyaları ayıklayın. Visual Studio ile projeyi açın.

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>LUIS'e söyleyiş göndermek ve yanıt almak için kodu gözden geçirme

1. Kullanıcı sözcüğünün söylenmesini LUIS tahmin bitiş noktasına göndermek için, **iletişim noktalarını -> flightBookingRecognizer.js** dosyasını açın. Robota girilen kullanıcı konuşmasının LUIS'e gönderildiği yer budur. LUIS'in yanıtı **executeLuisQuery** yönteminden döndürülür.

    ````javascript
    class FlightBookingRecognizer {

        ...

        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {TurnContext} context
         */
        async executeLuisQuery(context) {
            return await this.recognizer.recognize(context);
        }

        ...

    }
    ````

1. **Dialogs -> mainDialog** söyleyişi yakalar ve actStep yöntemiyle executeLuisQuery'ye gönderir.


    ````javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.

    const { LuisRecognizer } = require('botbuilder-ai');

    class FlightBookingRecognizer {
        constructor(config) {
            const luisIsConfigured = config && config.applicationId && config.endpointKey && config.endpoint;
            if (luisIsConfigured) {
                this.recognizer = new LuisRecognizer(config, {}, true);
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
    ````
<a name="ask-bot-a-question-for-the-book-flight-intent"></a>

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Bot test etmek için bot emülatörü kullanın

Kitap Uçuş niyeti için bot bir soru sorun.

1. Bot Emülatörü başlatın ve **Açık Bot**seçin.
1. Bir **bot** açılır pencere aç iletişim kutusunda, bot URL'nizi girin, örneğin. `http://localhost:3978/api/messages` `/api/messages` Rota bot için web adresidir.
1. İndirdiğiniz bot kodunun kökündeki **.env** dosyasında bulunan **Microsoft Uygulama Kimliği** ve Microsoft Uygulama **parolasını**girin.

1. Bot emülatöründe, `Book a flight from Seattle to Berlin tomorrow` **Web Sohbeti'ndeki Test'te**aldığınız temel bot için aynı yanıtı girin ve alın.

    [![Emülatörde temel bot yanıtı](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)

1. **Evet'i**seçin. Bot eylemlerinin bir özeti ile yanıt verir.
1. Bot emülatörü günlüğünden, içeren `Luis Trace`satırı seçin. Bu, söyleyiş niyeti ve varlıkları için LUIS gelen JSON yanıtı görüntüler.

    [![Emülatörde temel bot yanıtı](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)


[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

Konuşma botları ile daha fazla [örnek](https://github.com/microsoft/botframework-solutions) görün.

> [!div class="nextstepaction"]
> [Özel bir konu etki alanıyla Dil Anlama uygulaması oluşturma](luis-quickstart-intents-only.md)