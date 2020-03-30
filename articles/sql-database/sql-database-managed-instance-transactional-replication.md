---
title: İşlem çoğaltması
description: Azure SQL Veritabanı'nda tek, havuzlu ve örnek veritabanlarıyla SQL Server işlem çoğaltma yı kullanma hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 02/08/2019
ms.openlocfilehash: 41dd336bdb74fbe745ab48ebd3c168af0492ae2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75691012"
---
# <a name="transactional-replication-with-single-pooled-and-instance-databases-in-azure-sql-database"></a>Azure SQL Veritabanı'nda tek, havuzlu ve örnek veritabanları ile işlem çoğaltma

İşlemçoğaltma, Azure SQL Veritabanı ve SQL Server'ın, Azure SQL Veritabanı'ndaki bir tablodan veya SQL Server'dan uzak veritabanlarına yerleştirilen tablolara verileri çoğaltmanızı sağlayan bir özelliğidir. Bu özellik, farklı veritabanlarında birden çok tablo eşitleme sağlar.

## <a name="when-to-use-transactional-replication"></a>İşlemsel çoğaltma ne zaman kullanılır?

İşlemsel çoğaltma aşağıdaki senaryolarda yararlıdır:
- Bir veritabanında bir veya daha fazla tabloda yapılan değişiklikleri yayımlayın ve değişiklikler için abone olan bir veya birden çok SQL Server veya Azure SQL veritabanına dağıtın.
- Dağıtılmış birkaç veritabanını eşitlenmiş durumda tutun.
- Değişiklikleri sürekli olarak yayımlayarak veritabanlarını bir SQL Server'dan veya yönetilen örnekten başka bir veritabanına geçirin.

## <a name="overview"></a>Genel Bakış

İşlemsel çoğaltmadaki temel bileşenler aşağıdaki resimde gösterilmiştir:  

![SQL Veritabanı ile çoğaltma](media/replication-to-sql-database/replication-to-sql-database.png)

**Yayımcı,** güncelleştirmeleri Dağıtımcıya göndererek bazı tablolarda (makalelerde) yapılan değişiklikleri yayımlayan bir örnek veya sunucudur. Şirket içi bir SQL Server'dan herhangi bir Azure SQL veritabanına yayımlama, SQL Server'ın aşağıdaki sürümleriyle desteklenir:

- SQL Server 2019 (önizleme)
- SQL Server 2016 - SQL 2017
- SQL Server 2014 SP1 CU3 veya üzeri (12.00.4427)
- SQL Server 2014 RTM CU10 (12.00.2556)
- SQL Server 2012 SP3 veya üzeri (11.0.6020)
- SQL Server 2012 SP2 CU8 (11.0.5634.0)
- SQL Server'ın Azure'daki nesnelere yayımlanmayı desteklemeyen diğer sürümlerinde, verileri SQL Server'ın daha yeni sürümlerine taşımak için [yeniden yayımlama veri](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) yöntemini kullanmak mümkündür. 

**Dağıtımcı,** makalelerdeki değişiklikleri bir Yayımcı'dan toplayan ve Bunları Abonelere dağıtan bir örnek veya sunucudur. Dağıtıcı, Azure SQL Veritabanı yönetilen örnek veya SQL Server (Publisher sürümüne eşit veya daha yüksek olduğu sürece herhangi bir sürüm) olabilir. 

**Abone, Yayımcı'da** yapılan değişiklikleri alan bir örnek veya sunucudur. Aboneler, Azure SQL Veritabanı veya SQL Server veritabanlarında tek, havuzlu ve örnek veritabanları olabilir. Tek veya havuza konan bir veritabanındaki abone, push-subscriber olarak yapılandırılmalıdır. 

| Rol | Tek ve havuzlu veritabanları | Örnek veritabanları |
| :----| :------------- | :--------------- |
| **Yayımcı** | Hayır | Evet | 
| **Dağıtımcı** | Hayır | Evet|
| **Aboneyi çek** | Hayır | Evet|
| **Push Abone**| Evet | Evet|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > Dağıtıcı bir Örnek veritabanı olduğunda ve abone olmadığında çekme aboneliği desteklenmez. 

Çoğaltma farklı [türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)vardır:


