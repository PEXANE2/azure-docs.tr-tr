---
title: TDE koruyucusunu çıkarın - PowerShell
description: TDE'yi Kendi Anahtar (BYOK) desteğiyle TDE kullanarak bir Azure SQL Veritabanı veya Veri Ambarı için olası bir TDE koruyucusu için yanıt verme kılavuzu.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 02/24/2020
ms.openlocfilehash: 5a89c3f7d52c5717b902a69e9c64b3fcc422c481
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067204"
---
# <a name="remove-a-transparent-data-encryption-tde-protector-using-powershell"></a>PowerShell kullanarak Şeffaf Veri Şifreleme (TDE) koruyucusunu kaldırma

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz olmalı ve bu abonelikte yönetici olmalısınız
- Azure PowerShell'in yüklü ve çalıştırılan olması gerekir.
- Bu nasıl yapılır kılavuzu, Azure SQL Veritabanı veya Veri Ambarı için TDE koruyucusu olarak Azure Key Vault'tan bir anahtar zaten kullandığınızı varsayar. Daha fazla bilgi edinmek için [BYOK Desteği ile Şeffaf Veri Şifreleme'ye](transparent-data-encryption-byok-azure-sql.md) bakın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

 Az modülü yükleme yönergeleri için bkz. [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps). Belirli cmdlets için [AzureRM.Sql'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın.

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Yükleme için Azure [CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın.

* * *

## <a name="overview"></a>Genel Bakış

Bu nasıl yapılır kılavuzu, Azure Key Vault - Bring Own Key (BYOK) desteğinde müşteri tarafından yönetilen anahtarlarla TDE kullanan bir Azure SQL Veritabanı veya Veri Ambarı için tehlikeye girme potansiyeline sahip bir TDE koruyucuya nasıl yanıt verilebildiğini açıklar. TDE için BYOK desteği hakkında daha fazla bilgi edinmek için [genel bakış sayfasına](transparent-data-encryption-byok-azure-sql.md)bakın.

Aşağıdaki yordamlar yalnızca aşırı durumlarda veya test ortamlarında yapılmalıdır. Azure Key Vault'tan etkin olarak kullanılan TDE koruyucularını silmeden **veritabanının kullanılamamasına**neden olacağından, nasıl yapılacağını kılavuzu dikkatle gözden geçirin.

Bir anahtarın ele geçirilmeşüphesi varsa (örneğin, bir hizmet veya kullanıcının anahtara yetkisiz erişimi varsa), anahtarı silmek en iyisidir.

TDE koruyucusu Key Vault'ta silindikten sonra, 10 dakikaya kadar tüm şifreli veritabanlarının ilgili hata iletisi ile tüm bağlantıları reddetmeye başlayacağını ve durumunu [Erişilemez](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql#inaccessible-tde-protector)olarak değiştireceğini unutmayın.

Aşağıdaki adımlar, belirli bir veritabanının Sanal Günlük Dosyaları (VLF) tarafından hala kullanılmakta olan TDE Koruyucu parmak izlerini nasıl kontrol edileceğini özetler.
Veritabanının geçerli TDE koruyucusu ve veritabanı kimliği çalıştırılarak bulunabilir:

```sql
SELECT [database_id],
       [encryption_state],
       [encryptor_type], /*asymmetric key means AKV, certificate means service-managed keys*/
       [encryptor_thumbprint],
 FROM [sys].[dm_database_encryption_keys]
```

Aşağıdaki sorgu VLF'leri ve şifreleyiciyi kullanımdaki ilgili parmak izlerini döndürür. Her farklı parmak izi Azure Key Vault (AKV) farklı anahtar anlamına gelir:

```sql
SELECT * FROM sys.dm_db_log_info (database_id)
```

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

PowerShell komutu **Get-AzureRmSqlServerKeyKey sorguda** kullanılan TDE Koruyucusu'nun parmak izini sağlar, böylece AKV'de hangi tuşların ve hangi tuşların silinir olduğunu görebilirsiniz. Yalnızca veritabanı tarafından artık kullanılmayan anahtarlar Azure Key Vault'tan güvenle silinebilir.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

PowerShell **komutu az sql server key show** sorguda kullanılan TDE Koruyucusu'nun parmak izini sağlar, böylece akv'de hangi tuşların ve hangi tuşların silinir olduğunu görebilirsiniz. Yalnızca veritabanı tarafından artık kullanılmayan anahtarlar Azure Key Vault'tan güvenle silinebilir.

* * *

Bu nasıl yapılacağını kılavuzu, olay yanıtından sonra istenen sonuca bağlı olarak iki yaklaşımın üzerinden geçer:

- Azure SQL veritabanlarını / Veri Ambarlarını **erişilebilir** tutmak için
- Azure SQL veritabanları / Veri Ambarları **erişilemez** hale getirmek için

## <a name="to-keep-the-encrypted-resources-accessible"></a>Şifrelenmiş kaynakları erişilebilir tutmak için

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

1. Key [Vault'ta yeni](/powershell/module/az.keyvault/add-azkeyvaultkey)bir anahtar oluşturun. Erişim denetimi kasa düzeyinde sağlandığından, bu yeni anahtarın potansiyel olarak tehlikeye giren TDE koruyucudan ayrı bir anahtar kasasında oluşturulduğundan emin olun.

2. [Add-AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) ve [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlets kullanarak sunucuya yeni anahtar ekleyin ve sunucunun yeni TDE koruyucusu olarak güncelleyin.

   ```powershell
   # add the key from Key Vault to the server  
   Add-AzSqlServerKeyVaultKey -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> -KeyId <KeyVaultKeyId>

   # set the key as the TDE protector for all resources under the server
   Set-AzSqlServerTransparentDataEncryptionProtector -ResourceGroupName <SQLDatabaseResourceGroupName> `
       -ServerName <LogicalServerName> -Type AzureKeyVault -KeyId <KeyVaultKeyId>
   ```

3. Sunucunun ve tüm kopyaların [Get-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) cmdlet'i kullanarak yeni TDE koruyucuya güncelleştirdiğinden emin olun.

   > [!NOTE]
   > Yeni TDE koruyucunun sunucu nun altındaki tüm veritabanlarına ve ikincil veritabanlarına yayılması birkaç dakika sürebilir.

   ```powershell
   Get-AzSqlServerTransparentDataEncryptionProtector -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

4. Key [Vault'taki yeni anahtarın yedeğini](/powershell/module/az.keyvault/backup-azkeyvaultkey) al.

   ```powershell
   # -OutputFile parameter is optional; if removed, a file name is automatically generated.
   Backup-AzKeyVaultKey -VaultName <KeyVaultName> -Name <KeyVaultKeyName> -OutputFile <DesiredBackupFilePath>
   ```

5. [Kaldır-AzKeyVaultKey](/powershell/module/az.keyvault/remove-azkeyvaultkey) cmdlet'i kullanarak Anahtar Kasası'ndan tehlikeye geçirilen anahtarı silin.

   ```powershell
   Remove-AzKeyVaultKey -VaultName <KeyVaultName> -Name <KeyVaultKeyName>
   ```

6. [Geri Yükleme-AzKeyVaultKey](/powershell/module/az.keyvault/restore-azkeyvaultkey) cmdlet kullanarak gelecekte Key Vault için bir anahtar geri yüklemek için:

   ```powershell
   Restore-AzKeyVaultKey -VaultName <KeyVaultName> -InputFile <BackupFilePath>
   ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Komut başvurusu için [Azure CLI keyvault'a](/cli/azure/keyvault/key)bakın.

1. Key [Vault'ta yeni](/cli/azure/keyvault/key#az-keyvault-key-create)bir anahtar oluşturun. Erişim denetimi kasa düzeyinde sağlandığından, bu yeni anahtarın potansiyel olarak tehlikeye giren TDE koruyucudan ayrı bir anahtar kasasında oluşturulduğundan emin olun.

2. Sunucuya yeni anahtarı ekleyin ve sunucunun yeni TDE koruyucusu olarak güncelleştirin.

   ```azurecli
   # add the key from Key Vault to the server  
   az sql server key create --kid <KeyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <LogicalServerName>

   # set the key as the TDE protector for all resources under the server
   az sql server tde-key set --server-key-type AzureKeyVault --kid <KeyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <LogicalServerName>
   ```

3. Sunucunun ve yinelemelerin yeni TDE koruyucusu için güncelleştirdiğinden emin olun.

   > [!NOTE]
   > Yeni TDE koruyucunun sunucu nun altındaki tüm veritabanlarına ve ikincil veritabanlarına yayılması birkaç dakika sürebilir.

   ```azurecli
   az sql server tde-key show --resource-group <SQLDatabaseResourceGroupName> --server <LogicalServerName>
   ```

4. Key Vault'taki yeni anahtarın yedeğini al.

   ```azurecli
   # --file parameter is optional; if removed, a file name is automatically generated.
   az keyvault key backup --file <DesiredBackupFilePath> --name <KeyVaultKeyName> --vault-name <KeyVaultName>
   ```

5. Anahtar Kasası'ndan tehlikeye alınan anahtarı silin.

   ```azurecli
   az keyvault key delete --name <KeyVaultKeyName> --vault-name <KeyVaultName>
   ```

6. Gelecekte Key Vault için bir anahtar geri yüklemek için.

   ```azurecli
   az keyvault key restore --file <BackupFilePath> --vault-name <KeyVaultName>
   ```

* * *

## <a name="to-make-the-encrypted-resources-inaccessible"></a>Şifrelenmiş kaynakları erişilemez hale getirmek için

1. Tehlikeye girme potansiyeli olan anahtar tarafından şifrelenen veritabanlarını bırakın.

   Veritabanı ve günlük dosyaları otomatik olarak yedeklenir, böylece veritabanının zamanında geri yüklemesi herhangi bir noktada (anahtarı sağladığınız sürece) yapılabilir. En son işlemlerin 10 dakikasına kadar olası veri kaybını önlemek için veritabanları etkin bir TDE koruyucusu silinmeden önce bırakılmalıdır.

2. Anahtar Kasası'ndaki TDE koruyucunun anahtar malzemesini yedekle.
3. Key Vault'tan tehlikeye atma potansiyeline olan anahtarı kaldırın

[!INCLUDE [sql-database-akv-permission-delay](includes/sql-database-akv-permission-delay.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik gereksinimlerine uymak için sunucunun TDE koruyucusunu nasıl döndürdüğünüzi öğrenin: [PowerShell'i kullanarak Şeffaf Veri Şifreleme koruyucusunu döndürün](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- TDE için Kendi Anahtar Desteğinizi Getir: [PowerShell'i kullanarak Key Vault'tan kendi anahtarınızı kullanarak TDE'yi açın](transparent-data-encryption-byok-azure-sql-configure.md)
