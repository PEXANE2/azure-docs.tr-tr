---
title: XEvent Olay Dosya kodu
description: Azure SQL Veritabanı'nda genişletilmiş bir olayda Olay Dosyası hedefini gösteren iki aşamalı bir kod örneği için PowerShell ve Transact-SQL sağlar. Azure Depolama bu senaryonun gerekli bir parçasıdır.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: jrasnik
ms.date: 03/12/2019
ms.openlocfilehash: a9bf28fb1b3c5278d25b417fc646d2ad3d6f1abc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79213985"
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>SQL Veritabanı'ndaki genişletilmiş olaylar için Olay Dosyası hedef kodu

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Genişletilmiş bir olay için bilgileri yakalamanın ve bildirmenin sağlam bir yolu için tam bir kod örneği istiyorsunuz.

Microsoft SQL Server'da [Olay Dosyası hedefi,](https://msdn.microsoft.com/library/ff878115.aspx) olay çıktılarını yerel bir sabit disk dosyasında depolamak için kullanılır. Ancak bu tür dosyalar Azure SQL Veritabanı'nda kullanılamaz. Bunun yerine, Olay Dosyası hedefini desteklemek için Azure Depolama hizmetini kullanırız.

Bu konu iki aşamalı bir kod örneği sunar:

- PowerShell, bulutta bir Azure Depolama kapsayıcısı oluşturmak için.
- Transact-SQL:
  
  - Azure Depolama kapsayıcısını bir Olay Dosyası hedefine atamak için.
  - Olay oturumunu oluşturmak ve başlatmak için.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

