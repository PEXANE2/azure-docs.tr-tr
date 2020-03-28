---
title: "PowerShell: SSL'yi yükleme ve bağlama"
description: Uygulama Hizmeti'nin dağıtımını ve yönetimini otomatikleştirmek için Azure PowerShell'i nasıl kullanacağınızı öğrenin. Bu örnek, özel bir SSL sertifikasının bir uygulamaya nasıl bağlanılsüreceğini gösterir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc, seodec18
ms.openlocfilehash: 8422c4ef29366888a2bdd53c18462e6cfb680c78
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80044557"
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app-using-powershell"></a>PowerShell'i kullanarak özel bir SSL sertifikasını bir web uygulamasına bağlama

Bu örnek betik, App Service'te ilgili kaynaklarıyla birlikte bir web uygulaması oluşturur, ardından bu web uygulamasına özel bir etki alanı adının SSL sertifikasını bağlar. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell'i yükleyin ve ardından Azure ile bağlantı oluşturmak için çalıştırın. `Connect-AzAccount` Ayrıca, şunları yaptığınızdan emin olun:

- `az login` komutunu kullanarak Azure ile bir bağlantı oluşturuldu.
- Etki alanı kayıt şirketinizin DNS yapılandırma sayfasına erişiminiz var.
- Geçerli bir .PFX dosyanız ve karşıya yükleyip bağlamak istediğiniz SSL sertifikası için parolanız var.

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

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
| [Set-AzAppServicePlan](/powershell/module/az.websites/set-azappserviceplan) | Fiyatlandırma katmanını değiştirmek için App Service planını değiştirir. |
| [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) | Web uygulamasının yapılandırmasını değiştirir. |
| [Yeni-azwebappsslbinding](/powershell/module/az.websites/new-azwebappsslbinding) | Web uygulaması için bir SSL sertifikası bağlaması oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
