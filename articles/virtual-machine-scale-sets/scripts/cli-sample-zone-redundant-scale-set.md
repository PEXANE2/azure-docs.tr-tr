---
title: Azure CLI Örnekleri - Bölge yedekli ölçek kümesi
description: Bu komut dosyası, Birden çok Kullanılabilirlik Bölgesi arasında Ubuntu çalıştıran bir Azure sanal makine ölçeği kümesi oluşturur.
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.devlang: azurecli
ms.topic: sample
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d03eb9ca86472670caf89f8a9f9c4cf861b2eb62
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76278712"
---
# <a name="create-a-zone-redundant-virtual-machine-scale-set-with-azure-cli"></a>Azure CLI ile bölge gereksiz sanal makine ölçeği kümesi oluşturun
Bu betik, birden çok Kullanılabilirlik Alanı’nda Ubuntu çalıştıran bir sanal makine ölçek kümesi oluşturur. Betiği çalıştırdıktan sonra sanal makineye RDP üzerinden erişebilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh "Create zone-redundant scale set")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Kaynak grubunu, ölçek kümesini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, bir kaynak grubu, sanal makine ölçek kümesi ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/ad/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az vmss create](/cli/azure/vmss) | Sanal makine ölçek kümesi oluşturur ve bunu sanal ağa, alt ağa ve ağ güvenlik grubuna bağlar. Birden çok sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir.  |
| [az group delete](/cli/azure/ad/group) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine ölçek kümesi Azure CLI betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../cli-samples.md) bulunabilir.
