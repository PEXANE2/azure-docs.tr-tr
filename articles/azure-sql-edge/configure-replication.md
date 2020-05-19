---
title: Azure SQL Edge 'e çoğaltmayı yapılandırma (Önizleme)
description: Azure SQL Edge 'e çoğaltmayı yapılandırma hakkında bilgi edinin (Önizleme)
keywords: ''
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: e2b37e0f3ccf5fcebe4723c05d644f2cbb7c1d56
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83596949"
---
# <a name="configure-replication-to-azure-sql-edge-preview"></a>Azure SQL Edge 'e çoğaltmayı yapılandırma (Önizleme) 

Azure SQL Edge örneği, tek yönlü bir işlem çoğaltması veya anlık görüntü çoğaltması için anında iletme abonesi olarak yapılandırılabilir. Azure SQL Edge örneği, bir işlem çoğaltma yapılandırması için yayımcı veya dağıtıcı olarak çalışamaz. Birleştirme çoğaltması, P2P çoğaltma, Oracle yayımlaması Azure SQL Edge 'de desteklenmez.

## <a name="supported-configurations"></a>**Desteklenen konfigürasyonlar**:
  
- Azure SQL Edge örneği, yayımcı için bir anında iletme abonesi olmalıdır.
- Yayımcı ve dağıtıcı şunlardan biri olabilir
   - Şirket içinde çalışan bir SQL Server örneği veya bir Azure sanal makinesinde çalışan bir SQL Server örneği. Daha fazla bilgi için bkz. [Azure sanal makinelerine genel bakış SQL Server](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-infrastructure-services/). SQL Server örneklerin SQL Server 2016 ' den büyük bir sürümü kullanıyor olması gerekir.
   - Azure SQL veritabanı yönetilen örneği örneği. Yönetilen örnek, Yayımcı, dağıtıcı ve abone veritabanlarını barındırabilirler. Daha fazla bilgi için bkz. [SQL veritabanı yönetilen örneği Ile çoğaltma](https://docs.microsoft.com/azure/sql-database/replication-with-sql-database-managed-instance/).

- Dağıtım veritabanı ve çoğaltma aracıları bir Azure SQL Edge örneğine yerleştirilemez.  

> [!NOTE]
> Desteklenmeyen bir sürüm kullanılarak çoğaltmayı yapılandırmaya çalışmak, hata numarası MSSQL_REPL20084 (işlem aboneye bağlanamaz.) ve MSSQL_REPL40532 ( \< oturum açma tarafından istenen sunucu adı> açılamıyor. Oturum açılamadı.).  

Azure SQL Edge 'in tüm özelliklerini kullanmak için, en son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve [SQL Server veri araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)sürümlerini kullanıyor olmanız gerekir.  

## <a name="remarks"></a>Açıklamalar

- Çoğaltma, [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) kullanılarak veya SQL Server Management Studio veya [Azure veritabanı Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) kullanılarak YAYıMCıDA Transact-SQL deyimleri yürütülerek yapılandırılabilir
- Çoğaltma, Azure SQL Edge örneğine bağlanmak için yalnızca SQL Server kimlik doğrulaması oturum açma bilgilerini kullanabilir.
- Çoğaltılan tablolarda birincil anahtar olmalıdır.
- SQL Server tek bir yayını hem Azure SQL Edge hem de SQL Server (Şirket içi ve SQL Server bir Azure sanal makinesinde) abonelerine destekleyebilir.  
- Şirket içi SQL Server çoğaltma yönetimi, izleme ve sorun giderme işlemleri yapılmalıdır.  
- Yalnızca Azure SQL Edge 'e yönelik gönderme abonelikleri desteklenir.  
- Yalnızca `@subscriber_type = 0` Azure SQL Edge için **sp_addsubscription** desteklenir.  
- Azure SQL Edge, çift yönlü, anında, güncelleştirilebilir veya eşler arası çoğaltmayı desteklemez.
- Azure SQL Edge, bir veya daha fazla desteklenmeyen özellik içeren bir veritabanını (veya veritabanı içindeki nesneleri) çoğaltma girişimi nedeniyle yalnızca SQL Server veya Azure SQL veritabanı yönetilen örneği 'nde kullanılabilen özelliklerin bir alt kümesini destekler. Örneğin, uzamsal veri türlerine sahip nesneler içeren bir veritabanını çoğaltma girişimi bir hatayla sonuçlanır. Azure SQL Edge tarafından desteklenen özellikler hakkında daha fazla bilgi için bkz. [Azure SQL Edge 'In desteklenen özellikleri](features.md).

## <a name="scenarios"></a>Senaryolar  

### <a name="initializing-reference-data-on-an-edge-instance"></a>Bir kenar örneğinde başvuru verilerini başlatma

Zaman içinde değişen başvuru verileriyle kenar örneğini başlatmak için gerekli olan, çoğaltmanın yararlı olduğu yaygın bir senaryodur. Örneğin, Edge örneğindeki ML modellerini, şirket içi SQL Server örneği üzerinde eğitilen bir şekilde güncelleştirme.

1. Şirket içi SQL Server veritabanında bir işlem çoğaltması yayını oluşturun.  
2. Şirket içi SQL Server, Azure SQL Edge 'e yönelik bir aboneliğe gönderim oluşturmak için **yeni abonelik Sihirbazı 'nı** veya Transact-SQL deyimlerini kullanın.  
3. Azure SQL Edge 'deki çoğaltılan veritabanı, anlık görüntü Aracısı tarafından oluşturulan ve Dağıtım Aracısı tarafından dağıtılan ve sunulan bir anlık görüntü kullanılarak veya yayımcının bir veritabanı yedeklemesi kullanılarak başlatılabilir. Bir kez daha, veritabanı yedeklemesi Azure SQL Edge tarafından desteklenmeyen nesneler/özellikler içeriyorsa geri yükleme işlemi başarısız olur.

## <a name="limitations"></a>Sınırlamalar

Azure SQL Edge abonelikleri için aşağıdaki seçenekler desteklenmez:

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
- FILESTREAM, HierarchyId veya uzamsal veri türlerini kopyalayın.
- HierarchyId 'yi MAX veri türlerine Dönüştür  
- Uzamsal değeri en fazla veri türlerine Dönüştür  
- Genişletilmiş özellikleri Kopyala  
- İzinleri Kopyala  

## <a name="examples"></a>Örnekler

Yayın ve gönderme temelli bir abonelik oluşturun. Daha fazla bilgi için bkz.
  
- [Yayın oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- Abone olarak Azure SQL Edge sunucu adı/IP 'si (örneğin **, Myedgeınstance, 1433**) ve Azure SQL Edge örneğindeki hedef veritabanı (örneğin **AdventureWorks**) olarak bir veritabanı adı kullanarak [bir anında iletme aboneliği oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/) .  

## <a name="see-also"></a>Ayrıca bkz.  

- [Yayın oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Itme aboneliği oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Çoğaltma Türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [İzleme (çoğaltma)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Abonelik başlatma](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  


