---
title: Azure CLı kullanarak genel IP adresi için yönlendirme tercihini yapılandırma
titlesuffix: Azure Virtual Network
description: Internet trafiği yönlendirme tercihi ile genel IP oluşturmayı öğrenin
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2020
ms.author: mnayak
ms.custom: devx-track-azurecli
ms.openlocfilehash: 64284b198fc76c219ffe0dfbc57461b587b23130
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87504611"
---
# <a name="configure-routing-preference-for-a-public-ip-address-using-azure-cli"></a>Azure CLı kullanarak genel IP adresi için yönlendirme tercihini yapılandırma

Bu makalede, Azure CLı kullanarak genel bir IP adresi için ISS ağı (**Internet** seçeneği) aracılığıyla yönlendirme tercihini yapılandırma gösterilmektedir. Genel IP adresini oluşturduktan sonra, internet 'e gelen ve giden trafik için aşağıdaki Azure kaynaklarıyla ilişkilendirebilirsiniz:

* Sanal makine
* Sanal makine ölçek kümesi
* Azure Kubernetes Hizmeti (AKS)
* Internet 'e yönelik yük dengeleyici
* Application Gateway
* Azure Güvenlik Duvarı

Varsayılan olarak, genel IP adresi için yönlendirme tercihi tüm Azure hizmetleri için Microsoft Global Network 'e ayarlanır ve herhangi bir Azure hizmetiyle ilişkilendirilebilir.

> [!IMPORTANT]
> Yönlendirme tercihi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]
Bunun yerine Azure CLı 'yı yüklemek ve kullanmak isterseniz, bu hızlı başlangıç, Azure CLı sürüm 2.0.49 veya sonraki bir sürümünü kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için öğesini çalıştırın `az --version` . Bkz. Install veya Upgrade Info for [Azure CLI](/cli/azure/install-azure-cli) .

## <a name="register-the-feature-for-your-subscription"></a>Aboneliğiniz için özelliği kaydedin
Yönlendirme tercihi özelliği şu anda önizlemededir. Aboneliğiniz için özelliği aşağıdaki şekilde kaydedin:
```azurecli
az feature register --namespace Microsoft.Network --name AllowRoutingPreferenceFeature
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, **Doğu ABD** Azure bölgesinde bir kaynak grubu oluşturur:

```azurecli
  az group create --name myResourceGroup --location eastus
```
## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Aşağıda gösterildiği gibi, [az Network public-IP Create](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create)komutunu kullanarak "Internet" türünün yönlendirme tercihi ile genel bir IP adresi oluşturun.

Aşağıdaki komut, **Doğu ABD** Azure bölgesinde **Internet** yönlendirme tercihi Ile yeni bir genel IP oluşturur.

```azurecli
az network public-ip create \
--name MyRoutingPrefIP \
--resource-group MyResourceGroup \
--location eastus \
--ip-tags 'RoutingPreference=Internet' \
--sku STANDARD \
--allocation-method static \
--version IPv4
```

> [!NOTE]
>  Şu anda yönlendirme tercihi yalnızca ıPV4 genel IP adreslerini destekler.

Yukarıdaki oluşturulmuş genel IP adresini bir [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makinesiyle ilişkilendirebilirsiniz. Öğretici sayfasındaki CLı bölümünü kullanın: genel IP adresini VM 'niz ile ilişkilendirmek için bir [sanal makineyle genel IP adresi ilişkilendirin](associate-public-ip-address-vm.md#azure-cli) . Ayrıca, yük dengeleyici **ön uç** yapılandırmasına atayarak yukarıda oluşturulan genel IP adresini bir [Azure Load Balancer](../load-balancer/load-balancer-overview.md)ile ilişkilendirebilirsiniz. Genel IP adresi yükü dengelenmiş bir sanal IP adresi (VIP) olarak işlev görür.

## <a name="next-steps"></a>Sonraki adımlar

- [Genel IP adreslerinde yönlendirme tercihi](routing-preference-overview.md)hakkında daha fazla bilgi edinin. 
- [Azure CLI kullanarak BIR VM için yönlendirme tercihini yapılandırın](configure-routing-preference-virtual-machine-cli.md).

