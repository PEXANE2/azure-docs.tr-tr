---
title: Azure HPC önbelleği için Azure CLı önkoşulları
description: Azure CLı 'yı kullanarak bir Azure HPC önbelleği oluşturmak veya değiştirmek için kurulum adımları
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 07/08/2020
ms.author: v-erkel
ms.openlocfilehash: 7d0844e699c012d987c23a75e2b0874005cf535a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100333"
---
# <a name="set-up-azure-cli-for-azure-hpc-cache"></a>Azure HPC önbelleği için Azure CLı 'yı ayarlama

Azure HPC önbelleği oluşturmak veya yönetmek için Azure CLı kullanmadan önce ortamınızı hazırlamak üzere bu adımları izleyin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="install-azure-cli"></a>Azure CLI'yı yükleme

Azure HPC Cache, Azure CLı 'nın 2,7 veya sonraki bir sürümünü gerektirir. `az --version`Yüklenen sürümü ve bağımlı kitaplıkları bulmak için ' i çalıştırın. Yüklemek veya yükseltmek için bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).

```azurecli-interactive
az --version
```

### <a name="install-the-azure-hpc-cache-extension"></a>Azure HPC önbellek uzantısını yükler

Azure HPC önbellek işlevleri ana kod tabanının bir parçası olmadığından uzantı başvurusunu da yüklemelisiniz. Aşağıdaki yönergeleri izleyin.

1. Oturum aç

   CLı 'nın yerel olarak yüklü bir sürümünü kullanıyorsanız [az Login](/cli/azure/reference-index#az-login) komutunu kullanarak oturum açın.

    ```azurecli
    az login
    ```

    Kimlik doğrulama işlemini gerçekleştirmek için terminalinizde görünen adımları izleyin.

   > [!TIP]
   > Birden çok aboneliğiniz varsa, bir tane seçmeniz gerekir. Azure portalında bir Cloud Shell oturumu başlattığınızda seçin ya da aboneliğinizi komut satırından ayarlamak için [Azure CLI ile çalışmaya başlama](/cli/azure/get-started-with-azure-cli#sign-in) bölümündeki yönergeleri izleyin.

2. Azure CLı uzantısını yükler

   Azure HPC önbellek işlevleri bir Azure CLı uzantısı olarak sağlanır; henüz çekirdek CLı paketinin bir parçası değildir. Uzantı başvurusunu kullanabilmeniz için önce yüklemeniz gerekir.

   Azure CLı uzantıları, deneysel ve yayın öncesi komutlara erişmenizi sağlar. Güncelleştirme ve kaldırma dahil olmak üzere uzantılar hakkında daha fazla bilgi edinmek için bkz. [Azure CLI ile uzantıları kullanma](/cli/azure/azure-cli-extensions-overview).

   Aşağıdaki komutu çalıştırarak Azure HPC önbelleğinin uzantısını yüklersiniz:

    ```azurecli-interactive
    az extension add --name hpc-cache
   ```

## <a name="set-default-resource-group-optional"></a>Varsayılan kaynak grubunu ayarla (isteğe bağlı)

HPC-Cache komutlarının çoğu, önbelleğin kaynak grubunu geçirmenize gerek duyar. Varsayılan kaynak grubunu [az configure](/cli/azure/reference-index#az-configure)kullanarak ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı uzantısını yükledikten ve oturum açarak Azure HPC önbellek sistemleri oluşturup yönetmek için Azure CLı kullanabilirsiniz.

* [Azure HPC önbelleği oluşturma](hpc-cache-create.md)
* [Azure CLı HPC-önbellek belgeleri](/cli/azure/ext/hpc-cache/hpc-cache)
