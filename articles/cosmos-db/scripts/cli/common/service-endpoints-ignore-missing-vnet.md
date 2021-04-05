---
title: Mevcut bir Azure Cosmos hesabını sanal ağ hizmeti uç noktaları ile bağlama
description: Mevcut bir Azure Cosmos hesabını sanal ağ hizmeti uç noktaları ile bağlama
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: f66d219eff1919e62088e5c4f7aa72aab97ea4f9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94566240"
---
# <a name="connect-an-existing-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Azure CLı kullanarak mevcut bir Azure Cosmos hesabını sanal ağ hizmet uç noktaları ile bağlama
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Bu makale, Azure CLı 'nin sürüm 2.9.1 veya üstünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="sample-script"></a>Örnek betik

Bu örnek, mevcut bir Azure Cosmos hesabını, alt ağın, parametresi kullanılarak hizmet uç noktaları için henüz yapılandırılmadığı mevcut yeni bir sanal ağa nasıl bağlayabileceğini göstermek için tasarlanmıştır `ignore-missing-vnet-service-endpoint` . Bu, Cosmos hesabının yapılandırmasının, sanal ağın alt ağının yapılandırması tamamlanmadan önce hatasız olarak tamamlanmasını sağlar. Alt ağ yapılandırması tamamlandıktan sonra, Cosmos hesabına, yapılandırılmış alt ağ üzerinden erişilebilecektir.

> [!NOTE]
> Bu örnekte bir SQL (Core) API hesabı kullanılması gösterilmektedir. Bu örneği diğer API 'Ler için kullanmak üzere `enable-virtual-network` `virtual-network-rules` API 'ye özel betiğe aşağıdaki komut dosyasına ve parametrelerini uygulayın.

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
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Bir Azure sanal ağı oluşturur. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Azure sanal ağı için bir alt ağ oluşturur. |
| [az Network VNET subnet Show](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-show) | Bir Azure sanal ağı için bir alt ağ döndürür. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB hesabı oluşturur. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) | Bir Azure sanal ağı için bir alt ağ güncelleştirir. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

CLı Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
