---
title: IoT Hub ve cihaz sağlama hizmeti 'nde (DPS) TLS 1,0 ve 1,1 kullanımdan kaldırılıyor | Microsoft Docs
description: TLS 1,0 ve 1,1 ' nin kullanımdan kalkmasıyla ilgili yönergeler ve IoT Hub ve DPS 'de desteklenen şifrelemeler.
author: rezasherafat
ms.author: rezas
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: d61ca8fe7c6f5e7cc400714d7c31a0a7e50b8a88
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78402796"
---
# <a name="deprecation-of-tls-10-and-11-in-iot-hub-and-device-provisioning-service"></a>IoT Hub ve cihaz sağlama hizmeti 'nde TLS 1,0 ve 1,1 ' nin kullanımdan kaldırılması

En iyi sınıf şifrelemeyi sağlamak için, IoT Hub ve cihaz sağlama hizmeti (DPS), IoT cihazları ve hizmetleri için tercih edilen şifreleme mekanizması olarak Aktarım Katmanı Güvenliği (TLS) 1,2 ' ye taşınır. 

## <a name="supported-ciphers"></a>Desteklenen şifrelemeler

TLS el sıkışmasında kullanılan çeşitli şifrelemelerin kullanılabilirliği için zaman çizelgesi aşağıdaki gibidir:

* TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256 (Şu anda destekleniyor)
* TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384 (2020 ikinci yarısında desteklenecektir)
* TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (2020 ikinci yarısında desteklenecektir)
* TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (2020 ikinci yarısında desteklenecektir)


## <a name="customer-feedback"></a>Müşteri geri bildirimi

TLS 1,2 zorlaması, sektör genelinde en iyi sınıf şifreleme seçiminden ve planlanan şekilde etkinleştirilebilse de, müşterilerin belirli dağıtımlarıyla ilgili ve TLS 1,2 benimseme zorluklarını öğrenmek istiyoruz. Bu amaçla, yorumlarınızı [iot_tls1_deprecation@microsoft.com](mailto:iot_tls1_deprecation@microsoft.com)gönderebilirsiniz.
