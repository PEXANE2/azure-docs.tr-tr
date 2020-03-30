---
title: Azure CLI Script Örneği - Uygulamaların yüksek kullanılabilirliği için rota trafiği | Microsoft Dokümanlar
description: Azure CLI Script Örneği - Uygulamaların yüksek kullanılabilirliği için rota trafiği
services: traffic-manager
documentationcenter: traffic-manager
author: asudbring
manager: KumudD
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 06/26/2018
ms.author: allensu
ms.openlocfilehash: 2a083520a0da324d7033d4597a6b734b43c025b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74049267"
---
# <a name="route-traffic-for-high-availability-of-applications---azure-cli"></a>Uygulamaların yüksek kullanılabilirliği için rota trafiği - Azure CLI

Bu komut dosyası bir kaynak grubu, iki uygulama hizmeti planı, iki web uygulaması, bir trafik yöneticisi profili ve iki trafik yöneticisi bitiş noktası oluşturur. Trafik Yöneticisi trafiği birincil bölge olarak bir bölgedeki uygulamaya ve birincil bölgedeki uygulama kullanılamadığında ikincil bölgeye yönlendirir. Komut dosyasını uygulamadan önce MyWebApp, MyWebAppL1 ve MyWebAppL2 değerlerini Azure genelinde benzersiz değerlerle değiştirmeniz gerekir. Komut dosyasını çalıştırdıktan sonra, birincil bölgedeki uygulamaya URL mywebapp.trafficmanager.net ile erişebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Komut dosyası örneği çalıştırıldıktan sonra, kaynak grubunu, Uygulama Hizmeti uygulamasını ve ilgili tüm kaynakları kaldırmak için izleme komutu kullanılabilir.

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, web uygulaması, traffic manager profili ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan) | App Service planı oluşturur. Bu, Azure web uygulamanız için bir sunucu çiftliği gibidir. |
| [az webapp create](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Uygulama Hizmeti planı içinde bir Azure web uygulaması oluşturur. |
| [az ağ trafik yöneticisi profili oluşturmak](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile) | Bir Azure Traffic Manager profili oluşturur. |
| [az ağ trafik yöneticisi bitiş noktası oluşturmak](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint) | Azure Traffic Manager profiline bir uç nokta ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI komut dosyası örnekleri [Azure Ağ belgelerinde](../cli-samples.md)bulunabilir.
