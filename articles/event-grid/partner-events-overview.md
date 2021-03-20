---
title: Azure Event Grid Iş ortağı olayları
description: Üçüncü taraf Event Grid SaaS ve PaaS iş ortaklarından olayları, Azure Event Grid Azure hizmetlerine doğrudan gönderin.
ms.topic: conceptual
ms.date: 11/10/2020
ms.openlocfilehash: 31a5fe611871eb4734b6a68e3818592028ebc75c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94506155"
---
# <a name="partner-events-in-azure-event-grid-preview"></a>Azure Event Grid iş ortağı olayları (Önizleme)
**Iş ortağı olayları** özelliği, bir üçüncü taraf SaaS sağlayıcının, müşterilerin bu olaylara abone olabilmesi için kendi hizmetlerinden olay yayımlamasına olanak sağlar. Bu özellik, bir [Konu](concepts.md#topics) türü olan **iş ortağı konusunun** bulunduğu üçüncü taraf olay kaynaklarına ilk taraf bir deneyim sunar. Aboneler olayları tüketmek için bu konuya abonelikler oluşturur. Ayrıca, olay yayımcıları ve aboneler tarafından kullanılan kaynakların sahiplerini ve sahipliğini ayırarak temiz bir yayın-alt modeli sağlar.

> [!NOTE]
> Event Grid kullanmaya yeni başladıysanız bkz. [genel bakış](overview.md), [Kavramlar](concepts.md)ve [olay işleyicileri](event-handlers.md).

## <a name="what-is-partner-events-to-a-publisher"></a>Bir yayımcıya Iş ortağı olayları nedir?
Bir olay yayımcısına, Iş ortağı olayları özelliği yayımcıların aşağıdaki görevleri yapmasına izin verir:

- Event Grid 'e olay kaynaklarını ekleme
- Olayları yayımlayabilecekleri bir ad alanı (uç nokta) oluşturun
- Azure 'da abonelere ait ve olayları tüketmek için kullanılan iş ortağı konuları oluşturun

## <a name="what-is-partner-events-to-a-subscriber"></a>Bir aboneye Iş ortağı olayları nedir?
Bir aboneye Iş ortağı olayları özelliği, üçüncü taraf olay kaynaklarından gelen olayları tüketmek üzere Azure 'da iş ortağı konuları oluşturmalarına olanak tanır. Olay tüketimi, bir abonenin olay işleyicisine olay Gönderen (göndererek) olay abonelikleri oluşturularak gerçekleştirilir.

## <a name="why-should-i-use-partner-events"></a>Neden Iş ortağı olaylarını kullanmalıyım?
Aşağıdaki gereksinimlerinden biri veya daha fazlası varsa Iş ortağı olaylarını kullanmak isteyebilirsiniz.

### <a name="for-publishers"></a>Yayımcılar için

- Olaylarınızı Azure 'da kullanılabilir hale getirmek için bir mekanizma istiyorsunuz. Kullanıcılarınız, sahip oldukları ve yönettikleri iş ortağı konularını ve olay aboneliklerini kullanarak bu olayları filtreleyebilir ve yönlendirebilir. [Konular](custom-topics.md) ve [etki alanları](event-domains.md)gibi diğer tümleştirme yaklaşımlarını kullanabilirsiniz. Ancak, Yayımcılar ve aboneler arasında kaynak (iş ortağı konuları) sahipliğinin, yönetiminin ve faturalandırılmasına izin vermez. Ayrıca, bu yaklaşım iş ortağı konularını bulmayı ve kullanmayı kolaylaştıran daha sezgisel kullanıcı deneyimi sağlar.
- Olayları tek bir uç noktada, ad alanının uç noktasına yayımlamak istiyorsunuz. Ve, bu olayları yalnızca bir alt kümesinin kullanılabilir olmasını sağlayacak şekilde filtreleyebilmeyi istiyorsunuz. 
- Yayımlanan olaylarla ilgili ölçümlere ilişkin görünürlüğe sahip olmak istiyorsunuz.
- Etkinliklerinizin [bulut olayları 1,0](https://cloudevents.io/) şemasını kullanmak istiyorsunuz.

### <a name="for-subscribers"></a>Aboneler için

- [Üçüncü taraf yayımcıların](#available-third-party-event-publishers) olaylarına abone olmak ve Azure 'da veya başka bir yerde bulunan olay işleyicilerini kullanarak olayları işlemek istiyorsunuz.
- Üçüncü taraf kaynaklardaki olayları işlemek için zengin yönlendirme özellikleri ve [hedefler/olay işleyicilerinden](overview.md#event-handlers) yararlanmak istiyorsunuz. 
- Abone/olay işleyicinizin kullanılan İleti aracısının varlığını farkında olduğu gevşek olarak bağlanmış mimariler uygulamak istiyorsunuz. 
- Yeniden deneme desteği ve en az bir kez semantiği olan dayanıklı bir anında iletme teslim mekanizmasına ihtiyacınız vardır.
- Etkinliklerinizin [bulut olayları 1,0](https://cloudevents.io/) şemasını kullanmak istiyorsunuz. 


## <a name="available-third-party-event-publishers"></a>Kullanılabilir üçüncü taraf olay yayımcıları
Bir abonenin olaylarını tüketmeye başlayabilmesi için bir üçüncü taraf olay yayımcısının bir [ekleme işleminden](partner-onboarding-overview.md) geçmesi gerekir. 

Abone değilseniz ve bir üçüncü taraf hizmetinin olaylarını Event Grid aracılığıyla kullanıma sunmasına istiyorsanız, 

### <a name="auth0"></a>Auth0
**Auth0** , kullanılabilir ilk iş ortağı yayımcısıdır. Auth0 ve Azure hesaplarınızı bağlamak için bir [Auth0 iş ortağı konu başlığı](auth0-overview.md) oluşturabilirsiniz. Bu tümleştirme, Auth0 olaylarını gerçek zamanlı olarak tepki vermenize, günlüğe kaydetmenize ve izlemenize olanak tanır. Denemek için bkz. [Auto0 Ile tümleştirme Azure Event Grid](auth0-how-to.md)

Bir üçüncü taraf hizmetinin olaylarını Event Grid aracılığıyla kullanıma sunmaları isterseniz, [Kullanıcı Voice Portal](https://feedback.azure.com/forums/909934-azure-event-grid)' da fikrini gönderin.
 
## <a name="resources-managed-by-event-publishers"></a>Olay yayımcıları tarafından yönetilen kaynaklar
Olay yayımcıları aşağıdaki kaynakları oluşturur ve yönetir:

### <a name="partner-registration"></a>İş ortağı kaydı
Kayıt, bir yayımcının ilgili genel bilgileri barındırır. Kullanıcılar bir iş ortağı konusu oluşturmaya çalıştıklarında Azure portal bir seçenek olarak gösteren bir iş ortağı konu türü tanımlar. Yayımcı, abonelerinin ihtiyaçlarını karşılamak için birden fazla veya daha fazla iş ortağı konu türü gösterebilir. Diğer bir deyişle, yayımcı farklı hizmetlerden gelen olaylar için ayrı kayıtlar (iş ortağı konu türleri) oluşturabilir. Örneğin, insan kaynakları (HR) hizmeti için, yayımcı çalışana katılmış, çalışanın yükseltildiği ve çalışanın sol tarafında bulunan olaylar için bir iş ortağı konusu tanımlayabilir. 

Aşağıdaki noktaları göz önünde bulundurun:

- Yalnızca Azure onaylı iş ortağı kayıtları görünür. 
- Kayıtlar geneldir. Diğer bir deyişle, belirli bir Azure bölgesiyle ilişkilendirilmemektedir.
- Kayıt, isteğe bağlı bir kaynaktır. Ancak, (yayımcı olarak) bir kayıt oluşturmanız önerilir. Kullanıcıların, [Azure Portal](https://portal.azure.com/#create/Microsoft.EventGridPartnerTopic) **Iş ortağı oluşturma konu başlığı altındaki** konularınızı bulmasına olanak tanır. Ardından, Kullanıcı olay abonelikleri oluştururken olay türlerini (örneğin, çalışana katılmış, çalışan sol vb.) seçebilir.

### <a name="namespace"></a>Ad Alanı
[Özel konular](custom-topics.md) ve [etki alanları](event-domains.md)gibi, iş ortağı ad alanı olayları yayımlamak için bölgesel bir uç noktasıdır. Yayımcıların olay kanalları oluşturup yönetmeleri için ad alanları. Ad alanı olay kanalları için kapsayıcı kaynağı olarak da çalışır.

### <a name="event-channels"></a>Olay kanalları
Bir olay kanalı, bir iş ortağı konusunun yansıtılmış kaynağıdır. Yayımcı, yayımcının Azure aboneliğinde bir olay kanalı oluşturduğunda, bir abonenin Azure aboneliği altında bir iş ortağı konusu da oluşturur. Bir olay kanalında gerçekleştirilen işlemler (GET hariç), karşılık gelen abone iş ortağı konusuna uygulanır, hatta silinir. Ancak, yalnızca iş ortağı konuları, aboneliklerin ve olay tesliminin yapılandırılabileceği kaynak türüdür.

## <a name="resources-managed-by-subscribers"></a>Aboneler tarafından yönetilen kaynaklar 
Aboneler, bir yayımcı tarafından tanımlanan iş ortağı konularını kullanabilir ve bunları görüp yönettikleri tek kaynak türüdür. Bir iş ortağı konusu oluşturulduktan sonra, bir abone kullanıcısı, [hedeflere/olay işleyicilerine](overview.md#event-handlers)filtre kuralları tanımlayan olay abonelikleri oluşturabilir. Aboneler için, bir iş ortağı konusu ve ilişkili olay abonelikleri, [özel konularda](custom-topics.md) ve ilgili abonelikleriyle birlikte aynı özelliklere sahip olan bir önemli farkı sağlar: iş ortağı konuları yalnızca desteklenen diğer şemalardan daha zengin bir özellik kümesi sağlayan [bulut olayları 1,0 şemasını](cloudevents-schema.md)destekler.

Aşağıdaki görüntüde denetim düzlemi işlemlerinin akışı gösterilmektedir.

:::image type="content" source="./media/partner-events-overview/partner-control-plane-flow.png" alt-text="İş ortağı olayları-denetim düzlemi akışı":::

1. Yayımcı bir **iş ortağı kaydı** oluşturur. İş ortağı kayıtları geneldir. Diğer bir deyişle, belirli bir Azure bölgesiyle ilişkilendirilmemektedir. Bu adım isteğe bağlıdır.
1. Yayımcı belirli bir bölgede bir **iş ortağı ad alanı** oluşturur.
1. Abone 1 bir iş ortağı konusu oluşturmayı denediğinde, bir **olay** kanalı olan olay kanalı 1, önce yayımcının Azure aboneliğinde oluşturulur.
1. Daha sonra iş ortağı konu 1 olan **iş** ortağı konusu, abonenin Azure aboneliğinde oluşturulur. Abonenin iş ortağı konusunu etkinleştirmesi gerekir. 
1. Abone 1 Iş ortağı konu 1 ' e bir **Azure Logic Apps aboneliği** oluşturur.
1. Abone 1 Iş ortağı konu 1 ' e bir **Azure Blob depolama aboneliği** oluşturur. 
1. Abone 2 bir iş ortağı konusu oluşturmayı denediğinde, önce yayımcının Azure aboneliğinde başka bir **olay kanalı**, olay kanalı 2 oluşturulur. 
1. Daha sonra iş **ortağı konu** 2, Ikinci abonenin Azure aboneliğinde oluşturulur. Abonenin iş ortağı konusunu etkinleştirmesi gerekir. 
1. Abone 2 Iş ortağı konu 2 ' ye bir **Azure işlevleri aboneliği** oluşturur. 

## <a name="pricing"></a>Fiyatlandırma
İş ortağı konuları Event Grid kullanılırken gerçekleştirilen işlem sayısına göre ücretlendirilir. Faturalandırma ve ayrıntılı fiyat bilgileri için temel olarak kullanılan tüm işlem türleri hakkında daha fazla bilgi için bkz. [Event Grid fiyatlandırması](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="limits"></a>Sınırlar
İş ortağı konuları için sınırlamalar hakkında ayrıntılı bilgi için bkz. [Event Grid hizmet limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#event-grid-limits) .


## <a name="next-steps"></a>Sonraki adımlar

- [Auth0 iş ortağı konusu](auth0-overview.md)
- [Auth0 iş ortağı konusunu kullanma](auth0-how-to.md)
- [Event Grid iş ortağı olun](partner-onboarding-overview.md)