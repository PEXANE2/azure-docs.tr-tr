---
title: Genel IP adresi için yönlendirme tercihini Yapılandırma-Azure portal
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
ms.openlocfilehash: 7db8dcc87606e5c58ddac8f609d1538989af5ed7
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84689308"
---
# <a name="configure-routing-preference-for-a-public-ip-address-using-the-azure-portal"></a>Azure portal kullanarak genel IP adresi için yönlendirme tercihini yapılandırın

Bu makalede genel bir IP adresi için ISS ağı (**Internet** seçeneği) aracılığıyla yönlendirme tercihini yapılandırma gösterilmektedir. Genel IP adresini oluşturduktan sonra, internet 'e gelen ve giden trafik için aşağıdaki Azure kaynaklarıyla ilişkilendirebilirsiniz:

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

## <a name="register-the-feature-for-your-subscription"></a>Aboneliğiniz için özelliği kaydedin
Yönlendirme tercihi özelliği şu anda önizlemededir. Azure PowerShell kullanarak aboneliğiniz için özelliği aşağıdaki gibi kaydedin:
```azurepowershell
Register-AzProviderFeature -FeatureName AllowRoutingPreferenceFeature -ProviderNamespace Microsoft.Network
```

## <a name="create-a-public-ip-address-with-a-routing-preference"></a>Yönlendirme tercihi ile genel IP adresi oluşturma
1. [Azure Portal](https://preview.portal.azure.com/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Arama kutusuna *genel IP adresi*yazın.
3. Arama sonuçlarında **genel IP adresi**' ni seçin. Sonra, **genel IP adresi** sayfasında **Oluştur**' u seçin.
3. **Yönlendirme tercihi** seçeneklerinde **Internet**' i seçin.

      ![Genel IP adresi oluşturma](./media/routing-preference-portal/pip-new.png)

    > [!NOTE]
    > Genel IP adresleri IPv4 veya IPv6 adresiyle oluşturulur. Ancak, yönlendirme tercihi şu anda yalnızca ıPV4 'Ü destekler.

Yukarıdaki oluşturulmuş genel IP adresini bir [Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makinesiyle ilişkilendirebilirsiniz. Öğretici sayfasındaki CLı bölümünü kullanın: genel IP adresini VM 'niz ile ilişkilendirmek için bir [sanal makineyle genel IP adresi ilişkilendirin](associate-public-ip-address-vm.md#azure-cli) . Ayrıca, yük dengeleyici **ön uç** yapılandırmasına atayarak yukarıda oluşturulan genel IP adresini bir [Azure Load Balancer](../load-balancer/load-balancer-overview.md)ilişkilendirebilirsiniz. Genel IP adresi yükü dengelenmiş bir sanal IP adresi (VIP) olarak işlev görür.

## <a name="next-steps"></a>Sonraki adımlar
- [Yönlendirme tercihi ile genel IP](routing-preference-overview.md)hakkında daha fazla bilgi edinin.
- [BIR VM için yönlendirme tercihini yapılandırın](tutorial-routing-preference-virtual-machine-portal.md).
- [PowerShell kullanarak genel IP adresi için yönlendirme tercihini yapılandırın](routing-preference-powershell.md).
- Azure 'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin.
- Tüm [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)hakkında daha fazla bilgi edinin.
