---
title: Günlükler-Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı 'na yönelik veritabanı günlüklerine erişme-hiper ölçek (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: 1dc7bc8e119de7c8fdcf09713286be2633457486
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90895862"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda Günlükler-hiper ölçek (Citus)

PostgreSQL günlükleri, bir hiper ölçek (Citus) sunucu grubunun her düğümünde kullanılabilir. Günlükleri bir depolama sunucusuna veya bir analiz hizmetine gönderebilirsiniz. Günlükler, yapılandırma hatalarını belirlemek, sorunlarını gidermek ve onarmak ve performans performansını düzeltmek için kullanılabilir.

## <a name="accessing-logs"></a>Günlüklere erişme

Hiper ölçek (Citus) Düzenleyicisi veya çalışan düğümü için PostgreSQL günlüklerine erişmek için, Azure portal düğümü açın:

:::image type="content" source="media/howto-hyperscale-logging/choose-node.png" alt-text="düğümlerin listesi":::

Seçili düğüm için **Tanılama ayarları**' nı açın ve **+ Tanılama ayarı Ekle**' ye tıklayın.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-settings.png" alt-text="düğümlerin listesi":::

Yeni Tanılama ayarları için bir ad seçin ve **Postgressqllogs** kutusunu işaretleyin.  Hangi hedefin günlükleri alacağını seçin.

:::image type="content" source="media/howto-hyperscale-logging/diagnostic-create-setting.png" alt-text="düğümlerin listesi":::

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics sorgularını kullanmaya başlama](/azure/azure-monitor/log-query/get-started-portal)
- [Azure Olay Hub 'ları](/azure/event-hubs/event-hubs-about) hakkında bilgi edinin
