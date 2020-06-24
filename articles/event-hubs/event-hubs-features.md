---
title: Özelliklere genel bakış-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs özellikleri ve terminolojisi hakkında ayrıntılar sağlanmaktadır.
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
ms.openlocfilehash: bca8ccaf06fb63b9029b93a8c59a6304139c8ff1
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84976889"
---
# <a name="features-and-terminology-in-azure-event-hubs"></a>Azure Event Hubs'ın özellikleri ve terminolojisi

Azure Event Hubs, düşük gecikme süresi ve yüksek güvenilirlikle büyük hacimlerdeki olayları ve verileri işleyen ve işleyen ölçeklenebilir bir olay işleme hizmetidir. Üst düzey genel bakış için bkz. [Event Hubs nedir?](event-hubs-what-is-event-hubs.md) .

Bu makale, [genel bakış makalesindeki](event-hubs-what-is-event-hubs.md)bilgileri oluşturur ve Event Hubs bileşenleri ve özellikleri hakkında teknik ve uygulama ayrıntıları sağlar.

## <a name="namespace"></a>Ad Alanı
Bir Event Hubs ad alanı, [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)tarafından başvurulan, bir veya daha fazla olay hub 'ı veya Kafka konu oluşturacağınız benzersiz bir kapsam kapsayıcısı sağlar. 

## <a name="event-hubs-for-apache-kafka"></a>Apache Kafka için Event Hubs

[Bu özellik](event-hubs-for-kafka-ecosystem-overview.md) , müşterilerin Kafka protokolünü kullanarak Event Hubs konuşmalarını sağlayan bir uç nokta sağlar. Bu tümleştirme müşterilere bir Kafka uç noktası sağlar. Bu, müşterilerin, kendi Kafka kümelerini çalıştırmaya alternatif olarak, var olan Kafka uygulamalarını Event Hubs konuşmak üzere yapılandırmalarına olanak sağlar. Apache Kafka için Event Hubs, Kafka Protocol 1,0 ve üstünü destekler. 

Bu tümleştirmeyle Kafka kümelerini çalıştırmanız veya Zookeeper ile yönetmeniz gerekmez. Bu Ayrıca, yakalama, otomatik Şişir ve coğrafi olağanüstü durum kurtarma gibi Event Hubs en zorlu özelliklerden bazıları ile çalışmanıza olanak sağlar.

Bu tümleştirme Ayrıca, yalnızca yapılandırma değişiklikleriyle birlikte Kafka Connect gibi yansıtma Oluşturucu veya çerçeve gibi uygulamalara izin verir. 

## <a name="event-publishers"></a>Olay yayımcıları

Bir olay hub 'ına veri gönderen herhangi bir varlık bir olay üreticisi veya *olay yayımcısıdır*. Olay yayımcıları, HTTPS veya AMQP 1,0 ya da Kafka 1,0 ve üstünü kullanarak olayları yayımlayabilir. Olay yayımcıları kendilerini bir olay hub'ına tanıtmak için Paylaşılan Erişim İmzası (SAS) belirteci kullanır ve benzersiz bir kimliğe sahip olabilir ya da ortak bir SAS belirteci kullanabilir.

### <a name="publishing-an-event"></a>Olay yayımlama

AMQP 1,0, Kafka 1,0 (ve üzeri) veya HTTPS aracılığıyla bir olay yayımlayabilirsiniz. Event Hubs, .NET istemcilerinden gelen bir olay hub 'ına olay yayımlamaya yönelik [istemci kitaplıkları ve sınıflar](event-hubs-dotnet-framework-api-overview.md) sağlar. Diğer çalışma zamanları ve platformlar için [Apache Qpid](https://qpid.apache.org/) gibi herhangi bir AMQP 1.0 istemcisi kullanabilirsiniz. Olayları ayrı ayrı veya toplu olarak yayımlayabilirsiniz. Tek bir yayın (olay verileri örneği), tek bir olay veya toplu iş olmasına bakılmaksızın 1 MB 'lik bir sınıra sahiptir. Bu eşikten daha büyük yayımlama olayları hata ile sonuçlanır. Yayımcıların olay hub'ındaki bölümleri bilmemesi ve yalnızca bir *bölüm anahtarı* (sonraki bölümde açıklanmıştır) ya da kimliklerini SAS belirteci üzerinden belirtmeleri en iyi yöntemdir.

