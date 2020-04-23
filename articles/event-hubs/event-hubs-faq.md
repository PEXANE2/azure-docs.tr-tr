---
title: Sık sorulan sorular - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, Azure Etkinlik Hub'ları ve yanıtları için sık sorulan soruların (SSS) bir listesi yer almaktadır.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/02/2019
ms.author: shvija
ms.openlocfilehash: 7f6e1896c97c96cd484d15fb9e6a3056e5c5d6b2
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086377"
---
# <a name="event-hubs-frequently-asked-questions"></a>Olay Hub'ları sık sorulan sorular

## <a name="general"></a>Genel

### <a name="what-is-an-event-hubs-namespace"></a>Olay Hub'ları ad alanı nedir?
Ad alanı, Event Hub/Kafka Topics için bir kapsam kapsayıcısidir. Size benzersiz bir [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)verir. Ad alanı, birden çok Event Hub/Kafka Konularını barındırabilecek bir uygulama kapsayıcısı olarak hizmet vermektedir. 

### <a name="when-do-i-create-a-new-namespace-vs-use-an-existing-namespace"></a>Varolan bir ad alanı nın kullanımına karşı yeni bir ad alanı ne zaman oluştururum?
Kapasite ayırmaları[(iş birimi (TUs)](#throughput-units)ad alanı düzeyinde faturalandırılır. Ad alanı da bir bölgeyle ilişkilidir.

Aşağıdaki senaryolardan birinde varolan bir ad alanı kullanmak yerine yeni bir ad alanı oluşturmak isteyebilirsiniz: 

- Yeni bir bölgeyle ilişkili bir Olay Hub'ına ihtiyacınız var.
- Farklı bir abonelikle ilişkili bir Etkinlik Hub'ına ihtiyacınız var.
- Farklı kapasite ayırmasına sahip bir Olay Hub'ına ihtiyacınız vardır (diğer bir süre, eklenen olay göbeğine sahip ad alanı için kapasite gereksinimi 40 TU eşiğini aşar ve ilgili küme için gitmek istemezsiniz)  

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Olay Hub'ları Temel ve Standart katmanları arasındaki fark nedir?

Azure Etkinlik Hub'larının Standart katmanı, Temel katmanda mevcut olanın ötesinde özellikler sağlar. Aşağıdaki özellikler Standart ile birlikte verilir:

* Daha uzun olay bekletme
* Dahil edilen numaradan daha fazla bir fazla ücret ile ek aracılı bağlantılar,
* Tek bir [tüketici grubundan](event-hubs-features.md#consumer-groups) fazlası
* [Capture](event-hubs-capture-overview.md)
* [Kafka entegrasyonu](event-hubs-for-kafka-ecosystem-overview.md)

Etkinlik Hub'ları Özel dahil fiyatlandırma katmanları hakkında daha fazla bilgi için [Olay Hub'ları fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/event-hubs/)bakın.

### <a name="where-is-azure-event-hubs-available"></a>Azure Etkinlik Hub'ları nerede kullanılabilir?

Azure Etkinlik Hub'ları desteklenen tüm Azure bölgelerinde kullanılabilir. Liste için Azure [bölgeleri](https://azure.microsoft.com/regions/) sayfasını ziyaret edin.  

### <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Birden çok etkinlik merkezinden gönderip almak için tek bir AMQP bağlantısı kullanabilir miyim?

Evet, tüm olay hub'ları aynı ad alanında olduğu sürece.

### <a name="what-is-the-maximum-retention-period-for-events"></a>Olaylar için maksimum bekletme süresi nedir?

Olay Hub'ları Standart katmanı şu anda en fazla yedi günlük bekletme süresini destekler. Olay hub'ları kalıcı veri deposu olarak tasarlanmamıştır. 24 saatten büyük bekletme süreleri, bir olay akışını aynı sistemlerde yeniden oynatmanın uygun olduğu senaryolar için tasarlanmıştır; örneğin, varolan veriler üzerinde yeni bir makine öğrenimi modelini eğitmek veya doğrulamak için. Yedi günden fazla ileti saklamanız gerekiyorsa, etkinlik merkezinizdeki [Event Hub'larının Yakalanmasını](event-hubs-capture-overview.md) etkinleştirmek, etkinlik merkezinizdeki verileri seçtiğiniz Depolama hesabına veya Azure Veri Gölü Hizmeti hesabına çeker. Capture'i etkinleştirmek, satın aldığınız iş birimi birimlerine göre bir ücrete neden olabilir.

Depolama hesabınızda yakalanan verilerin bekletme süresini yapılandırabilirsiniz. Azure Depolama'nın **yaşam döngüsü yönetimi** özelliği, genel amaçlı v2 ve blob depolama hesapları için zengin, kural tabanlı bir ilke sunar. Verilerinizi uygun erişim katmanlarına dönüştürmek veya verilerin yaşam döngüsünün sonunda süresi dolmak için ilkeyi kullanın. Daha fazla bilgi için Azure [Blob depolama yaşam döngüsünü yönet'e](../storage/blobs/storage-lifecycle-management-concepts.md)bakın. 

### <a name="how-do-i-monitor-my-event-hubs"></a>Etkinlik Hub'larımı nasıl izleyebilirim?
Olay Hub'ları, kaynaklarınızın durumunu [Azure Monitor'a](../azure-monitor/overview.md)sağlayan ayrıntılı ölçümler yayır. Ayrıca, Olay Hub'ları hizmetinin genel durumunu yalnızca ad alanı düzeyinde değil, varlık düzeyinde de değerlendirmenize de izin verirler. [Azure Etkinlik Hub'ları](event-hubs-metrics-azure-monitor.md)için hangi izlemenin sunulduğu hakkında bilgi edinin.

