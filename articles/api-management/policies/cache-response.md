---
title: Örnek API yönetim ilkesi - Arka uç hizmetine yetenek ekleme
titleSuffix: Azure API Management
description: Azure API yönetim ilkesi örneği - Arka uç hizmetine nasıl yetenek ekleyeceğinigösterir. Örneğin, hava durumu tahmini API'sinde enlem ve boylam yerine bir yer adını kabul edin.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: e286f77009a17870332ed5caf1c66c18dd21d020
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442481"
---
# <a name="add-capabilities-to-a-backend-service"></a>Arka uç hizmetine özellikler ekleme

Bu makalede, arka uç hizmetine nasıl yetenek ekleyeceğini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Örneğin, hava durumu tahmini API'sinde enlem ve boylam yerine bir yer adını kabul edin. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

