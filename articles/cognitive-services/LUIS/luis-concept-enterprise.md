---
title: Kurumsal kavramlar - LUIS
titleSuffix: Azure Cognitive Services
description: Büyük LUIS uygulamaları veya LUIS ve QnA Maker gibi birden fazla uygulama için tasarım kavramlarını birlikte anlayın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: efef3faf3cc4ff04235254f0ff6538d92a831196
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79221067"
---
# <a name="enterprise-strategies-for-a-luis-app"></a>LUIS uygulaması için kurumsal stratejiler
Kurumsal uygulamanız için bu tasarım stratejilerini gözden geçirin.

## <a name="when-you-expect-luis-requests-beyond-the-quota"></a>Kotanın ötesinde LUIS isteklerini beklediğiniz zaman

LUIS, Azure kaynağının fiyatlandırma katmanına bağlı olarak aylık kotanın yanı sıra ikinci kotaya da sahiptir. 

LUIS uygulama istek oranınız izin verilen [kota oranını](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/)aşarsa, şunları yapabilirsiniz:

* [Aynı uygulama tanımıyla](#use-multiple-apps-with-same-app-definition)yükü daha fazla LUIS uygulamasına yayın. Buna isteğe bağlı olarak bir [kapsayıcıdan](luis-container-howto.md)LUIS çalıştırıl. 
* Uygulamanın birden çok anahtarı oluşturun ve [atayın.](#assign-multiple-luis-keys-to-same-app) 

### <a name="use-multiple-apps-with-same-app-definition"></a>Aynı uygulama tanımına sahip birden fazla uygulama kullanma
Orijinal LUIS uygulamasını dışa aktarın ve uygulamayı ayrı uygulamalara aktarın. Her uygulamanın kendi uygulama kimliği vardır. Tüm uygulamalarda aynı anahtarı kullanmak yerine yayımladığınızda, her uygulama için ayrı bir anahtar oluşturun. Tek bir uygulamanın bunalmış olmaması için tüm uygulamalardaki yükü dengeleyin. Kullanımı izlemek için [Uygulama Öngörüleri](luis-tutorial-bot-csharp-appinsights.md) ekleyin. 

Tüm uygulamalar arasında aynı üst niyeti elde etmek için, birinci ve ikinci niyet arasındaki niyet tahmininin LUIS'in karıştırılmayacak kadar geniş olduğundan emin olun ve bu da uygulamalar arasında küçük farklılıklar için farklı sonuçlar verir. 

Bu kardeş uygulamaları eğitirken, [tüm verilerle eğitim yaptığınızdan](luis-how-to-train.md#train-with-all-data)emin olun.

Ana olarak tek bir uygulama belirleyin. Gözden geçirilmesi önerilen tüm ifadeler ana uygulamaya eklenmeli ve diğer tüm uygulamalara geri taşınmalıdır. Bu, ya uygulamanın tam bir dışa aktarMası dır ya da ana kaynaktan çocuklara etiketlenmiş sözcükleri yükler. Yükleme ya [LUIS](luis-reference-regions.md) web sitesinden veya tek bir [söyleyiş](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08) için veya bir [toplu iş](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09)için api yazma yapılabilir. 

Her iki haftada bir, etkin öğrenme için [uç nokta söyleyişleri](luis-how-to-review-endpoint-utterances.md) gibi periyodik bir inceleme planlayın, ardından yeniden eğitin ve yeniden yayımlayın. 

### <a name="assign-multiple-luis-keys-to-same-app"></a>Aynı uygulamaya birden fazla LUIS anahtarı atama
LUIS uygulamanız, tek anahtarınızın kotasının izin verdiğinden daha fazla uç nokta isabeti alıyorsa, LUIS uygulamasına daha fazla anahtar oluşturun ve atayın. Bitiş noktası anahtarlarında uç nokta sorgularını yönetmek için bir trafik yöneticisi veya yük bakiyesi oluşturun. 

## <a name="when-your-monolithic-app-returns-wrong-intent"></a>Yekpare uygulamanız yanlış niyet döndürdüğünde
Uygulamanız çok çeşitli kullanıcı söyleyişlerini tahmin etmek içinyapılmışsa, [gönderim modelini](#dispatch-tool-and-model)uygulamayı düşünün. Yekpare bir uygulamayı birbirinden ayırmak, LUIS'in, ana uygulama ile alt uygulamalar arasındaki niyetler arasında kafa karıştırmak yerine, algılamayı kasıtlı lar arasında başarılı bir şekilde odaklamasına olanak tanır. 

Her iki haftada bir olduğu gibi, her iki haftada bir gibi [uç nokta sözcüklerinin](luis-how-to-review-endpoint-utterances.md) periyodik bir incelemesini planlayın, ardından yeniden eğitin ve yeniden yayımlayın. 

## <a name="when-you-need-to-have-more-than-500-intents"></a>500'den fazla niyete sahip olmanız gerektiğinde
500'den fazla amacı olan bir ofis asistanı geliştirdiğinizi varsayalım. 200 niyet zamanlama toplantılarıyla ilgiliyse, 200'ü anımsatıcılarla ilgilidir, 200'ü iş arkadaşları hakkında bilgi almakla ilgilidir ve 200'ü e-posta göndermek için, her grubun tek bir uygulamada olması için niyetleri gruplandırmak için, ardından her amacı içeren üst düzey bir uygulama oluşturun. Üst düzey uygulamayı oluşturmak için [gönderme modelini](#dispatch-tool-and-model) kullanın. Daha sonra [gönderme modelinin öğreticisinde](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&branch=master&tabs=cs)gösterildiği gibi basamaklı aramayı kullanmak için botunuzu değiştirin. 

## <a name="when-you-need-to-combine-several-luis-and-qna-maker-apps"></a>Birkaç LUIS ve QnA maker uygulamalarını birleştirmeniz gerektiğinde
Bir bota yanıt vermesi gereken birkaç LUIS ve QnA üreticisi uygulamanız varsa, üst düzey uygulamayı oluşturmak için [gönderme modelini](#dispatch-tool-and-model) kullanın.  Daha sonra [gönderme modelinin öğreticisinde](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&branch=master&tabs=cs)gösterildiği gibi basamaklı aramayı kullanmak için botunuzu değiştirin. 

## <a name="dispatch-tool-and-model"></a>Gönderme aracı ve modeli
Birden fazla LUIS ve/veya QnA Maker uygulamasını bir üst LUIS uygulamasında birleştirmek için [BotBuilder araçlarında](https://github.com/Microsoft/botbuilder-tools) bulunan [Dispatch][dispatch-tool] komut satırı aracını kullanın. Bu yaklaşım, ayrı uygulamalarda tüm konular ve farklı alt konu etki alanlarını içeren bir üst etki alanına sahip olmak sağlar. 

![Sevk mimarisinin kavramsal görüntüsü](./media/luis-concept-enterprise/dispatch-architecture.png)

Üst etki alanı, uygulamalar listesinde `Dispatch` adı geçen bir sürümle LUIS'te belirtilir. 

Sohbet botu söyleyişi alır, ardından tahmin için üst LUIS uygulamasına gönderir. Üst uygulamadan öngörülen en yüksek amaç, bir sonraki sırada hangi LUIS alt uygulamasının çağrıldığını belirler. Sohbet botu daha özel bir tahmin için çocuk uygulamasına söyleyiş gönderir.

Bu arama hiyerarşisinin Bot Builder v4 [dispatcher-application-tutorial'den](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&branch=master&tabs=cs)nasıl yapıldığını anlayın.  

### <a name="intent-limits-in-dispatch-model"></a>Gönderme modelinde amaç sınırları
Bir sevk uygulaması, maksimum olarak 500 niyete eşdeğer 500 sevk kaynağına sahiptir. 

## <a name="more-information"></a>Daha fazla bilgi

* [Bot çerçeve SDK](https://github.com/Microsoft/botframework)
* [Gönderme modeli öğretici](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-dispatch?view=azure-bot-service-4.0&branch=master&tabs=cs)
* [Sevk CLI](https://github.com/Microsoft/botbuilder-tools)
* Sevkiyat modeli bot örneği - [.NET](https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/csharp_dotnetcore/14.nlp-with-dispatch), [Düğüm.js](https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/javascript_nodejs/14.nlp-with-dispatch)

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu iş bir toplu iş test](luis-how-to-batch-test.md) etmeyi öğrenin

[dispatcher-application-tutorial]: https://aka.ms/bot-dispatch
[dispatch-tool]: https://aka.ms/dispatch-tool
