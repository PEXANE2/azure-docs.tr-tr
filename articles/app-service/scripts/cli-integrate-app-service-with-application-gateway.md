---
title: Azure CLı betik örneği-App Service Application Gateway ile tümleştirin | Microsoft Docs
description: Azure CLı betik örneği-App Service Application Gateway ile tümleştirin
services: appservice
documentationcenter: appservice
author: madsd
manager: ccompy
editor: ''
tags: azure-service-management
ms.assetid: 6c16d6f8-3c08-4a59-858e-684a2ceccb5f
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/09/2019
ms.author: madsd
ms.custom: seodec18, devx-track-azurecli
ms.openlocfilehash: 3ff268ca77fc855853b6506141a25c761341b451
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495514"
---
# <a name="integrate-app-service-with-application-gateway-using-cli"></a>CLı kullanarak Application Gateway App Service tümleştirme

Bu örnek betik, bir Azure App Service Web uygulaması, bir Azure sanal ağı ve bir Application Gateway oluşturur. Daha sonra, Web uygulaması için trafiği yalnızca Application Gateway alt ağından kaynaklanan şekilde kısıtlar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz, Azure CLı sürüm 2.0.74 veya sonraki bir sürümü gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/integrate-with-app-gateway/integrate-with-app-gateway.sh "Integrate with Application Gateway")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, App Service uygulaması, Cosmos DB ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [`az group create`](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [`az network vnet create`](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest#az-network-vnet-create) | Sanal ağ oluşturur. |
| [`az network public-ip create`](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) | Genel bir IP adresi oluşturur. |
| [`az network public-ip show`](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-show) | Genel bir IP adresinin ayrıntılarını gösterin. |
| [`az appservice plan create`](https://docs.microsoft.com/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | App Service planı oluşturur. |
| [`az webapp create`](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Bir App Service Web uygulaması oluşturur. |
| [`az webapp show`](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-show) | App Service bir Web uygulamasının ayrıntılarını gösterin. |
| [`az webapp config access-restriction add`](https://docs.microsoft.com/cli/azure/webapp/config/access-restriction?view=azure-cli-latest#az-webapp-config-access-restriction-add) | App Service Web uygulamasına bir erişim kısıtlaması ekler. |
| [`az network application-gateway create`](https://docs.microsoft.com/cli/azure/network/application-gateway?view=azure-cli-latest#az-network-application-gateway-create) | Bir Application Gateway oluşturur. |
| [`az network application-gateway http-settings update`](https://docs.microsoft.com/cli/azure/network/application-gateway/http-settings?view=azure-cli-latest#az-network-application-gateway-http-settings-update) | Application Gateway HTTP ayarlarını güncelleştirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../samples-cli.md) bulunabilir.
