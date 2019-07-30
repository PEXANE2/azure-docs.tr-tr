---
title: Sorun giderme-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Bu makalede, kişiselleştirici ile ilgili sorun giderme soruları bulunabilir.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: article
ms.date: 06/15/2019
ms.author: diberry
ms.openlocfilehash: 7f7a6a08b86d21287c644f6a851d465d97f32e74
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663676"
---
# <a name="personalizer-troubleshooting"></a>Kişiselleştirici sorunlarını giderme

Bu makale, kişiselleştirici hakkında sık sorulan sorun giderme sorularına yanıtlar içerir.

## <a name="learning-loop"></a>Öğrenme döngüsü

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>Öğrenme döngüsü öğreniyor gibi görünüyor. Bunu nasıl düzeltirim?

Öğrenme döngüsünün, derece çağrıları etkin bir şekilde önceliklendirilmesi için birkaç bin yeniden çağrısı gerekir. 

Öğrenme döngünüz Şu anda çalışır durumda olduğundan emin değilseniz, [çevrimdışı bir değerlendirme](concepts-offline-evaluation.md)çalıştırın ve düzeltilen öğrenme ilkesini uygulayın. 

## <a name="next-steps"></a>Sonraki adımlar

[Model güncelleştirme sıklığını yapılandırma](how-to-settings.md#model-update-frequency)