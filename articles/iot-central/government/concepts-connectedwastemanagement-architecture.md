---
title: Azure IoT Central ile oluşturulmuş bağlantılı atık yönetimi çözümü için referans mimarisi| Microsoft Dokümanlar
description: Azure IoT Central ile oluşturulmuş bağlantılı bir atık yönetimi çözümü için kavramları öğrenin.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 763fb82e0725e5a98112af3a79856c3703a5c2c3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77016094"
---
# <a name="connected-waste-monitoring-reference-architecture"></a>Bağlı atık izleme referans mimarisi 



Bağlı bir atık yönetimi çözümü, kick starter IoT uygulaması olarak **Azure IoT Central uygulama şablonu** kullanılarak oluşturulabilir. Bu makalede, uçtan uca çözüm oluşturma konusunda üst düzey bir başvuru mimarisi kılavuzu sağlar. 

![Bağlı atık yönetim mimarisi](./media/concepts-connectedwastemanagement-architecture/concepts-connectedwastemanagement-architecture1.png)


Kavramlar:

1. Cihazlar ve bağlantı  
1. IoT Central 
2. Genişletilebilirlik ve entegrasyonlar
3. İş uygulamaları

Genellikle bir su tüketimi izleme çözümünde rol oynayan temel bileşenlere bir göz atalım.

## <a name="devices-and-connectivity"></a>Cihazlar ve bağlantı 
Atık kutuları gibi açık ortamlarda kullanılan aygıtlar, düşük güçlü geniş alan ağları (LPWAN) aracılığıyla, üçüncü taraf bir ağ operatörü aracılığıyla bağlanabilir. Bu tür aygıtlar için, aygıt verilerinizi Azure IoT Central'daki IoT uygulamanıza göndermek için [Azure IoT Merkezi Aygıt Köprüsü'nü](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge) kullanabilirsiniz. Alternatif olarak, IP özellikli ve doğrudan IoT Central'a bağlanabilen aygıt ağ geçitleri olabilir.

## <a name="iot-central"></a>IoT Central 
Azure IoT Central, IoT çözümünüze hızlı bir şekilde başlamanızı ve çalıştırmanızı sağlayan bir IoT Uygulaması platformudur. Çözümünüzü markalayabilir, özelleştirebilir ve üçüncü taraf hizmetlerle bütünleştirebilirsiniz.
Akıllı su cihazlarınızı IoT Central'a bağladıktan sonra, aygıt komutu ve denetimi, izleme ve uyarı, yerleşik RBAC ile kullanıcı arabirimi, yapılandırılabilir öngörüler panoları ve genişletilebilirlik seçenekleri elde elabilirsiniz. 

## <a name="extensibility-and-integrations"></a>Genişletilebilirlik ve entegrasyonlar 
IoT uygulamanızı IoT Central'da ve isteğe bağlı olarak genişletebilirsiniz:
* IoT Central uygulamasından sürekli veri ihracatı yoluyla, ioT verilerinizi gelişmiş analitik (örneğin eğitim makinesi öğrenme modelleri) için dönüştürün ve entegre edin. 
* Microsoft Flow veya IoT Central uygulamasından webhooks kullanarak eylemleri tetikleyerek diğer sistemlerdeki iş akışlarını otomatikleştirin
* IoT Central'daki IoT Uygulamanıza IoT Merkezi API'leri aracılığıyla programlı olarak erişin.

## <a name="business-applications"></a>İş uygulamaları 
IoT verileri, bir atık programı içindeki çeşitli iş uygulamalarına güç sağlamak için kullanılabilir. IoT Central bağlantılı atık yönetimi uygulamanızı saha hizmetleriyle nasıl birleştiracağınızı öğrenmek için [Dynamics 365 Saha Hizmetleri ile nasıl entegre olunacağı](./how-to-configure-connected-field-services.md) yla ilgili öğreticiyi takip edin 

## <a name="next-steps"></a>Sonraki adımlar
* Bağlantılı bir atık yönetimi IoT Merkezi uygulaması [oluşturmayı](./tutorial-connected-waste-management.md) öğrenin
* [IoT Merkezi yönetim şablonları](./overview-iot-central-government.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi edinmek için [IoT Central genel bakış](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central)

