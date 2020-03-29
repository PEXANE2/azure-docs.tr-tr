---
title: Kimlik yapılandırma - Azure Olay Izgara IoT Edge | Microsoft Dokümanlar
description: Olay Izgara modülün kimliğini yapılandırma
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/05/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 0aedeea2a6ad08e1627c2d1a6ebde6c91a4d02d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841774"
---
# <a name="configure-identity-for-the-event-grid-module"></a>Olay Izgara modülü için kimlik yapılandırma

Bu makalede, Edge Grid için kimlik yapılandırmak için nasıl gösterir. Varsayılan olarak, Olay Izgara modülü ioT güvenlik daemon tarafından yapılandırılan kimlik sertifikasını sunar. Edge'deki Olay Grid, etkinlik sunarken kimlik sertifikasını giden çağrılarıyla birlikte sunar. Abone daha sonra, davet için olayı gönderdiği olay ısgarorta modülüolduğ

Tüm olası yapılandırmalar için [Güvenlik ve kimlik doğrulama](security-authentication.md) kılavuzuna bakın.

## <a name="always-present-identity-certificate"></a>Her zaman kimlik sertifikası nı sunun
Burada, giden aramalarda her zaman bir kimlik sertifikası sunmak için örnek bir yapılandırma verilmiştir. 

```json
 {
  "Env": [
    "outbound__clientAuth__clientCert__enabled=true",
    "outbound__clientAuth__clientCert__source=IoTEdge"
  ]
}
 ```

## <a name="dont-present-identity-certificate"></a>Kimlik belgesi ni sunmayın
İşte, giden aramalarda kimlik sertifikası ibraz etmemek için örnek bir yapılandırma. 

```json
 {
  "Env": [
    "outbound__clientAuth__clientCert__enabled=false"
  ]
}
 ```
