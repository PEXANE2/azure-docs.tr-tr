---
title: Azure IoT Central ile oluşturulmuş su kalitesi izleme çözümü için referans mimarisi| Microsoft Dokümanlar
description: Azure IoT Central ile oluşturulmuş bir su kalitesi izleme çözümü için kavramları öğrenin.
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 0c4b13c56a68205195bd5ad4b696d9e01786a8dd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77016553"
---
# <a name="water-quality-monitoring-reference-architecture"></a>Su kalitesi izleme referans mimarisi 



Bir kick starter IoT uygulaması olarak **Azure IoT Central uygulama şablonu** ile su kalitesi izleme çözümleri oluşturulabilir. Bu makalede, uçtan uca çözüm oluşturma konusunda üst düzey bir başvuru mimarisi kılavuzu sağlar. 


![Su kalitesi izleme mimarisi](./media/concepts-waterqualitymonitoring-architecture/concepts-waterqualitymonitoring-architecture1.png)

Kavramlar:

1. Cihazlar ve bağlantı  
1. IoT Central 
2. Genişletilebilirlik ve entegrasyonlar
3. İş uygulamaları

Genellikle bir su kalitesi izleme çözümünde rol oynayan temel bileşenlere bir göz atalım.

## <a name="devices-and-connectivity"></a>Cihazlar ve bağlantı 
Bu bölümde, genellikle akıllı su cihazları olarak su kalitesi izleme veya su tüketimi izleme kullanılan cihazlar atıfta bulunacaktır. Akıllı su cihazları akış ölçerler, su kalitesi monitörleri, akıllı vanalar, sızıntı dedektörleri ve benzeri olabilir.

Akıllı su çözümlerinde kullanılan cihazlar genellikle düşük güçlü geniş alan ağları (LPWAN) üzerinden, üçüncü taraf bir ağ operatörü aracılığıyla bağlanır. Bu tür aygıtlar için, aygıt verilerinizi Azure IoT Central'daki IoT uygulamanıza göndermek için [Azure IoT Merkezi Aygıt Köprüsü'nden](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge) yararlanabilirsiniz. Alternatif olarak, IP özellikli ve doğrudan IoT Central'a bağlanabilen aygıt ağ geçitleri olabilir.

## <a name="iot-central"></a>IoT Central 
Azure IoT Central, IoT çözümünüze hızlı bir şekilde başlamanızı ve çalıştırmanızı sağlayan bir IoT Uygulaması platformudur. Çözümünüzü markalayabilir, özelleştirebilir ve üçüncü taraf hizmetlerle bütünleştirebilirsiniz.
Akıllı su cihazlarınızı IoT Central'a bağladıktan sonra, aygıt komutu ve denetimi, izleme ve uyarı, yerleşik RBAC ile kullanıcı arabirimi, yapılandırılabilir öngörüler panoları ve genişletilebilirlik seçenekleri elde elabilirsiniz. 

## <a name="extensibility-and-integrations"></a>Genişletilebilirlik ve entegrasyonlar 
IoT uygulamanızı IoT Central'da ve isteğe bağlı olarak genişletebilirsiniz:
* IoT Central uygulamasından sürekli veri ihracatı yoluyla, örneğin eğitim makinesi öğrenme modelleri gibi gelişmiş analitik için IoT verilerinizi dönüştürün ve tümleştirin
* Microsoft Flow veya IoT Central uygulamasından webhooks kullanarak eylemleri tetikleyerek diğer sistemlerdeki iş akışlarını otomatikleştirin
* IoT Central'daki IoT Merkezi'ndeki IoT uygulamanıza IoT Merkezi API'leri aracılığıyla programlı olarak erişin

## <a name="business-applications"></a>İş uygulamaları 
IoT verileri, bir su yardımcı programı içindeki çeşitli iş uygulamalarına güç sağlamak için kullanılabilir. IoT Merkezi su kalitesi izleme uygulamanızı saha servislerine nasıl bağlayabileceğinizi öğrenmek için [Dynamics 365 Saha Hizmetleri ile nasıl entegre olunacağı](./how-to-configure-connected-field-services.md)yla ilgili makaleyi takip edin. 


## <a name="next-steps"></a>Sonraki adımlar
* Su kalitesi izleme IoT Central uygulamasını nasıl [oluşturabilirsiniz](./tutorial-water-quality-monitoring.md) öğrenin
* [IoT Merkezi yönetim şablonları](./overview-iot-central-government.md) hakkında daha fazla bilgi edinin
* IoT Central hakkında daha fazla bilgi edinmek için [IoT Central genel bakış](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central)

