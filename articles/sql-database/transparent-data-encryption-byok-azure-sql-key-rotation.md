---
title: TDE koruyucuyu döndürme-PowerShell
description: Azure SQL Server için Saydam Veri Şifrelemesi (TDE) koruyucusunu döndürme hakkında bilgi edinin.
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
ms.openlocfilehash: 553d8535d2fdbd7daa5c93535c7c4bd51f2da1a1
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74995814"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>PowerShell kullanarak Saydam Veri Şifrelemesi (TDE) koruyucusunu döndürme

Bu makalede, Azure Key Vault 'den bir TDE koruyucusu kullanılarak Azure SQL Server için anahtar döndürme açıklanmaktadır. Azure SQL Server 'ın TDE koruyucusunu döndürme, sunucudaki veritabanlarını koruyan yeni bir asimetrik anahtara geçiş anlamına gelir. Anahtar döndürme bir çevrimiçi işlemdir ve yalnızca birkaç saniye sürer, çünkü bu yalnızca veritabanının veri şifreleme anahtarını çözer ve veritabanının tamamını değil, yeniden şifreler.

Bu kılavuzda, sunucuda TDE koruyucusunu döndürmek için iki seçenek ele alınmaktadır.

> [!NOTE]
> Duraklatılmış bir SQL veri ambarı, anahtar döndürmelerinde devam etmelidir.

> [!IMPORTANT]
> Bir geçişten sonra anahtarın önceki sürümlerini silmeyin. Anahtarlar devralındığında, bazı veriler, daha eski veritabanı yedeklemeleri gibi önceki anahtarlarla de şifrelenir.

## <a name="prerequisites"></a>Önkoşullar

- Bu nasıl yapılır kılavuzunda, bir Azure SQL veritabanı veya veri ambarı için TDE koruyucusu olarak Azure Key Vault bir anahtar kullandığınızı varsaymış olursunuz. Bkz. [BYOK desteği ile saydam veri şifrelemesi](transparent-data-encryption-byok-azure-sql.md).
- Azure PowerShell yüklü ve çalışıyor olmanız gerekir.
- [Önerilir, ancak isteğe bağlı] Önce bir donanım güvenlik modülünde (HSM) veya yerel anahtar deposunda TDE koruyucusu için anahtar malzemesi oluşturun ve Azure Key Vault için anahtar malzemesini içeri aktarın. Daha fazla bilgi edinmek için [donanım güvenlik modülü (HSM) kullanma ve Key Vault yönergelerini](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) izleyin.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Az modülü yükleme yönergeleri için bkz. [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps). Belirli cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/).

> [!IMPORTANT]
> PowerShell Azure Resource Manager (RM) modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. AzureRM modülü, en az Aralık 2020 ' e kadar hata düzeltmeleri almaya devam edecektir.  Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Uyumluluklarını hakkında daha fazla bilgi için bkz. [new Azure PowerShell konusuna giriş az Module](/powershell/azure/new-azureps-module-az).

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Yükleme için bkz. [Azure CLI 'Yı yükleme](/cli/azure/install-azure-cli).

* * *

## <a name="manual-key-rotation"></a>El ile anahtar döndürme

El ile anahtar döndürme, yeni bir anahtar adı ya da başka bir Anahtar Kasası altında olabilecek tamamen yeni bir anahtar eklemek için aşağıdaki komutları kullanır. Bu yaklaşımın kullanılması, yüksek kullanılabilirlik ve coğrafi Dr senaryolarını desteklemek üzere farklı anahtar kasalarına aynı anahtarı eklemeyi destekler.

> [!NOTE]
> Anahtar Kasası adı ve anahtar adının birleşik uzunluğu 94 karakteri aşamaz.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

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

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

[Az keykasa anahtarı oluşturma](/cli/azure/keyvault/key#az-keyvault-key-create), [az SQL Server Key Create](/cli/azure/sql/server/key#az-sql-server-key-create)ve [az SQL Server TDE anahtar kümesi](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-set) komutları kullanın.

```azure-cli
# add a new key to Key Vault
az keyvault key create --name <keyVaultKeyName> --vault-name <keyVaultName> --protection <hsmOrSoftware>

# add the new key from Key Vault to the server
az sql server key create --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>

# set the key as the TDE protector for all resources under the server
az sql server tde-key set --server-key-type AzureKeyVault --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
```

* * *

## <a name="useful-powershell-cmdlets"></a>Yararlı PowerShell cmdlet'leri

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

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

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Aşağıdaki örneklerde [az SQL Server TDE anahtar kümesi](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector)kullanılır.

- TDE, Microsoft tarafından yönetilen ve BYOK moduna geçiş yapmak için

   ```azure-cli
   az sql server tde-key set --server-key-type AzureKeyVault --kid <keyVaultKeyId> --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
   ```

- TDE koruyucusunu BYOK modundan Microsoft tarafından yönetilen olarak değiştirmek için

   ```azure-cli
   az sql server tde-key set --server-key-type ServiceManaged --resource-group <SQLDatabaseResourceGroupName> --server <logicalServerName>
   ```

* * *

## <a name="next-steps"></a>Sonraki adımlar

- Güvenlik riskine karşı, riskli olabilecek bir TDE koruyucuyu kaldırma hakkında bilgi edinin: [riskli olabilecek bir anahtarı kaldırma](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)

- Azure Key Vault tümleştirme ile çalışmaya başlayın ve TDE için Kendi Anahtarını Getir desteğini kullanın: [PowerShell kullanarak Key Vault kendi anahtarınızı kullanarak TDE açma](transparent-data-encryption-byok-azure-sql-configure.md)
