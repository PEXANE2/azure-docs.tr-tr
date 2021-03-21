---
title: 'Öğretici: Azure SQL veritabanında güvenli şifreleme ile Always Encrypted kullanmaya başlama'
description: Bu öğreticide, Azure SQL veritabanı 'nda güvenli kuşmalar ile Always Encrypted için temel bir ortam oluşturma ve verileri yerinde şifreleme ve SQL Server Management Studio (SSMS) kullanarak şifrelenmiş sütunlarda zengin gizli sorgular verme öğretilir.
keywords: verileri şifreleme, SQL şifreleme, veritabanı şifreleme, hassas veriler, Always Encrypted, güvenli şifreleme, SGX, kanıtlama
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
ms.reviwer: vanto
ms.date: 01/15/2021
ms.openlocfilehash: 809ac72977b670faff984ad39effb1c70767e141
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102120955"
---
# <a name="tutorial-getting-started-with-always-encrypted-with-secure-enclaves-in-azure-sql-database"></a>Öğretici: Azure SQL veritabanında güvenli şifreleme ile Always Encrypted kullanmaya başlama

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Azure SQL veritabanı için güvenli şifreleme ile Always Encrypted Şu anda **genel önizleme** aşamasındadır.

Bu öğreticide, Azure SQL veritabanı 'nda [güvenli şifreli Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-enclaves) kullanmaya nasıl başlacağınız öğretilir. Şu şekilde görünür:

> [!div class="checklist"]
> - Güvenli şifreleme ile Always Encrypted test etmek ve değerlendirmek için bir ortam oluşturma.
> - Verileri yerinde şifreleme ve SQL Server Management Studio (SSMS) kullanarak şifrelenmiş sütunlarda zengin gizli sorgular verme.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici Azure PowerShell ve [SSMS](/sql/ssms/download-sql-server-management-studio-ssms)gerektirir.

### <a name="powershell-requirements"></a>PowerShell gereksinimleri

Azure PowerShell nasıl yükleneceği ve çalıştırılacağı hakkında bilgi için bkz. [Azure PowerShell genel bakış](/powershell/azure) . 

Kanıtlama işlemlerini desteklemek için gereken az modüllerin en düşük sürümü:

- Az 4.5.0
- Az. Accounts 1.9.2
- Az. kanıtlama 0.1.8

Tüm az modüllerin yüklü sürümünü doğrulamak için aşağıdaki komutu çalıştırın:

```powershell
Get-InstalledModule
```

Sürümler en düşük gereksinimle eşleştirilmemişse, `Update-Module` komutunu çalıştırın.

PowerShell Galerisi kullanım dışı Aktarım Katmanı Güvenliği (TLS) sürümleri 1,0 ve 1,1 ' dir. TLS 1,2 veya sonraki bir sürüm önerilir. 1,2 'den daha düşük bir TLS sürümü kullanıyorsanız aşağıdaki hataları alabilirsiniz:

- `WARNING: Unable to resolve package source 'https://www.powershellgallery.com/api/v2'`
- `PackageManagement\Install-Package: No match was found for the specified search criteria and module name.`

PowerShell Galerisi etkileşim kurmaya devam etmek için Install-Module komutlarından önce aşağıdaki komutu çalıştırın

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

### <a name="ssms-requirements"></a>SSMS gereksinimleri

SSMS 'nin nasıl indirileceği hakkında bilgi için bkz. [Download SQL Server Management Studio (SSMS)](/sql/ssms/download-sql-server-management-studio-ssms) .

SSMS 'nin gerekli en düşük sürümü 18,8 ' dir.


## <a name="step-1-create-and-configure-a-server-and-a-dc-series-database"></a>1. Adım: sunucu ve DC Serisi veritabanı oluşturma ve yapılandırma

