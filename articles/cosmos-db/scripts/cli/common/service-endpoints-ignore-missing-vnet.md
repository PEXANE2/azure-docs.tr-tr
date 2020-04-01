---
title: Varolan bir Azure Cosmos hesabını sanal ağ hizmeti bitiş noktalarıyla bağlayın
description: Varolan bir Azure Cosmos hesabını sanal ağ hizmeti bitiş noktalarıyla bağlayın
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: 46e93e864034c451e1da1848a318ab176a292b6e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71275566"
---
# <a name="connect-an-existing-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Azure CLI'yi kullanarak varolan bir Azure Cosmos hesabını sanal ağ hizmeti bitiş noktalarına bağlayın

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu konu Azure CLI sürümünü 2.0.73 veya daha yeni çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu örnek, varolan bir Azure Cosmos hesabının, `ignore-missing-vnet-service-endpoint` alt ağın henüz hizmet bitiş noktaları için yapılandırılmamış olduğu mevcut yeni bir sanal ağa nasıl bağlanacağını göstermek için tasarlanmıştır. Bu, sanal ağın alt ağına yapılandırma tamamlanmadan önce Cosmos hesabının yapılandırmasının hatasız olarak tamamlanmasını sağlar. Alt ağ yapılandırması tamamlandıktan sonra, Cosmos hesabına yapılandırılan alt ağ üzerinden erişilebilir.

> [!NOTE]
> Bu örnek, bir SQL (Core) API hesabı nın kullanılmasını gösterir. Bu örneği diğer API'ler `enable-virtual-network` için `virtual-network-rules` kullanmak için, aşağıdaki komut dosyasındaki parametreleri ve parametreleri API'nize özel komut dosyanıza uygulayın.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints-ignore-missing-vnet.sh "Create an Azure Cosmos account with service endpoints.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Azure sanal ağı oluşturur. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Azure sanal ağı için bir alt ağ oluşturur. |
| [az ağ vnet subnet gösterisi](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-show) | Azure sanal ağı için bir alt ağ döndürür. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB hesabı oluşturur. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) | Azure sanal ağı için bir alt ağ güncelleştirir. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB CLI hakkında daha fazla bilgi için [Azure Cosmos DB CLI belgelerine](/cli/azure/cosmosdb)bakın.

Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
