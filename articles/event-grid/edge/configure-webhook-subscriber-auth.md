---
title: Web kancası abonesi kimlik doğrulamasını Yapılandırma-Azure Event Grid IoT Edge | Microsoft Docs
description: Web kancası abone kimlik doğrulamasını yapılandırma
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 10/06/2019
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 101dcae5870322878cec48098f2efae32cc68c14
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76841739"
---
# <a name="configure-webhook-subscriber-authentication"></a>Web kancası abone kimlik doğrulamasını yapılandırma

Bu kılavuzda, bir Event Grid modülü için olası Web kancası abone yapılandırmalarının örnekleri verilmiştir. Varsayılan olarak, Web kancası aboneleri için yalnızca HTTPS uç noktaları kabul edilir. Abone, kendinden imzalı bir sertifika sunduğunda Event Grid modülü reddeder.

## <a name="allow-only-https-subscriber"></a>Yalnızca HTTPS abonesi izin ver

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

## <a name="allow-https-subscriber-with-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika ile HTTPS abonesi izin ver

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Özelliği, `outbound__webhook__allowUnknownCA` `true` genellikle otomatik olarak imzalanan sertifikaları kullanırken, yalnızca test ortamlarında olarak ayarlayın. Üretim iş yükleri için, bunların **false**olarak ayarlanması önerilir.

## <a name="allow-https-subscriber-but-skip-certificate-validation"></a>HTTPS abonesi izin ver ancak sertifika doğrulamayı atla

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=true",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

>[!NOTE]
>Özelliği, `outbound__webhook__skipServerCertValidation` `true` kimlik doğrulaması yapılması gereken bir sertifika sunmadığı için yalnızca test ortamlarında olarak ayarlayın. Üretim iş yükleri için, bunların **false** olarak ayarlanması önerilir

## <a name="allow-both-http-and-https-with-self-signed-certificates"></a>Otomatik olarak imzalanan sertifikalarla hem HTTP hem de HTTPS 'ye izin ver

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=false",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>`outbound__webhook__httpsOnly` `false` Önce bir http abonesi getirmek isteyebileceğiniz gibi, özelliği yalnızca test ortamlarında olarak ayarlayın. Üretim iş yükleri için, bunların **true** olarak ayarlanması önerilir
