---
title: Azure CLı kullanarak sunucusuz bir işlev uygulaması oluşturma
description: Azure CLı kullanarak Azure 'da sunucusuz yürütme için bir işlev uygulaması oluşturma
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.topic: sample
ms.date: 07/03/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 0fe4cb6e75a74f58b4c3b312f923935053ea4756
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75922603"
---
# <a name="create-a-function-app-for-serverless-code-execution"></a>Sunucusuz kod yürütme için bir işlev uygulaması oluşturma 

Bu Azure İşlevleri örnek betiği, işlevleriniz için kapsayıcı olan bir işlev uygulaması oluşturur. İşlev uygulaması, olay odaklı sunucusuz iş yükleri için ideal olan [Tüketim planı](../functions-scale.md#consumption-plan)kullanılarak oluşturulur.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu betik, [Tüketim planını](../functions-scale.md#consumption-plan)kullanarak bir Azure işlev uygulaması oluşturur.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a Consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Azure Depolama hesabı oluşturur. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Bir işlev uygulaması oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
