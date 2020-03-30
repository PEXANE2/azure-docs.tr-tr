---
title: Desteklenen sürümler - MySQL için Azure Veritabanı
description: MySQL hizmeti için Azure Veritabanı'nda MySQL sunucusunun hangi sürümlerinin desteklendiğine öğrenin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 197b3100190711a51cfe125fe1214a59c18e1491
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536981"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL sunucu sürümleri için desteklenen Azure Veritabanı

MySQL için Azure Veritabanı, InnoDB altyapısı kullanılarak [MySQL Community Edition'dan](https://www.mysql.com/products/community/)geliştirilmiştir.

MySQL X.Y.Z adlandırma düzenini kullanır. X ana sürümüdür, Y küçük sürümüdür ve Z hata düzeltme sagünüdür. Şema hakkında daha fazla bilgi için [MySQL belgelerine](https://dev.mysql.com/doc/refman/5.7/en/which-version.html)bakın.

> [!NOTE]
> Hizmette, bağlantıları sunucu örneklerine yeniden yönlendirmek için bir ağ geçidi kullanılır. Bağlantı kurulduktan sonra, MySQL istemcisi MySQL sunucu örneğinizde çalışan gerçek sürümü değil ağ geçidinde belirlenen MySQL sürümünü görüntüler. MySQL sunucu örneğinizin sürümünü öğrenmek için MySQL komut isteminde `SELECT VERSION();` komutunu kullanın.

MySQL için Azure Veritabanı şu anda aşağıdaki sürümleri destekler:

## <a name="mysql-version-56"></a>MySQL Sürüm 5.6

Hata düzeltme sürümü: 5.6.45

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-45.html) bakın.

## <a name="mysql-version-57"></a>MySQL Sürüm 5.7

Hata düzeltme sürümü: 5.7.27

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-27.html) bakın.

## <a name="mysql-version-80"></a>MySQL Sürüm 8.0

Hata düzeltme sürümü: 8.0.15

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) bakın.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
Hizmet, hata düzeltme sürüm güncelleştirmeleri için düzeltmeyi otomatik olarak yönetir. Örneğin, 5.7.20 ile 5.7.21 arasında.  

Şu anda alt ve ana sürüm yükseltmeleri desteklenmez. Örneğin MySQL 5.6'dan MySQL 5.7'ye yükseltme desteklenmez. 5.6'dan 5.7'ye yükseltmek isterseniz [döküm alın ve bunu yeni altyapı sürümüyle oluşturulmuş bir sunucuya geri yükleyin](./concepts-migrate-dump-restore.md).

## <a name="next-steps"></a>Sonraki adımlar

**Hizmet katmanınıza**dayalı belirli kaynak kotaları ve sınırlamaları hakkında bilgi için [Bkz.](./concepts-pricing-tiers.md)
