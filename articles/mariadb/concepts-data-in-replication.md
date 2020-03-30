---
title: Veri çoğaltma - MariaDB için Azure Veritabanı
description: Harici bir sunucudan MariaDB hizmeti için Azure Veritabanı'na eşitlemek için veri çoğaltma yı kullanma hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 1fbcc1fb27d5e6df4641f79c0d634580f74000b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532069"
---
# <a name="replicate-data-into-azure-database-for-mariadb"></a>MariaDB için verileri Azure Veritabanında çoğaltma

Gelen Verileri Çoğaltma, şirket içinde, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetlerinde çalışan bir MariaDB sunucusundan, MariaDB için Azure Veritabanı hizmetine verileri eşitlemenize olanak sağlar. Gelen Verileri Çoğaltma, MariaDB’de yerel olan ikili günlük (binlog) dosya konumuna dayalı çoğaltmayı temel alır. Binlog çoğaltma hakkında daha fazla bilgi edinmek için [binlog çoğaltma genel bakış](https://mariadb.com/kb/en/library/replication-overview/)bakın.

## <a name="when-to-use-data-in-replication"></a>Data-in Çoğaltma ne zaman kullanılır?
Data-in Çoğaltma'yı kullanmayı göz önünde bulundurmak gereken ana senaryolar şunlardır:

- **Karma Veri Senkronizasyonu:** Data-in Replication ile, verileri şirket içi sunucularınız ve MariaDB için Azure Veritabanı arasında eşitlenmiş olarak tutabilirsiniz. Bu eşitleme, karma uygulamalar oluşturmak için yararlıdır. Bu yöntem, varolan bir yerel veritabanı sunucunuz olduğunda, ancak verileri son kullanıcılara daha yakın bir bölgeye taşımak istediğinizde çekicidir.
- **Çoklu Bulut Senkronizasyonu:** Karmaşık bulut çözümleri için, MariaDB için Azure Veritabanı ile bu bulutlarda barındırılan sanal makineler ve veritabanı hizmetleri de dahil olmak üzere farklı bulut sağlayıcıları arasındaki verileri eşitlemek için Veri Çoğaltma'yı kullanın.

## <a name="limitations-and-considerations"></a>Sınırlamalar ve önemli noktalar

### <a name="data-not-replicated"></a>Veri çoğaltılmama
Ana sunucudaki [*mysql sistem veritabanı*](https://mariadb.com/kb/en/library/the-mysql-database-tables/) çoğaltılamaz. Ana sunucudaki hesaplarda ve izinlerde yapılan değişiklikler çoğaltılamaz. Ana sunucuda bir hesap oluşturursanız ve bu hesabın çoğaltma sunucusuna erişmesi gerekiyorsa, aynı hesabı çoğaltma sunucusu tarafında el ile oluşturun. Sistem veritabanında hangi tabloların bulunduğunu anlamak için [MariaDB belgelerine](https://mariadb.com/kb/en/library/the-mysql-database-tables/)bakın.

### <a name="requirements"></a>Gereksinimler
- Ana sunucu sürümü en az MariaDB sürüm 10.2 olmalıdır.
- Ana ve çoğaltma sunucusu sürümleri aynı olmalıdır. Örneğin, her ikisi de MariaDB sürüm 10.2 olmalıdır.
- Her tablonun birincil anahtarı olmalıdır.
- Ana sunucu InnoDB altyapısını kullanmalıdır.
- Kullanıcı, ikili günlüğe kaydetmeyi yapılandırmak ve ana sunucuda yeni kullanıcılar oluşturmak için izinlere sahip olmalıdır.
- Ana sunucusa SSL etkinleştirilmişse, etki alanı için sağlanan SSL `mariadb.az_replication_change_master` CA sertifikasının depolanan yordama dahil edildiğinden emin olun. Aşağıdaki [örneklere](https://docs.microsoft.com/azure/mariadb/howto-data-in-replication#link-the-master-and-replica-servers-to-start-data-in-replication) ve `master_ssl_ca` parametreye bakın.
- Ana sunucunun IP adresinin MariaDB için Azure Veritabanı çoğaltma sunucusunun güvenlik duvarı kurallarına eklendiğinden emin olun. [Azure portalını](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-portal) veya [Azure CLI](https://docs.microsoft.com/azure/mariadb/howto-manage-firewall-cli)’yı kullanarak güvenlik duvarı kurallarını güncelleştirin.
- Ana sunucuyu barındıran makinenin, 3306 numaralı bağlantı noktasında hem gelen hem de giden trafiğe izin verdiğinden emin olun.
- Ana sunucunun genel bir **IP adresine**sahip olduğundan, DNS'nin herkese açık olduğundan veya tam nitelikli bir etki alanı adı (FQDN) olduğundan emin olun.

### <a name="other"></a>Diğer
- Veri çoğaltma yalnızca Genel Amaç ve Bellek Optimize Edilmiş fiyatlandırma katmanlarında desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
- Veri çoğaltmayı nasıl [ayarlayatıyarı nızı](howto-data-in-replication.md)öğrenin.
