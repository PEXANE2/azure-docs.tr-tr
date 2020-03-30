---
title: Desteklenen sürümler - MariaDB için Azure Veritabanı
description: MariaDB hizmeti için Azure Veritabanı'nda MariaDB sunucusunun hangi sürümlerinin desteklendiğine öğrenin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 361ba17532d27a7020be1b6874993da999f48604
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527716"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>MariaDB sunucu sürümleri için desteklenen Azure Veritabanı

MariaDB için Azure Veritabanı, InnoDB altyapısı kullanılarak açık kaynak [mariadb server'dan](https://downloads.mariadb.org/)geliştirilmiştir.

MariaDB X.Y.Z adlandırma şemasını kullanır. X ana sürümüdür, Y küçük sürümüdür ve Z yama sürümüdür.

> [!NOTE]
> Hizmette, bağlantıları sunucu örneklerine yeniden yönlendirmek için bir ağ geçidi kullanılır. Bağlantı kurulduktan sonra, MySQL istemcisi MariaDB sunucu örneğiniz üzerinde çalışan gerçek sürümü değil, ağ geçidinde mariadb kümesi sürümünü görüntüler. MariaDB sunucu örneğinizin sürümünü belirlemek için `SELECT VERSION();` komutu kullanın.

MariaDB için Azure Veritabanı şu anda aşağıdaki sürümü destekler:

## <a name="mariadb-version-102"></a>MariaDB Sürüm 10.2

Yama sürümü: 10.2.25

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [MariaDB belgelerine](https://mariadb.com/kb/en/library/mariadb-10225-release-notes/) bakın.

## <a name="mariadb-version-103"></a>MariaDB Sürüm 10.3

Yama sürümü: 10.3.16

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [MariaDB belgelerine](https://mariadb.com/kb/en/library/mariadb-10316-release-notes/) bakın.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
Hizmet, yama güncelleştirmeleri için yükseltmeleri otomatik olarak yönetir. Örneğin, 10.2.21 ila 10.2.23.  

Şu anda alt ve ana sürüm yükseltmeleri desteklenmez. Örneğin MariaDB 10.2'den MariaDB 10.3'e yükseltme desteklenmez. 10,2'den 10,3'e yükseltmek istiyorsanız, bir dökümü alın ve yeni motor sürümüyle oluşturulan bir sunucuya [geri yükleyin.](./howto-migrate-dump-restore.md)

## <a name="next-steps"></a>Sonraki adımlar

- **Hizmet katmanınızı**temel alan belirli kaynak kotaları ve sınırlamaları hakkında bilgi için [Bkz. Hizmet katmanları.](./concepts-pricing-tiers.md)
