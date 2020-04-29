---
title: IoT Hub 'de TLS 1,0 ve 1,1 kullanımdan kaldırılıyor | Microsoft Docs
description: IoT Hub 'de TLS 1,0 ve 1,1 ' nin kullanımdan kaldırılması ve desteklenen şifrelemeler hakkında yönergeler.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: a887dd4df44ba58b0e6646ffb1c10eb21edf3e69
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81381308"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>IoT Hub 'de TLS 1,0 ve 1,1 kullanımdan kaldırılması

En iyi sınıf şifrelemeyi sağlamak için IoT Hub, IoT cihazları ve hizmetleri için tercih edilen şifreleme mekanizması olarak Aktarım Katmanı Güvenliği (TLS) 1,2 ' ye geçmektir. 

## <a name="timeline"></a>Zaman çizelgesi

IoT Hub, daha fazla bildirimde bulunuluncaya kadar TLS 1.0/1.1 'yi desteklemeye devam edecektir. Bununla birlikte, tüm müşterilerin TLS 1,2 'e en kısa sürede geçişini öneririz.

## <a name="supported-ciphers"></a>Desteklenen şifrelemeler

TLS el sıkışmasında kullanılan çeşitli şifrelemelerin kullanılabilirliği için zaman çizelgesi aşağıdaki gibidir:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (Şu anda destekleniyor)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (2020 ikinci yarısında desteklenecektir)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (2020 ikinci yarısında desteklenecektir)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (2020 ikinci yarısında desteklenecektir)

## <a name="customer-feedback"></a>Müşteri geri bildirimi

TLS 1,2 zorlaması, sektör genelinde en iyi sınıf şifreleme seçiminden ve planlanan şekilde etkinleştirilebilse de, müşterilerin belirli dağıtımlarıyla ilgili ve TLS 1,2 benimseme zorluklarını öğrenmek istiyoruz. Bu amaçla, yorumlarınızı öğesine [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com)gönderebilirsiniz.
