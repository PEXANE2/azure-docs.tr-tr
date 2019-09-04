---
title: Yinelemeli uygulama tasarımı-LUSıS
titleSuffix: Azure Cognitive Services
description: LUIS, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: diberry
ms.openlocfilehash: 4356d9e1cd3d6f1a924603f7405d612814d35859
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70256926"
---
# <a name="authoring-cycle-for-your-luis-app"></a>LUSıS uygulamanız için yazma çevrimi
LUIS, en iyi modeli değişiklikleri, utterance örnekler, yayımlama ve veri toplamayı yinelemeli bir döngüyle uç nokta sorgularından öğrenir. 

![Yazma döngüsü](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>LUIS modeline oluşturma
Modelinin amacı, hangi (amaç veya hedefi için) kullanıcıdan isteyen ve sorunun hangi bölümlerinin yanıt belirlemeye yardımcı olmak (varlık) ayrıntıları sağlama out şekil sağlamaktır. 

Model sözcükleri belirlemek için uygulama etki alanına özel olması ve ilgili yanı sıra tipik sözcük sıralama, tümceleri. 

Model için amaç gerekir ve varlıklar _olmalıdır_ . 

## <a name="add-training-examples"></a>Eğitim örnekleri Ekle
LUIS, örnek konuşma amacı de gerekir. Örnekler, sözcük seçimi ve sözcük sırasını utterance yöneliktir hangi hedefini belirlemek için yeterli sayıda çeşitlemesi gerekir. Her örnek utterance varlıklar olarak etiketlenen tüm gerekli veri olmalıdır. 

LUIS için utterance atayarak uygulamanızın etki ilgili olmayan konuşma yok saymak için toplamasını **hiçbiri** hedefi. Herhangi bir sözcük veya gerekmeyen tümcecikleri dışında bir utterance çekilen etiketlenmiş gerekmez. Sözcük ve tümcecikleri yok saymak için hiçbir etiket yok. 

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama
Her bir amaca göre 15 ila 30 farklı yere sahip olduktan sonra, etiketli gerekli varlıklar sayesinde [, daha sonra](luis-how-to-train.md) [yayımlamanız](luis-how-to-publish-app.md)gerekir. Yayımlama başarılı bildirimden bağlantı uç noktalarınızı almak için kullanın. Uygulamanızı, ihtiyacınız olan [uç nokta bölgelerinde](luis-reference-regions.md) kullanılabilir olacak şekilde oluşturup yayımladığınızdan emin olun. 

## <a name="https-prediction-endpoint-testing"></a>HTTPS tahmin uç noktası testi
LUSıS uygulamanızı HTTPS tahmin uç noktasından test edebilirsiniz. Tahmin uç noktasından test etmek, LUSıS 'nin [Gözden geçirme](luis-how-to-review-endpoint-utterances.md)için düşük güvenilirliğe sahip herhangi bir noktayı seçmesine olanak sağlar.  

## <a name="recycle"></a>Geri dönüştür

Geliştirme döngüsünü işiniz bittiğinde, yeniden başlayabilirsiniz. [İnceleme tahmini uç noktası utterations](luis-how-to-review-endpoint-utterances.md) lu, düşük güvenilirlikle işaretlendi. Bu konuşma amacı hem de varlık için denetleyin. Konuşma gözden sonra İnceleme listesi boş olmalıdır.  

Geçerli sürümü yeni bir sürüme [kopyalamayı](luis-concept-version.md#clone-a-version) düşünün ve sonra yazma değişikliklerinizi yeni sürümde başlatın. 

## <a name="batch-testing"></a>Toplu işe testi

[Toplu işlem testi](luis-concept-batch-test.md) , kaç tane örnek kaçın Luo tarafından puanlanmadığını görmenin bir yoludur. Örnekler için LUIS yeni olmalıdır ve amacı ve LUIS bulmak için istediğiniz varlıkları ile doğru etiketli. LUIS, konuşma kümesinde ne kadar iyi gerçekleştirecek test sonuçlarını gösterir. 

## <a name="next-steps"></a>Sonraki adımlar

Kavramları hakkında bilgi edinin [işbirliği](luis-concept-keys.md).
