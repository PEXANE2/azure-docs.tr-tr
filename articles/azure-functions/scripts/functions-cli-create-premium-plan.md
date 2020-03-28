---
title: Premium planda işlev uygulaması oluşturma - Azure CLI
description: Azure CLI'yi kullanarak Azure'da ölçeklenebilir Premium planında işlev uygulaması oluşturma
ms.service: azure-functions
ms.topic: sample
ms.date: 11/23/2019
ms.openlocfilehash: dd31dbadce4f0a55853607504b4322277784f27f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74536270"
---
# <a name="create-a-function-app-in-a-premium-plan---azure-cli"></a>Premium planda işlev uygulaması oluşturma - Azure CLI

Bu Azure İşlevleri örnek betiği, işlevleriniz için kapsayıcı olan bir işlev uygulaması oluşturur. Oluşturulan işlev uygulaması [ölçeklenebilir](../functions-premium-plan.md)premium bir plan kullanır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI sürüm 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu komut [dosyası, Premium planı](../functions-premium-plan.md)kullanarak bir işlev uygulaması oluşturur.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-premium-plan/create-function-app-premium-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Azure Depolama hesabı oluşturur. |
| [az functionapp planı oluşturmak](/cli/azure/functionapp/plan#az-functionapp-plan-create) | Premium plan oluşturur. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | App Service planında bir işlev uygulaması oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
