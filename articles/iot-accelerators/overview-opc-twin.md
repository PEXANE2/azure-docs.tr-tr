---
title: OPC Twin Nedir - Azure | Microsoft Dokümanlar
description: Bu makalede, OPC Twin'e genel bir bakış sağkalmaktadır. OPC Twin, REST API'leri aracılığıyla endüstriyel cihazların keşfini, kaydını ve uzaktan kontrolünü sağlar.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 91448f55f0ebb88ba6c685b960ece9d91cb98e25
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73826232"
---
# <a name="what-is-opc-twin"></a>OPC Twin nedir?

OPC Twin, bulutu ve fabrika ağını bağlamak için Azure IoT Edge ve IoT Hub'ı kullanan mikro hizmetlerden oluşur. OPC Twin, REST API'leri aracılığıyla endüstriyel cihazların keşfini, kaydını ve uzaktan kontrolünü sağlar. OPC Twin bir OPC Birleşik Mimari (OPC UA) SDK gerektirmez, programlama dili agnostik ve sunucusuz bir iş akışına dahil edilebilir. Bu makalede, birkaç OPC İkiz kullanım örnekleri açıklanmaktadır.

## <a name="discovery-and-control"></a>Bulma ve kontrol
OPC Twin'i basit bir şekilde keşif ve kayıt için kullanabilirsiniz.

### <a name="simple-discovery-and-registration"></a>Basit keşif ve kayıt
OPC Twin, fabrika operatörlerinin fabrika ağını tarayıp OPC UA sunucularının keşfedilmesine ve kaydedilmesine olanak tanır. Alternatif olarak, fabrika operatörleri bilinen bir bulma URL'si kullanarak OPC UA aygıtlarını el ile kaydedebilir. Örneğin, fabrika zeminine OPC Twin modülü ile IoT Edge ağ geçidi yüklendikten sonra tüm OPC UA aygıtlarına bağlanmak için, fabrika operatörü uzaktan ağın tadına tetikleyebilir ve tüm OPC UA sunucularını görsel olarak görebilir. 

### <a name="simple-control"></a>Basit kontrol
OPC Twin, fabrika operatörlerinin olaylara tepki göstermesine ve fabrika zemin makinelerini buluttan otomatik olarak veya manuel olarak yeniden yapılandırmasına olanak tanır. OPC Twin, OPC UA sunucusundaki hizmetleri çağırmak, adres alanına göz atmak ve değişkenleri okumak/yazmak ve yöntemleri yürütmek için REST API'leri sağlar. Örneğin, bir kazan üretim hattını kontrol etmek için kpi sıcaklığını kullanır. Sıcaklık sensörü, OPC Publisher'ı kullanarak veri değişimini yayınlar. Fabrika operatörü, sıcaklığın eşiğe ulaştığı na dair uyarı alır. Üretim hattı OPC Twin ile otomatik olarak soğur. Fabrika operatörü soğuması bildirilir.

## <a name="authentication"></a>Kimlik doğrulaması
OPC Twin'i kimlik doğrulama ve basit bir geliştirici deneyimi için basit olarak kullanabilirsiniz.

### <a name="simple-authentication"></a>Basit kimlik doğrulama 
OPC Twin, Azure Active Directory (AAD) tabanlı kimlik doğrulamave denetimini uçtan uca kullanır. Örneğin, OPC Twin, bir operatörün makinede ne performans gösterdiğini belirlemek için uygulamanın OPC Twin'in üzerine inşa edilmesini sağlar. Makine tarafında, OPC UA denetimi yoluyla. Bulut tarafında, bu, REST API'de değişmez bir istemci denetim günlüğü ve AAD kimlik doğrulaması depolama yoluyla yapılır.

### <a name="simple-developer-experience"></a>Basit geliştirici deneyimi 
OPC Twin, REST API'leri aracılığıyla herhangi bir programlama dilinde yazılmış uygulamalarla kullanılabilir. Geliştiriciler bir OPC UA istemcisini bir çözüme entegre ettikçe, OPC UA SDK bilgisi gerekli değildir. OPC Twin, durumsuz, sunucusuz mimarilere sorunsuz bir şekilde entegre edilebilir. Örneğin, bir alarm ve olay panosu için bir uygulama geliştiren tam bir yığın web geliştiricisi, C, C#veya tam OPC UA yığını uygulaması bilgisi olmadan OPC Twin kullanarak JavaScript veya TypeScript'teki olaylara yanıt verme mantığını yazabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Şimdi OPC Twin ve kullanımları hakkında öğrendim, burada önerilen bir sonraki adımdır:

> [!div class="nextstepaction"]
> [OPC Vault nedir?](overview-opc-vault.md)
