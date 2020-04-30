---
title: Çoğaltma
description: Azure SQL veritabanı tek veritabanları ve elastik havuzlardaki veritabanları SQL Server çoğaltma kullanma hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: mathoma
ms.date: 04/28/2020
ms.openlocfilehash: 49be53febc1783edfa16fd019a094a7e80e1cdf7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231654"
---
# <a name="replication-to-sql-database-single-and-pooled-databases"></a>SQL veritabanı tekil ve havuza alınmış veritabanlarına çoğaltma

SQL Server çoğaltma, Azure SQL veritabanı 'ndaki bir [SQL veritabanı sunucusundaki](sql-database-servers.md) tek ve havuza alınmış veritabanlarına yapılandırılabilir.  

> [!NOTE]
> Bu makalede, Azure SQL veritabanı 'nda [İşlemsel çoğaltmanın](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) kullanımı açıklanmaktadır. Tek tek veritabanlarının tam okunabilir çoğaltmalarını oluşturmanızı sağlayan bir Azure SQL veritabanı özelliği olan [etkin coğrafi çoğaltma](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication)ile ilgili değildir.

## <a name="supported-configurations"></a>Desteklenen Yapılandırmalar
  
- SQL Server, şirket içinde çalışan bir SQL Server örneği veya buluttaki bir Azure sanal makinesinde çalışan bir SQL Server örneği olabilir. Daha fazla bilgi için bkz. [Azure sanal makinelerine genel bakış SQL Server](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/).  
- Azure SQL veritabanı, bir SQL Server yayımcısının gönderim abonesi olmalıdır.  
- Dağıtım veritabanı ve çoğaltma aracıları bir Azure SQL veritabanına yerleştirilemez.  
- Anlık görüntü ve tek yönlü işlemsel çoğaltma desteklenir. Eşler arası işlem çoğaltma ve birleştirme çoğaltması desteklenmez.
- Çoğaltma, Azure SQL veritabanı yönetilen örneği 'nde genel önizleme için kullanılabilir. Yönetilen örnek, Yayımcı, dağıtıcı ve abone veritabanlarını barındırabilirler. Daha fazla bilgi için bkz. [SQL veritabanı yönetilen örneği Ile çoğaltma](replication-with-sql-database-managed-instance.md).

## <a name="versions"></a>Sürümler  

Şirket içi SQL Server yayımcılar ve dağıtımcılar, en azından aşağıdaki sürümlerden birini kullanıyor olmalıdır:  

