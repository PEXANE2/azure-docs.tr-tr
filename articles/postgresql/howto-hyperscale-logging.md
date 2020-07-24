---
title: Günlükler-Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı 'na yönelik veritabanı günlüklerine erişme-hiper ölçek (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 7/13/2020
ms.openlocfilehash: 4ca64a7793d229e7910fb122fb33996b1ac0c4fe
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100217"
---
# <a name="logs-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda Günlükler-hiper ölçek (Citus)

PostgreSQL günlükleri, bir hiper ölçek (Citus) sunucu grubunun her düğümünde kullanılabilir. Günlükleri bir depolama sunucusuna veya bir analiz hizmetine gönderebilirsiniz. Günlükler, yapılandırma hatalarını belirlemek, sorunlarını gidermek ve onarmak ve performans performansını düzeltmek için kullanılabilir.

## <a name="accessing-logs"></a>Günlüklere erişme

Hiper ölçek (Citus) Düzenleyicisi veya çalışan düğümü için PostgreSQL günlüklerine erişmek için, Azure portal düğümü açın:

![düğümlerin listesi](media/howto-hyperscale-logging/choose-node.png)

Seçili düğüm için **Tanılama ayarları**' nı açın ve **+ Tanılama ayarı Ekle**' ye tıklayın.

![Tanılama Ayarları Ekle düğmesi](media/howto-hyperscale-logging/diagnostic-settings.png)

Yeni Tanılama ayarları için bir ad seçin ve **Postgressqllogs** kutusunu işaretleyin.  Hangi hedefin günlükleri alacağını seçin.

![PostgreSQL günlüklerini seçin](media/howto-hyperscale-logging/diagnostic-create-setting.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics sorgularını kullanmaya başlama](/azure/azure-monitor/log-query/get-started-portal)
- [Azure Olay Hub 'ları](/azure/event-hubs/event-hubs-about) hakkında bilgi edinin
