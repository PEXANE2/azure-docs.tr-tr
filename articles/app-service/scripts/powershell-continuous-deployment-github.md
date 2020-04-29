---
title: "PowerShell: GitHub 'dan sürekli olarak dağıtın"
description: App Service dağıtımı ve yönetimini otomatik hale getirmek için Azure PowerShell nasıl kullanacağınızı öğrenin. Bu örnek, GitHub 'dan CI/CD ile bir uygulamanın nasıl oluşturulacağını gösterir.
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc
ms.openlocfilehash: eee6ac9f9c469f9e1a9344ab4a30626c219d7836
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74685163"
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>GitHub’dan sürekli dağıtım ile bir web uygulaması oluşturma

Bu örnek betik, App Service içinde ilgili kaynaklarıyla birlikte bir Web uygulaması oluşturur ve sonra bir GitHub deposundan [sürekli dağıtımı](../deploy-continuous-deployment.md) ayarlar. Sürekli dağıtım olmadan GitHub dağıtımı için bkz. [Bir web uygulaması oluşturma ve GitHub’dan kod dağıtma](powershell-deploy-github.md).

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell yükleyip Azure ile bağlantı oluşturmak için öğesini çalıştırın `Connect-AzAccount` . Ayrıca, şunları yaptığınızdan emin olun:

- Uygulama kodu, sahip olduğunuz genel veya özel bir GitHub deposunda bulunur. Otomatik derlemeler almak için, deponuzu [hazırlama](../deploy-continuous-deployment.md#prepare-your-repository) tablonuza göre yapısını yapın.
- [GitHub hesabınızda bir kişisel erişim belirteci oluşturdunuz](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line).

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
| [New-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | App Service planı oluşturur. |
| [New-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Bir web uygulaması oluşturur. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Bir kaynak grubundaki kaynağı değiştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
