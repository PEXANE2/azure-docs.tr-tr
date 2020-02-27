---
title: Azure SQL veritabanı yönetilen örneği ile çevrimiçi geçişlerle ilgili bilinen sorunlar ve sınırlamalar
description: Azure SQL veritabanı yönetilen örneği 'ne çevrimiçi geçişlerle ilişkili bilinen sorunlar/geçiş sınırlamaları hakkında bilgi edinin.
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
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77648674"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği 'ne çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları

SQL Server ile Azure SQL veritabanı yönetilen örneği arasındaki çevrimiçi geçişlerle ilişkili bilinen sorunlar ve sınırlamalar aşağıda açıklanmaktadır.

> [!IMPORTANT]
> Azure SQL veritabanı 'na SQL Server çevrimiçi geçişlerde SQL_variant veri türlerinin geçirilmesi desteklenmez.

## <a name="backup-requirements"></a>Yedekleme gereksinimleri

- **Sağlama toplamı olan yedeklemeler**

    Azure veritabanı geçiş hizmeti, şirket içi veritabanlarınızı SQL veritabanı yönetilen örneğine geçirmek için yedekleme ve geri yükleme yöntemini kullanır. Azure veritabanı geçiş hizmeti yalnızca sağlama toplamı kullanılarak oluşturulan yedeklemeleri destekler.

    [Yedekleme veya geri yükleme sırasında yedekleme sağlama toplamlarını etkinleştirme veya devre dışı bırakma (SQL Server)](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?view=sql-server-2017)

    > [!NOTE]
    > Veritabanı yedeklerini sıkıştırma ile alırsanız, açıkça devre dışı bırakılmadığı takdirde sağlama toplamı varsayılan davranıştır.

    Çevrimdışı geçişlerde, **Azure veritabanı geçiş hizmeti 'ne izin vereceğim...** ' yi seçerseniz, Azure veritabanı geçiş hizmeti, veritabanı yedeklemesini sağlama toplamı seçeneği etkin olacak şekilde alır.

- **Yedekleme medyası**

    Her yedeklemeyi ayrı bir yedekleme ortamında (yedekleme dosyaları) aldığınızdan emin olun. Azure veritabanı geçiş hizmeti, tek bir yedekleme dosyasına eklenen yedeklemeleri desteklemez. Yedekleme dosyalarını ayırmak için tam yedekleme ve günlük yedeklemeleri yapın.

## <a name="data-and-log-file-layout"></a>Veri ve günlük dosyası düzeni

- **Günlük dosyası sayısı**

    Azure veritabanı geçiş hizmeti birden çok günlük dosyası olan veritabanlarını desteklemez. Birden çok günlük dosyanız varsa, bunları daraltın ve tek bir işlem günlük dosyasında yeniden düzenleyin. Boş olmayan dosyaları günlüğe uzak olmadığınızdan, önce günlük dosyasını yedeklemeniz gerekir.

## <a name="sql-server-features"></a>SQL Server özellikleri

- **FILESTREAM/dosya tabloları**

    SQL veritabanı yönetilen örneği şu anda FILESTREAM ve FileTables 'ı desteklemiyor. Bu özelliklere bağımlı olan iş yükleri için Azure hedefi olarak Azure VM 'lerinde çalışan SQL Server 'Lar için tercih etmenizi öneririz.

- **Bellek içi tablolar**

    Bellek içi OLTP, SQL veritabanı yönetilen örneği için Premium ve İş Açısından Kritik katmanlarında kullanılabilir; Genel Amaçlı katmanı bellek Içi OLTP 'Yi desteklemez.

## <a name="migration-resets"></a>Geçiş sıfırlamaları

- **Dağıtımlar**

    SQL veritabanı yönetilen örneği, otomatik düzeltme eki uygulama ve sürüm güncelleştirmeleri içeren bir PaaS hizmetidir. SQL veritabanı yönetilen örneğinizin geçirilmesi sırasında kritik olmayan güncelleştirmeler 36 saate kadar yardımcı olur. Daha sonra (ve kritik güncelleştirmeler için), geçiş kesintiye uğratılıyor ise işlem tam geri yükleme durumuna sıfırlanır.

    Tam geçişi geçişi yalnızca tam yedekleme geri yüklendikten sonra çağrılabilir ve tüm günlük yedeklemeleriyle yapılır. Üretim geçişiniz etkileniyorsa [Azure DMS geri bildirim diğer adı](mailto:dmsfeedback@microsoft.com)ile iletişime geçin.
