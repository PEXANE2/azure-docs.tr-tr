---
title: 'PowerShell: Web sunucusu günlükleri ile Izleme'
description: App Service dağıtımı ve yönetimini otomatik hale getirmek için Azure PowerShell nasıl kullanacağınızı öğrenin. Bu örnek, Web sunucusu günlükleri ile bir uygulamanın nasıl izleneceğini gösterir.
tags: azure-service-management
ms.assetid: 5805d7cd-9e56-4eba-bd85-75b013690ff5
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc
ms.openlocfilehash: dbe1e0decf48e8300010d531998fa1fbafccf61b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87033964"
---
# <a name="monitor-a-web-appwith-web-server-logs"></a>Web sunucusu günlükleri ile bir web uygulamasını izleme

Bu senaryoda bir kaynak grubu, app service planı ve web uygulaması oluşturacaksınız ve web uygulamasını web sunucusu günlüklerini etkinleştirecek şekilde yapılandıracaksınız. Ardından, gözden geçirmek üzere günlük dosyalarını indireceksiniz.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/)bulunan yönergeyi kullanarak Azure PowerShell yükleyip `Connect-AzAccount` Azure ile bağlantı oluşturmak için öğesini çalıştırın.

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/monitor-with-logs/monitor-with-logs.ps1 "Monitor a web app with web server logs")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra, kaynak grubunu, web uygulamasını ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | App Service planı oluşturur. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir web uygulaması oluşturur. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | Web uygulamasının yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