### <a name="what-ports-do-i-need-to-open-on-the-firewall"></a>Güvenlik duvarında açmak için hangi bağlantı noktalarına ihtiyacım var? 
İleti göndermek ve almak için Azure Hizmet Veri Servisi ile aşağıdaki protokolleri kullanabilirsiniz:

- Gelişmiş İleti Sıraya Alma Protokolü (AMQP)
- HTTP
- Apache Kafka

Azure Etkinlik Hub'ları ile iletişim kurmak için bu protokolleri kullanmak için açmanız gereken giden bağlantı noktaları için aşağıdaki tabloya bakın. 

| Protokol | Bağlantı noktaları | Ayrıntılar | 
| -------- | ----- | ------- | 
| AMQP | 5671 ve 5672 | [Bkz. AMQP protokol kılavuzu](../service-bus-messaging/service-bus-amqp-protocol-guide.md) | 
| HTTP, HTTPS | 80, 443 |  |
| Kafka | 9093 | Kafka [uygulamalarından Etkinlik Hub'larını kullan](event-hubs-for-kafka-ecosystem-overview.md)

### <a name="what-ip-addresses-do-i-need-to-whitelist"></a>Beyaz listelemek için hangi IP adreslerine ihtiyacım var?
Bağlantılarınız için beyaz listeye doğru IP adreslerini bulmak için aşağıdaki adımları izleyin:

1. Komut isteminden aşağıdaki komutu çalıştırın: 

    ```
    nslookup <YourNamespaceName>.servicebus.windows.net
    ```
2. Döndürülen IP adresini `Non-authoritative answer`not edin. Yalnızca zaman, ad alanını farklı bir kümeye geri yüklemenizdir.

Ad alanınız için bölge artıklığını kullanıyorsanız, birkaç ek adım yapmanız gerekir: 

1. İlk olarak, ad alanında nslookup çalıştırın.

    ```
    nslookup <yournamespace>.servicebus.windows.net
    ```
2. Aşağıdaki biçimlerden birinde bulunan **yetkili olmayan yanıt** bölümündeki adı not edin: 

    ```
    <name>-s1.servicebus.windows.net
    <name>-s2.servicebus.windows.net
    <name>-s3.servicebus.windows.net
    ```
3. S1, s2 ve s3 ekleri olan her biri için nslookup çalıştırarak üç kullanılabilirlik bölgesinde çalışan her üç örneğin IP adreslerini alın, 

## <a name="apache-kafka-integration"></a>Apaçi Kafka entegrasyonu

### <a name="how-do-i-integrate-my-existing-kafka-application-with-event-hubs"></a>Mevcut Kafka uygulamamı Etkinlik Hub'larıyla nasıl bütünleştiririm?
Etkinlik Hub'ları, mevcut Apache Kafka tabanlı uygulamalarınız tarafından kullanılabilecek bir Kafka uç noktası sağlar. PaaS Kafka deneyimine sahip olmak için gereken tek şey yapılandırma değişikliğidir. Kendi Kafka kümenizi çalıştırmak için bir alternatif sağlar. Event Hub'lar Apache Kafka 1.0 ve daha yeni istemci sürümlerini destekler ve mevcut Kafka uygulamalarınız, araçlarınız ve çerçevelerinizle çalışır. Daha fazla bilgi [için Kafka repo için Etkinlik Hub'larına](https://github.com/Azure/azure-event-hubs-for-kafka)bakın.

### <a name="what-configuration-changes-need-to-be-done-for-my-existing-application-to-talk-to-event-hubs"></a>Etkinlik Hub'ları ile konuşmak için varolan uygulamamın ne gibi yapılandırma değişiklikleri yapılması gerekiyor?
Bir etkinlik hub'ına bağlanmak için Kafka istemci configs'ini güncelleştirmeniz gerekir. Olay Hub'ları ad alanı oluşturarak ve [bağlantı dizesini](event-hubs-get-connection-string.md)elde ederek yapılır. Olay Hub'ları FQDN ve bağlantı noktasını 9093'e çekmek için bootstrap.servers'ı değiştirin. Kafka istemcisini Olay Hub'larınız bitiş noktasına (elde ettiğiniz bağlantı dizesidir) yönlendirmek için sasl.jaas.config'i aşağıda gösterildiği gibi doğru kimlik doğrulamasıyla güncelleyin:

bootstrap.servers={SN. EVENTHUBS. FQDN}:9093 request.timeout.ms=60000 security.protocol=SASL_SSL sasl.mechanism=PLAIN sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule gerekli kullanıcı adı="$ConnectionString" password="{YOUR. EVENTHUBS. Bağlantı. STRING}";

Örnek:

bootstrap.servers=dummynamespace.servicebus.windows.net:9093 request.timeout.ms=60000 security.protocol=SASL_SSL sasl.mechanism=PLAIN sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="Endpoint=sb://dummynamespace.servicebus.windows.net; PaylaşılanAccessKeyName=DummyAccessKeyName; SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=";

Not: Sasl.jaas.config çerçevenizde desteklenen bir yapılandırma değilse, SASL kullanıcı adını ve parolasını ayarlamak için kullanılan yapılandırmaları bulun ve bunun yerine bunları kullanın. Kullanıcı adını $ConnectionString ve Olay Hub'ları bağlantı dizenizin parolasını ayarla.

### <a name="what-is-the-messageevent-size-for-event-hubs"></a>Olay Hub'ları için ileti/olay boyutu nedir?
Olay Hub'ları için izin verilen maksimum ileti boyutu 1 MB'dır.

## <a name="throughput-units"></a>İşleme birimleri

### <a name="what-are-event-hubs-throughput-units"></a>Olay Hub'ları iş birimi birimleri nelerdir?
Olay Hub'larında iş bölümü, mega bayttaki veri miktarını veya Olay Hub'ları aracılığıyla giriş yapan ve çıkan 1-KB olaylarının sayısını (binlerce) tanımlar. Bu iş, elde etme birimleri (TUs) cinsinden ölçülür. Olay Hub'ları hizmetini kullanmaya başlamadan önce TUs satın alın. Portal veya Olay Hub'ları Kaynak Yöneticisi şablonlarını kullanarak Olay Hub'ları TUs'larını açıkça seçebilirsiniz. 


### <a name="do-throughput-units-apply-to-all-event-hubs-in-a-namespace"></a>İş yeri birimleri ad alanındaki tüm olay hub'larına geçerli mi?
Evet, iş yapma birimleri (TUs) bir Olay Hub'ları ad alanındaki tüm olay hub'larına uygulanır. Bu, ad alanı düzeyinde TUs satın aldığınız ve bu ad alanı altında etkinlik hub'ları arasında paylaşılabildiğiniz anlamına gelir. Her TU, ad alanını aşağıdaki yeteneklere hak edin:

- Giriş olaylarının saniyede 1 MB'a kadar (olay hub'ına gönderilen olaylar), ancak saniyede en fazla 1000 giriş olayı, yönetim işlemleri veya denetim API çağrıları.
- Saniyede 2 MB'a kadar çıkış olayları (bir olay merkezinden tüketilen olaylar), ancak en fazla 4096 çıkış olayı.
- 84 GB'a kadar olay depolama alanı (varsayılan 24 saatlik saklama süresi için yeterlidir).

### <a name="how-are-throughput-units-billed"></a>Elde iş birimleri nasıl faturalandırılır?
Elde etme birimleri (TUs) saatlik olarak faturalandırılır. Faturalandırma, belirli bir saat içinde seçilen maksimum birim sayısına bağlıdır. 

### <a name="how-can-i-optimize-the-usage-on-my-throughput-units"></a>Veri birimimdeki kullanımı nasıl optimize edebilirim?
Bir iş birimi (TU) kadar düşük başlayıp [otomatik şişirme'yi açabilirsiniz.](event-hubs-auto-inflate.md) Otomatik şişirme özelliği, trafiğiniz/yükünüz arttıkça TUs'larınızı büyütmenizi sağlar. Ayrıca, TUs sayısına bir üst sınır belirleyebilirsiniz.

### <a name="how-does-auto-inflate-feature-of-event-hubs-work"></a>Olay Hub'larının Otomatik şişirme özelliği nasıl çalışır?
Otomatik şişirme özelliği, iş birimilerinizi (TUs) ölçeklendirmenize olanak tanır. Bu, girişiniz arttıkça düşük TUs satın alarak başlayıp otomatik şişirme ölçeklendirmeniz anlamına gelir. Size uygun maliyetli bir seçenek ve yönetmek için TUs sayısı tam kontrol sağlar. Bu özellik **yalnızca ölçeklendirme** özelliğidir ve tus sayısının büyütülmesini güncelleyerek tamamen denetleyebilirsiniz. 

Düşük iş birimi (TUs) ile başlamak isteyebilirsiniz, örneğin, 2 TUs. Trafiğinizin 15 TUs'a kadar büyüyebileceğini tahmin ederseniz, ad alanınızdaki otomatik şişirme özelliğini açın ve maksimum sınırı 15 TUs olarak ayarlayın. Artık trafiğiniz büyüdükçe tus'larınızı otomatik olarak büyütebilirsiniz.

### <a name="is-there-a-cost-associated-when-i-turn-on-the-auto-inflate-feature"></a>Otomatik şişirme özelliğini açtığınızda ilişkili bir maliyet var mı?
Bu özellik ile ilişkili **hiçbir maliyet yoktur.** 

### <a name="how-are-throughput-limits-enforced"></a>Elde iş sınırları nasıl uygulanır?
Bir ad alanındaki tüm olay hub'ları arasında toplam giriş iş akışı veya toplam giriş olay hızı toplam iş birimi ödeneklerini aşarsa, gönderenler daraltılır ve giriş kotasının aşıldığını gösteren hatalar alır.

Bir ad alanındaki tüm olay hub'ları arasında toplam çıkış iş bölümü veya toplam olay çıkış oranı toplam iş birimi ödeneklerini aşarsa, alıcılar daraltılır ve çıkış kotasının aşıldığını gösteren hatalar alır. Giriş ve çıkış kotaları ayrı olarak uygulanır, böylece hiçbir gönderen olay tüketiminin yavaşlamasına neden olamaz veya bir alıcı olayların bir olay hub'ına gönderilmesini engelleyemez.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-tus-that-can-be-reservedselected"></a>Ayrılan/seçilebilen iş birimi (TUs) sayısında bir sınır var mı?
Çok kiracılı bir teklifte, iş birimi 40 TUs'a kadar büyüyebilir (portalda en fazla 20 TUs seçebilir ve aynı ad alanında 40 TUs'a yükseltmek için destek bileti yükseltebilirsiniz). 40 TUs'un ötesinde, Event **Hub'ları, Olay Hub'larına Özel kümeler**adı verilen kaynak/kapasite tabanlı modeli sunar. Özel kümeler Kapasite Birimleri 'nde (CUs) satılır.

## <a name="dedicated-clusters"></a>Özel kümeler

### <a name="what-are-event-hubs-dedicated-clusters"></a>Event Hubs Ayrılmış kümeleri nedir?
Olay Hub'ları Özel kümeler, en zorlu gereksinimlere sahip müşteriler için tek kiracılı dağıtımlar sunar. Bu teklif, iş birimi birimleriyle bağlı olmayan kapasite tabanlı bir küme oluşturur. Bu, kümenin CPU ve bellek kullanımı tarafından dikte edilen verilerinizi yutmak ve akışı için kümeyi kullanabileceğiniz anlamına gelir. Daha fazla bilgi için olay [hub'larına özel kümeler'](event-hubs-dedicated-overview.md)e bakın.

### <a name="how-much-does-a-single-capacity-unit-let-me-achieve"></a>Tek bir kapasite birimiyle hangi değerlere ulaşabilirim?
Özel bir küme için, ne kadar yutup akış yapabileceğiniz, üreticileriniz, tüketicileriniz, sindirip işleme oranınız ve çok daha fazlası gibi çeşitli faktörlere bağlıdır. 

Aşağıdaki tablo, testlerimiz sırasında elde ettiğimiz kıyaslama sonuçlarını gösterir:

| Yük şekli | Alıcı | Giriş bant genişliği| İletileri girin | Çıkış bant genişliği | Çıkış mesajları | Toplam TUs | CU başına TUs |
| ------------- | --------- | ---------------- | ------------------ | ----------------- | ------------------- | --------- | ---------- |
| 100x1KB toplu iş | 2 | 400 MB/sn | 400k ileti/sn | 800 MB/sn | 800k ileti/sn | 400 TUs | 100 tüs | 
| 10x10KB toplu iş | 2 | 666 MB/sn | 66.6k ileti/sn | 1,33 GB/sn | 133k ileti/sn | 666 TUs | 166 TUs |
| 6x32KB toplu iş | 1 | 1,05 GB/sn | 34k mesajlar / sn | 1,05 GB/sn | 34k ileti/sn | 1000 TUs | 250 TÜs |

Testte aşağıdaki kriterler kullanılmıştır:

- Dört kapasite birimi (CUs) içeren özel bir Olay Hub'ları kümesi kullanıldı. 
- Yutma için kullanılan olay hub'ı 200 bölümden osahipti. 
- Yutulan veriler, tüm bölümlerden alan iki alıcı uygulaması tarafından alındı.

Sonuçlar, özel bir Olay Hub'ları kümesiyle neler inebileceği hakkında bir fikir verir. Buna ek olarak, bir adanmış küme, mikro toplu ve uzun vadeli bekletme senaryolarınız için etkin leştirilmiş Olay Hub'ları Yakalama ile birlikte gelir.

### <a name="how-do-i-create-an-event-hubs-dedicated-cluster"></a>Olay Hub'larına Adanmış kümeyi nasıl oluştururum?
[Kota artış destek isteği](https://portal.azure.com/#create/Microsoft.Support) göndererek veya Olay [Hub'ları ekibiyle](mailto:askeventhubs@microsoft.com)iletişimkurarak Etkinlik Hub'larına adanmış bir küme oluşturursunuz. Kümenin dağıtılması ve sizin yeriniz tarafından kullanılmak üzere teslim edilmesi genellikle yaklaşık iki hafta sürer. Bu işlem, Azure portalı üzerinden tam bir self servis kullanıma sunulana kadar geçicidir.

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="how-many-partitions-do-i-need"></a>Kaç bölüme ihtiyacım var?
Bölüm sayısı, oluşturma sırasında belirtilir ve 2 ile 32 arasında olmalıdır. Bölüm sayısı değiştirilemez, bu nedenle bölüm sayısını ayarlarken uzun vadeli ölçek düşünmelisiniz. Bölümler, tüketen uygulamalarda gerekli aşağı akış paralelliğiyle ilişkili bir veri düzenleme mekanizmasıdır. Bir olay hub'ındaki bölüm sayısı, sahip olmayı beklediğiniz eşzamanlı okuyucu sayısıyla doğrudan ilgilidir. Bölümler hakkında daha fazla bilgi için [bkz.](event-hubs-features.md#partitions)

Oluşturma sırasında mümkün olan en yüksek değer olan 32 olarak ayarlamak isteyebilirsiniz. Birden fazla bölüme sahip olmanın, gönderenleri yalnızca kalan 31 bölümden gereksiz bırakarak 32 bölümden tek bir bölüme gönderecek şekilde yapılandırmadığınız sürece, sırayı tutmadan birden fazla bölüme gönderilen olaylara neden olacağını unutmayın. Eski durumda, tüm 32 bölüm boyunca olayları okumak gerekir. İkinci durumda, Olay İşlemci Ana Bilgisayar'da yapmak zorunda olduğunuz ekstra yapılandırma dışında belirgin bir ek maliyet yoktur.

Olay Hub'ları, tüketici grubu başına tek bir bölüm okuyucuya izin verecek şekilde tasarlanmıştır. Çoğu kullanım durumunda, dört bölümün varsayılan ayarı yeterlidir. Olay işlemenizi ölçeklendirmek istiyorsanız, ek bölümler eklemeyi düşünebilirsiniz. Bir bölüm üzerinde belirli bir iş elde sınırı yoktur, ancak ad alanınızdaki toplam iş bölümü, iş birimi sayısıyla sınırlıdır. Ad alanınızdaki iş akışı birimlerinin sayısını artırdıkça, eşzamanlı okuyucuların kendi maksimum iş lerini elde etmelerine olanak sağlayacak ek bölümler isteyebilirsiniz.

Ancak, uygulamanızın belirli bir bölüme yakınlığı olan bir modeliniz varsa, bölüm sayısını artırmak sizin için herhangi bir fayda sağlamayabilir. Daha fazla bilgi için [kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)hakkında bilgi alabiliyorum.

## <a name="pricing"></a>Fiyatlandırma

### <a name="where-can-i-find-more-pricing-information"></a>Daha fazla fiyatlandırma bilginini nereden bulabilirim?

Olay Hub'ları fiyatlandırması hakkında tam bilgi için [Olay Hub'ları fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/event-hubs/)bakın.

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Olay Hub'ları etkinliklerini 24 saatten fazla tutmak için ücret alıyor mu?

Olay Hub'ları Standart katmanı, ileti bekletme sürelerinin en fazla yedi gün süreyle 24 saatten uzun sürmesine izin verir. Depolanan toplam olay sayısının boyutu, seçili iş birimi sayısı (iş birimi başına 84 GB) için depolama ödeneğini aşıyorsa, ödeneği aşan boyut, yayınlanan Azure Blob depolama fiyatıüzerinden ücretlendirilir. Her bir iş birimindeki depolama payı, elde etme birimi maksimum giriş payına kadar kullanılsa bile, 24 saatlik saklama süreleri (varsayılan) için tüm depolama maliyetlerini kapsar.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Olay Hub'ları depolama boyutu nasıl hesaplanır ve ücretlendirilir?

Olay üstbilgileri için veya tüm olay hub'larında disk depolama yapıları için herhangi bir iç ekyükü de dahil olmak üzere tüm depolanan olayların toplam boyutu gün boyunca ölçülür. Günün sonunda, en yüksek depolama boyutu hesaplanır. Günlük depolama ödeneği, gün içinde seçilen minimum iş birimi sayısına göre hesaplanır (her bir iş birimi 84 GB'lık bir ödenek sağlar). Toplam boyut hesaplanan günlük depolama sınırını aşıyorsa, fazla depolama alanı Azure Blob depolama hızları **(Yerel Olarak Yedekli Depolama** fiyatı üzerinden) kullanılarak faturalandırılır.

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Olay Hub'ları giriş olayları nasıl hesaplanır?

