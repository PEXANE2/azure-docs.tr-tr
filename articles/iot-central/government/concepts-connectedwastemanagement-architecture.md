---
title: Azure IoT Central ile oluşturulmuş bağlı çöp yönetimi çözümü için başvuru mimarisi | Microsoft Docs
description: Azure IoT Central ile oluşturulmuş bağlı bir çöp yönetimi çözümü için kavramlar öğrenin.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 763fb82e0725e5a98112af3a79856c3703a5c2c3
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "77016094"
---
# <a name="connected-waste-monitoring-reference-architecture"></a>Bağlı atık izleme referans mimarisi 



Bağlı bir çöp yönetimi çözümü, **Azure IoT Central uygulama şablonu** kullanılarak bir başlangıç olarak başlangıç IoT uygulaması olarak oluşturulabilir. Bu makale uçtan uca çözüm oluşturma konusunda üst düzey bir başvuru mimarisi kılavuzu sağlar. 

![Bağlı çöp yönetimi mimarisi](./media/concepts-connectedwastemanagement-architecture/concepts-connectedwastemanagement-architecture1.png)


Kavramlar:

1. Cihazlar ve bağlantı  
1. IoT Central 
2. Genişletilebilirlik ve tümleştirmeler
3. İş uygulamaları

Genellikle su tüketim izleme çözümünde bir parçayı oynayan önemli bileşenlere göz atalım.

## <a name="devices-and-connectivity"></a>Cihazlar ve bağlantı 
Çöp kutuları gibi açık ortamlarda kullanılan cihazlar, üçüncü taraf ağ operatörü aracılığıyla düşük güç geniş alan ağları (LPWAN) aracılığıyla bağlanabilir. Bu tür cihazlarda, cihaz verilerinizi Azure IoT Central IoT uygulamanıza göndermek için [azure IoT Central cihaz Köprüsü](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge) ' ne yararlanabilirsiniz. Alternatif olarak, IP özellikli cihaz ağ geçitleriniz olabilir ve IoT Central doğrudan bağlanabilir.

## <a name="iot-central"></a>IoT Central 
Azure IoT Central, IoT çözümünüzde hızla çalışmaya başlamanızı sağlayan bir IoT uygulama platformudur. Çözümünüzü, üçüncü taraf hizmetlerle markalayabilmeniz, özelleştirebilir ve tümleştirebilirsiniz.
Akıllı su cihazlarınızı IoT Central 'e bağladığınızda, cihaz komutu ve denetim, izleme ve uyarma, yerleşik RBAC, yapılandırılabilir Öngörüler panoları ve genişletilebilirlik seçenekleriyle kullanıcı arabirimi elde edersiniz. 

## <a name="extensibility-and-integrations"></a>Genişletilebilirlik ve tümleştirmeler 
IoT uygulamanızı IoT Central ve isteğe bağlı olarak genişletebilirsiniz:
* IoT Central uygulamadan sürekli veri dışa aktarma aracılığıyla eğitim makinesi öğrenimi modelleri gibi gelişmiş analizler için IoT verilerinizi dönüştürün ve tümleştirin. 
* IoT Central uygulamadan Microsoft Flow veya Web kancalarını kullanarak eylemleri tetikleyerek diğer sistemlerdeki iş akışlarını otomatikleştirin
* Program aracılığıyla API 'Leri IoT Central aracılığıyla IoT Central IoT uygulamanıza erişin.

## <a name="business-applications"></a>İş uygulamaları 
IoT verileri, bir çöp yardımcı programı dahilinde çeşitli iş uygulamalarını desteklemek için kullanılabilir. IoT Central bağlı çöp yönetimi uygulamanızı alan hizmetleriyle bağlamayı öğrenmek için, [Dynamics 365 alan hizmetleriyle tümleştirme](./how-to-configure-connected-field-services.md) hakkında öğreticiyi izleyin. 

## <a name="next-steps"></a>Sonraki adımlar
* [Bağlı bir çöp yönetimi](./tutorial-connected-waste-management.md) IoT Central uygulaması oluşturmayı öğrenin
* [IoT Central kamu şablonları](./overview-iot-central-government.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi edinmek için bkz. [IoT Central genel bakış](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central)

