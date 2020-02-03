---
title: Portal üzerinden abonelikler Azure Event Grid
description: Bu makalede, Azure portal kullanılarak Azure Blob depolama gibi desteklenen kaynaklar için Event Grid aboneliklerin nasıl oluşturulacağı açıklanır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 3172c92ecae094ab5d978803d2ccac7e6404a5e1
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721515"
---
# <a name="subscribe-to-events-through-portal"></a>Portala etkinliklere abone olma

Bu makalede Portal üzerinden Event Grid aboneliklerin nasıl oluşturulacağı açıklanır.

## <a name="create-event-subscriptions"></a>Olay abonelikleri oluşturma

Desteklenen [olay kaynaklarından](event-sources.md)herhangi biri için bir Event Grid aboneliği oluşturmak için aşağıdaki adımları kullanın. Bu makalede, bir Azure aboneliği için Event Grid aboneliğinin nasıl oluşturulacağı gösterilmektedir.

1. **Tüm Hizmetler**’i seçin.

   ![Tüm hizmetleri seçin](./media/subscribe-through-portal/select-all-services.png)

1. **Event Grid abonelikleri** arayın ve kullanılabilir seçeneklerden seçin.

   ![Arama](./media/subscribe-through-portal/search.png)

1. **+ Olay Aboneliği**'ni seçin.

   ![Abonelik ekleme](./media/subscribe-through-portal/add-subscription.png)

1. Oluşturmak istediğiniz abonelik türünü seçin. Örneğin, Azure Aboneliğinize yönelik olaylara abone olmak için **Azure abonelikleri** ' ni ve hedef aboneliği seçin.

   ![Azure aboneliğini seçin](./media/subscribe-through-portal/azure-subscription.png)

1. Bu olay kaynağı için tüm olay türlerine abone olmak için **tüm olay türlerine abone ol** seçeneğini işaretlenmiş olarak tutun. Aksi takdirde, bu abonelik için olay türlerini seçin.

   ![Olay türlerini seçin](./media/subscribe-through-portal/select-event-types.png)

1. Olay aboneliği hakkında olayları ve abonelik adını işlemek için uç nokta gibi ek ayrıntılar sağlayın.

   ![Abonelik ayrıntılarını belirtin](./media/subscribe-through-portal/provide-subscription-details.png)

1. Geçersiz kılmak ve yeniden deneme ilkelerini özelleştirmek için **ek özellikler**' i seçin.

   ![Ek özellikleri seçin](./media/subscribe-through-portal/select-additional-features.png)

1. Teslim edilmeyen olayları depolamak için kullanılacak bir kapsayıcı seçin ve yeniden denemelerin gönderilme şeklini ayarlayın.

   ![Atılacak ve yeniden denemeyi etkinleştir](./media/subscribe-through-portal/set-deadletter-retry.png)

1. İşiniz bittiğinde **Oluştur**’u seçin.

## <a name="create-subscription-on-resource"></a>Kaynak üzerinde abonelik oluştur

Bazı olay kaynakları, bu kaynak için Portal arabirimi aracılığıyla bir olay aboneliği oluşturmayı destekler. Olay kaynağını seçin ve sol bölmedeki **olayları** arayın.

![Abonelik ayrıntılarını belirtin](./media/subscribe-through-portal/resource-events.png)

Portal size bu kaynakla ilgili bir olay aboneliği oluşturma seçeneklerini sunar.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeler hakkında daha fazla bilgi için [Event Grid ileti teslimi ve yeniden deneyin](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid kullanmaya hızlıca başlamak için bkz. [özel olayları oluşturma ve Azure Event Grid ile yönlendirme](custom-event-quickstart.md).
