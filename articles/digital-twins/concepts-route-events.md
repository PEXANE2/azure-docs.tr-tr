---
title: Olay yolları
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS içindeki olayları ve diğer Azure hizmetlerini nasıl yönlendirildiğini öğrenin.
author: baanders
ms.author: baanders
ms.date: 10/12/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 9c7b08b92fad07cddbdb2783f2d68cdb9be034a4
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93097082"
---
# <a name="route-events-within-and-outside-of-azure-digital-twins"></a>Azure dijital TWINS 'in içindeki ve dışındaki olayları yönlendirme

Azure dijital TWINS, hizmet dışındaki tüketicilere veri göndermek için **olay yollarını** kullanır. 

Azure dijital TWINS verileri göndermek için iki önemli durum vardır:
* Azure dijital TWINS grafiğindeki bir ikizi 'den diğerine veri gönderme. Örneğin, bir dijital ikizi üzerinde bir özellik değiştiğinde, başka bir dijital ikizi 'e göre bildirim almak ve güncelleştirmek isteyebilirsiniz.
* Ek depolama veya işleme ( *veri* çıkışı olarak da bilinir) için aşağı akış veri hizmetlerine veri gönderme. Örneğin,
  - Bir barındırma, toplu analizler için el ile ilgili olayların zaman serisi verilerini kaydetmek üzere Azure dijital TWINS olay verilerini [Time Series Insights (TSI)](../time-series-insights/overview-what-is-tsi.md)' A göndermek isteyebilir.
  - Zaten [Azure Maps](../azure-maps/about-azure-maps.md) kullanan bir işletme, çözümlerini geliştirmek Için Azure dijital TWINS 'i kullanmak isteyebilir. Azure dijital TWINS 'i ayarladıktan sonra Azure haritasını hızlı bir şekilde etkinleştirebilir, Azure Map varlıklarını ikizi grafiğinde [dijital TWINS](concepts-twins-graph.md) olarak Azure dijital TWINS 'e taşıyın veya Azure haritalarını ve Azure dijital TWINS verilerini birlikte kullanarak güçlü sorgular çalıştırın.

Olay yolları bu senaryoların her ikisi için kullanılır.

## <a name="about-event-routes"></a>Olay rotaları hakkında

Bir olay yolu, Azure dijital TWINS 'deki dijital TWINS 'den, aboneliklerinizdeki özel tanımlı uç noktalara olay verileri göndermenizi sağlar. Şu anda uç noktalar için üç Azure hizmeti desteklenmektedir: [Olay Hub](../event-hubs/event-hubs-about.md)'ı, [Event Grid](../event-grid/overview.md)ve [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md). Bu Azure hizmetlerinin her biri, diğer hizmetlere bağlanabilir ve Middleman olarak görev yapabilir ve ihtiyacınız olan herhangi bir işleme için örneğin, TSI veya Azure haritaları gibi son hedeflere veri gönderiyor.

Aşağıdaki diyagramda, Azure dijital TWINS ile daha büyük bir IoT çözümü aracılığıyla olay verilerinin akışı gösterilmektedir:

:::image type="content" source="media/concepts-route-events/routing-workflow.png" alt-text="Azure dijital TWINS verileri uç noktalar aracılığıyla birçok aşağı akış hizmetine yönlendirme" border="false":::

Olay yolları için tipik aşağı akış hedefleri, TSI, Azure Maps, Storage ve Analytics çözümleri gibi kaynaklardır.

### <a name="event-routes-for-internal-digital-twin-events"></a>İç dijital ikizi olayları için olay yolları

Olay yolları, ikizi Graph içindeki olayları işlemek ve dijital ikizi 'den dijital ikizi 'e veri göndermek için de kullanılır. Bu, [Azure işlevleri](../azure-functions/functions-overview.md)gibi işlem kaynaklarına Event Grid aracılığıyla olay yolları bağlanarak yapılır. Bu işlevler daha sonra TWINS 'in olayları alıp nasıl yanıtlaması tanımlar. 

Bir işlem kaynağı, olay rotası aracılığıyla aldığı bir olaya göre ikizi grafiğini değiştirmek istediğinde, onun zaman içinde hangi ikizi üzerinde değişiklik yapılmasını istediğini bilmesini sağlamak yararlı olur. 

Alternatif olarak, olay iletisi iletiyi gönderen kaynak ikizi KIMLIĞINI de içerir, bu nedenle işlem kaynağı istenen işlem için bir hedef ikizi bulmak üzere sorguları veya çapraz geçiş ilişkilerini kullanabilir. 

İşlem kaynağının ayrıca güvenlik ve erişim izinlerini bağımsız olarak kurması gerekir.

Dijital ikizi olaylarını işlemek için bir Azure işlevi ayarlama sürecinde gezinmek için bkz. [*nasıl yapılır: verileri işlemek için bir Azure Işlevi ayarlama*](how-to-create-azure-function.md).

## <a name="create-an-endpoint"></a>Uç nokta oluşturma

Bir olay yolu tanımlamak için, geliştiriciler önce uç noktaları tanımlamalıdır. **Uç nokta** , Azure dijital TWINS 'in dışında bir yol bağlantısını destekleyen bir hedefdir. Desteklenen hedefler şunlardır:
* Event Grid özel konular
* Olay Hub'ı
* Service Bus

