---
title: Genel IP Azure portal oluşturma
description: Azure portal genel IP oluşturmayı öğrenin
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
ms.openlocfilehash: 7d0c83f1ae18d36557a7a5b0222aee2905e05cb7
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550244"
---
# <a name="quickstart-create-a-public-ip-address-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak genel IP adresi oluşturma

Bu makalede, Azure portal kullanarak genel IP adresi kaynağı oluşturma gösterilmektedir. Bu kaynakların hangi kaynaklarla ilişkilendiribileceği hakkında daha fazla bilgi için, temel ve standart SKU arasındaki fark ve diğer ilgili bilgiler için bkz. [genel IP adresleri](./public-ip-addresses.md).  Bu örnekte yalnızca IPv4 adreslerine odaklanacağız; IPv6 adresleri hakkında daha fazla bilgi için bkz. [Azure VNET Için IPv6](./ipv6-overview.md).

# <a name="standard-sku"></a>[**Standart SKU**](#tab/option-create-public-ip-standard-zones)

**Mystandardzrpublicıp** adlı standart bölge-YEDEKLI genel IP adresi oluşturmak için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Arama kutusuna *genel IP adresi* yazın.
4. Arama sonuçlarında **genel IP adresi**' ni seçin. Sonra, **genel IP adresi** sayfasında **Oluştur**' u seçin.
5. **Genel IP adresi oluştur** sayfasında, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                       |
    | ---                     | ---                         |
    | IP sürümü              | IPv4 seçin                 |    
    | SKU                     | **Standart** seçin         |
    | Katman (gösteriliyorsa *)                  | **Bölgesel** seçme         |
    | Name                    | *Mystandardzrpublicıp* girin          |
    | IP adresi ataması   | Bu, "static" olarak kilitlenecek                                        |
    | Boşta kalma zaman aşımı (dakika)  | Değeri 4 ' te bırakın        |
    | DNS ad etiketi          | Değeri boş bırakın    |
    | Abonelik            | Aboneliğinizi seçin.   |
    | Kaynak grubu          | **Yeni oluştur** ' u seçin, myresourcegroup yazın ve **Tamam** ' ı seçin. |
    | Konum                | **Doğu ABD 2** seçin      |
    | Kullanılabilirlik Alanı       | **Bölge yedekli**, bölge yok veya belirli bir bölgeyi Seç (aşağıdaki nota bakın) |

Bunların [kullanılabilirlik alanları](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#availability-zones)bölgelerde yalnızca geçerli seçimler olduğunu unutmayın.  (Bu bölgelerde belirli bir bölgeyi de seçebilirsiniz, ancak bu bölge, bir hataya dayanıklı olmayacaktır.)  Kullanılabilirlik bölgeleri hakkında daha fazla bilgi için bkz. [Kullanılabilirlik alanlarına genel bakış](https://docs.microsoft.com/azure/availability-zones/az-overview).

\* = Katman, şu anda önizleme aşamasında olan [çapraz bölge Load Balancer](../load-balancer/cross-region-overview.md) işlevleriyle ilgilidir.

# <a name="basic-sku"></a>[**Temel SKU**](#tab/option-create-public-ip-basic)

**Mybasicpublicıp** adlı temel bir STATIK genel IP adresi oluşturmak için aşağıdaki adımları kullanın.  Temel genel IP 'Lerde kullanılabilirlik alanları kavramı yoktur.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Arama kutusuna *genel IP adresi* yazın.
4. Arama sonuçlarında **genel IP adresi**' ni seçin. Sonra, **genel IP adresi** sayfasında **Oluştur**' u seçin.
5. **Genel IP adresi oluştur** sayfasında, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                       |
    | ---                     | ---                         |
    | IP sürümü              | IPv4 seçin                 |    
    | SKU                     | **Standart** seçin         |
    | Name                    | *Mybasicpublicıp* girin          |
    | IP adresi ataması   | **Statik** öğesini seçin (aşağıdaki nota bakın)                                     |
    | Boşta kalma zaman aşımı (dakika)  | Değeri 4 ' te bırakın        |
    | DNS ad etiketi          | Değeri boş bırakın    |
    | Abonelik            | Aboneliğinizi seçin.   |
    | Kaynak grubu          | **Yeni oluştur** ' u seçin, myresourcegroup yazın ve **Tamam** ' ı seçin. |
    | Konum                | **Doğu ABD 2** seçin      |

IP adresinin zaman içinde değiştirilmesi kabul edilebilir ise, **dinamik** IP ataması seçilebilir.

---

## <a name="additional-information"></a>Ek bilgiler 

Yukarıda listelenen ayrı alanlarla ilgili daha fazla ayrıntı için lütfen bkz. [genel IP adreslerini yönetme](./virtual-network-public-ip-address.md#create-a-public-ip-address).

## <a name="next-steps"></a>Sonraki adımlar
- [Genel IP adresini bir sanal makineyle](./associate-public-ip-address-vm.md#azure-portal) ilişkilendir
- Azure 'da [genel IP adresleri](./public-ip-addresses.md#public-ip-addresses) hakkında daha fazla bilgi edinin.
- Tüm [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)hakkında daha fazla bilgi edinin.
