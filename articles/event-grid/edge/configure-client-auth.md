---
title: Gelen çağrıların istemci kimlik doğrulamasını yapılandırma - Azure Event Grid IoT Edge | Microsoft Dokümanlar
description: IoT Edge'de Event Grid tarafından maruz kalan API protokollerini yapılandırın.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 3363db4557dd19e8d72747ccd62bb535abb7b1e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841800"
---
# <a name="configure-client-authentication-of-incoming-calls"></a>Gelen çağrıların istemci kimlik doğrulamasını yapılandırma

Bu kılavuz, Olay Izgara modülü için olası istemci kimlik doğrulama yapılandırmaları örnekleri verir. Olay Izgara modülü iki tür istemci kimlik doğrulamasını destekler:

* Paylaşılan erişim imzası (SAS) anahtar tabanlı
* Sertifika tabanlı

Tüm olası yapılandırmalar için [Güvenlik ve kimlik doğrulama](security-authentication.md) kılavuzuna bakın.

## <a name="enable-certificate-based-client-authentication-no-self-signed-certificates"></a>Sertifika tabanlı istemci kimlik doğrulamasını etkinleştirme, kendi imzalı sertifika yok

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=false"
  ]
}
 ```

## <a name="enable-certificate-based-client-authentication-allow-self-signed-certificates"></a>Sertifika tabanlı istemci kimlik doğrulamasını etkinleştirin, kendi imzalı sertifikalara izin verin

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=false",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true"
  ]
}
```

>[!NOTE]
>Genellikle kendi imzaladığı sertifikalar kullanabileceğiniz gibi özelliği **inbound__clientAuth__clientCert__allowUnknownCA yalnızca** test ortamlarında **doğru** olarak ayarlayın. Üretim iş yükleri için, bu özelliği bir sertifika yetkilisinden (CA) **yanlış** ve sertifikaolarak ayarlamanızı öneririz.

## <a name="enable-certificate-based-and-sas-key-based-client-authentication"></a>Sertifika tabanlı ve sas tuşu tabanlı istemci kimlik doğrulamasını etkinleştirme

```json
 {
  "Env": [
    "inbound__clientAuth__sasKeys__enabled=true",
    "inbound__clientAuth__sasKeys__key1=<some-secret1-here>",
    "inbound__clientAuth__sasKeys__key2=<some-secret2-here>",
    "inbound__clientAuth__clientCert__enabled=true",
    "inbound__clientAuth__clientCert__source=IoTEdge",
    "inbound__clientAuth__clientCert__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>SAS anahtar tabanlı istemci kimlik doğrulaması, IoT kenar modülü olmayan bir modülün yönetim ve çalışma zamanı işlemleri yapmasına olanak tanır, elbette API bağlantı noktalarına IoT Edge ağının dışında erişilebilir olduğunu varsayarsak.
