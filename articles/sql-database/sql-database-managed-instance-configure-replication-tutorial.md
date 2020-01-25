---
title: İki yönetilen örnek ve SQL Server arasında işlemsel çoğaltmayı yapılandırma
description: Yayımcı tarafından yönetilen bir örnek, bir dağıtıcı yönetilen örneği ve bir Azure VM 'de bir SQL Server abonesi ile özel DNS bölgesi ve VPN eşlemesi gibi gerekli ağ bileşenleri arasında çoğaltmayı yapılandıran bir öğretici.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 11/21/2019
ms.openlocfilehash: fa6e393500e9deeb91ee84aa5255320003817f08
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719900"
---
# <a name="tutorial-configure-transactional-replication-between-two-managed-instances-and-sql-server"></a>Öğretici: iki yönetilen örnek ve SQL Server arasında işlem çoğaltması yapılandırma


Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> - Yönetilen bir örneği Çoğaltma yayımcısı olarak yapılandırın. 
> - Yönetilen bir örneği, çoğaltma dağıtıcısı olarak yapılandırın. 
> - Abone olarak bir SQL Server yapılandırın. 

![SQL mı pub, SQL mı Dist ve bir SQL Server sub arasında çoğaltma](media/sql-database-managed-instance-failover-group-tutorial/sqlmi-to-sql-replication.png)

Bu öğretici, deneyimli bir hedef kitle için tasarlanmıştır ve kullanıcının hem yönetilen örneklere hem de Azure 'daki sanal makinelere SQL Server dağıtım ve bağlanma hakkında bilgi sahibi olduğunu varsayar. Bu şekilde, bu öğreticideki bazı adımlar glossed. 

Daha fazla bilgi edinmek için bkz. [Azure SQL veritabanı yönetilen örneğine genel bakış](sql-database-managed-instance-index.yml), [yetenekler](sql-database-managed-instance.md)ve [SQL işlem çoğaltma](sql-database-managed-instance-transactional-replication.md) makaleleri.

Yönetilen bir örnek yayımcısı ve yönetilen örnek abonesi arasında çoğaltmayı yapılandırmak için, bkz. [iki yönetilen örnek arasında işlemsel çoğaltmayı yapılandırma](replication-with-sql-database-managed-instance.md). 

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlayabilmeniz için aşağıdaki önkoşullara sahip olduğunuzdan emin olun:

