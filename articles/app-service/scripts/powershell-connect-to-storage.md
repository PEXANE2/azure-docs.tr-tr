---
title: 'PowerShell: bir depolama hesabına bağlanma'
description: App Service dağıtımı ve yönetimini otomatik hale getirmek için Azure PowerShell nasıl kullanacağınızı öğrenin. Bu örnek, bir uygulamanın depolama hesabına nasıl bağlanacağını gösterir.
tags: azure-service-management
ms.assetid: e4831bdc-2068-4883-9474-0b34c2e3e255
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc
ms.openlocfilehash: d1c8951445c0be6a3d4b9c4763d719fc7d79d2d5
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74685324"
---
# <a name="connect-an-app-service-app-to-a-storage-account"></a>Bir App Service uygulamasını depolama hesabına bağlama

Bu senaryoda, bir Azure depolama hesabı ve bir App Service uygulaması oluşturmayı öğreneceksiniz. Daha sonra uygulama ayarlarını kullanarak depolama hesabını uygulamaya bağlayacaksınız.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell yükleyip Azure ile bağlantı oluşturmak için öğesini çalıştırın `Connect-AzAccount` .

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/connect-to-storage/connect-to-storage.ps1 "Connect an app to a storage account")]

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
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Bir Depolama hesabı oluşturur. |
| [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) | Azure Depolama hesabının erişim anahtarlarını alır. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | App Service uygulamasının yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service için ek Azure PowerShell örnekleri [Azure PowerShell örneklerde](../samples-powershell.md)bulunabilir.
