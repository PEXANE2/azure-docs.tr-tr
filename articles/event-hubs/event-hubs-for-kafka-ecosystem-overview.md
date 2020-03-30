---
title: Apache Kafka uygulamasından etkinlik merkezini kullanma - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, Azure Etkinlik Hub'ları tarafından verilen Apache Kafka desteği hakkında bilgi verilmektedir.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/12/2020
ms.author: shvija
ms.openlocfilehash: 6dcbf0ad0f6678d892c5c02446cac09b4325384c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283659"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Apache Kafka uygulamalarından Azure Etkinlik Hub'larını kullanma
Olay Hub'ları, mevcut Kafka tabanlı uygulamalarınız tarafından kendi Kafka kümenizi çalıştırmaya alternatif olarak kullanılabilecek bir Kafka uç noktası sağlar. Etkinlik Hub'ları [Apache Kafka protokolü 1.0 ve sonrası](https://kafka.apache.org/documentation/)protokolünü destekler ve MirrorMaker da dahil olmak üzere mevcut Kafka uygulamalarınız ile çalışır.  

## <a name="what-does-event-hubs-for-kafka-provide"></a>Kafka için Etkinlik Hub'ları ne sağlar?

Kafka için Etkinlik Hub'ları özelliği, Kafka sürümleri 1.0 ile ikili uyumlu olan Azure Etkinlik Hub'larının üstüne bir protokol kafası sağlar ve daha sonra Kafka konularından hem okuma hem de yazma için. Kod değişikliği değil, en az yapılandırma değişikliği olmadan uygulamalarınızdan Kafka bitiş noktasını kullanmaya başlayabilirsiniz. Konfigürasyonlarda bağlantı dizesini, Kafka kümenizi işaret etmek yerine olay hub'ınıztarafından açığa çıkarılan Kafka uç noktasına işaret etmek için güncellersiniz. Daha sonra, Kafka protokolünü kullanan uygulamalarınızdan Etkinlik Hub'larına olayları akışına başlayabilirsiniz. Bu tümleştirme aynı zamanda şu anda önizlemede olan [Kafka Connect](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect)gibi çerçeveleri de destekler. 

Kavramsal olarak Kafka ve Olay Hub'ları hemen hemen aynıdır: her ikisi de veri akışı için oluşturulmuş bölümlü günlüklerdir. Aşağıdaki tablokafka ve Olay Hub'ları arasındaki kavramları eşler.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka ve Event Hub kavramsal haritalama

| Kafka Kavramı | Etkinlik Hub'ları Kavramı|
| --- | --- |
| Küme | Ad Alanı |
| Konu başlığı | Olay Hub'ı |
| Bölüm | Bölüm|
| Tüketici Grubu | Tüketici Grubu |
| Uzaklık | Uzaklık|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Kafka ve Etkinlik Merkezleri arasındaki temel farklar

[Apache Kafka](https://kafka.apache.org/) istediğiniz yerde çalıştırabileceğiniz bir yazılım olsa da, Etkinlik Hub'ları Azure Blob Depolama'ya benzer bir bulut hizmetidir. Yönetilen sunucu lar veya ağlar ve yapılandırış yapacak aracı lar yoktur. Konularınızın yaşadığı bir FQDN olan bir ad alanı oluşturursunuz ve ardından bu ad alanı içinde Olay Hub'ları veya konuları oluşturursunuz. Olay Hub'ları ve ad alanları hakkında daha fazla bilgi için [Olay Hub'ları özelliklerine](event-hubs-features.md#namespace)bakın. Bulut hizmeti olarak, Olay Hub'ları bitiş noktası olarak tek bir kararlı sanal IP adresi kullanır, bu nedenle istemcilerin küme içindeki aracılar veya makineler hakkında bilgi edinmesine gerek yoktur. 

Olay Hub'larında ölçeklendirme, satın aldığınız kaç iş birimi yle, her bir üretim birimi sizi saniyede 1 MB'a veya saniyede 1000 olaya kadar kontrol eder. Varsayılan olarak, Otomatik [Şişirme](event-hubs-auto-inflate.md) özelliği ile limitinize ulaştığınızda Olay Hub'ları iş birimi birimlerini ölçekler; Bu özellik kafka için Etkinlik Hub'ları ile de çalışır. 

### <a name="security-and-authentication"></a>Güvenlik ve kimlik doğrulaması
Kafka için bir Etkinlik Hub'ından etkinlikleri her yayımladığınızda veya tükseniz, istemciniz Olay Hub'ları kaynaklarına erişmeye çalışır. Kaynaklara yetkili bir varlık kullanılarak erişildiğından emin olmak istiyorsunuz. Müşterilerinizle Apache Kafka protokolünü kullanırken, SASL mekanizmalarını kullanarak yapılandırmanızı kimlik doğrulama ve şifreleme için ayarlayabilirsiniz. Kafka için Event Hub'ları kullanırken TLS-şifreleme gerekir (Event Hub'ları ile aktarımdaki tüm veriler TLS şifreli olduğundan). Yapılandırma dosyanızdaki SASL_SSL seçeneğini belirterek yapılabilir. 

Azure Etkinlik Hub'ları, güvenli kaynaklarınıza erişimi yetkilendirmek için birden çok seçenek sunar. 

- OAuth
- Paylaşılan erişim imzası (SAS)

#### <a name="oauth"></a>OAuth
Olay Hub'ları, **OAuth** 2.0 uyumlu merkezi leştirilmiş yetkilendirme sunucusu sağlayan Azure Active Directory (Azure AD) ile tümleşir. Azure AD ile, istemci kimliklerinize ince taneli izinler vermek için rol tabanlı erişim denetimini (RBAC) kullanabilirsiniz. Bu özelliği, protokol için **SASL_SSL** belirterek Kafka istemcilerinizve mekanizma için **OAUTHBEARER'ı** kullanarak kullanabilirsiniz. RBAC rolleri ve erişim düzeyleri hakkında daha fazla bilgi için Azure [AD ile erişimi yetkilendirme'ye](authorize-access-azure-active-directory.md)bakın.

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
```

#### <a name="shared-access-signature-sas"></a>Paylaşılan Erişim İmzası (SAS)
Olay Hub'ları, Kafka kaynakları için Etkinlik Hub'larına temsilci erişimi için **Paylaşılan Erişim İmzaları (SAS)** de sağlar. OAuth 2.0 belirteç tabanlı mekanizma yı kullanarak erişim yetkisi, SAS'a göre üstün güvenlik ve kullanım kolaylığı sağlar. Yerleşik roller, kullanıcı tarafından sürdürülmesi ve yönetilmesi gereken ACL tabanlı yetkilendirme gereksinimini de ortadan kaldırabilir. Bu özelliği, protokol için **SASL_SSL** belirterek Kafka istemcilerinizile ve mekanizma için **DÜZ** olarak kullanabilirsiniz. 

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

#### <a name="samples"></a>Örnekler 
Bir etkinlik hub'ı oluşturmak ve SAS veya OAuth kullanarak erişmek için adım adım yönergeleri içeren bir **öğretici** [için, Kafka protokolünü kullanarak Olay Hub'larıyla veri akışı na](event-hubs-quickstart-kafka-enabled-event-hubs.md)bakın.

Kafka için Olay Hub'ları ile OAuth'un nasıl kullanılacağını gösteren daha fazla **örnek** için [GitHub'daki örneklere](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth)bakın.

## <a name="other-event-hubs-features-available-for-kafka"></a>Kafka için diğer Etkinlik Hub'ları özellikleri

Kafka için Etkinlik Hub'ları özelliği, mevcut Kafka yapımcılarınızın Kafka üzerinden yayımyayınlamaya devam edebilmeleri ve Azure Akış Analizi veya Azure Fonksiyonları gibi Etkinlik Hub'ları ile okuyucular ekleyebilmeniz için bir protokolle yazmanızı ve başka bir protokolle okumanızı sağlar. Ayrıca, [Capture](event-hubs-capture-overview.md) ve [Geo Disaster-Recovery](event-hubs-geo-dr.md) gibi Event Hub'ları özellikleri Kafka özelliği için Etkinlik Hub'ları ile de çalışır.

## <a name="features-that-are-not-yet-supported"></a>Henüz desteklenmeyen özellikler 

İşte henüz desteklenmeyen Kafka özelliklerinin listesi:

*   İktidarlı yapımcı
*   İşlem
*   Sıkıştırma
*   Boyut tabanlı bekletme
*   Günlük sıkıştırma
*   Varolan bir konuya bölüm ekleme
*   HTTP Kafka API desteği
*   Kafka Akarsuları

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, Kafka için Etkinlik Hub'larına giriş sağlamıştır. Daha fazla bilgi için aşağıdaki bağlantılara bakın:

- [Olay hub'ı oluşturma](event-hubs-create.md)
- [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Bir olay hub'ında Kafka aracısı yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect'i bir etkinlik merkeziyle bütünleştirin](event-hubs-kafka-connect-tutorial.md)
- [Akka Akışlarını bir etkinlik merkezine bağlayın](event-hubs-kafka-akka-streams-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)


