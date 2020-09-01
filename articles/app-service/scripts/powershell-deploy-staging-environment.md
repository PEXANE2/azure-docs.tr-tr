---
title: 'PowerShell: hazırlama yuvasına kod dağıtma'
description: App Service dağıtımı ve yönetimini otomatik hale getirmek için Azure PowerShell nasıl kullanacağınızı öğrenin. Bu örnek, bir hazırlama ortamına kod dağıtmayı gösterir.
tags: azure-service-management
ms.assetid: 27cf0680-c3a9-4a58-9f71-6dec09f6b874
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: 1024aa360202b11d0cb1317db359f9c2e661ebb9
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89070386"
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a>Bir web uygulaması oluşturma ve hazırlama ortamına kod dağıtma

Bu örnek betik, App Service’te "hazırlama" adlı ek bir dağıtım yuvası ile bir web uygulaması oluşturur ve sonra "hazırlama" yuvasını örnek bir uygulama dağıtır.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/)bulunan yönergeyi kullanarak Azure PowerShell yükleyip `Connect-AzAccount` Azure ile bağlantı oluşturmak için öğesini çalıştırın.

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.ps1?highlight=1 "Create a web app and deploy code to a staging environment")]

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
| [Set-AzAppServicePlan](/powershell/module/az.websites/set-azappserviceplan) | Fiyatlandırma katmanını değiştirmek için App Service planını değiştirir. |
| [New-AzWebAppSlot](/powershell/module/az.websites/new-azwebappslot) | Bir web uygulaması için dağıtım yuvası oluşturur. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Bir kaynak grubundaki kaynağı değiştirir. |
| [Anahtar-AzWebAppSlot](/powershell/module/az.websites/switch-azwebappslot) | Bir web uygulamasının dağıtım yuvasını üretime taşır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
