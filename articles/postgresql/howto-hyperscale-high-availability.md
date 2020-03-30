---
title: Yüksek kullanılabilirlik yapılandırma - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Yüksek kullanılabilirliği etkinleştirme veya devre dışı etme
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: a8d4b5949b34d16191e9ec10a1dd39faff3660dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74977684"
---
# <a name="configure-hyperscale-citus-high-availability"></a>Yapılandırılan Hiperölçek (Citus) yüksek kullanılabilirlik

PostgreSQL için Azure Veritabanı - Hyperscale (Citus), veritabanı nın kapalı kalma süresini önlemek için yüksek kullanılabilirlik (HA) sağlar. HA etkinken, sunucu grubundaki her düğüm bekleme de alır. Orijinal düğüm sağlıksız hale gelirse, onun standby yerine yükseltilir.

> [!IMPORTANT]
> HA, gruptaki sunucu sayısını ikikatına çıkardığından, maliyeti de ikikatına çıkar.

HA'yı etkinleştirmek sunucu grubu oluşturma sırasında veya daha sonra Azure portalındaki sunucu grubunuz için Yapılandırma sekmesinde **mümkündür.** Kullanıcı arabirimi her iki durumda da benzer görünür. **Yüksek kullanılabilirlik** için kaydırıcıyı EVET'e sürükleyin:

![ha sürgü](./media/howto-hyperscale-high-availability/01-ha-slider.png)

Seçiminizi uygulamak için **Kaydet** düğmesini tıklatın. Sunucu grubu standby'leri sağladığı ve verileri bunlara aktardEttiği için HA'yı etkinleştirmek biraz zaman alabilir.

Sunucu grubu için **Genel Bakış** sekmesi, tüm düğümleri ve bekleme yerlerini ve her düğüm için HA'nın başarıyla etkinleştirilip etkinleştirilemediğini belirten **yüksek kullanılabilirlik** sütunu listeler.

![sunucu grubuna genel bakış ha sütun](./media/howto-hyperscale-high-availability/02-ha-column.png)

### <a name="next-steps"></a>Sonraki adımlar

[Yüksek kullanılabilirlik](concepts-hyperscale-high-availability.md)hakkında daha fazla bilgi edinin.
