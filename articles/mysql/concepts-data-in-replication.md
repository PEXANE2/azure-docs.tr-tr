---
title: Veri çoğaltma - MySQL için Azure Veritabanı
description: Harici bir sunucudan MySQL hizmeti için Azure Veritabanı'na eşitlemek için veri çoğaltma yı kullanma hakkında bilgi edinin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 20be34191355e6ade40e0f3b218818bfa5345a28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79533241"
---
# <a name="replicate-data-into-azure-database-for-mysql"></a>MySQL için verileri Azure Veritabanı'nda çoğaltma

Data-in Replication, harici bir MySQL sunucusundaki verileri MySQL hizmeti için Azure Veritabanına senkronize etmenizi sağlar. Harici sunucu, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan bir veritabanı hizmetinde şirket içinde olabilir. Gelen Verileri Çoğaltma, MySQL’de yerel olan ikili günlük (binlog) dosya konumuna dayalı çoğaltmayı temel alır. Binlog çoğaltma hakkında daha fazla bilgi edinmek için [MySQL binlog çoğaltma genel bakış](https://dev.mysql.com/doc/refman/5.7/en/binlog-replication-configuration-overview.html)bakın. 

## <a name="when-to-use-data-in-replication"></a>Data-in Çoğaltma ne zaman kullanılır?
Data-in Çoğaltma'yı kullanmayı göz önünde bulundurmak gereken ana senaryolar şunlardır:

- **Karma Veri Senkronizasyonu:** Data-in Replication ile, verileri şirket içi sunucularınız ve MySQL için Azure Veritabanı arasında eşitlenmiş olarak tutabilirsiniz. Bu eşitleme, karma uygulamalar oluşturmak için yararlıdır. Bu yöntem, varolan bir yerel veritabanı sunucunuz olduğunda ancak verileri son kullanıcılara daha yakın bir bölgeye taşımak istediğinizde çekicidir.
- **Çoklu Bulut Senkronizasyonu:** Karmaşık bulut çözümleri için, MySQL için Azure Veritabanı ile bu bulutlarda barındırılan sanal makineler ve veritabanı hizmetleri de dahil olmak üzere farklı bulut sağlayıcıları arasındaki verileri eşitlemek için Veri Çoğaltma'yı kullanın.
 
Geçiş senaryoları için [Azure Veritabanı Geçiş Hizmeti'ni](https://azure.microsoft.com/services/database-migration/)(DMS) kullanın.

## <a name="limitations-and-considerations"></a>Sınırlamalar ve önemli noktalar

### <a name="data-not-replicated"></a>Veri çoğaltılmama
Ana sunucudaki [*mysql sistem veritabanı*](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html) çoğaltılamaz. Ana sunucudaki hesaplarda ve izinlerde yapılan değişiklikler çoğaltılamaz. Ana sunucuda bir hesap oluşturursanız ve bu hesabın çoğaltma sunucusuna erişmesi gerekiyorsa, aynı hesabı çoğaltma sunucusu tarafında el ile oluşturun. Sistem veritabanında hangi tabloların bulunduğunu anlamak için [MySQL kılavuzuna](https://dev.mysql.com/doc/refman/5.7/en/system-schema.html)bakın.

### <a name="requirements"></a>Gereksinimler
- Ana sunucu sürümü en az MySQL sürüm 5.6 olmalıdır. 
- Ana ve çoğaltma sunucusu sürümleri aynı olmalıdır. Örneğin, her ikisi de MySQL sürüm 5.6 veya her ikisi de MySQL sürüm 5.7 olmalıdır.
- Her tablonun birincil anahtarı olmalıdır.
- Ana sunucu MySQL InnoDB altyapısını kullanmalıdır.
- Kullanıcı, ikili günlüğe kaydetmeyi yapılandırmak ve ana sunucuda yeni kullanıcılar oluşturmak için izinlere sahip olmalıdır.
- Ana sunucusa SSL etkinleştirilmişse, etki alanı için sağlanan SSL `mysql.az_replication_change_master` CA sertifikasının depolanan yordama dahil edildiğinden emin olun. Aşağıdaki [örneklere](https://docs.microsoft.com/azure/mysql/howto-data-in-replication#link-master-and-replica-servers-to-start-data-in-replication) ve `master_ssl_ca` parametreye bakın.
- Ana sunucunun IP adresinin MySQL için Azure Veritabanı çoğaltma sunucusunun güvenlik duvarı kurallarına eklendiğinden emin olun. [Azure portalını](https://docs.microsoft.com/azure/mysql/howto-manage-firewall-using-portal) veya [Azure CLI](https://docs.microsoft.com/azure/mysql/howto-manage-firewall-using-cli)’yı kullanarak güvenlik duvarı kurallarını güncelleştirin.
- Ana sunucuyu barındıran makinenin, 3306 numaralı bağlantı noktasında hem gelen hem de giden trafiğe izin verdiğinden emin olun.
- Ana sunucunun genel bir **IP adresine**sahip olduğundan, DNS'nin herkese açık olduğundan veya tam nitelikli bir etki alanı adı (FQDN) olduğundan emin olun.

### <a name="other"></a>Diğer
- Veri çoğaltma yalnızca Genel Amaç ve Bellek Optimize Edilmiş fiyatlandırma katmanlarında desteklenir.
- Genel işlem tanımlayıcıları (GTID) desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar
- Veri çoğaltmayı nasıl [ayarlayatıyarı nızı](howto-data-in-replication.md) öğrenin
- Okuma [yinelemeleriyle Azure'da çoğaltma](concepts-read-replicas.md) hakkında bilgi edinin
- [DMS kullanarak en az kapalı kalma süresiyle verileri nasıl geçirtin izleyin](howto-migrate-online.md)