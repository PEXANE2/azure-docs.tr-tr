---
title: Azure CLI Betik Örneği - ACS Windows Kubernetes Kümesi Oluşturma
description: Azure CLI Betik Örneği - ACS Windows Kubernetes Kümesi Oluşturma
author: iainfoulds
tags: acs, azure-container-service
keywords: Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure
ms.assetid: ''
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.date: 05/30/2017
ms.author: iainfou
ms.openlocfilehash: bc940f09a98eb4ee42290dcfd11d0800f6c3b9e4
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76270679"
---
# <a name="deprecated-create-an-azure-container-service-kubernetes-windows-cluster"></a>Kullanım DıŞı Azure Container Service Kubernetes Windows kümesi oluşturma

[!INCLUDE [ACS deprecation](../../../../includes/container-service-kubernetes-deprecation.md)]

Bu örnek, Windows tabanlı kapsayıcılar için Kubernetes çalıştıran bir Azure Container Service kümesi oluşturur.

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type kubernetes \
  --resource-group myResourceGroup \
  --name myK8SCluster \
  --generate-ssh-keys \
  --admin-username azureuser \
  --admin-password Password12 \
  --windows
```

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az acs create](https://docs.microsoft.com/cli/azure/acs#az-acs-create) | ACS kümesi oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Container Service CLI betik örnekleri, [Azure Container Service belgelerinde](../cli-samples.md) bulunabilir.