- Bir [Azure aboneliği](https://azure.microsoft.com/free/). 
- Aynı sanal ağ içinde iki yönetilen örnek dağıtmaya yönelik deneyim. 
- Şirket içinde ya da bir Azure VM SQL Server abone. Bu öğretici bir Azure VM kullanır.  
- [SQL Server Management Studio (SSMS) 18,0 veya üzeri](/sql/ssms/download-sql-server-management-studio-ssms).
- [Azure PowerShell](/powershell/azure/install-az-ps?view=azps-1.7.0)'in en son sürümü.
- Bağlantı noktası 445 ve 1433 hem Azure Güvenlik Duvarı hem de Windows güvenlik duvarı üzerinde SQL trafiğine izin verir. 

## <a name="1---create-the-resource-group"></a>1-kaynak grubunu oluşturma
Yeni bir kaynak grubu oluşturmak için aşağıdaki PowerShell kod parçacığını kullanın:

```powershell-interactive
# set variables
$ResourceGroupName = "SQLMI-Repl"
$Location = "East US 2"

# Create a new resource group
New-AzResourceGroup -Name  $ResourceGroupName -Location $Location
```

## <a name="2---create-two-managed-instances"></a>2-iki yönetilen örnek oluşturma
[Azure Portal](https://portal.azure.com)kullanarak bu yeni kaynak grubu içinde iki yönetilen örnek oluşturun. 

- Yayımcının yönettiği örneğinin adı: `sql-mi-publisher` (rastgele seçim için birkaç karakter ile birlikte) ve sanal ağın adı `vnet-sql-mi-publisher`olmalıdır.
- Dağıtımcı yönetilen örneğinin adı: `sql-mi-distributor` (rastgele seçim için birkaç karakterle birlikte) ve _Yayımcı tarafından yönetilen örnekle aynı sanal ağda_olmalıdır.

   ![Dağıtıcı için yayımcı VNET 'i kullanma](media/sql-database-managed-instance-configure-replication-tutorial/use-same-vnet-for-distributor.png)

Yönetilen örnek oluşturma hakkında daha fazla bilgi için bkz [. portalda yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md)

  > [!NOTE]
  > Kolaylık sağlaması için ve en yaygın yapılandırma olduğundan, bu öğretici dağıtıcı tarafından yönetilen örneği yayımcının aynı sanal ağ içinde yerleştirmesini önerir. Ancak, dağıtıcıyı ayrı bir sanal ağda oluşturmak mümkündür. Bunu yapmak için yayımcının sanal ağları ve dağıtıcı arasında VPN eşlemesi yapılandırmanız ve ardından dağıtımcı ve abonenin sanal ağları arasında VPN eşlemesi yapılandırmanız gerekir. 

## <a name="3---create-a-sql-server-vm"></a>3-SQL Server VM oluşturma
[Azure Portal](https://portal.azure.com)kullanarak SQL Server sanal makine oluşturun. SQL Server sanal makine aşağıdaki özelliklere sahip olmalıdır:

- Ad: `sql-vm-sub`
- Görüntü: SQL Server 2016 veya üzeri
- Kaynak grubu: yönetilen örnekle aynı
- Sanal ağ: `sql-vm-sub-vnet` 

SQL Server VM Azure 'a dağıtma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: oluşturma SQL Server VM](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md).

## <a name="4---configure-vpn-peering"></a>4-VPN eşlemesini yapılandırma
İki yönetilen örnek sanal ağ ve SQL Server sanal ağı arasında iletişimi etkinleştirmek için VPN eşlemesini yapılandırın. Bunu yapmak için şu PowerShell kod parçacığını kullanın:

```powershell-interactive
# Set variables
$SubscriptionId = '<SubscriptionID>'
$resourceGroup = 'SQLMI-Repl'
$pubvNet = 'sql-mi-publisher-vnet'
$subvNet = 'sql-vm-sub-vnet'
$pubsubName = 'Pub-to-Sub-Peer'
$subpubName = 'Sub-to-Pub-Peer'

$virtualNetwork1 = Get-AzVirtualNetwork `
  -ResourceGroupName $resourceGroup `
  -Name $pubvNet 

 $virtualNetwork2 = Get-AzVirtualNetwork `
  -ResourceGroupName $resourceGroup `
  -Name $subvNet  

# Configure VPN peering from publisher to subscriber
Add-AzVirtualNetworkPeering `
  -Name $pubsubName `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id

# Configure VPN peering from subscriber to publisher
Add-AzVirtualNetworkPeering `
  -Name $subpubName `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id

# Check status of peering on the publisher vNet; should say connected
Get-AzVirtualNetworkPeering `
 -ResourceGroupName $resourceGroup `
 -VirtualNetworkName $pubvNet `
 | Select PeeringState

# Check status of peering on the subscriber vNet; should say connected
Get-AzVirtualNetworkPeering `
 -ResourceGroupName $resourceGroup `
 -VirtualNetworkName $subvNet `
 | Select PeeringState

```

VPN eşlemesi kurulduktan sonra, SQL Server SQL Server Management Studio (SSMS) başlatarak ve her iki yönetilen örneğe bağlanarak bağlantıyı test edin. SSMS kullanarak yönetilen örneğe bağlanma hakkında daha fazla bilgi için, bkz. [SSMS kullanarak mı? sunucusuna bağlanma](sql-database-managed-instance-configure-p2s.md#use-ssms-to-connect-to-the-managed-instance). 

![Yönetilen örneklere yönelik bağlantıyı test etme](media/sql-database-managed-instance-configure-replication-tutorial/test-connectivity-to-mi.png)

## <a name="5---create-private-dns-zone"></a>5-özel DNS bölgesi oluşturma

Özel bir DNS bölgesi, yönetilen örnekler ve SQL Server arasında DNS yönlendirmeye izin verir. 

### <a name="create-private-dns-zone"></a>Özel DNS bölgesi oluştur
1. [Azure portal](https://portal.azure.com) oturum açın.
1. Yeni bir Azure kaynağı oluşturmak için **kaynak oluştur** ' u seçin. 
1. Azure Marketi 'nde `private dns zone` arayın. 
1. Microsoft tarafından yayımlanan **özel DNS bölge** kaynağını seçin ve ardından **Oluştur** ' u seçerek DNS bölgesini oluşturun. 
1. Açılan listeden abonelik ve kaynak grubunu seçin. 
1. DNS bölgeniz için `repldns.com`gibi rastgele bir ad sağlayın. 

   ![Özel DNS bölgesi oluştur](media/sql-database-managed-instance-configure-replication-tutorial/create-private-dns-zone.png)

1. **İncele ve oluştur**’u seçin. Özel DNS bölgeniz için parametreleri gözden geçirin ve ardından **Oluştur** ' u seçerek kaynağı oluşturun. 

### <a name="create-a-record"></a>Kayıt oluştur

1. Yeni **özel DNS bölgenize** gidin ve **genel bakış**' ı seçin. 
1. Yeni bir A-kaydı oluşturmak için **+ kayıt kümesi** seçin. 
1. SQL Server VM adının yanı sıra özel iç IP adresini de belirtin. 

   ![Kayıt yapılandırma](media/sql-database-managed-instance-configure-replication-tutorial/configure-a-record.png)

1. Bir kayıt oluşturmak için **Tamam ' ı** seçin. 

### <a name="link-the-virtual-network"></a>Sanal ağı bağlama

1. Yeni **özel DNS bölgenize** gidin ve **sanal ağ bağlantıları**' nı seçin. 
1. **+ Ekle** öğesini seçin. 
1. Bağlantı için `Pub-link`gibi bir ad girin. 
1. Açılır listeden aboneliğinizi seçin ve ardından yayımcı yönetilen örneğiniz için sanal ağı seçin. 
1. **Otomatik kaydolmayı etkinleştir ' in**yanındaki kutuyu işaretleyin. 

   ![VNet bağlantısı oluştur](media/sql-database-managed-instance-configure-replication-tutorial/configure-vnet-link.png)

1. Sanal ağınızı bağlamak için **Tamam ' ı** seçin. 
1. Abone sanal ağı için `Sub-link`gibi bir bağlantı eklemek için bu adımları tekrarlayın. 


## <a name="6---create-azure-storage-account"></a>6-Azure depolama hesabı oluşturma

Çalışma dizini için [bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) ve ardından depolama hesabı içinde bir [dosya paylaşma](../storage/files/storage-how-to-create-file-share.md) oluşturun. 

Dosya paylaşımının yolunu şu biçimde kopyalayın: `\\storage-account-name.file.core.windows.net\file-share-name`   

Örnek: `\\replstorage.file.core.windows.net\replshare`

Depolama erişim anahtarı bağlantı dizesini şu biçimde kopyalayın: `DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net`   

Örnek: `DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net`


Daha fazla bilgi için bkz. [depolama hesabı erişim anahtarlarını yönetme](../storage/common/storage-account-keys-manage.md). 


## <a name="7---create-a-database"></a>7-veritabanı oluşturma
Yayımcı mı üzerinde yeni bir veritabanı oluşturun. Bunu yapmak için aşağıdaki adımları izleyin:

1. SQL Server SQL Server Management Studio (SSMS) başlatın. 
1. `sql-mi-publisher` yönetilen örneğe bağlanın. 
1. Yeni bir **sorgu** penceresi açın ve veritabanını oluşturmak Için aşağıdaki T-SQL sorgusunu yürütün:

```sql
-- Create the databases
USE [master]
GO

-- Drop database if it exists
IF EXISTS (SELECT * FROM sys.sysdatabases WHERE name = 'ReplTutorial')
BEGIN
    DROP DATABASE ReplTutorial
END
GO

-- Create new database
CREATE DATABASE [ReplTutorial]
GO

-- Create table
USE [ReplTutorial]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO

-- Populate table with data
USE [ReplTutorial]
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

## <a name="8---configure-distribution"></a>8-dağıtımı yapılandırma 
Bağlantı kurulduktan sonra örnek bir veritabanınız varsa `sql-mi-distributor` yönetilen örnekte dağıtımı yapılandırabilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin:

1. SQL Server SQL Server Management Studio (SSMS) başlatın. 
1. `sql-mi-distributor` yönetilen örneğe bağlanın. 
1. Yeni bir **sorgu** penceresi açın ve dağıtıcı yönetilen örneğinde dağıtımı yapılandırmak Için aşağıdaki Transact-SQL kodunu çalıştırın: 

   ```sql
   EXEC sp_adddistpublisher @publisher = 'sql-mi-publisher.b6bf57.database.windows.net', -- primary publisher
        @distribution_db = N'distribution',
        @security_mode = 0,
        @login = N'azureuser',
        @password = N'<publisher_password>',
        @working_directory = N'\\replstorage.file.core.windows.net\replshare',
        @storage_connection_string = N'<storage_connection_string>'
        -- example: @storage_connection_string = N'DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net'

   ```

   > [!NOTE]
   > @working_directory parametresi için yalnızca ters eğik çizgi (`\`) kullandığınızdan emin olun. Eğik çizgi (`/`) kullanmak dosya paylaşımıyla bağlantı kurulurken hataya neden olabilir. 

1. `sql-mi-publisher` yönetilen örneğe bağlanın. 
1. Yeni bir **sorgu** penceresi açın ve dağıtıcıdan yayımcıya kaydetmek Için aşağıdaki Transact-SQL kodunu çalıştırın: 

```sql
Use MASTER
EXEC sys.sp_adddistributor @distributor = 'sql-mi-distributor.b6bf57.database.windows.net', @password = '<distributor_admin_password>' 
```


## <a name="9---create-the-publication"></a>9-yayını oluşturma
Dağıtım yapılandırıldıktan sonra yayını oluşturabilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin: 

1. SQL Server SQL Server Management Studio (SSMS) başlatın. 
1. `sql-mi-publisher` yönetilen örneğe bağlanın. 
1. **Nesne Gezgini**, **çoğaltma** düğümünü genişletin ve **Yerel yayın** klasörüne sağ tıklayın. **Yeni yayın seç...** . 
1. **İleri ' yi** seçerek hoş geldiniz sayfasının ötesine geçin. 
1. **Yayın veritabanı** sayfasında, daha önce oluşturduğunuz `ReplTutorial` veritabanını seçin. **İleri**’yi seçin. 
1. **Yayın türü** sayfasında **işlem yayını**' nı seçin. **İleri**’yi seçin. 
1. **Makaleler** sayfasında, **Tablolar**' ın yanındaki kutuyu işaretleyin. **İleri**’yi seçin. 
1. **Tablo satırlarını filtrele** sayfasında herhangi bir filtre eklemeden **İleri** ' yi seçin. 
1. Anlık görüntü **Aracısı** sayfasında, anlık görüntü oluştur ' un yanındaki kutuyu işaretleyin **ve abonelikleri başlatmak için anlık görüntüyü kullanılabilir tutun**. **İleri**’yi seçin. 
1. **Aracı Güvenlik** sayfasında güvenlik ayarları ' nı seçin **..** . Anlık görüntü Aracısı için kullanmak üzere SQL Server oturum açma kimlik bilgilerini sağlayın ve yayımcıya bağlanın. **Anlık görüntü Aracısı güvenlik** sayfasını kapatmak için **Tamam ' ı** seçin. **İleri**’yi seçin. 

   ![Snapshot Agent güvenliğini yapılandırma](media/sql-database-managed-instance-configure-replication-tutorial/snapshot-agent-security.png)

1. **Sihirbaz eylemleri** sayfasında, **yayını oluşturmayı** seçin ve (isteğe bağlı olarak) bu betiği daha sonra kaydetmek istiyorsanız **yayını oluşturma adımlarıyla bir betik dosyası oluşturmayı** seçin. 
1. **Sihirbazı tamamladıktan** sonra, yayınınızı oluşturmak için `ReplTest` yayınınızı adlandırın ve **İleri** ' yi seçin. 
1. Yayınınız oluşturulduktan sonra, **Nesne Gezgini** ' deki **çoğaltma** düğümünü yenileyin ve yeni yayınınızı görmek için **Yerel yayınlar** ' ı genişletin. 


## <a name="10---create-the-subscription"></a>10-abonelik oluşturma 

Yayın oluşturulduktan sonra, aboneliği oluşturabilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin: 

1. SQL Server SQL Server Management Studio (SSMS) başlatın. 
1. `sql-mi-publisher` yönetilen örneğe bağlanın. 
1. Yeni bir **sorgu** penceresi açın ve abonelik ve dağıtım aracısını eklemek Için aşağıdaki Transact-SQL kodunu çalıştırın. Abone adının bir parçası olarak DNS kullanın. 

```sql
use [ReplTutorial]
exec sp_addsubscription 
@publication = N'ReplTest',
@subscriber = N'sql-vm-sub.repldns.com', -- include the DNS configured in the private DNS zone
@destination_db = N'ReplSub', 
@subscription_type = N'Push', 
@sync_type = N'automatic', 
@article = N'all', 
@update_mode = N'read only', 
@subscriber_type = 0

exec sp_addpushsubscription_agent
@publication = N'ReplTest',
@subscriber = N'sql-vm-sub.repldns.com', -- include the DNS configured in the private DNS zone
@subscriber_db = N'ReplSub', 
@job_login = N'azureuser', 
@job_password = '<Complex Password>', 
@subscriber_security_mode = 0, 
@subscriber_login = N'azureuser', 
@subscriber_password = '<Complex Password>', 
@dts_package_location = N'Distributor'
GO
```

## <a name="11---test-replication"></a>11-test çoğaltma 

Çoğaltma yapılandırıldıktan sonra, yayımcıya yeni öğeler ekleyerek ve abonelere yayan değişiklikleri izleyerek test edebilirsiniz. 

Abone üzerindeki satırları görüntülemek için aşağıdaki T-SQL kod parçacığını çalıştırın:

```sql
Use ReplSub
select * from dbo.ReplTest
```

Yayımcıya ek satırlar eklemek için aşağıdaki T-SQL kod parçacığını çalıştırın ve ardından abonedeki satırları yeniden kontrol edin. 

```sql
Use ReplTutorial
INSERT INTO ReplTest (ID, c1) VALUES (15, 'pub')
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. [Azure Portal](https://portal.azure.com)kaynak grubunuza gidin. 
1. Yönetilen örnekleri seçin ve ardından **Sil**' i seçin. Kaynağı silmek istediğinizi onaylamak için metin kutusuna `yes` yazın ve ardından **Sil**' i seçin. Bu işlemin tamamlanması biraz zaman alabilir ve bu işlem "tamamlanana kadar" *sanal kümeyi* veya diğer bağımlı kaynakları silememeyeceksiniz. Yönetilen örneğinizin silindiğini onaylamak için etkinlik sekmesindeki silmeyi izleyin. 
1. Yönetilen örnek silindikten sonra, *sanal kümeyi* kaynak grubunuzda seçip **Sil**' i seçerek silin. Kaynağı silmek istediğinizi onaylamak için metin kutusuna `yes` yazın ve ardından **Sil**' i seçin. 
1. Kalan kaynakları silin. Kaynağı silmek istediğinizi onaylamak için metin kutusuna `yes` yazın ve ardından **Sil**' i seçin. 
1. Kaynak grubunu Sil ' i seçerek kaynak grubunu **Sil**' i seçin, kaynak grubunun adını yazın, `myResourceGroup`ve ardından **Sil**' i seçin. 

## <a name="known-errors"></a>Bilinen hatalar

### <a name="windows-logins-are-not-supported"></a>Windows oturumu açma desteklenmiyor

`Exception Message: Windows logins are not supported in this version of SQL Server.`

Aracı bir Windows oturum açma ile yapılandırılmış ve bunun yerine SQL Server bir oturum açma kullanması gerekiyor. SQL Server oturum açma kimlik bilgilerini değiştirmek için **yayın özelliklerinin** **Aracı Güvenlik** sayfasını kullanın. 


### <a name="failed-to-connect-to-azure-storage"></a>Azure depolama ile bağlantı kurulamadı


`Connecting to Azure Files Storage '\\replstorage.file.core.windows.net\replshare' Failed to connect to Azure Storage '' with OS error: 53.`

2019-11-19 02:21:05.07 for replstorage için Azure Storage bağlantı dizesi alındı 2019-11-19 02:21:05.07 Azure dosya depolama 'ya bağlanma '\\replstorage. File. Core. Windows. net\replshare ' 2019-11-19 02:21:31.21, ' ' adlı Azure depolama hizmetine şu işletim sistemi hatası: 53.


Bunun nedeni, bağlantı noktası 445 ' nin Azure Güvenlik Duvarı, Windows güvenlik duvarı veya her ikisinde de kapanmasından kaynaklanıyor olabilir. 

`Connecting to Azure Files Storage '\\replstorage.file.core.windows.net\replshare' Failed to connect to Azure Storage '' with OS error: 55.`

Dosya paylaşımının dosya yolunda ters eğik çizgi yerine eğik çizgi kullanmak bu hataya neden olabilir. Bu sorun normaldir: `\\replstorage.file.core.windows.net\replshare` bir OS 55 hatasına neden olabilir: `'\\replstorage.file.core.windows.net/replshare'`

### <a name="could-not-connect-to-subscriber"></a>Abone 'e bağlanılamadı

`The process could not connect to Subscriber 'SQL-VM-SUB`
`Could not open a connection to SQL Server [53].`
`A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections.`

Olası çözümler:
- 1433 bağlantı noktasının açık olduğundan emin olun. 
- Abone üzerinde TCP/IP 'nin etkinleştirildiğinden emin olun. 
- Abone oluşturulurken kullanılan DNS adını doğrulayın. 
- Sanal ağlarınızın özel DNS bölgesinde doğru şekilde bağlandığını doğrulayın. 
- A kaydlarınızın doğru şekilde yapılandırıldığını doğrulayın. 
- VPN eşlerinizin doğru şekilde yapılandırıldığını doğrulayın. 

### <a name="no-publications-to-which-you-can-subscribe"></a>Abone olabileceğiniz yayınlar yok

**Yeni** abonelik Sihirbazı 'nı kullanarak yeni bir abonelik eklerken, **yayın** sayfasında, kullanılabilir seçenekler olarak listelenen veritabanı ve yayınlar olmadığını fark edebilirsiniz ve aşağıdaki hata iletisini görebilirsiniz:

`There are no publications to which you can subscribe, either because this server has no publications or because you do not have sufficient privileges to access the publications.`
 
Bu hata iletisi doğru olsa da, bağlandığınız yayımcıdaki yayınlar gerçekten mevcut olmadığından veya yeterli izinlere sahip olmadığınızdan, bu hataya SQL Server Management Studio eski bir sürümü de neden olabilir. Bu soruna bir kök neden olarak kural yapmak için SQL Server Management Studio 18,0 veya üstünü yükseltmeyi deneyin. 


## <a name="next-steps"></a>Sonraki adımlar

### <a name="enable-security-features"></a>Güvenlik özelliklerini etkinleştir

Veritabanınızı güvenli hale getirme yöntemlerinin kapsamlı bir listesi için aşağıdaki [yönetilen örnek özellikleri güvenlik özellikleri](sql-database-managed-instance.md#azure-sql-database-security-features) makalesine bakın. Aşağıdaki güvenlik özellikleri ele alınmıştır:

- [Yönetilen örnek denetimi](sql-database-managed-instance-auditing.md) 
- [Her zaman şifreli](/sql/relational-databases/security/encryption/always-encrypted-database-engine)
- [Tehdit algılama](sql-database-managed-instance-threat-detection.md) 
- [Dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) 
- [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)

### <a name="managed-instance-capabilities"></a>Yönetilen örnek özellikleri

Yönetilen örnek özelliklerine yönelik kapsamlı bir genel bakış için bkz.:

> [!div class="nextstepaction"]
> [Yönetilen örnek özellikleri](sql-database-managed-instance.md)
