---
title: Azure IoT Hub TLS desteği
description: IoT Hub ile iletişim kuran cihazlar ve hizmetler için güvenli TLS bağlantıları kullanmanın en iyi yöntemleri
services: iot-hub
author: rezasherafat
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 01/10/2020
ms.author: rezas
ms.custom: Azure IoT Hub TLS
ms.openlocfilehash: eb6b04a476ac6100962e1103af37d75b719dd546
ms.sourcegitcommit: f34165bdfd27982bdae836d79b7290831a518f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921255"
---
# <a name="tls-support-in-iot-hub"></a>IoT Hub 'de TLS desteği

IoT Hub IoT cihazlarından ve hizmetlerinden gelen bağlantıları güvenli hale getirmek için Aktarım Katmanı Güvenliği 'ni (TLS) kullanır. TLS protokolünün üç sürümü şu anda desteklenmektedir. 1,0, 1,1 ve 1,2 sürümleri.

TLS 1,0 ve 1,1 eski olarak değerlendirilir ve [kullanımdan kaldırma için planlanmaktadır](./tls-1.2-everywhere.md). Bu nedenle IoT Hub bağlanırken tercih edilen TLS sürümü olarak TLS 1,2 kullanılması önemle önerilir.


## <a name="restrict-connections-to-tls-12-in-your-iot-hub-resource"></a>IoT Hub kaynağınız içindeki TLS 1,2 bağlantılarını kısıtla

Ek güvenlik için, IoT Hub 'larınızı _yalnızca_ TLS sürüm 1,2 kullanan istemci bağlantılarına izin verecek şekilde yapılandırmanız ve [Önerilen şifrelemelerin](#recommended-ciphers)kullanımını zorunlu hale getirilmesi önerilir.

Bu amaçla, [desteklenen bölgelerin](#supported-regions) herhangi birinde yeni bir IoT Hub sağlayın ve `minTlsVersion` özelliğini Azure Resource Manager şablonunuzun IoT Hub kaynak belirtiminde `1.2` olarak ayarlayın:

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

`minTlsVersion` özelliğinin salt okunurdur ve IoT Hub kaynağınız oluşturulduktan sonra değiştirilemez. Bu nedenle, _Tüm_ IoT cihazlarınızın ve hizmetlerinizin TLS 1,2 ile uyumlu olduğunu ve [Önerilen şifrelemeleri](#recommended-ciphers) önceden test etmek ve doğrulamak önemlidir.


### <a name="supported-regions"></a>Desteklenen bölgeler

TLS 1,2 kullanımını gerektiren IoT Hub 'Ları aşağıdaki bölgelerde oluşturulabilir:

* Doğu ABD
* Güney Orta ABD
* Batı ABD 2

> [!NOTE]
> Yük devretme sonrasında, IoT Hub `minTlsVersion` özelliği, coğrafi olarak eşleştirilmiş bölgenin yük devretmesinde etkili olmaya devam edecektir.



### <a name="recommended-ciphers"></a>Önerilen şifrelemeler

Yalnızca TLS 1,2 ' i kabul edecek şekilde yapılandırılan IoT Hub 'Ları, aşağıdaki önerilen şifrelemelerin kullanımını da zorunlu tutar:

* `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
* `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
* `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`


### <a name="use-tls-12-in-your-iot-hub-sdks"></a>IoT Hub SDK 'larınız için TLS 1,2 kullanın

IoT Hub istemci SDK 'lerinde TLS 1,2 ve izin verilen şifrelemeleri yapılandırmak için aşağıdaki bağlantıları kullanın.

| Dil | TLS 1,2 destekleniyor | Belgeler |
|----------|-------------------|---------------|
| C        | Evet               | [Bağlantı](https://aka.ms/Tls_C_SDK_IoT) |
| Python   | Evet               | [Bağlantı](https://aka.ms/Tls_Python_SDK_IoT) |
| C#       | Evet               | [Bağlantı](https://aka.ms/Tls_CSharp_SDK_IoT) |
| Java     | Evet               | [Bağlantı](https://aka.ms/Tls_Java_SDK_IoT) |
| NodeJS   | Evet               | [Bağlantı](https://aka.ms/Tls_Node_SDK_IoT) |


### <a name="use-tls-12-in-your-iot-edge-setup"></a>IoT Edge kurulumunda TLS 1,2 kullanma

IoT Edge cihazlar, IoT Hub iletişim kurarken TLS 1,2 kullanacak şekilde yapılandırılabilir. Bu amaçla [IoT Edge belge sayfasını](https://github.com/Azure/iotedge/blob/master/edge-modules/edgehub-proxy/README.md)kullanın.