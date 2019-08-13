---
title: Bilgi Bankası - soru-cevap Oluşturucu yaşam döngüsü
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 97a4673be2a611149806855e792c5bf1f7a0942a
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955164"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Soru-cevap Oluşturucu, Bilgi Bankası yaşam döngüsü
Soru-cevap Oluşturucu, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir. 

![Yazma döngüsü](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Soru-cevap Oluşturucu Bilgi Bankası oluşturma
Soru-cevap Oluşturucu Bilgi Bankası (KB) uç noktası, bir kullanıcı sorgu KB içeriğine göre en iyi eşleşme Yanıtla sağlar. Bilgi Bankası oluşturma, bir içerik deposu soru, yanıt ve ilişkili meta verileri ayarlama için tek seferlik bir işlemdir. Bilgi Bankası, SSS sayfaları, ürün kılavuzlarını veya yapılandırılmış Q A çiftleri gibi önceden varolan içerikte tarafından oluşturulabilir. Bilgi edinmek için nasıl [Bilgi Bankası oluşturma](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Test ve Bilgi Bankası güncelleştirme

Bilgi Bankası bilgi bankanızı düzenleyerek veya otomatik ayıklama aracılığıyla içerikle doldurulur sonra test etmek için hazırdır. Etkileşimli test, **Test** paneli aracılığıyla soru-cevap oluşturma portalında, ortak kullanıcı sorguları girerek ve yanıtların doğru yanıt ve yeterli Güvenirlik puanı ile döndürülen yanıtları doğrulayarak yapılabilir. 

* **Düşük güvenirlik puanlarını onarmak için**: alternatif sorular ekleyin. 
* **Sorgu yanlış bir şekilde [varsayılan yanıtı](confidence-score.md#change-default-answer)döndürdüğünde**: doğru soruya yeni yanıtlar ekleyin. 

Sonuçlardan memnun olana kadar test güncelleştirme sıkı Bu döngü devam eder. Bilgi edinmek için nasıl [bilgi bankanızı test](../How-To/test-knowledge-base.md).

Büyük KBS için, [generateanswer API 'si](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) ve `isTest` yayımlanan bilgi tabanı yerine `test` bilgi bankasını sorgulayan Body özelliği ile otomatik test kullanın. 

```json
{
  "question": "example question",
  "top": 3,
  "userId": "Default",
  "isTest": true
}
```

## <a name="publish-the-knowledge-base"></a>Bilgi bankasını yayımlama
İşiniz bittiğinde Bilgi Bankası sınanması, bunu yayımlayabilirsiniz. En son sürümünü test edilmiş Bilgi Bankası'na ayrılmış bir Azure Search dizinini temsil eden bildirim yayımlama **yayımlanan** Bilgi Bankası. Ayrıca uygulamanızda veya sohbet botunuzda çağrılabilecek bir uç nokta da oluşturulur.

Bu şekilde, bir üretim uygulamasında Canlı olabilecek yayımlanmış sürümüne Bilgi Bankası test sürümüne yapılan her değişikliği etkilemez.

Bu bilgi bankalarından herbiri ayrı olarak test etmek için hedeflenebilir. API 'leri kullanarak, Bilgi Bankası 'nın test sürümünü generateanswer çağrısında Body özelliği ile `isTest` hedefleyebilirsiniz.

Bilgi edinmek için nasıl [, Bilgi Bankası yayımlama](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).

## <a name="monitor-usage"></a>Kullanımı izleme
Sohbet günlükleri hizmetinizin oturum açabilmesi için Application Insights'ı etkinleştirmek ihtiyacınız zaman, [soru-cevap Oluşturucu hizmetinizi oluşturun](../How-To/set-up-qnamaker-service-azure.md).

Hizmet kullanımınızın çeşitli analiz elde edebilirsiniz. Almak için application ınsights'ı kullanma hakkında daha fazla bilgi edinin [soru-cevap Oluşturucu hizmetiniz için analytics](../How-To/get-analytics-knowledge-base.md).

Bağlı olarak, analytics'ten öğrenin, uygun hale [bilgi bankanızı güncelleştirmeleri](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Güvenilirlik puanı](./confidence-score.md)

## <a name="see-also"></a>Ayrıca bkz. 

[Bilgi Bankası](./knowledge-base.md)
[soru-cevap Oluşturucu genel bakış](../Overview/overview.md)
