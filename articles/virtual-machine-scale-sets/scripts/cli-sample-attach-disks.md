---
title: Azure CLı örnekleri-veri disklerini Iliştirme ve kullanma
description: Bu betik bir Azure sanal makine ölçek kümesi oluşturur ve Azure CLı ile veri disklerini iliştirir ve hazırlar.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 03/27/2018
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 0f2a05f6d7268bea3c5ccdaccc9ac99863aaf5fe
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198193"
---
# <a name="attach-and-use-data-disks-with-a-virtual-machine-scale-set-with-the-azure-cli"></a>Azure CLI ile sanal makine ölçek kümesi kullanarak veri diskleri ekleme ve kullanma
Bu betik, sanal makine ölçek kümesi oluşturur ve veri diskleri ekleyip hazırlar.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik
[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine-scale-sets/use-data-disks/use-data-disks.sh "Create a virtual machine scale set with data disks")]

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
| [az vmss disk attach](/cli/azure/vmss/disk) | Bir veri diski oluşturur ve bu veri diskini sanal makine ölçek kümesine ekler. |
| [az vmss extension set](/cli/azure/vmss/extension) | Her sanal makine örneğinde veri disklerini hazırlayan bir betik çalıştırmak için Azure Özel Betik Uzantısı’nı yükler. |
| [az group delete](/cli/azure/ad/group) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure/overview).

Ek sanal makine ölçek kümesi Azure CLI betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../cli-samples.md) bulunabilir.
