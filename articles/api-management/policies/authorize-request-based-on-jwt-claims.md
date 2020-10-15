---
title: Örnek API yönetimi ilkesi-JWT taleplerine göre erişimi yetkilendir
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-JWT taleplerini temel alan bir API 'de belirli HTTP yöntemlerine erişim yetkisi verme işlemlerini gösterir.
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
ms.openlocfilehash: b5b0c2feb31eab5e39bbebd7108b3ec8504769d4
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92076292"
---
# <a name="authorize-access-based-on-jwt-claims"></a>JWT talepleri temelinde erişim yetkisi verme

Bu makalede, JWT taleplerini temel alan bir API üzerinde belirli HTTP yöntemlerine erişimin nasıl yetkilendirdiğini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-reference.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Pre-authorize requests based on HTTP method with validate-jwt.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-reference.md)