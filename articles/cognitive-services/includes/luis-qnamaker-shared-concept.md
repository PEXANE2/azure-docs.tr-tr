---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 04/16/2020
ms.author: diberry
ms.openlocfilehash: 02610e647e2138cbf52f86c22107feec2d61273b
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604950"
---
Bilişsel Hizmetler iki doğal dil işleme hizmetleri sağlar, [Dil Anlayışı](../luis/what-is-luis.md) ve [QnA Maker](../qnamaker/overview/overview.md), her biri farklı bir amaç ile. Her hizmeti ne zaman kullanacağımı ve birbirlerine nasıl iltifat ettiklerini anlayın.

Doğal dil işleme (NLP), sohbet botu gibi istemci uygulamanızın doğal dil kullanarak kullanıcılarınızla birlikte çalışmasını sağlar. Kullanıcı bir cümle veya tümcecik girer. Kullanıcımetninde zayıf dilbilgisi, yazım ve noktalama işaretleri olabilir. Bilişsel Hizmet zaten kullanıcı cümlesi ile çalışabilir, sohbet bot kullanıcıya yardımcı olmak için gereken bilgileri döndürerek.

## <a name="cognitive-services-with-nlp"></a>NLP ile Bilişsel Hizmetler

Dil Anlama (LUIS) ve QnA Maker NLP sağlar. İstemci başvurusu doğal dil metni gönderir. Hizmet metni alır, işler ve bir sonuç döndürür.

## <a name="when-to-use-each-service"></a>Her hizmetin ne zaman kullanılacağı

Dil Anlama (LUIS) ve QnA Maker farklı sorunları çözmek. LUIS, kullanıcının metninin (söyleyiş olarak bilinir) amacını belirlerken, QnA Maker kullanıcının metninin (sorgu olarak bilinir) yanıtını belirler.

Doğru hizmeti seçmek için, istemci uygulamasından gelen kullanıcı metnini ve istemci uygulamasının Bilişsel Hizmetten hangi bilgileri alması gerektiğini anlamanız gerekir.

Sohbet robotunuz metni `How do I get to the Human Resources building on the Seattle North campus?`alıyorsa, her hizmetin metinle nasıl çalıştığını anlamak için aşağıdaki grafiği kullanın.

|Hizmet|İstemci uygulaması belirler|
|--|--|
|LUIS|**Kullanıcının** metin niyetini belirler - hizmet sorunun yanıtını döndürmez. Örneğin, bu metin `FindLocation` amaç eşleşen olarak sınıflandırılır.<br>|
|Soru-Cevap Oluşturucu|**Sorunun yanıtını** özel bir bilgi tabanından döndürür. Örneğin, bu metin statik metin yanıtı ile bir `Get on the #9 bus and get off at Franklin street`soru olarak belirlenir.|
|||

## <a name="when-do-you-use-luis"></a>LUIS'i ne zaman kullanıyorsun?

Sohbet botundaki bir sürecin parçası olarak söyleyişin niyetini bilmeniz gerektiğinde LUIS'i kullanın. Örnek metinle devam `How do I get to the Human Resources building on the Seattle North campus?`ederek, kullanıcının niyetinin bir konum bulmak olduğunu bildiğinizde, yanıtı almak için bir taşıma sunucusu gibi başka bir hizmete söyleyiş (varlıklarla birlikte çıkarılmış) ile ilgili ayrıntıları aktarabilirsiniz.

Niyeti belirlemek için LUIS ve QnA Maker'ı birleştirmeniz gerekmez.

