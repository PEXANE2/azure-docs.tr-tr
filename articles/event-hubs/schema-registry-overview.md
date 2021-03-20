---
title: Event Hubs’da Azure Şema Kayıt Defteri (Önizleme)
description: Bu makalede, Azure Event Hubs (Önizleme) tarafından sağlanan şema kayıt defteri desteğine genel bir bakış sunulmaktadır.
ms.topic: overview
ms.date: 09/22/2020
ms.custom: references_regions
ms.openlocfilehash: a876651b76aa259754623854b8fc4a7c6c8a939e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92330504"
---
# <a name="azure-schema-registry-in-event-hubs-preview"></a>Event Hubs’da Azure Şema Kayıt Defteri (Önizleme)
Birçok olay akışı ve mesajlaşma senaryosunda, olay veya ileti yükü, Apache avro gibi şema odaklı bir biçim kullanılarak serileştirilmiş veya serisi kaldırılan yapılandırılmış verileri içerir. Gönderenlerin ve alıcıların her ikisi de JSON şeması ile bir şema belgesiyle verilerin bütünlüğünü doğrulamak isteyebilir. Şema temelli biçimler için, şemanın ileti tüketicisi tarafından kullanılabilir olması, tüketicinin verileri seri durumdan çıkarabilmesi için bir önkoşuldur. 

**Azure şema kayıt defteri** , olay odaklı ve mesajlaşma merkezli uygulamalar için şema belgeleri için merkezi bir depo sağlayan Event Hubs özelliğidir. Üretici ve tüketici uygulamalarınızın, aralarında şemayı yönetmek ve paylaşmak zorunda kalmadan verileri alışverişi için esneklik sağlar ve farklı oranlarda de gelişir. Şema kayıt defteri Ayrıca yeniden kullanılabilir şemalar için basit bir idare çerçevesi sağlar ve bir gruplama yapısı (şema grupları) aracılığıyla şemalar arasındaki ilişkiyi tanımlar.

> [!NOTE]
> - **Şema kayıt defteri** özelliği şu anda **önizlemededir** ve üretim iş yükleri için önerilmez.
> - Özelliği, **temel** katmanda değil, yalnızca **Standart** ve **adanmış** katmanlarda kullanılabilir.

Apache avro gibi şema odaklı serileştirme çerçeveleri ile, serileştirme meta verilerini paylaşılan şemalara bölmek, her veri kümesine dahil edilen tür bilgilerinin ileti başına ek yükünü önemli ölçüde azaltarak, JSON gibi etiketli biçimlerde olduğu gibi da yardımcı olabilir. Olaylar ve olay altyapısının içinde depolanan şemalara sahip olmak, serileştirme/devre dışı bırakma için gereken meta verilerin her zaman erişim ve şemaların yanlış olmamasını sağlar. 

## <a name="event-hubs-namespace"></a>Event Hubs ad alanı
Event Hubs ad alanı artık, Olay Hub 'ları (veya Kafka konuları) ile şema gruplarını barındırabilir. Bir şema kayıt defteri barındırır ve birden çok şema grubu olabilir. Azure Event Hubs 'de barındırılmakta olan artma içinde, şema kayıt defteri tüm Azure mesajlaşma hizmetleri ve diğer herhangi bir ileti veya olay Aracısı ile evrensel olarak kullanılabilir. Bu şema gruplarının her biri, bir şema kümesi için ayrı bir güvenli kılınabilir depodur. Gruplar belirli bir uygulamayla veya bir kuruluş birimiyle hizalanabilir. 

## <a name="schema-groups"></a>Şema grupları
Şema grubu, iş ölçütlerinize göre benzer şemalardan oluşan mantıksal bir gruptur. Şema grubu, bir şemanın birden çok sürümünü tutabilir. Bir şema grubundaki uyumluluk zorlaması ayarı, yeni şema sürümlerinin geriye dönük olarak uyumlu olduğundan emin olmanıza yardımcı olabilir.

Gruplama mekanizmasının uyguladığı güvenlik sınırı, ticari parolaların, ad alanının birden çok iş ortağı arasında paylaşıldığı durumlarda meta verileri yanlışlıkla sızdırmamasını sağlar. Ayrıca, uygulama sahiplerinin aynı ad alanını paylaşan diğer uygulamalardan bağımsız olarak şemaları yönetmesine olanak tanır.