AMQP veya HTTPS kullanma seçimi kullanım senaryosuna bağlıdır. AMQP, taşıma düzeyi güvenliği (TLS) veya SSL/TLS’ye ek olarak kalıcı bir çift yönlü yuva oluşturulmasını gerektirir. AMQP, oturum başlatırken daha yüksek ağ maliyetlerine sahiptir, ancak HTTPS her istek için ek TLS yükü gerektirir. Daha sık yayımcılar için AMQP daha yüksek performans sunar.

![Event Hubs](./media/event-hubs-features/partition_keys.png)

Event Hubs aynı bölüm anahtarı değerini paylaşan tüm olayların sırayla ve aynı bölüme iletilmesini sağlar. Bölüm anahtarlarının yayımcı ilkeleriyle birlikte kullanılması durumunda yayımcı kimliğinin ve bölüm anahtarı değerinin eşleşmesi gerekir. Aksi takdirde bir hata oluşur.

### <a name="publisher-policy"></a>Yayımcı ilkesi

Event Hubs, *yayımcı ilkeleri* aracılığıyla olay yayımcıları üzerinde ayrıntılı denetim sağlar. Yayımcı ilkeleri çok sayıda bağımsız olay yayımcısını kolaylaştırmak için tasarlanmış çalışma zamanı özellikleridir. Yayımcı ilkeleriyle her yayımcı, olayları aşağıdaki mekanizmayı kullanarak bir olay hub'ında yayımlarken kendi benzersiz tanımlayıcısını kullanır:

```http
//<my namespace>.servicebus.windows.net/<event hub name>/publishers/<my publisher name>
```

Yayımcı adlarını önceden oluşturmanız gerekli değildir, ancak bunlar bağımsız yayımcı kimlikleri sağlamak amacıyla bir olayı yayımlarken kullanılan SAS belirteci ile eşleşmelidir. Yayımcı ilkelerini kullanırken **PartitionKey** değeri yayımcı adına ayarlanır. Bu hizmetin düzgün çalışması için bu değerlerin eşleşmesi gerekir.

## <a name="capture"></a>Capture

[Event Hubs yakalama](event-hubs-capture-overview.md) , akış verilerini Event Hubs otomatik olarak yakalamanızı ve bir BLOB depolama hesabı ya da bir Azure Data Lake hizmet hesabına kaydetmenizi sağlar. Azure portal yakalamayı etkinleştirebilir ve yakalama işlemini gerçekleştirmek için bir minimum boyut ve zaman penceresi belirtebilirsiniz. Event Hubs yakalama 'yı kullanarak, kendi Azure Blob depolama hesabınızı ve kapsayıcınızı ya da yakalanan verileri depolamak için kullanılan bir hizmet hesabı Azure Data Lake belirtirsiniz. Yakalanan veriler Apache avro biçiminde yazılır.

## <a name="partitions"></a>Bölümler
[!INCLUDE [event-hubs-partitions](../../includes/event-hubs-partitions.md)]


## <a name="sas-tokens"></a>SAS belirteçleri

Event Hubs, ad alanı ve Olay Hub 'ı düzeyinde kullanılabilen *paylaşılan erişim imzalarını*kullanır. SAS belirteci bir SAS anahtarından oluşturulur ve belirli bir biçimde kodlanmış bir URL’nin SHA karmasıdır. Event Hubs anahtar (ilke) ve belirtecin adını kullanarak karmayı yeniden oluşturabilir ve böylece gönderenin kimliğini doğrular. Normalde, olay yayımcıları için SAS belirteci yalnızca belirli bir olay hub'ı üzerindeki **gönder** ayrıcalıkları ile oluşturulur. Bu SAS belirteci URL mekanizması, yayımcı ilkesinde sunulan yayımcı kimliğinin temelini oluşturur. SAS ile çalışma hakkında daha fazla bilgi için bkz. [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması](../service-bus-messaging/service-bus-sas.md).

