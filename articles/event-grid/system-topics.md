---
title: Azure Event Grid sistem konuları
description: Azure Event Grid içindeki Sistem konularını açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: spelluru
ms.openlocfilehash: 67746ebd8a16eb02b8f02d238b0e3c0125989189
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84308277"
---
# <a name="system-topics-in-azure-event-grid"></a>Azure Event Grid sistem konuları
Azure Event Grid hizmeti, bir Azure Olay kaynağı için ilk olay aboneliği oluştururken sistem konuları oluşturur. Şu anda, Event Grid 11, 15, 2020 tarihinden önce oluşturulan konu kaynakları için sistem konuları oluşturmaz. Bu tarihte veya bu tarihten sonra oluşturduğunuz tüm konu kaynakları için Event Grid otomatik olarak sistem konuları oluşturur. Bu makalede Azure Event Grid içindeki **sistem konuları** açıklanmaktadır.

> [!NOTE]
> Bu özellik şu anda Azure Kamu Bulutu için etkinleştirilmemiş. 

## <a name="overview"></a>Genel Bakış
Azure depolama hesabı gibi bir Azure Olay kaynağı için ilk olay aboneliği oluşturduğunuzda, abonelik için sağlama işlemi **Microsoft. EventGrid/Systemkonuları**türünde ek bir kaynak oluşturur. Azure olay kaynağına yönelik son olay aboneliği silindiğinde, sistem konusu otomatik olarak silinir.

Sistem konusu özel konu senaryoları, diğer bir deyişle Event Grid konular ve Event Grid etki alanları için geçerli değildir. 

## <a name="name"></a>Name 
Daha önce, Azure kaynakları tarafından oluşturulan bir olay için abonelik oluşturduğunuzda, Event Grid hizmeti **rastgele oluşturulmuş bir ada**sahip bir sistem konusu otomatik olarak oluşturulur. Artık Azure portal konuyu oluştururken sistem konusu için bir ad belirtebilirsiniz. Bu sistem konu kaynağını, ölçümleri ve tanılama günlüklerini saptamak için kullanabilirsiniz.

## <a name="location"></a>Konum
Belirli bir bölgedeki/konumdaki Azure olay kaynakları için sistem konusu, Azure Olay kaynağıyla aynı konumda oluşturulur. Örneğin, Doğu ABD bir Azure Blob depolama alanı için bir olay aboneliği oluşturursanız, sistem konusu Doğu ABD oluşturulur. Azure abonelikleri, kaynak grupları veya Azure haritaları gibi genel Azure olay kaynakları için Event Grid, **genel** konumdaki Sistem konusunu oluşturur. 

## <a name="resource-group"></a>Kaynak grubu 
Genel olarak, sistem konusu Azure olay kaynağının bulunduğu kaynak grubunda oluşturulur. Azure abonelik kapsamında oluşturulan olay abonelikleri için, sistem konusu **varsayılan-EventGrid**kaynak grubu altında oluşturulur. Kaynak grubu yoksa, Azure Event Grid Sistem konusunu oluşturmadan önce onu oluşturur. 

Kaynak grubunu depolama hesabıyla silmeye çalıştığınızda, etkilenen kaynaklar listesinde Sistem konusunu görürsünüz.  

![Kaynak grubunu silme](./media/system-topics/delete-resource-group.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın: [sistem konularını oluşturma, görüntüleme ve yönetme](create-view-manage-system-topics.md).