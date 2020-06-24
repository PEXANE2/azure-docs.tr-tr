---
title: TDE sertifikası ile yönetilen örneği geçirme
description: Azure SQL yönetilen örneği Saydam Veri Şifrelemesi bir veritabanının veritabanı şifreleme anahtarını koruyan bir sertifika geçirme
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: carlrab, jovanpop
ms.date: 04/25/2019
ms.openlocfilehash: c9a9b42d6f6d8c89847b03f5eda858c75d198c58
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84711400"
---
# <a name="migrate-a-certificate-of-a-tde-protected-database-to-azure-sql-managed-instance"></a>TDE korumalı bir veritabanının sertifikasını Azure SQL yönetilen örneği 'ne geçirme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Yerel geri yükleme seçeneğini kullanarak [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) ile korunan bir VERITABANıNı Azure SQL yönetilen örneğine geçirdiğinizde, SQL Server örneğinden karşılık gelen sertifikanın veritabanı geri yüklemeden önce geçirilmesi gerekir. Bu makalede, sertifikayı Azure SQL yönetilen örneği 'ne el ile geçirme işleminde izlenecek yol gösterilmektedir:

> [!div class="checklist"]
>
> * Sertifikayı kişisel bilgi değişimi (. pfx) dosyasına dışarı aktarma
> * Sertifikayı bir dosyadan temel 64 dizeye Ayıkla
> * PowerShell cmdlet 'ini kullanarak karşıya yükleme

Hem TDE korumalı bir veritabanının hem de karşılık gelen sertifikanın sorunsuz geçirilmesi için tam olarak yönetilen bir hizmet kullanan alternatif bir seçenek için, [Azure veritabanı geçiş hizmeti 'ni kullanarak şirket içi veritabanınızı Azure SQL yönetilen örneği 'ne geçirme](../../dms/tutorial-sql-server-to-managed-instance.md)konusuna bakın.

> [!IMPORTANT]
> Geçirilen bir sertifika yalnızca TDE korumalı veritabanının geri yüklenmesi için kullanılır. Geri yükleme yapıldıktan kısa süre sonra, geçirilen sertifika, örnek üzerinde ayarlamış olduğunuz TDE türüne bağlı olarak, hizmet tarafından yönetilen bir sertifika veya anahtar kasasından asimetrik bir anahtarla değiştirilmiştir.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlayabilmeniz için şu önkoşullar gereklidir:

* Şirket içi sunucuya veya dosya olarak dışarı aktarılan sertifikaya erişimi olan başka bir bilgisayara yüklenmiş [Pvk2Pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) komut satırı aracı. Pvk2Pfx Aracı, kendi içinde bulunan bir komut satırı ortamı olan [Kurumsal Windows Sürücü Seti](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk)'nin bir parçasıdır.
* [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell) sürüm 5.0 veya üstü yüklenmiş olmalıdır.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Aşağıdakilere sahip olduğunuzdan emin olun:

* Azure PowerShell modül [yüklendi ve güncelleştirildi](https://docs.microsoft.com/powershell/azure/install-az-ps).
* [Az. SQL modülü](https://www.powershellgallery.com/packages/Az.Sql).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL yönetilen örneği tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Modülü yüklemek/güncelleştirmek için PowerShell 'de aşağıdaki komutları çalıştırın:

```azurepowershell
Install-Module -Name Az.Sql
Update-Module -Name Az.Sql
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

* * *

## <a name="export-the-tde-certificate-to-a-pfx-file"></a>TDE sertifikasını bir. pfx dosyasına dışarı aktarma

Sertifika, kaynak SQL Server örneğinden veya burada tutuluyorsa sertifika deposundan doğrudan aktarılabilir.

### <a name="export-the-certificate-from-the-source-sql-server-instance"></a>Sertifikayı kaynak SQL Server örneğinden dışarı aktarma

Sertifikayı SQL Server Management Studio dışarı aktarmak ve. pfx biçimine dönüştürmek için aşağıdaki adımları kullanın. *TDE_Cert* ve *full_path* genel adları, adımlar aracılığıyla sertifika ve dosya adları ve yollar için kullanılmaktadır. Bunlar gerçek adlarla değiştirilmelidir.

1. SSMS 'de yeni bir sorgu penceresi açın ve kaynak SQL Server örneğine bağlanın.

1. TDE korumalı veritabanlarını listelemek ve geçirilecek veritabanının şifrelemesini koruyan sertifikanın adını almak için aşağıdaki betiği kullanın:

   ```sql
   USE master
   GO
   SELECT db.name as [database_name], cer.name as [certificate_name]
   FROM sys.dm_database_encryption_keys dek
   LEFT JOIN sys.certificates cer
   ON dek.encryptor_thumbprint = cer.thumbprint
   INNER JOIN sys.databases db
   ON dek.database_id = db.database_id
   WHERE dek.encryption_state = 3
   ```

   ![TDE sertifikalarının listesi](./media/tde-certificate-migrate/onprem-certificate-list.png)

1. Sertifikayı, ortak ve özel anahtar bilgilerinin tutulduğu dosya çiftine (.cer ve .pvk) dışarı aktarmak için şu betiği yürütün:

   ```sql
   USE master
   GO
   BACKUP CERTIFICATE TDE_Cert
   TO FILE = 'c:\full_path\TDE_Cert.cer'
   WITH PRIVATE KEY (
     FILE = 'c:\full_path\TDE_Cert.pvk',
     ENCRYPTION BY PASSWORD = '<SomeStrongPassword>'
   )
   ```

   ![Yedekleme TDE sertifikası](./media/tde-certificate-migrate/backup-onprem-certificate.png)

1. Yeni oluşturulan dosyalardan bir çiftin sertifika bilgilerini Pvk2Pfx aracını kullanarak bir. pfx dosyasına kopyalamak için PowerShell konsolunu kullanın:

   ```cmd
   .\pvk2pfx -pvk c:/full_path/TDE_Cert.pvk  -pi "<SomeStrongPassword>" -spc c:/full_path/TDE_Cert.cer -pfx c:/full_path/TDE_Cert.pfx
   ```

### <a name="export-the-certificate-from-a-certificate-store"></a>Sertifikayı bir sertifika deposundan dışarı aktarma

Sertifika SQL Server yerel makine sertifika deposunda tutuluyorsa, bu, aşağıdaki adımlar kullanılarak verilebilirler:

1. PowerShell konsolunu açın ve Microsoft Yönetim Konsolu 'nun Sertifikalar ek bileşenini açmak için aşağıdaki komutu yürütün:

   ```cmd
   certlm
   ```

2. Sertifikalar MMC ek bileşeninde, sertifikaların listesini görmek için kişisel > sertifikaları yolunu genişletin.

3. Sertifikaya sağ tıklayın ve **dışarı aktar**' a tıklayın.

4. Sertifikayı ve özel anahtarı bir. pfx biçimine aktarmak için Sihirbazı izleyin.

## <a name="upload-the-certificate-to-azure-sql-managed-instance-using-an-azure-powershell-cmdlet"></a>Azure PowerShell cmdlet 'ini kullanarak sertifikayı Azure SQL yönetilen örneği 'ne yükleme

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. PowerShell’deki hazırlık adımlarını başlatın:

   ```azurepowershell
   # import the module into the PowerShell session
   Import-Module Az
   # connect to Azure with an interactive dialog for sign-in
   Connect-AzAccount
   # list subscriptions available and copy id of the subscription target the managed instance belongs to
   Get-AzSubscription
   # set subscription for the session
   Select-AzSubscription <subscriptionId>
   ```

2. Tüm hazırlık adımları yapıldıktan sonra, temel 64 kodlu sertifikayı hedef yönetilen örneğe yüklemek için aşağıdaki komutları çalıştırın:

   ```azurepowershell
   $fileContentBytes = Get-Content 'C:/full_path/TDE_Cert.pfx' -Encoding Byte
   $base64EncodedCert = [System.Convert]::ToBase64String($fileContentBytes)
   $securePrivateBlob = $base64EncodedCert  | ConvertTo-SecureString -AsPlainText -Force
   $password = "<password>"
   $securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
   Add-AzSqlManagedInstanceTransparentDataEncryptionCertificate -ResourceGroupName "<resourceGroupName>" `
       -ManagedInstanceName "<managedInstanceName>" -PrivateBlob $securePrivateBlob -Password $securePassword
   ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Önce *. pfx* dosyanız Ile [bir Azure Anahtar Kasası ayarlamanız](/azure/key-vault/key-vault-manage-with-cli2) gerekir.

1. PowerShell’deki hazırlık adımlarını başlatın:

   ```azurecli
   # connect to Azure with an interactive dialog for sign-in
   az login

   # list subscriptions available and copy id of the subscription target the managed instance belongs to
   az account list

   # set subscription for the session
   az account set --subscription <subscriptionId>
   ```

1. Tüm hazırlık adımları yapıldıktan sonra, temel 64 kodlu sertifikayı hedef yönetilen örneğe yüklemek için aşağıdaki komutları çalıştırın:

   ```azurecli
   az sql mi tde-key set --server-key-type AzureKeyVault --kid "<keyVaultId>" `
       --managed-instance "<managedInstanceName>" --resource-group "<resourceGroupName>"
   ```

* * *

Sertifika artık belirtilen yönetilen örnek tarafından kullanılabilir ve ilgili TDE korunan veritabanının yedeklemesi başarıyla geri yüklenebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şirket içi veya IaaS SQL Server örneğinden Azure SQL yönetilen örneği 'ne Saydam Veri Şifrelemesi olan bir veritabanının şifreleme anahtarını koruyan bir sertifikayı nasıl geçirebileceğiniz öğrendiniz.

Bir veritabanı yedeklemesini Azure SQL yönetilen örneği 'ne geri yüklemeyi öğrenmek için bkz. [Azure SQL yönetilen örneği 'ne veritabanı yedeklemesini geri yükleme](restore-sample-database-quickstart.md) .
