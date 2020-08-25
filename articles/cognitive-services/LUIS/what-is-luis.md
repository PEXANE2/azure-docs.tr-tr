---
title: Language Understanding (LUIS) nedir?
description: Language Understanding (LUSıS)-, anlam tahmin etmek ve bilgi ayıklamak için, doğal dilde makine öğrenimini kullanan bulut tabanlı bir API hizmetidir.
keywords: Azure, yapay zeka, AI, doğal dil işleme, NLP, doğal dil anlama, NLU, AI konuşması, konuşma sohbet botu, sohbet botu Maker, LUO, NLP AI, lusıs AI, Azure LUL, doğal dil anlama
ms.topic: overview
ms.date: 08/07/2020
ms.custom: cog-serv-seo-aug-2020
ms.openlocfilehash: 22fe99e1552a9612adfbc455d60852f1591a1a54
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88752156"
---
# <a name="what-is-language-understanding-luis"></a>Language Understanding (LUIS) nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Language Understanding (LUIS), genel anlamı tahmin etmek ve ilgili, ayrıntılı bilgileri çekme amacıyla kullanıcının konuşmasına, doğal dil metnine özel makine öğrenimi zekası uygulayan bulut tabanlı API hizmetidir.

LUIS için istemci uygulaması, bir görevi tamamlamak için kullanıcıyla doğal dil kullanarak iletişim kuran konuşma uygulamasıdır. İstemci uygulamalarına örnek olarak sosyal medya uygulamaları, sohbet botları ve konuşma özellikli masaüstü uygulamaları verilebilir.

![Bilişsel hizmetler Language Understanding (LUSıS) ile çalışan 3 istemci uygulamasının kavramsal resmi](./media/luis-overview/luis-entry-point.png "Bilişsel hizmetler Language Understanding (LUSıS) ile çalışan 3 istemci uygulamasının kavramsal resmi")

## <a name="use-luis-in-a-chat-bot"></a>Sohbet botunda LUIS kullanımı

<a name="Accessing-LUIS"></a>

