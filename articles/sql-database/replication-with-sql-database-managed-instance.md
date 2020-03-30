---
title: Yönetilen örnek veritabanında çoğaltmayı yapılandırma
description: Azure SQL Veritabanı yönetilen örnek yayımcısı/dağıtıcısı ve yönetilen örnek abone arasında işlem çoğaltma yapılandırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: ferno
ms.reviewer: mathoma
ms.date: 02/07/2019
ms.openlocfilehash: 9af7b471210ca3cc69428e68aef4aafaee159344
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299082"
---
# <a name="configure-replication-in-an-azure-sql-database-managed-instance-database"></a>Azure SQL Veritabanı yönetilen örnek veritabanında çoğaltmayı yapılandırma

İşlemsel çoğaltma, verileri BIR SQL Server veritabanından veya başka bir örnek veritabanından Azure SQL Veritabanı yönetilen örnek veritabanında çoğaltmanızı sağlar. 

Bu makalede, yönetilen bir örnek yayımcı/dağıtıcı ve yönetilen bir örnek abone arasında çoğaltma nasıl yapılandırılabildiğini gösterir. 

![Yönetilen iki örnek arasında çoğaltma](media/replication-with-sql-database-managed-instance/sqlmi-sqlmi-repl.png)

Ayrıca, Azure SQL Veritabanı'nda yönetilen bir örnek veritabanında yapılan değişiklikleri şu şekilde itmek için işlem çoğaltmayı da kullanabilirsiniz:

- Bir SQL Server veritabanı.
- Azure SQL Veritabanı'nda tek bir veritabanı.
- Azure SQL Veritabanı elastik havuzunda havuzlu veritabanı.
 
