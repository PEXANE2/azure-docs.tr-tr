---
title: IoT Hub ve Cihaz Sağlama Hizmetinde (DPS) 1.0 ve 1.1 TLS'nin Amortismana Sokulma | Microsoft Dokümanlar
description: TLS 1.0 ve 1.1'in amortismanı ile IoT Hub ve DPS'de desteklenen şifrelere ilişkin yönergeler.
author: rezasherafat
ms.author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: d61ca8fe7c6f5e7cc400714d7c31a0a7e50b8a88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78402796"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub-and-device-provisioning-service"></a>IoT Hub ve Cihaz Sağlama Hizmetinde TLS 1.0 ve 1.1 amortismanı

Sınıfının en iyisi şifreleme sağlamak için, IoT Hub ve Aygıt Sağlama Hizmeti (DPS), IoT aygıtları ve hizmetleri için tercih edilen şifreleme mekanizması olarak Aktarım Katmanı Güvenliği (TLS) 1'e geçiyor. 

## <a name="supported-ciphers"></a>Desteklenen şifreler

TLS el sıkışmasında kullanılan çeşitli şifrelerin kullanılabilirliği için zaman çizelgesi aşağıdaki gibidir:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (şu anda desteklenir)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (2020 yılının ikinci yarısında desteklenecektir)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (2020 yılının ikinci yarısında desteklenecektir)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (2020 yılının ikinci yarısında desteklenecektir)


## <a name="customer-feedback"></a>Müşteri geri bildirimi

TLS 1.2 uygulaması endüstri çapında sınıfının en iyisi şifreleme seçeneği olsa da ve planlandığı gibi etkinleştirilecek olsa da, müşterilerimizin TLS 1.2'yi benimsemelerine ilişkin özel dağıtımları ve zorlukları hakkında bilgi almak isteriz. Bu amaçla yorumlarınızı [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com)gönderebilirsiniz.
