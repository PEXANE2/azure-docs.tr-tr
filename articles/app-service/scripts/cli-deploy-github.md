---
title: "CLI: GitHub'dan bir uygulama dağıtma"
description: Uygulama Hizmeti uygulamanızın dağıtımını ve yönetimini otomatikleştirmek için Azure CLI'yi nasıl kullanacağınızı öğrenin. Bu örnek, GitHub'dan bir uygulamanın nasıl dağıtılandığını gösterir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 368a5d6810de5628ab98a7bef43cfceab750f95f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80057834"
---
# <a name="create-an-app-service-app-with-deployment-from-github-using-azure-cli"></a>Azure CLI'yi kullanarak GitHub'dan dağıtım la birlikte bir Uygulama Hizmeti uygulaması oluşturun

Bu örnek komut dosyası, ilgili kaynakları yla Birlikte App Service'de bir uygulama oluşturur. Ardından uygulama kodunuzu herkese açık bir GitHub deposundan (sürekli dağıtım olmadan) dağıtur. Sürekli dağıtıma sahip GitHub dağıtımı için [bkz.](cli-continuous-deployment-github.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "Create an app with deployment from GitHub")]

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
