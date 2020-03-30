---
title: Çoğaltma
description: Sql SQL Veritabanı ile SQL Server çoğaltma işlemini elastik havuzlarda tek veritabanları ve veritabanları ile kullanma hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
ms.date: 01/25/2019
ms.openlocfilehash: f718bc17b987926f4324635f096d5983acdb63fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256477"
---
# <a name="replication-to-sql-database-single-and-pooled-databases"></a>SQL Veritabanı tek ve havuzlu veritabanlarına çoğaltma

SQL Server çoğaltma, Azure SQL Veritabanı'ndaki bir [SQL Veritabanı sunucusunda](sql-database-servers.md) tek ve havuzlu veritabanlarına yapılandırılabilir.  

## <a name="supported-configurations"></a>**Desteklenen Yapılandırmalar:**
  
- SQL Server, şirket içinde çalışan SQL Server'ın bir örneği veya buluttaki bir Azure sanal makinesinde çalışan SQL Server örneği olabilir. Daha fazla bilgi için [Azure Sanal Makinelere genel bakışta SQL Server'a](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/)bakın.  
- Azure SQL veritabanı, bir SQL Server yayımcısının anında netme abonesi olmalıdır.  
- Dağıtım veritabanı ve çoğaltma aracıları bir Azure SQL veritabanına yerleştirilemez.  
- Anlık görüntü ve tek yönlü işlem çoğaltma desteklenir. Eşler arası işlem çoğaltma ve birleştirme çoğaltma desteklenmez.
- Çoğaltma, Azure SQL Veritabanı Yönetilen Örneği'nde genel önizleme için kullanılabilir. Yönetilen Örnek yayımcı, dağıtıcı ve abone veritabanlarını barındırabilir. Daha fazla bilgi için, [SQL Veritabanı Yönetilen Örneği ile Çoğaltma'ya](replication-with-sql-database-managed-instance.md)bakın.

## <a name="versions"></a>Sürümler  

Şirket içi SQL Server yayıncıları ve dağıtıcıları aşağıdaki sürümlerden birini (en azından) kullanıyor olmalıdır:  

