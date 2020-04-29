---
title: Microsoft 'tan Azure CDN için HTTP üstbilgilerini hata ayıkla | Microsoft Docs
description: Hata ayıklama önbelleği istek üstbilgileri, istenen varlığa uygulanan önbellek ilkesi hakkında ek bilgiler sağlar. Bu üstbilgiler, Microsoft 'tan Azure CDN özgüdür.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: allensu
ms.openlocfilehash: 2475bdce3ab8f153cc837601964bf4a2e90a470c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81260426"
---
# <a name="debug-http-header-for-azure-cdn-from-microsoft"></a>Microsoft 'tan Azure CDN için HTTP üst bilgisinde hata ayıkla
Hata ayıklama yanıt üst bilgisi `X-Cache`, içerik tarafından hangi CDN yığınının hangi katmana sunulduğunu, ayrıntılar sağlar. Bu üst bilgi, Microsoft 'tan Azure CDN özgüdür.

### <a name="response-header-format"></a>Yanıt üst bilgisi biçimi

Üst bilgi | Açıklama
-------|------------
X-önbellek: TCP_HIT | Bu üst bilgi, içerik CDN Edge önbelleğinden sunulduğunda döndürülür. 
X-önbellek: TCP_REMOTE_HIT | Bu üst bilgi, içerik CDN bölgesel önbelleğinden (kaynak kalkan katmanı) sunulduğunda döndürülür
X-önbellek: TCP_MISS | Bu üst bilgi, bir önbellek isabetsizliği olduğunda ve içerik kaynaktan sunulduğunda döndürülür. 


