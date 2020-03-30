---
title: Örnek Azure API yönetim ilkesi - Ağ geçidi ve arka uç arasında yetkilendirme için OAuth2'yi kullanın
titleSuffix: Azure API Management
description: Azure API yönetim ilkesi örneği - Ağ geçidi ve arka uç arasında yetkilendirme için OAuth2'nin nasıl kullanılacağını gösterir. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir.
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
ms.openlocfilehash: 09d51759c07e7dacc25d5b5ffce9698831c37a7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442362"
---
# <a name="use-oauth2-for-authorization-between-the-gateway-and-a-backend"></a>Ağ geçidi ve arka uç arasında yetkilendirme için OAuth2'yi kullanın

Bu makalede, ağ geçidi ve arka uç arasında yetkilendirme için OAuth2'nin nasıl kullanılacağını gösteren bir Azure API yönetim ilkesi örneği gösterilmektedir. AAD'den erişim belirtecini alma ve bunu arka uca iletme işlemini de gösterir. 

İlke kodunu ayarlamak veya ayarlamak için, Ayarla'da açıklanan adımları izleyin [veya bir ilkeyi ayarlayın.](../set-edit-policies.md) Diğer örnekleri görmek için [ilke örneklerine](../policy-samples.md)bakın.

Aşağıdaki komut dosyası {{property}} içinde görünen özellikleri kullanır. Özellikler ve bunları API Yönetimi ilkelerinde nasıl kullanacağınızı öğrenmek için [bu](../api-management-howto-properties.md) konuya bakın.
 
## <a name="policy"></a>İlke

Kodu **gelen** bloğa yapıştır.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get OAuth2 access token from AAD and forward it to the backend.policy.xml)]
  
## <a name="next-steps"></a>Sonraki adımlar

APIM politikaları hakkında daha fazla bilgi edinin:

+ [Dönüştürme ilkeleri](../api-management-transformation-policies.md)
+ [İlke örnekleri](../policy-samples.md)

