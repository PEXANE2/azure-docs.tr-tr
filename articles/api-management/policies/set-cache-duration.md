---
title: Örnek API yönetimi ilkesi-yanıt önbelleği süresini ayarlama
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-yanıt önbelleği süresinin, arka uç tarafından gönderilen Cache-Control üstbilgisinde maxAge değeri kullanılarak nasıl ayarlanacağını gösterir.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75442390"
---
# <a name="set-response-cache-duration"></a>Yanıt önbellek süresini ayarlama

Bu makalede, yanıt önbelleği süresinin, arka uç tarafından gönderilen Cache-Control üstbilgisinde maxAge değeri kullanılarak nasıl ayarlanacağı gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-samples.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

