---
title: Kullanıcı söyleyinmelerini gözden geçirin - LUIS
titleSuffix: Azure Cognitive Services
description: Aktif öğrenme ile, doğru niyet ve varlık için gözden uç nokta söyleyiş. LUIS emin olduğu uç nokta söyleyişlerini seçer.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219945"
---
# <a name="concepts-for-enabling-active-learning-by-reviewing-endpoint-utterances"></a>Uç nokta söyleyerek aktif öğrenmeyi etkinleştirme kavramları
Aktif öğrenme, tahmin doğruluğunu ve uygulanması en kolay olanın geliştirilmesi için üç stratejiden biridir. Aktif öğrenme ile, doğru niyet ve varlık için gözden uç nokta söyleyiş. LUIS emin olduğu uç nokta söyleyişlerini seçer.

## <a name="what-is-active-learning"></a>Aktif öğrenme nedir
Aktif öğrenme iki aşamalı bir süreçtir. İlk olarak, LUIS doğrulama gerektiren uygulamanın bitiş noktasında aldığı sözcükleri seçer. İkinci adım, doğru niyet ve amaç içindeki tüm varlıklar da dahil olmak üzere, [gözden geçirilmesi](luis-how-to-review-endpoint-utterances.md)için seçilen sözcükleri doğrulamak için uygulama sahibi veya ortak layıcı tarafından gerçekleştirilir. Sözcükleri inceledikten sonra uygulamayı yeniden eğitin ve yayınlayın.

## <a name="which-utterances-are-on-the-review-list"></a>İnceleme listesinde hangi sözlerle yer alıyor?
LUIS, en iyi atış niyeti düşük bir puana sahip olduğunda veya en iyi iki niyetin puanları çok yakın olduğunda inceleme listesine ifadeler ekler.

## <a name="single-pool-for-utterances-per-app"></a>Uygulama başına söyleyişler için tek havuz
**Gözden Geçirme uç nokta seleçler** listesi sürüme göre değişmez. Etkin olarak hangi sürümün ifadesini düzenlediğinizden veya uç noktada uygulamanın hangi sürümünün yayımlandığından bağımsız olarak, gözden geçirilecek tek bir ifade havuzu vardır.

## <a name="where-are-the-utterances-from"></a>Nereden gelen söz
Endpoint söyleyişleri, uygulamanın HTTP bitiş noktasındaki son kullanıcı sorgularından alınır. Uygulamanız yayınlanmadıysa veya henüz isabet almadıysa, incelemeniz gereken herhangi bir söz yoktur. Belirli bir amaç veya varlık için bitiş noktası isabetleri alınmazsa, bunları içeren bunları gözden geçirmek için söyleyinin yoktur.

## <a name="schedule-review-periodically"></a>İncelemeyi periyodik olarak zamanlama
Önerilen söyleyişlerin gözden geçirilmesi her gün yapılması gerekmez, ancak LUIS'in düzenli bakımının bir parçası olmalıdır.

## <a name="delete-review-items-programmatically"></a>İnceleme öğelerini programlı olarak silme
Silin **[etiketlenmemiş söyleyinçler](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9)** API'yi kullanın. **[Günlük dosyalarını dışa aktararak](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)** silinmeden önce bu söyleyiyi yedekleyin.

## <a name="enable-active-learning"></a>Etkin öğrenmeyi etkinleştirme

Etkin öğrenmeyi etkinleştirmek için kullanıcı sorgularını günlüğe kaydetmeniz gerekir. Bu querystring parametresi ve değeri `log=true` ile [bitiş noktası sorgusu](luis-get-started-create-app.md#query-the-v3-api-prediction-endpoint) çağırarak gerçekleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

* Uç nokta söyleyişlerini nasıl [inceleyire](luis-how-to-review-endpoint-utterances.md) öğrenin
