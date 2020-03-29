---
title: Örnek API yönetim ilkesi - Günlük için Stackify'ye hata gönder
titleSuffix: Azure API Management
description: Azure API yönetim ilkesi örneği - Günlük günlüğe kaydetme için Stackify'ye hata göndermek için hata günlüğe kaydetme ilkesinin nasıl ekleyeceğini gösterir.
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
ms.openlocfilehash: 6662761df005211729dffb16282b8e0a8e2a8444
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442451"
---
# <a name="send-errors-to-stackify-for-logging"></a>Günlük için Stackify'ye hata gönderme

Bu makalede, günlük günlüğe kaydetmek için Stackify'ye hata göndermek için hata günlüğe kaydetme ilkesinin nasıl ekleniştiğini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="policy"></a>İlke

Kodu **hata bloğuna** yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

