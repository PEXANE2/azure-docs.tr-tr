---
title: Gelen çağrılar için istemci kimlik doğrulamasını Yapılandırma-Azure Event Grid IoT Edge | Microsoft Docs
description: IoT Edge Event Grid tarafından açığa çıkarılan API protokollerini yapılandırın.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/03/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 3363db4557dd19e8d72747ccd62bb535abb7b1e2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76841800"
---
# <a name="configure-client-authentication-of-incoming-calls"></a>Gelen çağrıların istemci kimlik doğrulamasını yapılandırma

Bu kılavuzda, Event Grid modülü için olası istemci kimlik doğrulama yapılandırmalarının örnekleri verilmiştir. Event Grid modülü iki tür istemci kimlik doğrulamasını destekler:

* Paylaşılan erişim imzası (SAS) anahtar tabanlı
* Sertifika tabanlı

Tüm olası yapılandırmalara yönelik [güvenlik ve kimlik doğrulama Kılavuzu '](security-authentication.md) na bakın.

## <a name="enable-certificate-based-client-authentication-no-self-signed-certificates"></a>Sertifika tabanlı istemci kimlik doğrulamasını etkinleştirme, otomatik olarak imzalanan sertifika yok

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

## <a name="enable-certificate-based-client-authentication-allow-self-signed-certificates"></a>Sertifika tabanlı istemci kimlik doğrulamasını etkinleştirme, otomatik olarak imzalanan sertifikalara izin verme

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
>**İnbound__clientAuth__clientCert__allowUnknownCA** özelliği, genellikle otomatik olarak imzalanan sertifikaları kullanırken, yalnızca test ortamlarında **true** olarak ayarlayın. Üretim iş yükleri için, bu özelliği **yanlış** ve bir sertifika YETKILISINDEN (CA) sertifikalar olarak ayarlamanızı öneririz.

## <a name="enable-certificate-based-and-sas-key-based-client-authentication"></a>Sertifika tabanlı ve SAS anahtar tabanlı istemci kimlik doğrulamasını etkinleştir

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
>SAS anahtar tabanlı istemci kimlik doğrulaması, IoT olmayan bir uç modülün yönetim ve çalışma zamanı işlemlerine izin verir ve bu da API bağlantı noktalarına IoT Edge ağı dışından erişilebilir.
