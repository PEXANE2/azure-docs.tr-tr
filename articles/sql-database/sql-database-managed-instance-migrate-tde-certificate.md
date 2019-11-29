---
title: TDE sertifikası ile yönetilen örneği geçirme
description: Azure SQL veritabanı yönetilen örneği 'ne saydam veri şifrelemesi ile bir veritabanının veritabanı şifreleme anahtarını koruyan sertifikayı geçirme
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: carlrab, jovanpop
ms.date: 04/25/2019
ms.openlocfilehash: 0f6e379287323d9353acd887cf30d5c9c0065959
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555379"
---
# <a name="migrate-certificate-of-tde-protected-database-to-azure-sql-database-managed-instance"></a>TDE korumalı veritabanının sertifikasını Azure SQL veritabanı yönetilen örneği 'ne geçirme

Yerel geri yükleme seçeneği kullanılarak [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) tarafından korunan bir VERITABANıNı Azure SQL veritabanı yönetilen örneği 'ne geçirirken, şirket Içi veya ıaas SQL Server karşılık gelen sertifikanın veritabanı geri yüklemeden önce geçirilmesi gerekir. Bu makale, sertifikanın Azure SQL Veritabanı Yönetilen Örneği’ne el ile geçiş işleminde size yol gösterir:

> [!div class="checklist"]
> * Sertifikayı Kişisel Bilgi Değişimi (.pfx) dosyası olarak dışarı aktarma
> * Sertifikayı dosyadan base-64 dizesine ayıklama
> * PowerShell cmdlet’ini kullanarak bunu karşıya yükleme