Sohbet botu, metni niyetlere ve varlıklara (LUIS kullanarak) göre işlemesi ve belirli statik metin yanıtını (QnA Maker'ı kullanarak) bulması gerekiyorsa, bu söziçin iki hizmeti birleştirebilirsiniz.

## <a name="when-do-you-use-qna-maker"></a>QnA Maker'ı ne zaman kullanıyorsunuz?

Yanıtlardan oluşan statik bir bilgi tabanınız olduğunda QnA Maker'ı kullanın. Bu bilgi tabanı, PDF'ler ve URL'ler gibi belgelerle oluşturduğunuz ihtiyaçlarınıza özeldir.

Örnek söyleyiş ile devam,, `How do I get to the Human Resources building on the Seattle North campus?`yayınlanan QnA Maker hizmetine bir sorgu olarak metin göndermek ve en iyi cevabı almak.

Sorunun cevabını belirlemek için LUIS ve QnA Maker'ı birleştirmeniz gerekmez.

Sohbet botu niyetve varlıklara (LUIS kullanarak) dayalı metni işlemek ve (QnA Maker kullanarak) yanı sıra cevap bulmak gerekiyorsa, bu söyleyiş için iki hizmet birleştirebilirsiniz.

## <a name="use-both-services-when-your-knowledge-base-is-incomplete"></a>Bilgi tabanınız eksikolduğunda her iki hizmeti de kullanın

QnA Maker bilgi tabanınızı oluşturuyorsanız ancak konu alanının değiştiğini biliyorsanız (zamanında bilgi gibi), LUIS ve QnA Maker hizmetlerini birleştirebilirsiniz. Bu, bilgi tabanınızdaki bilgileri kullanmanıza ve kullanıcının niyetini belirlemek için LUIS'i kullanmanıza olanak tanır. İstemci uygulaması nın amacı olduğunda, ilgili bilgileri başka bir kaynaktan isteyebilir.

Müşteri uygulamanız puanları için hem LUIS hem de QnA Maker yanıtlarını izlemeniz gerekir. QnA Maker'ın puanı bazı rasgele eşiğin altındaysa, bilgileri bir üçüncü taraf hizmetine aktarmak için LUIS'ten döndürülen niyet ve varlık bilgilerini kullanın.

Örnek metinle devam `How do I get to the Human Resources building on the Seattle North campus?`ederken, QnA Maker'ın düşük bir güven puanı döndürür olduğunu varsayalım. Bu bilgileri başka bir `FindLocation` yanıt için bir eşleme `Human Resources building` `Seattle North campus`veya arama hizmetine göndermek için LUIS'ten döndürülen niyeti ve bu bilgileri ayıklanmış varlıklar gibi kullanın.

Doğrulama için bu üçüncü taraf yanıtı kullanıcıya sunabilirsiniz. Kullanıcının onayını aldıktan sonra, bilginizi artırmak için bilgileri eklemek için QnA Maker'a geri dönebilirsiniz.

## <a name="use-both-services-when-your-chat-bot-needs-more-information"></a>Sohbet botunuz daha fazla bilgiye ihtiyaç duyduğunda her iki hizmeti de kullanın

Sohbet robotunuzun her iki hizmetin sağladığından daha fazla bilgiye ihtiyacı varsa, bir karar ağacı üzerinden devam etmek için, istemci uygulamasında her iki hizmeti de kullanın ve her iki yanıtı da işleyin.

Her iki hizmetle de çalışmak için bir işlem oluşturmaya yardımcı olmak için Bot çerçevesi **[Gönderme CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Dispatch)** aracını kullanın. Bu araç, luis ve QnA Maker arasında alt uygulamalar olarak gönderilen en iyi niyet ler için bir LUIS uygulaması oluşturur.

Bu tür sohbet botlarını uygulamak için [C#](https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/csharp_dotnetcore/14.nlp-with-dispatch) veya [Node.js'de](https://github.com/microsoft/BotBuilder-Samples/tree/master/samples/javascript_nodejs/14.nlp-with-dispatch)Bot oluşturucu örneğini, **NLP'yi sevkiyatla**birlikte kullanın.

## <a name="best-practices"></a>En iyi uygulamalar

Her hizmet için en iyi uygulamaları uygulayın:

* [LUIS](../luis/luis-concept-best-practices.md) en iyi uygulamaları
* [QnA Maker](../qnamaker/concepts/best-practices.md) en iyi uygulamaları

## <a name="see-also"></a>Ayrıca bkz.

* [Language Understanding (LUIS)](../luis/what-is-luis.md)
* [QnA Üreticisi](../qnamaker/overview/overview.md)
* [Sevk CLI](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Dispatch)
* [Bot çerçeve örnekleri](https://github.com/Microsoft/BotBuilder-Samples)
* [Azure bot hizmeti](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0)
* [Azure bot emülatörü](https://github.com/Microsoft/BotFramework-Emulator)
* [Bot çerçeve web sohbet](https://github.com/microsoft/BotFramework-WebChat)