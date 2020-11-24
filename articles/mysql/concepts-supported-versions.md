---
title: Desteklenen sürümler-MySQL için Azure veritabanı
description: MySQL Server 'ın hangi sürümlerini MySQL için Azure veritabanı hizmetinde destekleyeceğinizi öğrenin.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 6/3/2020
ms.openlocfilehash: 8033117d9e3c31f8aa9bba06afb7c3b1b7bba67f
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95751037"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL sunucu sürümleri için desteklenen Azure veritabanı

MySQL için Azure veritabanı, InnoDB altyapısı kullanılarak [MySQL Community Edition](https://www.mysql.com/products/community/)'dan geliştirilmiştir.

MySQL, X. Y. Z adlandırma şemasını kullanır. X ana sürümdür, Y ise küçük sürümdür ve Z hata çözme sürümüdür. Düzen hakkında daha fazla bilgi için [MySQL belgelerine](https://dev.mysql.com/doc/refman/5.7/en/which-version.html)bakın.


> [!NOTE]
> Hizmette, bağlantıları sunucu örneklerine yeniden yönlendirmek için bir ağ geçidi kullanılır. Bağlantı kurulduktan sonra, MySQL istemcisi MySQL sunucu örneğinizde çalışan gerçek sürümü değil ağ geçidinde belirlenen MySQL sürümünü görüntüler. MySQL sunucu örneğinizin sürümünü öğrenmek için MySQL komut isteminde `SELECT VERSION();` komutunu kullanın.

MySQL için Azure veritabanı şu anda aşağıdaki sürümleri desteklemektedir:

## <a name="mysql-version-56"></a>MySQL sürüm 5,6

Hata çözme sürümü: 5.6.47

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-47.html) bakın.

## <a name="mysql-version-57"></a>MySQL sürüm 5,7

Hata çözme sürümü: 5.7.29

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html) bakın.

## <a name="mysql-version-80"></a>MySQL sürüm 8,0

Hata çözme sürümü: 8.0.15

Bu sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için MySQL [sürüm notlarına](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) bakın.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
Hizmet, hata düzeltme sürümü güncelleştirmeleri için düzeltme eki uygulamayı otomatik olarak yönetir. Örneğin, 5.7.20 yükle to 5.7.21.  

Ana sürüm yükseltmesi şu anda, MySQL v 5.6 'dan v 5.7 'e yükseltmeler için hizmet tarafından desteklenmektedir. Daha ayrıntılı bilgi için bkz. [ana sürüm yükseltmeleri gerçekleştirme](how-to-major-version-upgrade.md). 5,7 ' den 8,0 ' ye yükseltmek istiyorsanız, yeni altyapı sürümüyle oluşturulmuş bir sunucuya [döküm ve geri yükleme](./concepts-migrate-dump-restore.md) işlemleri yapmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

- MySQL için Azure veritabanı sürüm oluşturma ilkesi hakkında daha fazla bilgi için [Bu belgeye](concepts-version-policy.md)bakın.
- **Hizmet katmanınıza** dayalı belirli kaynak kotaları ve sınırlamalar hakkında bilgi için bkz. [hizmet katmanları](./concepts-pricing-tiers.md)
