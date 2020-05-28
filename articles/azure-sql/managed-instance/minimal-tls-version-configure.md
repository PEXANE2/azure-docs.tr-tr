---
title: Minimum TLS sürümü ile yönetilen örneği yapılandırma
description: Yönetilen örnek için en düşük TLS sürümünü yapılandırmayı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: ''
ms.date: 05/25/2020
ms.openlocfilehash: bf09994e8ebae09771ce110f9284aa3e99fc1816
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84122482"
---
# <a name="configure-minimal-tls-version-in-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği 'nde en düşük TLS sürümünü yapılandırma
Minimum [Aktarım Katmanı Güvenliği (TLS)](https://support.microsoft.com/help/3135244/tls-1-2-support-for-microsoft-sql-server) sürümü ayarı, MÜŞTERILERIN Azure SQL yönetilen örneği tarafından kullanılan TLS sürümünü denetlemesine olanak tanır.

Şu anda TLS 1,0, 1,1 ve 1,2 destekliyoruz. Minimum TLS sürümü ayarlandığında, daha yeni olan TLS sürümlerinin desteklenmesini sağlar. Örneğin, örneğin, 1,1 'den büyük bir TLS sürümü seçme. Yalnızca TLS 1,1 ve 1,2 arasındaki bağlantıların kabul edildiği ve TLS 1,0 reddedildiği anlamına gelir. Uygulamalarınızın bu uygulamayı desteklediğini doğrulamak için test ettikten sonra, önceki sürümlerde bulunan güvenlik açıklarına yönelik düzeltmeler içerdiğinden ve Azure SQL yönetilen örneği 'nde desteklenen en yüksek TLS sürümüne sahip olduğundan, en az TLS sürümünü 1,2 olarak ayarlamayı öneririz.

TLS 'nin eski sürümlerini kullanan uygulamalar için, uygulamalarınızın gereksinimlerine göre en düşük TLS sürümünü ayarlamayı öneririz. Şifrelenmemiş bir bağlantı kullanarak bağlanmak için uygulamalara bağlı olan müşteriler için, en az TLS sürümü ayarlamamız önerilir. 

Daha fazla bilgi için bkz. [SQL veritabanı bağlantısı Için TLS konuları](../database/connect-query-content-reference-guide.md#tls-considerations-for-sql-database-connectivity).

En düşük TLS sürümünü ayarladıktan sonra, sunucunun en düşük TLS sürümünü kullanarak bir TLS sürümü kullanan istemcilerden gelen oturum açma girişimleri aşağıdaki hatayla başarısız olur:

```output
Error 47072
Login failed with invalid TLS version
```

## <a name="set-minimal-tls-version-via-powershell"></a>PowerShell ile en az TLS sürümü ayarla

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır. Aşağıdaki betik [Azure PowerShell modülünü](/powershell/azure/install-az-ps)gerektiriyor.

Aşağıdaki PowerShell betiği, `Get` örnek düzeyinde nasıl ve `Set` **en az TLS sürümü** özelliğinin olduğunu gösterir:

```powershell
#Get the Minimal TLS Version property
(Get-AzSqlInstance -Name sql-instance-name -ResourceGroupName resource-group).MinimalTlsVersion

# Update Minimal TLS Version Property
Set-AzSqlInstance -Name sql-instance-name -ResourceGroupName resource-group -MinimalTlsVersion "1.2"
```

## <a name="set-minimal-tls-version-via-azure-cli"></a>Azure CLı ile en az TLS sürümü ayarlama

> [!IMPORTANT]
> Bu bölümdeki tüm betikler [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)gerektirir.

### <a name="azure-cli-in-a-bash-shell"></a>Bash kabuğu 'nda Azure CLı

Aşağıdaki CLı betiği, bir bash kabuğunda **En düşük TLS sürümü** ayarının nasıl değiştirileceğini göstermektedir:

```azurecli-interactive
# Get current setting for Minimal TLS Version
az sql mi show -n sql-instance-name -g resource-group --query "minimalTlsVersion"

# Update setting for Minimal TLS Version
az sql mi update -n sql-instance-name -g resource-group --set minimalTlsVersion="1.2"
```
