---
title: GitHub dağıtımı ile bir işlev uygulaması oluşturma-Azure CLı
description: Azure İşlevleri’ni kullanarak bir işlev uygulaması oluşturun ve bir GitHub deposundan işlev kodu dağıtın.
ms.date: 07/03/2018
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: 37dc0235b258f6d47f8813546fe953e92799454a
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74532814"
---
# <a name="create-a-function-app-in-azure-that-is-deployed-from-github"></a>Azure’da GitHub’dan dağıtılmış bir işlev uygulaması oluşturma

Bu Azure İşlevleri örnek betiği [tüketim planı](../functions-scale.md#consumption-plan) ve ilgili kaynakları kullanarak bir işlev uygulaması oluşturur. Betik ayrıca işlev kodunuzu bir GitHub deposundan sürekli dağıtım için yapılandırır. 

Bu örnekte aşağıdakilere ihtiyacınız olacak:

* Yönetme izinlerine sahip olduğunuz işlevler kodu ile bir GitHub deposu.
* GitHub hesabınız için bir [Kişisel Erişim Belirteci (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI’yi yerel olarak kullanırsanız, sürüm 2.0 veya sonraki bir sürümü yükleyip kullanmanız gerekir. Azure CLI sürümünü belirlemek için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu örnek, bir Azure İşlev uygulaması oluşturur ve işlev kodunu GitHub'dan dağıtır.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | İşlev uygulaması için gereken depolama hesabını oluşturur. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Sunucusuz [tüketim planında](../functions-scale.md#consumption-plan) bir işlev uygulaması oluşturur ve bunu Git veya Mercurial deposuyla ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
