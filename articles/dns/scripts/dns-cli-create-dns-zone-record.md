---
title: Etki alanı adı için bir DNS bölgesi ve kaydı oluşturma-Azure CLı-Azure DNS
description: Bu Azure CLI betik örneği, bir etki alanı için DNS bölgesi ve kaydı oluşturmayı gösterir
services: dns
author: asudbring
ms.service: dns
ms.topic: sample
ms.date: 09/20/2019
ms.author: allensu
ms.openlocfilehash: 8f5dde0a35f31d2c33ab2857659efc88339c2ead
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74210392"
---
# <a name="azure-cli-script-example-create-a-dns-zone-and-record"></a>Azure CLI betik örneği: DNS bölgesi ve kaydı oluşturma

Bu Azure CLI betik örneği, bir etki alanı için DNS bölgesi ve kaydı oluşturur. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/dns/create-dns-zone-and-record/create-dns-zone-and-record.sh "Create DNS zone and record")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, DS bölgesini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network dns zone create](/cli/azure/network/dns/zone#az-network-dns-zone-create) | Bir Azure DNS bölgesi oluşturur. |
| [az network dns record-set a add-record](/cli/azure/network/dns/record-set) | Bir DNS bölgesine *A* kaydı ekler. |
| [az network dns record-set list](/cli/azure/network/dns/record-set) | Bir DNS bölgesindeki tüm *A* kaydı kümelerini listeler. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

