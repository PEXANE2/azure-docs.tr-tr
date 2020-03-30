---
title: Azure SQL Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar ve sınırlamalar yönetilen örnek
description: Azure SQL Veritabanı yönetilen örneğe çevrimiçi geçişlerle ilişkili bilinen sorunlar/geçiş sınırlamaları hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 88e2b5894686ee93caecf33e04940803eb75f394
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77648674"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları yönetilen örnek

SQL Server'dan Azure SQL Veritabanı yönetilen örneğine çevrimiçi geçişlerle ilişkili bilinen sorunlar ve sınırlamalar aşağıda açıklanmıştır.

> [!IMPORTANT]
> SQL Server'ın Azure SQL Veritabanı'na çevrimiçi geçişleri ile SQL_variant veri türlerinin geçişi desteklenmez.

## <a name="backup-requirements"></a>Yedekleme gereksinimleri

- **Checksum'lu yedeklemeler**

    Azure Veritabanı Geçiş Hizmeti, şirket içi veritabanlarınızı SQL Veritabanı yönetilen örneğine geçirmek için yedekleme ve geri yükleme yöntemini kullanır. Azure Veritabanı Geçiş Hizmeti yalnızca checksum kullanılarak oluşturulan yedeklemeleri destekler.

    [Yedekleme veya Geri Yükleme Sırasında Yedekleme Denetimleri Etkinleştirme veya Devre Dışı Kalım (SQL Server)](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?view=sql-server-2017)

    > [!NOTE]
    > Veritabanı yedeklemelerini sıkıştırma ile alırsanız, açıkça devre dışı bırakılmadığı sürece denetim aracı varsayılan bir davranıştır.

    Çevrimdışı geçişlerde, eğer seçerseniz **Azure Veritabanı Geçiş Hizmeti'ne izin verecektir...** sonra Azure Veritabanı Geçiş Hizmeti, denetimler seçeneği etkinleştirilmiş veritabanı yedeklemesini alır.

- **Yedekleme ortamı**

    Her yedeklemeyi ayrı bir yedekleme ortamına (yedekleme dosyaları) aldığından emin olun. Azure Veritabanı Geçiş Hizmeti, tek bir yedekleme dosyasına eklenen yedeklemeleri desteklemez. Yedekleme dosyalarını ayırmak için tam yedekleme alın ve yedeklemeleri günlüğe kaydedin.

## <a name="data-and-log-file-layout"></a>Veri ve günlük dosya düzeni

- **Günlük dosyalarının sayısı**

    Azure Veritabanı Geçiş Hizmeti, birden çok günlük dosyası olan veritabanlarını desteklemez. Birden çok günlük dosyanız varsa, bunları tek bir işlem günlüğü dosyasında küçültün ve yeniden düzenleyin. Boş olmayan dosyaları günlüğe kaydetmek için uzaktan kumanda edemediğinden, önce günlük dosyasını yedeklemeniz gerekir.

## <a name="sql-server-features"></a>SQL Server özellikleri

- **FileStream/FileTables**

    SQL Veritabanı yönetilen örnek şu anda FileStream ve FileTables desteklemiyor. Bu özelliklere bağlı iş yükleri için Azure VM'lerinde çalışan SQL Sunucularını Azure hedefiniz olarak seçmenizi öneririz.

- **Bellek tabloları**

    In-memory OLTP, SQL Veritabanı yönetilen örnek için Premium ve İş Kritik katmanlarında kullanılabilir; Genel Amaç katmanı bellek içi OLTP'yi desteklemez.

## <a name="migration-resets"></a>Geçiş sıfırlamaları

- **Dağıtımlar**

    SQL Veritabanı yönetilen örnek otomatik yama ve sürüm güncellemeleri ile bir PaaS hizmetidir. SQL Veritabanı yönetilen örneğinin geçişi sırasında, kritik olmayan güncelleştirmeler 36 saate kadar yardımcı olur. Daha sonra (ve kritik güncelleştirmeler için), geçiş kesintiye uğrarsa, işlem tam bir geri yükleme durumuna sıfırlanır.

    Geçiş kesme yalnızca tam yedekleme geri yüklendikten ve tüm günlük yedeklemelerini yakaladıktan sonra çağrılabilir. Üretim geçişi kesintileriniz etkileniyorsa, [Azure DMS Geri Bildirim takma adı ile](mailto:dmsfeedback@microsoft.com)iletişime geçin.
