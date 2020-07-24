---
title: Azure CLı betik örneği-eş iki sanal ağ | Microsoft Docs
description: Azure CLı betik örneği-eşler arası iki sanal ağ
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: c301c66c3b668ae48314c4d1eb06657d8573209c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87030422"
---
# <a name="use-an-azure-cli-sample-script-to-connect-two-virtual-networks"></a>İki sanal ağı bağlamak için bir Azure CLı örnek betiği kullanın

Bu betik, Azure ağı aracılığıyla aynı bölgedeki iki sanal ağı oluşturur ve bağlar. Betiği çalıştırdıktan sonra iki sanal ağ arasında eşleme oluşturmuş olursunuz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) | Bir Azure sanal ağı ve alt ağ oluşturur. |
| [az network vnet peering create](https://docs.microsoft.com/cli/azure/network/vnet/peering) | İki sanal ağ arasında eşleme oluşturur.  |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek ağ CLI betiği örnekleri, [Azure Ağına Genel Bakış belgeleri](../cli-samples.md) içinde bulunabilir.
