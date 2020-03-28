---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 01/15/2020
ms.author: diberry
ms.openlocfilehash: 7ef219e6b5f7547029612ec3898efec51abd4712
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76122897"
---
## <a name="change-the-model-update-frequency"></a>Model güncelleştirme sıklığını değiştirme

Azure portalında, **Yapılandırma** sayfasındaki Personalizer kaynağında **Model güncelleştirme sıklığını** 10 saniyeye değiştirin. Bu kısa süre, her yineleme için en iyi eylemin nasıl değiştiğini görmenizi sağlayarak hizmeti hızla eğitecektir.

![Model güncelleştirme sıklığını değiştirme](../media/settings/configure-model-update-frequency-settings.png)

Bir Personalizer döngüsü ilk anlık olduğunda, eğitim için hiçbir Ödül API çağrıları olmamıştır beri hiçbir model yoktur. Sıralama çağrıları her öğe için eşit olasılıklar döndürecektir. Uygulamanız yine de her zaman RewardActionId çıktısını kullanarak içeriği sıralamalıdır.