---
title: Azure IoT Hub TLS desteği
description: IoT Hub ile iletişim kuran aygıtlar ve hizmetler için güvenli TLS bağlantılarını kullanmanın en iyi uygulamaları
services: iot-hub
author: rezasherafat
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 01/10/2020
ms.author: rezas
ms.openlocfilehash: 7ab3b48d22f116a707f68cbf6284928c7d2557e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79409520"
---
# <a name="tls-support-in-iot-hub"></a>IoT Hub'da TLS desteği

IoT Hub, IoT aygıtlarından ve hizmetlerinden gelen bağlantıları güvence altına almak için Aktarım Katmanı Güvenliği'ni (TLS) kullanır. TLS protokolünün üç sürümü şu anda desteklenir: 1.0, 1.1 ve 1.2 sürümleri.

TLS 1.0 ve 1.1 eski olarak kabul edilir ve amortisman için planlanmıştır. Daha fazla bilgi [için, IoT Hub için TLS 1.0 ve 1.1'i amortismana](iot-hub-tls-deprecating-1-0-and-1-1.md)alma bölümüne bakın. IoT Hub'a bağlanırken TLS 1.2'yi tercih edilen TLS sürümü olarak kullanmanız önerilir.

## <a name="restrict-connections-to-tls-12-in-your-iot-hub-resource"></a>IoT Hub kaynağınızdaki bağlantıları TLS 1,2 ile sınırlandırın

Daha fazla güvenlik için, IoT Hub'larınızı *yalnızca* TLS sürüm 1.2 kullanan istemci bağlantılarına izin verecek şekilde yapılandırmanız ve [önerilen şifrelerin](#recommended-ciphers)kullanımını zorlamanız önerilir.

Bu amaçla, [desteklenen bölgelerden](#supported-regions) herhangi birinde yeni bir IoT `minTlsVersion` Hub'ı sağlama ve özelliği `1.2` Azure Kaynak Yöneticisi şablonunuzun IoT hub kaynak belirtimine ayarlayın:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Devices/IotHubs",
            "apiVersion": "2020-01-01",
            "name": "<provide-a-valid-resource-name>",
            "location": "<any-of-supported-regions-below>",
            "properties": {
                "minTlsVersion": "1.2"
            },
            "sku": {
                "name": "<your-hubs-SKU-name>",
                "tier": "<your-hubs-SKU-tier>",
                "capacity": 1
            }
        }
    ]
}
```

Bu yapılandırmayı kullanan oluşturulan IoT Hub kaynağı, TLS 1.0 ve 1.1 sürümlerini kullanarak bağlanmaya çalışan aygıt ve servis istemcilerini reddeder. Benzer şekilde, istemci HELLO mesajı [önerilen şifrelerin](#recommended-ciphers)herhangi birini listelemiyorsa TLS el sıkışma sıyrık reddedilecektir.

> [!NOTE]
> Özellik `minTlsVersion` salt okunur ve IoT Hub kaynağınız oluşturulduktan sonra değiştirilemez. Bu *nedenle, tüm* IoT cihazlarınızın ve hizmetlerinizin TLS 1.2 ve önerilen [şifrelerle](#recommended-ciphers) önceden uyumlu olduğunu doğrubir şekilde test etmeniz ve doğrulamanız esastır.

### <a name="supported-regions"></a>Desteklenen bölgeler

TLS 1.2 kullanımını gerektiren IoT Hub'ları aşağıdaki bölgelerde oluşturulabilir:

* Doğu ABD
* Orta Güney ABD
* Batı ABD 2
* US Gov Arizona
* US Gov Virginia

> [!NOTE]
> Başarısız olduktan sonra, `minTlsVersion` IoT Hub'ınızın özelliği, başarısız lık sonrası coğrafi eşleştirilmiş bölgede etkili olmaya devam edecektir.

### <a name="recommended-ciphers"></a>Önerilen şifreler

Yalnızca TLS 1.2 kabul edecek şekilde yapılandırılan IoT Hub'ları da aşağıdaki önerilen şifrelerin kullanımını zorunlu kılabilir:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`

### <a name="use-tls-12-in-your-iot-hub-sdks"></a>TLS 1.2'yi IoT Hub SDK'larınızda kullanın

TLS 1.2'yi yapılandırmak ve IoT Hub istemci SDK'larında şifrelere izin vermek için aşağıdaki bağlantıları kullanın.

| Dil | TLS 1.2'yi destekleyen sürümler | Belgeler |
|----------|------------------------------------|---------------|
| C        | Etiket 2019-12-11 veya yeni            | [Bağlantı](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Sürüm 2.0.0 veya daha yeni             | [Bağlantı](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Sürüm 1.21.4 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Sürüm 1.19.0 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Sürüm 1.12.2 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_Node_SDK_IoT) |


### <a name="use-tls-12-in-your-iot-edge-setup"></a>IoT Edge kurulumunuzda TLS 1.2'yi kullanın

IoT Edge aygıtları, IoT Hub ile iletişim kurarken TLS 1.2 kullanacak şekilde yapılandırılabilir. Bu amaçla, [IoT Edge dokümantasyon sayfasını](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md)kullanın.