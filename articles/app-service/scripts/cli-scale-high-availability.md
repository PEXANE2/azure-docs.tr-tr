---
title: 'CLı: uygulamayı Traffic Manager ölçeklendirin'
description: Azure CLı kullanarak App Service uygulamanızın dağıtımını ve yönetimini otomatik hale getirmeyi öğrenin. Bu örnek, Traffic Manager ile dünya çapında nasıl ölçeklendirilebilen gösterilmektedir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: e4033a50-0e05-4505-8ce8-c876204b2acc
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 9c1c4ad16a491f7e868a395fa5c36d9605348cdb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97005707"
---
# <a name="scale-an-app-service-app-worldwide-with-a-high-availability-architecture-using-azure-cli"></a>Azure CLı kullanarak yüksek kullanılabilirliğe sahip bir mimariye sahip bir App Service uygulamasını dünya çapında ölçeklendirin

Bu örnek betik bir kaynak grubu, iki App Service planı, iki uygulama, Traffic Manager profili ve iki Traffic Manager uç noktası oluşturur. Alıştırma tamamlandıktan sonra, en düşük ağ gecikmesi temelinde uygulamanızın genel kullanılabilirliğini sağlayan, yüksek oranda kullanılabilir bir mimariye sahip olursunuz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Bu öğretici, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/scale-geographic/scale-geographic.sh "Geographic Scale")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, App Service uygulaması, Traffic Manager profili ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp#az-webapp-create) | App Service uygulaması oluşturur. |
| [`az network traffic-manager profile create`](/cli/azure/network/traffic-manager/profile#az-network-traffic-manager-profile-create) | Bir Azure Traffic Manager profili oluşturur. |
| [`az network traffic-manager endpoint create`](/cli/azure/network/traffic-manager/endpoint#az-network-traffic-manager-endpoint-create) | Azure Traffic Manager profiline bir uç nokta ekler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../samples-cli.md) bulunabilir.
