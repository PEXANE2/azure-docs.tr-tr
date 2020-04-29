---
title: 'Öğretici: Language Understanding bot Node. js v4'
description: Node. js ' yi kullanarak, bu öğreticide dil anlama (LUU) ile tümleştirilmiş bir sohbet bot oluşturun. Bu sohbet robotu, bir robot çözümünü kısa sürede gerçekleştirmek için İnsan Kaynakları uygulamasını kullanır. Robot, Bot Framework sürümü 4 ve Azure Web uygulaması robotu ile geliştirilmiştir.
ms.topic: tutorial
ms.date: 02/03/2020
ms.openlocfilehash: 3ce12176957412a5599ced8b043f553969194efb
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76987858"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Öğretici: node. js ' de Language Understanding etkin bir Web uygulaması bot kullanın

Dil anlama (LUU) ile tümleştirilmiş bir sohbet bot oluşturmak için Node. js ' yi kullanın. Bot, Azure [Web App bot](https://docs.microsoft.com/azure/bot-service/) kaynağı ve [bot Framework sürüm](https://github.com/Microsoft/botbuilder-dotnet) v4 ile oluşturulmuştur.

**Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:**

> [!div class="checklist"]
> * Web uygulaması robotu oluşturma. Bu işlem sizin için yeni bir LUIS uygulaması oluşturur.
> * Web bot hizmeti tarafından oluşturulan bot projesini indirin
> * Robotu ve öykünücüyü bilgisayarınızda yerel olarak başlatma
> * Robotta konuşma sonuçlarını görüntüleme

## <a name="prerequisites"></a>Ön koşullar

* [Robot öykünücüsü](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-a-web-app-bot-resource"></a>Web uygulaması bot kaynağı oluşturma

1. [Azure portalda](https://portal.azure.com)**Yeni kaynak oluştur**'u seçin.

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

    > [!div class="mx-imgBorder"]
    > [![Web uygulaması robotu oluşturma](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Devam etmeden önce bot hizmeti oluşturuluncaya kadar bekleyin.

## <a name="the-bot-has-a-language-understanding-model"></a>Bot 'ın bir Language Understanding modeli vardır

Bot hizmeti oluşturma işlemi, amaçlar ve örnek dıklarla yeni bir LUO uygulaması da oluşturur. Robot, yeni LUIS uygulamasına şu amaçlar için amaç eşlemesi sağlar:

|Temel robot LUIS amaçları|örnek konuşma|
|--|--|
|Kitap kolu|`Travel to Paris`|
|İptal|`bye`|
|Gethava durumu|`what's the weather like?`|
|Hiçbiri|Uygulamanın etki alanı dışındaki her şey.|

## <a name="test-the-bot-in-web-chat"></a>Web sohbetinde bot 'ı test etme

1. Yeni bot için Azure portal devam ederken, **Web sohbetinde test**' i seçin.
1. **Iletinizi yazın** metin kutusuna metni `Book a flight from Seattle to Berlin tomorrow`girin. Bot, bir uçuş sağlamak istediğiniz doğrulama ile yanıt verir.

    ![Azure portal ekran görüntüsü, ' Merhaba ' metnini girin.](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    Botunuzu hızlı bir şekilde test etmek için test işlevselliğini kullanabilirsiniz. Hata ayıklama da dahil olmak üzere daha kapsamlı bir test için, bot kodunu indirin ve Visual Studio 'Yu kullanın.

## <a name="download-the-web-app-bot-source-code"></a>Web uygulaması bot kaynak kodunu indirin
Web uygulaması robot kodunu geliştirmek için, yerel bilgisayarınızda kodu indirin ve kullanın.

1. Azure portalda, **Robot yönetimi** bölümünden **Derle**'yi seçin.

1. **Robot kaynak kodunu indir**'i seçin.

    [![Temel bot için Web uygulaması bot kaynak kodunu indirin](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Açılan iletişim kutusu **indirilen ZIP dosyasında uygulama ayarlarını dahil**Istediğinde, **Evet**' i seçin. Bu, LUSıS ayarlarını sağlar.

1. Kaynak kodu .zip dosyasına sıkıştırılmışsa, bir iletide kodu indirme bağlantısı sağlanır. Bağlantıyı seçin.

1. Bu .zip dosyasını yerel bilgisayarınıza kaydedin ve dosyaları ayıklayın. Projeyi Visual Studio ile açın.

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Lua 'ya ve yanıtı almaya yönelik kodu gözden geçirin

1. Kullanıcı iletişim kutusunu LUSıS tahmin uç noktasına göndermek için, **iletişim > Flightbookingtanıyıcı. js** dosyasını açın. Robota girilen kullanıcı konuşmasının LUIS'e gönderildiği yer budur. LU, **executeLuisQuery** yönteminden yanıt döndürülür.

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

1. **İletişim kutuları-> MainDialog** , söylenişi 'i yakalar ve actstep yönteminde executeLuisQuery 'e gönderir.


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

## <a name="use-the-bot-emulator-to-test-the-bot"></a>Bot öykünücüsünü test etmek için kullanın

Kitap uçuş amacı için bir soru sorun.

1. Bot öykünücüsünü başlatın ve **bot 'ı aç**' ı seçin.
1. **Bir bot** açılır penceresi açın iletişim kutusunda, gıbı bot URL 'nizi girin `http://localhost:3978/api/messages`. `/api/messages` Yol, bot 'ın Web adresidir.
1. İndirdiğiniz bot kodunun kökündeki **. env** dosyasında bulunan **Microsoft uygulama kimliği** ve **Microsoft uygulama parolasını**girin.

1. Bot öykünücüsünde, `Book a flight from Seattle to Berlin tomorrow` **Web sohbetinde testte**aldığınız gibi temel bot için aynı yanıtı girin ve alın.

    [![Öykünücüde temel bot yanıtı](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)

1. **Evet**' i seçin. Bot, eylemlerinin bir özeti ile yanıt verir.
1. Bot öykünücüsünün günlüğünden, dahil `Luis Trace`edilen satırı seçin. Bu, deterance 'in amacı ve varlıkları için LUSıS 'den gelen JSON yanıtını görüntüler.

    [![Öykünücüde temel bot yanıtı](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)


[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Sonraki adımlar

Konuşma botları ile daha fazla [örnek](https://github.com/microsoft/botframework-solutions) görüntüleyin.

> [!div class="nextstepaction"]
> [Özel konu etki alanı ile Language Understanding uygulaması oluşturma](luis-quickstart-intents-only.md)