---
title: SQL Server veritabanıtek/havuzlu bir veritabanına geçiş
description: SQL Server veritabanının Azure SQL Veritabanı'nda tek bir veritabanına veya esnek bir havuza geçişi hakkında bilgi edinin.
keywords: veritabanı geçişi,sql server veritabanı geçişi,veritabanı taşıma araçları,veritabanı taşıma,sql veritabanı geçişi
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 02/11/2019
ms.openlocfilehash: 9cec91ccc80b9072b1a3da756f26f47eb88b951c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268619"
---
# <a name="sql-server-database-migration-to-azure-sql-database"></a>SQL Server veritabanının Azure SQL Veritabanına geçişi

Bu makalede, BIR SQL Server 2005 veya daha sonraki bir veritabanını Azure SQL Veritabanı'nda tek veya havuzlu bir veritabanına geçirmenin birincil yöntemleri hakkında bilgi edinin. Yönetilen Bir Örneğe geçiş hakkında bilgi [için](sql-database-managed-instance-migrate.md)bkz. Diğer platformlardan geçiş hakkında geçiş bilgileri için [Azure Veritabanı Geçiş Kılavuzu'na](https://datamigration.microsoft.com/)bakın.

## <a name="migrate-to-a-single-database-or-a-pooled-database"></a>Tek bir veritabanına veya havuza girilen veritabanına geçiş

SQL Server 2005 veya daha sonra bir veritabanını Azure SQL Veritabanı'nda tek veya havuzlanmış bir veritabanına geçirmek için iki birincil yöntem vardır. İlk yöntem basittir, ancak geçiş sırasında önemli olabilecek bazı kapalı kalma sürelerine neden olabilir. İkinci yöntem daha karmaşık olmasına karşın, geçiş sırasında kapalı kalma süresini önemli ölçüde ortadan kaldırır.

Her iki durumda da, [veri aktarış yardımcısını (DMA)](https://www.microsoft.com/download/details.aspx?id=53595)kullanarak kaynak veritabanının Azure SQL Veritabanı ile uyumlu olduğundan emin olmanız gerekir. SQL Database V12, sunucu düzeyi ve veritabanı lar arası işlemlerle ilgili sorunlar dışında SQL Server ile [özellik eşitliğine](sql-database-features.md) yaklaşıyor. [Kısmen desteklenen veya desteklenmeyen işlevleri](sql-database-transact-sql-information.md) kullanan veritabanları ve uygulamalar için SQL Server veritabanının geçirilebilmesi için [bu uyumsuzlukların giderilmesi amacıyla yeniden mühendislik](sql-database-single-database-migrate.md#resolving-database-migration-compatibility-issues) işlemlerinin yapılması gerekir.

> [!NOTE]
> Microsoft Access, Sybase, MySQL Oracle ve DB2 olmak üzere SQL Server harici veritabanlarını Azure SQL Veritabanına geçirmek için bkz. [SQL Server Geçiş Yardımcısı](https://blogs.msdn.microsoft.com/datamigration/2017/09/29/release-sql-server-migration-assistant-ssma-v7-6/).

## <a name="method-1-migration-with-downtime-during-the-migration"></a>Yöntem 1: Geçiş sırasında kapalı kalma süresi ile geçiş

 Biraz kapalı kalma süresini karşılayamıyorsanız veya daha sonraki geçişler için bir üretim veritabanısına sınanma geçişi gerçekleştiriyorsanız, tek bir veya havuzlanmış veritabanına geçiş yapmak için bu yöntemi kullanın. Öğretici için [bkz.](../dms/tutorial-sql-server-to-azure-sql.md)

Aşağıdaki liste, bu yöntemi kullanarak tek veya havuzlu bir veritabanının SQL Server veritabanı geçişi için genel iş akışını içerir. Yönetilen Örnek'e geçiş için yönetilen [örne geçiş](sql-database-managed-instance-migrate.md)bakın.

  ![VSSSDT geçiş şeması](./media/sql-database-cloud-migrate/azure-sql-migration-sql-db.png)

1. [Veri Geçişi Yardımcısının (DMA)](https://www.microsoft.com/download/details.aspx?id=53595)en son sürümünü kullanarak veritabanını uyumluluk açısından [değerlendirin.](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
2. Transact-SQL betikleri halinde tüm gerekli düzeltmeleri hazırlayın.
3. Geçirilmekte olan kaynak veritabanının işlem açısından tutarlı bir kopyasını yapın veya geçiş gerçekleşirken kaynak veritabanında yeni hareketlerin oluşmasını durdurun. Bu ikinci seçeneği gerçekleştirmek için yöntemler istemci bağlantısı devre dışı bırakma veya bir [veritabanı anlık oluşturma](https://msdn.microsoft.com/library/ms175876.aspx)içerir. Geçişten sonra, geçiş yapan veritabanlarını geçiş için kesme noktasından sonra oluşan değişikliklerle güncelleştirmek için işlem çoğaltmasını kullanabilirsiniz. Bkz. [İşlemsel Geçiş'i kullanarak Geçir.](sql-database-single-database-migrate.md#method-2-use-transactional-replication)  
4. Düzeltmeleri veritabanı kopyasına uygulamak için Transact-SQL betiklerini dağıtın.
5. Veri Geçişi Yardımcısı'nı kullanarak veritabanı kopyasını yeni bir Azure SQL Veritabanına [geçirin.](https://docs.microsoft.com/sql/dma/dma-migrateonpremsql)

> [!NOTE]
> DMA kullanmak yerine, bir BACPAC dosyası da kullanabilirsiniz. Bkz. [BACPAC dosyasını yeni bir Azure SQL Veritabanına Aktar.](sql-database-import.md)

### <a name="optimizing-data-transfer-performance-during-migration"></a>Geçiş sırasında veri aktarımı performansını en iyi duruma getirme

Aşağıdaki liste, içeri aktarma işlemi sırasında en iyi performans için öneriler içerir.

- Bütçenizin aktarım performansını en üst düzeye çıkarmasına izin verdiği en yüksek hizmet katmanını ve işlem boyutunu seçin. Geçiş tamamlandıktan sonra paradan tasarruf etmek için ölçeği azaltabilirsiniz.
- BACPAC dosyanızla hedef veri merkezi arasındaki mesafeyi en aza indirin.
- Geçiş sırasında otomatik istatistikleri devre dışı bırakma
- Tabloları ve dizinleri bölümleme
- Dizini oluşturulmuş görünümleri bırakma ve tamamlandıktan sonra yeniden oluşturma
- Nadiren sorgulanan geçmiş verileri başka bir veritabanına kaldırın ve bu geçmiş verileri ayrı bir Azure SQL veritabanına geçirin. Daha sonra bu geçmiş verileri [esnek sorgular](sql-database-elastic-query-overview.md) kullanarak sorgulayabilirsiniz.

### <a name="optimize-performance-after-the-migration-completes"></a>Geçiş tamamlandıktan sonra performansı en iyi duruma getirme

Geçiş tamamlandıktan sonra tam tarama ile [istatistikleri güncelleştirin](https://docs.microsoft.com/sql/t-sql/statements/update-statistics-transact-sql).

## <a name="method-2-use-transactional-replication"></a>Yöntem 2: İşlem Çoğaltma Kullanma

Geçiş gerçekleşirken SQL Server veritabanınızı üretimden kaldırmak kabul edilebilir bir durum değilse, geçiş çözümü olarak SQL Server işlem çoğaltmayı kullanabilirsiniz. Bu yöntemi kullanmak için, kaynak veritabanının [işlem çoğaltma gereksinimlerini](https://msdn.microsoft.com/library/mt589530.aspx) karşılaması ve Azure SQL Veritabanı ile uyumlu olması gerekir. Always On ile SQL çoğaltma hakkında daha fazla bilgi için, [Her Zaman Kullanılabilirlik Grupları (SQL Server) için Çoğaltma Yıkıntısıya](/sql/database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server)bakın.

Bu çözümü kullanmak için, Azure SQL Veritabanınızı, geçirmek istediğiniz SQL Server örneğine abone olacak şekilde yapılandırabilirsiniz. Yeni işlemler gerçekleşmeye devam ederken, işlem çoğaltma dağıtıcısı, veritabanındaki eşitlenecek verileri eşitler (yayımcı).

İşlem çoğaltma ile verilerinizde veya şemanıza yapılan tüm değişiklikler Azure SQL Veritabanınızda gösterilir. Eşitleme tamamlandıktan sonra geçişe hazır olduğunuzda, uygulamalarınızın bağlantı dizesini Azure SQL Veritabanınıza işaret edecek şekilde değiştirin. İşlem çoğaltma özelliği kaynak veritabanınızda kalan tüm değişiklikleri boşalttığında ve tüm uygulamalarınız Azure DB’yi işaret ettiğinde, işlem çoğaltma özelliğini kaldırabilirsiniz. Azure SQL Veritabanınız artık üretim sisteminizdir.

 ![SeedCloudTR diyagramı](./media/sql-database-cloud-migrate/SeedCloudTR.png)

> [!TIP]
> İşlem çoğaltmayı, kaynak veritabanınızın bir alt kümesini geçirmek için de kullanabilirsiniz. Azure SQL Veritabanına çoğalttığınız yayın, çoğaltmakta olduğunuz veritabanındaki bir tablo alt kümesiyle sınırlanabilir. Çoğaltılmakta olan her tablo için verileri bir satır alt kümesi ve/veya sütun alt kümesi ile sınırlayabilirsiniz.

## <a name="migration-to-sql-database-using-transaction-replication-workflow"></a>İşlem Çoğaltma iş akışı kullanılarak SQL Veritabanına geçiş

> [!IMPORTANT]
> Microsoft Azure ve SQL Veritabanı güncelleştirmeleriyle aynı özelliklere sahip olması için SQL Server Management Studio’nun en son sürümünü kullanın. SQL Server Management Studio’nun eski sürümleri, SQL Veritabanını abone olarak ayarlayamaz. [SQL Server Management Studio’yu güncelleyin](https://msdn.microsoft.com/library/mt238290.aspx).

1. Dağıtımı Ayarlama
   - [SQL Server Management Studio (SSMS) kullanma](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_1)
   - [Transact-SQL kullanma](https://msdn.microsoft.com/library/ms151192.aspx#Anchor_2)

2. Yayın Oluşturma
   - [SQL Server Management Studio (SSMS) kullanma](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_1)
   - [Transact-SQL kullanma](https://msdn.microsoft.com/library/ms151160.aspx#Anchor_2)
3. Abonelik Oluşturma
   - [SQL Server Management Studio (SSMS) kullanma](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_0)
   - [Transact-SQL kullanma](https://msdn.microsoft.com/library/ms152566.aspx#Anchor_1)

SQL veritabanına geçiş için bazı ipuçları ve farklılıklar

- Yerel dağıtıcı kullanma
  - Bunu yapmak sunucu üzerinde bir performans etkisine neden olur.
  - Performans etkisi kabul edilemez boyuttaysa başka bir sunucu kullanabilirsiniz, ancak bunun yapılması yönetimi daha karmaşık hale getirir.
- Bir anlık görüntü klasörü seçerken, seçtiğiniz klasörün çoğaltmak istediğiniz her tabloya ait BCP’yi saklayacak kadar büyük olduğundan emin olun.
- Anlık görüntü oluşturma işlemi tamamlanana kadar ilişkili tabloları kilitler, bu nedenle anlık görüntünüzü uygun şekilde zamanlayın.
- Azure SQL Veritabanında yalnızca iletme abonelikleri desteklenir. Aboneleri yalnızca kaynak veritabanından ekleyebilirsiniz.

## <a name="resolving-database-migration-compatibility-issues"></a>Veritabanı geçişi uyumluluk sorunlarını çözme

Kaynak veritabanındaki SQL Server sürümüne ve geçirdiğiniz veritabanının karmaşıklığına bağlı olarak karşılaşabileceğiniz çok çeşitli uyumluluk sorunları vardır. Eski SQL Server sürümlerinde daha fazla uyumluluk sorunları algılanabilir. Aşağıdaki kaynakları kullanabilir ve ek olarak istediğiniz arama motorunu kullanarak hedefli bir İnternet araması yapabilirsiniz:

- [Azure SQL Veritabanında desteklenmeyen SQL Server veritabanı özellikleri](sql-database-transact-sql-information.md)
- [SQL Server 2016'da Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [SQL Server 2014'te Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [SQL Server 2012'de Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [SQL Server 2008 R2'de Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [SQL Server 2005'te Artık Sağlanmayan Veritabanı Altyapısı İşlevleri](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

İnternet aramasına ve bu kaynaklara ek olarak [MSDN SQL Server topluluk forumlarını](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) veya [StackOverflow](https://stackoverflow.com/) sitesini de kullanabilirsiniz.

> [!IMPORTANT]
> SQL Database Managed Instance, varolan bir SQL Server örneğini ve veritabanlarını en az uyumluluk sorunu olmadan geçirmenizi sağlar. [Yönetilen Örnek Nedir'ı](sql-database-managed-instance.md)Görün.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş sırasında tempdb kullanımını izlemek](https://blogs.msdn.microsoft.com/azuresqlemea/2016/12/28/lesson-learned-10-monitoring-tempdb-usage/) için Azure SQL EMEA Mühendisleri blogundaki betiği kullanın.
- [Geçiş devam ederken veritabanınızın işlem günlüğü alanını izlemek](https://docs.microsoft.com/archive/blogs/azuresqlemea/lesson-learned-7-monitoring-the-transaction-log-space-of-my-database) için Azure SQL EMEA Mühendisleri blogundaki betiği kullanın.
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).
- Geçişten sonra UTC saati ile çalışma hakkında daha fazla bilgi için bkz. [Yerel saat diliminiz için varsayılan saat dilimini değiştirme](https://blogs.msdn.microsoft.com/azuresqlemea/2016/07/27/lesson-learned-4-modifying-the-default-time-zone-for-your-local-time-zone/).
- Geçişten sonra veritabanının varsayılan dilini değiştirme hakkında daha fazla bilgi için bkz. [Azure SQL Veritabanının varsayılan dilini değiştirme](https://blogs.msdn.microsoft.com/azuresqlemea/2017/01/13/lesson-learned-16-how-to-change-the-default-language-of-azure-sql-database/).
