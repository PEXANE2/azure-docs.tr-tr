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
ms.openlocfilehash: 94923b13181290a290f13339da5b05f6fdddff38
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98253748"
---
# <a name="tutorial-getting-started-with-always-encrypted-with-secure-enclaves-in-azure-sql-database"></a>Öğretici: Azure SQL veritabanında güvenli şifreleme ile Always Encrypted kullanmaya başlama

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Azure SQL veritabanı için güvenli şifreleme ile Always Encrypted Şu anda **genel önizleme** aşamasındadır.

Bu öğreticide, Azure SQL veritabanı 'nda [güvenli şifreli Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) kullanmaya nasıl başlacağınız öğretilir. Şu şekilde görünür:

> [!div class="checklist"]
> - Güvenli şifreleme ile Always Encrypted test etmek ve değerlendirmek için bir ortam oluşturma.
> - Verileri yerinde şifreleme ve SQL Server Management Studio (SSMS) kullanarak şifrelenmiş sütunlarda zengin gizli sorgular verme.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici Azure PowerShell ve [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)gerektirir.

### <a name="powershell-requirements"></a>PowerShell gereksinimleri

Azure PowerShell nasıl yükleneceği ve çalıştırılacağı hakkında bilgi için bkz. [Azure PowerShell genel bakış](https://docs.microsoft.com/powershell/azure) . 

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

SSMS 'nin nasıl indirileceği hakkında bilgi için bkz. [Download SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) .

SSMS 'nin gerekli en düşük sürümü 18,8 ' dir.


## <a name="step-1-create-a-server-and-a-dc-series-database"></a>1. Adım: sunucu ve DC Serisi veritabanı oluşturma

 Bu adımda, DC Serisi donanım yapılandırmasını kullanarak yeni bir Azure SQL veritabanı mantıksal sunucusu ve yeni bir veritabanı oluşturacaksınız. Azure SQL veritabanı 'nda güvenli şifrelerle Always Encrypted, DC Serisi donanım yapılandırmasında desteklenen Intel SGX enclaven kullanır. Daha fazla bilgi için bkz. [DC-Series](service-tiers-vcore.md#dc-series).

1. Bir PowerShell konsolu açın ve Azure 'da oturum açın. Gerekirse, bu öğretici için kullandığınız [aboneliğe geçin](https://docs.microsoft.com/powershell/azure/manage-subscriptions-azureps) .

  ```PowerShell
  Connect-AzAccount
  $subscriptionId = <your subscription ID>
  Set-AzContext -Subscription $serverSubscriptionId
  ```

2. Veritabanı sunucunuzu içerecek bir kaynak grubu oluşturun. 

  ```powershell
  $serverResourceGroupName = "<server resource group name>"
  $serverLocation = "<Azure region that supports DC-series in SQL Database>"
  New-AzResourceGroup -Name $serverResourceGroupName -Location $serverLocation 
  ```

  > [!IMPORTANT]
  > Kaynak grubunuzu DC Serisi donanım yapılandırmasını destekleyen bir bölgede oluşturmanız gerekir. Şu anda desteklenen bölgelerin listesi için bkz. [DC Serisi kullanılabilirliği](service-tiers-vcore.md#dc-series-1).

3. Veritabanı sunucusu oluşturun. İstendiğinde, Sunucu Yöneticisi adını ve parolasını girin.

  ```powershell
  $serverName = "<server name>" 
  New-AzSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -Location $serverLocation
  ```

4. Belirtilen IP aralığından erişime izin veren bir sunucu güvenlik duvarı kuralı oluşturma
  
  ```powershell
  # The ip address range that you want to allow to access your server
  $startIp = "<start of IP range>"
  $endIp = "<end of IP range>"
  $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
  ```

5. Sunucunuza yönetilen bir sistem kimliği atayın. Daha sonra sunucunuza Microsoft Azure kanıtlama için erişim izni vermeniz gerekir.

  ```powershell
  Set-AzSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -AssignIdentity 
  ```

6. Sunucunuza atanan kimliğin nesne KIMLIĞINI alın. Elde edilen nesne KIMLIĞINI kaydedin. KIMLIğIN sonraki bir bölümünde olması gerekir.

  > [!NOTE]
  > Yeni atanan yönetilen sistem kimliğinin Azure Active Directory içinde yayılması birkaç saniye sürebilir. Aşağıdaki betik boş bir sonuç döndürtikten sonra yeniden deneyin.

  ```PowerShell
  $server = Get-AzSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 
  $serverObjectId = $server.Identity.PrincipalId
  $serverObjectId
  ```

7. DC Serisi veritabanı oluşturma.

  ```powershell
  $databaseName = "ContosoHR"
  $edition = "GeneralPurpose"
  $vCore = 2
  $generation = "DC"
  New-AzSqlDatabase -ResourceGroupName $serverResourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $edition -Vcore $vCore -ComputeGeneration $generation
  ```

## <a name="step-2-configure-an-attestation-provider"></a>2. Adım: bir kanıtlama sağlayıcısı yapılandırma

Bu adımda Microsoft Azure kanıtlama için bir kanıtlama sağlayıcısı oluşturacaksınız ve yapılandıracaksınız. Bu, veritabanı sunucunuzdaki güvenli kuşve güvenliğini test etmek için gereklidir.

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

2. Ve için gerekli sürümlerini içeri `Az.Accounts` aktarın `Az.Attestation` .  

  ```powershell
  Import-Module "Az.Accounts" -MinimumVersion "1.9.2"
  Import-Module "Az.Attestation" -MinimumVersion "0.1.8"
  ```

3. Kanıtlama sağlayıcısı için bir kaynak grubu oluşturun.

  ```powershell
  $attestationLocation = $serverLocation
  $attestationResourceGroupName = "<attestation provider resource group name>"
  New-AzResourceGroup -Name $attestationResourceGroupName -Location $location  
  ```

4. Bir kanıtlama sağlayıcısı oluşturun. 

  ```powershell
  $attestationProviderName = "<attestation provider name>" 
  New-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName -Location $attestationLocation
  ```

5. Kanıtlama ilkenizi yapılandırın.
  
  ```powershell
  $policyFile = "<the pathname of the file from step 1 in this section"
  $teeType = "SgxEnclave"
  $policyFormat = "Text"
  $policy=Get-Content -path $policyFile -Raw
  Set-AzAttestationPolicy -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName -Tee $teeType -Policy $policy -PolicyFormat  $policyFormat
  ```

6. Azure SQL mantıksal sunucunuza kanıtlama sağlayıcınıza erişim izni verin. Bu adımda, daha önce sunucunuza atadığınız yönetilen hizmet kimliğinin nesne KIMLIĞINI kullandık.

  ```powershell
  New-AzRoleAssignment -ObjectId $serverObjectId -RoleDefinitionName "Attestation Reader" -ResourceGroupName $attestationResourceGroupName  
  ```

7. Kanıtlama URL 'sini alın.

  ```powershell
  $attestationProvider = Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroupName 
  $attestationUrl = $attestationProvider.AttestUri + “/attest/SgxEnclave”
  Write-Host "Your attestation URL is: " $attestationUrl 
  ```

8.  Sonuç olarak, SGX şifreleme için yapılandırdığınız bir kanıtlama ilkesini işaret eden kanıtlama URL 'sini kaydedin. Buna daha sonra ihtiyacınız olacak. Kanıtlama URL 'SI şöyle görünmelidir: `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`

## <a name="step-3-populate-your-database"></a>3. Adım: veritabanınızı doldurma

Bu adımda, bir tablo oluşturacak ve daha sonra şifreleyip Sorgulayabileceğiniz bazı verilerle dolduracaksınız.

1. SSMS 'yi açın ve veritabanı bağlantısında Always Encrypted etkin **olmadan** oluşturduğunuz Azure SQL mantıksal sunucusu 'nda **ContosoHR** veritabanına bağlanın.
    1. **Sunucuya Bağlan** iletişim kutusunda sunucunuzun adını (örneğin, *myserver123.Database.Windows.net*) belirtin ve daha önce yapılandırdığınız Kullanıcı adını ve parolayı girin.
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
    2. **Sunucuya Bağlan** iletişim kutusunda, sunucu adınızı belirtin, bir kimlik doğrulama yöntemi seçin ve kimlik bilgilerinizi belirtin.
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
- [Öğretici: güvenli şifreleme ile Always Encrypted kullanarak bir .NET uygulaması geliştirin](https://docs.microsoft.com/sql/connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps)
- [Öğretici: güvenli şifreleme ile Always Encrypted kullanarak .NET Framework uygulaması geliştirme](https://docs.microsoft.com/sql/relational-databases/security/tutorial-always-encrypted-enclaves-develop-net-framework-apps)
- [Öğretici: rastgele şifreleme kullanarak şifreleme etkin sütunlarda dizinler oluşturma ve kullanma](https://docs.microsoft.com/sql/relational-databases/security/tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption)

## <a name="see-also"></a>Ayrıca Bkz.

- [Güvenli şifreleme ile Always Encrypted yapılandırma ve kullanma](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-always-encrypted-enclaves)