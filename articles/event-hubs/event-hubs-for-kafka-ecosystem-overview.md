---
title: Apache Kafka App 'ten Olay Hub 'ı kullanma-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Event Hubs tarafından Apache Kafka desteği hakkında bilgi sağlanır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 733623895176bb3b573c2efcbda8a40b9e2d87c0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85320537"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Apache Kafka uygulamalardan Azure Event Hubs kullanma
Event Hubs, kendi Kafka kümenizi çalıştırmaya alternatif olarak, var olan Kafka tabanlı uygulamalarınız tarafından kullanılabilecek bir Kafka uç noktası sağlar. Event Hubs, [Apache Kafka protokol 1,0 ve üstünü](https://kafka.apache.org/documentation/)destekler ve mirrormaker da dahil olmak üzere var olan Kafka uygulamalarınızla birlikte kullanılabilir.  

> [!VIDEO https://www.youtube.com/embed/UE1WgB96_fc]

## <a name="what-does-event-hubs-for-kafka-provide"></a>Kafka için Event Hubs neler sağlar?

Kafka özelliği için Event Hubs, Kafka konularındaki okuma ve yazma için Kafka sürümleri 1,0 ve üzeri ile ikili uyumlu Azure Event Hubs 'nin en üstünde bir protokol başı sağlar. Kafka uç noktasını, kod değişikliği olmadan uygulamalarınızdan kullanmaya başlayabilir, ancak en az bir yapılandırma değişikliği yapabilirsiniz. Yapılandırmalarda bağlantı dizesini, Kafka kümenize işaret etmek yerine, Olay Hub 'ınız tarafından sunulan Kafka uç noktasını işaret etmek üzere güncelleştirebilirsiniz. Daha sonra, Kafka protokolünü kullanan uygulamalarınızdan akış olaylarını Event Hubs ' a başlatabilirsiniz. Bu tümleştirme, şu anda önizleme aşamasında olan [Kafka Connect](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect)gibi çerçeveleri de destekler. 

Kavramsal Kafka ve Event Hubs neredeyse aynıdır: akış verileri için oluşturulan bölümlenmiş günlüklerdir. Aşağıdaki tabloda Kafka ve Event Hubs arasındaki kavramlar eşlenir.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka ve Event hub kavramsal eşleme

| Kafka kavram | Event Hubs kavram|
| --- | --- |
| Küme | Ad Alanı |
| Konu başlığı | Olay Hub'ı |
| Bölüm | Bölüm|
| Tüketici grubu | Tüketici grubu |
| Uzaklık | Uzaklık|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Kafka ve Event Hubs arasındaki temel farklılıklar

[Apache Kafka](https://kafka.apache.org/) , seçtiğiniz her yerde çalıştırabileceğiniz bir yazılımdır, Event Hubs Azure Blob depolama alanına benzer bir bulut hizmetidir. Yönetilecek bir sunucu veya ağ yok ve yapılandırılacak aracılar yok. Başlıklarınızın bulunduğu bir FQDN olan bir ad alanı oluşturun ve ardından bu ad alanı içinde Event Hubs veya konu başlığı oluşturun. Event Hubs ve ad alanları hakkında daha fazla bilgi için bkz. [Event Hubs özellikleri](event-hubs-features.md#namespace). Bulut hizmeti olarak Event Hubs, uç nokta olarak tek bir kararlı sanal IP adresi kullanır, bu nedenle istemcilerin bir küme içindeki aracılar veya makineler hakkında bilmeleri gerekmez. 

Event Hubs ölçek, satın aldığınız kaç üretilen iş birimi ile, her bir üretilen iş birimi için, saniyede 1 MB veya saniyede 1000 olay entitling. Varsayılan olarak, bir [Otomatik şişeden](event-hubs-auto-inflate.md) özelliği ile sınırınıza ulaştığınızda Event Hubs üretilen iş birimlerini ölçeklendirir; Bu özellik ayrıca Event Hubs for Kafka özelliği ile de kullanılabilir. 

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

#### <a name="shared-access-signature-sas"></a>Paylaşılan Erişim İmzası (SAS)
Event Hubs Ayrıca, Kafka kaynakları için Event Hubs temsilci erişimi için **paylaşılan erişim imzaları (SAS)** sağlar. OAuth 2,0 belirteç tabanlı mekanizmayı kullanarak erişimi yetkilendirmek, SAS üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Yerleşik roller Ayrıca, Kullanıcı tarafından saklanması ve yönetilmesi gereken ACL tabanlı yetkilendirme gereksinimini ortadan kaldırabilir. Bu özelliği protokol için **SASL_SSL** ve mekanizma için **düz** olarak belirterek Kafka istemcileriniz ile kullanabilirsiniz. 

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

#### <a name="samples"></a>Örnekler 
Bir olay hub 'ı oluşturmak ve SAS veya OAuth kullanarak buna erişmek için adım adım yönergeler içeren bir **öğretici** için bkz. [hızlı başlangıç: Kafka protokolünü kullanarak Event Hubs veri akışı](event-hubs-quickstart-kafka-enabled-event-hubs.md).

Kafka için Event Hubs ile OAuth 'ın nasıl kullanılacağını gösteren daha fazla **örnek** için bkz. [GitHub 'da örnekler](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth).

## <a name="other-event-hubs-features-available-for-kafka"></a>Kafka için kullanılabilen diğer Event Hubs özellikleri

Kafka özelliği için Event Hubs, bir protokolle yazmanızı ve diğer kişilerle okumanızı sağlar. böylece, geçerli Kafka üreticileri Kafka aracılığıyla yayımlamaya devam edebilir ve Azure Stream Analytics ya da Azure Işlevleri gibi Event Hubs okuyucular ekleyebilirsiniz. Ayrıca, [yakalama](event-hubs-capture-overview.md) ve [coğrafi olağanüstü durum kurtarma](event-hubs-geo-dr.md) gibi Event Hubs özellikler, ayrıca, Kafka için Event Hubs özelliği ile de çalışır.

## <a name="features-that-are-not-yet-supported"></a>Henüz desteklenmeyen özellikler 

Henüz desteklenmeyen Kafka özelliklerinin listesi aşağıda verilmiştir:

*   Idempotent üreticisi
*   İşlem
*   Sıkıştırma
*   Boyut tabanlı bekletme
*   Günlük düzenleme
*   Mevcut bir konuya bölüm ekleme
*   HTTP Kafka API desteği
*   Kafka akışlar

## <a name="next-steps"></a>Sonraki adımlar
Bu makale, Kafka için Event Hubs bir giriş sağlamıştır. Daha fazla bilgi için bkz. [Azure Event Hubs için Apache Kafka Geliştirici Kılavuzu](apache-kafka-developer-guide.md).


