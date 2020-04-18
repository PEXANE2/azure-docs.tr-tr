---
title: Sık sorulan sorular - Apache Kafka için Azure Etkinlik Hub'ları
description: Bu makalede, farklı protokoller (AMQP, Apache Kafka ve HTTPS) kullanan tüketicilerin ve üreticilerin Azure Etkinlik Hub'larını kullanırken etkinlik alışverişi nasıl yapabildiği gösterilmektedir.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2020
ms.author: shvija
ms.openlocfilehash: 0186b90e1d75c5dba6e1ca26e4ba079a3456cea4
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606749"
---
# <a name="frequently-asked-questions---event-hubs-for-apache-kafka"></a>Sık sorulan sorular - Apache Kafka için Etkinlik Hub'ları 
Bu makalede, Apache Kafka için Olay Hub'larına geçiş le ilgili sık sorulan bazı soruların yanıtları verilmektedir.

## <a name="do-you-run-apache-kafka"></a>Apaçi Kafka'yı yönetiyor musun?

Hayır.  Olay Hubları altyapısına karşı Kafka API operasyonlarını yürütüyoruz.  Apache Kafka ve Event Hubs AMQP işlevleri (yani, üretmek, almak, yönetim, vb.) arasında sıkı bir korelasyon olduğundan, Kafka PaaS alanına Event Hub'larının bilinen güvenilirliğini getirebiliyoruz.

## <a name="event-hubs-consumer-group-vs-kafka-consumer-group"></a>Olay Hubs tüketici grubu vs Kafka tüketici grubu
Event Hub tüketici grubu ile Etkinlik Hub'larında Kafka tüketici grubu arasındaki fark nedir? Etkinlik Hub'ları üzerindeki Kafka tüketici grupları standart Event Hub'ları tüketici gruplarından tamamen farklıdır.

**Olay Hub'ları tüketici grupları**

- Portal, SDK veya Azure Kaynak Yöneticisi şablonları aracılığıyla oluşturma, alma, güncelleştirme ve silme (CRUD) işlemleriyle yönetilirler. Olay Hub'ları tüketici grupları otomatik olarak oluşturulamıyor.
- Bunlar, bir olay merkezinin çocuk varlıklarıdır. Bu, aynı tüketici grubu adının ayrı varlıklar olmaları nedeniyle aynı ad alanında olay hub'ları arasında yeniden kullanılabildiği anlamına gelir.
- Uzaklıkları depolamak için kullanılmaz. Düzenlenmiş AMQP tüketimi, örneğin Azure Depolama gibi harici ofset depolama kullanılarak yapılır.

**Kafka tüketici grupları**

- Otomatik olarak oluşturulurlar.  Kafka grupları Kafka tüketici grubu API'leri üzerinden yönetilebilir.
- Uzaklıkları Olay Hub'ları hizmetinde depolayabilirler.
- Bunlar, etkin bir ofset anahtar değeri deposunda anahtar olarak kullanılır. Benzersiz bir çift `group.id` `topic-partition`ve , bir ofset için Azure Depolama (3x çoğaltma) saklıyoruz. Event Hubs kullanıcıları Kafka uzaklıklarını depolamaktan ek depolama maliyeti ne tabidir. Uzaklıklar Kafka tüketici grubu API'leri aracılığıyla kullanılabilir, ancak ofset depolama *hesapları* Doğrudan görünür veya Event Hub kullanıcıları için manevra yapamaz.  
- İsim alanını kaplarlar. Birden çok konuda birden fazla uygulama için aynı Kafka grup adının kullanılması, tüm uygulamaların ve Kafka istemcilerinin yalnızca tek bir uygulamanın yeniden dengelenmesi gerektiğinde yeniden dengelenmesi anlamına gelir.  Grup adlarınızı akıllıca seçin.
- Event Hub'ların tüketici gruplarından tamamen farklıdırlar. '$Default' kullanmanız anın ne de Kafka müşterilerinin AMQP iş yüklerini engellemesi konusunda endişelenmenize gerek yoktur. **don't**
- Azure portalında görüntülenebilirler. Tüketici grubu bilgilerine Kafka API'leri üzerinden ulaşılabilir.

## <a name="next-steps"></a>Sonraki adımlar
Kafka için Etkinlik Hub'ları ve Etkinlik Hub'ları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:  

- [Etkinlik Hub'ları için Apache Kafka geliştirici kılavuzu](apache-kafka-developer-guide.md)
- [Etkinlik Hub'ları için Apache Kafka geçiş kılavuzu](apache-kafka-migration-guide.md)
- [Etkinlik Hub'ları için Apache Kafka sorun giderme kılavuzu](apache-kafka-troubleshooting-guide.md)
- [Önerilen yapılandırmalar](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md)