- Bir Azure hesabı ve aboneliği [Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.
- Tablo oluşturabileceğiniz herhangi bir veritabanı.
  
  - İsteğe bağlı olarak birkaç dakika içinde [bir **AdventureWorksLT** gösteri veritabanı oluşturabilirsiniz.](sql-database-get-started.md)

- SQL Server Management Studio (ssms.exe), ideal olarak en son aylık güncelleme sürümü.
  En son ssms.exe indirebilirsiniz:
  
  - Konu [Başlıklı SQL Server Management Studio İndir](https://msdn.microsoft.com/library/mt238290.aspx).
  - [İndirmek için doğrudan bir bağlantı.](https://go.microsoft.com/fwlink/?linkid=616025)

- [Azure PowerShell modüllerini](https://go.microsoft.com/?linkid=9811175) yüklemiş olmalısınız.

  - Modüller gibi komutlar sağlar - **New-AzStorageAccount**.

## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Aşama 1: Azure Depolama kapsayıcısı için PowerShell kodu

Bu PowerShell, iki aşamalı kod örneğinin 1.

Komut dosyası, olası bir önceki çalıştırmadan sonra temizlemek için komutlarla başlar ve yeniden çalıştırılabilir.

1. PowerShell komut dosyasını Notepad.exe gibi basit bir metin düzenleyicisine yapıştırın ve komut dosyasını uzantısı **.ps1**olan bir dosya olarak kaydedin.
2. PowerShell ISE'yi Yönetici olarak başlatın.
3. İsteyiş sırasında,<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>ve sonra Enter tuşuna basın.
4. PowerShell ISE'de **.ps1** dosyanızı açın. Betiği çalıştırın.
5. Komut dosyası ilk olarak Azure'da oturum açabileceğiniz yeni bir pencere başlatır.

   - Oturumunuzu kesintiye uğratmadan komut dosyasını yeniden çalıştırıyorsanız, **Azure Hesabı Ekle** komutunu açıklama seçeneğine sahip olursunuz.

![PowerShell ISE, Azure modülü yüklü, komut dosyası çalıştırmaya hazır.][30_powershell_ise]

### <a name="powershell-code"></a>PowerShell kodu

Bu PowerShell komut dosyası, Az modüllerini yüklediğinizi varsayar. Daha fazla bilgi için Azure [PowerShell modüllerini yükleyin.](/powershell/azure/install-Az-ps)

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

cls;

#--------------- 1 -----------------------

'Script assumes you have already logged your PowerShell session into Azure.
But if not, run  Connect-AzAccount (or  Connect-AzAccount), just one time.';
#Connect-AzAccount;   # Same as  Connect-AzAccount.

#-------------- 2 ------------------------

'
TODO: Edit the values assigned to these variables, especially the first few!
';

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME';
$resourceGroupName   = 'YOUR_RESOURCE-GROUP-NAME';

$policySasExpiryTime = '2018-08-28T23:44:56Z';
$policySasStartTime  = '2017-10-01';

$storageAccountLocation = 'YOUR_STORAGE_ACCOUNT_LOCATION';
$storageAccountName     = 'YOUR_STORAGE_ACCOUNT_NAME';
$contextName            = 'YOUR_CONTEXT_NAME';
$containerName          = 'YOUR_CONTAINER_NAME';
$policySasToken         = ' ? ';

$policySasPermission = 'rwl';  # Leave this value alone, as 'rwl'.

#--------------- 3 -----------------------

# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.';

Select-AzSubscription -Subscription $subscriptionName;

#-------------- 4 ------------------------

'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.';

if ($storageAccountName) {
    Remove-AzStorageAccount `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName;
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString();

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...';

New-AzStorageAccount `
    -Name              $storageAccountName `
    -Location          $storageAccountLocation `
    -ResourceGroupName $resourceGroupName `
    -SkuName           'Standard_LRS';

[System.DateTime]::Now.ToString();
[System.Media.SystemSounds]::Beep.Play();

'
Get the access key for your storage account.
';

$accessKey_ForStorageAccount = `
    (Get-AzStorageAccountKey `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName
        ).Value[0];

"`$accessKey_ForStorageAccount = $accessKey_ForStorageAccount";

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
';

#--------------- 6 -----------------------

# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
';

$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $accessKey_ForStorageAccount;

'Create a container within the storage account.
';

$containerObjectInStorageAccount = New-AzStorageContainer `
    -Name    $containerName `
    -Context $context;

'Create a security policy to be applied to the SAS token.
';

New-AzStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime;

'
Generate a SAS token for the container.
';
try {
    $sasTokenWithPolicy = New-AzStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken;
}
catch {
    $Error[0].Exception.ToString();
}

#-------------- 7 ------------------------

'Display the values that YOU must edit into the Transact-SQL script next!:
';

"storageAccountName: $storageAccountName";
"containerName:      $containerName";
"sasTokenWithPolicy: $sasTokenWithPolicy";

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
';

'
(Later, return here to delete your Azure Storage account. See the preceding  Remove-AzStorageAccount -Name $storageAccountName)';

'
Now shift to the Transact-SQL portion of the two-part code sample!';

# EOFile
```

PowerShell komut dosyasının sona erdiğinde yazdırdığı birkaç adlandırılmış değere dikkat edin. Bu değerleri, 2.

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Aşama 2: Azure Depolama kapsayıcısı kullanan Transact-SQL kodu

- Bu kod örneğinin 1.
- Sonraki aşama 2, aşağıdaki Transact-SQL komut dosyası kapsayıcı kullanmanız gerekir.

Komut dosyası, olası bir önceki çalıştırmadan sonra temizlemek için komutlarla başlar ve yeniden çalıştırılabilir.

PowerShell komut dosyası sona erdiğinde birkaç adlandırılmış değer yazdırdı. Bu değerleri kullanmak için Transact-SQL komut dosyasını yeniden etkinleştirmeniz gerekir. İşlem noktalarını bulmak için Transact-SQL komut dosyasında **TODO'yu** bulun.

1. SQL Server Management Studio 'yı açın (ssms.exe).
2. Azure SQL Veritabanı veritabanınıza bağlanın.
3. Yeni bir sorgu bölmesi açmak için tıklatın.
4. Aşağıdaki İşlem-SQL komut dosyasını sorgu bölmesine yapıştırın.
5. Komut dosyasındaki tüm **TODO'ları** bulun ve uygun yapılantılar yapın.
6. Kaydet ve sonra komut dosyasını çalıştırın.

> [!WARNING]
> Önceki PowerShell komut dosyası tarafından oluşturulan SAS anahtar değeri bir '?' ile başlayabilir (soru işareti). Aşağıdaki T-SQL komut dosyasında SAS tuşunu kullandığınızda, *satır aralığını '?' kaldırmanız*gerekir. Aksi takdirde çabalarınız güvenlik tarafından engellenebilir.

### <a name="transact-sql-code"></a>Transact-SQL kodu

```sql
---- TODO: First, run the earlier PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.

SET NOCOUNT ON;
GO

----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.

IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO

CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO

INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).

IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO

IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO

CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO

------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO

CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO

------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO

-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                -- TODO: The name of the .xel file needs to be an exact match to the files in the storage account Container (You can use Storage Account explorer from the portal to find out the exact file names or you can retrieve the name using the following DMV-query: select target_data from sys.dm_xe_database_session_targets. The 3rd xml-node, "File name", contains the name of the file currently written to.)
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO

-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzStorageAccount to delete your Azure Storage account!';
GO
```

Hedef çalıştırdığınızda eklenmezse, olay oturumunu durdurmalı ve yeniden başlatmanız gerekir:

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```

## <a name="output"></a>Çıktı

Transact-SQL komut dosyası tamamlandığında, **event_data_XML** sütun üstbilgisinin altındaki bir hücreyi tıklatın. Bir ** \<olay>** öğesi bir UPDATE deyimi ni gösterir görüntülenir.

Burada sınama sırasında oluşturulan bir ** \<olay>** öğesi:

```xml
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

Önceki Transact-SQL komut dosyası event_file okumak için aşağıdaki sistem işlevini kullandı:

- [sys.fn_xe_file_target_read_file (Transact-SQL)](https://msdn.microsoft.com/library/cc280743.aspx)

Genişletilmiş etkinliklerden verilerin görüntülenmesi için gelişmiş seçeneklerin açıklaması şu anda kullanılabilir:

- [Genişletilmiş Etkinliklerden Hedef Verilerin Gelişmiş Görüntülenmesi](https://msdn.microsoft.com/library/mt752502.aspx)

## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Kod örneğini SQL Server'da çalışacak şekilde dönüştürme

Önceki Transact-SQL örneğini Microsoft SQL Server'da çalıştırmak istediğinizi varsayalım.

- Basitlik için, Azure Depolama kapsayıcısının kullanımını *C:\myeventdata.xel*gibi basit bir dosyayla tamamen değiştirmek isteyebilirsiniz. Dosya, SQL Server'ı barındıran bilgisayarın yerel sabit sürücüsüne yazılır.
- **CREATE MASTER KEY** ve CREATE **CREDENTIAL**için her türlü Transact-SQL deyimine ihtiyacınız olmaz.
- CREATE **EVENT SESSION** deyiminde, **HEDEF EKLE** yan tümcesinde, dosya **adı=** için atanan Http değerini *C:\myfile.xel*gibi tam yol dizesi ile değiştirirsiniz.
  
  - Azure Depolama hesabının dahil edilmesine gerek yoktur.

## <a name="more-information"></a>Daha fazla bilgi

Azure Depolama hizmetindeki hesaplar ve kapsayıcılar hakkında daha fazla bilgi için bkz:

- [.NET'ten Blob depolama nasıl kullanılır?](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [Kapsayıcıları, Blob'ları ve Meta Verileri Adlandırma ve Yönlendirme](https://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Kök Konteyneri ile Çalışma](https://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Ders 1: Azure kapsayıcıda depolanmış bir erişim ilkesi ve paylaşılan erişim imzası oluşturma](https://msdn.microsoft.com/library/dn466430.aspx)
  - [Ders 2: Paylaşılan erişim imzası kullanarak BIR SQL Server kimlik bilgisi oluşturma](https://msdn.microsoft.com/library/dn466435.aspx)
- [Microsoft SQL Server için Genişletilmiş Etkinlikler](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events)

<!-- Image references. -->
[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png