Bir uç nokta oluşturmak için Azure dijital TWINS [**Denetim düzlemi API 'leri**](how-to-manage-routes-apis-cli.md#create-an-endpoint-for-azure-digital-twins), [**clı komutları**](how-to-manage-routes-apis-cli.md#manage-endpoints-and-routes-with-cli)veya [**Azure Portal**](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins)kullanabilirsiniz. 

Bir uç nokta tanımlarken şunları sağlamanız gerekir:
* Uç noktanın adı
* Uç nokta türü (Event Grid, Olay Hub 'ı veya Service Bus)
* Kimlik doğrulaması için birincil bağlantı dizesi ve ikincil bağlantı dizesi 
* Uç noktanın konu yolu (örneğin, *Your-topic.westus2.eventgrid.Azure.net* )

Denetim düzleminde kullanılabilen uç nokta API 'Leri şunlardır:
* Uç nokta oluştur
* Uç noktaların listesini al
* Uç noktayı ada göre al
* Uç noktayı ada göre Sil

## <a name="create-an-event-route"></a>Olay yolu oluşturma
 
Bir olay yolu oluşturmak için Azure dijital TWINS [**veri düzlemi API 'leri**](how-to-manage-routes-apis-cli.md#create-an-event-route), [**clı komutları**](how-to-manage-routes-apis-cli.md#manage-endpoints-and-routes-with-cli)veya [**Azure Portal**](how-to-manage-routes-portal.md#create-an-event-route)kullanabilirsiniz. 

`CreateEventRoute` [.Net (C#) SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet-preview&preserve-view=true) çağrısını kullanarak bir istemci uygulaması içinde bir olay yolu oluşturma örneği aşağıda verilmiştir: 

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter("true"); //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

1. İlk olarak, bir `EventRoute` nesne oluşturulur ve Oluşturucu bir uç noktanın adını alır. Bu `endpointName` alan, bir olay hub 'ı, Event Grid veya Service Bus gibi bir uç noktayı tanımlar. Bu uç noktaların aboneliğinizde oluşturulması ve bu kayıt çağrısını yapmadan önce denetim düzlemi API 'Leri kullanılarak Azure Digital TWINS 'e eklenmesi gerekir.

2. Olay Yönlendirme nesnesi Ayrıca, bu yolu izleyen olay türlerini kısıtlamak için kullanılabilecek bir [**filtre**](how-to-manage-routes-apis-cli.md#filter-events) alanına sahiptir. Bir filtresi `true` , ek filtre olmadan rotayı etkinleştirilir (bir filtre `false` yolu devre dışı bırakır). 

3. Bu olay yolu nesnesi daha sonra `CreateEventRoute` yol için bir adla birlikte öğesine geçirilir.

> [!TIP]
> Tüm SDK işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde gelir.

Rotalar [Azure Digital TWINS CLI](how-to-use-cli.md)kullanılarak da oluşturulabilir.

## <a name="dead-letter-events"></a>Atılacak mektup olayları

Bir uç nokta belirli bir süre içinde bir olayı teslim edimezse veya olayı belirli bir sayıda sunmaya çalıştıktan sonra, teslim edilmemiş olayı bir depolama hesabına gönderebilir. Bu işlem, **atılacak** olarak bilinir. **Aşağıdaki koşullardan biri** karşılandığında Azure dijital TWINS, bir olayı atılacak. 

* Etkinlik, yaşam süresi aralığında teslim edilmemiş
* Olayı teslim etmeye yönelik denemeler sayısı sınırı aştı

Koşullardan biri karşılanıyorsa, olay bırakılır veya atılacak. Varsayılan olarak, her uç nokta, atılacak noktaları **yapmaz** . Bunu etkinleştirmek için, uç noktayı oluştururken teslim edilmemiş olayları tutmak üzere bir depolama hesabı belirtmeniz gerekir. Daha sonra teslimleri çözümlemek için bu depolama hesabından olay çekebilirsiniz.

Atılacak mektup konumunu ayarlamadan önce, kapsayıcısı olan bir depolama hesabınız olmalıdır. Uç nokta oluştururken bu kapsayıcının URL 'sini sağlarsınız. Atılacak mektup, bir SAS belirtecine sahip kapsayıcı URL 'SI olarak sağlanır. Bu belirteç yalnızca `write` depolama hesabındaki hedef kapsayıcı için izne ihtiyaç duyuyor. Tamamen oluşturulmuş URL şu biçimde olacaktır: `https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>`

SAS belirteçleri hakkında daha fazla bilgi edinmek için bkz. [ *paylaşılan erişim IMZALARı (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme*](https://docs.microsoft.com/azure/storage/common/storage-sas-overview)

Kullanım dışı bir uç nokta ayarlamayı öğrenmek için bkz. [*nasıl yapılır: Azure dijital TWINS 'de uç noktaları ve yolları yönetme (API 'ler ve CLI)*](how-to-manage-routes-apis-cli.md#create-an-endpoint-with-dead-lettering).

### <a name="types-of-event-messages"></a>Olay iletilerinin türleri

IoT Hub ve Azure dijital TWINS 'de farklı olay türleri aşağıda açıklandığı gibi farklı türlerde bildirim iletileri üretir.

[!INCLUDE [digital-twins-notifications.md](../../includes/digital-twins-notifications.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bkz. bir olay yolunu ayarlama ve yönetme:
* [*Nasıl yapılır: uç noktaları ve yolları yönetme*](how-to-manage-routes-apis-cli.md)

Ya da Azure Digital TWINS içindeki olayları yönlendirmek için bkz. Azure Işlevleri 'ni kullanma:
* [*Nasıl yapılır: verileri işlemek için bir Azure işlevi ayarlama*](how-to-create-azure-function.md)