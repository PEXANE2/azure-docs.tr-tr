---
title: Genel IP Azure portal oluşturma
description: Azure portal genel IP oluşturmayı öğrenin
services: virtual-network
author: asudbring
ms.service: virtual-network
ms.subservice: ip-services
ms.topic: how-to
ms.date: 02/22/2021
ms.author: allensu
ms.openlocfilehash: 5c5650d896442f10846e16903a1231010d032a44
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101675202"
---
# <a name="create-a-public-ip-address-using-the-azure-portal"></a>Azure portal kullanarak genel IP adresi oluşturma

Bu makalede, Azure portal kullanarak genel IP adresi kaynağı oluşturma gösterilmektedir. 

Bu genel IP 'nin ilişkilendirilebilecek kaynaklar hakkında daha fazla bilgi ve temel ve standart SKU 'Lar arasındaki fark için bkz. [genel IP adresleri](./public-ip-addresses.md). 

Bu makale, IPv4 adreslerine odaklanır. IPv6 adresleri hakkında daha fazla bilgi için bkz. [Azure VNET Için IPv6](./ipv6-overview.md).

# <a name="standard-sku"></a>[**Standart SKU**](#tab/option-create-public-ip-standard-zones)

**Mystandardzrpublicıp** adlı standart bölge-YEDEKLI genel IP adresi oluşturmak için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Arama kutusuna **genel IP adresi** girin. Arama sonuçlarında **genel IP adresi** ' ni seçin.
4. **Genel IP adresi** sayfasında **Oluştur**' u seçin.
5. **Genel IP adresi oluştur** sayfasında girin veya aşağıdaki bilgileri seçin: 

    | Ayar                 | Değer                       |
    | ---                     | ---                         |
    | IP sürümü              | IPv4 seçin                 |    
    | SKU                     | **Standart** seçin         |
    | Tier                   | **Bölgesel** seçme         |
    | Name                    | **Mystandardzrpublicıp** girin          |
    | IP adresi ataması   | Bu seçimin "static" olarak kilitli olduğunu aklınızda                                        |
    | Yönlendirme tercihi      | Varsayılan **Microsoft ağı**' nı bırakın. </br> Yönlendirme tercihi hakkında daha fazla bilgi için bkz. [yönlendirme tercihi (Önizleme) nedir?](./routing-preference-overview.md). |
    | Boşta kalma zaman aşımı (dakika)  | Varsayılan olarak **4**' i bırakın.        |
    | DNS ad etiketi          | Değeri boş bırakın.    |
    | Abonelik            | Aboneliğinizi seçin.   |
    | Kaynak grubu          | **Yeni oluştur**' u seçin, **myresourcegroup** girin. </br> **Tamam**’ı seçin. |
    | Konum                | **Doğu ABD 2** seçin      |
    | Kullanılabilirlik Alanı       | **Bölge yedekli**, bölge yok veya belirli bir bölgeyi Seç (aşağıdaki nota bakın) |

:::image type="content" source="./media/create-public-ip-portal/create-standard-ip.png" alt-text="Azure portal standart IP adresi oluşturma" border="false":::

> [!NOTE]
> Bu seçimler [kullanılabilirlik alanları](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#availability-zones)bölgelerde geçerlidir. </br>
Bu bölgelerde belirli bir bölgeyi seçebilirsiniz, ancak bu, bir hataya dayanıklı olmaz. </br> Kullanılabilirlik bölgeleri hakkında daha fazla bilgi için bkz. [Kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).

\* = Katmanı, şu anda önizleme aşamasında olan [çapraz bölge yük dengeleyici](../load-balancer/cross-region-overview.md) işleviyle ilgilidir.

# <a name="basic-sku"></a>[**Temel SKU**](#tab/option-create-public-ip-basic)

Bu bölümde, **Mybasicpublicıp** adlı temel BIR genel IP adresi oluşturun. 

> [!NOTE]
> Temel genel IP 'Ler kullanılabilirlik bölgelerini desteklemez.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Arama kutusuna **genel IP adresi** girin. Arama sonuçlarında **genel IP adresi** ' ni seçin.
4. **Genel IP adresi** sayfasında **Oluştur**' u seçin.
5. **Genel IP adresi oluştur** sayfasında girin veya aşağıdaki bilgileri seçin: 

    | Ayar                 | Değer                       |
    | ---                     | ---                         |
    | IP sürümü              | IPv4 seçin                 |    
    | SKU                     | **Temel** seçin         |
    | Name                    | *Mybasicpublicıp* girin          |
    | Yönlendirme tercihi      | Varsayılan **Microsoft ağı**' nı bırakın. </br> Yönlendirme tercihi hakkında daha fazla bilgi için bkz. [yönlendirme tercihi (Önizleme) nedir?](./routing-preference-overview.md). |
    | IP adresi ataması   | **Statik** öğesini seçin (aşağıdaki nota bakın)                                     |
    | Boşta kalma zaman aşımı (dakika)  | Varsayılan olarak **4**' i bırakın.       |
    | DNS ad etiketi          | Değeri boş bırakın    |
    | Abonelik            | Aboneliğinizi seçin.   |
    | Kaynak grubu          | **Yeni oluştur**' u seçin, **myresourcegroup** girin. </br> **Tamam**’ı seçin. |
    | Konum                | **Doğu ABD 2** seçin      |

:::image type="content" source="./media/create-public-ip-portal/create-standard-ip.png" alt-text="Azure portal standart IP adresi oluşturma" border="false":::

IP adresinin zaman içinde değiştirilmesi kabul edilebilir ise, **dinamik** IP ataması seçilebilir.

---

## <a name="additional-information"></a>Ek bilgiler 

Yukarıda listelenen ayrı alanlarla ilgili daha fazla bilgi için bkz. [genel IP adreslerini yönetme](./virtual-network-public-ip-address.md#create-a-public-ip-address).

## <a name="next-steps"></a>Sonraki adımlar
- [Genel IP adresini bir sanal makineyle](./associate-public-ip-address-vm.md#azure-portal) ilişkilendir
- Azure 'da [genel IP adresleri](./public-ip-addresses.md#public-ip-addresses) hakkında daha fazla bilgi edinin.
- Tüm [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)hakkında daha fazla bilgi edinin.