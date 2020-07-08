---
title: Kimlik Yapılandırma-Azure Event Grid IoT Edge | Microsoft Docs
description: Event Grid modülünün kimliğini yapılandırma
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/05/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 0aedeea2a6ad08e1627c2d1a6ebde6c91a4d02d9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76841774"
---
# <a name="configure-identity-for-the-event-grid-module"></a>Event Grid modülü için kimlik yapılandırma

Bu makale, kenardaki kılavuz için kimliğin nasıl yapılandırılacağını gösterir. Varsayılan olarak Event Grid modülü, kendi kimlik sertifikasını IoT güvenlik arka plan programı tarafından yapılandırılan şekilde sunar. Uç Event Grid, olay sunarken kendi kimlik sertifikasını giden çağrılarıyla birlikte sunar. Daha sonra bir abone, kabul etmeden önce olayı gönderen Event Grid modülünü doğrulayabilir.

Tüm olası yapılandırmalara yönelik [güvenlik ve kimlik doğrulama Kılavuzu '](security-authentication.md) na bakın.

## <a name="always-present-identity-certificate"></a>Her zaman kimlik sertifikası sunun
Giden çağrılar üzerinde her zaman bir kimlik sertifikası sunmaya yönelik örnek bir yapılandırma aşağıda verilmiştir. 

```json
 {
  "Env": [
    "outbound__clientAuth__clientCert__enabled=true",
    "outbound__clientAuth__clientCert__source=IoTEdge"
  ]
}
 ```

## <a name="dont-present-identity-certificate"></a>Kimlik sertifikası sunma
Giden çağrılar üzerinde kimlik sertifikası sunulmayan bir yapılandırma aşağıda verilmiştir. 

```json
 {
  "Env": [
    "outbound__clientAuth__clientCert__enabled=false"
  ]
}
 ```
