---
title: Azure API yönetim ilkesi örneği - X-CSRF deseni uygulayın | Microsoft Dokümanlar
description: Azure API yönetim ilkesi örneği - Birçok API tarafından kullanılan X-CSRF deseni nasıl uygulanacağını gösterir. Bu örnek SAP Ağ Geçidi'ne özgüdür.
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
ms.openlocfilehash: 14ea9113bf5712d6ffce356d02abb7224c21771a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70067718"
---
# <a name="implement-x-csrf-pattern"></a>X-CSRF deseni uygulayın

Bu makalede, birçok API tarafından kullanılan X-CSRF deseni nasıl uygulanacağını gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Bu örnek SAP Ağ Geçidi'ne özgüdür. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get X-CSRF token from SAP gateway using send request.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

