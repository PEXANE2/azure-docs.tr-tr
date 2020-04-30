---
title: Azure Nesnelerin İnterneti (IoT) çözüm seçenekleri
description: Bir IoT çözümü oluşturmaya yönelik platform hizmetleri veya yönetilen uygulama platformu arasında seçim yapma kılavuzu. Platform hizmeti yaklaşımı, derleme blokları olarak IoT Hub ve dijital TWINS gibi hizmetleri kullanır. Yönetilen uygulama platformu yaklaşımı hızlı bir şekilde başlamak için IoT Central kullanır.
author: dominicbetts
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 02/05/2020
ms.author: dobett
ms.openlocfilehash: 046116addf643d6c0eb0faf2f821b6ef78d440c6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77050343"
---
# <a name="choose-the-right-iot-solution"></a>Doğru IoT çözümünü seçme

İşletmeniz için bir IoT çözümü oluşturmak üzere genellikle *platform hizmetlerini* veya *yönetilen uygulama platformu* yaklaşımını kullanmayı seçersiniz.

Platform Hizmetleri, özelleştirilmiş ve esnek IoT uygulamalarına yönelik derleme blokları sağlar. Cihazları bağladığınızda ve verilerinizi alma, depolama ve çözümleme konusunda daha fazla seçeneğiniz vardır. Azure IoT Platformu Hizmetleri, Azure IoT Hub ve Azure dijital TWINS ürünlerini içerir.

Yönetilen bir uygulama platformu, sonuçlara ulaşmak için gereken kararların sayısını azaltarak platform hizmetlerinden daha hızlı uygulamalar oluşturmaya başlamanıza olanak sağlar. Yönetilen uygulama platformu çözümünüzün birçok öğesi ile ilgilenir. böylece sektör bilgisi eklemeye ve cihazları ölçeklendirmeye ve bağlamaya odaklanırsınız. Azure IoT Central, yönetilen bir uygulama platformudur.

Bu iki yaklaşım arasından seçim yapmak için şunları göz önünde bulundurmanız gerekir:

- Çözümünüzü nasıl yönetmek istiyorsunuz.
- Çözümünüz üzerinde istediğiniz özelleştirme ve denetim düzeyi.
- İstediğiniz Fiyatlandırma yapısı.

## <a name="management"></a>Yönetim

Sistem Yönetimi zaman ve kaynaklarınızı nerede kullanmak istiyorsunuz? 

- Çözümünüzde temeldeki hizmetler üzerinde tam denetime sahip olmak için platform hizmetleri yaklaşımını seçin. Örneğin, şunları yapmak istiyorsunuz:

    - Gereksinimlerinizi karşılamak için ölçeklendirmeyi yönetin ve hizmetleri güvenli hale getirme.
    - Cihazları eklemek ve hizmetleri sağlamak için şirket içi veya iş ortağı uzmanlığından yararlanın.

- IoT uygulamalarınızın ve cihazlarınızın ölçeğini, güvenliğini ve yönetimini işleyen bir platformdan faydalanmak için yönetilen uygulama platformu yaklaşımını seçin.

## <a name="control"></a>Denetim

Çözümünüzün hangi öğelerini özelleştirmek istiyorsunuz?

- Çözüm mimarisi üzerinde toplam özelleştirme ve denetim için platform hizmetleri yaklaşımını seçin.

- Markalamayı, panoları, Kullanıcı rollerini, cihazları ve telemetrisi özelleştirmek için yönetilen uygulama platformu yaklaşımını seçin. Ancak, temeldeki IoT Sistem Yönetimi ek yükünü işlemek istemezsiniz.

## <a name="pricing"></a>Fiyatlandırma

Gereksinimlerinize en uygun Fiyatlandırma yapısı hangisi?

- Hizmetler üzerinde ince ayar yapmak ve genel maliyetlerimi denetlemek için platform hizmetleri yaklaşımını seçin.

- Basit ve öngörülebilir bir fiyatlandırma yapısı için yönetilen uygulama platformu yaklaşımını seçin.

## <a name="summary"></a>Özet

Platform Hizmetleri yaklaşımı, bulut çözümü ve cihaz uzmanlığına sahip olan ve şunları yapmak isteyen bir işletme için uygundur:

- Çözümdeki Hizmetleri hassas olarak ayarlayın.
- Çözümdeki hizmetler üzerinde yüksek düzeyde denetime sahip olmak.
- Çözümü tam olarak özelleştirin.

Yönetilen uygulama platformu yaklaşımı şu iş için uygundur:

- , Sistem tasarımına, geliştirmeye ve yönetimine kapsamlı kaynaklar atamak istemiyor.
- Tahmin edilebilir bir fiyatlandırma yapısı istiyor.
- , Bazı özelleştirme özellikleri istiyor.

## <a name="next-steps"></a>Sonraki adımlar

Farklı hizmet ve platformların ve bunların nasıl kullanıldıklarından daha kapsamlı bir açıklama için bkz. [Azure IoT Hizmetleri ve teknolojileri](iot-services-and-technologies.md).

Başarılı IoT çözümlerinin anahtar öznitelikleri hakkında daha fazla bilgi edinmek için [başarılı IoT çözümleri teknik incelemesi 'nin 8 özniteliklerine](https://aka.ms/8attributes) bakın.

IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi](https://aka.ms/iotrefarchitecture).
