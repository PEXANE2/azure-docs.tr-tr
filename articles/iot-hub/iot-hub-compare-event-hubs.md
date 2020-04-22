---
title: Azure IoT Hub'ı Azure Etkinlik Hub'larıyla karşılaştırın | Microsoft Dokümanlar
description: IoT Hub ve Etkinlik Hub'ları Azure hizmetlerinin işlevsel farklılıkları ve kullanım durumlarını vurgulayan karşılaştırması. Karşılaştırma desteklenen protokolleri, aygıt yönetimini, izlemeyi ve dosya yüklemelerini içerir.
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
ms.openlocfilehash: 7aa59f8181ad60b3d43846a3f4f1f471a050b238
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733431"
---
# <a name="connecting-iot-devices-to-azure-iot-hub-and-event-hubs"></a>IoT Aygıtlarını Azure'a Bağlama: IoT Hub'ı ve Etkinlik Hub'ları

Azure, verilerinizi bulutun gücüne bağlamanıza yardımcı olmak için çeşitli bağlantı ve iletişim türleri için özel olarak geliştirilmiş hizmetler sunar. Hem Azure IoT Hub'ı hem de Azure Etkinlik Hub'ları, büyük miktarda veri yutabilecek ve iş öngörüleri için bu verileri işleyebilir veya depolayabilen bulut hizmetleridir. Bu iki hizmet, her ikisi nin de düşük gecikme gecikmesi ve yüksek güvenilirlikle verilerin alımını desteklemesi açısından benzerdir, ancak farklı amaçlar için tasarlanmıştır. IoT Hub, IoT aygıtlarını Azure bulutuna bağlamanın benzersiz gereksinimlerini karşılamak üzere geliştirilirken, Etkinlik Hub'ları büyük veri akışı için tasarlanmıştır. Microsoft, IoT aygıtlarını Azure'a bağlamak için Azure IoT Hub'ı kullanmanızı önerir

Azure IoT Hub, veri toplamak ve iş öngörülerini ve otomasyonu sağlamak için IoT aygıtlarını birbirine bağlayan bulut ağ geçididir. Ayrıca, IoT Hub aygıtlarınız ve arka uç sistemleriniz arasındaki ilişkiyi zenginleştiren özellikler içerir. Çift yönlü iletişim özellikleri, aygıtlardan veri alırken komutları ve ilkeleri aygıtlara geri gönderebileceğiniz anlamına gelir. Örneğin, özellikleri güncelleştirmek veya aygıt yönetimi eylemlerini çağırmak için bulut-aygıt iletisini kullanın. Bulut-aygıt iletişimi, Azure IoT Edge ile kenar aygıtlarınıza bulut zekası göndermenize de olanak tanır. IoT Hub tarafından sağlanan benzersiz cihaz düzeyindekimlik, IoT çözümünüzü olası saldırılara karşı daha iyi korumaya yardımcı olur. 

[Azure Etkinlik Hub'ları](../event-hubs/event-hubs-what-is-event-hubs.md) Azure'un büyük veri akışı hizmetidir. Müşterilerin günde milyarlarca istek gönderebildiği, yüksek aktarım hızında veri akışı senaryoları için tasarlanmıştır. Event Hubs akışınızın ölçeğini genişletmek için bölümlenmiş bir tüketici modeli kullanır ve Azure'ın Databricks, Stream Analytics, ADLS ve HDInsight gibi büyük veri ve analiz hizmetleriyle tümleşik çalışır. Event Hub'ları Yakalama ve Otomatik Şişirme gibi özelliklerle bu hizmet, büyük veri uygulamalarınızı ve çözümlerinizi desteklemek üzere tasarlanmıştır. Ayrıca, IoT Hub telemetri akış yolu için Olay Hub'larını kullanır, böylece IoT çözümünüz De'nin Etkinlik Hub'larının muazzam gücünden yararlanır.

Özetlemek gerekirse, her iki çözüm de büyük ölçekte veri alımı için tasarlanmıştır. Yalnızca IoT Hub, IoT aygıtlarınızı Azure bulutuna bağlamanın iş değerini en üst düzeye çıkarmak için tasarlanmış ioT'ye özel zengin özellikleri sağlar.  IoT yolculuğunuz daha yeni başlıyorsa, veri alma senaryolarınızı desteklemek için IoT Hub ile başlamak, iş ve teknik ihtiyaçlarınız gerektirdiğinde tam özellikli IoT yeteneklerine anında erişmenizi sağlayacaktır.

Aşağıdaki tablo, IoT Hub'ının iki katmanının IoT yetenekleri için değerlendirirken Olay Hub'larıyla nasıl karşılaştırışları hakkında ayrıntılı bilgi sağlar. IoT Hub'ın standart ve temel katmanları hakkında daha fazla bilgi için [doğru IoT Hub katmanını nasıl seçeceğinizi](iot-hub-scaling.md)öğrenin.

| IoT Yeteneği | IoT Hub standart katmanı | IoT Hub temel katmanı | Event Hubs |
| --- | --- | --- | --- |
| Aygıt-bulut mesajlaşma | ![İşaretli][checkmark] | ![İşaretli][checkmark] | ![İşaretli][checkmark] |
| Protokoller: HTTPS, AMQP, WEBSockets üzerinden AMQP | ![İşaretli][checkmark] | ![İşaretli][checkmark] | ![İşaretli][checkmark] |
| Protokoller: MQTT, MQTT webSockets üzerinde | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Cihaz başına kimlik | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Aygıtlardan dosya yükleme | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Cihaz Sağlama Hizmeti | ![İşaretli][checkmark] | ![İşaretli][checkmark] |  |
| Bulut-aygıt mesajlaşma | ![İşaretli][checkmark] |  |  |
| Aygıt ikizi ve cihaz yönetimi | ![İşaretli][checkmark] |  |  |
| Aygıt akışları (önizleme) | ![İşaretli][checkmark] |  |  |
| IoT Edge | ![İşaretli][checkmark] |  |  |

Tek kullanım örneği aygıt-bulut veri alımı olsa bile, IoT aygıt bağlantısı için tasarlanmış bir hizmet sağladığı için IoT Hub'ı kullanmanızı şiddetle öneririz. 

### <a name="next-steps"></a>Sonraki adımlar

IoT Hub'ın yeteneklerini daha fazla keşfetmek için [IoT Hub geliştirici kılavuzuna](iot-hub-devguide.md)bakın.

<!-- This one reference link is used over and over. --robinsh -->
[checkmark]: ./media/iot-hub-compare-event-hubs/ic195031.png