Tam yönetilen hizmet kullanılarak hem TDE korumalı veritabanının hem de ilgili sertifikanın sorunsuz geçişini sağlamaya yönelik alternatif bir seçenek için bkz. [Azure Veritabanı Geçiş Hizmeti'ni kullanarak şirket içi veritabanınızı Yönetilen Örneğe geçirme](../dms/tutorial-sql-server-to-managed-instance.md).

> [!IMPORTANT]
> Geçirilen sertifika yalnızca TDE korumalı veritabanını geri yüklemek için kullanılır. Geri yükleme işlemi yapıldıktan kısa süre sonra, geçirilen sertifika, örnekte belirlediğiniz saydam veri şifrelemesinin türüne bağlı olarak, hizmet tarafından yönetilen sertifika veya asimetrik anahtar tarafından farklı bir koruyucu ile değiştirilmiştir.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için şu önkoşullar gereklidir:

- Şirket içi sunucuya veya dosya olarak dışarı aktarılan sertifikaya erişimi olan başka bir bilgisayara yüklenmiş [Pvk2Pfx](https://docs.microsoft.com/windows-hardware/drivers/devtest/pvk2pfx) komut satırı aracı. Pvk2Pfx aracı, tek başına kendi içinde bir komut satırı ortamı olan [Enterprise Windows Driver Kit](https://docs.microsoft.com/windows-hardware/drivers/download-the-wdk)'in bir parçasıdır.
- [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell) sürüm 5.0 veya üstü yüklenmiş olmalıdır.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Aşağıdakilere sahip olduğunuzdan emin olun:

- Azure PowerShell modül [yüklendi ve güncelleştirildi](https://docs.microsoft.com/powershell/azure/install-az-ps).
- [Az. SQL modülü](https://www.powershellgallery.com/packages/Az.Sql).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

Modülü yüklemek/güncelleştirmek için PowerShell 'de aşağıdaki komutları çalıştırın:

```azurepowershell
Install-Module -Name Az.Sql
Update-Module -Name Az.Sql
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli).

* * *

## <a name="export-tde-certificate-to-a-personal-information-exchange-pfx-file"></a>TDE sertifikasını Kişisel Bilgi Değişimi (.pfx) dosyasında dışarı aktarma

Sertifika doğrudan kaynak SQL Server’dan veya sertifika depolama alanından (burada tutuluyorsa) dışarı aktarılabilir.

### <a name="export-certificate-from-the-source-sql-server"></a>Sertifikayı kaynak SQL Server’dan dışarı aktarma

Sertifikayı SQL Server Management Studio ile dışarı aktarmak ve bunu pfx biçimine dönüştürmek için aşağıdaki adımları kullanın. Bu adımlarda sertifikanın dosya adları ve yolları için *TDE_Cert* ve *full_path* genel adları kullanılır. Bunlar gerçek adlarla değiştirilmelidir.

1. SSMS’de yeni bir sorgu penceresi açın ve kaynak SQL Server’a bağlanın.

1. TDE korumalı veritabanlarını listelemek ve geçirilecek veritabanının şifrelemesini koruyan sertifikanın adını almak için şu betiği kullanın:

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

   ![TDE sertifikalarının listesi](./media/sql-database-managed-instance-migrate-tde-certificate/onprem-certificate-list.png)

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

   ![yedek TDE sertifikası](./media/sql-database-managed-instance-migrate-tde-certificate/backup-onprem-certificate.png)

1. Pvk2Pfx aracını kullanarak, sertifika bilgilerini yeni oluşturulmuş dosya çiftinden Kişisel Bilgi Değişimi (.pfx) dosyasına kopyalamak için PowerShell konsolunu kullanın:

   ```cmd
   .\pvk2pfx -pvk c:/full_path/TDE_Cert.pvk  -pi "<SomeStrongPassword>" -spc c:/full_path/TDE_Cert.cer -pfx c:/full_path/TDE_Cert.pfx
   ```

### <a name="export-certificate-from-certificate-store"></a>Sertifika depolama alanından sertifikayı dışarı aktarma

Sertifika SQL Server’ın yerel makine sertifika depolama alanında tutuluyorsa şu adımlar kullanılarak dışarı aktarılabilir:

1. PowerShell konsolunu açın ve Microsoft Yönetim Konsolu’nun Sertifikalar ek bileşenini açmak için şu komutu yürütün:

   ```cmd
   certlm
   ```

2. Sertifikalar MMC ek bileşeninde, sertifikaların listesini görmek için Kişisel -> Sertifikalar yolunu genişletin

3. Sertifikaya sağ tıklayın ve Dışarı Aktar... seçeneğine tıklayın

4. Sertifikayı ve özel anahtarı Kişisel Bilgi Değişimi biçiminde dışarı aktarmak için sihirbazı izleyin

## <a name="upload-certificate-to-azure-sql-database-managed-instance-using-azure-powershell-cmdlet"></a>Azure PowerShell cmdlet 'ini kullanarak Azure SQL veritabanı yönetilen örneği 'ne sertifika yükleme

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

1. PowerShell’deki hazırlık adımlarını başlatın:

   ```azurepowershell
   # import the module into the PowerShell session
   Import-Module Az
   # connect to Azure with an interactive dialog for sign-in
   Connect-AzAccount
   # list subscriptions available and copy id of the subscription target Managed Instance belongs to
   Get-AzSubscription
   # set subscription for the session
   Select-AzSubscription <subscriptionId>
   ```

2. Tüm hazırlık adımları tamamladıktan sonra, base-64 kodlamalı sertifikayı hedef Yönetilen Örneğe yüklemek için şu komutları çalıştırın:

   ```azurepowershell
   $fileContentBytes = Get-Content 'C:/full_path/TDE_Cert.pfx' -Encoding Byte
   $base64EncodedCert = [System.Convert]::ToBase64String($fileContentBytes)
   $securePrivateBlob = $base64EncodedCert  | ConvertTo-SecureString -AsPlainText -Force
   $password = "<password>"
   $securePassword = $password | ConvertTo-SecureString -AsPlainText -Force
   Add-AzSqlManagedInstanceTransparentDataEncryptionCertificate -ResourceGroupName "<resourceGroupName>" `
       -ManagedInstanceName "<managedInstanceName>" -PrivateBlob $securePrivateBlob -Password $securePassword
   ```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Önce *. pfx* dosyanız ile [bir Azure Key Vault](/azure/key-vault/key-vault-manage-with-cli2) ayarlamanız gerekir.

1. PowerShell’deki hazırlık adımlarını başlatın:

   ```azurecli
   # connect to Azure with an interactive dialog for sign-in
   az login

   # list subscriptions available and copy id of the subscription target Managed Instance belongs to
   az account list

   # set subscription for the session
   az account set --subscription <subscriptionId>
   ```

1. Tüm hazırlık adımları tamamladıktan sonra, base-64 kodlamalı sertifikayı hedef Yönetilen Örneğe yüklemek için şu komutları çalıştırın:

   ```azurecli
   az sql mi tde-key set --server-key-type AzureKeyVault --kid "<keyVaultId>" `
       --managed-instance "<managedInstanceName>" --resource-group "<resourceGroupName>"
   ```

* * *

Sertifikaya artık belirtilen Yönetilen Örnekten ulaşılabilir ve buna karşılık gelen TDE korumalı veritabanının yedeği başarılı bir şekilde geri yüklenebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şirket içi veya IaaS SQL Server Azure SQL veritabanı yönetilen örneği ' ne Saydam Veri Şifrelemesi ile veritabanının şifreleme anahtarını koruyan sertifikayı nasıl geçirebileceğiniz öğrendiniz.

Azure SQL Veritabanı Yönetilen Örneği’nde veritabanı yedeğini geri yüklemeyi öğrenmek için bkz. [Veritabanı yedeğini Azure SQL Veritabanı Yönetilen Örneği’ne geri yükleme](sql-database-managed-instance-get-started-restore.md).
