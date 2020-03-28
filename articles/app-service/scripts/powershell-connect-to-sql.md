---
title: 'PowerShell: SQL Veritabanına Bağlanın'
description: Uygulama Hizmeti'nin dağıtımını ve yönetimini otomatikleştirmek için Azure PowerShell'i nasıl kullanacağınızı öğrenin. Bu örnek, bir uygulamanın BIR SQL Veritabanına nasıl bağlanılsüreceğini gösterir.
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc
ms.openlocfilehash: bb5fc8bcc99a4439276f53325c029635143c86c5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74685394"
---
# <a name="connect-an-app-service-app-to-a-sql-database"></a>Uygulama Hizmeti uygulamasını SQL veritabanına bağlama

Bu senaryoda, bir Azure SQL veritabanı ve Bir Uygulama Hizmeti uygulaması oluşturmayı öğreneceksiniz. Daha sonra uygulama ayarlarını kullanarak SQL veritabanını uygulamaya bağlarsınız.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell'i yükleyin ve ardından Azure ile bağlantı oluşturmak için çalıştırın. `Connect-AzAccount`

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect an app to a SQL database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Komut dosyası örneği çalıştırıldıktan sonra kaynak grubunu, App Service uygulamasını ve ilgili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | App Service planı oluşturur. |
| [Yeni-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir Uygulama Hizmeti uygulaması oluşturur. |
| [Yeni-AzSQLServer](/powershell/module/az.sql/new-azsqlserver) | SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | SQL Veritabanı sunucusu için bir güvenlik duvarı kuralı oluşturur. |
| [Yeni-AzSQLVeritabanı](/powershell/module/az.sql/new-azsqldatabase) | Bir veritabanı veya elastik bir veritabanı oluşturur. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | Bir Uygulama Hizmeti uygulamasının yapılandırması değişir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure Uygulama Hizmeti için ek Azure [Powershell örnekleri Azure PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
