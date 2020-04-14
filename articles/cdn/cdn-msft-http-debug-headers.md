---
title: Microsoft'tan Azure CDN için HTTP üstbilgisini hata ayıklama | Microsoft Dokümanlar
description: Hata ayıklama önbellek istek üstbilgileri, istenen varlığa uygulanan önbellek ilkesi hakkında ek bilgiler sağlar. Bu üstbilgi, Microsoft'un Azure CDN'ine özgüdir.
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
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260426"
---
# <a name="debug-http-header-for-azure-cdn-from-microsoft"></a>Microsoft'tan Azure CDN için HTTP üstbilgisini hata ayıklama
Hata ayıklama yanıtı `X-Cache`üstbilgisi, içeriğin hangi katmandan sunulduğuna ilişkin ayrıntıları sağlar. Bu üstbilgi Microsoft'tan Azure CDN'ye özgüdür.

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

Üst bilgi | Açıklama
-------|------------
X-Önbellek: TCP_HIT | Bu üstbilgi, içerik CDN kenar önbelleğinden sunulduğunda döndürülür. 
X-Önbellek: TCP_REMOTE_HIT | Bu üstbilgi, içerik CDN bölgesel önbelleğinden (Başlangıç kalkanı katmanı) sunulduğunda döndürülür
X-Önbellek: TCP_MISS | Bu üstbilgi, önbellek miss olduğunda döndürülür ve içerik Origin'den sunulur. 


