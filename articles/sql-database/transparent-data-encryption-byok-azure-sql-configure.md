---
title: Azure Anahtar Kasası ile SQL TDE'yi etkinleştirme
description: PowerShell veya CLI kullanarak şifreleme için Saydam Veri Şifreleme (TDE) kullanmaya başlamak için Bir Azure SQL Veritabanı ve Veri Ambarı'nı nasıl yapılandıracak şekilde yapılandırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 03/12/2019
ms.openlocfilehash: 81927575b99604e71f7b0920bc3a448f7796f565
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067181"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell ve CLI: Azure Key Vault'tan müşteri tarafından yönetilen anahtarla Şeffaf Veri Şifrelemesini etkinleştirin

Bu makalede, Bir SQL Veritabanı veya Veri Ambarı'nda Saydam Veri Şifreleme (TDE) için Azure Key Vault'tan (TDE) bir anahtarın nasıl kullanılacağı ele ait. Azure Anahtar Kasası tümleştirmesi ile TDE hakkında daha fazla bilgi edinmek için - Kendi Anahtarınızı Getir (BYOK) Desteğinizi getirin, [Azure Key Vault'ta müşteri tarafından yönetilen anahtarlarla TDE'yi](transparent-data-encryption-byok-azure-sql.md)ziyaret edin. 

## <a name="prerequisites-for-powershell"></a>PowerShell için Ön Koşullar

- Azure aboneliğiniz olmalı ve bu abonelikte yönetici olmalısınız.
- [Önerilen ama İsteğe Bağlı] TDE Koruyucu anahtar malzemesinin yerel bir kopyasını oluşturmak için bir donanım güvenlik modülü (HSM) veya yerel anahtar deposuna sahip olun.
- Azure PowerShell'in yüklü ve çalıştırılan olması gerekir.
- TDE için kullanmak üzere bir Azure Anahtar Kasası ve Anahtar oluşturun.
  - [Donanım güvenlik modülü (HSM) ve Key Vault kullanma talimatları](../key-vault/key-vault-hsm-protected-keys.md)
    - Anahtar kasası TDE için kullanılacak aşağıdaki özelliğe sahip olmalıdır:
  - [yumuşak silme](../key-vault/key-vault-ovw-soft-delete.md) ve temizleme koruması