## <a name="schemas"></a>Şemalar
Şemalar, üreticileri ve tüketiciler arasındaki sözleşmeyi tanımlar. Event Hubs şeması kayıt defterinde tanımlanan bir şema, sözleşmenin olay verileri dışında yönetilmesine yardımcı olur ve böylece yük yükünü kaldırır. Bir şemanın adı, türü (örnek: kayıt, dizi vb.), uyumluluk modu (None, ilet, geriye doğru, tam) ve serileştirme türü (yalnızca avro için geçerlidir) vardır. Bir şemanın birden çok sürümünü oluşturabilir ve bir şemanın belirli bir sürümünü alabilir ve kullanabilirsiniz. 

## <a name="client-sdks"></a>İstemci SDK'ları
Şema kayıt defteri şema tanımlayıcılarını ve avro kodlu verileri içeren yükleri seri hale getirmek ve seri durumdan çıkarmak için kullanabileceğiniz bir avro serileştirici içeren aşağıdaki kitaplıklardan birini kullanabilirsiniz.

- [.NET-Microsoft. Azure. Data. SchemaRegistry. ApacheAvro](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/schemaregistry/Microsoft.Azure.Data.SchemaRegistry.ApacheAvro)
- [Java-Azure-Data-schemaregistry-avro](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/schemaregistry/azure-data-schemaregistry-avro/)
- [Python-Azure-schemaregistry-avroserializer](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/schemaregistry/azure-schemaregistry-avroserializer)
- [JavaScript @azure/schema-registry-avro](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/schemaregistry/schema-registry-avro)
- [Apache Kafka](https://github.com/Azure/azure-schema-registry-for-kafka/) -Azure şema kayıt defteri tarafından desteklenen Kafka Ile tümleşik Apache avro serileştiricileri ve yinelenenleri kaldırma çalıştırın. Java istemcisinin Azure şema kayıt defteri için Apache Kafka istemci serileştirici, herhangi bir Apache Kafka senaryosunda ve herhangi bir Apache Kafka® tabanlı dağıtım veya bulut hizmeti ile kullanılabilir. 

Aşağıdaki görüntüde Event Hubs şema kayıt defterinin bilgi akışı gösterilmektedir: 

:::image type="content" source="./media/schema-registry-overview/flow-diagram.png" alt-text="Akış diyagramı":::

## <a name="standard-vs-dedicated-limits"></a>Standart ve adanmış sınırlar
Event Hubs standart ve ayrılmış katmanları için aynı ve farklı olan sınırlar için (örneğin, bir ad alanındaki şema gruplarının sayısı), bkz. [şema kayıt defteri sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md#schema-registry-limitations)

## <a name="azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi
Şema kayıt defterine programlı bir şekilde erişirken, Azure Active Directory (Azure AD) bir uygulamayı kaydetmeniz ve uygulamanın güvenlik sorumlusunu Azure rol tabanlı erişim denetimi (Azure RBAC) rollerinden birine eklemeniz gerekir:

| Rol | Açıklama | 
| ---- | ----------- | 
| Sahip | Şema kayıt defteri gruplarını ve şemalarını okuyun, yazın ve silin. |
| Katılımcı | Şema kayıt defteri gruplarını ve şemalarını okuyun, yazın ve silin. |
| [Şema kayıt defteri okuyucusu (Önizleme)](../role-based-access-control/built-in-roles.md#schema-registry-reader-preview) | Şema kayıt defteri gruplarını ve şemaları okuyun ve listeleyin. |
| [Şema kayıt defterine katkıda bulunan (Önizleme)](../role-based-access-control/built-in-roles.md#schema-registry-reader-preview) | Şema kayıt defteri gruplarını ve şemalarını okuyun, yazın ve silin. |

Azure portal kullanarak uygulama kaydetme hakkında yönergeler için bkz. [Azure AD ile uygulama kaydetme](../active-directory/develop/quickstart-register-app.md). Kodda kullanılacak olan istemci KIMLIĞI (uygulama KIMLIĞI), kiracı KIMLIĞI ve gizli anahtarı aklınızda edin. 

## <a name="next-steps"></a>Sonraki adımlar

- Azure portal kullanarak bir şema kayıt defteri oluşturmayı öğrenmek için, bkz. [Azure Portal kullanarak Event Hubs şeması kayıt defteri oluşturma](create-schema-registry.md).
- Aşağıdaki **şema kayıt defteri avro istemci kitaplığı** örneklerine bakın.
    - [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/schemaregistry/Microsoft.Azure.Data.SchemaRegistry.ApacheAvro/tests/Samples)
    - [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/schemaregistry/azure-data-schemaregistry-avro/src/samples)
    - [JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/schemaregistry/schema-registry-avro/samples )
    - [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/schemaregistry/azure-schemaregistry-avroserializer/samples )
    - [Azure şema kayıt defteri için Kafka avro tümleştirmesi](https://github.com/Azure/azure-schema-registry-for-kafka/tree/master/csharp/avro/samples)
