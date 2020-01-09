---
title: IoT Hub ve cihaz sağlama hizmeti 'nde (DPS) TLS 1,0 ve 1,1 kullanımdan kaldırılması | Microsoft Docs
description: TLS 1,0 ve 1,1 ' nin kullanımdan kalkmasıyla ilgili yönergeler ve IoT Hub ve DPS 'de desteklenen şifrelemeler.
author: rezas
ms.author: reza
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: f1ee3156e5639ae47d5bb323cf992586580668f5
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75485916"
---
# <a name="tls-10-and-11-deprecation-in-iot-hub-and-device-provisioning-service"></a>IoT Hub ve cihaz sağlama hizmeti 'nde TLS 1,0 ve 1,1 kullanımdan kaldırma

En iyi sınıf şifrelemeyi sağlamak için, IoT Hub ve cihaz sağlama hizmeti (DPS), IoT cihazları ve hizmetleri için tercih edilen şifreleme mekanizması olarak Aktarım Katmanı Güvenliği (TLS) 1,2 ' ye taşınır. Bu nedenle, TLS 1,0 ve TLS 1,1 için eski destek ve ayrıca, önerilen bazı eski şifrelemeler **1 temmuz 2020**' de kullanım dışı olacaktır.


## <a name="impact"></a>Etki
Müşterilerin belirli koşullara ve yapılandırmalarına bağlı olarak, TLS 1,0 ve 1,1 ' nin kullanımdan kaldırılması ve önerilmeyen eski şifrelemeler, IoT Hub veya DPS ile iletişim Kuran IoT cihazlarınız ve hizmetleriniz için kesin bir değişiklik olabilir. Bazı durumlarda, bu değişikliklerle uyumsuz olan cihazlar ve hizmetler, belirtilen kesme tarihinden sonra IoT Hub veya DPS 'e bağlanamaz.


## <a name="supported-ciphers"></a>Desteklenen şifrelemeler

TLS el sıkışması sırasında yalnızca aşağıdaki şifrelemeleri kullanılabilir olacaktır:

* TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
* TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
* TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384


## <a name="customer-feedback"></a>Müşteri geri bildirimi

TLS 1.2 uygulaması, sektör genelinde en iyi sınıf şifreleme seçimi olarak planlanan şekilde etkinleştirilse de, müşterilerin kendi dağıtımlarıyla ilgili zorlukları ve TLS 1.2’yi benimseme zorluklarını öğrenmek istiyoruz. Bu amaçla, yorumlarınızı [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com)gönderebilirsiniz.