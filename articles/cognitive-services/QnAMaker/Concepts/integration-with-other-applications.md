---
title: Diğer uygulamalarla tümleştirme - QnA Maker
description: QnA Maker, sohbet botları gibi istemci uygulamalarına ve Dil Anlama (LUIS) gibi diğer doğal dil işleme hizmetlerine entegre dir.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: c1edbfb6badfb73ce08a99709da0f8bfb61b7dc3
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804199"
---
# <a name="design-knowledge-base-for-client-applications"></a>İstemci uygulamaları için tasarım bilgi tabanı

QnA Maker, sohbet botları gibi istemci uygulamalarına ve Dil Anlama (LUIS) gibi diğer doğal dil işleme hizmetlerine entegre dir.

## <a name="integration-with-a-conversational-client"></a>Konuşma istemcisi ile tümleştirme

QnA Maker, [Microsoft Bot Framework](https://dev.botframework.com/)gibi konuşma istemcisi uygulamalarıyla tümleşir. QnA Maker'a gönderilen metnin temizlenmesi veya dönüştürülmesi gerekmez. QnA Maker doğal dilleri kabul eder ve en iyi cevabı geri verir.

## <a name="create-a-bot-without-writing-any-code"></a>Kod yazmadan bir bot oluşturma

Bilgi tabanınızı yayımladıktan **sonra, Bot Oluştur** düğmesini seçerek **Yayımla** sayfasından bir bot oluşturun. Düğmeyi seçtikten sonra ne olacağını öğrenmek için [bot öğreticisini](../Quickstarts/create-publish-knowledge-base.md) kullanın.

## <a name="providing-multi-turn-conversations"></a>Çok döndürerek konuşmalar sağlama

Bir bot istemcisi bilgi tabanınızdan en iyi seçilen yanıtı sağlar ve yanıt çok döndürülme QnA çiftinin bir parçasıysa izleme istemleri sağlayabilir. Bilgi [tabanınıza](../how-to/multiturn-conversation.md) çok döndürme sorusu ve yanıt kümelerini nasıl ekleyeceğinizi öğrenin.

## <a name="natural-language-processing"></a>Doğal dil işleme

QnA Maker, doğal dil işleme kullanan soruları işlerken, birden çok bilgi tabanından gelen soruları yanıtlayan daha büyük bir sistemin bir parçası da kullanılabilir. Belirli bir bilgi tabanına gelmeden önce doğal dil işleme sağlamak için QnA Maker'ı başka bir Bilişsel Hizmet olan Dil Anlayışı (LUIS) ile birleştirebilirsiniz. LUIS ve [QnA Maker'ın](../../luis/choose-natural-language-processing-service.md?toc=/azure/cognitive-services/qnamaker/toc.json) ne zaman ve nasıl kullanılacağı hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

QnA Maker için geliştirme döngüsü [kavramlarını](development-lifecycle-knowledge-base.md) öğrenin.