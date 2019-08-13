---
title: Sorun giderme-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Bu makalede, kişiselleştirici ile ilgili sorun giderme soruları bulunabilir.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/15/2019
ms.author: diberry
ms.openlocfilehash: 9f4c4129217923f7fb32996f7447ed09a034f888
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68955223"
---
# <a name="personalizer-troubleshooting"></a>Kişiselleştirici sorunlarını giderme

Bu makale, kişiselleştirici hakkında sık sorulan sorun giderme sorularına yanıtlar içerir.

## <a name="learning-loop"></a>Öğrenme döngüsü

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>Öğrenme döngüsü öğreniyor gibi görünüyor. Bunu nasıl düzeltirim?

Öğrenme döngüsünün, derece çağrıları etkin bir şekilde önceliklendirilmesi için birkaç bin yeniden çağrısı gerekir. 

Öğrenme döngünüz Şu anda çalışır durumda olduğundan emin değilseniz, [çevrimdışı bir değerlendirme](concepts-offline-evaluation.md)çalıştırın ve düzeltilen öğrenme ilkesini uygulayın. 

## <a name="next-steps"></a>Sonraki adımlar

[Model güncelleştirme sıklığını yapılandırma](how-to-settings.md#model-update-frequency)