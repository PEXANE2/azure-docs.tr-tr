---
title: Microsoft'tan Azure CDN için HTTP üstbilgisini hata ayıklama | Microsoft Dokümanlar
description: Hata ayıklama önbellek istek üstbilgileri, istenen varlığa uygulanan önbellek ilkesi hakkında ek bilgiler sağlar. Bu üstbilgi, Microsoft'un Azure CDN'ine özgüdir.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2019
ms.author: magattus
ms.openlocfilehash: 297c65c1cd89163b8663819f844dc6c2a83fd1bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68814079"
---
# <a name="debug-http-header-for-azure-cdn-from-microsoft"></a>Microsoft'tan Azure CDN için HTTP üstbilgisini hata ayıklama
Hata ayıklama yanıtı `X-Cache`üstbilgisi, içeriğin hangi katmandan sunulduğuna ilişkin ayrıntıları sağlar. Bu üstbilgi Microsoft'tan Azure CDN'ye özgüdür.

### <a name="response-header-format"></a>Yanıt üstbilgi biçimi

Üst bilgi | Açıklama
-------|------------
X-Önbellek: TCP_HIT | Bu üstbilgi, içerik CDN kenar önbelleğinden sunulduğunda döndürülür. 
X-Önbellek: TCP_REMOTE_HIT | Bu üstbilgi, içerik CDN bölgesel önbelleğinden (Başlangıç kalkanı katmanı) sunulduğunda döndürülür
X-Önbellek: TCP_MISS | Bu üstbilgi, önbellek miss olduğunda döndürülür ve içerik Origin'den sunulur. 


