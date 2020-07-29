---
title: Azure IoT Hub Azure Event Hubs karşılaştırın | Microsoft Docs
description: IoT Hub karşılaştırması ve Azure hizmetleri Event Hubs işlevsel farklılıkları ve kullanım örneklerini vurgulamaktadır. Karşılaştırma desteklenen protokolleri, cihaz yönetimini, izlemeyi ve dosya yüklemelerini içerir.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.custom:
- amqp
- mqtt
- 'Role: Cloud Development'
- 'Role: System Architecture'
ms.openlocfilehash: 1b51e2338dabd34ec69d311a9eb1a5c4a644f628
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87322763"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>IoT cihazlarını Azure 'a bağlama: IoT Hub ve Event Hubs

Azure, verilerinizi bulutun gücüne bağlamanıza yardımcı olacak farklı türlerde bağlantılar ve iletişim için özel olarak geliştirilmiş hizmetler sunar. Hem Azure IoT Hub hem de Azure Event Hubs, büyük miktarlarda veri alabilen ve bu verileri iş öngörüleri için depolayabilen bulut hizmetlerdir. Bu iki hizmet, düşük gecikme süresi ve yüksek güvenilirlikle verilerin alımını desteklemeye benzer ancak farklı amaçlar için tasarlanırlar. IoT Hub, IoT cihazlarını Azure bulutuna bağlamaya yönelik benzersiz gereksinimleri karşılamak üzere geliştirildiği için Event Hubs büyük veri akışı için tasarlanmıştı. Microsoft IoT cihazlarını Azure 'a bağlamak için Azure IoT Hub kullanılmasını önerir

Azure IoT Hub, IoT cihazlarını veri toplamak ve iş öngörülerini ve Otomasyonu yönlendirmek için bağlayan bulut ağ geçidindür. Ayrıca, IoT Hub cihazlarınız ve arka uç sistemleriniz arasındaki ilişkiyi zenginleştiren özellikler içerir. İki yönlü iletişim özellikleri, cihazlardan veri alırken aynı zamanda komutları ve ilkeleri cihazlara geri gönderebilirken olduğu anlamına gelir. Örneğin, özellikleri güncelleştirmek veya cihaz yönetimi eylemlerini çağırmak için buluttan cihaza mesajlaşma 'yı kullanın. Buluttan cihaza iletişim Ayrıca, Azure IoT Edge ile uç cihazlarınıza bulut zekası göndermenizi de sağlar. IoT Hub tarafından sunulan benzersiz cihaz düzeyi kimliği, IoT çözümünüzün olası saldırılara karşı daha iyi korunmasına yardımcı olur. 

Azure [Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) , Azure 'un büyük veri akışı hizmetidir. Müşterilerin günde milyarlarca istek gönderebildiği, yüksek aktarım hızında veri akışı senaryoları için tasarlanmıştır. Event Hubs akışınızın ölçeğini genişletmek için bölümlenmiş bir tüketici modeli kullanır ve Azure'ın Databricks, Stream Analytics, ADLS ve HDInsight gibi büyük veri ve analiz hizmetleriyle tümleşik çalışır. Event Hubs yakalama ve otomatik Şişir gibi özelliklerle bu hizmet, büyük veri uygulamalarınızı ve çözümlerinizi destekleyecek şekilde tasarlanmıştır. Ayrıca, IoT Hub telemetri akış yolu için Event Hubs kullanır, bu nedenle IoT çözümünüz Event Hubs inanılmaz gücünden de faydalanır.

Özetlemek gerekirse, her iki çözüm de büyük ölçekte veri alımı için tasarlanmıştır. Yalnızca IoT Hub, IoT cihazlarınızı Azure bulutuna bağlayan iş değerini en üst düzeye çıkarmak için tasarlanan zengin IoT 'ye özgü yetenekler sağlar.  IoT yolculuğu, veri alma senaryolarınızı desteklemek için IoT Hub başlayıp, işiniz ve teknik gereksinimleriniz için ihtiyaç duyduktan sonra tam özellikli IoT özelliklerine anında erişebilmeniz güvence altına alınır.

Aşağıdaki tabloda, IoT Hub iki katmanının IoT özelliklerine göre değerlendirilirken Event Hubs nasıl Karşılaştırıldığı hakkında ayrıntılar verilmektedir. IoT Hub standart ve temel katmanları hakkında daha fazla bilgi için, bkz. [doğru IoT Hub katmanını seçme](iot-hub-scaling.md).

| IoT özelliği | Standart katman IoT Hub | Temel katmanı IoT Hub | Event Hubs |
| --- | --- | --- | --- |
| Cihazdan buluta mesajlaşma | ![İşaretli][checkmark] | ![İşaretli][checkmark] | ![İşaretli][checkmark] |
| Protokoller: HTTPS, AMQP, AMQP webSockets üzerinden | ![İşaretli][checkmark] | ![İşaretli][checkmark] | ![İşaretli][checkmark] |
| Protokoller: MQTT, webSockets üzerinden MQTT | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Cihaz başına kimlik | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Cihazlardan karşıya dosya yükleme | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Cihaz Sağlama Hizmeti | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Buluttan cihaza mesajlaşma | ![İşaretli][checkmark] |  |  |
| Cihaz ikizi ve cihaz yönetimi | ![İşaretli][checkmark] |  |  |
| Cihaz akışları (Önizleme) | ![İşaretli][checkmark] |  |  |
| IoT Edge | ![İşaretli][checkmark] |  |  |

Tek kullanım durumu cihazdan buluta veri alımı olsa bile, IoT cihaz bağlantısı için tasarlanan bir hizmet sağladığından IoT Hub kullanmanızı kesinlikle öneririz. 

### <a name="next-steps"></a>Sonraki adımlar

IoT Hub yeteneklerini daha fazla incelemek için, [IoT Hub Geliştirici Kılavuzu](iot-hub-devguide.md)' na bakın.

<!-- This one reference link is used over and over. --robinsh -->
[checkmark]: ./media/iot-hub-compare-event-hubs/ic195031.png
