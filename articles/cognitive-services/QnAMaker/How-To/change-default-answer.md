---
title: Varsayılan yanıtı al-Soru-Cevap Oluşturma
description: Söz konusu soruda eşleşme olmadığında varsayılan yanıt döndürülür. Varsayılan yanıtı standart varsayılan cevabı değiştirmek isteyebilirsiniz.
ms.topic: how-to
ms.date: 07/13/2020
ms.openlocfilehash: d37e63d84be58e6ccd2f1e23a1344961d39ffa01
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87054165"
---
# <a name="change-default-answer-for-a-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağı için varsayılan yanıtı değiştirme

Bir bilgi tabanı için varsayılan yanıt, bir yanıt bulunamadığında döndürülür. [Azure bot hizmeti](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs#calling-qna-maker-from-your-bot)gibi bir istemci uygulaması kullanıyorsanız, puan eşiğini karşılayan bir yanıt olmadığını belirten ayrı bir varsayılan yanıta da sahip olabilir.

## <a name="types-of-default-answer"></a>Varsayılan yanıt türleri

Bilgi tabanınız için iki tür varsayılan yanıt vardır. Her birinin bir tahmin sorgusundan nasıl ve ne zaman döndürüldüğünü anlamak önemlidir:


|Soru türü|Yanıtın açıklaması|
|--|--|
|Yanıt saptanmadığı zaman KB yanıtı|`No good match found in KB.`- [Generateanswer API 'si](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer) söz konusu soruya eşleşen bir yanıt bulmazsa, `DefaultAnswer` App Service ayarı döndürülür. Aynı Soru-Cevap Oluşturma kaynaktaki tüm bilgi tabanları aynı varsayılan yanıt metnini paylaşır.<br>Ayarları, App Service aracılığıyla veya ayarı [almak](https://docs.microsoft.com/rest/api/appservice/webapps/listapplicationsettings) ya da [güncelleştirmek](https://docs.microsoft.com/rest/api/appservice/webapps/updateapplicationsettings) için REST API 'lerle birlikte Azure Portal yönetebilirsiniz.|
|İzleme istemi yönerge metni|Bir konuşma akışında bir izleme istemi kullanırken, kullanıcının izleme istemlerinde seçmesini istediğiniz için QnA çiftiyle bir yanıta gerek duymayabilir. Bu durumda, her bir tahmine yönelik her bir tahmine göre döndürülen varsayılan yanıt metnini ayarlayarak belirli bir metni ayarlayın. Metin, izleme istemleri seçimine ilişkin yönerge metni olarak görüntülenmek üzere tasarlanmıştır. Bu varsayılan yanıt metni için bir örnek vardır `Please select from the following choices` . Bu yapılandırma, bu belgenin sonraki birkaç bölümünde açıklanmaktadır. , REST API kullanarak Bilgi Bankası tanımının bir parçası olarak da `defaultAnswerUsedForExtraction` ayarlanabilir [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create).|

### <a name="client-application-integration"></a>İstemci uygulama tümleştirmesi

**Azure bot hizmeti**olan bir bot gibi bir istemci uygulaması için aşağıdaki senaryolardan birini seçebilirsiniz:

* Bilgi Bankası 'nın ayarını kullanın
* Bir yanıtın ne zaman döndürüleceğini, ancak puan eşiğini karşılamadığında ayırt etmek için istemci uygulamada farklı bir metin kullanın. Bu metin, kodda depolanan statik metin veya istemci uygulamasının ayarlar listesinde depolanabilir.

## <a name="set-follow-up-prompts-default-answer-when-you-create-knowledge-base"></a>Bilgi Bankası oluştururken izleme isteminin varsayılan yanıtını ayarla

Yeni bir Bilgi Bankası oluşturduğunuzda varsayılan yanıt metni ayarlardan biridir. Oluşturma işlemi sırasında ayarlamayı tercih ederseniz, daha sonra aşağıdaki yordamı kullanarak değiştirebilirsiniz.

## <a name="change-follow-up-prompts-default-answer-in-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında izleme isteminin varsayılan yanıtını değiştirme

Bilgi Bankası varsayılan yanıtı, Soru-Cevap Oluşturma hizmetinden yanıt döndürülmediğinde döndürülür.

1. [Soru-cevap oluşturma portalında](https://www.qnamaker.ai/) oturum açın ve listeden bilgi bankasını seçin.
1. Gezinti çubuğundan **Ayarlar** ' ı seçin.
1. **Bilgi bankasını Yönet** bölümünde **Varsayılan Yanıt metninin** değerini değiştirin.

    :::image type="content" source="../media/qnamaker-concepts-confidencescore/change-default-answer.png" alt-text="Varsayılan yanıt metin kutusu vurgulanmış şekilde Soru-Cevap Oluşturma Portal, Ayarlar sayfasının ekran görüntüsü.":::

1. Değişikliği kaydetmek için **Kaydet ve eğitme '** yi seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Bilgi bankası oluşturma](../How-to/manage-knowledge-bases.md)
