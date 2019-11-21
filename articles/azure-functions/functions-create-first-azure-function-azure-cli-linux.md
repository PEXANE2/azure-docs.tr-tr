---
title: Azure'da ilk Linux işlevinizi oluşturma
description: Learn how to create your first function hosted on Linux in Azure using the command line tools, Azure Functions Core Tools and the Azure CLI.
ms.date: 03/12/2019
ms.topic: quickstart
ms.custom: mvc, fasttrack-edit
ms.openlocfilehash: 19abfee69db53c560dfa2696d85f8c1c3d770c09
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230807"
---
# <a name="quickstart-create-your-first-function-hosted-on-linux-using-command-line-tools"></a>Quickstart: Create your first function hosted on Linux using command line tools

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu [sunucusuz](https://azure.com/serverless) bir Linux ortamında yürütmenize olanak tanır. Linux-hosting requires [the Functions 2.x runtime](functions-versions.md). Serverless functions run in the [Consumption plan](functions-scale.md#consumption-plan).

Bu hızlı başlangıç makalesi, Azure CLI ile Linux üzerinde çalışan ilk işlev uygulamanızı oluşturma adımlarını göstermektedir. İşlev kodu yerel ortamda oluşturulur ve ardından [Azure Functions Core Tools](functions-run-local.md) ile Azure'a dağıtılır.

Aşağıdaki adımlar Mac, Windows veya Linux bilgisayarlarda desteklenir. Bu makalede JavaScript veya C# ile işlev oluşturma adımları gösterilmektedir. To learn how to create Python functions, see [Create your first Python function using Core Tools and the Azure CLI](functions-create-first-function-python.md).

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmadan önce aşağıdakilere sahip olmanız gerekir:

+ Install [Azure Functions Core Tools](./functions-run-local.md#v2) version 2.6.666 or above.

+ [Azure CLI]( /cli/azure/install-azure-cli)’yi yükleyin. Bu makale, Azure CLI 2.0 veya sonraki bir sürümü gerektirir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. [Azure Cloud Shell](https://shell.azure.com/bash)’i de kullanabilirsiniz.

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-create-function-app-cli](../../includes/functions-create-function-app-cli.md)]

## <a name="enable-extension-bundles"></a>Enable extension bundles

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Azure'da bir Linux işlev uygulaması oluşturma

Linux’ta işlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun yürütülmesi için sunucusuz bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak Linux üzerinde çalışan bir işlev uygulaması oluşturun.

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. Bu ad Azure'daki tüm uygulamalar arasında benzersiz olmalıdır. You should also set the `<language>` runtime for your function app, from `dotnet` (C#), `node` (JavaScript/TypeScript), or `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westus --os-type Linux \
--name <app_name> --storage-account  <storage_name> --runtime <language>
```

İşlev uygulaması oluşturulduktan sonra şu ileti görüntülenir:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Artık projenizi Azure'daki yeni işlev uygulamasında yayımlayabilirsiniz.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]