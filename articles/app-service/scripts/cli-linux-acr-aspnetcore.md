---
title: "CLı: ACR 'den ASP.NET Core uygulaması oluşturma"
description: Azure CLı kullanarak App Service uygulamanızın dağıtımını ve yönetimini otomatik hale getirmeyi öğrenin. Bu örnekte ACR 'den bir Linux ASP.NET Core uygulamasının nasıl oluşturulacağı gösterilmektedir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 3a2d1983-ff7b-476a-ac44-49ec2aabb31a
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/13/2018
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 64177d00b302f9df8a0b28067031bd7cc1b3a156
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80057764"
---
# <a name="create-an-aspnet-core-app-in-a-docker-container-in-app-service-from-azure-container-registry"></a>Azure Container Registry App Service bir Docker kapsayıcısında ASP.NET Core uygulaması oluşturma

Bu örnek betik bir kaynak grubu, bir Linux App Service planı ve bir uygulama oluşturur. Daha sonra Azure Container Registry’den bir Docker Kapsayıcısı kullanarak ASP.NET Core uygulamasını dağıtır.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Azure CLı sürüm 2.0.52 veya sonraki bir sürümü gereklidir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-linux-acr/deploy-linux-acr.sh "Linux Azure Container Registry")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, App Service uygulaması ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | App Service uygulaması oluşturur. |
| [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) | App Service uygulaması için Docker kapsayıcısını ayarlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../samples-cli.md) bulunabilir.