- SQL Server 2016 ve üzeri
- SQL Server 2014 [RTM CU10 (12.0.4427.24)](https://support.microsoft.com/help/3094220/cumulative-update-10-for-sql-server-2014) veya [SP1 CU3 (12.0.2556.4)](https://support.microsoft.com/help/3094221/cumulative-update-3-for-sql-server-2014-service-pack-1)
- SQL Server 2012 [SP2 CU8 (11.0.5634.1)](https://support.microsoft.com/help/3082561/cumulative-update-8-for-sql-server-2012-sp2) veya [SP3 (11.0.6020.0)](https://www.microsoft.com/download/details.aspx?id=49996)

> [!NOTE]
> Desteklenmeyen bir sürüm kullanılarak çoğaltmayı yapılandırmaya çalışmak, hata numarası MSSQL_REPL20084 (işlem aboneye bağlanamaz.) ve MSSQL_REPL40532 (oturum açma tarafından istenen sunucu \<adı> açılamıyor. Oturum açılamadı.).  

Azure SQL veritabanı 'nın tüm özelliklerini kullanmak için, en son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve [SQL Server veri araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)sürümlerini kullanıyor olmanız gerekir.  

  
## <a name="remarks"></a>Açıklamalar

- Çoğaltma, [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) kullanılarak veya yayımcıdaki Transact-SQL deyimleri yürütülerek yapılandırılabilir. Azure portal kullanarak çoğaltmayı yapılandıramazsınız.  
- Çoğaltma, Azure SQL veritabanına bağlanmak için yalnızca SQL Server kimlik doğrulaması oturum açma bilgilerini kullanabilir.
- Çoğaltılan tablolarda birincil anahtar olmalıdır.  
- Mevcut bir Azure aboneliğinizin olması gerekir.  
- Azure SQL veritabanı abonesi herhangi bir bölgede olabilir.  
- SQL Server tek bir yayını hem Azure SQL veritabanını hem de SQL Server (Şirket içi ve SQL Server bir Azure sanal makinesinde) aboneleri destekleyebilir.  
- Şirket içi SQL Server çoğaltma yönetimi, izleme ve sorun giderme işlemleri yapılmalıdır.  
- Yalnızca Azure SQL veritabanı 'na yönelik gönderme abonelikleri desteklenir.  
- Yalnızca `@subscriber_type = 0` SQL veritabanı için **sp_addsubscription** desteklenir.  
- Azure SQL veritabanı, çift yönlü, anında, güncelleştirilebilir veya eşler arası çoğaltmayı desteklemez.

## <a name="replication-architecture"></a>Çoğaltma mimarisi  

![çoğaltma-SQL-veritabanı](./media/replication-to-sql-database/replication-to-sql-database.png)  

## <a name="scenarios"></a>Senaryolar  

### <a name="typical-replication-scenario"></a>Tipik çoğaltma senaryosu  

1. Şirket içi SQL Server veritabanında bir işlem çoğaltması yayını oluşturun.  
2. Şirket içi SQL Server Azure SQL veritabanı 'na bir abonelik gönderimi oluşturmak için **yeni abonelik Sihirbazı 'nı** veya Transact-SQL deyimlerini kullanın.  
3. Azure SQL veritabanı 'nda tek ve havuza alınmış veritabanları sayesinde, ilk veri kümesi, anlık görüntü Aracısı tarafından oluşturulan ve Dağıtım Aracısı tarafından dağıtılan ve uygulanan bir anlık görüntüdür. Yönetilen örnek veritabanıyla, abone veritabanını temel almak için bir veritabanı yedeklemesi de kullanabilirsiniz.

### <a name="data-migration-scenario"></a>Veri geçiş senaryosu  

1. Şirket içi SQL Server veritabanından Azure SQL veritabanı 'na veri çoğaltmak için işlem çoğaltmayı kullanın.  
2. Azure SQL veritabanı kopyasını güncelleştirmek için istemciyi veya orta katman uygulamaları yeniden yönlendirin.  
3. Tablonun SQL Server sürümünün güncelleştirilmesini durdurun ve yayını kaldırın.  

## <a name="limitations"></a>Sınırlamalar

Azure SQL veritabanı abonelikleri için aşağıdaki seçenekler desteklenmez:

- Dosya grupları ilişkilendirmesini Kopyala  
- Tablo bölümleme düzenlerini Kopyala  
- Dizin bölümleme düzenlerini Kopyala  
- Kullanıcı tanımlı istatistikleri kopyalama  
- Varsayılan bağlamaları Kopyala  
- Kural bağlamalarını Kopyala  
- Tam metin dizinlerini Kopyala  
- XML XSD 'yi Kopyala  
- XML dizinlerini Kopyala  
- İzinleri Kopyala  
- Uzamsal dizinleri Kopyala  
- Filtrelenmiş dizinleri Kopyala  
- Veri sıkıştırma özniteliğini Kopyala  
- Seyrek sütun özniteliğini Kopyala  
- FILESTREAM 'i MAX veri türlerine Dönüştür  
- HierarchyId 'yi MAX veri türlerine Dönüştür  
- Uzamsal değeri en fazla veri türlerine Dönüştür  
- Genişletilmiş özellikleri Kopyala  
- İzinleri Kopyala  

### <a name="limitations-to-be-determined"></a>Belirlenecek sınırlamalar

- Harmanlamayı Kopyala  
- SP 'nin serileştirilmiş bir işleminde yürütme  

## <a name="examples"></a>Örnekler

Yayın ve gönderme temelli bir abonelik oluşturun. Daha fazla bilgi için bkz.
  
- [Yayın oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- Abone olarak Azure SQL veritabanı sunucu adını (örneğin, **Niazurestodbdns. Database. Windows. net '**) ve Azure SQL veritabanı adını hedef veritabanı (örneğin **AdventureWorks**) olarak kullanarak [bir anında iletme aboneliği oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) .  

## <a name="see-also"></a>Ayrıca Bkz.  

- [İşlem çoğaltması](sql-database-managed-instance-transactional-replication.md)
- [Yayın oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Itme aboneliği oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Çoğaltma Türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [İzleme (çoğaltma)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Abonelik başlatma](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
