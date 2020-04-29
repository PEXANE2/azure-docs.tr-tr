---
title: 'PowerShell: özel bir etki alanı atama'
description: App Service dağıtımı ve yönetimini otomatik hale getirmek için Azure PowerShell nasıl kullanacağınızı öğrenin. Bu örnekte, bir uygulamaya özel bir etki alanı atama gösterilmektedir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 356f5af9-f62e-411c-8b24-deba05214103
ms.topic: sample
ms.date: 03/20/2017
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 4629157142c17122c0aa6c74c1918c1d01d796c0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80044731"
---
# <a name="assign-a-custom-domain-to-a-web-app-using-powershell"></a>PowerShell kullanarak Web uygulamasına özel bir etki alanı atama

Bu örnek betik, App Service’te ilgili kaynaklarıyla birlikte bir web uygulaması oluşturur ve sonra onu `www.<yourdomain>` ile eşler. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell yükleyip Azure ile bağlantı oluşturmak için öğesini çalıştırın `Connect-AzAccount` . Ayrıca etki alanı kayıt şirketinizin DNS yapılandırma sayfasına erişiminizin olması gerekir.

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/map-custom-domain/map-custom-domain.ps1?highlight=1 "Assign a custom domain to a web app")]

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
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | Web uygulamasının yapılandırmasını değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
