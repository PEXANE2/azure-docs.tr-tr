---
title: Veri çoğaltma-MariaDB için Azure veritabanı
description: Bir dış sunucudan MariaDB hizmeti için Azure veritabanı ile eşitleme yapmak üzere verileri çoğaltma hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: e98f0dffe1ae004905c2b0969d825a1bca89014a
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74772647"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'na veri çoğaltma

Gelen Verileri Çoğaltma, şirket içinde, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetlerinde çalışan bir MariaDB sunucusundan, MariaDB için Azure Veritabanı hizmetine verileri eşitlemenize olanak sağlar. Gelen Verileri Çoğaltma, MariaDB’de yerel olan ikili günlük (binlog) dosya konumuna dayalı çoğaltmayı temel alır. Binlog çoğaltma hakkında daha fazla bilgi edinmek için bkz. [binlog Çoğaltmaya genel bakış](https://mariadb.com/kb/en/library/replication-overview/).

## <a name="when-to-use-data-in-replication"></a>Ne zaman kullanılacağı Gelen Verileri Çoğaltma
Gelen Verileri Çoğaltma kullanmayı göz önünde bulundurmanız gereken temel senaryolar şunlardır:

- **Karma veri eşitleme:** Gelen Verileri Çoğaltma ile, verileri şirket içi sunucularınız ve MariaDB için Azure veritabanı arasında eşitlenmiş durumda tutabilirsiniz. Bu eşitleme, karma uygulamalar oluşturmak için kullanışlıdır. Bu yöntem, var olan bir yerel veritabanı sunucusuna sahip olduğunuzda ve verileri son kullanıcılara yakın bir bölgeye taşımak istediğinizde çarpıcı olur.
- **Çoklu bulut eşitlemesi:** Karmaşık bulut çözümleri için, bu bulutlarda barındırılan sanal makineler ve veritabanı hizmetleri dahil olmak üzere MariaDB için Azure veritabanı ve farklı bulut sağlayıcıları arasında veri eşitlemesi için Gelen Verileri Çoğaltma kullanın.

## <a name="limitations-and-considerations"></a>Sınırlamalar ve önemli noktalar

### <a name="data-not-replicated"></a>Çoğaltılan veriler
Ana sunucudaki [*MySQL sistem veritabanı*](https://mariadb.com/kb/en/library/the-mysql-database-tables/) çoğaltılmaz. Ana sunucudaki hesaplar ve izinler üzerinde yapılan değişiklikler çoğaltılmaz. Ana sunucuda bir hesap oluşturursanız ve bu hesabın çoğaltma sunucusuna erişmesi gerekiyorsa, çoğaltma sunucusu tarafında el ile aynı hesabı oluşturun. Hangi tabloların sistem veritabanına dahil olduğunu anlamak için, [MariaDB belgelerine](https://mariadb.com/kb/en/library/the-mysql-database-tables/)bakın.

### <a name="requirements"></a>Gereksinimler
- Ana sunucu sürümü en azından MariaDB sürüm 10,2 olmalıdır.
- Ana sunucu ve çoğaltma sunucusu sürümleri aynı olmalıdır. Örneğin, her ikisi de MariaDB sürüm 10,2 olmalıdır.
- Her tablo bir birincil anahtara sahip olmalıdır.
- Ana sunucu InnoDB altyapısını kullanmalıdır.
- Kullanıcının, ikili günlüğü yapılandırma ve ana sunucuda yeni kullanıcılar oluşturma izinlerine sahip olması gerekir.
- Ana sunucuda SSL etkinse, etki alanı için sağlanan SSL CA sertifikasının `mariadb.az_replication_change_master` saklı yordamına eklendiğinden emin olun. Aşağıdaki [örneklere](https://docs.microsoft.com/azure/mariadb/howto-data-in-replication#link-the-master-and-replica-servers-to-start-data-in-replication) ve `master_ssl_ca` parametresine bakın.
- Ana sunucunun IP adresinin MariaDB için Azure Veritabanı çoğaltma sunucusunun güvenlik duvarı kurallarına eklendiğinden emin olun. [Azure portalını](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-portal) veya [Azure CLI](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-cli)’yı kullanarak güvenlik duvarı kurallarını güncelleştirin.
- Ana sunucuyu barındıran makinenin, 3306 numaralı bağlantı noktasında hem gelen hem de giden trafiğe izin verdiğinden emin olun.
- Ana sunucunun **Genel BIR IP adresi**olduğundan, DNS 'nin genel olarak erişilebilir olduğundan veya tam etki alanı adı (FQDN) olduğundan emin olun.

### <a name="other"></a>Diğer
- Veri içi çoğaltma yalnızca Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarında desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
- [Verileri çoğaltmanın nasıl ayarlanacağını](howto-data-in-replication.md)öğrenin.
