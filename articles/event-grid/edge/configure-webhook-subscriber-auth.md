---
title: Webhook abone kimlik doğrulamasını yapılandırma - Azure Event Grid IoT Edge | Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76841739"
---
# <a name="configure-webhook-subscriber-authentication"></a>Web kancası abone kimlik doğrulamasını yapılandırma

Bu kılavuz, bir Olay Izgara modülü için olası webhook abone yapılandırmaları örnekleri verir. Varsayılan olarak, webhook aboneleri için yalnızca HTTPS uç noktaları kabul edilir. Abone kendi imzalı bir sertifika sunarsa Olay Izgara modülü reddeder.

## <a name="allow-only-https-subscriber"></a>Yalnızca HTTPS abonesi ne kadar izin ver

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

## <a name="allow-https-subscriber-with-self-signed-certificate"></a>Https aboneye kendi imzalı sertifikasına izin verme

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
>Genellikle kendi `outbound__webhook__allowUnknownCA` `true` imzalı sertifikalar kullanabileceğiniz şekilde özelliği yalnızca test ortamlarında ayarla. Üretim iş yükleri için bunların **yanlış**olarak ayarlanmasını öneririz.

## <a name="allow-https-subscriber-but-skip-certificate-validation"></a>HTTPS abonesi izin ver ama sertifika doğrulama atlamak

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
>Kimliği doğrulaması gereken bir sertifika sunmuyor olsanız da özelliği `outbound__webhook__skipServerCertValidation` `true` yalnızca test ortamlarına ayarlayın. Üretim iş yükleri için, bunların **yanlış** olarak ayarlanmasını öneririz

## <a name="allow-both-http-and-https-with-self-signed-certificates"></a>Kendi imzalı sertifikalarla hem HTTP hem de HTTPS'ye izin verin

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
>Önce bir `outbound__webhook__httpsOnly` `false` HTTP abonesi getirmek isteyebileceğin şekilde özelliği yalnızca test ortamlarına ayarlayın. Üretim iş yükleri için bunların **doğru** şekilde ayarlanmalarını öneririz
