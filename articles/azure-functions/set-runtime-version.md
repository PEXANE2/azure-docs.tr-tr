---
title: Azure İşlevleri çalışma zamanı sürümleri nasıl hedeflenir?
description: Azure İşlevleri, birden fazla çalışma zamanı sürümünü destekler. Azure'da barındırılan bir işlev uygulamasında çalışma zamanı sürümünü nasıl belirteceğinizi öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: glenga
ms.openlocfilehash: 6e9ac50f38b2cf7bc3531e58e87ff2a8768c0a45
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69650480"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Azure İşlevleri çalışma zamanı sürümleri nasıl hedeflenir?

Bir işlev uygulaması, Azure İşlevleri çalışma zamanının belirli bir sürümünde çalışır. İki ana sürüm vardır: [1. x ve 2. x](functions-versions.md). İşlev uygulamaları, varsayılan olarak çalışma zamanının 2.x sürümünde oluşturulur. Bu makalede, Azure'daki bir işlev uygulamasının seçtiğiniz sürüm üzerinde çalıştırılacak şekilde nasıl yapılandırılacağı açıklanmaktadır. Yerel bir geliştirme ortamını belirli bir sürüm için yapılandırma hakkında bilgi için bkz.[Code and test Azure Functions locally (Azure İşlevleri'ni yerel olarak kodlama ve test etme)](functions-run-local.md).

## <a name="automatic-and-manual-version-updates"></a>Otomatik ve el ile sürüm güncelleştirmeleri

Azure işlevleri, bir işlev uygulamasındaki `FUNCTIONS_EXTENSION_VERSION` uygulama ayarını kullanarak çalışma zamanının belirli bir sürümünü hedeflemenizi sağlar. İşlev uygulaması siz açıkça yeni bir sürüme geçene kadar belirtilen ana sürüm üzerinde tutulur.

Yalnızca ana sürümü (2.x için "~2" veya 1.x için "~1") belirtirseniz işlev uygulaması, çalışma zamanının yeni ikincil sürümleri kullanıma sunulduğunda otomatik olarak bu sürümlere güncelleştirilir. Yeni ikincil sürümler hataya neden olan değişiklikler içermez. İkincil bir sürüm (örneğin, "2.0.12345") belirtirseniz işlev uygulaması, siz açıkça değiştirene kadar ilgili sürüme sabitlenir.

> [!NOTE]
> Azure Işlevleri 'nin belirli bir sürümüne sabitleyebilir ve sonra Visual Studio 'Yu kullanarak Azure 'a yayımlamayı denerseniz, en son sürüme güncelleştirmenizi isteyip istemediğinizi soran bir iletişim kutusu penceresi açılır veya yayımlamayı iptal edersiniz. Bunu önlemek için, `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>` `.csproj` dosyanıza özelliği ekleyin.

Yeni bir sürüm genel kullanıma sunulduğunda portalda görüntülenecek bir istem size yazılımınızı söz konusu sürüme yükseltme olanağı sunar. Yeni bir sürüme yükseltme yaptıktan sonra dilediğiniz zaman `FUNCTIONS_EXTENSION_VERSION` ayarını kullanarak önceki bir sürüme geri dönebilirsiniz.

Çalışma zamanı sürümündeki değişiklikler işlev uygulamanızın yeniden başlatılmasına neden olur.

Otomatik güncelleştirmeleri etkinleştirmek için `FUNCTIONS_EXTENSION_VERSION` uygulama ayarında şu anda 1.x çalışma zamanı için "~1" ve 2.x için "~2" değerini belirleyebilirsiniz.

## <a name="view-and-update-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümünü görüntüleme ve güncelleştirme

İşlev uygulamanız tarafından kullanılan çalışma zamanı sürümünü değiştirebilirsiniz. Son değişiklikler nedeniyle, işlev uygulamanızda herhangi bir işlev oluşturmadan önce çalışma zamanı sürümünü değiştirebilirsiniz. Çalışma zamanı sürümü `FUNCTIONS_EXTENSION_VERSION` ayarı tarafından belirlendiği halde, ayarı doğrudan değiştirip Azure Portal bu değişikliği yapmanız gerekir. Bunun nedeni, portalın yaptığınız değişiklikleri doğrulaması ve ilgili diğer değişiklikleri gerekli hale getirir.

### <a name="from-the-azure-portal"></a>Azure portal

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> Azure portal kullanarak, zaten işlevleri bulunan bir işlev uygulamasının çalışma zamanı sürümünü değiştiremezsiniz.

### <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Azure CLı 'dan

`FUNCTIONS_EXTENSION_VERSION` ayarını Azure CLI'sinden de görüntüleyebilir ve belirleyebilirsiniz.

>[!NOTE]
>Çalışma zamanı sürümü diğer ayarları etkileyebileceğinden sürümü portalda değiştirmeniz gerekir. Çalışma zamanı sürümlerini değiştirdiğinizde portal, Node.js sürümü ve çalışma zamanı yığını gibi diğer gerekli güncelleştirmeleri de otomatik olarak yapar.  

Azure CLI'sini kullanarak [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) komutuyla geçerli çalışma zamanı sürümünü görüntüleyebilirsiniz.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Bu kodun `<function_app>` kısmını işlev uygulamanızın adıyla, `<my_resource_group>` kısmını ise işlev uygulamanıza ilişkin kaynak grubunun adıyla değiştirin. 

Kolayca anlaşılması için kesilmiş olan aşağıdaki çıktıda `FUNCTIONS_EXTENSION_VERSION` ayarını görebilirsiniz.

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

`FUNCTIONS_EXTENSION_VERSION` işlev uygulaması ayarını [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) komutu ile güncelleştirebilirsiniz.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

İşlev `<function_app>` uygulamanızın adıyla değiştirin. `<my_resource_group>` kısmını ise işlev uygulamanıza ilişkin kaynak grubunun adıyla değiştirin. Ayrıca `<version>`kısmını, 1.x çalışma zamanının geçerli bir sürümüyle veya 2.x sürümü için `~2` ile değiştirin.

Önceki kod örneğinde **Deneyin** seçeneğini belirleyerek bu komutu [Azure Cloud Shell](../cloud-shell/overview.md) üzerinde de çalıştırabilirsiniz. Oturum açmak için [az login](/cli/azure/reference-index#az-login) komutunu yürüttükten sonra bu komutu yürütmek için Azure [Azure CLI'yi yerel olarak](/cli/azure/install-azure-cli) da kullanabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel geliştirme ortamınızda 2.0 çalışma zamanını hedefleyin](functions-run-local.md)

> [!div class="nextstepaction"]
> [Bkz. çalışma zamanı sürümleri için sürüm notları](https://github.com/Azure/azure-webjobs-sdk-script/releases)
