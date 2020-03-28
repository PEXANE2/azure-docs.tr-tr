---
title: Azure Repos'tan sürekli dağıtım
description: Uygulama Hizmeti uygulamanızın dağıtımını ve yönetimini otomatikleştirmek için Azure CLI'yi nasıl kullanacağınızı öğrenin. Bu örnek, Azure Repos'tan NASıL CI/CD ayarlaylaylayabildiğini gösterir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: c0e9db3825de9c8c1429bec1edcc6bbb65588fcd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80058035"
---
# <a name="create-an-app-service-app-with-continuous-deployment-using-azure-cli"></a>Azure CLI'yi kullanarak sürekli dağıtım içeren bir Uygulama Hizmeti uygulaması oluşturun

Bu örnek komut dosyası, ilgili kaynaklarıyla Birlikte Uygulama Hizmeti'nde bir uygulama oluşturur ve ardından bir Azure DevOps deposundan sürekli dağıtım ayarlar. Bu örnekte şunlar gereklidir:

* Uygulama koduiçeren ve yönetim izinlerine sahip olduğunuz bir Azure DevOps deposu.
* Azure DevOps kuruluşunuz için bir [Kişisel Erişim Belirteci (PAT)](https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Create an app with continuous deployment from Azure DevOps")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Bir Uygulama Hizmeti uygulaması oluşturur. |
| [`az webapp deployment source config`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config) | Bir Uygulama Hizmeti uygulamasını Git veya Mercurial deposuyla ilişkilendirin. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../samples-cli.md) bulunabilir.