## <a name="event-consumers"></a>Olay tüketicileri

Bir olay hub'ından olay verilerini okuyan herhangi bir varlık *olay tüketicisidir*. Tüm Event Hubs tüketicileri AMQP 1.0 oturumu üzerinden bağlanır ve olaylar, kullanılabilir olduğu anda oturum üzerinden iletilir. İstemcinin veri kullanılabilirliğini yoklaması gerekmez.

### <a name="consumer-groups"></a>Tüketici grupları

Event Hubs yayımlama/abonelik mekanizması *tüketici grupları* aracılığıyla etkinleştirilir. Tüketici grubu tüm olay hub'ının bir görünümüdür (durum, konum veya uzaklık). Tüketici grupları birden çok tüketen uygulamayı her biri olay akışının ayrı bir görünümüne sahip olacak ve akışı kendi hızlarında ve kendi sapmalarıyla bağımsız bir şekilde okuyacak şekilde etkinleştirir.

Bir akış işleme mimarisinde her bir aşağı akış uygulaması bir tüketici grubuna karşılık gelir. Olay verilerini uzun süreli depolama alanına yazmak isterseniz bu depolama yazma uygulaması bir tüketici grubudur. Bundan sonra karmaşık olay işlemesi başka ve ayrı bir tüketici grubu tarafından gerçekleştirilebilir. Bölümlere yalnızca bir tüketici grubu üzerinden erişebilirsiniz. Bir olay hub'ında her zaman varsayılan bir tüketici grubu vardır ve Standart katmanlı bir olay hub'ı için en fazla 20 tüketici grubu oluşturabilirsiniz.

Tüketici grubu başına bölüm üzerinde en fazla 5 eşzamanlı okuyucu olabilir; Ancak **, Tüketici grubu başına bir bölümde yalnızca bir etkin alıcı olması önerilir**. Tek bir bölüm içinde her okuyucu tüm iletileri alır. Aynı bölümde birden fazla okuyucu varsa, yinelenen iletileri işleyebilirsiniz. Kodunuzda bu, önemsiz olmayan bir işlem yapmanız gerekir. Ancak, bazı senaryolarda bu geçerli bir yaklaşımdır.

Azure SDK 'Ları tarafından sunulan bazı istemciler, her bir bölümün tek bir okuyucu içerdiğinden ve bir olay hub 'ının tüm bölümlerinin okunmakta olduğundan emin olmanın ayrıntılarını otomatik olarak yöneten akıllı tüketici aracılarıdır. Bu, kodunuzun, bölümlerin ayrıntılarının çoğunu yoksayabilmesi için Olay Hub 'ından okunan olayların işlenmesine odaklanmasını sağlar. Daha fazla bilgi için bkz. [bir bölüme bağlanma](#connect-to-a-partition).

Aşağıdaki örneklerde, Tüketici grubu URI kuralı gösterilmektedir:

```http
//<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #1>
//<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #2>
```

Aşağıdaki şekilde Event Hubs akış işleme mimarisi gösterilmektedir:

![Event Hubs](./media/event-hubs-features/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Akış uzaklıkları

*Uzaklık* bir olayın bölüm içindeki konumudur. Uzaklığı istemci tarafındaki bir imleç olarak düşünebilirsiniz. Uzaklık, olayın bayt cinsinden numaralandırılmasıdır. Bu uzaklık, olay tüketicisinin (okuyucu) olay akışında olayları okumaya başlamak istediği bir noktayı belirtmesini sağlar. Uzaklığı bir zaman damgası veya bir uzaklık değeri olarak belirtebilirsiniz. Tüketiciler, kendi uzaklık değerlerini Event Hubs hizmetinin dışında saklamaktan sorumludur. Bir bölüm içinde her olay bir uzaklık içerir.

![Event Hubs](./media/event-hubs-features/partition_offset.png)

### <a name="checkpointing"></a>Denetim noktası oluşturma

*Denetim noktası oluşturma*, okuyucuların bir bölüm olay dizisindeki konumlarını işaretledikleri veya uyguladıkları bir işlemdir. Denetim noktası oluşturma, tüketicinin sorumluluğundadır ve bir tüketici grubunda bölüm başına temelinde gerçekleşir. Bu sorumluluk, her bir tüketici grubu için her bölüm okuyucusunun geçerli konumunu olay akışında izlemesi gerektiği ve veri akışının tamamlandığını düşündüğünde hizmeti bilgilendirebileceği anlamına gelir.

Bir okuyucunun bölüm bağlantısı kesilirse yeniden bağlandığında ilgili tüketici grubundaki o bölümün son okuyucusu tarafından daha önce gönderilen denetim noktasında okumaya başlar. Okuyucu bağlandığı zaman, okumayı başlatacak konumu belirtmek için Olay Hub 'ına geçişi geçirir. Bu şekilde, denetim noktası oluşturma özelliğini hem aşağı akış uygulamaları ile olayları "tamamlandı" olarak işaretlemek hem de farklı makinelerde çalışan okuyucular arasında bir yük devretme oluşması durumunda esneklik sağlamak amacıyla kullanabilirsiniz. Bu denetim noktası oluşturma işleminden daha düşük bir uzaklık belirterek daha eski verilere geri dönülebilir. Bu mekanizmayla denetim noktası oluşturma özelliği hem yük devretme esnekliği hem de olay akışı yeniden yürütmesi sağlar.

> [!NOTE]
> Azure Blob Storage 'ı, Azure 'da bulunan farklı bir Storage blob SDK sürümünü destekleyen bir ortamda denetim noktası deposu olarak kullanıyorsanız, depolama hizmeti API sürümünü bu ortam tarafından desteklenen belirli bir sürümle değiştirmek için kodu kullanmanız gerekir. Örneğin, [Event Hubs bir Azure Stack hub sürümü 2002](https://docs.microsoft.com/azure-stack/user/event-hubs-overview)çalıştırıyorsanız, depolama hizmeti için en yüksek sürüm 2017-11-09 ' dir. Bu durumda, Storage Service API sürümünü 2017-11-09 'e hedeflemek için kodu kullanmanız gerekir. Belirli bir depolama API sürümünün nasıl hedeflenecek hakkında bir örnek için GitHub 'da şu örneklere bakın: 
> - [.Net](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample10_RunningWithDifferentStorageVersion.cs). 
> - [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/)
> - [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript) veya [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/typescript)
> - [Python](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/)

### <a name="common-consumer-tasks"></a>Ortak tüketici görevleri

Tüm Event Hubs tüketicileri, durum algılayan çift yönlü iletişim kanalı olan bir AMQP 1,0 oturumu aracılığıyla bağlanır. Her bölümde bölüme göre ayrılmış olayların taşınmasını kolaylaştıran bir AMQP 1.0 oturumu vardır.

#### <a name="connect-to-a-partition"></a>Bir bölüme bağlanma

Bölümlere bağlanırken, okuyucu bağlantılarını belirli bölümlerle koordine etmek için bir kiralama mekanizması kullanılması yaygın bir uygulamadır. Bu şekilde, bir tüketici grubundaki her bölümün yalnızca bir etkin okuyucu olması mümkündür. Akıllı tüketici aracıları işlevi gören Event Hubs SDK 'lar içindeki istemcileri kullanarak denetim noktası oluşturma, kiralama ve yönetme, okuyucuları basitleştirilir. Bunlar:

- .NET için [Eventprocessorclient](/dotnet/api/azure.messaging.eventhubs.eventprocessorclient)
- Java için [Eventprocessorclient](/java/api/com.azure.messaging.eventhubs.eventprocessorclient)
- Python için [Eventhubconsumerclient](/python/api/azure-eventhub/azure.eventhub.aio.eventhubconsumerclient)
- JavaScript/TypeScript için [Eventhubsonkısaerclient](/javascript/api/@azure/event-hubs/eventhubconsumerclient)

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
    - [.NET](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [JavaScript](get-started-java-send-v2.md)
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs örnekleri](event-hubs-samples.md)