Olay hub'ına gönderilen her olay faturalandırılabilir ileti olarak sayılır. Giriş *olayı,* 64 KB'den daha az veya eşit olan bir veri birimi olarak tanımlanır. Boyutu 64 KB'den küçük veya eşit olan herhangi bir olay faturalandırılabilir bir olay olarak kabul edilir. Olay 64 KB'den büyükse, faturalandırılabilir olay sayısı olay boyutuna göre 64 KB'nin katları olarak hesaplanır. Örneğin, olay hub'ına gönderilen bir 8-KB olayı tek bir olay olarak faturalandırılır, ancak olay merkezine gönderilen 96 KB iletisi iki olay olarak faturalandırılır.

Bir olay merkezinden tüketilen olaylar, yönetim işlemleri ve denetim noktaları gibi denetim çağrıları faturalandırılabilir giriş olayları olarak sayılmaz, ancak iş birimi ödeneğine kadar tahakkuk eder.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Aracılı bağlantı ücretleri Olay Hub'ları için geçerli midir?

Bağlantı ücretleri yalnızca AMQP protokolü kullanıldığında geçerlidir. Gönderen sistem veya aygıt sayısına bakılmaksızın, HTTP kullanarak olay göndermek için bağlantı ücreti yoktur. AMQP kullanmayı planlıyorsanız (örneğin, daha verimli olay akışı elde etmek veya IoT komut ve denetim senaryolarında çift yönlü iletişimi etkinleştirmek için), her hizmet katmanında kaç bağlantının bulunduğuyla ilgili ayrıntılar için [Olay Hub'ları fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/event-hubs/) sayfasına bakın.

### <a name="how-is-event-hubs-capture-billed"></a>Olay Hub'ları Yakalama nasıl faturalandırılır?

Ad alanındaki herhangi bir olay hub'ı Yakalama seçeneği etkinleştirildiğinde yakalama etkinleştirilir. Olay Hub'ları Yakalama satın alınan iş birimi başına saatlik faturalandırılır. İş birimi sayısı arttıkça veya azaltıldıkça, Olay Hub'ları Yakalama faturalandırması bu değişiklikleri tüm saat artışlarında yansıtır. Olay Hub'ları Yakalama faturalandırması hakkında daha fazla bilgi için [Olay Hub'ları fiyatlandırma bilgilerine](https://azure.microsoft.com/pricing/details/event-hubs/)bakın.

### <a name="do-i-get-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>Olay Hub'ları Yakalama için seçtiğim depolama hesabı için faturalandırılır mıyım?

Capture, bir etkinlik hub'ında etkinleştirildiğinde sağladığınız bir depolama hesabı kullanır. Depolama hesabınız olduğundan, bu yapılandırmaya yönelik tüm değişiklikler Azure aboneliğinize faturalandırılır.

## <a name="quotas"></a>Kotalar

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Olay Hub'ları ile ilişkili herhangi bir kota var mı?

Tüm Olay Hub'ları kotalarının listesi için [kotalara](event-hubs-quotas.md)bakın.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>Neden başka bir abonelikten silen bir ad alanı oluşturamıyorum? 
Bir ad alanını abonelikten sildiğinizde, başka bir abonelikte aynı adla yeniden oluşturmadan önce 4 saat bekleyin. Aksi takdirde, aşağıdaki hata iletisi alabilirsiniz: `Namespace already exists`. 

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Olay Hub'ları ve önerilen eylemleri tarafından oluşturulan özel durumlar nelerdir?

Olası Olay Hub'ları özel durumları listesi için özel [durumlara genel bakış](event-hubs-messaging-exceptions.md)bölümüne bakın.

### <a name="diagnostic-logs"></a>Tanılama günlükleri

Olay Hub'ları, her ikisi de json'da temsil edilen ve Azure portalı üzerinden açılabilir olan iki tür [tanılama günlüklerini](event-hubs-diagnostic-logs.md) destekler : Hata günlüklerini ve işlem günlüklerini yakalama.

### <a name="support-and-sla"></a>Destek ve SLA

Etkinlik Hub'ları için teknik destek [topluluk forumları](https://social.msdn.microsoft.com/forums/azure/home?forum=servbus)aracılığıyla kullanılabilir. Faturalandırma ve abonelik yönetimi desteği ücretsiz olarak sağlanır.

SLA hakkında daha fazla bilgi edinmek için [Hizmet Düzeyi Anlaşmaları](https://azure.microsoft.com/support/legal/sla/) sayfasına bakın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay Hub'larına genel bakış](event-hubs-what-is-event-hubs.md)
* [Etkinlik Merkezi Oluşturma](event-hubs-create.md)
* [Olay Hub'ları Otomatik Şişirme](event-hubs-auto-inflate.md)
