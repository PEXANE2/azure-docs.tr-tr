---
title: Azure Nesnelerin İnterneti (IoT) çözüm seçenekleri
description: Bir IoT çözümü oluşturmak için bir platform hizmetleri veya yönetilen uygulama platformu yaklaşımı arasında seçim yapma kılavuzu. Platform hizmet yaklaşımı, IoT Hub ve Digital Twins gibi hizmetleri yapı taşları olarak kullanır. Yönetilen uygulama platformu yaklaşımı, hızlı bir şekilde başlamak için IoT Central'ı kullanır.
author: dominicbetts
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 02/05/2020
ms.author: dobett
ms.openlocfilehash: 046116addf643d6c0eb0faf2f821b6ef78d440c6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77050343"
---
# <a name="choose-the-right-iot-solution"></a>Doğru IoT çözümünü seçme

İşletmeniz için bir IoT çözümü oluşturmak için genellikle *platform hizmetlerini* veya *yönetilen uygulama platformu* yaklaşımını kullanmayı tercih edersiniz.

Platform hizmetleri, özelleştirilmiş ve esnek IoT uygulamaları için yapı taşları sağlar. Aygıtları bağladığınızda seçip kodlamak ve verilerinizi yutmak, depolamak ve çözümlemek için daha fazla seçeneğiniz vardır. Azure IoT platform hizmetleri, Azure IoT Hub ve Azure Digital Twins ürünlerini içerir.

Yönetilen bir uygulama platformu, sonuçlar elde etmek için gereken karar sayısını azaltarak uygulamaları platform hizmetlerinden daha hızlı oluşturmanıza olanak tanır. Yönetilen uygulama platformu, endüstri bilgisi eklemeye, ölçekleme ve aygıtları bağlamaya odaklanabilmeniz için çözümünuzun çoğu öğesini ele alır. Azure IoT Central yönetilen bir uygulama platformudur.

Bu iki yaklaşım arasında seçim yapmak için şunları göz önünde bulundurmalısınız:

- Çözümünüzü nasıl yönetmek istediğinizi.
- Çözümünüz üzerinde ne düzeyde özelleştirme ve denetim istediğinizi.
- Ne fiyatlandırma yapısı istediğiniz.

## <a name="management"></a>Yönetim

Sistem yönetimi zamanınızı ve kaynaklarınızı nerede harcamak istiyorsunuz? 

- Çözümünüzde temel hizmetler üzerinde tam kontrole sahip olmak için platform hizmetleri yaklaşımını seçin. Örneğin, şunları yapmak istiyorsunuz:

    - İhtiyaçlarınızı karşılamak için ölçekleme ve güvenlik hizmetlerini yönetin.
    - Dahili cihazlara ve tedarik hizmetlerine şirket içi veya iş ortağı uzmanlığından yararlanın.

- IoT uygulamalarınızın ve cihazlarınızın ölçeğini, güvenliğini ve yönetimini işleyen bir platformdan yararlanmak için yönetilen uygulama platformu yaklaşımını seçin.

## <a name="control"></a>Denetim

Çözümünüzün hangi öğelerini özelleştirmek istiyorsunuz?

- Toplam özelleştirme ve çözüm mimarisi üzerinde denetim için platform hizmetleri yaklaşımını seçin.

- Markalamayı, panoları, kullanıcı rollerini, cihazları ve telemetriyi özelleştirmek için yönetilen uygulama platformu yaklaşımını seçin. Ancak, altta yatan IoT sistem yönetimi genel yükü işlemek istemiyorum.

## <a name="pricing"></a>Fiyatlandırma

İhtiyaçlarınıza en uygun fiyatlandırma yapısı nedir?

- Hizmetlere ince ayar yapmak için platform hizmetleri yaklaşımını seçin ve genel maliyetlerimi kontrol edin.

- Basit ve öngörülebilir bir fiyatlandırma yapısı için yönetilen uygulama platformu yaklaşımını seçin.

## <a name="summary"></a>Özet

Platform hizmetleri yaklaşımı, bulut çözümü ve aşağıdakileri isteyen cihaz uzmanlığına sahip bir işletme için uygundur:

- Çözümdeki hizmetlerde ince ayar.
- Çözümdeki hizmetler üzerinde yüksek derecede kontrole sahip olun.
- Çözümü tamamen özelleştirin.

Yönetilen uygulama platformu yaklaşımı, aşağıdakileri yapan bir işletme için uygundur:

- Sistem tasarımı, geliştirme ve yönetim için kapsamlı kaynaklar ayırmak istemiyor.
- Öngörülebilir bir fiyatlandırma yapısı istiyor.
- Bazı özelleştirme yetenekleri istiyor.

## <a name="next-steps"></a>Sonraki adımlar

Farklı hizmetler ve platformlar ve bunların nasıl kullanıldığı hakkında daha kapsamlı bir açıklama için [Azure IoT hizmetleri ve teknolojilerine](iot-services-and-technologies.md)bakın.

Başarılı IoT çözümlerinin temel öznitelikleri hakkında daha fazla bilgi edinmek [için, başarılı IoT çözümlerinin 8 özniteliğine](https://aka.ms/8attributes) bakın.

IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi](https://aka.ms/iotrefarchitecture).
