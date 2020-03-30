---
title: Örnek API yönetim ilkesi - Yanıt önbelleği süresini ayarlama
titleSuffix: Azure API Management
description: Azure API yönetim ilkesi örneği - Arka uç tarafından gönderilen Önbellek Denetimi üstbilgisinde maksimum Yat değerini kullanarak yanıt önbelleği süresinin nasıl ayarlanacağını gösterir.
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
ms.openlocfilehash: 3101c5695272e8fa6b577ad313897cbc1fa29629
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442390"
---
# <a name="set-response-cache-duration"></a>Yanıt önbellek süresini ayarlama

Bu makalede, arka uç tarafından gönderilen Önbellek Denetimi üstbilgisinde maksimum Yat değerini kullanarak yanıt önbelleği süresinin nasıl ayarlanacağını gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

