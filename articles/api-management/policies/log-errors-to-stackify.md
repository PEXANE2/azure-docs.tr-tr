---
title: Örnek API yönetimi ilkesi-günlüğe kaydetme için Stackbelirt hataları gönderme
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-günlüğe kaydetme için Stackbelirt 'e hata göndermek üzere bir hata günlüğü ilkesinin nasıl ekleneceğini gösterir.
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
ms.openlocfilehash: 89428e9a64c6d4ae78d333c0cf597531588b1638
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92072076"
---
# <a name="send-errors-to-stackify-for-logging"></a>Günlüğe kaydetme için Stackbelirt hatalarını gönderme

Bu makalede, günlüğe kaydetme için Stackbelirt hata günlüğü ilkesinin nasıl ekleneceğini gösteren bir Azure API Management Policy örneği gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-reference.md).

## <a name="policy"></a>İlke

Kodu, **hata üzerindeki** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-reference.md)