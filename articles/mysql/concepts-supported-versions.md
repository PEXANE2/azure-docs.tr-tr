---
title: Desteklenen sürümler-MySQL için Azure veritabanı
description: MySQL Server 'ın hangi sürümlerini MySQL için Azure veritabanı hizmetinde destekleyeceğinizi öğrenin.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: conceptual
ms.date: 6/3/2020
ms.openlocfilehash: 8b85307f01a11366a2147c947f26658f548932e8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103467723"
---
# <a name="supported-azure-database-for-mysql-server-versions"></a>MySQL sunucu sürümleri için desteklenen Azure veritabanı

MySQL için Azure veritabanı, InnoDB depolama altyapısı kullanılarak [MySQL Community Edition](https://www.mysql.com/products/community/)'dan geliştirilmiştir. Hizmet, topluluk tarafından desteklenen tüm geçerli ana sürümü destekler, burada MySQL 5,6, 5,7 ve 8,0. MySQL, X 'in ana sürüm olan X. Y. Z adlandırma şemasını kullanır, Y ise ikincil sürümdür ve Z hata çözme sürümüdür. Düzen hakkında daha fazla bilgi için [MySQL belgelerine](https://dev.mysql.com/doc/refman/5.7/en/which-version.html)bakın.

> [!NOTE]
> Tek sunuculu dağıtım seçeneğinde, sunucu örneklerine bağlantıları yeniden yönlendirmek için bir ağ geçidi kullanılır. Bağlantı kurulduktan sonra, MySQL istemcisi MySQL sunucu örneğinizde çalışan gerçek sürümü değil ağ geçidinde belirlenen MySQL sürümünü görüntüler. MySQL sunucu örneğinizin sürümünü öğrenmek için MySQL komut isteminde `SELECT VERSION();` komutunu kullanın.

MySQL için Azure veritabanı şu anda MySQL 'in aşağıdaki büyük ve küçük sürümlerini desteklemektedir:


| Sürüm | Tek sunucu <br/> Geçerli ikincil sürüm |Esnek Sunucu (Önizleme) <br/> Geçerli ikincil sürüm  |
|:-------------------|:-------------------------------------------|:---------------------------------------------|
|MySQL sürüm 5,6 |  [5.6.47](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-47.html) (kullanımdan kaldırıldı) | Desteklenmez|
|MySQL sürüm 5,7 | [5.7.29](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html) | [5.7.29](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-29.html)|
|MySQL sürüm 8,0 | [8.0.15](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-15.html) | [8.0.21](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-21.html)|

[Sürüm desteği ilkesi belgelerindeki](concepts-version-policy.md#retired-mysql-engine-versions-not-supported-in-azure-database-for-mysql) kullanımdan kaldırılan sürümler için sürüm desteği ilkesini okuyun.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
Hizmet, hata düzeltme sürümü güncelleştirmeleri için düzeltme eki uygulamayı otomatik olarak yönetir. Örneğin, 5.7.20 yükle to 5.7.21.  

Ana sürüm yükseltmesi şu anda, MySQL v 5.6 'dan v 5.7 'e yükseltmeler için hizmet tarafından desteklenmektedir. Daha ayrıntılı bilgi için bkz. [ana sürüm yükseltmeleri gerçekleştirme](how-to-major-version-upgrade.md). 5,7 ' den 8,0 ' ye yükseltmek istiyorsanız, yeni altyapı sürümüyle oluşturulmuş bir sunucuya [döküm ve geri yükleme](./concepts-migrate-dump-restore.md) işlemleri yapmanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar

- MySQL için Azure veritabanı sürüm oluşturma ilkesi hakkında daha fazla bilgi için [Bu belgeye](concepts-version-policy.md)bakın.
- **Hizmet katmanınıza** dayalı belirli kaynak kotaları ve sınırlamalar hakkında bilgi için bkz. [hizmet katmanları](./concepts-pricing-tiers.md)
