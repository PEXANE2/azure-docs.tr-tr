---
title: Azure Event Grid 'de olay etki alanları için kullanım örnekleri
description: Bu makalede Azure Event Grid içindeki olay etki alanlarını kullanmaya yönelik birkaç kullanım durumu açıklanır.
ms.topic: conceptual
ms.date: 03/04/2021
ms.openlocfilehash: 00318fc78053ed55e3599c329746d89d2eee4f99
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102204413"
---
# <a name="use-cases-for-event-domains-in-azure-event-grid"></a>Azure Event Grid 'de olay etki alanları için kullanım örnekleri
Bu makalede Azure Event Grid içindeki olay etki alanlarını kullanmaya yönelik birkaç kullanım durumu açıklanır. 

## <a name="use-case-1"></a>Kullanım durumu 1 
[!INCLUDE [event-grid-domain-example-use-case.md](../../includes/event-grid-domain-example-use-case.md)]

## <a name="use-case-2"></a>Kullanım durumu 2
Sistem konuları kullanılırken 500 olay aboneliği sınırlaması vardır. Bir sistem konusu için 500 ' den fazla etkinlik aboneliğine ihtiyacınız varsa, etki alanlarını kullanabilirsiniz. 

Azure Blob depolama için bir sistem konusu oluşturdunuz ve konuya 500 'den fazla abonelik oluşturmanız gerektiğini varsayalım, ancak bu sınırlama nedeniyle (Konu başına 500 abonelikleri) mümkün değildir. Bu durumda, bir olay etki alanı kullanan aşağıdaki çözümü kullanabilirsiniz. 

1. En fazla 100.000 konuyu destekleyebilen ve her bir etki alanı konusunun 500 olay abonelikleri olabilecek bir etki alanı oluşturun. Bu model size 500 * 100.000 olay abonelikleri sağlar. 
1. Azure depolama sistemi konusu için bir Azure işlev aboneliği oluşturun. İşlev Azure Storage 'dan olayları aldığında, uygun bir etki alanı konusunun olaylarını zenginleştirerek yayımlayabilir. Örneğin, işlev, hedef etki alanı konusunu ve olayı etki alanı konusuna yayımlamak için blob dosya adını ayrıştırabilecek. 

:::image type="content" source="./media/event-domains-use-cases/use-case-2.jpg" alt-text="AzureEvent Grid etki alanları-kullanım durumu 2":::


## <a name="next-steps"></a>Sonraki adımlar
Olay etki alanlarını ayarlama, konu oluşturma, olay abonelikleri oluşturma ve olayları yayımlama hakkında bilgi edinmek için bkz. [olay etki alanlarını yönetme](./how-to-event-domains.md).
