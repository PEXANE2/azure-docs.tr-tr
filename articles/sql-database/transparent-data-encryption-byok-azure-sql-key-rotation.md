---
title: TDE koruyucusunu döndürün - PowerShell
description: Bir Azure SQL sunucusu için Saydam Veri Şifreleme (TDE) koruyucusu nasıl döndürdüğünüzi öğrenin.
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
ms.openlocfilehash: aaed06ac086893f63fde530e46b936b3fb637766
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067179"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>PowerShell kullanarak Şeffaf Veri Şifreleme (TDE) koruyucusunu döndürme

Bu makalede, Azure Anahtar Kasası'ndan bir TDE koruyucusu kullanan bir Azure SQL sunucusuiçin anahtar döndürme açıklanmaktadır. Azure SQL sunucusunun TDE koruyucusu döndürmek, sunucudaki veritabanlarını koruyan yeni bir asimetrik anahtara geçmek anlamına gelir. Anahtar döndürme çevrimiçi bir işlemdir ve tamamlanması yalnızca birkaç saniye sürer, çünkü bu yalnızca veritabanının tüm veritabanını değil veritabanının veri şifreleme anahtarının şifresini çözer ve yeniden şifreler.

Bu kılavuzda, Sunucudaki TDE koruyucuyu döndürmek için iki seçenek anlatılmaktadır.

> [!NOTE]
> Anahtar döndürmeden önce duraklatılmış bir SQL Veri Ambarı'na devam edilmelidir.

> [!IMPORTANT]
> Rollover'dan sonra anahtarın önceki sürümlerini silmeyin. Anahtarlar devrildiğinde, bazı veriler eski veritabanı yedeklemeleri gibi önceki anahtarlarla yine de şifrelenir.

## <a name="prerequisites"></a>Ön koşullar

- Bu nasıl yapılır kılavuzu, Azure SQL Veritabanı veya Veri Ambarı için TDE koruyucusu olarak Azure Key Vault'tan bir anahtar zaten kullandığınızı varsayar. [BYOK Desteği ile Şeffaf Veri Şifreleme'ye](transparent-data-encryption-byok-azure-sql.md)bakın.
- Azure PowerShell'in yüklü ve çalıştırılan olması gerekir.
- [Önerilen ancak isteğe bağlı] Önce donanım güvenlik modülünde (HSM) veya yerel anahtar deposunda TDE koruyucusu için anahtar malzemeoluşturun ve anahtar malzemeyi Azure Anahtar Kasası'na aktarın. Daha fazla bilgi [edinmek için donanım güvenlik modülü (HSM) ve Key Vault kullanma yönergelerini](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) izleyin.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Az modülü yükleme yönergeleri için bkz. [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps). Belirli cmdlets için [AzureRM.Sql'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın.

> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi (RM) modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirmeler Az.Sql modülü içindir. AzureRM modülü en az Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecektir.  Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumlulukları hakkında daha fazla bilgi için [yeni Azure PowerShell Az modüllerini tanıtın.](/powershell/azure/new-azureps-module-az)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Yükleme için Azure [CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın.

* * *

## <a name="manual-key-rotation"></a>Manuel anahtar döndürme

El ile anahtar döndürme, yeni bir anahtar adı veya hatta başka bir anahtar kasası altında olabilecek tamamen yeni bir anahtar eklemek için aşağıdaki komutları kullanır. Bu yaklaşımın kullanılması, yüksek kullanılabilirlik ve geo-dr senaryolarını desteklemek için farklı anahtar kasalarına aynı anahtarı eklemeyi destekler.

> [!NOTE]
> Anahtar tonoz adı ve anahtar adı için birleştirilmiş uzunluk 94 karakteri geçemez.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[Ekle-AzKeyVaultKey,](/powershell/module/az.keyvault/Add-AzKeyVaultKey) [Add-AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey)ve [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlets kullanın.

```powershell
# add a new key to Key Vault
Add-AzKeyVaultKey -VaultName <keyVaultName> -Name <keyVaultKeyName> -Destination <hardwareOrSoftware>

# add the new key from Key Vault to the server
Add-AzSqlServerKeyVaultKey -KeyId <keyVaultKeyId> -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
  
# set the key as the TDE protector for all resources under the server
Set-AzSqlServerTransparentDataEncryptionProtector -Type AzureKeyVault -KeyId <keyVaultKeyId> `
   -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Az [keyvault tuşunu kullanarak oluştur](/cli/azure/keyvault/key#az-keyvault-key-create), [az sql server key create](/cli/azure/sql/server/key#az-sql-server-key-create), ve az sql server [tde-key set](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-set) komutları.

```azurecli
# add a new key to Key Vault
az keyvault key create --name <keyVaultKeyName> --vault-name <keyVaultName> --protection <hsmOrSoftware>

# add the new key from Key Vault to the server
az sql server key create --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>

# set the key as the TDE protector for all resources under the server
az sql server tde-key set --server-key-type AzureKeyVault --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
```

* * *

## <a name="useful-powershell-cmdlets"></a>Faydalı PowerShell cmdlets

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

- TDE koruyucuyu Microsoft tarafından yönetilen BYOK moduna geçmek için [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet'i kullanın.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector -Type AzureKeyVault `
       -KeyId <keyVaultKeyId> -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- TDE koruyucuyu BYOK modundan Microsoft'un yönetimindeki ayarı geçmek için [Set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet'i kullanın.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector -Type ServiceManaged `
       -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Aşağıdaki [örneklerde az sql server tde-key kümesi](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector)kullanılır.

- TDE koruyucuyu Microsoft tarafından yönetilen BYOK moduna geçmek için,

   ```azurecli
   az sql server tde-key set --server-key-type AzureKeyVault --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
   ```

- TDE koruyucuyu BYOK modundan Microsoft tarafından yönetilen bir şekilde değiştirmek için,

   ```azurecli
   az sql server tde-key set --server-key-type ServiceManaged --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
   ```

* * *

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik riski durumunda, tehlikeye girme potansiyeline karşı bir TDE koruyucusu nasıl kaldırılacak: [Tehlikeye girme potansiyeline bağlı bir anahtarı kaldırın](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)

- Azure Key Vault entegrasyonu ve TDE için Kendi Anahtar desteğinizi getir: [PowerShell'i kullanarak Key Vault'tan kendi anahtarınızı kullanarak TDE'yi açın](transparent-data-encryption-byok-azure-sql-configure.md)
