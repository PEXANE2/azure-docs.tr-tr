---
title: Azure Event Grid sistem konuları
description: Azure Event Grid içindeki Sistem konularını açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: spelluru
ms.openlocfilehash: c34ada2c7437d8415b52c68fb66103ec3aa81d95
ms.sourcegitcommit: ba8df8424d73c8c4ac43602678dae4273af8b336
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84456938"
---
# <a name="system-topics-in-azure-event-grid"></a>Azure Event Grid sistem konuları
Sistem konuları, Azure depolama ve Azure Event Hubs gibi Azure hizmetleri için oluşturulan konulardır. Azure portal, PowerShell, CLı veya Azure Resource Manager şablonunu kullanarak sistem konuları oluşturabilirsiniz.  

> [!NOTE]
> Bu özellik şu anda Azure Kamu Bulutu için etkinleştirilmemiş. 

## <a name="create-system-topics"></a>Sistem oluşturma konuları
Bir sistem konusunu iki şekilde oluşturabilirsiniz: 

- Bir Azure kaynağı için bir sistem konusu oluşturun ve ardından bu sistem konusu için bir olay aboneliği oluşturun.
- Azure kaynağında, sizin için dahili olarak bir sistem konusu oluşturan bir olay aboneliği oluşturun.

İlk yaklaşımı kullandığınızda, söz konusu sistem konusunun son olay aboneliği silindiğinde sistem konusu otomatik olarak silinmez. İkinci yaklaşımı kullandığınızda, son olay aboneliği silindiğinde sistem konusu otomatik olarak silinir. 

Azure portal, PowerShell veya CLı kullanarak sistem konuları oluşturmayla ilgili ayrıntılı yönergeler için aşağıdaki makalelere bakın:

- [Azure Portal kullanarak sistem konuları oluşturun, görüntüleyin ve yönetin](create-view-manage-system-topics.md).
- [Azure CLı kullanarak Event Grid sistem konuları oluşturma, görüntüleme ve yönetme](create-view-manage-system-topics-cli.md)
- [Azure Resource Manager şablonları kullanarak Event Grid sistem konuları oluşturma](create-view-manage-system-topics-arm.md)

## <a name="system-topic-name"></a>Sistem konu adı
Event Grid, 11, 15, 2020 tarihinden önce oluşturulan Azure kaynakları (Azure depolama, Azure Event Hubs vb.) için sistem konuları oluşturmamıştı. 3/15/2020 ile 6/2/2020 arasında Azure kaynakları tarafından oluşturulan bir olay için abonelik oluşturduysanız, Event Grid hizmeti **rastgele oluşturulmuş bir ada**sahip bir sistem konusu otomatik olarak oluşturulur. 6/2/2020 sonra, bir Azure kaynağı için bir olay aboneliği oluştururken sistem konusu için bir ad belirtebilirsiniz. 

## <a name="location-and-resource-group"></a>Konum ve kaynak grubu
Belirli bir bölgedeki/konumdaki Azure olay kaynakları için sistem konusu, Azure Olay kaynağıyla aynı konumda oluşturulur. Örneğin, Doğu ABD bir Azure Blob depolama alanı için bir olay aboneliği oluşturursanız, sistem konusu Doğu ABD oluşturulur. Azure abonelikleri, kaynak grupları veya Azure haritaları gibi genel Azure olay kaynakları için Event Grid, **genel** konumdaki Sistem konusunu oluşturur. 

Genel olarak, sistem konusu Azure olay kaynağının bulunduğu kaynak grubunda oluşturulur. Azure abonelik kapsamında oluşturulan olay abonelikleri için, sistem konusu **varsayılan-EventGrid**kaynak grubu altında oluşturulur. Kaynak grubu yoksa, Azure Event Grid Sistem konusunu oluşturmadan önce onu oluşturur. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 

- [Azure Portal kullanarak sistem konuları oluşturun, görüntüleyin ve yönetin](create-view-manage-system-topics.md).
- [Azure CLı kullanarak Event Grid sistem konuları oluşturma, görüntüleme ve yönetme](create-view-manage-system-topics-cli.md)
- [Azure Resource Manager şablonları kullanarak Event Grid sistem konuları oluşturma](create-view-manage-system-topics-arm.md)
