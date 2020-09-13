---
title: Yüksek kullanılabilirlik-hiper ölçek yapılandırma (Citus)-PostgreSQL için Azure veritabanı
description: Yüksek kullanılabilirliği etkinleştirme veya devre dışı bırakma
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 07/27/2020
ms.openlocfilehash: f08fb01cf26ddf73d31b575242b27d7d8b4017d9
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90033293"
---
# <a name="configure-hyperscale-citus-high-availability"></a>Hiper ölçek (Citus) yüksek kullanılabilirliği yapılandırma

PostgreSQL için Azure veritabanı-hiper ölçek (Citus), veritabanı kapalı kalma süresini önlemek için yüksek kullanılabilirlik (HA) sağlar. HA etkinken, bir sunucu grubundaki her düğüm bekleme moduna alınır. Özgün düğüm sağlıksız hale gelirse, bekleme konumu yerine geçecek şekilde yükseltilir.

> [!IMPORTANT]
> HA gruptaki sunucu sayısını çift katına katdığı için aynı zamanda maliyeti de aynı olacaktır.

Sunucu grubu oluşturma sırasında HA 'nin etkinleştirilmesi veya daha sonra Azure portal sunucu grubunuz için **işlem + depolama** sekmesinde daha sonra mümkündür. Kullanıcı arabirimi her iki durumda da benzer şekilde görünür. **Yüksek kullanılabilirlik** IÇIN kaydırıcıyı Hayır 'dan Evet 'e sürükleyin:

![ha kaydırıcısı](./media/howto-hyperscale-high-availability/01-ha-slider.png)

Seçiminizi uygulamak için **Kaydet** düğmesine tıklayın. Sunucu grubu, her zaman bir veri akışı sağlarken ve bunlara veri akışı sağlarken, HA 'yi etkinleştirme biraz zaman alabilir.

Sunucu grubu için **genel bakış** sekmesi, tüm düğümleri ve bunların kendi kendi bu değerlerini, her düğüm için ha 'nın başarılı bir şekilde etkinleştirilip etkinleştirilmediğini belirten **yüksek kullanılabilirlik** sütunuyla birlikte listeler.

![sunucu grubundaki ha sütununa genel bakış](./media/howto-hyperscale-high-availability/02-ha-column.png)

### <a name="next-steps"></a>Sonraki adımlar

[Yüksek kullanılabilirlik](concepts-hyperscale-high-availability.md)hakkında daha fazla bilgi edinin.
