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
ms.openlocfilehash: 49a89ee90d28c9c7a3f59424b773ee0f221381e4
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89303533"
---
# <a name="quickstart-create-a-public-ip-address-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak genel IP adresi oluşturma

Bu makalede, Azure portal kullanarak genel IP adresi kaynağı oluşturma gösterilmektedir. Bu kaynakların hangi kaynaklarla ilişkilendiribileceği hakkında daha fazla bilgi için, temel ve standart SKU arasındaki fark ve diğer ilgili bilgiler için bkz. [genel IP adresleri](https://docs.microsoft.com/azure/virtual-network/public-ip-addresses).  Bu örnekte yalnızca IPv4 adreslerine odaklanacağız; IPv6 adresleri hakkında daha fazla bilgi için bkz. [Azure VNET Için IPv6](https://docs.microsoft.com/azure/virtual-network/ipv6-overview).

# <a name="standard-sku---using-zones"></a>[**Standart SKU-bölgeleri kullanma**](#tab/option-create-public-ip-standard-zones)

**Mystandardzrpublicıp**adlı standart bölge-YEDEKLI genel IP adresi oluşturmak için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Arama kutusuna *genel IP adresi*yazın.
4. Arama sonuçlarında **genel IP adresi**' ni seçin. Sonra, **genel IP adresi** sayfasında **Oluştur**' u seçin.
5. **Genel IP adresi oluştur** sayfasında, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                       |
    | ---                     | ---                         |
    | IP sürümü              | IPv4 seçin                 |    
    | SKU                     | **Standart** seçin         |
    | Name                    | *Mystandardzrpublicıp* girin          |
    | IP adresi ataması   | Bu, "static" olarak kilitlenecek                                        |
    | Boşta kalma zaman aşımı (dakika)  | Değeri 4 ' te bırakın        |
    | DNS ad etiketi          | Değeri boş bırakın    |
    | Abonelik            | Aboneliğinizi seçin.   |
    | Kaynak grubu          | **Yeni oluştur** ' u seçin, myresourcegroup yazın ve **Tamam** ' ı seçin. |
    | Konum                | **Doğu ABD 2** seçin      |
    | Kullanılabilirlik Alanı       | **Bölge yedekli** seçeneğini belirleyin veya belirli bir bölgeyi seçin (aşağıdaki nota bakın) |

Bunların [kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones)bölgelerde yalnızca geçerli seçimler olduğunu unutmayın.  (Bu bölgelerde belirli bir bölgeyi de seçebilirsiniz, ancak bu bölge, bir hataya dayanıklı olmayacaktır.)

# <a name="standard-sku---no-zones"></a>[**Standart SKU-bölge yok**](#tab/option-create-public-ip-standard)

Standart genel IP adresini **Mystandardpublicıp**adlı bir kaynak olmayan kaynak olarak oluşturmak için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Arama kutusuna *genel IP adresi*yazın.
4. Arama sonuçlarında **genel IP adresi**' ni seçin. Sonra, **genel IP adresi** sayfasında **Oluştur**' u seçin.
5. **Genel IP adresi oluştur** sayfasında, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                       |
    | ---                     | ---                         |
    | IP sürümü              | IPv4 seçin                 |    
    | SKU                     | **Standart** seçin         |
    | Name                    | *Mystandardpublicıp* girin          |
    | IP adresi ataması   | Bu, "static" olarak kilitlenecek                                        |
    | Boşta kalma zaman aşımı (dakika)  | Değeri 4 ' te bırakın        |
    | DNS ad etiketi          | Değeri boş bırakın    |
    | Abonelik            | Aboneliğinizi seçin.   |
    | Kaynak grubu          | **Yeni oluştur** ' u seçin, myresourcegroup yazın ve **Tamam** ' ı seçin. |
    | Konum                | **Doğu ABD 2** seçin      |
    | Kullanılabilirlik Alanı       | **Bölge yok** ' u seçin (aşağıdaki nota bakın) |

Bu seçim tüm bölgelerde geçerlidir ve [kullanılabilirlik alanları](https://docs.microsoft.com/azure/availability-zones/az-overview?toc=/azure/virtual-network/toc.json#availability-zones)olmayan bölgelerde standart genel IP adresleri için varsayılan seçimdir.

# <a name="basic-sku"></a>[**Temel SKU**](#tab/option-create-public-ip-basic)

**Mybasicpublicıp**adlı temel bir STATIK genel IP adresi oluşturmak için aşağıdaki adımları kullanın.  Temel genel IP 'Lerde kullanılabilirlik alanları kavramı yoktur.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. **Kaynak oluştur**’u seçin. 
3. Arama kutusuna *genel IP adresi*yazın.
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

Yukarıda listelenen ayrı alanlarla ilgili daha fazla ayrıntı için lütfen bkz. [genel IP adreslerini yönetme](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address#create-a-public-ip-address).

## <a name="next-steps"></a>Sonraki adımlar
- [Genel IP adresini bir sanal makineyle](https://docs.microsoft.com/azure/virtual-network/associate-public-ip-address-vm#azure-portal) ilişkilendir
- Azure 'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin.
- Tüm [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address)hakkında daha fazla bilgi edinin.