---
title: Azure Kaynak taşıyıcısı ile bölgeler arasında Azure SQL kaynaklarını taşıma desteği.
description: Azure Kaynak taşıyıcısı ile bölgeler arasında Azure SQL kaynaklarını taşıma desteğini gözden geçirin.
author: rayne-wiselman
manager: evansma
ms.service: resource-move
ms.topic: conceptual
ms.date: 09/07/2020
ms.author: raynew
ms.openlocfilehash: 4925f6ffd2383c21f8ff9b0e3196d44fc15bb657
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653001"
---
# <a name="support-for-moving-azure-sql-resources-between-azure-regions"></a>Azure bölgeleri arasında Azure SQL kaynaklarını taşıma desteği

Bu makale Azure Kaynak taşıyıcısı ile Azure bölgeleri arasında Azure SQL kaynaklarını taşımaya yönelik destek ve önkoşulları özetler.

## <a name="requirements"></a>Gereksinimler

Gereksinimler aşağıdaki tabloda özetlenmiştir.

**Özellik** | **Desteklenir/desteklenmez** | **Ayrıntılar**
--- | --- | ---
**Azure SQL veritabanı hiper ölçek** | Desteklenmez | Kaynak taşıyıcısı ile Azure SQL hiper ölçek hizmeti katmanında veritabanları taşınamaz.
**Bölge yedekliliği** | Desteklenir |  Desteklenen taşıma seçenekleri:<br/><br/> -Bölge artıklığı destekleyen bölgeler arasında.<br/><br/> -Bölge yedekliliği desteklemeyen bölgeler arasında.<br/><br/> -Bölge yedekliliği destekleyen bölge arasında bölge yedekliliği desteklemeyen bir bölge arasında.<br/><br/> -Bölge yedekliliği desteklemeyen bir bölge arasında bölge artıklığı destekleyen bir bölge arasında. 
**Veri eşitleme** | Merkez/eşitleme veritabanı: desteklenmiyor<br/><br/> Eşitleme üyesi: destekleniyor. | Bir eşitleme üyesi taşınmışsa, yeni hedef veritabanına veri eşitlemesi ayarlamanız gerekir.
**Mevcut coğrafi çoğaltma** | Desteklenir | Mevcut coğrafi çoğaltmalar, hedef bölgedeki yeni birincil ile eşleştirilir.<br/><br/> Taşıma işleminden sonra dengeli dağıtım başlatılmalıdır. [Daha fazla bilgi edinin](/azure/sql-database/sql-database-active-geo-replication-portal)
**Kendi Anahtarını Getir (BYOK) ile Saydam Veri Şifrelemesi (TDE)** | Desteklenir | Anahtar kasalarını bölgeler arasında taşıma hakkında [daha fazla bilgi edinin](../key-vault/general/move-region.md) .
**Hizmet tarafından yönetilen anahtarla TDE** | Destekleniyor. |  Anahtar kasalarını bölgeler arasında taşıma hakkında [daha fazla bilgi edinin](../key-vault/general/move-region.md) .
**Dinamik veri maskeleme kuralları** | Destekleniyor. | Kurallar, taşımanın bir parçası olarak hedef bölgeye otomatik olarak kopyalanır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started-portal).
**Gelişmiş veri güvenliği** | Desteklenmez. | Geçici çözüm: hedef bölgedeki SQL Server düzeyinde ayarlanır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-advanced-data-security).
**Güvenlik duvarı kuralları** | Desteklenmez. | Geçici çözüm: hedef bölgede SQL Server için güvenlik duvarı kuralları ayarlayın. Veritabanı düzeyinde güvenlik duvarı kuralları, kaynak sunucudan hedef sunucuya kopyalanır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-server-level-firewall-rule).
**Denetim ilkeleri** | Desteklenmez. | Taşıma işleminden sonra ilkeler varsayılana sıfırlanır. Sıfırlamayı [öğrenin](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) .
**Yedekleme dosyası saklama** | Destekleniyor. | Kaynak veritabanının yedekleme bekletme ilkeleri hedef veritabanına taşınır. Taşıma işleminden sonra ayarları değiştirmeyi [öğrenin](/azure/sql-database/sql-database-long-term-backup-retention-configure) .
**Otomatik ayarlama** | Desteklenmez. | Geçici çözüm: taşıdıktan sonra otomatik ayarlama ayarlarını ayarlayın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-automatic-tuning-enable).
**Veritabanı uyarıları** | Desteklenmez. | Geçici çözüm: taşıdıktan sonra uyarıları ayarlayın. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-insights-alerts-portal).
**Azure SQL Server Esnetme Veritabanı** | Desteklenmiyor | SQL Server Esnetme veritabanlarını kaynak taşıyıcısı ile taşıyamıyor.
**Azure Synapse Analytics** | Desteklenmiyor | SYNAPSE Analytics (eski adıyla Azure SQL veri ambarı) kaynak taşıyıcısı ile taşınamaz.
## <a name="next-steps"></a>Sonraki adımlar

[Azure SQL kaynaklarını](tutorial-move-region-sql.md) kaynak taşıyıcısı ile başka bir bölgeye deneyin.