---
title: Azure SQL Edge 'de veri değişikliklerini izleme (Önizleme)
description: Azure SQL Edge 'de değişiklik izleme ve değişiklik verilerini yakalama hakkında bilgi edinin (Önizleme).
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 6d0a081f2b0adb143a6b37a647a00014846f8fe2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84669605"
---
# <a name="track-data-changes-in-azure-sql-edge-preview"></a>Azure SQL Edge 'de veri değişikliklerini izleme (Önizleme)

Azure SQL Edge, bir veritabanındaki verilerde yapılan değişiklikleri izleyen iki SQL Server özelliğini destekler: [değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#Tracking) ve [değişiklik verilerini yakalama](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#Capture). Bu özellikler, uygulamaların bir veritabanındaki kullanıcı tablolarında yapılan veri değiştirme dili değişikliklerini (ekleme, güncelleştirme ve silme işlemleri) belirlemesine olanak sağlar. Değişiklik verilerini yakalama ve değişiklik izlemeyi aynı veritabanında etkinleştirebilirsiniz. Özel bir dikkat etmeniz gerekmez.

Veritabanında değiştirilen verileri sorgulama özelliği bazı uygulamaların verimli olması için önemli bir gereksinimdir. Genellikle, veri değişikliklerini öğrenmek için, uygulama geliştiricilerinin Tetikleyiciler, zaman damgası sütunları ve ek tabloların birleşimini kullanarak uygulamalarında özel bir izleme yöntemi uygulaması gerekir. Bu uygulamaların oluşturulması genellikle uygulama için çok fazla iş içerir, şema güncelleştirmelerine yol açar ve genellikle yüksek performanslı bir ek yük taşır.

Düzenli aralıklarla verileri uçtan buluta veya veri merkezine taşımanız gereken bir IoT çözümü olması durumunda değişiklik izleme çok faydalı olabilir. Kullanıcılar yalnızca son eşitlemeden değişiklikleri hızla ve etkili bir şekilde sorgulayabilir ve bu değişiklikleri buluta veya veri merkezi hedefine yükleyebilir. Daha fazla ayrıntı için bkz. [değişiklik verilerini yakalama veya değişiklik izlemeyi kullanmanın avantajları](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#benefits-of-using-change-data-capture-or-change-tracking). 

Bu iki özellik aynı değildir. Daha fazla bilgi için bkz. [değişiklik verilerini yakalama ve değişiklik izleme arasındaki özellik farklılıkları](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#feature-differences-between-change-data-capture-and-change-tracking)

## <a name="change-data-capture"></a>Değişiklik verilerini yakalama

Bu özelliğin nasıl çalıştığına ilişkin ayrıntıları öğrenmek için bkz. [değişiklik verilerini yakalama hakkında](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server).

Bu özelliğin nasıl etkinleştirileceğini veya devre dışı bırakılacağını öğrenmek için bkz. [değişiklik verilerini yakalamayı etkinleştirme ve devre dışı bırakma](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server).

Bu özelliği yönetmek ve izlemek için bkz. [değişiklik verilerini yakalamayı yönetme ve izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server).

Değişen verilerle sorgulama ve çalışmayı anlamak için bkz. [değişiklik verileriyle çalışma](https://docs.microsoft.com/sql/relational-databases/track-changes/work-with-change-data-sql-server).

## <a name="change-tracking"></a>Change tracking

Bu özelliğin nasıl çalıştığına ilişkin ayrıntıları anlamak için bkz. [değişiklik Izleme hakkında](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server).

Bu özelliğin nasıl etkinleştirileceğini veya devre dışı bırakılacağını öğrenmek için bkz. [değişiklik Izlemeyi etkinleştirme ve devre dışı bırakma](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server).

Bu özelliği yönetmek, izlemek ve yönetmek için bkz. [değişiklik Izlemeyi yönetme ve izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/manage-change-tracking-sql-server).

Değişen verilerle sorgulama ve çalışmayı anlamak için bkz. [değişiklik verileriyle çalışma](https://docs.microsoft.com/sql/relational-databases/track-changes/work-with-change-tracking-sql-server).

## <a name="temporal-tables"></a>Zamana bağlı tablolar

Azure SQL Edge, SQL Server zamana bağlı tablolar özelliğini de destekler. Bu Özellik ( *sistem sürümü tutulan*zamana bağlı tablolar olarak da bilinir), her zaman bir noktada tabloda depolanan veriler hakkında bilgi sağlamak için yerleşik destek sunar. Özelliği yalnızca geçerli anda doğru olan veriler hakkında bilgi sağlamaz.

Sistem sürümü tutulan zamana bağlı bir tablo, veri değişikliklerinin tam geçmişini tutmak ve kolay bir noktadan noktaya Analize izin vermek için tasarlanan bir tür Kullanıcı tablosudur. Her bir satırın geçerlilik süresi sistem tarafından yönetildiği için (yani, veritabanı altyapısı), bu tür bir zamana bağlı tablo, sistem sürümlü zamana bağlı bir tablo olarak adlandırılır.

Her zamana bağlı tabloda, her biri veri türüne sahip iki açıkça tanımlanmış sütun vardır `datetime2` . Bu sütunlar, *Dönem* sütunları olarak adlandırılır. Sistem bu dönem sütunlarını, her satır değiştirildiğinde her bir satırın geçerlilik süresini kaydetmek için özel olarak kullanır.

Bu dönem sütunlarının yanı sıra, zamana bağlı bir tablo, yansıtılan bir şemaya sahip başka bir tablo başvurusu da içerir. Sistem bu tabloyu, zamana bağlı tablodaki her satır güncelleştirildiği veya silindiği zaman satırın önceki sürümünü otomatik olarak depolamak için kullanır. Bu ek tabloya *Geçmiş* tablosu olarak başvurulur, ancak geçerli (gerçek) satır sürümlerini depolayan ana tablo, *geçerli* tablo olarak veya yalnızca zamana bağlı tablo olarak adlandırılır. Zamana bağlı tablo oluşturma sırasında, kullanıcılar mevcut bir geçmiş tablosu belirtebilir (şemayla uyumlu olmalıdır) veya sistemin varsayılan geçmiş tablosunu oluşturmasını sağlayabilirsiniz.

Daha fazla bilgi için bkz. zamana bağlı [Tablolar](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL Edge 'de veri akışı (Önizleme)](stream-data.md)
- [Azure SQL Edge 'de ONNX ile makine öğrenimi ve AI (Önizleme)](onnx-overview.md)
- [Azure SQL Edge 'e çoğaltmayı yapılandırma (Önizleme)](configure-replication.md)
- [Azure SQL Edge 'de veritabanlarını yedekleme ve geri yükleme (Önizleme)](backup-restore.md)



