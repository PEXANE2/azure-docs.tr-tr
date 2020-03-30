---
title: Özelliklere genel bakış - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, Azure Olay Hub'larının özellikleri ve terminolojisi hakkında ayrıntılı bilgi verilmektedir.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 568a21cee5b50a8914c603976f5951d0235dbff7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281489"
---
# <a name="features-and-terminology-in-azure-event-hubs"></a>Azure Event Hubs'ın özellikleri ve terminolojisi

Azure Olay Hub'ları, düşük gecikme gecikmesi ve yüksek güvenilirlikle büyük hacimli olayları ve verileri yutan ve işleyen ölçeklenebilir bir olay işleme hizmetidir. Üst düzey bir genel bakış için [Olay Hub'ları nedir?](event-hubs-what-is-event-hubs.md)

Bu makale, genel bakış [makalesindeki](event-hubs-what-is-event-hubs.md)bilgilere dayanmaktadır ve Olay Hub'ları bileşenleri ve özellikleri hakkında teknik ve uygulama ayrıntıları sağlar.

## <a name="namespace"></a>Ad Alanı
Olay Hub'ları ad alanı, [tam nitelikli etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)ile başvurulan ve bir veya daha fazla etkinlik hub'ı veya Kafka konusu oluşturduğunuz benzersiz bir kapsam kapsayıcısı sağlar. 

## <a name="event-hubs-for-apache-kafka"></a>Apache Kafka için Event Hubs

[Bu özellik,](event-hubs-for-kafka-ecosystem-overview.md) müşterilerin Kafka protokolünü kullanarak Event Hub'ları ile konuşmasını sağlayan bir uç nokta sağlar. Bu entegrasyon müşterilere Kafka bitiş noktası sağlar. Bu, müşterilerin mevcut Kafka uygulamalarını, Kendi Kafka kümelerini çalıştırmaya alternatif sunarak Event Hub'larıyla konuşmak üzere yapılandırmalarına olanak tanır. Apache Kafka için Etkinlik Hub'ları Kafka protokolü 1.0 ve sonrası protokolünü destekler. 

Bu entegrasyon ile Kafka kümelerini çalıştırmanız veya Zookeeper ile yönetmeniz gerekmez. Bu aynı zamanda Capture, Auto-flate ve Geo-disaster Recovery gibi Olay Hub'larının en zorlu özelliklerinden bazılarıyla çalışmanızı sağlar.

Bu tümleştirme aynı zamanda Mirror Maker veya Kafka Connect gibi çerçeve gibi uygulamaların sadece yapılandırma değişiklikleriyle kümesiz çalışmasını sağlar. 

## <a name="event-publishers"></a>Olay yayımcıları

Bir olay hub'ına veri gönderen herhangi bir varlık bir olay üreticisi veya *olay yayımcısıdır.* Etkinlik yayıncıları olayları HTTPS veya AMQP 1.0 veya Kafka 1.0 ve sonrası kullanarak yayımlayabilir. Olay yayımcıları kendilerini bir olay hub'ına tanıtmak için Paylaşılan Erişim İmzası (SAS) belirteci kullanır ve benzersiz bir kimliğe sahip olabilir ya da ortak bir SAS belirteci kullanabilir.

### <a name="publishing-an-event"></a>Olay yayımlama

