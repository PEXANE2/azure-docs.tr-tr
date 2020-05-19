---
title: Azure SQL Edge 'de veri değişikliklerini izleme (Önizleme)
description: Azure SQL Edge 'de değişiklik izleme ve değişiklik verilerini yakalama hakkında bilgi edinin (Önizleme)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: a91ed5d4763aa521c9f6ed913dc532b08e37039f
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597271"
---
# <a name="tracking-data-changes-in-azure-sql-edge-preview"></a>Azure SQL Edge 'de veri değişikliklerini izleme (Önizleme)

Azure SQL Edge, bir veritabanındaki verilerde yapılan değişiklikleri izleyen iki SQL Server özelliğini destekler:[değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#Tracking) ve [veri yakalamayı Değiştir](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#Capture). Bu özellikler, uygulamaların bir veritabanındaki kullanıcı tablolarında yapılan DML değişikliklerini (ekleme, güncelleştirme ve silme işlemleri) belirlemesine olanak sağlar. Değişiklik verilerini yakalama ve değişiklik izleme aynı veritabanında etkinleştirilebilir; özel bir dikkat etmeniz gerekmez.

Veritabanında değiştirilen verileri sorgulama özelliği bazı uygulamaların verimli olması için önemli bir gereksinimdir. Genellikle, veri değişikliklerini öğrenmek için, uygulama geliştiricilerinin Tetikleyiciler, zaman damgası sütunları ve ek tabloların birleşimini kullanarak uygulamalarında özel bir izleme yöntemi uygulaması gerekir. Bu uygulamaların oluşturulması genellikle uygulama için çok fazla iş içerir, şema güncelleştirmelerine yol açar ve genellikle yüksek performanslı bir ek yük taşır. IoT çözümü söz konusu olduğunda, verileri uçtan bir buluta veya veri merkezine düzenli olarak taşımak için değişiklik izleme çok faydalı olabilir. Bu, en son eşitlemeden yalnızca Delta değişikliklerini hızla ve etkili bir şekilde sorgulamak ve bu değişiklikleri buluta veya veri merkezi hedefine yüklemek için kullanılmasına izin verir. Değişiklik İzleme kullanmanın ve veri yakalamalarını değiştirme avantajları hakkında daha fazla bilgi için, [değişiklik verilerini yakalama veya değişiklik izleme kullanma avantajlarına](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#benefits-of-using-change-data-capture-or-change-tracking)bakın. 

Değişiklik İzleme ve değişiklik verilerini yakalama arasındaki özellik farklarını anlamak için, [değişiklik verilerini yakalama ve değişiklik izleme arasındaki özellik farklarını](https://docs.microsoft.com/sql/relational-databases/track-changes/track-data-changes-sql-server#feature-differences-between-change-data-capture-and-change-tracking) inceleyin

## <a name="change-data-capture"></a>Değişiklik Verilerini Yakalama

Değişiklik verilerini yakalama 'nın nasıl çalıştığını anlamak için [değişiklik verilerini yakalama hakkında](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server)bilgi edinin.

Değişiklik verilerini yakalama 'yı etkinleştirmeyi veya devre dışı bırakmayı anlamak için bkz. [değişiklik verilerini yakalamayı etkinleştirme ve devre dışı bırakma](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server)

Değişiklik verilerini yakalama 'yı yönetmek ve izlemek için, [değişiklik verilerini yakalamayı yönetme ve izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server) bölümüne bakın

Değişen verilerle sorgulama ve çalışmayı anlamak için [değişiklik verileriyle çalışma](https://docs.microsoft.com/sql/relational-databases/track-changes/work-with-change-data-sql-server) konusuna bakın

## <a name="change-tracking"></a>Değişiklik İzleme

Değişiklik İzleme nasıl çalıştığına ilişkin ayrıntıları anlamak için [değişiklik izleme hakkında](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server)bilgi edinin.

Değişiklik İzleme etkinleştirmeyi veya devre dışı bırakmayı anlamak için, bkz. [Etkinleştir ve devre dışı bırak değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/enable-and-disable-change-tracking-sql-server)

Değişiklik İzleme yönetmek, izlemek ve yönetmek için, [Yönetim ve izleme değişiklik izleme](https://docs.microsoft.com/sql/relational-databases/track-changes/manage-change-tracking-sql-server) başvurun

Değişen verilerle sorgulama ve çalışmayı anlamak için [değişiklik verileriyle çalışma](https://docs.microsoft.com/sql/relational-databases/track-changes/work-with-change-tracking-sql-server) konusuna bakın

## <a name="temporal-tables"></a>Zamana bağlı tablolar

Azure SQL Edge, Değişiklik İzleme desteklemeye ve SQL Server veri yakalama özelliklerini değiştirmeye ek olarak SQL Server zamana bağlı tablolar özelliğini de destekler. İsteğe bağlı tablolar (sistem sürümü tutulan zamana bağlı tablolar olarak da bilinir), yalnızca geçerli anda doğru olan veriler yerine, herhangi bir zamanda tabloda depolanan veriler hakkında bilgi sağlamak için yerleşik destek sağlayan bir veritabanı özelliği olarak kullanılır.

Sistem sürümü tutulan zamana bağlı bir tablo, veri değişikliklerinin tam geçmişini tutmak ve zaman içinde kolay noktaya izin vermek için tasarlanan bir tür Kullanıcı tablosudur. Her bir satır için geçerlilik süresi sistem tarafından yönetildiğinden (yani, veritabanı altyapısı), bu tür zamana bağlı tablo sistem sürümü tutulan zamana bağlı tablo olarak adlandırılır.

Her zamana bağlı tabloda, datetime2 veri türüne sahip olan iki açıkça tanımlanmış sütun vardır. Bu sütunlar, dönem sütunları olarak adlandırılır. Bu dönem sütunları, her satırda her değişiklik yapıldığında her bir satır için geçerlilik süresini kaydetmek üzere sistem tarafından özel olarak kullanılır.

Bu dönem sütunlarının yanı sıra, zamana bağlı bir tablo, yansıtılan bir şemaya sahip başka bir tablo başvurusu da içerir. Sistem bu tabloyu, zamana bağlı tablodaki her satır güncelleştirildiği veya silindiği zaman satırın önceki sürümünü otomatik olarak depolamak için kullanır. Bu ek tabloya geçmiş tablosu olarak başvurulur, ancak geçerli (gerçek) satır sürümlerini depolayan ana tablo, geçerli tablo olarak veya yalnızca zamana bağlı tablo olarak adlandırılır. Zamana bağlı tablo oluşturma sırasında, kullanıcılar mevcut geçmiş tablosunu belirtebilir (şemaya uyumlu olmalıdır) veya sistem varsayılan geçmiş tablosu oluşturulmasına izin verir.

Zamana bağlı tablolar hakkında daha fazla bilgi için bkz. zamana bağlı [Tablolar](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables)

## <a name="see-also"></a>Ayrıca Bkz.

- [Azure SQL Edge 'de veri akışı (Önizleme)](stream-data.md)
- [Azure SQL Edge 'de ONNX ile makine öğrenimi ve AI (Önizleme)](onnx-overview.md)
- [Azure SQL Edge 'e çoğaltmayı yapılandırma (Önizleme)](configure-replication.md)
- [Azure SQL Edge 'de veritabanlarını yedekleme ve geri yükleme (Önizleme)](backup-restore.md)



