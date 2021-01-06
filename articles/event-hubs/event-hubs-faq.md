---
title: Sık sorulan sorular-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs ve yanıtları hakkında sık sorulan soruların (SSS) bir listesi sunulmaktadır.
ms.topic: article
ms.date: 10/27/2020
ms.openlocfilehash: e7a34fe0f2ef04fffeeddc5615d3ac1749467902
ms.sourcegitcommit: 19ffdad48bc4caca8f93c3b067d1cf29234fef47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97955425"
---
# <a name="event-hubs-frequently-asked-questions"></a>Event Hubs sık sorulan sorular

## <a name="general"></a>Genel

### <a name="what-is-an-event-hubs-namespace"></a>Event Hubs ad alanı nedir?
Ad alanı, Olay Hub 'ı/Kafka konuları için kapsam bir kapsayıcıdır. Size benzersiz bir [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)sağlar. Bir ad alanı, birden çok olay hub 'ı/Kafka konu başlığı veren bir uygulama kapsayıcısı işlevi görür. 

### <a name="when-do-i-create-a-new-namespace-vs-use-an-existing-namespace"></a>Ne zaman yeni bir ad alanı oluşturabilirim? var olan bir ad alanını kullan?
Kapasite ayırmaları ([işleme birimleri (DTU 'lar)](#throughput-units)) ad alanı düzeyinde faturalandırılır. Bir ad alanı aynı zamanda bir bölgeyle ilişkilendirilir.

Aşağıdaki senaryolardan birinde var olan birini kullanmak yerine yeni bir ad alanı oluşturmak isteyebilirsiniz: 

- Yeni bir bölgeyle ilişkili bir olay hub 'ı gerekir.
- Farklı bir abonelikle ilişkili bir olay hub 'ı gerekir.
- Ayrı kapasite ayırması olan bir olay hub 'ınız olması gerekir (yani, eklenen Olay Hub 'ına sahip ad alanı için kapasite gereksinimi 40 TU eşiğini aşacağından ve adanmış kümeye gitmek istemezsiniz)  

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Event Hubs temel ve standart Katmanlar arasındaki fark nedir?

Azure Event Hubs standart katmanı, temel katmanda kullanılabilir olan özelliklerden daha fazla özellik sağlar. Aşağıdaki özellikler standart ile birlikte verilmiştir:

* Daha uzun olay saklama
* Dahil edilen sayıdan daha fazla ücret ödemelerine sahip ek aracılı bağlantılar
* Tek bir [Tüketici grubundan](event-hubs-features.md#consumer-groups) daha fazla
* [Yakalama](event-hubs-capture-overview.md)
* [Kafka tümleştirmesi](event-hubs-for-kafka-ecosystem-overview.md)

Event Hubs Ayrılmış dahil olmak üzere fiyatlandırma katmanları hakkında daha fazla bilgi için [Event Hubs fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/event-hubs/)bakın.

### <a name="where-is-azure-event-hubs-available"></a>Azure Event Hubs nerede kullanılabilir?

Azure Event Hubs, desteklenen tüm Azure bölgelerinde kullanılabilir. Bir liste için, [Azure bölgeleri](https://azure.microsoft.com/regions/) sayfasını ziyaret edin.  

### <a name="can-i-use-a-single-advanced-message-queuing-protocol-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Birden çok olay hub 'ını göndermek ve almak için tek bir Gelişmiş İleti Sıraya Alma Protokolü (AMQP) bağlantısı kullanabilir miyim?

Evet, tüm olay hub 'ları aynı ad alanında olduğu sürece.

### <a name="what-is-the-maximum-retention-period-for-events"></a>Olaylar için en fazla saklama süresi nedir?

Event Hubs Standart katman Şu anda en fazla yedi gün bekletme süresini desteklemektedir. Olay Hub 'ları kalıcı veri deposu olarak tasarlanmamıştır. 24 saatten büyük saklama süreleri, bir olay akışını aynı sistemlere yeniden yürütmeye uygun olan senaryolar için tasarlanmıştır. Örneğin, var olan veriler üzerinde yeni bir makine öğrenimi modeli eğitme veya doğrulama. Yedi günden daha fazla süre boyunca ileti bekletmesi gerekiyorsa, Olay Hub 'ınızdaki [Event Hubs yakalamayı](event-hubs-capture-overview.md) etkinleştirmek, Olay Hub 'ınızdaki verileri seçtiğiniz depolama hesabına veya Azure Data Lake hizmet hesabına çeker. Yakalamanın etkinleştirilmesi, satın alınan üretilen iş birimleriniz temelinde bir ücret doğurur.

Depolama hesabınızdaki yakalanan veriler için saklama süresini yapılandırabilirsiniz. Azure depolama 'nın **yaşam döngüsü yönetimi** özelliği, genel amaçlı v2 ve BLOB depolama hesapları için zengin, kural tabanlı bir ilke sunar. Verilerinizi uygun erişim katmanlarına geçirmeye veya veri yaşam döngüsünün sonunda sona ermesini sağlamak için ilkeyi kullanın. Daha fazla bilgi için bkz. [Azure Blob depolama yaşam döngüsünü yönetme](../storage/blobs/storage-lifecycle-management-concepts.md). 

### <a name="how-do-i-monitor-my-event-hubs"></a>Event Hubs izleme Nasıl yaparım??
Event Hubs kaynaklarınızın durumunu [Azure izleyici](../azure-monitor/overview.md)'ye sağlayan ayrıntılı ölçümleri yayar. Ayrıca, yalnızca ad alanı düzeyinde değil, varlık düzeyinde de Event Hubs hizmetin genel durumunu değerlendirmenize imkan tanır. [Azure Event Hubs](event-hubs-metrics-azure-monitor.md)için hangi izlemenin sunulmakta olduğunu öğrenin.

### <a name="where-does-azure-event-hubs-store-customer-data"></a><a name="in-region-data-residency"></a>Azure Event Hubs müşteri verilerini nerede depolar?
Azure Event Hubs, müşteri verilerini depolar. Bu veriler, tek bir bölgede Event Hubs tarafından otomatik olarak depolanır. bu nedenle, bu hizmet [Güven Merkezi](https://azuredatacentermap.azurewebsites.net/)'nde belirtilenler dahil olmak üzere bölge verileri 'nin gereksinimlerini otomatik olarak karşılar.

[!INCLUDE [event-hubs-connectivity](../../includes/event-hubs-connectivity.md)]

## <a name="apache-kafka-integration"></a>Apache Kafka tümleştirme

### <a name="how-do-i-integrate-my-existing-kafka-application-with-event-hubs"></a>Mevcut Kafka uygulamamı Event Hubs ile tümleştirmi Nasıl yaparım??
Event Hubs, mevcut Apache Kafka tabanlı uygulamalarınız tarafından kullanılabilecek bir Kafka uç noktası sağlar. PaaS Kafka deneyimine sahip olmak için gereken bir yapılandırma değişikliği vardır. Kendi Kafka kümenizi çalıştırmaya bir alternatif sağlar. Event Hubs, Apache Kafka 1,0 ve daha yeni istemci sürümlerini destekler ve var olan Kafka uygulamalarınız, araçlarınızla ve çerçevelerinizdeki ile birlikte kullanılabilir. Daha fazla bilgi için bkz. [Kafka for Event Hubs deposu](https://github.com/Azure/azure-event-hubs-for-kafka).

### <a name="what-configuration-changes-need-to-be-done-for-my-existing-application-to-talk-to-event-hubs"></a>Mevcut uygulamamın Event Hubs ile iletişim kurmasını sağlamak için hangi yapılandırma değişikliklerinin yapılması gerekir?
Bir olay hub 'ına bağlanmak için Kafka istemci yapılandırması ' nı güncelleştirmeniz gerekir. Event Hubs bir ad alanı oluşturarak ve [bağlantı dizesi](event-hubs-get-connection-string.md)elde ederek yapılır. Önyükleme. sunucuları Event Hubs FQDN ve bağlantı noktası 9093 ' ye işaret etmek üzere değiştirin. Kafka istemcisini, aşağıda gösterildiği gibi doğru kimlik doğrulamasıyla Event Hubs uç noktanıza (aldığınız bağlantı dizesi) yönlendirmek için sasl.jaas.config güncelleştirin:

```properties
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
request.timeout.ms=60000
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

Örnek:

```properties
bootstrap.servers=dummynamespace.servicebus.windows.net:9093
request.timeout.ms=60000
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXX";
```
Note: sasl.jaas.config ortamınızda desteklenen bir yapılandırma değilse, SASL Kullanıcı adı ve parolasını ayarlamak için kullanılan yapılandırmaları bulun ve bunların yerine kullanın. Kullanıcı adını $ConnectionString ve parolayı Event Hubs bağlantı dizeniz olarak ayarlayın.

### <a name="what-is-the-messageevent-size-for-event-hubs"></a>Event Hubs için ileti/olay boyutu nedir?
Event Hubs için izin verilen en büyük ileti boyutu 1 MB 'tır.

## <a name="throughput-units"></a>İşleme birimleri

### <a name="what-are-event-hubs-throughput-units"></a>Event Hubs üretilen iş birimi nedir?
Event Hubs aktarım hızı, Event Hubs gelen ve çıkış yapan 1 KB 'lik olayların sayısını mega bayt cinsinden veya sayı olarak tanımlar (binlerce). Bu aktarım hızı, üretilen iş birimleri (DTU 'lar) cinsinden ölçülür. Event Hubs hizmetini kullanmaya başlayabilmeniz için önce satın alma. Portal veya Event Hubs Kaynak Yöneticisi şablonları kullanarak Event Hubs tüs 'yi açık olarak seçebilirsiniz. 


### <a name="do-throughput-units-apply-to-all-event-hubs-in-a-namespace"></a>Üretilen iş birimleri, bir ad alanındaki tüm olay hub 'larına uygulanabilir mi?
Evet, üretilen iş birimleri (DTU 'lar) bir Event Hubs ad alanındaki tüm olay hub 'larına uygulanır. Bu, ad alanı düzeyinde bir tüs satın almanızı ve bu ad alanı altındaki Olay Hub 'ları arasında paylaşılmanızı anlamına gelir. Her bir TU, ad alanını aşağıdaki yetenekler sahibine:

- Saniyede 1 MB 'a kadar giriş olayı (bir olay hub 'ına gönderilen olaylar), ancak saniyede 1000 giriş olayı, yönetim işlemi veya denetim API çağrısı yok.
- Saniyede 2 MB 'a kadar çıkış olayı (bir olay hub 'ından tüketilen olaylar), ancak 4096 'den fazla çıkış olayı yoktur.
- 84 GB 'a kadar olay depolama (varsayılan 24 saatlik saklama süresi için yeterlidir).

### <a name="how-are-throughput-units-billed"></a>Üretilen iş birimleri nasıl faturalandırılır?
Üretilen iş birimleri (DTU 'lar) saatlik olarak faturalandırılır. Faturalandırma, belirtilen saat boyunca seçilen maksimum birim sayısına göre belirlenir. 

### <a name="how-can-i-optimize-the-usage-on-my-throughput-units"></a>Verimlilik birimlerimde kullanımı nasıl iyileştirebilirim?
Bir üretilen iş birimi (TU) kadar düşük olarak başlayabilir ve [Otomatik Şişir](event-hubs-auto-inflate.md)özelliğini açabilirsiniz. Otomatik Şişir özelliği, trafik/Yük arttıkça iş yüklerinizi büyütmenize olanak tanır. Ayrıca, DTU sayısında bir üst sınır belirleyebilirsiniz.

### <a name="how-does-auto-inflate-feature-of-event-hubs-work"></a>Event Hubs otomatik olarak Şişir özelliği nasıl çalışır?
Otomatik Şişir özelliği, üretilen iş birimlerinizi (DTU 'lar) ölçeklendirmenize olanak tanır. Bu, düşük artış satın alarak başlayabileceğiniz anlamına gelir ve Alım arttıkça, otomatik şişillerinizi ölçeklendirin. Bu, size uygun maliyetli bir seçenek ve yönetilecek DTU sayısı denetimini tam olarak kontrol etmenizi sağlar. Bu özellik **yalnızca ölçeği artırma** özelliğine sahiptir ve güncelleyerek, bu sayının ölçeğini tamamen kontrol edebilirsiniz. 

Düşük işleme birimleri (DTU 'lar) ile başlamak isteyebilirsiniz, örneğin, 2. Trafiğinizin 15 ' e büyüyebileceğini tahmin ediyorsanız, ad uzayındaki otomatik Şişir özelliğini açın ve en yüksek sınırı 15 ' e ayarlayın. Artık, trafiğiniz büyüdükçe, ne kadar olan değerleri otomatik olarak büyütün.

### <a name="is-there-a-cost-associated-when-i-turn-on-the-auto-inflate-feature"></a>Otomatik Şişir özelliğini açıp, ilişkili bir maliyet var mı?
Bu özellikle ilişkili bir **maliyet yoktur** . 

### <a name="how-are-throughput-limits-enforced"></a>Verimlilik limitleri nasıl zorlanır?
Bir ad alanındaki tüm olay hub 'ları genelinde toplam giriş aktarım hızı veya toplam giriş olayı **oranı toplam aktarım** hızı birimi kullanım sayısını aşarsa, Gönderenler kısıtlanır ve giriş kotasının aşıldığını belirten hatalar alır.

Bir ad alanındaki tüm olay hub 'larının toplam **Çıkış** üretilen işi veya toplam olay çıkış oranı toplam aktarım hızı birimi kullanım sayısını aşarsa, alıcılar kısıtlanır, ancak hiçbir azaltma hatası oluşturulmaz. 

Giriş ve çıkış kotaları ayrı ayrı uygulanır; böylece hiçbir Gönderen olay tüketiminin yavaşlamasına veya bir alıcı olayların bir olay hub 'ına gönderilmesini engelleyebilir.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-reservedselected"></a>Ayrılan/seçili olabilecek üretilen iş birimi sayısı için bir sınır var mıdır?

Azure portal temel veya Standart katman ad alanı oluştururken ad alanı için en fazla 20 ' yi seçebilirsiniz. Bunu **tam** 40 bir şekilde yükseltmek için bir  [destek isteği](../azure-portal/supportability/how-to-create-azure-support-request.md)gönderebilirsiniz.  

1. **Olay veri yolu ad alanı** sayfasında, sol taraftaki menüden **Yeni destek isteği** ' ni seçin. 
1. **Yeni destek isteği** sayfasında, aşağıdaki adımları izleyin:
    1. **Özet** için, sorunu birkaç sözcükten açıklama. 
    1. **Sorun türü** için **Kota**' i seçin. 
    1. **Sorun alt türü** için, **Aktarım hızı birimi artışı veya azaltma isteği**' ni seçin. 
    
        :::image type="content" source="./media/event-hubs-faq/support-request-throughput-units.png" alt-text="Destek isteği sayfası":::

Event Hubs 40 ' nin ötesinde, Event Hubs Ayrılmış kümeleri olarak adlandırılan kaynak/kapasite tabanlı modeli sunar. Adanmış kümeler kapasite birimlerinde (cu düzeyinde kapsanır) satılır. Daha fazla bilgi için bkz. [Event Hubs ayrılmış-genel bakış](event-hubs-dedicated-overview.md).

## <a name="dedicated-clusters"></a>Ayrılmış kümeler

### <a name="what-are-event-hubs-dedicated-clusters"></a>Event Hubs Ayrılmış kümeleri nedir?
Event Hubs Ayrılmış kümeler, en zorlu gereksinimleri olan müşteriler için tek kiracılı dağıtımlar sunar. Bu teklif, işleme birimleri tarafından bağlı olmayan kapasite tabanlı bir küme oluşturur. Bu, kümenin CPU ve bellek kullanımı tarafından dikte edildiği şekilde verilerinizi almak ve aktarmak için kümeyi kullanabileceğiniz anlamına gelir. Daha fazla bilgi için bkz. [Event Hubs ayrılmış kümeleri](event-hubs-dedicated-overview.md).

### <a name="how-do-i-create-an-event-hubs-dedicated-cluster"></a>Nasıl yaparım? bir Event Hubs Ayrılmış kümesi mi oluşturulsun?
Event Hubs adanmış bir küme ayarlamaya yönelik adım adım yönergeler ve daha fazla bilgi için bkz. [hızlı başlangıç: Azure Portal kullanarak adanmış Event Hubs kümesi oluşturma](event-hubs-dedicated-cluster-create-portal.md). 


[!INCLUDE [event-hubs-dedicated-clusters-faq](../../includes/event-hubs-dedicated-clusters-faq.md)]


## <a name="partitions"></a>Bölümler

### <a name="how-many-partitions-do-i-need"></a>Kaç bölüme ihtiyacım var?
Bölüm sayısı, oluşturma sırasında belirtilmiştir ve 1 ile 32 arasında olmalıdır. Bölüm sayısı [adanmış katman](event-hubs-dedicated-overview.md)hariç tüm katmanlarda değiştirilemez olmadığından, bölüm sayısını ayarlarken uzun vadeli ölçeği dikkate almanız gerekir. Bölümler, tüketen uygulamalarda gerekli aşağı akış paralelliğiyle ilişkili bir veri düzenleme mekanizmasıdır. Bir olay hub'ındaki bölüm sayısı, sahip olmayı beklediğiniz eşzamanlı okuyucu sayısıyla doğrudan ilgilidir. Bölümler hakkında daha fazla bilgi için bkz. [bölümler](event-hubs-features.md#partitions).

Oluşturma sırasında 32 olan mümkün olan en yüksek değer olarak ayarlamak isteyebilirsiniz. Göndericilerin, geri kalan 31 bölümden oluşan 32 ' dan yalnızca tek bir bölüme gönderilmesi için yapılandırmadığınız müddetçe, birden fazla bölüme sahip olmanın sırayı korumadan birden çok bölüme gönderilmesine neden olacağını unutmayın. Önceki durumda, tüm 32 bölümlerdeki olayları okumanız gerekir. İkinci durumda, olay Işlemcisi ana bilgisayarında yapmanız gerekek yapılandırmadan farklı ek maliyet yoktur.

Event Hubs, Tüketici grubu başına tek bir bölüm okuyucusuna izin verecek şekilde tasarlanmıştır. Çoğu kullanım durumunda, varsayılan dört bölüm ayarı yeterlidir. Olay işlemenin ölçeğini ölçeklendirmek istiyorsanız ek bölümler eklemeyi göz önünde bulundurmanız gerekebilir. Bir bölümde belirli bir işleme sınırı yoktur, ancak ad uzayındaki toplam aktarım hızı, üretilen iş birimi sayısıyla sınırlıdır. Ad uzayındaki üretilen iş birimi sayısını artırdıkça, ek bölümlerin, eşzamanlı okuyucuların en yüksek aktarım hızını elde etmelerini sağlamak isteyebilirsiniz.

Ancak, uygulamanızın belirli bir bölüme benzeşim içeren bir modeliniz varsa, bölüm sayısının artırılması sizin için herhangi bir avantaj olmayabilir. Daha fazla bilgi için bkz. [kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md).

### <a name="increase-partitions"></a>Bölümleri artırma
Bir destek isteği göndererek bölüm sayısının 40 ' e (tam) yükseldiğini isteyebilirsiniz. 

1. **Olay veri yolu ad alanı** sayfasında, sol taraftaki menüden **Yeni destek isteği** ' ni seçin. 
1. **Yeni destek isteği** sayfasında, aşağıdaki adımları izleyin:
    1. **Özet** için, sorunu birkaç sözcükten açıklama. 
    1. **Sorun türü** için **Kota**' i seçin. 
    1. **Sorun alt türü** için, **bölüm değişikliği isteği**' ni seçin. 
    
        :::image type="content" source="./media/event-hubs-faq/support-request-increase-partitions.png" alt-text="Bölüm sayısını artır":::

Bölüm sayısı tam olarak 40 yükseltilebilir. Bu durumda, Ayrıca, 40 sayısının de arttığı bir artış vardır. Daha sonra TU limitini <= 20 ' ye düşürürseniz, en yüksek bölüm sayısı da 32 olarak azaltılır. 

Bölümler Olay Hub 'ı düzeyinde uygulandığından ve hub oluşturulduktan sonra sabit olmaları nedeniyle bölümlerdeki azalmaları var olan olay hub 'larını etkilemez. 

## <a name="pricing"></a>Fiyatlandırma

### <a name="where-can-i-find-more-pricing-information"></a>Daha fazla fiyatlandırma bilgilerini nerede bulabilirim?

Event Hubs fiyatlandırmasıyla ilgili tüm bilgiler için [Event Hubs fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/event-hubs/)bakın.

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Event Hubs olaylarını 24 saatten fazla saklamak için ücret alınır mi?

Event Hubs standart katmanı, en fazla yedi gün boyunca, 24 saatten daha uzun mesaj bekletme dönemlerine izin verir. Depolanan olayların toplam sayısının boyutu, seçilen üretilen iş birimi sayısı (üretilen iş birimi başına 84 GB) için depolama limitini aşarsa, indirimi aşan boyut yayımlanan Azure Blob depolama fiyatı üzerinden ücretlendirilir. Her bir üretilen iş birimi için depolama alanı kullanım süresi, aktarım hızı birimi en fazla giriş kesintisinden kullanılsa bile, 24 saatin (varsayılan) bekletme dönemlerine yönelik tüm depolama maliyetlerini içerir.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Event Hubs depolama boyutu nasıl hesaplanıp ücretlendirilir?

Tüm kayıtlı olayların, olay üst bilgileri veya tüm olay hub 'larında disk depolama yapıları dahil olmak üzere, tüm saklanan olayların toplam boyutu gün boyunca ölçülür. Günün sonunda, yoğun depolama boyutu hesaplanır. Günlük depolama alanı tahsisatı, gün içinde seçilen en az üretilen iş birimi sayısına göre hesaplanır (her üretilen iş birimi, 84 GB bir kesinti sağlar). Toplam boyut, hesaplanan günlük depolama indirimini aşarsa, fazla depolama, Azure Blob Depolama ücretleri ( **yerel olarak yedekli depolama** fiyatı) kullanılarak faturalandırılır.

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Event Hubs giriş olayları nasıl hesaplanır?

Bir olay hub 'ına gönderilen her olay faturalandırılabilir ileti olarak sayılır. Giriş *olayı* , 64 KB 'tan küçük veya buna eşit bir veri birimi olarak tanımlanır. Boyutu 64 KB 'tan küçük veya buna eşit olan herhangi bir olay, faturalandırılabilir bir olay olarak kabul edilir. Olay 64 KB 'tan büyükse, faturalanabilir olay sayısı, 128 KB 64 'ın katları halinde olay boyutuna göre hesaplanır. Örneğin, Olay Hub 'ına gönderilen 8 KB 'lik bir olay bir olay olarak faturalandırılır, ancak olay hub 'ına gönderilen 96 KB 'lik bir ileti iki olay olarak faturalandırılır.

Bir olay hub 'ından tüketilen olaylar ve kontrol noktaları gibi yönetim işlemleri ve denetim görüşmeleri, faturalandırılabilir giriş olayları olarak sayılmaz, ancak üretilen iş birimi tahsisatlarına tahakkuk edilmez.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Aracılı bağlantı ücretleri Event Hubs için geçerlidir mi?

Bağlantı ücretleri yalnızca AMQP protokolü kullanıldığında geçerlidir. Gönderme sistemi veya cihaz sayısından bağımsız olarak, HTTP kullanarak olay göndermeye yönelik bir bağlantı ücreti yoktur. AMQP kullanmayı planlıyorsanız (örneğin, daha verimli olay akışı elde etmek veya IoT komutunda ve denetim senaryolarında iki yönlü iletişimi etkinleştirmek için), her bir hizmet katmanına kaç bağlantı dahil edildiğini öğrenmek için [Event Hubs fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/event-hubs/) sayfasına bakın.

### <a name="how-is-event-hubs-capture-billed"></a>Event Hubs yakalama nasıl faturalandırılır?

Ad alanındaki herhangi bir olay hub 'ında yakalama seçeneği etkinken yakalama etkinleştirilir. Event Hubs yakalama, satın alınan üretilen iş birimi başına saatlik olarak faturalandırılır. Üretilen iş birimi sayısı arttırılır veya azaldıkça, Event Hubs yakalama faturalandırması bu değişiklikleri tam saat artışlarla yansıtır. Event Hubs yakalama faturalaması hakkında daha fazla bilgi için bkz. [Event Hubs fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="do-i-get-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>Event Hubs yakalama için seçdiğim depolama hesabı için faturalandırılırım mı?

Yakalama, bir olay hub 'ında etkinleştirildiğinde sağladığınız bir depolama hesabını kullanır. Depolama hesabınız olduğu için, bu yapılandırmayla ilgili tüm değişiklikler Azure aboneliğinize faturalandırılır.

## <a name="quotas"></a>Kotalar

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Event Hubs ilişkili kotalar var mı?

Tüm Event Hubs kotaları listesi için bkz. [Kotalar](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Sorun giderme

### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>Neden bir ad alanını başka bir abonelikten sildikten sonra oluşturamadım? 
Bir aboneliğden bir ad alanını sildiğinizde, başka bir abonelikte aynı adla yeniden oluşturmadan önce 4 saat bekleyin. Aksi takdirde, aşağıdaki hata iletisini alabilirsiniz: `Namespace already exists` . 

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Event Hubs tarafından oluşturulan özel durumların bazıları ve önerilen eylemleri nelerdir?

Olası Event Hubs özel durumların listesi için bkz. [özel durumlara genel bakış](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Event Hubs iki tür [Tanılama günlüğünü](event-hubs-diagnostic-logs.md) destekler-her ikisi de JSON içinde temsil edilen ve Azure Portal aracılığıyla açılabilir hata günlüklerini ve işlemsel günlükleri yakalar.

### <a name="support-and-sla"></a>Destek ve SLA

[Azure Service Bus Için Microsoft Q&soru sayfası](/answers/topics/azure-service-bus.html)aracılığıyla Event Hubs için teknik destek sunulmaktadır. Faturalandırma ve abonelik yönetim desteği ücretsiz olarak sunulmaktadır.

SLA 'umuz hakkında daha fazla bilgi edinmek için bkz. [hizmet düzeyi anlaşmaları](https://azure.microsoft.com/support/legal/sla/) sayfası.

## <a name="azure-stack-hub"></a>Azure Stack Hub

### <a name="how-can-i-target-a-specific-version-of-azure-storage-sdk-when-using-azure-blob-storage-as-a-checkpoint-store"></a>Azure Blob depolamayı bir denetim noktası deposu olarak kullanırken Azure Storage SDK 'sının belirli bir sürümünü nasıl hedefleyebilirsiniz?
Bu kodu Azure Stack hub 'da çalıştırırsanız, belirli bir depolama API sürümünü hedefetmediğiniz takdirde çalışma zamanı hatalarıyla karşılaşırsınız. Bunun nedeni, Event Hubs SDK 'sının Azure 'da kullanılabilen ve Azure Stack hub platformunda kullanılamayan en son Azure Storage API 'sini kullanması nedeniyle oluşur. Azure Stack hub, Azure 'da genel kullanıma sunulan farklı bir Depolama Blobu SDK sürümü destekleyebilir. Azure blog depolamayı bir denetim noktası deposu olarak kullanıyorsanız, [Azure Stack hub derlemesi için desteklenen Azure depolama API sürümünü](/azure-stack/user/azure-stack-acs-differences?#api-version) denetleyin ve bu sürümü kodunuzda hedefleyin. 

Örneğin, Azure Stack hub sürüm 2005 ' de çalışıyorsanız, depolama hizmeti için en yüksek sürüm 2019-02-02 ' dir. Event Hubs SDK istemci kitaplığı, varsayılan olarak Azure 'da kullanılabilen en yüksek sürümü (SDK 'nın sürümü sırasında 2019-07-07) kullanır. Bu durumda, bu bölümdeki adımların yanı sıra Storage Service API sürüm 2019-02-02 ' i hedeflemek için de kod eklemeniz gerekir. Belirli bir depolama API sürümünün nasıl hedeflenecek hakkında bir örnek için C#, Java, Python ve JavaScript/TypeScript için aşağıdaki örneklere bakın.  

Kodunuzda belirli bir depolama API sürümünün nasıl hedeflenecek hakkında bir örnek için GitHub 'da aşağıdaki örneklere bakın: 

- [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/)
- [Java](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithCustomStorageVersion.java)
- Python- [zaman uyumlu](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob/samples/receive_events_using_checkpoint_store_storage_api_version.py), [zaman uyumsuz](https://github.com/Azure/azure-sdk-for-python/blob/master/sdk/eventhub/azure-eventhub-checkpointstoreblob-aio/samples/receive_events_using_checkpoint_store_storage_api_version_async.py)
- [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript/receiveEventsWithApiSpecificStorage.js) ve [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/typescript/src/receiveEventsWithApiSpecificStorage.ts)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs genel bakış](./event-hubs-about.md)
* [Olay Hub 'ı oluşturma](event-hubs-create.md)
* [Event Hubs otomatik Şişir](event-hubs-auto-inflate.md)
