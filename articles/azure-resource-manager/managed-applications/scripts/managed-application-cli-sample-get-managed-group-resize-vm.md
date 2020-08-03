---
title: Yönetilen kaynak grubunu al & VM 'Leri yeniden boyutlandırma-Azure CLı
description: Azure yönetilen uygulamasında yönetilen bir kaynak grubu alan Azure CLı örnek betiği sağlar. Betik, VM 'Leri yeniden boyutlandırır.
author: tfitzmac
ms.devlang: azurecli
ms.topic: sample
ms.date: 10/25/2017
ms.author: tomfitz
ms.custom: devx-track-azurecli
ms.openlocfilehash: 711b516d1ba1154e574b0d8bbd8d86a02d7df018
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87497826"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Azure CLI ile bir yönetilen kaynak grubundaki kaynakları alma ve VM’leri yeniden boyutlandırma

Bu betik bir yönetilen kaynak grubundan kaynakları alır ve bu kaynak grubundaki VM’leri yeniden boyutlandırır.


[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulamayı dağıtmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az managedapp list](/cli/azure/managedapp#az-managedapp-list) | Yönetilen uygulamaları listeler. Sonuçlara odaklanmak için sorgu değerleri sağlar. |
| [az resource list](/cli/azure/resource#az-resource-list) | Kaynakları listeler. Sonuca odaklanmak için bir kaynak grubu ve sorgu değerleri sağlar. |
| [az vm resize](/cli/azure/vm#az-vm-resize) | Sanal makinenin boyutunu güncelleştirir. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