Bu adımda, yeni bir Azure SQL veritabanı mantıksal sunucusu ve DC Serisi donanım oluşturma kullanarak güvenli şifreleme ile Always Encrypted için gereken yeni bir veritabanı oluşturacaksınız. Daha fazla bilgi için bkz. [DC-Series](service-tiers-vcore.md#dc-series).

1. Bir PowerShell konsolu açın ve az önce gerekli sürümünü içeri aktarın.

  ```PowerShell
  Import-Module "Az" -MinimumVersion "4.5.0"
  ```
  
2. Azure 'da oturum açın. Gerekirse, bu öğretici için kullandığınız [aboneliğe geçin](/powershell/azure/manage-subscriptions-azureps) .

  ```PowerShell
  Connect-AzAccount
  $subscriptionId = "<your subscription ID>"
  Set-AzContext -Subscription $subscriptionId
  ```

3. Yeni bir kaynak grubu oluşturma. 

  > [!IMPORTANT]
  > Kaynak grubunuzu hem DC Serisi donanım oluşturma hem de Microsoft Azure kanıtlama destekleyen bir bölgede (konum) oluşturmanız gerekir. DC serisini destekleyen bölgelerin listesi için bkz. [DC Serisi kullanılabilirliği](service-tiers-vcore.md#dc-series-1). Microsoft Azure kanıtlama 'nın bölgesel kullanılabilirliği [aşağıda](https://azure.microsoft.com/global-infrastructure/services/?products=azure-attestation) verilmiştir.

  ```powershell
  $resourceGroupName = "<your new resource group name>"
  $location = "<Azure region supporting DC-series and Microsoft Azure Attestation>"
  New-AzResourceGroup -Name $resourceGroupName -Location $location
  ```

4. Azure SQL mantıksal sunucusu oluşturun. İstendiğinde, Sunucu Yöneticisi adını ve parolasını girin. Yönetici adını ve parolayı hatırlamanız gerekir. sunucuya bağlanmak için daha sonra ihtiyacınız olacak.

  ```powershell
  $serverName = "<your server name>" 
  New-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName -Location $location 
  ```

5. Belirtilen IP aralığından erişime izin veren bir sunucu güvenlik duvarı kuralı oluşturun.
  
  ```powershell
  $startIp = "<start of IP range>"
  $endIp = "<end of IP range>"
  $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
  ```

6. Sunucunuza yönetilen bir sistem kimliği atayın. 

  ```PowerShell
  $server = Set-AzSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName -AssignIdentity
  $serverObjectId = $server.Identity.PrincipalId
  ```

7. DC Serisi veritabanı oluşturma.

  ```powershell
  $databaseName = "ContosoHR"
  $edition = "GeneralPurpose"
  $vCore = 2
  $generation = "DC"
  New-AzSqlDatabase -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -DatabaseName $databaseName `
    -Edition $edition `
    -Vcore $vCore `
    -ComputeGeneration $generation
  ```

8. Sunucunuz ve veritabanı hakkındaki bilgileri alın ve kaydedin. Bu bilgilerin yanı sıra, bu bölümün sonraki bölümlerinde bulunan 4. adımdaki yönetici adı ve parola gerekir.

  ```powershell
  Write-Host 
  Write-Host "Fully qualified server name: $($server.FullyQualifiedDomainName)" 
  Write-Host "Server Object Id: $serverObjectId"
  Write-Host "Database name: $databaseName"
  ```
  
## <a name="step-2-configure-an-attestation-provider"></a>2. Adım: bir kanıtlama sağlayıcısı yapılandırma 

Bu adımda Microsoft Azure kanıtlama için bir kanıtlama sağlayıcısı oluşturacaksınız ve yapılandıracaksınız. Bu, veritabanınızın kullandığı güvenli kuşve güvenliğini denemek için gereklidir.

1. Aşağıdaki kanıtlama ilkesini kopyalayın ve ilkeyi bir metin dosyasına (txt) kaydedin. Aşağıdaki ilke hakkında daha fazla bilgi için bkz. [kanıtlama sağlayıcısı oluşturma ve yapılandırma](always-encrypted-enclaves-configure-attestation.md#create-and-configure-an-attestation-provider).

  ```output
  version= 1.0;
  authorizationrules 
  {
       [ type=="x-ms-sgx-is-debuggable", value==false ]
        && [ type=="x-ms-sgx-product-id", value==4639 ]
        && [ type=="x-ms-sgx-svn", value>= 0 ]
        && [ type=="x-ms-sgx-mrsigner", value=="e31c9e505f37a58de09335075fc8591254313eb20bb1a27e5443cc450b6e33e5"] 
    => permit();
  };
  ```

2. Gerekli sürümünü içeri aktarın `Az.Attestation` .  

  ```powershell
  Import-Module "Az.Attestation" -MinimumVersion "0.1.8"
  ```
  
3. Bir kanıtlama sağlayıcısı oluşturun. 

  ```powershell
  $attestationProviderName = "<your attestation provider name>" 
  New-AzAttestation -Name $attestationProviderName -ResourceGroupName $resourceGroupName -Location $location
  ```

4. Kanıtlama ilkenizi yapılandırın.
  
  ```powershell
  $policyFile = "<the pathname of the file from step 1 in this section>"
  $teeType = "SgxEnclave"
  $policyFormat = "Text"
  $policy=Get-Content -path $policyFile -Raw
  Set-AzAttestationPolicy -Name $attestationProviderName `
    -ResourceGroupName $resourceGroupName `
    -Tee $teeType `
    -Policy $policy `
    -PolicyFormat  $policyFormat
  ```

5. Azure SQL mantıksal sunucunuza kanıtlama sağlayıcınıza erişim izni verin. Bu adımda, daha önce sunucunuza atadığınız yönetilen hizmet kimliğinin nesne KIMLIĞINI kullanıyorsunuz.

  ```powershell
  New-AzRoleAssignment -ObjectId $serverObjectId `
    -RoleDefinitionName "Attestation Reader" `
    -ResourceName $attestationProviderName `
    -ResourceType "Microsoft.Attestation/attestationProviders" `
    -ResourceGroupName $resourceGroupName  
  ```

6. SGX Enclave için yapılandırdığınız bir kanıtlama ilkesini işaret eden kanıtlama URL 'sini alın. Daha sonra ihtiyacınız olacağı için URL 'YI kaydedin.

  ```powershell
  $attestationProvider = Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $resourceGroupName 
  $attestationUrl = $attestationProvider.AttestUri + “/attest/SgxEnclave”
  Write-Host
  Write-Host "Your attestation URL is: $attestationUrl"
  ```
  
  Kanıtlama URL 'SI şöyle görünmelidir: `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`

## <a name="step-3-populate-your-database"></a>3. Adım: veritabanınızı doldurma

Bu adımda, bir tablo oluşturacak ve daha sonra şifreleyip Sorgulayabileceğiniz bazı verilerle dolduracaksınız.

1. SSMS 'yi açın ve veritabanı bağlantısında Always Encrypted etkin **olmadan** oluşturduğunuz Azure SQL mantıksal sunucusu 'nda **ContosoHR** veritabanına bağlanın.
    1. **Sunucuya Bağlan** iletişim kutusunda sunucunuzun tam adını (örneğin, *myserver123.Database.Windows.net*) belirtin ve yönetici kullanıcı adını ve sunucuyu oluştururken belirttiğiniz parolayı girin.
    2. **Seçenekler >>** tıklayın ve **bağlantı özellikleri** sekmesini seçin. **ContosoHR** veritabanını seçtiğinizden emin olun (varsayılan, ana veritabanı değil). 
    3. **Always Encrypted** sekmesini seçin.
    4. **Always Encrypted etkinleştir (sütun şifreleme)** onay **kutusunun seçili olmadığından** emin olun.

        ![Always Encrypted olmadan Bağlan](media/always-encrypted-enclaves/connect-without-always-encrypted-ssms.png)

    5. **Bağlan**'a tıklayın.

2. **Çalışanlar** adlı yeni bir tablo oluşturun.

    ```sql
    CREATE SCHEMA [HR];
    GO

    CREATE TABLE [HR].[Employees]
    (
        [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NOT NULL,
        [LastName] [nvarchar](50) NOT NULL,
        [Salary] [money] NOT NULL
    ) ON [PRIMARY];
    GO
    ```

3. **Çalışanlar** tablosuna birkaç çalışan kaydı ekleyin.

    ```sql
    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('795-73-9838'
            , N'Catherine'
            , N'Abel'
            , $31692);

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('990-00-6818'
            , N'Kim'
            , N'Abercrombie'
            , $55415);
    ```


## <a name="step-4-provision-enclave-enabled-keys"></a>4. Adım: şifreleme özellikli anahtarları sağlama

Bu adımda, bir sütun ana anahtarı ve şifreli hesaplamalar sağlayan bir sütun şifreleme anahtarı oluşturacaksınız.

1. Önceki adımda SSMS örneğini kullanarak, **Nesne Gezgini**, veritabanınızı genişletin ve **güvenlik**  >  **Always Encrypted anahtarlarına** gidin.
1. Yeni bir şifreleme etkin sütun ana anahtarı sağlayın:
    1. **Always Encrypted anahtarlar** ' a sağ tıklayın ve **Yeni sütun ana anahtarı ' nı seçin...**
    2. Sütununuzu seçin ana anahtar adı: **CMK1**.
    3. **Windows sertifika deposu (geçerli kullanıcı veya yerel makine)** ya da **Azure Key Vault** seçtiğinizden emin olun.
    4. **Şifreli hesaplamalar Için Izin ver**' i seçin.
    5. Azure Key Vault seçtiyseniz Azure 'da oturum açın ve anahtar kasanızı seçin. Always Encrypted için Anahtar Kasası oluşturma hakkında daha fazla bilgi için, bkz. [Azure Portal, anahtar kasalarınızı yönetme](/archive/blogs/kv/manage-your-key-vaults-from-new-azure-portal).
    6. Zaten varsa sertifikanızı veya Azure anahtar değer anahtarınızı seçin ya da yeni bir tane oluşturmak için **sertifika oluştur** düğmesine tıklayın.
    7. **Tamam**’ı seçin.

        ![Şifreleme hesaplamaları sağlar](media/always-encrypted-enclaves/allow-enclave-computations.png)

1. Yeni bir şifreleme etkin sütun şifreleme anahtarı oluşturun:

    1. **Always Encrypted anahtarlar** ' a sağ tıklayın ve **Yeni sütun şifreleme anahtarı**' nı seçin.
    2. Yeni sütun şifreleme anahtarı için bir ad girin: **CEK1**.
    3. **Sütun ana anahtarı** açılan listesinde, önceki adımlarda oluşturduğunuz sütun ana anahtarını seçin.
    4. **Tamam**’ı seçin.

## <a name="step-5-encrypt-some-columns-in-place"></a>5. Adım: bazı sütunları yerinde şifreleyin

Bu adımda, sunucu tarafı şifrelemesi içindeki **SSK** ve **maaş** sütunlarında depolanan verileri şifreleyebilir ve sonra verilerde bir SELECT sorgusu test edersiniz.

1. Yeni bir SSMS örneği açın ve veritabanı bağlantısı için Always Encrypted etkinleştirilmiş **olan** veritabanınıza bağlanın.
    1. Yeni bir SSMS örneği başlatın.
    2. **Sunucuya Bağlan** iletişim kutusunda sunucunuzun tam adını (örneğin, *myserver123.Database.Windows.net*) belirtin ve yönetici kullanıcı adını ve sunucuyu oluştururken belirttiğiniz parolayı girin.
    3. **Seçenekler >>** tıklayın ve **bağlantı özellikleri** sekmesini seçin. **ContosoHR** veritabanını seçtiğinizden emin olun (varsayılan, ana veritabanı değil). 
    4. **Always Encrypted** sekmesini seçin.
    5. **Always Encrypted etkinleştir (sütun şifreleme)** onay kutusunun seçili olduğundan emin olun.
    6. [2. Adım: bir kanıtlama sağlayıcısı yapılandırma](#step-2-configure-an-attestation-provider)içindeki adımları izleyerek aldığınız ŞIFRELEME kanıtlama URL 'nizi belirtin. Aşağıdaki ekran görüntüsüne bakın.

        ![Kanıtlama ile bağlanma](media/always-encrypted-enclaves/connect-to-server-configure-attestation.png)

    7. **Bağlan**’ı seçin.
    8. Always Encrypted sorguları için Parametreleştirme 'yi etkinleştirmeniz istenirse **Etkinleştir**' i seçin.



1. Aynı SSMS örneğini (Always Encrypted etkinleştirilmiş) kullanarak, yeni bir sorgu penceresi açın ve aşağıdaki deyimlerini çalıştırarak **SSK** ve **maaş** sütunlarını şifreleyin.

    ```sql
    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [SSN] [char] (11) COLLATE Latin1_General_BIN2
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [Salary] [money]
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```

    > [!NOTE]
    > Yukarıdaki betikteki veritabanının sorgu planı önbelleğini temizlemek için ALTER DATABASE KAPSAMLı yapılandırma CLEAR PROCEDURE_CACHE deyimini unutmayın. Tabloyu değiştirdikten sonra, parametreleri şifreleme bilgilerini yenilemek için tabloya erişen tüm toplu işler ve saklı yordamlar için planları temizlemeniz gerekir. 

1. **SSN** ve **maaş** sütunlarının artık şifrelendiğinden emin olmak için, veritabanı bağlantısı için Always Encrypted etkin **olmadan** SSMS örneğinde yeni bir sorgu penceresi açın ve aşağıdaki ifadeyi yürütün. Sorgu penceresi, **SSK** ve **maaş** sütunlarında şifreli değerler döndürmelidir. Aynı sorguyu SSMS örneğini kullanarak çalıştırırsanız, Always Encrypted etkinleştirilmiş olarak, şifresi çözülmüş olan verileri görmeniz gerekir.

    ```sql
    SELECT * FROM [HR].[Employees];
    ```

## <a name="step-6-run-rich-queries-against-encrypted-columns"></a>6. Adım: şifrelenmiş sütunlarda zengin sorgular çalıştırma

Şifrelenmiş sütunlarda zengin sorgular çalıştırabilirsiniz. Bazı sorgu işlemleri, sunucu tarafı şifrelerinizin içinde gerçekleştirilecek. 

1. **Always Encrypted etkinleştirilmiş** SSMS örneğinde, Always Encrypted Için Parametreleştirme özelliğinin de etkinleştirildiğinden emin olun.
    1. SSMS ana menüsünden **Araçlar** ' ı seçin.
    2. **Seçenekleri seçin...**.
    3. **Sorgu yürütme**  >  **SQL Server**  >  **Gelişmiş**' e gidin.
    4. **Always Encrypted Için parametreleştirmeyi etkinleştir** ' in işaretli olduğundan emin olun.
    5. **Tamam**’ı seçin.
2. Yeni bir sorgu penceresi açın, aşağıdaki sorguyu yapıştırın ve yürütün. Sorgu, belirtilen arama ölçütlerine uyan düz metin değerleri ve satırlar döndürmelidir.

    ```sql
    DECLARE @SSNPattern [char](11) = '%6818';
    DECLARE @MinSalary [money] = $1000;
    SELECT * FROM [HR].[Employees]
    WHERE SSN LIKE @SSNPattern AND [Salary] >= @MinSalary;
    ```

3. Always Encrypted etkinleştirilmiş olan SSMS örneğinde aynı sorguyu yeniden deneyin. Bir hata oluşur.
 
## <a name="next-steps"></a>Sonraki adımlar

Bu Öğreticiyi tamamladıktan sonra aşağıdaki öğreticilerden birine gidebilirsiniz:
- [Öğretici: güvenli şifreleme ile Always Encrypted kullanarak bir .NET uygulaması geliştirin](/sql/connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps)
- [Öğretici: güvenli şifreleme ile Always Encrypted kullanarak .NET Framework uygulaması geliştirme](/sql/relational-databases/security/tutorial-always-encrypted-enclaves-develop-net-framework-apps)
- [Öğretici: rastgele şifreleme kullanarak şifreleme etkin sütunlarda dizinler oluşturma ve kullanma](/sql/relational-databases/security/tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption)

## <a name="see-also"></a>Ayrıca Bkz.

- [Güvenli şifreleme ile Always Encrypted yapılandırma ve kullanma](/sql/relational-databases/security/encryption/configure-always-encrypted-enclaves)
