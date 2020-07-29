---
title: Azure IoT Hub TLS desteği
description: IoT Hub ile iletişim kuran cihazlar ve hizmetler için güvenli TLS bağlantıları kullanmanın en iyi yöntemleri
services: iot-hub
author: jlian
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: jlian
ms.openlocfilehash: 8c52037684215d1672ed813389d0bbace9a03e42
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85080613"
---
# <a name="tls-support-in-iot-hub"></a>IoT Hub 'de TLS desteği

IoT Hub IoT cihazlarından ve hizmetlerinden gelen bağlantıları güvenli hale getirmek için Aktarım Katmanı Güvenliği 'ni (TLS) kullanır. TLS protokolünün üç sürümü şu anda desteklenmektedir. 1,0, 1,1 ve 1,2 sürümleri.

TLS 1,0 ve 1,1 eski olarak değerlendirilir ve kullanımdan kaldırma için planlanmaktadır. Daha fazla bilgi için bkz. [IoT Hub IÇIN TLS 1,0 ve 1,1 kullanımdan](iot-hub-tls-deprecating-1-0-and-1-1.md)kaldırılıyor. IoT Hub bağlanırken tercih edilen TLS sürümü olarak TLS 1,2 kullanmanız önemle önerilir.

## <a name="tls-12-enforcement-available-in-select-regions"></a>TLS 1,2 zorlaması seçim bölgelerinde kullanılabilir

Ek güvenlik için IoT Hub 'larınızı *yalnızca* TLS sürüm 1,2 kullanan istemci bağlantılarına izin verecek şekilde yapılandırın ve [Önerilen şifrelemelerin](#recommended-ciphers)kullanımını zorunlu kılabilirsiniz. Bu özellik yalnızca şu bölgelerde desteklenir:

* Doğu ABD
* Orta Güney ABD
* Batı ABD 2
* US Gov Arizona
* US Gov Virginia

Bu amaçla, desteklenen bölgelerin herhangi birinde yeni bir IoT Hub sağlayın ve `minTlsVersion` özelliğini `1.2` Azure Resource Manager şablonunuzun IoT Hub kaynak belirtiminde olarak ayarlayın:

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

Bu yapılandırmayı kullanan oluşturulan IoT Hub kaynak, 1,0 ve 1,1 TLS sürümlerini kullanarak bağlanmayı deneyen cihaz ve hizmet istemcilerini reddeder. Benzer şekilde, istemci Merhaba iletisi [Önerilen şifrelemelerin](#recommended-ciphers)hiçbirini LISTELEMEZ ise TLS el sıkışması reddedilir.

> [!NOTE]
> `minTlsVersion`Özelliği salt okunurdur ve IoT Hub kaynağınız oluşturulduktan sonra değiştirilemez. Bu nedenle, *Tüm* IoT cihazlarınızın ve hizmetlerinizin TLS 1,2 ile uyumlu olduğunu ve [Önerilen şifrelemeleri](#recommended-ciphers) önceden test etmek ve doğrulamak önemlidir.
> 
> `minTlsVersion`Yük devretme sonrasında, IoT Hub özelliği, coğrafi olarak eşleştirilmiş bölgenin yük devretmesinde etkili olmaya devam edecektir.

## <a name="recommended-ciphers"></a>Önerilen şifrelemeler

Yalnızca TLS 1,2 ' i kabul edecek şekilde yapılandırılan IoT Hub 'Ları, aşağıdaki önerilen şifrelemelerin kullanımını da zorunlu tutar:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`

TLS 1,2 zorlaması için yapılandırılmayan IoT Hub 'Ları için, TLS 1,2 hala aşağıdaki şifrelemeler ile çalışmaktadır:

* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_DHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_DHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_RSA_WITH_AES_256_CBC_SHA256`
* `TLS_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_AES_256_CBC_SHA`
* `TLS_RSA_WITH_AES_128_CBC_SHA`
* `TLS_RSA_WITH_3DES_EDE_CBC_SHA`

## <a name="use-tls-12-in-your-iot-hub-sdks"></a>IoT Hub SDK 'larınız için TLS 1,2 kullanın

IoT Hub istemci SDK 'lerinde TLS 1,2 ve izin verilen şifrelemeleri yapılandırmak için aşağıdaki bağlantıları kullanın.

| Dil | TLS 1,2 destekleyen sürümler | Belgeler |
|----------|------------------------------------|---------------|
| C        | Etiket 2019-12-11 veya üzeri            | [Bağlantı](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Sürüm 2.0.0 veya daha yeni             | [Bağlantı](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Sürüm 1.21.4 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Sürüm 1.19.0 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Sürüm 1.12.2 veya daha yeni            | [Bağlantı](https://aka.ms/Tls_Node_SDK_IoT) |


## <a name="use-tls-12-in-your-iot-edge-setup"></a>IoT Edge kurulumunda TLS 1,2 kullanma

IoT Edge cihazlar, IoT Hub iletişim kurarken TLS 1,2 kullanacak şekilde yapılandırılabilir. Bu amaçla [IoT Edge belge sayfasını](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md)kullanın.