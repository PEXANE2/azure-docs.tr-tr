---
title: MariaDB için Azure veritabanı 'nda desteklenen sürümler
description: MariaDB için Azure veritabanı 'nda desteklenen sürümleri açıklar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 08/21/2019
ms.openlocfilehash: 950294094584958e83f6a16630a6e1f897785e46
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897279"
---
# <a name="supported-azure-database-for-mariadb-server-versions"></a>MariaDB sunucu sürümleri için desteklenen Azure veritabanı

MariaDB için Azure veritabanı, InnoDB altyapısı kullanılarak açık kaynaklı [MariaDB sunucusundan](https://downloads.mariadb.org/)geliştirilmiştir. 

MariaDB, X. Y. Z adlandırma şemasını kullanır. X ana sürümdür, Y ise ikincil sürümdür ve Z, yama sürümüdür.

> [!NOTE]
> Hizmette, sunucu örneklerine bağlantıları yeniden yönlendirmek için bir ağ geçidi kullanılır. Bağlantı kurulduktan sonra MySQL istemcisi, MariaDB sunucu örneğiniz üzerinde çalışan gerçek sürümü değil, ağ geçidinde bulunan MariaDB sürümünü görüntüler. MariaDB sunucu örneğinizin sürümünü öğrenmek için `SELECT VERSION();` komutunu kullanın.

MariaDB için Azure veritabanı şu anda şu sürümü desteklemektedir:

## <a name="mariadb-version-102"></a>MariaDB sürüm 10,2

Düzeltme Eki Sürümü: 10.2.23

MariaDB 10.2.23 'teki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [MariaDB belgelerine](https://mariadb.com/kb/en/library/mariadb-10223-release-notes/) bakın.

## <a name="mariadb-version-103"></a>MariaDB sürüm 10,3

Düzeltme Eki Sürümü: 10.3.14

MariaDB 10.3.14 'teki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [MariaDB belgelerine](https://mariadb.com/kb/en/library/mariadb-10314-release-notes/) bakın.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
Hizmet, düzeltme eki güncelleştirmelerine yönelik yükseltmeleri otomatik olarak yönetir. Örneğin, 10.2.21 to 10.2.23.  

Şu anda, küçük ve büyük sürüm yükseltmeleri desteklenmez. Örneğin, MariaDB 10,2 ' den MariaDB 10,3 ' ye yükseltme desteklenmez. 10,2 ' den 10,3 ' ye yükseltmek istiyorsanız, bir dökümünü alın ve yeni altyapı sürümüyle oluşturulmuş bir sunucuya [geri yükleyin](./howto-migrate-dump-restore.md) .

## <a name="next-steps"></a>Sonraki adımlar

- **Hizmet katmanınıza**dayalı belirli kaynak kotaları ve sınırlamalar hakkında bilgi için bkz. [hizmet katmanları](./concepts-pricing-tiers.md).