LUIS uygulaması yayımlandıktan sonra istemci uygulaması konuşmaları (metni) LUIS doğal dil işleme uç noktası [API'sine][endpoint-apis] gönderir ve sonuçları JSON yanıtı olarak alır. Sık kullanılan LUIS istemci uygulamalarından biri, sohbet botudur.


![Kullanıcı metnini doğal dil anlama (NLP) ile tahmin etmek için, lu](./media/luis-overview/LUIS-chat-bot-request-response.svg "Kullanıcı metnini doğal dil anlama ile tahmin etmek için, lu")

|Adım|Eylem|
|:--|:--|
|1|İstemci uygulaması, kullanıcının "İK temsilcimi aramak istiyorum." şeklindeki _konuşmasını_ (kendi kullandıkları kelimelerle) bir HTTP isteği olarak LUIS uç noktasına gönderir.|
|2|LUO, uygulamanıza zeka eklemek için özel dil modellerinizi yapmanızı sağlar. Makine tarafından öğrenilen dil modelleri kullanıcının yapılandırılmamış giriş metnini alır ve en iyi amaç ile JSON biçimli bir yanıt döndürür `HRContact` . JSON uç nokta yanıtı minimumda sorgu konuşmasını ve en yüksek puanlı amacı içerir. Ayrıca, _kişi türü_ varlığı gibi verileri de ayıklayabilir.|
|3|İstemci uygulaması, JSON yanıtını kullanarak kullanıcının isteklerini gerçekleştirmeyle ilgili kararları verir. Bu kararlar, bot Framework kodunda karar ağacı ve diğer hizmetlere çağrılar içerebilir. |

LUIS uygulaması, istemci uygulamasının akıllı seçimler yapabilmesi için gerekli bilgileri sunar. LUIS bu seçenekleri sağlamaz.

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-understanding-nlu"></a>Doğal dil anlama (NLU)

Luo, doğal dil işlemenin (NLP) bir alt kümesi olan NLU biçiminde [yapay zeka (AI) sağlar](artificial-intelligence.md) .

LUSıS uygulamanız, etki alanına özgü doğal dil modeli içerir. LUIS uygulamasını önceden oluşturulmuş bir etki alanı modeliyle başlatabilir, kendi modelinizi oluşturabilir veya önceden oluşturulmuş etki alanının belirli bölümlerini kendi özel bilgilerinizle karıştırabilirsiniz.

* **Önceden oluşturulmuş model**: LUIS amaç, konuşma ve önceden oluşturulmuş varlık içeren birçok önceden oluşturulmuş etki alanı modeline sahiptir. Önceden oluşturulmuş modelin amaçlarını ve konuşmalarını kullanmak zorunda kalmadan önceden oluşturulmuş varlıkları kullanabilirsiniz. [Önceden oluşturulmuş etki alanı modelleri](luis-how-to-use-prebuilt-domains.md), tasarımın tamamını içerir ve LUIS hizmetini kullanmaya başlamak için ideal bir yoldur.

* **Özel model** LUO, amaçları ve varlıkları dahil kendi özel modellerinizi belirlemek için size çeşitli yollar sağlar. Varlıklar, makine öğrenimi varlıklarını, belirli veya değişmez varlıkları ve makine öğrenimi ve değişmez değer birleşimini içerir.

[NLP](artificial-intelligence.md)hakkında daha fazla bilgi ve NLU 'ın Luo 'ya özgü alanı.

## <a name="step-1-design-and-build-your-model"></a>1. Adım: modelinizi tasarlama ve oluşturma

Modellerinizi, **[Amaç](luis-concept-intent.md)** olarak adlandırılan Kullanıcı kullanıcıları kategorilerine tasarlayın. Her amaç için kullanıcı **[konuşmaları](luis-concept-utterance.md)** örneklerine ihtiyaç duyulur. Her söylük, [makine öğrenimi varlıklarıyla](luis-concept-entity-types.md#effective-machine-learned-entities)ayıklanmak için gereken verileri sağlayabilir.

|Örnek kullanıcı konuşması|Amaç|Ayıklanan veriler|
|-----------|-----------|-----------|
|`Book a flight to Seattle?`|BookFlight|Seattle|
|`When does your store open?`|StoreHoursAndLocation|open|
|`Schedule a meeting at 1pm with Bob in Distribution`|ScheduleMeeting|13, Bob|

Modeli [yazma](https://go.microsoft.com/fwlink/?linkid=2092087) API 'leri, ya da [**LUIS portalı**](https://www.luis.ai)veya her ikisiyle birlikte oluşturun. [Portal](get-started-portal-build-app.md) ve [SDK istemci kitaplıkları](quickstart-sdk.md)ile derleme hakkında daha fazla bilgi edinin.

## <a name="step-2-get-the-query-prediction"></a>2. Adım: sorgu tahminini alın

Uygulamanızın modeli eğitilen ve uç noktada yayımlandıktan sonra, bir istemci uygulaması (örneğin, bir sohbet bot), tahmin [uç noktası](https://go.microsoft.com/fwlink/?linkid=2092356) API 'sine yönelik olarak gönderilir. API, modeli Analize analiz için uygular ve tahmin sonuçlarıyla bir JSON biçiminde yanıt verir.

JSON uç nokta yanıtı minimumda sorgu konuşmasını ve en yüksek puanlı amacı içerir. Ayrıca, aşağıdaki **kişi türü** varlığı ve genel yaklaşım gibi verileri de ayıklayabilir.

```JSON
{
    "query": "I want to call my HR rep",
    "prediction": {
        "topIntent": "HRContact",
        "intents": {
            "HRContact": {
                "score": 0.8582669
            }
        },
        "entities": {
            "Contact Type": [
                "call"
            ]
        },
        "sentiment": {
            "label": "neutral",
            "score": 0.5
        }
    }
}
```

## <a name="step-3-improve-model-prediction"></a>3. Adım: model tahminini geliştirme

LUP uygulamanız yayımlandıktan ve gerçek Kullanıcı dıklılığını aldıktan sonra, Lud, tahmin doğruluğunu artırmak için uç nokta dıklarınızın etkin bir şekilde [öğrenilmesine](luis-concept-review-endpoint-utterances.md) olanak sağlar. Geliştirme yaşam döngünüzün düzenli bakım çalışmalarınızın bir parçası olarak bu önerileri gözden geçirin.

<a name="using-luis"></a>

## <a name="development-lifecycle-and-tools"></a>Geliştirme yaşam döngüsü ve araçları
LUO, tüm [geliştirme yaşam döngüsüyle](luis-concept-app-iteration.md)tümleştirilecek diğer Luo yazarlarıyla birlikte araçlar, sürüm oluşturma ve işbirliği sağlar.

Language Understanding (LUU), bir REST API olarak HTTP isteğiyle herhangi bir ürünle, hizmette veya çerçevede kullanılabilir. LUDA çeşitli popüler programlama dilleri için istemci kitaplıkları (SDK 'lar) sağlar. Sunulan [Geliştirici kaynakları](developer-reference-resource.md) hakkında daha fazla bilgi edinin.

LUIS'i hızlı ve kolay bir şekilde botla birlikte kullanmanızı sağlayacak uygulamalar:
* [LUSıS CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) NPM paketi, tek başına komut satırı aracı olarak veya içeri aktarma olarak ile yazma ve tahmin sağlar.
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen, dışarı aktarılan bir LUIS modelinden ayrıntılı C# ve typescript kaynak kodu yazmak için kullanılan bir araçtır.
* [Gönderme](https://aka.ms/dispatch-tool), çeşitli LUIS ve Soru-Cevap Oluşturma uygulamalarının gönderme modelini kullanan bir üst uygulamadan kullanılmasını sağlar.
* [Luaşağı](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) Luaşağı, bot için dil modellerinin yönetilmesine yardımcı olan bir komut satırı aracıdır.

## <a name="integrate-with-a-bot"></a>Bir bot ile tümleştirme

[Microsoft bot Framework](https://dev.botframework.com/) Ile [Azure bot hizmetini](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0) kullanarak bir sohbet bot oluşturun ve dağıtın. Grafik arabirim Aracı, [besteci](https://docs.microsoft.com/composer/)veya üst bot senaryoları için tasarlanan [çalışan bot örnekleri](https://github.com/microsoft/BotBuilder-Samples) ile tasarım ve geliştirme.

## <a name="integrate-with-other-cognitive-services"></a>Diğer bilişsel hizmetlerle tümleştirin

LUIS ile kullanılan diğer Bilişsel Hizmetler:
* [Soru-Cevap Oluşturma][qnamaker], farklı metin türlerinin birleştirilerek soru ve yanıt bilgi bankası oluşturulmasını sağlar.
* [Konuşma hizmeti](../Speech-Service/overview.md), sözlü dil isteklerini metne dönüştürür.

LUO, var olan LUSıS kaynaklarınızın bir parçası olarak Metin Analizi işlevsellik sağlar. Bu işlevsellik, önceden oluşturulmuş keyPhrase varlığıyla yaklaşım [analizini](luis-how-to-publish-app.md#configuring-publish-settings) ve [anahtar tümceciği ayıklamasını](luis-reference-prebuilt-keyphrase.md) içerir.

## <a name="learn-with-the-quickstarts"></a>Hızlı başlangıçlarla öğrenin

[Portalı](get-started-portal-build-app.md) ve [SDK istemci kitaplıklarını](quickstart-sdk.md)kullanarak UYGULAMALı hızlı başlangıçlarla halsıs hakkında bilgi edinin.


## <a name="next-steps"></a>Sonraki adımlar

* Hizmet ve [belge yenilikleri](whats-new.md)
* [Hedefleri](luis-concept-intent.md) ve [varlıkları](luis-concept-entity-types.md)kullanarak [Uygulamanızı planlayın](luis-how-plan-your-app.md) .
* [Tahmin uç noktasını sorgulayın](luis-get-started-get-intent-from-browser.md).
* LUSıS için [Geliştirici kaynakları](developer-reference-resource.md) .

[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://go.microsoft.com/fwlink/?linkid=2092087
[endpoint-apis]: https://go.microsoft.com/fwlink/?linkid=2092356
[qnamaker]: https://qnamaker.ai/
