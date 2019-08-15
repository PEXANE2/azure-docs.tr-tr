---
title: MySQL için Azure veritabanı 'nda desteklenen sürümler
description: MySQL için Azure veritabanı 'nda desteklenen sürümleri açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/09/2019
ms.openlocfilehash: 28d635dc5042799790d032ef4b46bf28118cb326
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947166"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL sunucu sürümleri için desteklenen Azure veritabanı

MySQL için Azure veritabanı, InnoDB altyapısı kullanılarak [MySQL Community Edition](https://www.mysql.com/products/community/)'dan geliştirilmiştir.

MySQL, X. Y. Z adlandırma şemasını kullanır. X ana sürümdür, Y ise küçük sürümdür ve Z hata çözme sürümüdür. Düzen hakkında daha fazla bilgi için [MySQL belgelerine](https://dev.mysql.com/doc/refman/5.7/en/which-version.html)bakın.

> [!NOTE]
> Hizmette, sunucu örneklerine bağlantıları yeniden yönlendirmek için bir ağ geçidi kullanılır. Bağlantı kurulduktan sonra MySQL istemcisi MySQL sunucusu Örneğinizde çalışan gerçek sürümü değil, ağ geçidinde belirlenen MySQL sürümünü görüntüler. MySQL sunucu örneğinizin sürümünü öğrenmek için MySQL komut isteminde `SELECT VERSION();` komutunu kullanın.

MySQL için Azure veritabanı şu anda aşağıdaki sürümleri desteklemektedir:

## <a name="mysql-version-56"></a>MySQL sürüm 5,6

Hata çözme sürümü: 5.6.42

MySQL 5.6.42 'teki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-42.html) bakın.

## <a name="mysql-version-57"></a>MySQL sürüm 5,7

Hata çözme sürümü: 5.7.24

MySQL 5.7.24 'teki geliştirmeler ve düzeltmeler hakkında bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-24.html) bakın.

## <a name="mysql-version-80"></a>MySQL sürüm 8,0

> [!IMPORTANT]
> MySQL 8,0 şu anda önizleme aşamasındadır.

Hata çözme sürümü: 8.0.15

MySQL 8.0.15 'teki geliştirmeler ve düzeltmeler hakkında bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) bakın.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
Hizmet, hata düzeltme sürümü güncelleştirmeleri için düzeltme eki uygulamayı otomatik olarak yönetir. Örneğin, 5.7.20 yükle to 5.7.21.  

Şu anda, küçük ve büyük sürüm yükseltmeleri desteklenmez. Örneğin, MySQL 5,6 ' den MySQL 5,7 ' ye yükseltme desteklenmez. 5,6 ' den 5,7 ' ye yükseltmek istiyorsanız, bir dökümünü alın ve yeni altyapı sürümüyle oluşturulmuş bir sunucuya [geri yükleyin](./concepts-migrate-dump-restore.md) .

## <a name="next-steps"></a>Sonraki adımlar

**Hizmet katmanınıza**dayalı belirli kaynak kotaları ve sınırlamalar hakkında bilgi için bkz. [hizmet katmanları](./concepts-pricing-tiers.md)
