---
title: Uygulamaların HA için rota trafiği - Azure PowerShell - Trafik Yöneticisi
description: Azure PowerShell komut dosyası örneği - Uygulamaların yüksek kullanılabilirliği için rota trafiği
services: traffic-manager
documentationcenter: traffic-manager
author: rohinkoul
manager: kumudD
editor: ''
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 04/26/2018
ms.author: rohink
ms.openlocfilehash: 3f448683dbb210ab196e27cfd98a376429d03aec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76934744"
---
# <a name="route-traffic-for-high-availability-of-applications-using-azure-powershell"></a>Azure PowerShell kullanarak uygulamaların yüksek kullanılabilirliği için rota trafiği

Bu komut dosyası bir kaynak grubu, iki uygulama hizmeti planı, iki web uygulaması, bir trafik yöneticisi profili ve iki trafik yöneticisi bitiş noktası oluşturur. Trafik Yöneticisi trafiği birincil bölge olarak bir bölgedeki uygulamaya ve birincil bölgedeki uygulama kullanılamadığında ikincil bölgeye yönlendirir. Komut dosyasını uygulamadan önce MyWebApp, MyWebAppL1 ve MyWebAppL2 değerlerini Azure genelinde benzersiz değerlerle değiştirmeniz gerekir. Komut dosyasını çalıştırdıktan sonra, birincil bölgedeki uygulamaya URL mywebapp.trafficmanager.net ile erişebilirsiniz.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure)bulunan yönergeyi kullanarak Azure PowerShell'i yükleyin ve ardından Azure ile bağlantı oluşturmak için çalıştırın. `Connect-AzAccount`

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.ps1 "Route traffic for high availability")]


Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup1
Remove-AzResourceGroup -Name myResourceGroup2
```


## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, web uygulaması, traffic manager profili ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)  | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzAppServicePlan](/powershell/module/az.websites/new-azappserviceplan) | App Service planı oluşturur. Bu, Azure web uygulamanız için bir sunucu çiftliği gibidir. |
| [Yeni-AzWebApp](/powershell/module/az.websites/new-azwebapp) | Uygulama Hizmeti planı içinde bir Azure web uygulaması oluşturur. |
| [Set-AzResource](/powershell/module/az.resources/new-azresource) | Uygulama Hizmeti planı içinde bir Azure web uygulaması oluşturur. |
| [Yeni-AzTrafficManagerProfile](/powershell/module/az.trafficmanager/new-aztrafficmanagerprofile) | Bir Azure Traffic Manager profili oluşturur. |
| [Yeni-AzTrafficManagerEndpoint](/powershell/module/az.trafficmanager/new-aztrafficmanagerendpoint) | Azure Traffic Manager profiline bir uç nokta ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/overview).

Ek ağ PowerShell betiği örnekleri, [Azure Ağına Genel Bakış belgeleri](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json) içinde bulunabilir.
