---
title: Kullanım olay hub'ından Apache Kafka uygulama - Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs ile Apache Kafka desteği hakkında bilgi sağlar.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/12/2020
ms.author: shvija
ms.openlocfilehash: fc81226e754178ad0edfff96a494dd7522662261
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79264901"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Apache Kafka uygulamalarından Azure Event Hubs'ı kullanın
Event Hubs, kullanılabilir bir Kafka uç noktası sağlar, varolan Kafka kendi Kafka kümesi çalıştıran alternatif olarak tabanlı uygulamalar. Event Hubs, [Apache Kafka protokol 1,0 ve üstünü](https://kafka.apache.org/documentation/)destekler ve mirrormaker da dahil olmak üzere var olan Kafka uygulamalarınızla birlikte kullanılabilir.  

## <a name="what-does-event-hubs-for-kafka-provide"></a>Event Hubs, Kafka için neler sağlar?

Olay hub'ları Kafka özelliği için bir protokol head ile Kafka sürümleri 1.0 ve daha sonra hem okuma hem de Kafka konularını yazmak için ikili uyumlu olan Azure Event Hubs üzerinde sağlar. Kafka uç noktasından uygulamalarınızı kod değişikliği ancak en az bir yapılandırma değişikliği kullanarak başlayabilir. Kafka kümenize işaret eden yerine olay hub'ınız tarafından kullanıma sunulan Kafka uç noktasını işaret edecek şekilde yapılandırmaları bağlantı dizesinde güncelle Ardından, Event Hubs'a Kafka protokolünü kullanan uygulamalarınızı olaylardan akış başlatabilirsiniz. Bu tümleştirme, şu anda önizleme aşamasında olan [Kafka Connect](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect)gibi çerçeveleri de destekler. 

Kavramsal Kafka ve Event Hubs neredeyse aynıdır: akış verileri için oluşturulan bölümlenmiş günlüklerdir. Aşağıdaki tablo, Event Hubs ile Kafka arasındaki kavramları eşler.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka ve olay hub'ı kavramsal eşleme

| Kafka kavramı | Olay hub'ları kavramını|
| --- | --- |
| Küme | Ad alanı |
| Konu başlığı | Olay Hub'ı |
| Bölüm | Bölüm|
| Tüketici Grubu | Tüketici Grubu |
| Uzaklık | Uzaklık|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Event Hubs ile Kafka arasındaki temel farklılıklar

[Apache Kafka](https://kafka.apache.org/) , seçtiğiniz her yerde çalıştırabileceğiniz bir yazılımdır, Event Hubs Azure Blob depolama alanına benzer bir bulut hizmetidir. Sunucu yok veya yönetmek için ağ ve hiçbir aracıları yapılandırmak için vardır. İçinde konuları canlı bir FQDN olduğundan, bir ad alanı oluşturur ve ardından olay hub'ları veya bu ad alanı içindeki konuları oluşturun. Event Hubs ve ad alanları hakkında daha fazla bilgi için bkz. [Event Hubs özellikleri](event-hubs-features.md#namespace). Bulut hizmeti olarak Event Hubs, uç nokta olarak tek bir kararlı sanal IP adresi kullanır, bu nedenle istemcilerin bir küme içindeki aracılar veya makineler hakkında bilmeleri gerekmez. 

Event Hubs ölçek tarafından kaç tane işleme birimi, saniyede 1 MB ile entitling her bir üretilen iş birimi veya giriş saniyede 1000 olay ile satın aldığınız denetlenir. Varsayılan olarak, bir [Otomatik şişeden](event-hubs-auto-inflate.md) özelliği ile sınırınıza ulaştığınızda Event Hubs üretilen iş birimlerini ölçeklendirir; Bu özellik ayrıca Event Hubs for Kafka özelliği ile de kullanılabilir. 

### <a name="security-and-authentication"></a>Güvenlik ve kimlik doğrulaması
Kafka için bir Event Hubs olay yayımlamanızda veya kullandığınızda, istemciniz Event Hubs kaynaklarına erişmeye çalışıyor. Kaynaklara yetkili bir varlık kullanılarak erişildiğinden emin olmak istiyorsunuz. İstemcileriniz ile Apache Kafka Protokolü kullanırken, SASL mekanizmalarını kullanarak kimlik doğrulama ve şifreleme için yapılandırmanızı ayarlayabilirsiniz. Kafka için Event Hubs kullandığınızda TLS şifrelemesi gerekir (Event Hubs ile iletim içindeki tüm veriler TLS şifreli olarak). Yapılandırma dosyanızdaki SASL_SSL seçeneği belirtilerek yapılabilir. 

Azure Event Hubs, güvenli kaynaklarınıza erişim yetkisi vermek için birden çok seçenek sağlar. 

- OAuth
- Paylaşılan erişim imzası (SAS)

#### <a name="oauth"></a>OAuth
Event Hubs, **OAuth** 2,0 uyumlu merkezi bir yetkilendirme sunucusu sağlayan Azure Active Directory (Azure AD) ile tümleşir. Azure AD ile, istemci kimliklerinize ayrıntılı izinler vermek için rol tabanlı erişim denetimi 'ni (RBAC) kullanabilirsiniz. Bu özelliği, mekanizmaya yönelik protokol ve **Oauthtaşıyıcı** için **SASL_SSL** belirterek, Kafka istemcileriniz ile kullanabilirsiniz. Erişimi kapsama için RBAC rolleri ve düzeyleri hakkında daha fazla bilgi için bkz. [Azure AD ile erişim yetkisi verme](authorize-access-azure-active-directory.md).

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
```

#### <a name="shared-access-signature-sas"></a>Paylaşılan erişim Imzası (SAS)
Event Hubs Ayrıca, Kafka kaynakları için Event Hubs temsilci erişimi için **paylaşılan erişim imzaları (SAS)** sağlar. OAuth 2,0 belirteç tabanlı mekanizmayı kullanarak erişimi yetkilendirmek, SAS üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Yerleşik roller Ayrıca, Kullanıcı tarafından saklanması ve yönetilmesi gereken ACL tabanlı yetkilendirme gereksinimini ortadan kaldırabilir. Bu özelliği protokol için **SASL_SSL** ve mekanizma için **düz** olarak belirterek Kafka istemcileriniz ile kullanabilirsiniz. 

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

#### <a name="samples"></a>Örnekler 
Kafka etkin bir olay hub 'ı oluşturmak ve SAS veya OAuth kullanarak buna erişmek için adım adım yönergeler içeren bir **öğretici** için bkz. [hızlı başlangıç: Kafka protokolünü kullanarak Event Hubs veri akışı](event-hubs-quickstart-kafka-enabled-event-hubs.md).

Kafka için Event Hubs ile OAuth 'ın nasıl kullanılacağını gösteren daha fazla **örnek** için bkz. [GitHub 'da örnekler](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth).

## <a name="other-event-hubs-features-available-for-kafka"></a>Kafka için diğer Event Hubs özellikleri

Kafka özelliği için Event Hubs ile bir protokol okunup yazılacağını başka ile geçerli Kafka üreticiler, Kafka aracılığıyla yayımlama devam edebilirsiniz ve Event Hubs, Azure Stream Analytics veya Azure işlevleri gibi okuyucularıyla ekleyebilirsiniz. böylece sağlar. Ayrıca, [yakalama](event-hubs-capture-overview.md) ve [coğrafi olağanüstü durum kurtarma](event-hubs-geo-dr.md) gibi Event Hubs özellikler, ayrıca, Kafka için Event Hubs özelliği ile de çalışır.

## <a name="features-that-are-not-yet-supported"></a>Henüz desteklenmeyen özellikler 

Henüz desteklenmeyen Kafka özelliklerin listesi aşağıda verilmiştir:

*   Idempotent üretici
*   İşlem
*   Sıkıştırma
*   Boyutu bağlı bekletme
*   Günlük sıkıştırma
*   Varolan bir konuya bölümleri ekleme
*   HTTP Kafka API'si desteği
*   Kafka akışlar

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Kafka için Event hubs'a giriş sağlandı. Daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Kafka özellikli Event Hubs oluşturma](event-hubs-create-kafka-enabled.md)
- [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Kafka özellikli bir olay hub'ında Kafka aracısını yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı Kafka özellikli bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i Kafka özellikli bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect'i Kafka özellikli olay hub'ıyla tümleştirme](event-hubs-kafka-connect-tutorial.md)
- [Akka Streams’i Kafka özellikli olay hub'ına bağlama](event-hubs-kafka-akka-streams-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)


