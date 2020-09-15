---
title: Sık Sorulan Sorular-Apache Kafka için Azure Event Hubs
description: Bu makalede, başka bir yerde kapsanmayan Apache Kafka istemciler için Azure Event Hubs ' desteğiyle ilgili sık sorulan sorular yanıtlanmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 1ca54bf314d795b88b727ddb648f3e1e74133fd3
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90061470"
---
# <a name="frequently-asked-questions---event-hubs-for-apache-kafka"></a>Sık Sorulan Sorular-Apache Kafka için Event Hubs 
Bu makalede, Apache Kafka için Event Hubs geçiş hakkında sık sorulan soruların bazılarına yanıtlar verilmektedir.

## <a name="does-azure-event-hubs-run-on-apache-kafka"></a>Azure Event Hubs Apache Kafka üzerinde çalışıyor mu?

Hayır. Azure Event Hubs, Microsoft tarafından geliştirilen ve bakımını yapan ve herhangi bir Apache Kafka kodu kullanmayan birden fazla protokol için desteğe sahip, bulutta yerel bir çok katmanlı aracıdır. Desteklenen protokollerden biri, Kafka istemcisinin tüketicisi ve üreticisi API 'Leri için Kafka RPC protokolüdür. Event Hubs, mevcut Kafka uygulamalarınızın birçoğu ile birlikte kullanılabilir. Daha fazla bilgi için bkz. [Apache Kafka Event Hubs](event-hubs-for-kafka-ecosystem-overview.md). Apache Kafka ve Azure Event Hubs kavramları çok benzer (ancak aynı değil), mevcut Apache Kafka yatırımlarıyla müşterilere eşleşmeyen Azure Event Hubs güvenilirliğini sunabileğiz. 

## <a name="event-hubs-consumer-group-vs-kafka-consumer-group"></a>Event Hubs Tüketici grubu vs. Kafka Tüketici grubu
Event Hubs üzerinde bir olay hub 'ı Tüketici grubu ve Kafka Tüketici grubu arasındaki fark nedir? Event Hubs üzerindeki Kafka tüketici grupları standart Event Hubs tüketici gruplarından tamamen farklıdır.

**Event Hubs tüketici grupları**

- Portal, SDK veya Azure Resource Manager şablonları aracılığıyla oluşturma, alma, güncelleştirme ve silme (CRUD) işlemleri ile yönetilir. Event Hubs tüketici grupları oto oluşturulamıyor.
- Bunlar bir olay hub 'ının alt varlıklarıdır. Aynı Tüketici grubu adının aynı ad alanındaki Olay Hub 'ları arasında ayrı olarak yeniden kullanılabilmesi anlamına gelir, çünkü bunlar ayrı varlıklardır.
- Bu kişiler, uzaklıkları depolamak için kullanılmaz. Genişletilmiş AMQP tüketimi, dış konum depolama (örneğin, Azure depolama) kullanılarak yapılır.

**Kafka tüketici grupları**

- Bunlar, oto olarak oluşturulur.  Kafka grupları Kafka Tüketici grubu API 'Leri aracılığıyla yönetilebilir.
- Event Hubs hizmetinde uzaklıkları depolayabilirler.
- Bunlar, etkili bir şekilde bir konum anahtar-değer deposu olan anahtarlar olarak kullanılırlar. Ve ' nin benzersiz bir çifti için `group.id` `topic-partition` Azure Storage 'da (3x çoğaltma) bir fark depoluyoruz. Event Hubs kullanıcılar, Kafka farklarını depolamadan daha fazla depolama maliyeti vermez. Uzaklıklar Kafka Tüketici grubu API 'Leri aracılığıyla yapılır, ancak uzaklık depolama *hesapları* doğrudan görünür değildir veya Olay Hub 'ı kullanıcıları için kullanılabilir değildir.  
- Bir ad alanı yayırlar. Birden çok konuda birden çok uygulama için aynı Kafka grubu adının kullanılması, yalnızca tek bir uygulamanın yeniden dengelenmesi gerektiğinde tüm uygulamaların ve Kafka istemcilerinin yeniden dengeleneceği anlamına gelir.  Grup adlarınızı akıllıca seçin.
- Event Hubs tüketici gruplarından tamamen farklıdır. ' $Default ' **kullanmanız gerekmez veya** AMQP iş yüklerine engel olan Kafka istemcileri hakkında endişelenmeniz gerekmez.
- Azure portal görüntülenemez. Tüketici grubu bilgilerine Kafka API 'Leri aracılığıyla erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar
Kafka için Event Hubs ve Event Hubs hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:  

- [Event Hubs için Apache Kafka Geliştirici Kılavuzu](apache-kafka-developer-guide.md)
- [Event Hubs için Apache Kafka geçiş kılavuzu](apache-kafka-migration-guide.md)
- [Event Hubs için sorun giderme kılavuzu Apache Kafka](apache-kafka-troubleshooting-guide.md)
- [Önerilen yapılandırma](apache-kafka-configurations.md)

