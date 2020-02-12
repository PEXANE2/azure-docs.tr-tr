---
title: Azure İşlevleri çalışma zamanı sürümleri nasıl hedeflenir?
description: Azure İşlevleri, birden fazla çalışma zamanı sürümünü destekler. Azure'da barındırılan bir işlev uygulamasında çalışma zamanı sürümünü nasıl belirteceğinizi öğrenin.
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: 5a71338b1b9735d7e7494dc2667bd7addf5d4a53
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77151964"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Azure İşlevleri çalışma zamanı sürümleri nasıl hedeflenir?

Bir işlev uygulaması, Azure İşlevleri çalışma zamanının belirli bir sürümünde çalışır. Üç ana sürüm vardır: [1. x, 2. x ve 3. x](functions-versions.md). Varsayılan olarak, işlev uygulamaları çalışma zamanının 2. x sürümünde oluşturulur. Bu makalede, Azure'daki bir işlev uygulamasının seçtiğiniz sürüm üzerinde çalıştırılacak şekilde nasıl yapılandırılacağı açıklanmaktadır. Belirli bir sürüm için yerel bir geliştirme ortamının nasıl yapılandırılacağı hakkında bilgi için bkz. [Code ve Azure işlevlerini yerel olarak test](functions-run-local.md)etme.

## <a name="automatic-and-manual-version-updates"></a>Otomatik ve el ile sürüm güncelleştirmeleri

Azure Işlevleri, bir işlev uygulamasındaki `FUNCTIONS_EXTENSION_VERSION` uygulama ayarını kullanarak çalışma zamanının belirli bir sürümünü hedeflemenizi sağlar. İşlev uygulaması siz açıkça yeni bir sürüme geçene kadar belirtilen ana sürüm üzerinde tutulur.

Yalnızca ana sürümü belirtirseniz, işlev uygulaması kullanılabilir hale geldiğinde çalışma zamanının yeni ikincil sürümlerine otomatik olarak güncelleştirilir. Yeni ikincil sürümler, son değişiklikleri sunmamalıdır. İkincil bir sürüm (örneğin, "2.0.12345") belirtirseniz işlev uygulaması, siz açıkça değiştirene kadar ilgili sürüme sabitlenir.

> [!NOTE]
> Azure Işlevleri 'nin belirli bir sürümüne sabitleyebilir ve sonra Visual Studio 'Yu kullanarak Azure 'a yayımlamayı denerseniz, en son sürüme güncelleştirmenizi isteyip istemediğinizi soran bir iletişim kutusu penceresi açılır veya yayımlamayı iptal edersiniz. Bunu önlemek için, `.csproj` dosyanıza `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>` özelliğini ekleyin.

Yeni bir sürüm genel kullanıma sunulduğunda portalda görüntülenecek bir istem size yazılımınızı söz konusu sürüme yükseltme olanağı sunar. Yeni bir sürüme taşıdıktan sonra, `FUNCTIONS_EXTENSION_VERSION` uygulaması ayarını her zaman önceki bir sürüme geri dönmek için kullanabilirsiniz.

Aşağıdaki tabloda, otomatik güncelleştirmeleri etkinleştirmek üzere her ana sürüm için `FUNCTIONS_EXTENSION_VERSION` değerleri gösterilmektedir:

| Ana sürüm | `FUNCTIONS_EXTENSION_VERSION` değeri |
| ------------- | ----------------------------------- |
| 3.x  | `~3` |
| 2.x  | `~2` |
| 'in  | `~1` |

Çalışma zamanı sürümündeki değişiklikler işlev uygulamanızın yeniden başlatılmasına neden olur.

## <a name="view-and-update-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümünü görüntüleme ve güncelleştirme

İşlev uygulamanız tarafından kullanılan çalışma zamanı sürümünü değiştirebilirsiniz. Son değişiklikler nedeniyle, işlev uygulamanızda herhangi bir işlev oluşturmadan önce çalışma zamanı sürümünü değiştirebilirsiniz. 

> [!IMPORTANT]
> Çalışma zamanı sürümü `FUNCTIONS_EXTENSION_VERSION` ayarı tarafından belirlendiği halde, ayarı doğrudan değiştirerek bu değişikliği Azure portal yapmalısınız. Bunun nedeni, portalın yaptığınız değişiklikleri doğrulaması ve ilgili diğer değişiklikleri gerekli hale getirir.

### <a name="from-the-azure-portal"></a>Azure portalından

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> Azure portal kullanarak, zaten işlevleri bulunan bir işlev uygulamasının çalışma zamanı sürümünü değiştiremezsiniz.

### <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Azure CLı 'dan

Ayrıca Azure CLı 'dan `FUNCTIONS_EXTENSION_VERSION` görüntüleyebilir ve ayarlayabilirsiniz.

>[!NOTE]
>Çalışma zamanı sürümü diğer ayarları etkileyebileceğinden sürümü portalda değiştirmeniz gerekir. Çalışma zamanı sürümlerini değiştirdiğinizde portal, Node.js sürümü ve çalışma zamanı yığını gibi diğer gerekli güncelleştirmeleri de otomatik olarak yapar.  

Azure CLı 'yı kullanarak, [az functionapp config appSettings set](/cli/azure/functionapp/config/appsettings) komutuyla geçerli çalışma zamanı sürümünü görüntüleyin.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Bu kodda `<function_app>`, işlev uygulamanızın adıyla değiştirin. Ayrıca `<my_resource_group>`, işlev uygulamanızın kaynak grubunun adıyla değiştirin. 

Aşağıdaki çıktıda `FUNCTIONS_EXTENSION_VERSION` görürsünüz ve bu, açıklık açısından kısaltıldı:

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

İşlev uygulamasındaki `FUNCTIONS_EXTENSION_VERSION` ayarını [az functionapp config appSettings set](/cli/azure/functionapp/config/appsettings) komutuyla güncelleştirebilirsiniz.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

`<function_app>`, işlev uygulamanızın adıyla değiştirin. Ayrıca `<my_resource_group>`, işlev uygulamanızın kaynak grubunun adıyla değiştirin. Ayrıca, `<version>`, 1. x çalışma zamanının geçerli bir sürümüyle veya sürüm 2. x için `~2` ile değiştirin.

Yukarıdaki kod örneğinde **deneyin** ' i seçerek bu komutu [Azure Cloud Shell](../cloud-shell/overview.md) çalıştırabilirsiniz. Ayrıca, oturum açmak için [az Login](/cli/azure/reference-index#az-login) komutunu çalıştırdıktan sonra bu komutu yürütmek IÇIN [Azure CLI 'yı yerel olarak](/cli/azure/install-azure-cli) da kullanabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel geliştirme ortamınızda 2,0 çalışma zamanını hedefleyin](functions-run-local.md)

> [!div class="nextstepaction"]
> [Bkz. çalışma zamanı sürümleri için sürüm notları](https://github.com/Azure/azure-webjobs-sdk-script/releases)
