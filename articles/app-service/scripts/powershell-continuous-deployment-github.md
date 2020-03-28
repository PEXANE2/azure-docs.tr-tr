---
title: "PowerShell: GitHub'dan sürekli dağıtın"
description: Uygulama Hizmeti'nin dağıtımını ve yönetimini otomatikleştirmek için Azure PowerShell'i nasıl kullanacağınızı öğrenin. Bu örnek, GitHub'dan CI/CD ile nasıl bir uygulama oluşturulacak larını gösterir.
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc
ms.openlocfilehash: eee6ac9f9c469f9e1a9344ab4a30626c219d7836
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74685163"
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>GitHub’dan sürekli dağıtım ile bir web uygulaması oluşturma

Bu örnek komut dosyası, ilgili kaynaklarıyla Birlikte App Service'de bir web uygulaması oluşturur ve ardından GitHub deposundan [sürekli dağıtım](../deploy-continuous-deployment.md) ayarlar. Sürekli dağıtım olmadan GitHub dağıtımı için bkz. [Bir web uygulaması oluşturma ve GitHub’dan kod dağıtma](powershell-deploy-github.md).

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell'i yükleyin ve ardından Azure ile bağlantı oluşturmak için çalıştırın. `Connect-AzAccount` Ayrıca, şunları yaptığınızdan emin olun:

- Uygulama kodu, sahip olduğunuz genel veya özel bir GitHub deposunda bulunur. Otomatik yapılar elde etmek için [deponuzu hazırla tablonuza](../deploy-continuous-deployment.md#prepare-your-repository) göre yapılayın.
- [GitHub hesabınızda kişisel erişim jetonu oluşturdunuz.](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line)

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Create a web app with continuous deployment from GitHub")]

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
| [Yeni-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | App Service planı oluşturur. |
| [Yeni-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir web uygulaması oluşturur. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Bir kaynak grubundaki kaynağı değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
