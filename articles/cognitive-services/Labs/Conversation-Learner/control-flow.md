---
title: Conversation Learner denetim akışı-Microsoft bilişsel hizmetler | Microsoft Docs
titleSuffix: Azure
description: Conversation Learner denetim akışı hakkında bilgi edinin.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 3ec839c1a930ffbe73989149360f1b02866a3c50
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705322"
---
## <a name="control-flow"></a>Denetim akışı

Bu belge, aşağıdaki diyagramda gösterildiği gibi Conversation Learner (CL) denetim akışını açıklar.

![](media/controlflow.PNG)

1. Kullanıcı, bot 'ta bir terim veya tümcecik girer, örneğin ' Seattle 'daki Hava durumu nedir? '
1. CL, Kullanıcı girişini varlıkları çıkaran bir makine öğrenimi modeline geçirir
   - Bu model Conversation Learner tarafından oluşturulur ve www.luis.ai tarafından barındırılır
1. Ayıklanan tüm varlıklar ve kullanıcının giriş metni, bot 'un kodundaki varlık algılama geri çağırma yöntemine geçirilir.
    - Bu kod, varlık değerlerini ayarlayabilir/temizleyebilir/işleyebilir
1. CL sinir Network, varlık ayıklamanın ve Kullanıcı girişinin çıkışını alır ve botda tanımlanan tüm eylemlere puan alır
   - Bu örnekte, en yüksek olasılık, hava durumu tahminini sağlamaktır:

     ![](media/controlflow_forecast.PNG)

1. Bu durumda, seçilen eylem, hava durumu tahminini almak için bir API çağrısı gerektirir. 
1. CL kullanılarak kaydedilmiş bu API. AddCallback yöntemi daha sonra çağrılır.  Daha sonra bu API 'nin sonucu kullanıcıya bir ileti olarak döndürülür (örneğin, yüksek oranda 67 ile güneş).
1. Daha sonra bu çağrı, önceki adıma göre sonraki eylemi belirlemede sinir ağına yapılır.
1. Sinir ağı daha sonra olası eylemlerin bir sonraki kümesini tahmin eder ve seçili eylem kullanıcıya, bu durumda ' başka bir şey? ' olarak sunulur.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Conversation Learner nasıl öğretir](./how-to-teach-cl.md)
