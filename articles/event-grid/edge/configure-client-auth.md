---
title: Gelen çağrılar için istemci kimlik doğrulamasını Yapılandırma-Azure Event Grid IoT Edge | Microsoft Docs
description: Event Grid modülü için olası istemci kimlik doğrulama yapılandırmalarının hakkında bilgi edinin.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: 9525da4204e270d033f0c2354318bd71874eaf54
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91290781"
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
