---
title: Language Understanding (LUIS) nedir?
description: Language Understanding (LUIS), genel anlamı tahmin etmek ve ilgili, ayrıntılı bilgileri çekme amacıyla kullanıcının konuşmasına, doğal dil metnine özel makine öğrenimi zekası uygulayan bulut tabanlı API hizmetidir.
ms.topic: overview
ms.date: 02/23/2020
ms.openlocfilehash: 98fb936422f8e23e728efea19fa2cd75d90fac57
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80053346"
---
# <a name="what-is-language-understanding-luis"></a>Language Understanding (LUIS) nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Language Understanding (LUIS), genel anlamı tahmin etmek ve ilgili, ayrıntılı bilgileri çekme amacıyla kullanıcının konuşmasına, doğal dil metnine özel makine öğrenimi zekası uygulayan bulut tabanlı API hizmetidir.

LUIS için istemci uygulaması, bir görevi tamamlamak için kullanıcıyla doğal dil kullanarak iletişim kuran konuşma uygulamasıdır. İstemci uygulamalarına örnek olarak sosyal medya uygulamaları, sohbet botları ve konuşma özellikli masaüstü uygulamaları verilebilir.

![Bilişsel Hizmetler Dil Anlayışı (LUIS) ile çalışan 3 istemci uygulamasının kavramsal görüntüsü](./media/luis-overview/luis-entry-point.png "Bilişsel Hizmetler Dil Anlayışı (LUIS) ile çalışan 3 istemci uygulamasının kavramsal görüntüsü")

## <a name="use-luis-in-a-chat-bot"></a>Sohbet botunda LUIS kullanımı

<a name="Accessing-LUIS"></a>