| Çoğaltma | Tek ve havuzlu veritabanları | Örnek veritabanları|
| :----| :------------- | :--------------- |
| [**Standart İşlemsel**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Evet (sadece abone olarak) | Evet | 
| [**Anlık Görüntü**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Evet (sadece abone olarak) | Evet|
| [**Birleştirme birleştirme**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Hayır | Hayır|
| [**Eşler arası**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Hayır | Hayır|
| [**Çift yönlü**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Hayır | Evet|
| [**Güncel abonelikler**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Hayır | Hayır|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - Çoğaltmayı eski bir sürümü kullanarak yapılandırmaya çalışmak, hata numarası MSSQL_REPL20084 (İşlem Abone'ye bağlanamadı.) \<ve MSSQ_REPL40532 (Oturum açma nın istediği> sunucu adını açılamaz. Giriş başarısız oldu.)
  > - Azure SQL Veritabanı'nın tüm özelliklerini kullanmak için SQL [Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve [SQL Server Data Tools'un (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)en son sürümlerini kullanıyor olmalısınız.
  
  ### <a name="supportability-matrix-for-instance-databases-and-on-premises-systems"></a>Örnek Veritabanları ve Şirket İçi sistemler için desteklenebilirlik matrisi
  Örneğin veritabanları için çoğaltma desteklenebilirlik matrisi, SQL Server'ın şirket içi matrisi ile aynıdır. 
  
| **Yayımcı**   | **Dağıtımcı** | **Abone** |
| :------------   | :-------------- | :------------- |
| SQL Server 2019 | SQL Server 2019 | SQL Server 2019 <br/> SQL Server 2017 <br/> SQL Server 2016 <br/>  |
| SQL Server 2017 | SQL Server 2019 <br/>SQL Server 2017 | SQL Server 2019 <br/> SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 |
| SQL Server 2016 | SQL Server 2019 <br/>SQL Server 2017 <br/> SQL Server 2016 | SQL Server 2019 <br/> SQL Server 2017 <br/>SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 |
| SQL Server 2014 | SQL Server 2019 <br/> SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>| SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 |
| SQL Server 2012 | SQL Server 2019 <br/> SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>SQL Server 2012 <br/> | SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 | 
| SQL Server 2008 R2 <br/> SQL Server 2008 | SQL Server 2019 <br/> SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 |  SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 <br/>  |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="requirements"></a>Gereksinimler

- Bağlantı, çoğaltma katılımcıları arasında SQL Kimlik Doğrulaması kullanır. 
- Çoğaltma tarafından kullanılan çalışma dizini için bir Azure Depolama Hesabı paylaşımı. 
- Azure dosya paylaşımına erişmek için yönetilen örnek alt netin güvenlik kurallarında Bağlantı Noktası 445 'in (TCP giden) açık olması gerekir. 
- Yayımcı/Dağıtıcı yönetilen bir durumdaysa ve abone şirket içindeyse Bağlantı Noktası 1433 (TCP giden) açılması gerekir.
- Her tür çoğaltma katılımcısı (Publisher, Distributor, Pull Subscriber ve Push Subscriber) yönetilen örneklere yerleştirilebilir, ancak yayımcı ve dağıtıcı hem bulutta hem de şirket içinde olmalıdır.
- Yayımcı, dağıtıcı ve/veya abone farklı sanal ağlarda varsa, yayıncı ve dağıtıcı arasında VPN eşleme olması ve/veya dağıtıcı ile abone arasında VPN eşlemeolması gibi her bir varlık arasında VPN eşlemesi kurulmalıdır. 


>[!NOTE]
> - Giden ağ güvenlik grubu (NSG) bağlantı noktası 445, dağıtıcı bir örnek veritabanı olduğunda ve abone şirket içindeyse, bir Azure Depolama Dosyasına bağlanırken hata 53 ile karşılaşabilirsiniz. Bu sorunu gidermek için [vNet NSG'yi güncelleştirin.](/azure/storage/files/storage-troubleshoot-windows-file-connection-problems) 


### <a name="compare-data-sync-with-transactional-replication"></a>İşlemsel Çoğaltma ile Veri Eşitleme karşılaştırın

| | Data Sync | İşlem Çoğaltması |
|---|---|---|
| Yararları | - Aktif-aktif destek<br/>- Şirket içi ve Azure SQL Veritabanı arasında çift yönlü | - Daha düşük gecikme<br/>- İşlemsel tutarlılık<br/>- Göç sonrası mevcut topolojiyi yeniden kullanma |
| Dezavantajlar | - 5 dk veya daha fazla gecikme<br/>- İşlemsel tutarlılık yok<br/>- Daha yüksek performans etkisi | - Azure SQL Veritabanı'ndan tek veritabanıveya havuzlu veritabanından yayımlanamıyor<br/>- Yüksek bakım maliyeti |
| | | |

## <a name="common-configurations"></a>Yaygın yapılandırmalar

Genel olarak, yayıncı ve dağıtıcı bulutta veya şirket içinde olmalıdır. Aşağıdaki yapılandırmalar desteklenir: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Yönetilen bir örnekte yerel Dağıtıcı ile yayımcı

![Publisher ve Distributor olarak tek örnek](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

Yayımcı ve dağıtıcı, tek bir yönetilen örnek içinde yapılandırılır ve değişiklikleri diğer yönetilen örnek, tek veritabanı, havuzlu veritabanı veya SQL Server'a şirket içinde dağıtılır. 

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Yönetilen bir örnekte uzak dağıtıcılı yayımcı

Bu yapılandırmada, yönetilen bir örnek, yönetilen örnek, tek veritabanı, havuzlu veritabanı veya yönetilen örnekteki bir veya birden çok hedefe değişiklik dağıtabilen başka bir yönetilen örneğe yerleştirilen dağıtıcıdeğişiklikleri yayımlar veya SQL Server.

![Publisher ve Distributor için ayrı örnekler](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

Yayımcı ve dağıtıcı, yönetilen iki örnekte yapılandırılır. Bu yapılandırma ile bazı kısıtlamalar vardır: 

- Yönetilen her iki örnek de aynı vNet'te dir.
- Yönetilen örneklerin her ikisi de aynı konumdadır.


### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-single-pooled-and-instance-database"></a>Tek, havuzlu ve örnek veritabanında abone olan yayıncı ve dağıtıcı şirket içinde 

![Abone olarak Azure SQL DB](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
Bu yapılandırmada, bir Azure SQL Veritabanı (tek, havuzlu ve örnek veritabanı) bir abonedir. Bu yapılandırma şirket içi Azure'a geçişi destekler. Abone tek veya havuza edilmiş bir veritabanındaysa, itme modunda olmalıdır.  

## <a name="with-failover-groups"></a>Başarısız gruplarla

Bir başarısız **gruptayayımveya** **dağıtıcı** örneğinde coğrafi [failover group](sql-database-auto-failover-group.md)çoğaltma etkinleştirilmişse, yönetilen örnek yöneticinin eski birincil deki tüm yayınları temizlemesi ve bir hata oluştuktan sonra bunları yeni birincil üzerinde yeniden yapılandırması gerekir. Bu senaryoda aşağıdaki etkinlikler gereklidir:

1. Varsa, veritabanında çalışan tüm çoğaltma işlerini durdurun.
2. Aşağıdaki komut dosyasını yayımcı veritabanında çalıştırarak yayıncıdan abonelik meta verilerini bırakın:

   ```sql
   EXEC sp_dropsubscription @publication='<name of publication>', @article='all',@subscriber='<name of subscriber>'
   ```             
 
1. Aboneden abonelik meta verilerini bırakın. Abone örneğinde abonelik veritabanında aşağıdaki komut dosyasını çalıştırın:

   ```sql
   EXEC sp_subscription_cleanup
      @publisher = N'<full DNS of publisher, e.g. example.ac2d23028af5.database.windows.net>', 
      @publisher_db = N'<publisher database>', 
      @publication = N'<name of publication>'; 
   ```                

1. Yayımlanan veritabanında aşağıdaki komut dosyasını çalıştırarak yayımcıdaki tüm çoğaltma nesnelerini zorla bırakın:

   ```sql
   EXEC sp_removedbreplication
   ```

1. Eski dağıtıcıyı zorla orijinal birincil örnekten bırakın (eskiden distribütörü olan eski bir ana bölüme geri dönülmese). Eski dağıtıcı yönetilen örnekte ana veritabanında aşağıdaki komut dosyasını çalıştırın:

   ```sql
   EXEC sp_dropdistributor 1,1
   ```

Bir failover grubundaki bir **abone** örneğinde coğrafi çoğaltma etkinleştirilmişse, yayın, abone yönetilen örnek için başarısız grup dinleyici bitiş noktasına bağlanacak şekilde yapılandırılmalıdır. Bir başarısızlık durumunda, yönetilen örnek yönetici tarafından sonraki eylem meydana gelen failover türüne bağlıdır: 

- Veri kaybı olmayan bir hata için çoğaltma, başarısız olduktan sonra çalışmaya devam edecektir. 
- Veri kaybı ile bir failover için, çoğaltma da çalışacaktır. Kaybolan değişiklikleri tekrar kopyalar. 
- Veri kaybı olan bir hata için, ancak veri kaybı dağıtım veritabanı saklama süresinin dışında, yönetilen örnek yöneticinin abonelik veritabanını yeniden başlatması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Mi yayımcısı ve abone arasında çoğaltma yapılandırma](replication-with-sql-database-managed-instance.md)
- [MI yayımcısı, MI dağıtıcısı ve SQL Server abonesi arasında çoğaltma yapılandırma](sql-database-managed-instance-configure-replication-tutorial.md)
- [Yayın oluşturma.](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- Abone olarak Azure SQL Veritabanı sunucu adını kullanarak [bir itme aboneliği oluşturun](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) (örneğin, `N'azuresqldbdns.database.windows.net` hedef veritabanı olarak Azure SQL Veritabanı adı (örneğin **Adventureworks**. )


İşlem çoğaltma yapılandırma hakkında daha fazla bilgi için aşağıdaki öğreticilere bakın:



## <a name="see-also"></a>Ayrıca Bkz.  

- [MI ve bir başarısız grup ile çoğaltma](sql-database-managed-instance-transact-sql-information.md#replication)
- [SQL Veritabanına Çoğaltma](replication-to-sql-database.md)
- [Yönetilen örneğe çoğaltma](replication-with-sql-database-managed-instance.md)
- [Yayın Oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Push Aboneliği Oluşturma](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Çoğaltma Türleri](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [İzleme (Çoğaltma)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Aboneliği Başlatma](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
