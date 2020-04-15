---
title: IoT Hub'da TLS 1.0 ve 1.1 amortisman | Microsoft Dokümanlar
description: IoT Hub'da TLS 1.0 ve 1.1 ve desteklenen şifrelerin amortismanına ilişkin esaslar.
author: jlian
ms.author: jlian
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: a887dd4df44ba58b0e6646ffb1c10eb21edf3e69
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81381308"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub"></a>IoT Hub'da TLS 1.0 ve 1.1 amortismanı

Sınıfının en iyisi şifreleme sağlamak için IoT Hub, IoT aygıtları ve hizmetleri için tercih edilen şifreleme mekanizması olarak Aktarım Katmanı Güvenliği (TLS) 1'e geçiyor. 

## <a name="timeline"></a>Zaman çizelgesi

IoT Hub, bir sonraki duyuruya kadar TLS 1.0/1.1'i desteklemeye devam edecektir. Ancak, tüm müşterilerin tls 1.2'ye en kısa sürede geçiş lerini öneririz.

## <a name="supported-ciphers"></a>Desteklenen şifreler

TLS el sıkışmasında kullanılan çeşitli şifrelerin kullanılabilirliği için zaman çizelgesi aşağıdaki gibidir:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (şu anda desteklenir)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (2020 yılının ikinci yarısında desteklenecektir)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (2020 yılının ikinci yarısında desteklenecektir)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (2020 yılının ikinci yarısında desteklenecektir)

## <a name="customer-feedback"></a>Müşteri geri bildirimi

TLS 1.2 uygulaması endüstri çapında sınıfının en iyisi şifreleme seçeneği olsa da ve planlandığı gibi etkinleştirilecek olsa da, müşterilerimizin TLS 1.2'yi benimsemelerine ilişkin özel dağıtımları ve zorlukları hakkında bilgi almak isteriz. Bu amaçla yorumlarınızı [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com)gönderebilirsiniz.
