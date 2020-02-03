---
title: Kullanıcı söylenişi-Lua gözden geçirme
titleSuffix: Azure Cognitive Services
description: Etkin öğrenme, doğru amacı ve varlık için gözden geçirme uç nokta Konuşma ile. LUIS, uç nokta konuşma emin seçer.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: diberry
ms.openlocfilehash: 375d4b4e7c3fcafbdfde1ff447bedc3e16aff2f2
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76710548"
---
# <a name="concepts-for-enabling-active-learning-by-reviewing-endpoint-utterances"></a>Etkin öğrenme konuşma uç noktası inceleyerek etkinleştirmek için kavramları
Etkin öğrenme tahmin doğruluğunu ve kolay uygulama geliştirmek için üç strateji biridir. Etkin öğrenme, doğru amacı ve varlık için gözden geçirme uç nokta Konuşma ile. LUIS, uç nokta konuşma emin seçer.

## <a name="what-is-active-learning"></a>Etkin öğrenme nedir
Etkin öğrenme iki adımlı bir işlemdir. İlk olarak, doğrulama gerekli uygulamanın uç noktada aldığı konuşma LUIS seçer. İkinci adım, uygulama sahibi veya ortak çalışan tarafından, doğru amaç ve amaç içindeki herhangi bir varlık dahil olmak üzere, [Gözden geçirme](luis-how-to-review-endpoint-utterances.md)için seçilen atları doğrulamak üzere gerçekleştirilir. Konuşma inceledikten sonra eğitme ve uygulamayı yeniden yayımlayın.

## <a name="which-utterances-are-on-the-review-list"></a>Hangi konuşma gözden geçirme listede yer
LUIS konuşma amacı tetikleme en düşük puan sahip veya üst iki amacı puanları çok Kapat gözden geçirme listesine ekler.

## <a name="single-pool-for-utterances-per-app"></a>Tek bir havuz için uygulama başına konuşma
**Gözden geçirme uç noktası sıralayıcısı** listesi, sürüme göre değişmez. Etkin olarak hangi sürümün ifadesini düzenlediğinizden veya uç noktada uygulamanın hangi sürümünün yayımlandığından bağımsız olarak, gözden geçirilecek tek bir ifade havuzu vardır.

## <a name="where-are-the-utterances-from"></a>Gelen sesleri nerede
Konuşma uç noktası son kullanıcı sorgularını uygulamanın HTTP uç noktasında alınmıştır. Uygulamanızı yayımlanmadı veya isabet henüz almadı, gözden geçirmek için herhangi bir konuşma yoktur. Belirli bir amaç veya varlık için hiçbir uç noktası İsabeti alınırsa, bunları içeren gözden geçirmek için konuşma yoktur.

## <a name="schedule-review-periodically"></a>Zamanlama düzenli aralıklarla gözden geçirme
Önerilen konuşma gözden geçirme, her gün yapılması gerekmez, ancak, düzenli bakım LUIS'ın bir parçası olması gerekir.

## <a name="delete-review-items-programmatically"></a>Gözden geçirme öğelerini program aracılığıyla silme
**[Etiketlenmemiş utterslar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9)** API 'sini Sil. **[Günlük dosyalarını dışa aktararak](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)** silinmeden önce bu utbotları yedekleyin.

## <a name="enable-active-learning"></a>Etkin öğrenmeyi etkinleştir

Etkin öğrenmeyi etkinleştirmek için Kullanıcı sorgularını günlüğe yazmanız gerekir. Bu, `log=true` QueryString parametresi ve değeri ile [Endpoint sorgusu](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint) çağırarak gerçekleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

* Endpoint deterlerini [incelemeyi](luis-how-to-review-endpoint-utterances.md) öğrenin
