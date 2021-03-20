---
title: Örnek API yönetimi ilkesi-arka uç hizmetine özellik ekleme
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-bir arka uç hizmetine nasıl özellik ekleneceğini gösterir. Örneğin, hava durumu tahmini API'sinde enlem ve boylam yerine bir yer adını kabul edin.
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
ms.openlocfilehash: 64db4e5451425002eeaac11695ac011a96047d9b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92076190"
---
# <a name="add-capabilities-to-a-backend-service"></a>Arka uç hizmetine özellik ekleme

Bu makalede, bir arka uç hizmetine nasıl özellik ekleneceğini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Örneğin, hava durumu tahmini API'sinde enlem ve boylam yerine bir yer adını kabul edin. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-reference.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-reference.md)