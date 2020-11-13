---
title: DevOps dağıtımı ile bir işlev uygulaması oluşturma-Azure CLı
description: Bir İşlev Uygulaması oluşturma ve işlev kodunu Azure DevOps'tan dağıtma
ms.date: 07/03/2018
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 25e0f43a851bbf7658847aa52cc9ee778941f718
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565220"
---
# <a name="create-a-function-in-azure-that-is-deployed-from-azure-devops"></a>Azure DevOps 'tan dağıtılan Azure 'da bir işlev oluşturma

Bu konu başlığı altında, [Tüketim planını](../functions-scale.md#consumption-plan)kullanarak [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir Işlev uygulaması oluşturmak için Azure işlevlerinin nasıl kullanılacağı gösterilmektedir. İşlevleriniz için bir kapsayıcı olan işlev uygulaması, sürekli olarak bir Azure DevOps deposundan dağıtılır. 

Bu konuyu tamamlamak için şunlara sahip olmalısınız:

* İşlev uygulaması projenizi içeren ve yönetim izinlerine sahip olduğunuz bir Azure DevOps deposu.
* Azure DevOps deponuza erişmek için bir [kişisel erişim belirteci (PAT)](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Bu öğretici, Azure CLı 'nin 2,0 veya sonraki bir sürümünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür. 

## <a name="sample-script"></a>Örnek betik

Bu örnek, bir Azure İşlev uygulaması oluşturur ve işlev kodunu Azure DevOps'tan dağıtır.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, depolama hesabı, işlev uygulaması ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | İşlev uygulaması için gereken depolama hesabını oluşturur. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Sunucusuz [Tüketim planında](../functions-scale.md#consumption-plan)bir işlev uygulaması oluşturur. |
| [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config) | Bir işlev uygulamasını Git veya Mercurial deposu ile ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
