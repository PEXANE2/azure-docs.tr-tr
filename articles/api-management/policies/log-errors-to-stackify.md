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
ms.openlocfilehash: 6662761df005211729dffb16282b8e0a8e2a8444
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75442451"
---
# <a name="send-errors-to-stackify-for-logging"></a>Günlüğe kaydetme için Stackbelirt hatalarını gönderme

Bu makalede, günlüğe kaydetme için Stackbelirt hata günlüğü ilkesinin nasıl ekleneceğini gösteren bir Azure API Management Policy örneği gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodu, **hata üzerindeki** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

