---
title: Örnek API yönetim ilkesi - Dış yetkili kullanarak isteği yetkilendirme
titleSuffix: Azure API Management
description: Azure API yönetim ilkesi örneği - Özel veya eski kimlik doğrulama/yetkilendirme mantığını kapsülleme yi kapsayan harici yetkili kullanarak istekleri nasıl onaylayanları gösterir.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 99bf1068042eb7ab0c43e2a683ca7116d2e426f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442493"
---
# <a name="authorize-requests-using-external-authorizer"></a>Dış yetkilendirici kullanarak istekleri yetkilendirme

Bu makalede, özel kimlik doğrulaması/yetkilendirme mantığını kapsülleyen harici bir yetkili kullanarak API erişiminin nasıl güvenli hale alınabildiğini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Erişim kısıtlamaları ilkeleri](../api-management-access-restriction-policies.md)
+ [İlke örnekleri](../policy-samples.md)