Bir etkinliği AMQP 1.0, Kafka 1.0 (ve sonrası) veya HTTPS üzerinden yayınlayabilirsiniz. Olay Hub'ları, .NET istemcilerinden bir olay hub'ına olayları yayımlamak için [istemci kitaplıkları ve sınıfları](event-hubs-dotnet-framework-api-overview.md) sağlar. Diğer çalışma zamanları ve platformlar için [Apache Qpid](https://qpid.apache.org/) gibi herhangi bir AMQP 1.0 istemcisi kullanabilirsiniz. Olayları ayrı ayrı veya toplu olarak yayımlayabilirsiniz. Tek bir yayının (olay veriörneği) tek bir olay veya toplu iş olup olmadığına bakılmaksızın 1 MB sınırı vardır. Bu eşikten daha büyük olayları yayımlama bir hatayla sonuçlanır. Yayımcıların olay hub'ındaki bölümleri bilmemesi ve yalnızca bir *bölüm anahtarı* (sonraki bölümde açıklanmıştır) ya da kimliklerini SAS belirteci üzerinden belirtmeleri en iyi yöntemdir.

AMQP veya HTTPS kullanma seçimi kullanım senaryosuna bağlıdır. AMQP, taşıma düzeyi güvenliği (TLS) veya SSL/TLS’ye ek olarak kalıcı bir çift yönlü yuva oluşturulmasını gerektirir. Oturum başlatılırken AMQP’nin ağ maliyetleri daha yüksektir, ancak HTTPS her istek için ek SSL yükü gerektirir. Daha sık yayımcılar için AMQP daha yüksek performans sunar.

![Event Hubs](./media/event-hubs-features/partition_keys.png)

Event Hubs aynı bölüm anahtarı değerini paylaşan tüm olayların sırayla ve aynı bölüme iletilmesini sağlar. Bölüm anahtarlarının yayımcı ilkeleriyle birlikte kullanılması durumunda yayımcı kimliğinin ve bölüm anahtarı değerinin eşleşmesi gerekir. Aksi takdirde bir hata oluşur.

### <a name="publisher-policy"></a>Yayımcı ilkesi

Event Hubs, *yayımcı ilkeleri* aracılığıyla olay yayımcıları üzerinde ayrıntılı denetim sağlar. Yayımcı ilkeleri çok sayıda bağımsız olay yayımcısını kolaylaştırmak için tasarlanmış çalışma zamanı özellikleridir. Yayımcı ilkeleriyle her yayımcı, olayları aşağıdaki mekanizmayı kullanarak bir olay hub'ında yayımlarken kendi benzersiz tanımlayıcısını kullanır:

```http
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Yayımcı adlarını önceden oluşturmanız gerekli değildir, ancak bunlar bağımsız yayımcı kimlikleri sağlamak amacıyla bir olayı yayımlarken kullanılan SAS belirteci ile eşleşmelidir. Yayımcı ilkelerini kullanırken **PartitionKey** değeri yayımcı adına ayarlanır. Bu hizmetin düzgün çalışması için bu değerlerin eşleşmesi gerekir.

## <a name="capture"></a>Capture

[Olay Hub'ları Yakalama,](event-hubs-capture-overview.md) Olay Hub'larında akış verilerini otomatik olarak yakalamanızı ve blob depolama hesabı veya Azure Veri Gölü Hizmeti hesabı seçiminize kaydetmenizi sağlar. Azure portalından Capture'i etkinleştirebilir ve yakalamayı gerçekleştirmek için minimum boyut ve zaman penceresi belirtebilirsiniz. Olay Hub'ları Yakalama'yı kullanarak, yakalanan verileri depolamak için kullanılan kendi Azure Blob Depolama hesabınızı ve kapsayıcınızı veya Azure Veri Gölü Hizmeti hesabınızı belirtirsiniz. Yakalanan veriler Apache Avro formatında yazılır.

## <a name="partitions"></a>Bölümler
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]


## <a name="sas-tokens"></a>SAS belirteçleri

Olay Hub'ları, ad alanı ve etkinlik hub düzeyinde kullanılabilen *Paylaşılan Erişim İmzaları*kullanır. SAS belirteci bir SAS anahtarından oluşturulur ve belirli bir biçimde kodlanmış bir URL’nin SHA karmasıdır. Event Hubs anahtar (ilke) ve belirtecin adını kullanarak karmayı yeniden oluşturabilir ve böylece gönderenin kimliğini doğrular. Normalde, olay yayımcıları için SAS belirteci yalnızca belirli bir olay hub'ı üzerindeki **gönder** ayrıcalıkları ile oluşturulur. Bu SAS belirteci URL mekanizması, yayımcı ilkesinde sunulan yayımcı kimliğinin temelini oluşturur. SAS ile çalışma hakkında daha fazla bilgi için bkz. [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması](../service-bus-messaging/service-bus-sas.md).

## <a name="event-consumers"></a>Olay tüketicileri

Bir olay hub'ından olay verilerini okuyan herhangi bir varlık *olay tüketicisidir*. Tüm Event Hubs tüketicileri AMQP 1.0 oturumu üzerinden bağlanır ve olaylar, kullanılabilir olduğu anda oturum üzerinden iletilir. İstemcinin veri kullanılabilirliğini yoklaması gerekmez.

### <a name="consumer-groups"></a>Tüketici grupları

Event Hubs yayımlama/abonelik mekanizması *tüketici grupları* aracılığıyla etkinleştirilir. Tüketici grubu tüm olay hub'ının bir görünümüdür (durum, konum veya uzaklık). Tüketici grupları birden çok tüketen uygulamayı her biri olay akışının ayrı bir görünümüne sahip olacak ve akışı kendi hızlarında ve kendi sapmalarıyla bağımsız bir şekilde okuyacak şekilde etkinleştirir.

Bir akış işleme mimarisinde her bir aşağı akış uygulaması bir tüketici grubuna karşılık gelir. Olay verilerini uzun süreli depolama alanına yazmak isterseniz bu depolama yazma uygulaması bir tüketici grubudur. Bundan sonra karmaşık olay işlemesi başka ve ayrı bir tüketici grubu tarafından gerçekleştirilebilir. Bölümlere yalnızca bir tüketici grubu üzerinden erişebilirsiniz. Bir olay hub'ında her zaman varsayılan bir tüketici grubu vardır ve Standart katmanlı bir olay hub'ı için en fazla 20 tüketici grubu oluşturabilirsiniz.

Tüketici grubu başına bir bölüm üzerinde en fazla 5 eşzamanlı okuyucu olabilir; ancak **tüketici grubu başına bir bölüm üzerinde yalnızca bir etkin alıcı olması önerilir.** Tek bir bölüm içinde, her okuyucu tüm iletileri alır. Aynı bölümüzerinde birden çok okuyucunuz varsa, yinelenen iletileri işlersiniz. Bunu kodunuzda ele almanız gerekir, ki bu önemsiz olmayabilir. Ancak, bazı senaryolarda geçerli bir yaklaşımdır.


Tüketici grubu URI kuralının örnekleri aşağıda verilmiştir:

```http
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

Aşağıdaki şekilde Event Hubs akış işleme mimarisi gösterilmektedir:

![Event Hubs](./media/event-hubs-features/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Akış uzaklıkları

*Uzaklık* bir olayın bölüm içindeki konumudur. Uzaklığı istemci tarafındaki bir imleç olarak düşünebilirsiniz. Uzaklık, olayın bayt cinsinden numaralandırılmasıdır. Bu uzaklık, olay tüketicisinin (okuyucu) olay akışında olayları okumaya başlamak istediği bir noktayı belirtmesini sağlar. Uzaklığı bir zaman damgası veya bir uzaklık değeri olarak belirtebilirsiniz. Tüketiciler, kendi uzaklık değerlerini Event Hubs hizmetinin dışında saklamaktan sorumludur. Bir bölüm içinde her olay bir uzaklık içerir.

![Event Hubs](./media/event-hubs-features/partition_offset.png)

### <a name="checkpointing"></a>Denetim noktası oluşturma

*Denetim noktası oluşturma*, okuyucuların bir bölüm olay dizisindeki konumlarını işaretledikleri veya uyguladıkları bir işlemdir. Denetim noktası oluşturma, tüketicinin sorumluluğundadır ve bir tüketici grubunda bölüm başına temelinde gerçekleşir. Bu sorumluluk, her bir tüketici grubu için her bölüm okuyucusunun geçerli konumunu olay akışında izlemesi gerektiği ve veri akışının tamamlandığını düşündüğünde hizmeti bilgilendirebileceği anlamına gelir.

Bir okuyucunun bölüm bağlantısı kesilirse yeniden bağlandığında ilgili tüketici grubundaki o bölümün son okuyucusu tarafından daha önce gönderilen denetim noktasında okumaya başlar. Okuyucu bağlandığında, okumaya başlayacağınız yeri belirtmek için ofset'i olay merkezine geçirir. Bu şekilde, denetim noktası oluşturma özelliğini hem aşağı akış uygulamaları ile olayları "tamamlandı" olarak işaretlemek hem de farklı makinelerde çalışan okuyucular arasında bir yük devretme oluşması durumunda esneklik sağlamak amacıyla kullanabilirsiniz. Bu denetim noktası oluşturma işleminden daha düşük bir uzaklık belirterek daha eski verilere geri dönülebilir. Bu mekanizmayla denetim noktası oluşturma özelliği hem yük devretme esnekliği hem de olay akışı yeniden yürütmesi sağlar.

### <a name="common-consumer-tasks"></a>Ortak tüketici görevleri

Tüm Event Hub'ları tüketicileri, devlet tarafından bilinçli bir çift yönlü iletişim kanalı olan AMQP 1.0 oturumu aracılığıyla bağlanır. Her bölümde bölüme göre ayrılmış olayların taşınmasını kolaylaştıran bir AMQP 1.0 oturumu vardır.

#### <a name="connect-to-a-partition"></a>Bir bölüme bağlanma

Bölümlere doğrudan bağlanırken okuyucu bağlantılarının belirli bölümlerle koordine edilmesi için bir kiralama mekanizmasının kullanılması yaygın bir uygulamadır. Bu şekilde, bir tüketici grubundaki her bölümün yalnızca bir etkin okuyucuya sahip olması mümkündür. Denetim noktası oluşturma, kiralama ve okuyucuları yönetme işlemleri, .NET istemcileri için [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) sınıfı kullanılarak basitleştirilir. Event Processor Host, akıllı bir tüketici aracısıdır.

#### <a name="read-events"></a>Olayları okuma

Belirli bir bölüm için bir AMQP 1.0 oturumu ve bağlantı açıldıktan sonra olaylar Event Hubs hizmeti tarafından AMQP 1.0 istemcisine teslim edilir. Bu teslim mekanizması, HTTP GET gibi çekme tabanlı mekanizmalardan daha yüksek verimlilik ve daha düşük gecikme sağlar. Olaylar istemciye gönderildiğinde her bir olay verisi örneği, olay dizisinde denetim noktası oluşturmayı kolaylaştırmak için kullanılan uzaklık ve dizi numarası gibi önemli meta veriler içerir.

Olay verileri:
* Uzaklık
* Sıra numarası
* Gövde
* Kullanıcı özellikleri
* Sistem özellikleri

Uzaklığın yönetilmesi sizin sorumluluğunuzdadır.

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

- Event Hubs kullanmaya başlayın
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [Javascript](get-started-java-send-v2.md)
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Olay Hub'ları örnekleri][]

[Olay Hub'ları örnekleri]: https://github.com/Azure/azure-event-hubs/tree/master/samples
