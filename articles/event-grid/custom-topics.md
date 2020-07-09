---
title: Azure Event Grid özel konular
description: Azure Event Grid özel konuları açıklar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 8f48824989c8ec51b766385188ad99e9e59cf621
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86113794"
---
# <a name="custom-topics-in-azure-event-grid"></a>Azure Event Grid özel konular
Olay Kılavuzu konusu, kaynağın olayları gönderdiği bir uç nokta sağlar. Yayımcı, olay Kılavuzu konusunu oluşturur ve bir olay kaynağının bir konu veya birden fazla konu ihtiyacı olup olmadığına karar verir. İlgili olayların toplanması için bir konu kullanılır. Belirli olay türlerine yanıt vermek için, aboneler hangi konuların abone olacağına karar verir.

**Özel konular** uygulama ve üçüncü taraf konulardır. Özel konu başlığı oluşturduğunuzda veya özel konu başlığına erişim için atandığınızda, aboneliğinizde özel konu başlığını görürsünüz. 

Uygulamanızı tasarlarken, kaç tane konu oluşturacağına karar verirken esneklik elde edersiniz. Büyük çözümler için **ilgili olayların her kategorisi**için **özel bir konu** oluşturun. Örneğin, kullanıcı hesaplarını değiştirme ve siparişleri işleme ile ilgili olaylar gönderen bir uygulamayı ele alalım. Herhangi bir olay işleyicisinin her iki olay kategorisini de istemesi pek olası değildir. İki özel konu başlığı oluşturun ve olay işleyicilerinin ilgilendikleri konu başlığına abone olmalarına izin verin. Küçük çözümler için tüm olayları tek bir konuya göndermenizi tercih edebilirsiniz. Olay aboneleri istedikleri olay türlerini filtreleyebilir.

## <a name="event-schema"></a>Olay şeması
Olay şemasına ilişkin ayrıntılı bir genel bakış için bkz. [Azure Event Grid olay şeması](event-schema.md). Özel konularda, Olay Yayımcısı **veri** nesnesini belirler. En üst düzey veriler standart kaynak tanımlı olaylarla aynı alanlara sahip olmalıdır.

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

Aşağıdaki bölümlerde Azure portal, CLı, PowerShell ve Azure Resource Manager (ARM) şablonları kullanarak özel konular oluşturmaya yönelik öğreticilerin bağlantıları sağlanmaktadır. 


## <a name="azure-portal-tutorials"></a>Azure portal öğreticileri
|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Azure portal özel olaylar oluşturma ve yönlendirme](custom-event-quickstart-portal.md) | Portalın özel olayları göndermek için nasıl kullanılacağını gösterir. |
| [Hızlı başlangıç: özel olayları Azure kuyruk depolama 'ya yönlendirme](custom-event-to-queue-storage.md) | Bir kuyruk depolamasına özel olayların nasıl gönderileceğini açıklar. |
| [Nasıl yapılır: özel konuya gönderi](post-to-custom-topic.md) | Bir olayın özel bir konuya nasıl nakledileceğini gösterir. |


## <a name="azure-cli-tutorials"></a>Azure CLı öğreticileri
|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Azure CLı ile özel olaylar oluşturma ve yönlendirme](custom-event-quickstart.md) | Özel olayları göndermek için Azure CLı 'nın nasıl kullanılacağını gösterir. |
| [Azure CLı: Event Grid özel konu oluştur](./scripts/event-grid-cli-create-custom-topic.md)|Özel bir konu oluşturan örnek komut dosyası. Betik, uç noktayı ve bir anahtarı alır.|
| [Azure CLı: özel konu için olaylara abone olma](./scripts/event-grid-cli-subscribe-custom-topic.md)|Özel konu için abonelik oluşturan örnek komut dosyası. Olayları bir Web kancasına gönderir.|

## <a name="azure-powershell-tutorials"></a>Azure PowerShell öğreticileri
|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Azure PowerShell özel olaylar oluşturma ve yönlendirme](custom-event-quickstart-powershell.md) | Özel olayları göndermek için Azure PowerShell nasıl kullanacağınızı gösterir. |
| [PowerShell: Event Grid özel konu oluştur](./scripts/event-grid-powershell-create-custom-topic.md)|Özel bir konu oluşturan örnek komut dosyası. Betik, uç noktayı ve bir anahtarı alır.|
| [PowerShell: özel konu için olaylara abone olma](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Özel konu için abonelik oluşturan örnek komut dosyası. Olayları bir Web kancasına gönderir.|

## <a name="arm-template-tutorials"></a>ARM şablon öğreticileri
|Başlık  |Açıklama  |
|---------|---------|
| [Kaynak Yöneticisi şablonu: özel konu ve Web kancası uç noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) | Bu özel konu için özel konu ve abonelik oluşturan Kaynak Yöneticisi şablonu. Olayları bir Web kancasına gönderir. |
| [Kaynak Yöneticisi şablonu: özel konu ve Event Hubs uç noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Özel konu için abonelik oluşturan Kaynak Yöneticisi şablonu. Olayları bir Azure Event Hubs gönderir. |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Sistem konuları](system-topics.md)
- [Etki alanları](event-domains.md)