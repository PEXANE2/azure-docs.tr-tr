---
title: Diğer uygulamalarla tümleştirin-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma, sohbet botları gibi istemci uygulamalarıyla ve Language Understanding (LUSıS) gibi diğer doğal dil işleme hizmetlerine tümleştirilir.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: c1edbfb6badfb73ce08a99709da0f8bfb61b7dc3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80804199"
---
# <a name="design-knowledge-base-for-client-applications"></a>İstemci uygulamaları için Bilgi Bankası tasarlama

Soru-Cevap Oluşturma, sohbet botları gibi istemci uygulamalarıyla ve Language Understanding (LUSıS) gibi diğer doğal dil işleme hizmetlerine tümleştirilir.

## <a name="integration-with-a-conversational-client"></a>Konuşma istemcisiyle tümleştirme

Soru-Cevap Oluşturma, [Microsoft bot Framework](https://dev.botframework.com/)gibi konuşma istemci uygulamalarıyla tümleşir. Soru-Cevap Oluşturma gönderilen metnin temizlenmesi veya dönüştürülmesi gerekmez. Soru-Cevap Oluşturma doğal dilleri kabul eder ve en iyi yanıtı döndürür.

## <a name="create-a-bot-without-writing-any-code"></a>Herhangi bir kod yazmadan bir bot oluştur

Bilgi tabanınızı yayımladıktan sonra, **bir bot Oluştur düğmesini seçerek** **Yayımla** sayfasından bir bot oluşturun. Düğmeyi seçtikten sonra ne olacağını öğrenmek için [bot öğreticisini](../Quickstarts/create-publish-knowledge-base.md) kullanın.

## <a name="providing-multi-turn-conversations"></a>Çoklu açma konuşmaları sağlama

Bir bot istemcisi, bilgi tabanınızdan en iyi seçili yanıtı sağlar ve yanıt çok yönlü bir QnA çiftinin parçasıysa, izleme istemleri sağlayabilir. Bilgi bankamanıza çok yönlü konuşma sorusu ve yanıt [kümeleri eklemeyi öğrenin](../how-to/multiturn-conversation.md) .

## <a name="natural-language-processing"></a>Doğal dil işleme

Soru-Cevap Oluşturma doğal dil işleme kullanan soruları işlerken, birden fazla bilgi bankasından gelen soruları yanıtlayan daha büyük bir sisteminin bir parçası de kullanılabilir. Belirli bir bilgi tabanına başlamadan önce doğal dil işleme sağlamak için, Soru-Cevap Oluşturma başka bir bilişsel hizmetle, Language Understanding (LUU) ile birleştirebilirsiniz. Lune zaman ve nasıl kullanılacağı [ve soru-cevap oluşturma](../../luis/choose-natural-language-processing-service.md?toc=/azure/cognitive-services/qnamaker/toc.json) birlikte hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Soru-Cevap Oluşturma için geliştirme döngüsünün [kavramlarını](development-lifecycle-knowledge-base.md) öğrenin.