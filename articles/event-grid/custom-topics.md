---
title: Azure Etkinlik Ağıt'ında özel konular
description: Azure Etkinlik Ağıt'ında özel konuları açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: spelluru
ms.openlocfilehash: 166a48d74d32c8b0a3a59310e693ea96ada29116
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394364"
---
# <a name="custom-topics-in-azure-event-grid"></a>Azure Etkinlik Ağıt'ında özel konular
Olay ızgarası konusu, kaynağın olayları gönderdiği bir bitiş noktası sağlar. Yayımcı olay ızgarası konusunu oluşturur ve olay kaynağının bir konuya mı yoksa birden fazla konuya mı ihtiyacı olacağına karar verir. Bir konu, ilgili olaylar topluluğu için kullanılır. Belirli türdeki olaylara yanıt vermek için, aboneler hangi konulara abone olunacağına karar verir.

**Özel konular** uygulama ve üçüncü taraf konularıdır. Özel konu başlığı oluşturduğunuzda veya özel konu başlığına erişim için atandığınızda, aboneliğinizde özel konu başlığını görürsünüz. 

Uygulamanızı tasarlarken, kaç konu oluşturabileceğinize karar verirken esnekliğe sahip siniz. Büyük çözümler için, ilgili **olayların her kategorisi**için özel bir **konu** oluşturun. Örneğin, kullanıcı hesaplarını değiştirme ve siparişleri işleme ile ilgili olaylar gönderen bir uygulamayı ele alalım. Herhangi bir olay işleyicisinin her iki olay kategorisini de istemesi pek olası değildir. İki özel konu başlığı oluşturun ve olay işleyicilerinin ilgilendikleri konu başlığına abone olmalarına izin verin. Küçük çözümler için, tüm olayları tek bir konuya göndermeyi tercih edebilirsiniz. Olay aboneleri istedikleri olay türleri için filtre uygulayabilirsiniz.

## <a name="event-schema"></a>Olay şeması
Olay şemasına ayrıntılı bir genel bakış için Azure [Olay Ağı olay şemasına](event-schema.md)bakın. Özel konular için olay yayımcısı **veri** nesnesini belirler. Üst düzey veriler, standart kaynak tanımlı olaylarla aynı alanlara sahip olmalıdır.

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

Aşağıdaki bölümlerde Azure portalı, CLI, PowerShell ve Azure Kaynak Yöneticisi (ARM) şablonlarını kullanarak özel konular oluşturmak için öğreticilere bağlantılar sağlanmaktadır. 


## <a name="azure-portal-tutorials"></a>Azure portalı eğitimleri
|Başlık  |Açıklama  |
|---------|---------|
| [Quickstart: Azure portalı ile özel etkinlikler oluşturun ve yönlendirin](custom-event-quickstart-portal.md) | Özel olaylar göndermek için portalın nasıl kullanılacağını gösterir. |
| [Hızlı başlangıç: Özel olayları Azure Sıra depolama alanına yönlendirin](custom-event-to-queue-storage.md) | Özel olayların Sıra depolama alanına nasıl gönderilebildiğini açıklar. |
| [Nasıl: özel konuya göndermek](post-to-custom-topic.md) | Bir olayı özel bir konuya nasıl yayınlayabildiğini gösterir. |


## <a name="azure-cli-tutorials"></a>Azure CLI eğitimleri
|Başlık  |Açıklama  |
|---------|---------|
| [Quickstart: Azure CLI ile özel etkinlikler oluşturun ve yönlendirin](custom-event-quickstart.md) | Özel etkinlikler göndermek için Azure CLI'nin nasıl kullanılacağını gösterir. |
| [Azure CLI: Olay Ağı özel konu oluşturma](./scripts/event-grid-cli-create-custom-topic.md)|Özel bir konu oluşturan örnek komut dosyası. Komut dosyası bitiş noktasını ve anahtarı alır.|
| [Azure CLI: Özel bir konu için etkinliklere abone olun](./scripts/event-grid-cli-subscribe-custom-topic.md)|Özel bir konu için abonelik oluşturan örnek komut dosyası. Olayları Bir WebHook'a gönderir.|

## <a name="azure-powershell-tutorials"></a>Azure PowerShell eğitimleri
|Başlık  |Açıklama  |
|---------|---------|
| [Quickstart: Azure PowerShell ile özel etkinlikler oluşturun ve yönlendirin](custom-event-quickstart-powershell.md) | Özel etkinlikler göndermek için Azure PowerShell'in nasıl kullanılacağını gösterir. |
| [PowerShell: Olay Izgara özel konu oluşturmak](./scripts/event-grid-powershell-create-custom-topic.md)|Özel bir konu oluşturan örnek komut dosyası. Komut dosyası bitiş noktasını ve anahtarı alır.|
| [PowerShell: özel bir konu için etkinliklere abone](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Özel bir konu için abonelik oluşturan örnek komut dosyası. Olayları Bir WebHook'a gönderir.|

## <a name="arm-template-tutorials"></a>ARM şablon uyramaları
|Başlık  |Açıklama  |
|---------|---------|
| [Kaynak Yöneticisi şablonu: özel konu ve WebHook bitiş noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid) | Özel bir konu ve bu özel konu için abonelik oluşturan bir Kaynak Yöneticisi şablonu. Olayları Bir WebHook'a gönderir. |
| [Kaynak Yöneticisi şablonu: özel konu ve Olay Hub'ları bitiş noktası](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Özel bir konu için abonelik oluşturan Kaynak Yöneticisi şablonu. Etkinlikleri bir Azure Etkinlik Hub'larına gönderir. |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Sistem konu başlıkları](system-topics.md)
- [Etki alanları](event-domains.md)