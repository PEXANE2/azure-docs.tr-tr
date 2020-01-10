---
title: Azure CLI kullanarak ilk işlevinizi oluşturma
description: Azure CLI ve Azure Functions Core Tools kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/13/2018
ms.topic: quickstart
ms.custom: mvc
ms.openlocfilehash: 222e4a98974a1af40ff860cfc4fdb246d9c97bca
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75769396"
---
# <a name="quickstart-create-your-first-function-from-the-command-line-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak komut satırından ilk işlevinizi oluşturma

Bu hızlı başlangıç konusu, komut satırından veya terminalden ilk işlevinizi oluşturma hakkında bilgi vermektedir. İşlev uygulaması oluşturmak için, işlevinizi barındıran [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir altyapı olan Azure CLI’yi kullanın. İşlev kodu projesi, bir uygulamadan, işlev uygulaması projesini Azure’ye dağıtmak için de kullanılan [Azure Functions Core Tools](functions-run-local.md) kullanılarak bir şablondan oluşturulur.

Mac, Windows veya Linux bilgisayar kullanarak aşağıdaki adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmadan önce aşağıdakilere sahip olmanız gerekir:

+ [Azure Functions Core Tools](./functions-run-local.md#v2) sürüm 2.6.666 veya üstünü yükler.

+ [Azure CLI](/cli/azure/install-azure-cli)’yi yükleyin. Bu makale için Azure CLı 2,0 veya sonraki bir sürümü gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. [Azure Cloud Shell](https://shell.azure.com/bash)’i de kullanabilirsiniz.

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-create-function-app-cli](../../includes/functions-create-function-app-cli.md)]

## <a name="enable-extension-bundles"></a>Uzantı paketlerini etkinleştir

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanızı sağlar. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak bir işlev uygulaması oluşturun.

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<APP_NAME>` yer tutucusunun ve `<STORAGE_NAME>` depolama hesabı adının yerine ekleyin. `<APP_NAME>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Ayrıca, işlev uygulamanız için `dotnet` (C#) veya `node` (JavaScript) `<language>` çalışma zamanını da ayarlamanız gerekir.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <APP_NAME> --storage-account  <STORAGE_NAME> --runtime <language>
```

_Consumption-plan-location_ parametresini ayarlamak, işlev uygulamasının bir Tüketim barındırma planında barındırıldığı anlamına gelir. Bu sunucusuz planda, işlevleriniz gerektirdikçe kaynaklar dinamik olarak eklenir ve yalnızca işlevler çalışırken ücret ödersiniz. Daha fazla bilgi için bkz. [Doğru barındırma planını seçme](functions-scale.md).

İşlev uygulaması oluşturulduktan sonra Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

