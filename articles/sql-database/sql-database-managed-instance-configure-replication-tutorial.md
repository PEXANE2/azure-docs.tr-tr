---
title: Yönetilen iki örnek ve SQL Server arasında işlem çoğaltma yapılandırma
description: Publisher yönetilen örneği, Dağıtıcı yönetilen örneği ve Azure VM'deki SQL Server abonesi ile özel DNS bölgesi ve VPN eşleme gibi gerekli ağ bileşenleri arasında çoğaltmayı yapılandıran bir öğretici.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 11/21/2019
ms.openlocfilehash: fa6e393500e9deeb91ee84aa5255320003817f08
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76719900"
---
# <a name="tutorial-configure-transactional-replication-between-two-managed-instances-and-sql-server"></a>Öğretici: Yönetilen iki örnek ve SQL Server arasında işlem çoğaltma yapılandırma


Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Yönetilen Örneği çoğaltma Yayımcısı olarak yapılandırın. 
> - Yönetilen Örneği çoğaltma Dağıtıcısı olarak yapılandırın. 
> - Bir SQL Server'ı abone olarak yapılandırın. 

![SQL MI pub, SQL MI Dist ve SQL Server alt arasında çoğaltma](media/sql-database-managed-instance-failover-group-tutorial/sqlmi-to-sql-replication.png)

Bu öğretici deneyimli bir hedef kitleye yöneliktir ve kullanıcının azure içindeki her iki yönetilen örneği ve SQL Server VM'leri dağıtmaya ve bunlara bağlanmaya aşina olduğunu varsayar. Bu nedenle, bu öğreticibazı adımlar üzerinde parlak. 

Daha fazla bilgi edinmek için [Azure SQL Veritabanı yönetilen örnek genel bakış,](sql-database-managed-instance-index.yml) [özellikler](sql-database-managed-instance.md)ve [SQL İşlemel Çoğaltma](sql-database-managed-instance-transactional-replication.md) makalelerine bakın.

Yönetilen örnek yayımcı ile yönetilen örnek abone arasında çoğaltma yapılandırmak için [bkz.](replication-with-sql-database-managed-instance.md) 

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlamak için aşağıdaki ön koşullara sahip olduğunuzdan emin olun:

