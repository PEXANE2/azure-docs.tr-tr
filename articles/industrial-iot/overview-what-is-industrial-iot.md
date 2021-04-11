---
title: Azure endüstriyel IoT 'ye Genel Bakış
description: Bu makalede endüstriyel IoT 'ye genel bakış sunulmaktadır. Bu, IIoT 'de atölye bağlantısı ve güvenlik bileşenlerini açıklamaktadır.
author: jehona-m
ms.author: jemorina
ms.service: industrial-iot
ms.topic: overview
ms.date: 3/22/2021
ms.openlocfilehash: 940391d26b5a8455fef01c8094cd08e05ab51290
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104787954"
---
# <a name="what-is-industrial-iot-iiot"></a>Endüstriyel IoT (IIoT) nedir?

IIoT (endüstriyel Nesnelerin İnterneti), üretim sektöründe IoT uygulaması aracılığıyla sektörel verimliliği geliştirir.

![Endüstriyel IoT](media/overview-what-is-Industrial-IoT/icon-255-px.png)

## <a name="improve-industrial-efficiencies"></a>Endüstriyel verimliliği artırmak
Azure endüstriyel IoT ile operasyonel üretkenliğinizi ve karlılığını geliştirin. Yerinde varlıklara erişmek için zaman alan işlem süresini azaltın. Fabrika katında zaten çalışan makineleriniz dahil olmak üzere, bulut ortamında endüstriyel ekipman ve cihazlarınızı bağlayın ve izleyin. Tüm sitenin performansını artırmanıza yardımcı olan Öngörüler için IoT Verilerinizi çözümleyin.

## <a name="industrial-iot-components"></a>Endüstriyel IoT bileşenleri

**IoT Edge cihazlar** IoT Edge bir cihaz Edge çalışma zamanı ve kenar modülleriyle oluşur. 
- *Edge modülleri* , OPC yayımcısı ve OPC ikizi gibi en küçük hesaplama birimi olan Docker kapsayıcılarıdır. 
- *Edge cihazı* , OPC UA sunucusu ve bulutta IoT Hub arasında ortalama işlevi gören bu tür modülleri dağıtmak için kullanılır. IoT Edge hakkında daha fazla bilgi [burada](https://azure.microsoft.com/services/iot-edge/)verilmiştir.

**IoT Hub** IoT Hub IoT uygulaması ile yönettiği cihazlar arasındaki çift yönlü iletişim için bir merkezi ileti hub 'ı görevi görür. Bu, açık kaynak SDK 'Ları ve birden çok protokolü destekleyen bir hizmet olarak açık ve esnek bir bulut platformudur. [IoT Hub hakkında](https://azure.microsoft.com/services/iot-hub/)daha fazla bilgi edinin.

**Endüstriyel Edge modülleri**
- *OPC yayımcısı*: opc yayımcısı IoT Edge içinde çalışır. OPC UA sunucularına bağlanır ve OPC UA "pub/Sub" biçimindeki bu sunuculardan JSON kodlu telemetri verilerini Azure IoT Hub yayımlar. Azure IoT Hub istemci SDK 'Sı tarafından desteklenen tüm aktarım protokolleri, yani HTTPS, AMQP ve MQTT kullanılabilir.
- *OPC ikizi*: OPC ikizi, bulutu ve Factory ağını bağlamak için Azure IoT Edge ve IoT Hub kullanan mikro hizmetlerden oluşur. OPC Ikizi, REST API 'Leri aracılığıyla endüstriyel cihazların keşif, kayıt ve uzaktan denetimini sağlar. OPC Ikizi, OPC Birleşik mimarisi (OPC UA) SDK 'Sı gerektirmez. Programlama dili belirsiz olur ve sunucusuz bir iş akışına dahil edilebilir.
- *Bulma*: Bulucu kimliğiyle temsil edilen bulma modülü, OPC UA sunucu bulmayı içeren, uç üzerinde bulma hizmetleri sağlar. Bulma yapılandırıldıysa ve etkinleştirilirse, modül IoT Edge ve ekleme hizmeti için telemetri yolu IoT Hub aracılığıyla bir tarama araştırması sonuçlarını gönderir. Hizmet sonuçları işler ve kayıt defterindeki tüm ilgili kimlikleri günceller.


**Azure Ile endüstriyel varlıklarınızı bulun, kaydedin ve yönetin** Azure endüstriyel IoT, Tesis işleçleri 'nin bir fabrika ağında OPC UA etkin sunucularını bulmasına ve Azure IoT Hub 'e kaydolmalarına olanak tanır. Operasyon personeli, dünyanın herhangi bir yerinden fabrika katında olaylara abone olabilir ve tepki verebilir. Mikro hizmetler 'in REST API 'Leri OPC UA Hizmetleri kenar tarafını yansıtır. Azure Active Directory (AAD) tarafından desteklenen OAUTH kimlik doğrulaması ve yetkilendirme kullanılarak güvenli hale getirilir. Bu, bulut uygulamalarınızın sunucu adres alanlarına gözatmasını veya okuma/yazma değişkenlerini ve HTTPS ve basit OPC UA JSON yüklerini kullanarak yöntemleri yürütmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Endüstriyel IoT 'nin ne olduğunu öğrendiğinize göre, endüstriyel IoT Platformu ve OPC yayımcısı hakkında bilgi edinebilirsiniz:

> [!div class="nextstepaction"]
> [Endüstriyel IoT Platformu nedir?](overview-what-is-industrial-iot-platform.md)

> [!div class="nextstepaction"]
> [OPC yayımcısı nedir?](overview-what-is-opc-publisher.md)