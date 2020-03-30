---
title: Azure İşlevleri çalışma zamanı sürümlerini hedefleme
description: Azure İşlevler çalışma zamanının birden çok sürümlerini destekler. Azure'da barındırılan bir işlev uygulamasının çalışma zamanı sürümünü nasıl belirtin.
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: 5a71338b1b9735d7e7494dc2667bd7addf5d4a53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77151964"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Azure İşlevleri çalışma zamanı sürümlerini hedefleme

Bir işlev uygulaması Azure İşlevleri çalışma zamanının belirli bir sürümünde çalışır. Üç ana versiyonu vardır: [1.x, 2.x ve 3.x](functions-versions.md). Varsayılan olarak, işlev uygulamaları çalışma zamanının sürüm 2.x'inde oluşturulur. Bu makalede, Seçtiğiniz sürümde çalışacak şekilde Azure'daki bir işlev uygulamasını nasıl yapılandırabileceğiniz açıklanmaktadır. Belirli bir sürüm için yerel geliştirme ortamını nasıl yapılandırılayacağıhakkında bilgi için [Kod'a bakın ve Azure İşlevlerini yerel olarak test edin.](functions-run-local.md)

## <a name="automatic-and-manual-version-updates"></a>Otomatik ve manuel sürüm güncellemeleri

Azure İşlevler, bir işlev uygulamasındauygulama ayarını `FUNCTIONS_EXTENSION_VERSION` kullanarak çalışma zamanının belirli bir sürümünü hedeflemenize olanak tanır. İşlev uygulaması, yeni bir sürüme geçmeyi açıkça seçene kadar belirtilen ana sürümde tutulur.

Yalnızca ana sürümü belirtirseniz, işlev uygulaması kullanılabilir olduklarında çalışma zamanının yeni küçük sürümlerine otomatik olarak güncelleştirilir. Yeni küçük sürümler kırılma değişiklikleri tanıtmak olmamalıdır. Küçük bir sürüm belirtirseniz (örneğin, "2.0.12345"), işlev uygulaması, siz açıkça değiştirene kadar belirli bir sürüme sabitlenir.

> [!NOTE]
> Azure İşlevlerinin belirli bir sürümüne sabitleyip Visual Studio'yu kullanarak Azure'da yayımlamaya çalışırsanız, en son sürüme güncelleştirmenizi veya yayımlamayı iptal etmenize neden olan bir iletişim penceresi açılır. Bunu önlemek için `<DisableFunctionExtensionVersionUpdate>true</DisableFunctionExtensionVersionUpdate>` dosyanızdaki `.csproj` özelliği ekleyin.

Yeni bir sürüm herkese açık olduğunda, portaldaki bir istem size bu sürüme geçme şansı verir. Yeni bir sürüme geçtikten sonra, `FUNCTIONS_EXTENSION_VERSION` önceki sürüme geri dönmek için uygulama ayarını her zaman kullanabilirsiniz.

Aşağıdaki tablo, `FUNCTIONS_EXTENSION_VERSION` otomatik güncelleştirmeleri etkinleştirmek için her ana sürümün değerlerini gösterir:

| Ana sürüm | `FUNCTIONS_EXTENSION_VERSION`Değer |
| ------------- | ----------------------------------- |
| 3.x  | `~3` |
| 2.x  | `~2` |
| 1.x  | `~1` |

Çalışma zamanı sürümünde yapılan bir değişiklik, işlev uygulamasının yeniden başlatılmasına neden olur.

## <a name="view-and-update-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümünü görüntüleme ve güncelleştirme

İşlev uygulamanız tarafından kullanılan çalışma zamanı sürümünü değiştirebilirsiniz. Değişiklikleri bölme potansiyeli nedeniyle, yalnızca işlev uygulamanızda herhangi bir işlev oluşturmadan önce çalışma zamanı sürümünü değiştirebilirsiniz. 

> [!IMPORTANT]
> Çalışma zamanı sürümü `FUNCTIONS_EXTENSION_VERSION` ayarı tarafından belirlense de, bu değişikliği doğrudan ayarı değiştirerek değil, Azure portalında yapmalısınız. Bunun nedeni, portalın değişikliklerinizi doğrulayabı ve gerektiğinde diğer ilgili değişiklikleri yapmasıdır.

### <a name="from-the-azure-portal"></a>Azure portalından

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

> [!NOTE]
> Azure portalını kullanarak, zaten işlevler içeren bir işlev uygulamasının çalışma zamanı sürümünü değiştiremezsiniz.

### <a name="from-the-azure-cli"></a><a name="view-and-update-the-runtime-version-using-azure-cli"></a>Azure CLI'den

Azure `FUNCTIONS_EXTENSION_VERSION` CLI'yi görüntüleyebilir ve ayarlayabilirsiniz.

>[!NOTE]
>Diğer ayarlar çalışma zamanı sürümünden etkilenebileceğinden, portaldaki sürümü değiştirmeniz gerekir. Portal, çalışma zamanı sürümlerini değiştirdiğinizde Node.js sürümü ve çalışma zamanı yığını gibi diğer gerekli güncelleştirmeleri otomatik olarak yapar.  

Azure CLI'yi kullanarak, geçerli çalışma zamanı sürümünü [az functionapp config appsettings kümesi](/cli/azure/functionapp/config/appsettings) komutuyla görüntüleyin.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Bu kodda, `<function_app>` işlev uygulamanızın adı ile değiştirin. Ayrıca `<my_resource_group>` işlev uygulamanız için kaynak grubunun adı ile değiştirin. 

Netlik `FUNCTIONS_EXTENSION_VERSION` için kesilen aşağıdaki çıktıyı görürsünüz:

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

Fonksiyon uygulamasındaki `FUNCTIONS_EXTENSION_VERSION` ayarı [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) komutu ile güncelleyebilirsiniz.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

İşlev uygulamanızın adı ile değiştirin. `<function_app>` Ayrıca `<my_resource_group>` işlev uygulamanız için kaynak grubunun adı ile değiştirin. Ayrıca, `<version>` 1.x çalışma zamanının geçerli bir `~2` sürümüyle veya sürüm 2.x ile değiştirin.

Bu komutu, önceki kod örneğinde **Taz'ı** seçerek [Azure Bulut Kabuğu'ndan](../cloud-shell/overview.md) çalıştırabilirsiniz. Oturum açmak için [az oturum](/cli/azure/reference-index#az-login) açtıktan sonra bu komutu yürütmek için [Azure CLI'yi yerel olarak](/cli/azure/install-azure-cli) da kullanabilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel geliştirme ortamınızda 2,0 çalışma süresini hedefleme](functions-run-local.md)

> [!div class="nextstepaction"]
> [Çalışma zamanı sürümleri için Yayın notlarına bakın](https://github.com/Azure/azure-webjobs-sdk-script/releases)
