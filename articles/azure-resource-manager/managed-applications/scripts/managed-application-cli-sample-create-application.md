---
title: Azure CLı betik örneği-yönetilen bir uygulama dağıtma
description: Aboneliğe Azure yönetilen uygulama tanımını dağıtan Azure CLı örnek betiği sağlar.
author: tfitzmac
ms.devlang: azurecli
ms.topic: sample
ms.date: 10/25/2017
ms.author: tomfitz
ms.custom: devx-track-azurecli
ms.openlocfilehash: a9912f5134c3596740ef53f23531d57fe9467682
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87497877"
---
# <a name="deploy-a-managed-application-for-service-catalog-with-azure-cli"></a>Azure CLI ile hizmet kataloğu için bir yönetilen uygulamayı dağıtma

Bu betik, hizmet kataloğundan yönetilen bir uygulama tanımını dağıtır. 


[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../../cli_scripts/managed-applications/create-application/create-application.sh "Create application")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulamayı dağıtmak için aşağıdaki komutu kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az managedapp create](/cli/azure/managedapp#az-managedapp-create) | Yönetilen uygulama oluşturur. Şablonun tanım kimliğini ve parametrelerini sağlar. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
