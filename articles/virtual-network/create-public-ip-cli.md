---
title: Genel IP oluşturma-Azure CLı
description: Azure CLı kullanarak genel IP oluşturmayı öğrenin
services: virtual-network
documentationcenter: na
author: blehr
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2020
ms.author: blehr
ms.openlocfilehash: 4d0934dd30f9738e2a67d4aff23c96a48aaaa61b
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89303605"
---
# <a name="quickstart-create-a-public-ip-address-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak genel IP adresi oluşturma

Bu makalede, Azure CLı kullanarak genel IP adresi kaynağı oluşturma konusu gösterilmektedir. Bu kaynakların hangi kaynaklarla ilişkilendiribileceği hakkında daha fazla bilgi için, temel ve standart SKU arasındaki fark ve diğer ilgili bilgiler için bkz. [genel IP adresleri](https://docs.microsoft.com/azure/virtual-network/public-ip-addresses).  Bu örnekte yalnızca IPv4 adreslerine odaklanacağız; IPv6 adresleri hakkında daha fazla bilgi için bkz. [Azure VNET Için IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview).

## <a name="prerequisites"></a>Ön koşullar

- Azure CLı yerel olarak veya Azure Cloud Shell yüklendi

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2.0.28 veya üstünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

**Eastus2** konumunda, [az Group Create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) **myresourcegroup** adlı bir kaynak grubu oluşturun.

```azurecli-interactive
  az group create \
    --name myResourceGroup \
    --location eastus2
```
---
# <a name="standard-sku---using-zones"></a>[**Standart SKU-bölgeleri kullanma**](#tab/option-create-public-ip-standard-zones)

>[!NOTE]
>Aşağıdaki komut API sürüm 2020-08-01 veya üzeri için geçerlidir.  Kullanılmakta olan API sürümü hakkında daha fazla bilgi için lütfen [kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types)inceleyin.

**Myresourcegroup**Içinde **Mystandardzrpublicıp** adlı standart bölge-yedekli genel IP adresi oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) komutunu kullanın.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myStandardZRPublicIP \
    --sku Standard
    --zone 1,2,3
```
> [!IMPORTANT]
> API 'nin 2020-08-01 'den eski sürümleri için, bölgesel olarak yedekli bir IP adresi oluşturmak üzere bölge parametresi belirtmeden yukarıdaki komutu çalıştırın. 
>

**Myresourcegroup**Içinde **Mystandardzonalpublicıp** adlı bölge 2 standart bir genel IP adresi oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroupLB \
    --name myStandardZonalPublicIP \
    --sku Standard \
    --zone 2
```

Bölgeler için yukarıdaki seçeneklerin yalnızca [kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones)bölgelerinde geçerli seçimler olduğunu unutmayın.

# <a name="standard-sku---no-zones"></a>[**Standart SKU-bölge yok**](#tab/option-create-public-ip-standard)

>[!NOTE]
>Aşağıdaki komut API sürüm 2020-08-01 veya üzeri için geçerlidir.  Kullanılmakta olan API sürümü hakkında daha fazla bilgi için lütfen [kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/management/resource-providers-and-types)inceleyin.

**Myresourcegroup**Içinde **mystandardpublicpublic** adlı bir kaynak olarak standart bir genel IP adresi oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) komutunu kullanın.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myStandardPublicIP \
    --sku Standard
```
Bu seçim tüm bölgelerde geçerlidir ve [kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones)olmayan bölgelerde standart genel IP adresleri için varsayılan seçimdir.

# <a name="basic-sku"></a>[**Temel SKU**](#tab/option-create-public-ip-basic)

**Myresourcegroup**Içinde **mybasicpublicıp** adlı temel bir statik genel IP adresi oluşturmak için [az Network public-ip Create](https://docs.microsoft.com/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) komutunu kullanın.  Temel genel IP 'Lerde kullanılabilirlik alanları kavramı yoktur.

```azurecli-interactive
  az network public-ip create \
    --resource-group myResourceGroup \
    --name myBasicPublicIP \
    --sku Standard
    --allocation-method Static
```
IP adresinin zaman içinde değiştirilmesi kabul edilebilir ise, ayırma yöntemi ' Dynamic ' olarak değiştirilerek **dinamik** IP ataması seçilebilir.

---

## <a name="additional-information"></a>Ek bilgiler 

Yukarıda listelenen bağımsız değişkenlerle ilgili daha fazla ayrıntı için lütfen bkz. [genel IP adreslerini yönetme](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#create-a-public-ip-address).

## <a name="next-steps"></a>Sonraki adımlar
- Bir [genel IP adresini bir sanal makineyle](https://docs.microsoft.com/azure/virtual-network/associate-public-ip-address-vm#azure-portal)ilişkilendirin.
- Azure 'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin.
- Tüm [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)hakkında daha fazla bilgi edinin.