İşlem çoğaltma, Azure SQL [Veritabanı yönetilen örnekte](sql-database-managed-instance.md)genel önizlemededir. Yönetilen bir örnek yayımcı, dağıtıcı ve abone veritabanlarını barındırabilir. Kullanılabilir yapılandırmalar için [işlem çoğaltma yapılandırmalarına](sql-database-managed-instance-transactional-replication.md#common-configurations) bakın.

  > [!NOTE]
  > - Bu makalede, kaynak grubu oluşturma ile başlayarak, bir Azure Veritabanı yönetilen örnek ile çoğaltma yapılandırmada bir kullanıcı ya da baştan sona yönlendirmek için tasarlanmıştır. Zaten dağıtılmış örnekleri başardıysanız, yayımcı veritabanınızı oluşturmak için [Adım 4'e](#4---create-a-publisher-database) veya zaten bir yayımcı ve abone veritabanınız varsa ve çoğaltma yapılandırmaya başlamaya hazırsanız [Adım 6'ya](#6---configure-distribution) geçin.  
  > - Bu makale, yayımcınızı ve dağıtıcınızı aynı yönetilen örnekte yapılandırır. Dağıtıcıyı ayrı bir manged örneğine yerleştirmek için, [bir MI yayımcısı ile BIR MI dağıtıcısı arasındaki](sql-database-managed-instance-configure-replication-tutorial.md)çoğaltmayı yapılandıran öğreticiye bakın. 

## <a name="requirements"></a>Gereksinimler

Yönetilen bir örneğin yayımcı ve/veya dağıtıcı olarak işlev göreyönetilmesi şunları gerektirir:

- Yayımcı yönetilen örneğinin dağıtıcı ve aboneyle aynı sanal ağda olması veya üç varlığın sanal ağları arasında [vNet eşleme](../virtual-network/tutorial-connect-virtual-networks-powershell.md) sayılmı oluşturulmuştur. 
- Bağlantı, çoğaltma katılımcıları arasında SQL Kimlik Doğrulaması kullanır.
- Çoğaltma çalışma dizini için bir Azure Depolama Hesabı paylaşımı.
- Azure dosya paylaşımına erişmek için yönetilen örnekler için NSG'nin güvenlik kurallarında Bağlantı Noktası 445 (TCP giden) açıktır.  "Azure depolama depolama \<hesap adı> os hatası 53 ile bağlanamadığını" hatayla karşılaşırsanız, ilgili SQL Yönetilen Örnek Alt netinNG'ye giden bir kural eklemeniz gerekir.


 > [!NOTE]
 > Azure SQL Veritabanı'nda tek veritabanları ve havuza toplanan veritabanları yalnızca abone olabilir. 


## <a name="features"></a>Özellikler

Destekle -yen:

- Sql Server'ın şirket içi ve yönetilen örneklerinin Azure SQL Veritabanı'nda işlem selve anlık çoğaltma karışımı.
- Aboneler şirket içi SQL Server veritabanlarında, Azure SQL Veritabanı'nda tek veritabanlarında/yönetilen örneklerde veya Azure SQL Veritabanı elastik havuzlarında birleştirilmiş veritabanlarında bulunabilir.
- Tek yönlü veya çift yönlü çoğaltma.

Aşağıdaki özellikler Azure SQL Veritabanı'nda yönetilen bir örnekte desteklenmez:

- [Güncel abonelikler.](/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication)
- İşlemsel çoğaltma ile [etkin coğrafi çoğaltma.](sql-database-active-geo-replication.md) Etkin coğrafi çoğaltma yerine, [Otomatik başarısız grupları](sql-database-auto-failover-group.md)kullanın, ancak yayının birincil yönetilen örnekten [el ile silinmesi](sql-database-managed-instance-transact-sql-information.md#replication) ve başarısız olduktan sonra ikincil yönetilen örnekte yeniden oluşturulması gerektiğini unutmayın.  
 
## <a name="1---create-a-resource-group"></a>1 - Kaynak grubu oluşturma

Adında `SQLMI-Repl`bir kaynak grubu oluşturmak için [Azure portalını](https://portal.azure.com) kullanın.  

## <a name="2---create-managed-instances"></a>2 - Yönetilen örnekleri oluşturma

Aynı sanal ağ da ve alt ağda yönetilen iki [örnek](sql-database-managed-instance-create-tutorial-portal.md) oluşturmak için [Azure portalını](https://portal.azure.com) kullanın. Örneğin, yönetilen iki örneği adlandırın:

- `sql-mi-pub`(rastgeleleştirme için bazı karakterlerle birlikte)
- `sql-mi-sub`(rastgeleleştirme için bazı karakterlerle birlikte)

Ayrıca, Azure SQL Veritabanı yönetilen örneklerinize [bağlanmak için bir Azure VM yapılandırmanız](sql-database-managed-instance-configure-vm.md) gerekir. 

## <a name="3---create-azure-storage-account"></a>3 - Azure Depolama Hesabı Oluştur

Çalışma dizini için [bir Azure Depolama Hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) ve ardından depolama hesabı içinde bir dosya [paylaşımı](../storage/files/storage-how-to-create-file-share.md) oluşturun. 

Dosya paylaşım yolunu aşağıdaki biçiminde kopyalayın:`\\storage-account-name.file.core.windows.net\file-share-name`

Örnek: `\\replstorage.file.core.windows.net\replshare`

Depolama erişim anahtarlarını aşağıdaki biçiminde kopyalayın:`DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net`

Örnek: `DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net`

Daha fazla bilgi için [bkz.](../storage/common/storage-account-keys-manage.md) 

## <a name="4---create-a-publisher-database"></a>4 - Yayıncı veritabanı oluşturma

SQL Server `sql-mi-pub` Management Studio'yu kullanarak yönetilen örneğinize bağlanın ve yayıncı veritabanınızı oluşturmak için aşağıdaki Transact-SQL (T-SQL) kodunu çalıştırın:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_PUB]
GO

USE [ReplTran_PUB]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO


USE [ReplTran_PUB]
GO

INSERT INTO ReplTest (ID, c1) VALUES (6, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (2, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (3, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (4, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (5, 'pub')
GO
SELECT * FROM ReplTest
GO
```

## <a name="5---create-a-subscriber-database"></a>5 - Abone veritabanı oluşturma

SQL Server `sql-mi-sub` Management Studio'yu kullanarak yönetilen örneğinize bağlanın ve boş abone veritabanınızı oluşturmak için aşağıdaki T-SQL kodunu çalıştırın:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_SUB]
GO

USE [ReplTran_SUB]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO
```

## <a name="6---configure-distribution"></a>6 - Yapıdağılımı

SQL Server `sql-mi-pub` Management Studio'yu kullanarak yönetilen örneğinize bağlanın ve dağıtım veritabanınızı yapılandırmak için aşağıdaki T-SQL kodunu çalıştırın. 

```sql
USE [master]
GO

EXEC sp_adddistributor @distributor = @@ServerName;
EXEC sp_adddistributiondb @database = N'distribution';
GO
```

## <a name="7---configure-publisher-to-use-distributor"></a>7 - Yayıncıyı distribütör kullanacak şekilde yapılandırın 

Yayımcı yönetilen `sql-mi-pub`örneğinde, sorgu yürütmesini [SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) moduna geçin ve yeni dağıtıcıyı yayımcınıza kaydetmek için aşağıdaki kodu çalıştırın. 

```sql
:setvar username loginUsedToAccessSourceManagedInstance
:setvar password passwordUsedToAccessSourceManagedInstance
:setvar file_storage "\\storage-account-name.file.core.windows.net\file-share-name"
-- example: file_storage "\\replstorage.file.core.windows.net\replshare"
:setvar file_storage_key "DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net"
-- example: file_storage_key "DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net"

USE [master]
EXEC sp_adddistpublisher
  @publisher = @@ServerName,
  @distribution_db = N'distribution',
  @security_mode = 0,
  @login = N'$(username)',
  @password = N'$(password)',
  @working_directory = N'$(file_storage)',
  @storage_connection_string = N'$(file_storage_key)'; -- Remove this parameter for on-premises publishers
```

   > [!NOTE]
   > file_storage parametresi için yalnızca`\`ters eğik çizgi () kullandığınızdan emin olun. İleri eğik`/`çizgi () kullanmak, dosya paylaşımına bağlanırken hataya neden olabilir. 

Bu komut dosyası yönetilen örnekte yerel bir yayımcıyı yapılandırır, bağlantılı bir sunucu ekler ve SQL Server Agent için bir iş kümesi oluşturur. 

## <a name="8---create-publication-and-subscriber"></a>8 - Yayın ve abone oluşturma

[SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) modunu kullanarak, veritabanınız için çoğaltmayı etkinleştirmek ve yayımcınız, dağıtıcınız ve aboneniz arasında çoğaltmayı yapılandırmak için aşağıdaki T-SQL komut dosyasını çalıştırın. 

```sql
-- Set variables
:setvar username sourceLogin
:setvar password sourcePassword
:setvar source_db ReplTran_PUB
:setvar publication_name PublishData
:setvar object ReplTest
:setvar schema dbo
:setvar target_server "sql-mi-sub.wdec33262scj9dr27.database.windows.net"
:setvar target_username targetLogin
:setvar target_password targetPassword
:setvar target_db ReplTran_SUB

-- Enable replication for your source database
USE [$(source_db)]
EXEC sp_replicationdboption
  @dbname = N'$(source_db)',
  @optname = N'publish',
  @value = N'true';

-- Create your publication
EXEC sp_addpublication
  @publication = N'$(publication_name)',
  @status = N'active';


-- Configure your log reaer agent
EXEC sp_changelogreader_agent
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the publication snapshot
EXEC sp_addpublication_snapshot
  @publication = N'$(publication_name)',
  @frequency_type = 1,
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the ReplTest table to the publication
EXEC sp_addarticle 
  @publication = N'$(publication_name)',
  @type = N'logbased',
  @article = N'$(object)',
  @source_object = N'$(object)',
  @source_owner = N'$(schema)';

-- Add the subscriber
EXEC sp_addsubscription
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @destination_db = N'$(target_db)',
  @subscription_type = N'Push';

-- Create the push subscription agent
EXEC sp_addpushsubscription_agent
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @subscriber_db = N'$(target_db)',
  @subscriber_security_mode = 0,
  @subscriber_login = N'$(target_username)',
  @subscriber_password = N'$(target_password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Initialize the snapshot
EXEC sp_startpublication_snapshot
  @publication = N'$(publication_name)';
```

## <a name="9---modify-agent-parameters"></a>9 - Aracı parametrelerini değiştirin

Azure SQL Veritabanı yönetilen örnek şu anda çoğaltma aracıları ile bağlantı ile bazı arka uç sorunları yaşıyor. Bu sorun giderilirken, çoğaltma aracıları için oturum açma süresi değerini artırmak için geçici çözüm. 

Giriş zamanını artırmak için yayımcıda aşağıdaki T-SQL komutunu çalıştırın: 

```sql
-- Increase login timeout to 150s
update msdb..sysjobsteps set command = command + N' -LoginTimeout 150' 
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Oturum açma zaman larını varsayılan değere geri ayarlamak için aşağıdaki T-SQL komutunu yeniden çalıştırın, bunu yapmanız gerekir:

```sql
-- Increase login timeout to 30
update msdb..sysjobsteps set command = command + N' -LoginTimeout 30' 
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Bu değişiklikleri uygulamak için üç aracıyı yeniden başlatın. 

## <a name="10---test-replication"></a>10 - Test çoğaltma

Çoğaltma yapılandırıldıktan sonra, yayımcıya yeni öğeler ekleyerek ve değişikliklerin aboneye yayılmasını izleyerek bunu sınayabilirsiniz. 

Abonedeki satırları görüntülemek için aşağıdaki T-SQL snippet'i çalıştırın:

```sql
select * from dbo.ReplTest
```

Yayımcıya ek satırlar eklemek için aşağıdaki T-SQL snippet'i çalıştırın ve ardından abonelerdeki satırları yeniden denetleyin. 

```sql
INSERT INTO ReplTest (ID, c1) VALUES (15, 'pub')
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yayını bırakmak için aşağıdaki T-SQL komutunu çalıştırın:

```sql
-- Drops the publication
USE [ReplTran_PUB]
EXEC sp_droppublication @publication = N'PublishData'
GO
```

Çoğaltma seçeneğini veritabanından kaldırmak için aşağıdaki T-SQL komutunu çalıştırın:

```sql
-- Disables publishing of the database
USE [ReplTran_PUB]
EXEC sp_removedbreplication
GO
```

Yayımlama ve dağıtımı devre dışı bırakıp aşağıdaki T-SQL komutunu çalıştırın:

```sql
-- Drops the distributor
USE [master]
EXEC sp_dropdistributor @no_checks = 1
GO
```

Yönetilen örnek kaynaklarını kaynak [grubundan silerek](../azure-resource-manager/management/manage-resources-portal.md#delete-resources) ve kaynak grubunu `SQLMI-Repl`silerek Azure kaynaklarınızı temizleyebilirsiniz. 

   
## <a name="see-also"></a>Ayrıca Bkz.

- [İşlemsel çoğaltma](sql-database-managed-instance-transactional-replication.md)
- [Öğretici: Bir MI yayımcısı ile SQL Server abonesi arasında işlem çoğaltma yapılandırma](sql-database-managed-instance-configure-replication-tutorial.md)
- [Yönetilen Örnek nedir?](sql-database-managed-instance.md)