- [Azure aboneliği.](https://azure.microsoft.com/free/) 
- Aynı sanal ağ içinde yönetilen iki örneği dağıtma deneyimi. 
- Bir SQL Server abonesi, şirket içi veya Azure VM. Bu öğretici bir Azure VM kullanır.  
- [SQL Server Management Studio (SSMS) 18.0 veya üzeri.](/sql/ssms/download-sql-server-management-studio-ssms)
- [Azure Powershell'in](/powershell/azure/install-az-ps?view=azps-1.7.0)en son sürümü.
- Bağlantı noktası 445 ve 1433, hem Azure Güvenlik Duvarı'nda hem de Windows Güvenlik Duvarı'nda SQL trafiğine izin verir. 

## <a name="1---create-the-resource-group"></a>1 - Kaynak grubunu oluşturma
Yeni bir kaynak grubu oluşturmak için aşağıdaki PowerShell kod parçacıklarını kullanın:

```powershell-interactive
# set variables
$ResourceGroupName = "SQLMI-Repl"
$Location = "East US 2"

# Create a new resource group
New-AzResourceGroup -Name  $ResourceGroupName -Location $Location
```

## <a name="2---create-two-managed-instances"></a>2 - Yönetilen iki örnek oluşturma
[Azure portalını](https://portal.azure.com)kullanarak bu yeni kaynak grubunda yönetilen iki örnek oluşturun. 

- Yayımcı yönetilen örnek adı olmalıdır: `sql-mi-publisher` (rasgeleleştirme için birkaç karakter ile birlikte) ve sanal `vnet-sql-mi-publisher`ağ adı olmalıdır .
- Dağıtıcı yönetilen örneğin adı olmalıdır: `sql-mi-distributor` (rasgeleleştirme için birkaç karakter ile birlikte) ve _yayıncı yönetilen örnek olarak aynı sanal ağda_olmalıdır.

   ![Distribütör için yayıncı vnet kullanın](media/sql-database-managed-instance-configure-replication-tutorial/use-same-vnet-for-distributor.png)

Yönetilen bir örnek oluşturma hakkında daha fazla bilgi için [bkz.](sql-database-managed-instance-get-started.md)

  > [!NOTE]
  > Basitlik uğruna ve en yaygın yapılandırma olduğundan, bu öğretici yayıncı ile aynı sanal ağ içinde dağıtıcı yönetilen örnek yerleştirerek önerir. Ancak, ayrı bir sanal ağda distribütör oluşturmak mümkündür. Bunu yapmak için, yayıncı ve dağıtıcı sanal ağlar arasında VPN bakan yapılandırmak ve daha sonra distribütör ve abonesanal ağlar arasında VPN bakan yapılandırmak gerekir. 

## <a name="3---create-a-sql-server-vm"></a>3 - SQL Server VM oluşturma
[Azure portalını](https://portal.azure.com)kullanarak bir SQL Server sanal makinesi oluşturun. SQL Server sanal makinesi aşağıdaki özelliklere sahip olmalıdır:

- Adı:`sql-vm-sub`
- Resim: SQL Server 2016 veya üzeri
- Kaynak grubu: yönetilen örnekle aynı
- Sanal ağ:`sql-vm-sub-vnet` 

SQL Server VM'yi Azure'a dağıtma hakkında daha fazla bilgi için [Bkz. Quickstart: Create SQL Server VM](../virtual-machines/windows/sql/quickstart-sql-vm-create-portal.md).

## <a name="4---configure-vpn-peering"></a>4 - VPN eşleme sini yapılandırın
Yönetilen iki örneğin sanal ağı ile SQL Server'ın sanal ağı arasındaki iletişimi etkinleştirecek şekilde VPN eşlemesini yapılandırın. Bunu yapmak için bu PowerShell kod parçacıklarını kullanın:

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

VPN eşleme kurulduktan sonra, SQL Server'ınızda SQL Server Management Studio'ya (SSMS) başlatArak ve yönetilen her iki örne de bağlanarak bağlantıyı test edin. SSMS kullanarak yönetilen bir örne bağlanma hakkında daha fazla bilgi için [MI'ye bağlanmak için SSMS'i kullan'a](sql-database-managed-instance-configure-p2s.md#use-ssms-to-connect-to-the-managed-instance)bakın. 

![Yönetilen örneklere bağlantı testi](media/sql-database-managed-instance-configure-replication-tutorial/test-connectivity-to-mi.png)

## <a name="5---create-private-dns-zone"></a>5 - Özel DNS bölgesi oluşturma

Özel bir DNS bölgesi, yönetilen örnekler le SQL Server arasında DNS yönlendirmesine olanak tanır. 

### <a name="create-private-dns-zone"></a>Özel DNS Bölgesi oluşturma
1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Yeni bir Azure kaynağı oluşturmak için **kaynak oluştur'u** seçin. 
1. Azure `private dns zone` Marketi'nde arama yapın. 
1. Microsoft tarafından yayımlanan **Özel DNS bölge** kaynağını seçin ve ardından DNS bölgesini oluşturmak için **Oluştur'u** seçin. 
1. Açılan sistemden abonelik ve kaynak grubunu seçin. 
1. DNS bölgeniz için `repldns.com`'. 

   ![Özel DNS bölgesi oluşturma](media/sql-database-managed-instance-configure-replication-tutorial/create-private-dns-zone.png)

1. **İncele ve oluştur**’u seçin. Özel DNS bölgenizin parametrelerini gözden geçirin ve ardından kaynağınızı oluşturmak için **Oluştur'u** seçin. 

### <a name="create-a-record"></a>Kayıt Oluşturma

1. Yeni Özel **DNS bölgenize** gidin ve **Genel Bakış'ı**seçin. 
1. Yeni bir A Kaydı oluşturmak için **ayaryı +** Kaydet'i seçin. 
1. SQL Server VM'nizin adını ve özel iç IP adresini sağlayın. 

   ![A kaydını yapılandırma](media/sql-database-managed-instance-configure-replication-tutorial/configure-a-record.png)

1. A kaydını oluşturmak için **Tamam'ı** seçin. 

### <a name="link-the-virtual-network"></a>Sanal ağı birbirine bağla

1. Yeni Özel **DNS bölgenize** gidin ve **Sanal ağ bağlantılarını**seçin. 
1. **+ Ekle** öğesini seçin. 
1. Bağlantı için bir ad sağlayın, örneğin. `Pub-link` 
1. Açılan abonelikten aboneliğinizi seçin ve ardından yayımcı yönetilen örneğiniz için sanal ağı seçin. 
1. **Otomatik kaydı etkinleştirmek**için yanındaki kutuyu işaretleyin. 

   ![vnet bağlantısı oluşturma](media/sql-database-managed-instance-configure-replication-tutorial/configure-vnet-link.png)

1. Sanal ağınızı bağlamak için **Tamam'ı** seçin. 
1. Abone nin sanal ağı için bir bağlantı eklemek için bu `Sub-link`adımları yineleyin. 


## <a name="6---create-azure-storage-account"></a>6 - Azure Depolama Hesabı Oluşturma

Çalışma dizini için [bir Azure Depolama Hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) ve ardından depolama hesabı içinde bir dosya [paylaşımı](../storage/files/storage-how-to-create-file-share.md) oluşturun. 

Dosya paylaşım yolunu aşağıdaki biçiminde kopyalayın:`\\storage-account-name.file.core.windows.net\file-share-name`   

Örnek: `\\replstorage.file.core.windows.net\replshare`

Depolama erişim anahtarı bağlantı dizesini aşağıdaki biçimde kopyalayın:`DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net`   

Örnek: `DefaultEndpointsProtocol=https;AccountName=replstorage;AccountKey=dYT5hHZVu9aTgIteGfpYE64cfis0mpKTmmc8+EP53GxuRg6TCwe5eTYWrQM4AmQSG5lb3OBskhg==;EndpointSuffix=core.windows.net`


Daha fazla bilgi için [bkz.](../storage/common/storage-account-keys-manage.md) 


## <a name="7---create-a-database"></a>7 - Veritabanı oluşturma
Yayımcı MI'de yeni bir veritabanı oluşturun. Bunu yapmak için şu adımları uygulayın:

1. SQL Server'ınızda SQL Server Management Studio'u (SSMS) başlatın. 
1. Yönetilen örne bağlanın. `sql-mi-publisher` 
1. Yeni **Sorgu** penceresini açın ve veritabanını oluşturmak için aşağıdaki T-SQL sorgusunu çalıştırın:

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

## <a name="8---configure-distribution"></a>8 - Yapıdağılımı 
Bağlantı kurulduktan ve örnek bir veritabanına sahip olduktan sonra, `sql-mi-distributor` yönetilen örneğiniz üzerinde dağıtımı yapılandırabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. SQL Server'ınızda SQL Server Management Studio'u (SSMS) başlatın. 
1. Yönetilen örne bağlanın. `sql-mi-distributor` 
1. **Yeni** Sorgu penceresi açın ve dağıtıcı yönetilen örnekte dağıtımı yapılandırmak için aşağıdaki Transact-SQL kodunu çalıştırın: 

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
   > Parametre için yalnızca ters`\`eğik @working_directory çizgi ( ) kullandığınızdan emin olun. İleri eğik`/`çizgi () kullanmak, dosya paylaşımına bağlanırken hataya neden olabilir. 

1. Yönetilen örne bağlanın. `sql-mi-publisher` 
1. **Yeni** Sorgu penceresi açın ve yayımcıda dağıtıcıyı kaydetmek için aşağıdaki Transact-SQL kodunu çalıştırın: 

```sql
Use MASTER
EXEC sys.sp_adddistributor @distributor = 'sql-mi-distributor.b6bf57.database.windows.net', @password = '<distributor_admin_password>' 
```


## <a name="9---create-the-publication"></a>9 - Yayını oluşturma
Dağıtım yapılandırıldıktan sonra, artık yayını oluşturabilirsiniz. Bunu yapmak için şu adımları uygulayın: 

1. SQL Server'ınızda SQL Server Management Studio'u (SSMS) başlatın. 
1. Yönetilen örne bağlanın. `sql-mi-publisher` 
1. **Object Explorer'da** **Çoğaltma** düğümünü genişletin ve **Yerel Yayın** klasörünü sağ tıklatın. **Yeni Yayın'ı seçin...**. 
1. Karşılama sayfasını geçmek için **İleri'yi** seçin. 
1. Yayın **Veritabanı** sayfasında, daha `ReplTutorial` önce oluşturduğunuz veritabanını seçin. **Sonraki'ni**seçin. 
1. Yayın **türü** **sayfasında, İşlemsel yayını**seçin. **Sonraki'ni**seçin. 
1. **Makaleler** sayfasında, **Tablolar'ın**yanındaki kutuyu işaretleyin. **Sonraki'ni**seçin. 
1. Filtre **Tablosu Satırları** sayfasında, filtre eklemeden **İleri'yi** seçin. 
1. Anlık **Görüntü Aracısı** sayfasında, anlık görüntü oluşturmanın yanındaki kutuyu hemen işaretleyin **ve anlık görüntüleri abonelikleri başlatmayı kullanılabilir tutun.** **Sonraki'ni**seçin. 
1. Aracı **Güvenlik** sayfasında **Güvenlik Ayarları'nı seçin...** Anlık Görüntü aracısı için kullanmak ve Publisher'a bağlanmak için SQL Server giriş kimlik bilgilerini sağlayın. **Anlık Görüntü Aracısı Güvenlik** sayfasını kapatmak için **Tamam'ı** seçin. **Sonraki'ni**seçin. 

   ![Anlık Görüntü Aracısı güvenliğini yapılandırma](media/sql-database-managed-instance-configure-replication-tutorial/snapshot-agent-security.png)

1. Sihirbaz **Eylemleri** sayfasında, **yayını Oluşturma'yı** seçin ve (isteğe bağlı olarak) bu komut dosyasını daha sonra kaydetmek istiyorsanız **yayını oluşturmak için adımları içeren bir komut dosyası dosyası oluşturmayı** seçin. 
1. **Sihirbazı Tamamla** sayfasında, yayınınızı `ReplTest` adlandırın ve yayınınızı oluşturmak için **İleri'yi** seçin. 
1. Yayınınız oluşturulduktan sonra Nesne **Gezgini'ndeki** **Çoğaltma** düğümunu yenileyin ve yeni yayınınızı görmek için **Yerel Yayınları** genişletin. 


## <a name="10---create-the-subscription"></a>10 - Aboneliği oluştur 

Yayın oluşturulduktan sonra aboneliği oluşturabilirsiniz. Bunu yapmak için şu adımları uygulayın: 

1. SQL Server'ınızda SQL Server Management Studio'u (SSMS) başlatın. 
1. Yönetilen örne bağlanın. `sql-mi-publisher` 
1. Yeni **Sorgu** penceresini açın ve abonelik ve dağıtım aracısını eklemek için aşağıdaki Transact-SQL kodunu çalıştırın. Abone adının bir parçası olarak DNS'yi kullanın. 

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

## <a name="11---test-replication"></a>11 - Test çoğaltma 

Çoğaltma yapılandırıldıktan sonra, yayımcıya yeni öğeler ekleyerek ve değişikliklerin aboneye yayılmasını izleyerek bunu sınayabilirsiniz. 

Abonedeki satırları görüntülemek için aşağıdaki T-SQL snippet'i çalıştırın:

```sql
Use ReplSub
select * from dbo.ReplTest
```

Yayımcıya ek satırlar eklemek için aşağıdaki T-SQL snippet'i çalıştırın ve ardından abonelerdeki satırları yeniden denetleyin. 

```sql
Use ReplTutorial
INSERT INTO ReplTest (ID, c1) VALUES (15, 'pub')
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. [Azure portalındaki](https://portal.azure.com)kaynak grubunuza gidin. 
1. Yönetilen örnek(ler)'i seçin ve sonra **Sil'i**seçin. Kaynağı `yes` silmek istediğinizi doğrulamak için metin kutusuna yazın ve sonra **Sil'i**seçin. Bu işlemin arka planda tamamlanması biraz zaman alabilir ve tamamlanana kadar *Sanal kümeyi* veya diğer bağımlı kaynakları silemezsiniz. Yönetilen örneğinizin silindiğini doğrulamak için Etkinlik sekmesindeki silmeyi izleyin. 
1. Yönetilen örnek silindikten sonra, kaynak grubunuzda seçerek *Sanal kümeyi* silin ve sonra **Sil'i**seçin. Kaynağı `yes` silmek istediğinizi doğrulamak için metin kutusuna yazın ve sonra **Sil'i**seçin. 
1. Kalan kaynakları silin. Kaynağı `yes` silmek istediğinizi doğrulamak için metin kutusuna yazın ve sonra **Sil'i**seçin. 
1. **Kaynak grubunu sil'i**seçerek kaynak grubunu sil, kaynak `myResourceGroup`grubunun adını yazarak ve sonra **Sil'i**seçerek kaynak grubunu silin. 

## <a name="known-errors"></a>Bilinen hatalar

### <a name="windows-logins-are-not-supported"></a>Windows oturum açmaları desteklenmiyor

`Exception Message: Windows logins are not supported in this version of SQL Server.`

Aracı bir Windows girişi ile yapılandırıldı ve bunun yerine bir SQL Server girişi kullanması gerekiyor. Giriş kimlik bilgilerini SQL Server girişiyle değiştirmek için **Yayın özelliklerinin** **Agent Security** sayfasını kullanın. 


### <a name="failed-to-connect-to-azure-storage"></a>Azure Depolama'ya bağlanılamamış


`Connecting to Azure Files Storage '\\replstorage.file.core.windows.net\replshare' Failed to connect to Azure Storage '' with OS error: 53.`

2019-11-19 02:21:05.07 Yeniden depolama için Elde Edilen Azure Depolama Bağlantı Dizesi 2019-11-19 02:21:05.07 Azure Dosyalarına Bağlanma Depolama '\\replstorage.file.core.windows.net\replshare' 2019-11-19 02:21:31.21 İşletim Sistemi hatası ile Azure Depolama '' bağlanmak için başarısız oldu: 53.


Bunun nedeni, bağlantı noktası 445'in Azure güvenlik duvarında, Windows güvenlik duvarında veya her ikisinde de kapalı olmasıdır. 

`Connecting to Azure Files Storage '\\replstorage.file.core.windows.net\replshare' Failed to connect to Azure Storage '' with OS error: 55.`

Dosya paylaşımı için dosya yolunda ters eğik çizgi yerine ileri eğik çizgi kullanmak bu hataya neden olabilir. Bu tamam: `\\replstorage.file.core.windows.net\replshare` Bu bir işletim sistemi 55 hatasına neden olabilir:`'\\replstorage.file.core.windows.net/replshare'`

### <a name="could-not-connect-to-subscriber"></a>Aboneye bağlanamadı

`The process could not connect to Subscriber 'SQL-VM-SUB`
`Could not open a connection to SQL Server [53].`
`A network-related or instance-specific error has occurred while establishing a connection to SQL Server. Server is not found or not accessible. Check if instance name is correct and if SQL Server is configured to allow remote connections.`

Olası çözümler:
- 1433 portunaçık olduğundan emin olun. 
- Abonede TCP/IP etkin olduğundan emin olun. 
- Abone oluşturulurken DNS adının kullanıldığını doğrulayın. 
- Sanal ağlarınızın özel DNS bölgesinde doğru şekilde bağlandığını doğrulayın. 
- A kaydınızın doğru şekilde yapılandırıldığından doğrulayın. 
- VPN eşlemenizin doğru şekilde yapılandırıldığından doğrulayın. 

### <a name="no-publications-to-which-you-can-subscribe"></a>Abone olabileceğiniz yayın yok

**Yayın** sayfasında **Yeni Abonelik** sihirbazını kullanarak yeni bir abonelik eklerken, kullanılabilir seçenekler olarak listelenen veritabanları ve yayınlar olmadığını görebilirsiniz ve aşağıdaki hata iletisini görebilirsiniz:

`There are no publications to which you can subscribe, either because this server has no publications or because you do not have sufficient privileges to access the publications.`
 
Bu hata iletisinin doğru olması ve bağlı olduğunuz yayımcıda gerçekten kullanılabilir yayınlar olmaması veya yeterli izinlerin olmaması mümkün olsa da, bu hata nın nedeni SQL Server Management Studio'nun eski bir sürümü de olabilir. Bunu temel neden olarak ekarte etmek için SQL Server Management Studio 18.0 veya daha büyük bir yükseltmeyi deneyin. 


## <a name="next-steps"></a>Sonraki adımlar

### <a name="enable-security-features"></a>Güvenlik özelliklerini etkinleştirme

Veritabanınızı korumanın kapsamlı bir listesi için aşağıdaki [yönetilen örnek özellikleri güvenlik özellikleri](sql-database-managed-instance.md#azure-sql-database-security-features) makalesine bakın. Aşağıdaki güvenlik özellikleri tartışılır:

- [Yönetilen örnek denetimi](sql-database-managed-instance-auditing.md) 
- [Her zaman şifrelenir](/sql/relational-databases/security/encryption/always-encrypted-database-engine)
- [Tehdit algılama](sql-database-managed-instance-threat-detection.md) 
- [Dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) 
- [Saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)

### <a name="managed-instance-capabilities"></a>Yönetilen örnek özellikleri

Yönetilen örnek özelliklerine tam bir genel bakış için bkz:

> [!div class="nextstepaction"]
> [Yönetilen örnek özellikleri](sql-database-managed-instance.md)
