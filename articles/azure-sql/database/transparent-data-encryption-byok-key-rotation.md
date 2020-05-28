---
title: TDE koruyucuyu döndürme (PowerShell & Azure CLı)
titleSuffix: Azure SQL Database & Azure Synapse Analytics
description: Azure SQL veritabanı ve Azure SYNAPSE Analytics tarafından PowerShell ve Azure CLı kullanarak kullanılan bir sunucu için Saydam Veri Şifrelemesi (TDE) koruyucusunu döndürme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-lt-2019 sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 03/12/2019
ms.openlocfilehash: 36706372f4b49150aad5511e3d8c6c23f5be12ec
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84051174"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector"></a>Saydam Veri Şifrelemesi (TDE) koruyucusunu döndürme
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]


Bu makalede, Azure Key Vault 'den bir TDE koruyucusu kullanan bir [sunucu](logical-servers.md) için anahtar döndürme açıklanmaktadır. Bir sunucu için mantıksal TDE koruyucuyu döndürmek, sunucudaki veritabanlarını koruyan yeni bir asimetrik anahtara geçiş anlamına gelir. Anahtar döndürme bir çevrimiçi işlemdir ve yalnızca birkaç saniye sürer, çünkü bu yalnızca veritabanının veri şifreleme anahtarını çözer ve veritabanının tamamını değil, yeniden şifreler.

Bu kılavuzda, sunucuda TDE koruyucusunu döndürmek için iki seçenek ele alınmaktadır.

> [!NOTE]
> Duraklatılmış bir Azure SYNAPSE Analytics SQL havuzu, anahtar döndürmelerinde devam etmelidir.

> [!IMPORTANT]
> Bir geçişten sonra anahtarın önceki sürümlerini silmeyin. Anahtarlar devralındığında, bazı veriler, daha eski veritabanı yedeklemeleri gibi önceki anahtarlarla de şifrelenir.

## <a name="prerequisites"></a>Ön koşullar

- Bu nasıl yapılır kılavuzunda, Azure SQL veritabanı veya Azure SYNAPSE için TDE koruyucusu olarak Azure Key Vault bir anahtar kullandığınızı varsaymış olursunuz. Bkz. [BYOK desteği ile saydam veri şifrelemesi](transparent-data-encryption-byok-overview.md).
- Azure PowerShell yüklü ve çalışıyor olmanız gerekir.
- [Önerilir, ancak isteğe bağlı] Önce bir donanım güvenlik modülünde (HSM) veya yerel anahtar deposunda TDE koruyucusu için anahtar malzemesi oluşturun ve Azure Key Vault için anahtar malzemesini içeri aktarın. Daha fazla bilgi edinmek için [donanım güvenlik modülü (HSM) kullanma ve Key Vault yönergelerini](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) izleyin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Az modülü yükleme yönergeleri için bkz. [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps). Belirli cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/).

> [!IMPORTANT]
> PowerShell Azure Resource Manager (RM) modülü hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. AzureRM modülü, en az Aralık 2020 ' e kadar hata düzeltmeleri almaya devam edecektir.  Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumluluklarını hakkında daha fazla bilgi için bkz. [new Azure PowerShell konusuna giriş az Module](/powershell/azure/new-azureps-module-az).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Yükleme için bkz. [Azure CLI 'Yı yükleme](/cli/azure/install-azure-cli).

* * *

## <a name="manual-key-rotation"></a>El ile anahtar döndürme

El ile anahtar döndürme, yeni bir anahtar adı ya da başka bir Anahtar Kasası altında olabilecek tamamen yeni bir anahtar eklemek için aşağıdaki komutları kullanır. Bu yaklaşımın kullanılması, yüksek kullanılabilirlik ve coğrafi Dr senaryolarını desteklemek üzere farklı anahtar kasalarına aynı anahtarı eklemeyi destekler.

> [!NOTE]
> Anahtar Kasası adı ve anahtar adının birleşik uzunluğu 94 karakteri aşamaz.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[Add-AzKeyVaultKey](/powershell/module/az.keyvault/Add-AzKeyVaultKey), [Add-AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey)ve [set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet 'lerini kullanın.

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

[Az keykasa anahtarı oluşturma](/cli/azure/keyvault/key#az-keyvault-key-create), [az SQL Server Key Create](/cli/azure/sql/server/key#az-sql-server-key-create)ve [az SQL Server TDE anahtar kümesi](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-set) komutları kullanın.

```azurecli
# add a new key to Key Vault
az keyvault key create --name <keyVaultKeyName> --vault-name <keyVaultName> --protection <hsmOrSoftware>

# add the new key from Key Vault to the server
az sql server key create --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>

# set the key as the TDE protector for all resources under the server
az sql server tde-key set --server-key-type AzureKeyVault --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
```

* * *

## <a name="switch-tde-protector-mode"></a>TDE koruyucu modunu değiştir

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

- TDE, Microsoft tarafından yönetilen ve BYOK moduna geçiş yapmak için [set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet 'ini kullanın.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector -Type AzureKeyVault `
       -KeyId <keyVaultKeyId> -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- TDE koruyucusunu BYOK modundan Microsoft tarafından yönetilen olarak değiştirmek için [set-AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet 'ini kullanın.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector -Type ServiceManaged `
       -ServerName <logicalServerName> -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Aşağıdaki örneklerde [az SQL Server TDE anahtar kümesi](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector)kullanılır.

- TDE, Microsoft tarafından yönetilen ve BYOK moduna geçiş yapmak için

   ```azurecli
   az sql server tde-key set --server-key-type AzureKeyVault --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
   ```

- TDE koruyucusunu BYOK modundan Microsoft tarafından yönetilen olarak değiştirmek için

   ```azurecli
   az sql server tde-key set --server-key-type ServiceManaged --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
   ```

* * *

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik riskine karşı, riskli olabilecek bir TDE koruyucuyu kaldırma hakkında bilgi edinin: [riskli olabilecek bir anahtarı kaldırma](transparent-data-encryption-byok-remove-tde-protector.md)

- Azure Key Vault tümleştirme ile çalışmaya başlayın ve TDE için Kendi Anahtarını Getir desteğini kullanın: [PowerShell kullanarak Key Vault kendi anahtarınızı kullanarak TDE açma](transparent-data-encryption-byok-configure.md)
