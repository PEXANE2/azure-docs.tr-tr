---
title: Statik genel IP adresine sahip bir VM oluşturma - Azure CLI | Microsoft Dokümanlar
description: Azure komut satırı arabirimini (CLI) kullanarak statik genel IP adresine sahip bir VM'yi nasıl oluşturabilirsiniz öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: kumud
ms.openlocfilehash: 4b8f91d7d9fc414ed0ae6387c25f71b1601e0b6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76043398"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-the-azure-cli"></a>Azure CLI'yi kullanarak statik genel IP adresine sahip sanal bir makine oluşturun

Statik genel IP adresine sahip sanal bir makine oluşturabilirsiniz. Genel BIR IP adresi, internetten sanal bir makineyle iletişim kurmanızı sağlar. Adresin hiçbir zaman değişmemesini sağlamak için dinamik bir adres yerine statik bir genel IP adresi atayın. [Statik genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method)hakkında daha fazla bilgi edinin. Varolan bir sanal makineye atanan genel bir IP adresini dinamikten statike değiştirmek veya özel IP adresleriyle çalışmak için [bkz.](virtual-network-network-interface-addresses.md) Ortak IP adreslerinin [nominal ücreti](https://azure.microsoft.com/pricing/details/ip-addresses)vardır ve abonelik başına kullanabileceğiniz genel IP adreslerinin sayısı için bir [sınır](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vardır.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Aşağıdaki adımları yerel bilgisayarınızdan veya Azure Bulut Kabuğu'nu kullanarak tamamlayabilirsiniz. Yerel bilgisayarınızı kullanmak için Azure [CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)yüklü olduğundan emin olun. Azure Bulut Kabuğu'nu kullanmak için, aşağıdaki komut kutusunun sağ üst köşesinde **Bunu Dene'yi** seçin. Bulut Kabuğu sizi Azure'a imzalar.

1. Bulut Kabuğu'nu kullanıyorsanız, adım 2'ye geçin. Bir komut oturumu açın ve `az login`Azure'da 'ile oturum açın
2. [az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, Doğu ABD Azure bölgesinde bir kaynak grubu oluşturur:

   ```azurecli-interactive
   az group create --name myResourceGroup --location eastus
   ```

3. [az vm create](/cli/azure/vm#az-vm-create) komutuyla bir sanal makine oluşturun. Seçenek, `--public-ip-address-allocation=static` sanal makineye statik bir genel IP adresi atar. Aşağıdaki örnek, *myPublicIpAddress*adlı statik, temel SKU genel IP adresi ile bir Ubuntu sanal makine oluşturur:

   ```azurecli-interactive
   az vm create \
     --resource-group myResourceGroup \
     --name myVM \
     --image UbuntuLTS \
     --admin-username azureuser \
     --generate-ssh-keys \
     --public-ip-address myPublicIpAddress \
     --public-ip-address-allocation static
   ```

   Genel IP adresi standart bir SKU `--public-ip-sku Standard` olması gerekiyorsa, önceki komuta ekleyin. [Genel IP adresi SK'ler](virtual-network-ip-addresses-overview-arm.md#sku)hakkında daha fazla bilgi edinin. Sanal makine ortak bir Azure Yük Dengeleyicisinin arka uç havuzuna eklenecekse, sanal makinenin genel IP adresinin SKU'su yük bakiyesi ortak IP adresinin SKU'suyla eşleşmelidir. Ayrıntılar için Azure [Yük Dengeleyicisi'ne](../load-balancer/concepts-limitations.md#skus)bakın.

4. Atanan genel IP adresini görüntüleyin ve [az ağ public-ip gösterisi](/cli/azure/network/public-ip#az-network-public-ip-show)ile statik, temel SKU adresi olarak oluşturulduğunu onaylayın:

   ```azurecli-interactive
   az network public-ip show \
     --resource-group myResourceGroup \
     --name myPublicIpAddress \
     --query [ipAddress,publicIpAllocationMethod,sku] \
     --output table
   ```

   Azure, sanal makineyi oluşturduğunuz bölgede kullandığınız adreslerden genel bir IP adresi atadı. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz.

> [!WARNING]
> Sanal makinenin işletim sistemindeki IP adresi ayarlarını değiştirmeyin. İşletim sistemi Azure genel IP adreslerinden habersizdir. İşletim sistemine özel IP adresi ayarları ekleyebilmenize rağmen, gerekli olmadıkça bunu yapmamanızı ve bir [işletim sistemine özel bir IP adresi eklemeyi](virtual-network-network-interface-addresses.md#private)okuyana kadar yapmamanızı öneririz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin
- Tüm genel [IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address) hakkında daha fazla bilgi edinin
- Azure sanal makinesine statik özel [IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) atama ve [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) hakkında daha fazla bilgi edinin
- [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineleri oluşturma hakkında daha fazla bilgi edinin
