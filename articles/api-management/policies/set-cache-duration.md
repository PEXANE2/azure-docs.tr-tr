---
title: Örnek API yönetimi ilkesi-yanıt önbelleği süresini ayarlama
titleSuffix: Azure API Management
description: Azure API Management ilkesi örneği-arka uç tarafından gönderilen Cache-Control üst bilgisinde maxAge değeri kullanarak yanıt önbelleği süresinin nasıl ayarlanacağını gösterir.
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
ms.openlocfilehash: 088dfbcbfe00357c47de4662ac31b52e83d1d8ab
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92070988"
---
# <a name="set-response-cache-duration"></a>Yanıt önbellek süresini ayarlama

Bu makalede, arka uç tarafından gönderilen Cache-Control üst bilgisinde maxAge değeri kullanarak yanıt önbelleği süresinin nasıl ayarlanacağını gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. Bir ilke kodu ayarlamak veya düzenlemek için, [Ilke ayarlama veya düzenleme](../set-edit-policies.md)bölümünde açıklanan adımları izleyin. Diğer örnekleri görmek için bkz. [ilke örnekleri](../policy-reference.md).

## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştırın.

[!code-xml[Main](../../../api-management-policy-samples/examples/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APıM ilkeleri hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-reference.md)