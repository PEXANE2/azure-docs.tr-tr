---
title: Portal üzerinden Azure Olay Izgara abonelikleri
description: Bu makalede, Azure portalını kullanarak Azure Blob Depolama gibi desteklenen kaynaklar için Olay Ağı aboneliklerinin nasıl oluşturulacak olduğu açıklanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 599f48ed241010d8551bd110c7f778c9ef508eac
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393170"
---
# <a name="subscribe-to-events-through-portal"></a>Portal üzerinden etkinliklere abone olun

Bu makalede, portal üzerinden Olay Izgara abonelikleri nasıl oluşturulacak açıklanmaktadır.

## <a name="create-event-subscriptions"></a>Etkinlik abonelikleri oluşturma

Desteklenen [olay kaynaklarından](overview.md#event-sources)herhangi biri için Olay Izgara sıyrık aboneliği oluşturmak için aşağıdaki adımları kullanın. Bu makalede, Azure aboneliği için Olay Izgara aboneliği nasıl oluşturulacak gösterilmektedir.

1. **Tüm Hizmetler**’i seçin.

   ![Tüm hizmetleri seçin](./media/subscribe-through-portal/select-all-services.png)

1. Olay **Izgara Abonelikleri'ni** arayın ve kullanılabilir seçeneklerden seçin.

   ![Arama](./media/subscribe-through-portal/search.png)

1. **+ Olay Aboneliği**'ni seçin.

   ![Abonelik ekleme](./media/subscribe-through-portal/add-subscription.png)

1. Oluşturmak istediğiniz abonelik türünü seçin. Örneğin, Azure aboneliğiniz için etkinliklere abone olmak için **Azure Abonelikleri'ni** ve hedef aboneliği seçin.

   ![Azure aboneliğini seçin](./media/subscribe-through-portal/azure-subscription.png)

1. Bu olay kaynağıiçin tüm etkinlik türlerine abone olmak **için, Tüm etkinlik türlerine Abone Ol** seçeneğini işaretlenebiyi tutun. Aksi takdirde, bu abonelik için olay türlerini seçin.

   ![Etkinlik türlerini seçin](./media/subscribe-through-portal/select-event-types.png)

1. Olayları işleme için bitiş noktası ve abonelik adı gibi olay aboneliği hakkında ek ayrıntılar sağlayın.

   ![Abonelik ayrıntılarını sağlama](./media/subscribe-through-portal/provide-subscription-details.png)

1. Ölü harfleri etkinleştirmek ve yeniden deneme ilkelerini özelleştirmek için **Ek Özellikler'i**seçin.

   ![Ek özellikler seçin](./media/subscribe-through-portal/select-additional-features.png)

1. Teslim edilmeyen olayları depolamak için kullanılacak bir kapsayıcı seçin ve yeniden denemelerin nasıl gönderilebildiğini ayarlayın.

   ![Ölü harfleri etkinleştirme ve yeniden deneme](./media/subscribe-through-portal/set-deadletter-retry.png)

1. İşiniz bittiğinde **Oluştur**’u seçin.

## <a name="create-subscription-on-resource"></a>Kaynakta abonelik oluşturma

Bazı olay kaynakları, bu kaynak için portal arabirimi üzerinden bir olay aboneliği oluşturmayı destekler. Olay kaynağını seçin ve sol bölmedeki **Olayları** arayın.

![Abonelik ayrıntılarını sağlama](./media/subscribe-through-portal/resource-events.png)

Portal, bu kaynakla alakalı bir etkinlik aboneliği oluşturmak için seçenekler sunar.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeleri hakkında bilgi için, [Olay Grid ileti teslimi ve yeniden deneme.](delivery-and-retry.md)
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
