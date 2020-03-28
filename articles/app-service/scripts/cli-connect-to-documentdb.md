---
title: "CLI: Bir uygulamayı Cosmos DB'ye bağlayın"
description: Uygulama Hizmeti uygulamanızın dağıtımını ve yönetimini otomatikleştirmek için Azure CLI'yi nasıl kullanacağınızı öğrenin. Bu örnek, bir uygulamayı MongoDB'ye (Cosmos DB) nasıl bağlayabilirsiniz şekilde gösterir.
author: msangapu-msft
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 911282e71491c86fefa86d342e916cdff4fa8c9b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80058659"
---
# <a name="connect-an-app-service-app-to-cosmos-db-using-cli"></a>CLI kullanarak bir Uygulama Hizmeti uygulamasını Cosmos DB'ye bağlayın

Bu örnek komut dosyası, Azure Cosmos DB'nin MongoDB için API'sini ve Uygulama Hizmeti uygulamasını kullanarak bir Azure Cosmos DB hesabı oluşturur. Daha sonra uygulama ayarlarını kullanarak web uygulamasına bir MongoDB bağlantı dizesini bağlar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, bir kaynak grubu, App Service uygulaması, Cosmos DB ve ilgili tüm kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Bir Uygulama Hizmeti uygulaması oluşturur. |
| [`az cosmosdb create`](/cli/azure/cosmosdb?view=azure-cli-latest#az-cosmosdb-create) | Bir Cosmos DB hesabı oluşturur. |
| [`az cosmosdb list-connection-strings`](/cli/azure/cosmosdb?view=azure-cli-latest#az-cosmosdb-list-connection-strings) | Belirtilen Cosmos DB hesabı için bağlantı dizelerini listeler. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) | Bir Uygulama Hizmeti uygulaması için bir uygulama ayarı oluşturur veya günceller. Uygulama ayarları, uygulamanız için ortam değişkenleri olarak kullanıma sunulur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../samples-cli.md) bulunabilir.
