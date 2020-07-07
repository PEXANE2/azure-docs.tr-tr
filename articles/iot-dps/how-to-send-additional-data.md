---
title: Cihaz ve Azure cihaz sağlama hizmeti arasında yük aktarma
description: Bu belgede cihaz ve cihaz sağlama hizmeti (DPS) arasında bir yükün nasıl aktarılacağı açıklanmaktadır
author: menchi
ms.author: menchi
ms.date: 02/11/2020
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: de6bb3fe0879a69467283e93a04a355876a02cba
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80246696"
---
# <a name="how-to-transfer-a-payload-between-device-and-dps"></a>Cihaz ve DPS arasında yük aktarma
Bazen DPS, cihazların doğru IoT Hub doğru şekilde sağlaması için cihazlardan daha fazla veriye ihtiyaç duyuyor ve bu verilerin cihaz tarafından sağlanması gerekiyor. Bunun tersine, DPS istemci tarafı LOGI 'yi kolaylaştırmak için cihaza veri döndürebilir. 

## <a name="when-to-use-it"></a>Kullanılması gereken durumlar
Bu özellik, [özel ayırma](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies)geliştirmesi olarak kullanılabilir. Örneğin, cihazları, insan müdahalesi olmadan cihaz modeline göre ayırmak istiyorsunuz. Bu durumda, [özel tahsisi](https://docs.microsoft.com/azure/iot-dps/how-to-use-custom-allocation-policies)kullanacaksınız. Cihazı, [kayıt cihazı çağrısının](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice)bir parçası olarak model bilgilerini bildirmek üzere yapılandırabilirsiniz. DPS, cihazın yükünü özel ayırma Web kancasına geçilecektir. Ve işleviniz, cihaz modeli bilgilerini aldığında bu cihazın hangi IoT Hub gideceğine karar verebilir. Benzer şekilde, Web kancası cihaza bazı veriler döndürmesini istiyorsa, verileri Web kancası yanıtında bir dize olarak geri iletir.  

## <a name="device-sends-data-payload-to-dps"></a>Cihaz, veri yükünü DPS 'e gönderir
Cihazınız, DPS 'ye bir [yazmaç cihaz çağrısı](https://docs.microsoft.com/rest/api/iot-dps/runtimeregistration/registerdevice) gönderirken, kayıt çağrısı, gövdedeki diğer alanları alacak şekilde geliştirilebilir. Gövde aşağıdaki gibi görünür: 
   ```
   { 
       “registrationId”: “mydevice”, 
       “tpm”:                
       { 
           “endorsementKey”: “stuff”, 
           “storageRootKey”: “things” 
       }, 
       “payload”: “your additional data goes here. It can be nested JSON.” 
    } 
   ```

## <a name="dps-returns-data-to-the-device"></a>DPS cihaza veri döndürüyor
Özel ayırma ilkesi Web kancası cihaza bazı veriler döndürmesini istiyorsa, verileri Web kancası yanıtında bir dize olarak geri iletir. Değişiklik aşağıdaki yük bölümünde bulunur. 
   ```
   { 
       "iotHubHostName": "sample-iot-hub-1.azure-devices.net", 
       "initialTwin": { 
           "tags": { 
               "tag1": true 
               }, 
               "properties": { 
                   "desired": { 
                       "tag2": true 
                    } 
                } 
            }, 
        "payload": "whatever is returned by the webhook" 
    } 
   ```

## <a name="sdk-support"></a>SDK desteği
Bu özellik C, C#, JAVA ve Node.js [Istemci SDK](https://docs.microsoft.com/azure/iot-dps/)'lerinde kullanılabilir.  

## <a name="next-steps"></a>Sonraki adımlar
* Azure IoT Hub ve Azure IoT Hub cihaz sağlama hizmeti için [Azure ıOT SDK 'sını]( https://github.com/Azure/azure-iot-sdks) kullanarak geliştirme