LUIS uygulaması yayımlandıktan sonra istemci uygulaması konuşmaları (metni) LUIS doğal dil işleme uç noktası [API'sine][endpoint-apis] gönderir ve sonuçları JSON yanıtı olarak alır. Sık kullanılan LUIS istemci uygulamalarından biri, sohbet botudur.


![Doğal dil anlayışı (NLP) ile kullanıcı metnini tahmin etmek için Chat bot ile çalışan LUIS kavramsal görüntüleri](./media/luis-overview/LUIS-chat-bot-request-response.svg "Doğal dil anlayışı ile kullanıcı metnini tahmin etmek için Chat bot ile çalışan LUIS kavramsal görüntüleri (NLP")

|Adım|Eylem|
|:--|:--|
|1|İstemci uygulaması, kullanıcının "İK temsilcimi aramak istiyorum." şeklindeki _konuşmasını_ (kendi kullandıkları kelimelerle) bir HTTP isteği olarak LUIS uç noktasına gönderir.|
|2|LUIS, uygulamanıza zeka katmak için özel dil modellerinizi oluşturmanıza olanak tanır. Makine öğrenilen dil modelleri kullanıcının yapılandırılmamış giriş metnini alır ve json biçimli `HRContact`bir yanıtı üst niyetle döndürür. JSON uç nokta yanıtı minimumda sorgu konuşmasını ve en yüksek puanlı amacı içerir. _Ayrıca, Kişi Türü_ varlığı gibi verileri ayıklayabilir.|
|3|İstemci uygulaması, JSON yanıtını kullanarak kullanıcının isteklerini gerçekleştirmeyle ilgili kararları verir. Bu kararlar, bot çerçeve kodunda karar ağacı nı ve diğer hizmetlere yapılan çağrıları içerebilir. |

LUIS uygulaması, istemci uygulamasının akıllı seçimler yapabilmesi için gerekli bilgileri sunar. LUIS bu seçenekleri sağlamaz.

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Doğal dil işleme

LUIS uygulamanız etki alanına özgü bir doğal dil modeli içerir. LUIS uygulamasını önceden oluşturulmuş bir etki alanı modeliyle başlatabilir, kendi modelinizi oluşturabilir veya önceden oluşturulmuş etki alanının belirli bölümlerini kendi özel bilgilerinizle karıştırabilirsiniz.

* **Önceden oluşturulmuş model**: LUIS amaç, konuşma ve önceden oluşturulmuş varlık içeren birçok önceden oluşturulmuş etki alanı modeline sahiptir. Önceden oluşturulmuş modelin amaçlarını ve konuşmalarını kullanmak zorunda kalmadan önceden oluşturulmuş varlıkları kullanabilirsiniz. [Önceden oluşturulmuş etki alanı modelleri](luis-how-to-use-prebuilt-domains.md), tasarımın tamamını içerir ve LUIS hizmetini kullanmaya başlamak için ideal bir yoldur.

* **Özel model** LUIS, niyetler ve varlıklar da dahil olmak üzere kendi özel modellerinizi belirlemeniz için çeşitli yollar sunar. Varlıklar, makinede öğrenilen varlıklar, belirli veya gerçek varlıklar ve makinede öğrenilen ve gerçek bir birleşimi içerir.

## <a name="build-the-luis-model"></a>LUIS modelini derleme
Yazma [authoring](https://go.microsoft.com/fwlink/?linkid=2092087) API'leri veya LUIS [portalı](https://www.luis.ai)ile modeli oluşturun.

LUIS modeli, **[amaçlar](luis-concept-intent.md)** olarak adlandırılan kullanıcı amacı kategorileriyle başlar. Her amaç için kullanıcı **[konuşmaları](luis-concept-utterance.md)** örneklerine ihtiyaç duyulur. Her söyleyiş ayıklanması gereken verileri sağlayabilir.

|Örnek kullanıcı konuşması|Amaç|Çıkarılan veriler|
|-----------|-----------|-----------|
|`Book a flight to __Seattle__?`|BookFlight|Seattle|
|`When does your store __open__?`|StoreHoursAndLocation|açık|
|`Schedule a meeting at __1pm__ with __Bob__ in Distribution`|ScheduleMeeting|13, Bob|

## <a name="query-prediction-endpoint"></a>Sorgu tahmin uç noktası

Uygulamanız eğitildikten ve bitiş noktasına kadar yayınlandıktan sonra, istemci uygulaması tahmin [bitiş noktası](https://go.microsoft.com/fwlink/?linkid=2092356) API'sine sözcük gönderir. API, uygulamayı analiz için söyleyiş için uygular ve tahmin sonuçlarıyla JSON formatında yanıt verir.

JSON uç nokta yanıtı minimumda sorgu konuşmasını ve en yüksek puanlı amacı içerir. Ayrıca, aşağıdaki **İletişim Türü** varlığı ve genel duyarlılık gibi verileri ayıklayabilir.

```JSON
{
    "query": "I want to call my HR rep",
    "prediction": {
        "normalizedQuery": "i want to call my hr rep",
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
            "label": "negative",
            "score": 0.103343368
        }
    }
}
```

## <a name="improve-model-prediction"></a>Model tahminini geliştirme

LUIS uygulamanız yayınlandıktan ve gerçek kullanıcı söyleyişlerini aldıktan sonra, LUIS tahmin doğruluğunu artırmak için uç nokta söyleyişlerinin etkin bir şekilde [öğrenilmesini](luis-concept-review-endpoint-utterances.md) sağlar.

<a name="using-luis"></a>

## <a name="development-lifecycle"></a>Geliştirme yaşam döngüsü
LUIS, tam [gelişim yaşam döngüsüne](luis-concept-app-iteration.md)entegre etmek için diğer LUIS yazarlarıyla araçlar, sürüm ler ve işbirliği sağlar.

## <a name="implementing-luis"></a>LUIS uygulama
Dil Anlama (LUIS), bir REST API olarak, herhangi bir ürün, hizmet veya çerçeve ile bir HTTP isteği ile kullanılabilir. Aşağıdaki liste, LUIS ile birlikte en çok kullanılan Microsoft ürünlerini ve hizmetlerini göstermektedir.

En çok kullanılan LUIS istemci uygulamaları şunlardır:
* [Web uygulaması botu](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0), kullanıcıyla metin girişi aracılığıyla konuşmak için hızlıca LUIS destekli bir sohbet botu oluşturur. Tam bir bot deneyimi için [Bot Framework][bot-framework] sürüm [4.x](https://github.com/Microsoft/botbuilder-dotnet) kullanır.

LUIS'i hızlı ve kolay bir şekilde botla birlikte kullanmanızı sağlayacak uygulamalar:
* [LUIS CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS) NPM paketi, tek başına komut satırı aracı veya alma olarak yazma ve tahmin sağlar.
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen, dışarı aktarılan bir LUIS modelinden ayrıntılı C# ve typescript kaynak kodu yazmak için kullanılan bir araçtır.
* [Gönderme](https://aka.ms/dispatch-tool), çeşitli LUIS ve Soru-Cevap Oluşturma uygulamalarının gönderme modelini kullanan bir üst uygulamadan kullanılmasını sağlar.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown, botunuzun dil modellerini yönetmenize yardımcı olan bir komut satırı aracıdır.
* [Bot çerçevesi - Besteci](https://github.com/microsoft/BotFramework-Composer) - Geliştiriciler ve çok disiplinli ekipler için entegre bir geliştirme aracı Microsoft Bot Framework ile botlar ve konuşma deneyimleri oluşturmak için

LUIS ile kullanılan diğer Bilişsel Hizmetler:
* [Soru-Cevap Oluşturma][qnamaker], farklı metin türlerinin birleştirilerek soru ve yanıt bilgi bankası oluşturulmasını sağlar.
* [Konuşma hizmeti](../Speech-Service/overview.md), sözlü dil isteklerini metne dönüştürür.
* [Konuşma öğrenici](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview), LUIS ile daha hızlı bir şekilde sohbet botları oluşturmanızı sağlar.

LUIS kullanan numuneler:
* [Konuşma AI](https://github.com/Microsoft/AI) GitHub deposu.
* [Bot çerçevesi - Bot örnekleri](https://github.com/microsoft/BotBuilder-Samples)

## <a name="next-steps"></a>Sonraki adımlar

* [Yenilikler](whats-new.md)
* [Önceden oluşturulmuş](luis-get-started-create-app.md) veya [özel](luis-quickstart-intents-only.md) etki alanıyla yeni bir LUIS uygulaması yazma.
* Genel IoT uygulamasının [tahmin uç noktasını sorgulama](luis-get-started-get-intent-from-browser.md).
* LUIS için [geliştirici kaynakları.](developer-reference-resource.md)

[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://go.microsoft.com/fwlink/?linkid=2092087
[endpoint-apis]: https://go.microsoft.com/fwlink/?linkid=2092356
[qnamaker]: https://qnamaker.ai/