- SQL Server 2016 ve üzeri
- SQL Server 2014 [RTM CU10 (12.0.4427.24)](https://support.microsoft.com/help/3094220/cumulative-update-10-for-sql-server-2014) veya [SP1 CU3 (12.0.2556.4)](https://support.microsoft.com/help/3094221/cumulative-update-3-for-sql-server-2014-service-pack-1)
- SQL Server 2012 [SP2 CU8 (11.0.5634.1)](https://support.microsoft.com/help/3082561/cumulative-update-8-for-sql-server-2012-sp2) veya [SP3 (11.0.6020.0)](https://www.microsoft.com/download/details.aspx?id=49996)

> [!NOTE]
> Desteklenmeyen bir sürümü kullanarak çoğaltmayı yapılandırmaya çalışmak hata numarası MSSQL_REPL20084 (İşlem Abone'ye bağlanamadı)ve \<MSSQL_REPL40532 (Giriş tarafından istenen sunucu adını> açılamaz. Giriş başarısız oldu.).  

Azure SQL Veritabanı'nın tüm özelliklerini kullanmak için SQL [Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve [SQL Server Data Tools'un](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)en son sürümlerini kullanıyor olmalısınız.  

  
## <a name="remarks"></a>Açıklamalar

- Çoğaltma, [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) kullanılarak veya yayımcıda Transact-SQL deyimleri yürütülerek yapılandırılabilir. Azure portalını kullanarak çoğaltmayı yapılandıramazsınız.  
- Çoğaltma, bir Azure SQL veritabanına bağlanmak için yalnızca SQL Server kimlik doğrulama oturumlarını kullanabilir.
- Çoğaltılan tabloların birincil anahtarı olmalıdır.  
- Varolan bir Azure aboneliğiniz olmalıdır.  
- Azure SQL veritabanı abonesi herhangi bir bölgede olabilir.  
- SQL Server'daki tek bir yayın hem Azure SQL Veritabanı'nı hem de SQL Server'ı (şirket içi ve Sql Server bir Azure sanal makinesinde) aboneleri destekleyebilir.  
- Çoğaltma yönetimi, izleme ve sorun giderme şirket içi SQL Server'dan yapılmalıdır.  
- Yalnızca Azure SQL Veritabanı'na itme abonelikleri desteklenir.  
- Yalnızca `@subscriber_type = 0` SQL Veritabanı için **sp_addsubscription** olarak desteklenir.  
- Azure SQL Veritabanı çift yönlü, anında, güncellenebilir veya eşler arası çoğaltmayı desteklemez.

## <a name="replication-architecture"></a>Çoğaltma Mimarisi  

![çoğaltma-to-sql-veritabanı](./media/replication-to-sql-database/replication-to-sql-database.png)  

## <a name="scenarios"></a>Senaryolar  

### <a name="typical-replication-scenario"></a>Tipik Çoğaltma Senaryosu  

1. Şirket içi BIR SQL Server veritabanında işlem çoğaltma yayını oluşturun.  
2. Şirket içi SQL Server'da, Azure SQL Veritabanı'na abone olmak için bir itme oluşturmak için **Yeni Abonelik Sihirbazı** veya Transact-SQL deyimleri kullanın.  
3. Azure SQL Veritabanı'ndaki tek ve havuzlu veritabanlarıyla, ilk veri kümesi Anlık Görüntü Aracısı tarafından oluşturulan ve Dağıtım Aracısı tarafından dağıtılan ve uygulanan bir anlık görüntüdür. Yönetilen bir örnek veritabanı yla, abone veritabanını tohumlamak için bir veritabanı yedeklemesi de kullanabilirsiniz.

### <a name="data-migration-scenario"></a>Veri Geçiş Senaryosu  

1. Şirket içi BIR SQL Server veritabanından Azure SQL Veritabanı'na verileri çoğaltmak için işlem çoğaltmayı kullanın.  
2. Azure SQL veritabanı kopyasını güncelleştirmek için istemci veya orta katman uygulamalarını yeniden yönlendirin.  
3. Tablonun SQL Server sürümünü güncelleştirmeyi durdurun ve yayını kaldırın.  

## <a name="limitations"></a>Sınırlamalar

Azure SQL Veritabanı abonelikleri için aşağıdaki seçenekler desteklenmez:

- Dosya grupları ilişkilendirme  
- Tablo bölümleme düzenlerini kopyalama  
- Dizin bölümleme düzenlerini kopyalama  
- Kullanıcı tanımlı istatistikleri kopyalama  
- Varsayılan bağlamaları kopyalama  
- Kural bağlamaları kopyalama  
- Tam metin dizinlerini kopyalama  
- XML XSD kopyala  
- XML dizinlerini kopyalama  
- İzinleri kopyalama  
- Uzamsal dizinleri kopyalama  
- Filtre uygulanmış dizinleri kopyalama  
- Veri sıkıştırma özniteliğini kopyalama  
- Seyrek sütun özniteliğini kopyala  
- Dosya akışını MAX veri türlerine dönüştürme  
- Hiyerarşiyi MAX veri türlerine dönüştürme  
- Uzamsal alanı MAX veri türlerine dönüştürme  
- Genişletilmiş özellikleri kopyalama  
- İzinleri kopyalama  

### <a name="limitations-to-be-determined"></a>Belirlenecek sınırlamalar

- Kopya harmanlama  
- SP'nin serileştirilmiş işleminde yürütme  

## <a name="examples"></a>Örnekler

Bir yayın ve itme aboneliği oluşturun. Daha fazla bilgi için bkz.
  
- [Yayın Oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- Azure SQL Veritabanı sunucu adını abone olarak (örneğin **N'azuresqldbdns.database.windows.net'**) ve hedef veritabanı olarak Azure SQL veritabanı adını **(örneğin AdventureWorks)** kullanarak [Bir Push Aboneliği oluşturun.](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)  

## <a name="see-also"></a>Ayrıca Bkz.  

- [İşlemsel çoğaltma](sql-database-managed-instance-transactional-replication.md)
- [Yayın Oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Push Aboneliği Oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Çoğaltma Türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [İzleme (Çoğaltma)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Aboneliği Başlatma](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