- Anahtar, TDE için kullanılacak aşağıdaki özniteliklere sahip olmalıdır:
   - Son kullanma tarihi yok
   - Devre dışı bırakılmadı
   - *Get,* wrap *tuşu,* *açma tuşu* işlemlerini gerçekleştirebilme

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Az modülü yükleme yönergeleri için bkz. [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps). Belirli cmdlets için [AzureRM.Sql'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın.

Key Vault hakkında ayrıntılı bilgi için Key [Vault'tan PowerShell talimatlarına](../key-vault/quick-create-powershell.md) bakın ve [PowerShell ile Key Vault yumuşak silme nasıl kullanılır.](../key-vault/key-vault-soft-delete-powershell.md)

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

## <a name="assign-an-azure-ad-identity-to-your-server"></a>Sunucunuza Azure AD kimliği atama

Varolan bir sunucunuz varsa, sunucunuza Azure AD kimliği eklemek için aşağıdakileri kullanın:

   ```powershell
   $server = Set-AzSqlServer -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> -AssignIdentity
   ```

Bir sunucu oluşturuyorsanız, sunucu oluşturma sırasında bir Azure AD kimliği eklemek için -Kimlik etiketine sahip [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) cmdlet'i kullanın:

   ```powershell
   $server = New-AzSqlServer -ResourceGroupName <SQLDatabaseResourceGroupName> -Location <RegionName> `
       -ServerName <LogicalServerName> -ServerVersion "12.0" -SqlAdministratorCredentials <PSCredential> -AssignIdentity
   ```

## <a name="grant-key-vault-permissions-to-your-server"></a>Anahtar Kasa sı izinlerini sunucunuza ver

TDE için bir anahtar kullanmadan önce sunucunuza anahtar kasasına erişim izni vermek için [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet'ini kullanın.

   ```powershell
   Set-AzKeyVaultAccessPolicy -VaultName <KeyVaultName> `
       -ObjectId $server.Identity.PrincipalId -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Anahtar Vault tuşunu sunucuya ekleyin ve TDE Koruyucusu'nu ayarlayın

- Anahtar kasasından anahtar kimliğini almak için [Get-AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey?view=azps-2.4.0) cmdlet'ini kullanın
- Anahtar Kasası'ndan sunucuya anahtarı eklemek için [Add-AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) cmdlet'ini kullanın.
- Tüm sunucu kaynakları için anahtarı TDE koruyucusu olarak ayarlamak için [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet'i kullanın.
- TDE koruyucunun beklendiği gibi yapılandırıldığını doğrulamak için [Get-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) cmdlet'i kullanın.

> [!NOTE]
> Anahtar tonoz adı ve anahtar adı için birleştirilmiş uzunluk 94 karakteri geçemez.

> [!TIP]
> Key Vault bir örnek KeyId:https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h

```powershell
# add the key from Key Vault to the server
Add-AzSqlServerKeyVaultKey -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> -KeyId <KeyVaultKeyId>

# set the key as the TDE protector for all resources under the server
Set-AzSqlServerTransparentDataEncryptionProtector -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId>

# confirm the TDE protector was configured as intended
Get-AzSqlServerTransparentDataEncryptionProtector -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName>
```

## <a name="turn-on-tde"></a>TDE'yi aç

TDE'yi açmak için [Set-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) cmdlet'i kullanın.

```powershell
Set-AzSqlDatabaseTransparentDataEncryption -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName> -State "Enabled"
```

Şimdi veritabanı veya veri ambarı TDE Key Vault bir şifreleme anahtarı ile etkin vardır.

## <a name="check-the-encryption-state-and-encryption-activity"></a>Şifreleme durumu ve şifreleme etkinliğini denetleme

Bir veritabanı veya veri ambarı için şifreleme ilerlemesini denetlemek için şifreleme durumunu ve [Get-AzSqlDatabaseTransparentDataEncryptionActivity'i](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) almak için [Get-AzSqlDatabaseTransparentDataEncryption'i](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) kullanın.

```powershell
# get the encryption state
Get-AzSqlDatabaseTransparentDataEncryption -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName> `

# check the encryption progress for a database or data warehouse
Get-AzSqlDatabaseTransparentDataEncryptionActivity -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName>  
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Gerekli Komut Satırı Arabirimi sürüm 2.0 veya sonraki sürümü yüklemek ve Azure aboneliğinize bağlanmak için Azure [Çapraz Platform Komut Satırı Arabirimi 2.0'ı yükleyin ve yapılandırın.](https://docs.microsoft.com/cli/azure/install-azure-cli)

Key Vault ile ilgili ayrıntılar için [CLI 2.0 kullanarak Anahtar Kasasını Yönet](../key-vault/key-vault-manage-with-cli2.md) ve [CLI ile Key Vault yumuşak silme nasıl kullanılır.](../key-vault/key-vault-soft-delete-cli.md)

## <a name="assign-an-azure-ad-identity-to-your-server"></a>Sunucunuza Azure AD kimliği atama

```azurecli
# create server (with identity) and database
az sql server create --name <servername> --resource-group <rgname>  --location <location> --admin-user <user> --admin-password <password> --assign-identity
az sql db create --name <dbname> --server <servername> --resource-group <rgname>
```

> [!TIP]
> Sunucu oluşturma "principalID" tutun, bir sonraki adımda anahtar kasa izinleri atamak için kullanılan nesne kimliği

## <a name="grant-key-vault-permissions-to-your-server"></a>Anahtar Kasa sı izinlerini sunucunuza ver

```azurecli
# create key vault, key and grant permission
az keyvault create --name <kvname> --resource-group <rgname> --location <location> --enable-soft-delete true
az keyvault key create --name <keyname> --vault-name <kvname> --protection software
az keyvault set-policy --name <kvname>  --object-id <objectid> --resource-group <rgname> --key-permissions wrapKey unwrapKey get
```

> [!TIP]
> Bir sonraki adım için yeni anahtarın URI veya keyid anahtarını saklayın, örneğin:https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h

## <a name="add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Anahtar Vault tuşunu sunucuya ekleyin ve TDE Koruyucusu'nu ayarlayın

```azurecli
# add server key and update encryption protector
az sql server key create --server <servername> --resource-group <rgname> --kid <keyID>
az sql server tde-key set --server <servername> --server-key-type AzureKeyVault  --resource-group <rgname> --kid <keyID>
```

> [!NOTE]
> Anahtar tonoz adı ve anahtar adı için birleştirilmiş uzunluk 94 karakteri geçemez.

## <a name="turn-on-tde"></a>TDE'yi aç

```azurecli
# enable encryption
az sql db tde set --database <dbname> --server <servername> --resource-group <rgname> --status Enabled
```

Artık veritabanı veya veri ambarı, Azure Key Vault'ta müşteri tarafından yönetilen bir şifreleme anahtarıyla TDE'yi etkinleştirmiştir.

## <a name="check-the-encryption-state-and-encryption-activity"></a>Şifreleme durumu ve şifreleme etkinliğini denetleme

```azurecli
# get encryption scan progress
az sql db tde list-activity --database <dbname> --server <servername> --resource-group <rgname>  

# get whether encryption is on or off
az sql db tde show --database <dbname> --server <servername> --resource-group <rgname>
```

* * *

## <a name="useful-powershell-cmdlets"></a>Faydalı PowerShell cmdlets

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

- TDE'yi kapatmak için [Set-AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) cmdlet'i kullanın.

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName> `
      -DatabaseName <DatabaseName> -State "Disabled"
   ```

- Sunucuya eklenen Anahtar Kasa anahtarlarının listesini döndürmek için [Get-AzSqlServerKeyVaultKey](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) cmdlet'ini kullanın.

   ```powershell
   # KeyId is an optional parameter, to return a specific key version
   Get-AzSqlServerKeyVaultKey -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

- Sunucudan bir Key Vault anahtarını kaldırmak için [Remove-AzSqlServerKeyVaultKey](/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) tuşunu kullanın.

   ```powershell
   # the key set as the TDE Protector cannot be removed
   Remove-AzSqlServerKeyVaultKey -KeyId <KeyVaultKeyId> -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

- Genel veritabanı ayarları için [az sql'e](/cli/azure/sql)bakın.

- Kasa tuşu ayarları için [az sql server tuşuna](/cli/azure/sql/server/key)bakın.

- TDE ayarları için [az sql server tde-key](/cli/azure/sql/server/tde-key) ve az sql [db tde'ye](/cli/azure/sql/db/tde)bakın.

* * *

## <a name="troubleshooting"></a>Sorun giderme

Bir sorun oluşursa aşağıdakileri denetleyin:

- Anahtar kasası bulunamıyorsa, doğru abonelikte olduğunuzdan emin olun.

   # <a name="powershell"></a>[Powershell](#tab/azure-powershell)

   ```powershell
   Get-AzSubscription -SubscriptionId <SubscriptionId>
   ```

   # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

   ```powershell
   az account show - s <SubscriptionId>
   ```

   * * *

- Sunucuya yeni anahtar eklenemiyorsa veya yeni anahtar TDE Koruyucusu olarak güncelleştirilemiyorsa, aşağıdakileri denetleyin:
   - Anahtarın son kullanma tarihi olmamalıdır
   - Anahtar, *get*, *wrap tuşu*ve anahtar işlemlerini *açma* etkin olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik gereksinimlerine uyacak şekilde sunucunun TDE Koruyucusunu nasıl döndürdüğünüzi öğrenin: [PowerShell'i kullanarak Şeffaf Veri Şifreleme koruyucusunu döndürün.](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- Güvenlik riski durumunda, tehlikeye girme potansiyeline karşı bir TDE Koruyucusu'nun nasıl kaldırılış yapılacağını öğrenin: [Tehlikeye girme potansiyeline bağlı bir anahtarı kaldırın.](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)
