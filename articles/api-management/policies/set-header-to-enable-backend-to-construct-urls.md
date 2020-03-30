---
title: Azure API yönetim ilkesi örneği - İletmeli üstbilgi ekleme | Microsoft Dokümanlar
description: Azure API yönetim ilkesi örneği - Arka uç API'sinin uygun URL'ler oluşturmasına izin vermek için gelen istekte Iletmeli üstbilginin nasıl ekleyeceğini gösterir.
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
ms.openlocfilehash: e525029ae8eab086d11126a4e18958423e207aa1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70067496"
---
# <a name="add-a-forwarded-header"></a>İlliyeli üstbilgi ekleme

Bu makalede, arka uç API'nin uygun URL'ler oluşturmasına izin vermek için gelen istekte Iletilen üstbilginin nasıl ekleyeceğini gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

## <a name="code"></a>Kod

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)
