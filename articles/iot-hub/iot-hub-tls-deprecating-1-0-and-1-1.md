---
title: IoT Hub 'de TLS 1,0 ve 1,1 kullanımdan kaldırılıyor | Microsoft Docs
description: IoT Hub 'de TLS 1,0 ve 1,1 ' nin kullanımdan kaldırılması ve desteklenen şifrelemeler hakkında yönergeler.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: 5c717a02c2008436617d16f08625a1cecc204340
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83849527"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>IoT Hub 'de TLS 1,0 ve 1,1 kullanımdan kaldırılması

En iyi sınıf şifrelemeyi sağlamak için IoT Hub, IoT cihazları ve hizmetleri için tercih edilen şifreleme mekanizması olarak Aktarım Katmanı Güvenliği (TLS) 1,2 ' ye geçmektir. 

## <a name="timeline"></a>Zaman çizelgesi

IoT Hub, daha fazla bildirimde bulunuluncaya kadar TLS 1.0/1.1 'yi desteklemeye devam edecektir. Bununla birlikte, tüm müşterilerin TLS 1,2 'e en kısa sürede geçişini öneririz.

## <a name="deprecating-tls-11-ciphers"></a>TLS 1,1 şifrelemeleri kullanımdan kaldırma

* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SH`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="deprecating-tls-10-ciphers"></a>TLS 1,0 şifrelemeleri kullanımdan kaldırma

* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="tls-12-ciphers"></a>TLS 1,2 şifrelemeleri

Bkz. [IoT Hub TLS 1,2 önerilen şifre](iot-hub-tls-support.md#recommended-ciphers).
 
## <a name="customer-feedback"></a>Müşteri geri bildirimi

TLS 1,2 zorlaması, sektör genelinde en iyi sınıf şifreleme seçiminden ve planlanan şekilde etkinleştirilebilse de, müşterilerin belirli dağıtımlarıyla ilgili ve TLS 1,2 benimseme zorluklarını öğrenmek istiyoruz. Bu amaçla, yorumlarınızı öğesine gönderebilirsiniz [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com) .
