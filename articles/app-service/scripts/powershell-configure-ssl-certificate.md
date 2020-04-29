---
title: 'PowerShell: TLS/SSL yükleme ve bağlama'
description: App Service dağıtımı ve yönetimini otomatik hale getirmek için Azure PowerShell nasıl kullanacağınızı öğrenin. Bu örnekte, özel bir TLS/SSL sertifikasının bir uygulamaya nasıl bağlanacağı gösterilmektedir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 23e83b74-614a-49a0-bc08-7542120eeec5
ms.topic: sample
ms.date: 03/20/2017
ms.custom: mvc, seodec18
ms.openlocfilehash: 5116585b701717a82b757cae70f938c321a1f7d1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81532570"
---
# <a name="bind-a-custom-tlsssl-certificate-to-a-web-app-using-powershell"></a>PowerShell kullanarak özel bir TLS/SSL sertifikasını bir Web uygulamasına bağlama

Bu örnek betik, App Service içinde ilgili kaynaklarıyla bir Web uygulaması oluşturur ve sonra özel bir etki alanı adının TLS/SSL sertifikasını bu sertifikaya bağlar. 

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeyi kullanarak Azure PowerShell yükleyip Azure ile bağlantı oluşturmak için öğesini çalıştırın `Connect-AzAccount` . Ayrıca, şunları yaptığınızdan emin olun:

- `az login` komutunu kullanarak Azure ile bir bağlantı oluşturuldu.
- Etki alanı kayıt şirketinizin DNS yapılandırma sayfasına erişiminiz var.
- Geçerli bir. Karşıya yüklemek ve bağlamak istediğiniz TLS/SSL sertifikası için PFX dosyası ve parolası.

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom TLS/SSL certificate to a web app")]

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
| [New-AzWebAppSSLBinding](/powershell/module/az.websites/new-azwebappsslbinding) | Bir Web uygulaması için TLS/SSL sertifikası bağlaması oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure App Service Web Apps için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../samples-powershell.md) bölümünde bulabilirsiniz.
