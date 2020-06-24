---
title: "PowerShell: SQL veritabanı 'na bağlanma"
description: App Service dağıtımı ve yönetimini otomatik hale getirmek için Azure PowerShell nasıl kullanacağınızı öğrenin. Bu örnek, bir uygulamanın bir SQL veritabanına nasıl bağlanacağını gösterir.
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc
ms.openlocfilehash: 9086e00e4b6caf89ab249bbf25ca03a6f068ba49
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85248310"
---
# <a name="connect-an-app-service-app-to-sql-database"></a>App Service uygulamasını SQL veritabanına bağlama

Bu senaryoda, Azure SQL veritabanı ve App Service uygulamasında bir veritabanı oluşturmayı öğreneceksiniz. Daha sonra uygulama ayarlarını kullanarak veritabanını uygulamaya bağlayacaksınız.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell yükleyip `Connect-AzAccount` Azure ile bağlantı oluşturmak için öğesini çalıştırın.

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect an app to SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, App Service uygulamayı ve tüm ilgili kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | App Service planı oluşturur. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | App Service uygulaması oluşturur. |
| [New-AzSQLServer](/powershell/module/az.sql/new-azsqlserver) | Bir sunucu oluşturur. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturur. |
| [New-AzSQLDatabase](/powershell/module/az.sql/new-azsqldatabase) | Bir veritabanı veya elastik bir veritabanı oluşturur. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | App Service uygulamasının yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service için ek Azure PowerShell örnekleri [Azure PowerShell örneklerde](../samples-powershell.md)bulunabilir.
