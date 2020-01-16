---
title: Statik bir genel IP adresi olan VM oluşturma-Azure portal | Microsoft Docs
description: Azure portal kullanarak statik bir genel IP adresi ile VM oluşturmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: kumud
ms.openlocfilehash: 66050c16f40e0a06117327ef53e3aae87d03c5db
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76043549"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-the-azure-portal"></a>Azure portal kullanarak statik genel IP adresi içeren bir sanal makine oluşturun

Statik bir genel IP adresine sahip bir sanal makine oluşturabilirsiniz. Genel bir IP adresi, internet 'ten bir sanal makineyle iletişim kurmanızı sağlar. Adresin hiçbir şekilde değişmeyeceğinden emin olmak için dinamik bir adres yerine statik bir genel IP adresi atayın. [Statik genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method)hakkında daha fazla bilgi edinin. Var olan bir sanal makineye atanan bir genel IP adresini dinamik veya özel IP adresleriyle çalışacak şekilde değiştirmek için bkz. [IP adreslerini ekleme, değiştirme veya kaldırma](virtual-network-network-interface-addresses.md). Genel IP adreslerinin nominal bir [ücreti](https://azure.microsoft.com/pricing/details/ip-addresses)vardır ve abonelik başına KULLANABILECEĞINIZ genel IP adresi sayısı için bir [sınır](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vardır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturun

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **İşlem**' ı seçin ve ardından **WINDOWS Server 2016 VM**veya seçtiğiniz başka bir işletim sistemini seçin.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Ad|myVM|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| **Mevcut olanı kullan**’ı seçin ve **myResourceGroup** seçeneğini belirleyin.|
    |Konum| **Doğu ABD**’yi seçin|

4. Sanal makine için bir boyut seçin ve **Seç** seçeneğini belirleyin.
5. **Ayarlar**altında **genel IP adresi**' ni seçin.
6. *Mypublicıpaddress*yazın, **statik**' i seçin ve ardından aşağıdaki resimde gösterildiği gibi **Tamam**' ı seçin:

   ![Statik seçin](./media/virtual-network-deploy-static-pip-arm-portal/select-static.png)

   Genel IP adresinin standart bir SKU olması gerekiyorsa, **SKU**altında **Standart** ' ı seçin. [Genel IP adresi SKU 'ları](virtual-network-ip-addresses-overview-arm.md#sku)hakkında daha fazla bilgi edinin. Sanal makine, genel bir Azure Load Balancer arka uç havuzuna eklenecektir, sanal makinenin genel IP adresi SKU 'su, yük dengeleyicinin genel IP adresi SKU 'SU ile aynı olmalıdır. Ayrıntılar için bkz. [Azure Load Balancer](../load-balancer/concepts-limitations.md#skus).

6. Bir bağlantı noktası seçin veya **Genel gelen bağlantı noktalarını seçin**altında bağlantı noktası yok. Portal 3389, Windows Server sanal makinesine internet 'ten uzaktan erişimi etkinleştirmek için seçilidir. 3389 numaralı bağlantı noktasını Internet 'ten açmak, üretim iş yükleri için önerilmez.

   ![Bir bağlantı noktası seçin](./media/virtual-network-deploy-static-pip-arm-portal/select-port.png)

7. Kalan varsayılan ayarları kabul edin ve **Tamam**' ı seçin.
8. **Özet** sayfasında **Oluştur**'u seçin. Sanal makinenin dağıtılması birkaç dakika sürer.
9. Sanal makine dağıtıldıktan sonra portalın üst kısmındaki arama kutusuna *Mypublicıpaddress* yazın. Arama sonuçlarında **Mypublicıpaddress** göründüğünde seçin.
10. Aşağıdaki resimde gösterildiği gibi, atanan genel IP adresini ve adresin **Myvm** sanal makinesine atandığını görebilirsiniz:

    ![Genel IP adresini görüntüle](./media/virtual-network-deploy-static-pip-arm-portal/public-ip-overview.png)

    Azure, içinde sanal makineyi oluşturduğunuz bölgede kullanılan adreslerden ortak bir IP adresi atamıştır. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz.

11. Atamanın **statik**olduğunu onaylamak için **yapılandırma** ' yı seçin.

    ![Genel IP adresini görüntüle](./media/virtual-network-deploy-static-pip-arm-portal/public-ip-configuration.png)

> [!WARNING]
> Sanal makinenin işletim sistemi içindeki IP adresi ayarlarını değiştirmeyin. İşletim sistemi, Azure genel IP adreslerinden oluşan farkında değildir. İşletim sistemine özel IP adresi ayarları ekleyebilse de, gerekmedikçe, [bir işletim sistemine özel bır IP adresi ekleme](virtual-network-network-interface-addresses.md#private)' yi bulana kadar yapmamasını öneririz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **Ara** kutusuna *myResourceGroup* değerini girin. Arama sonuçlarında **myResourceGroup** seçeneğini gördüğünüzde bunu seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** için *myResourceGroup* girin ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure 'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin
- Tüm [genel IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address) hakkında daha fazla bilgi edinin
- [Özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) hakkında daha fazla bilgi edinin ve bir Azure sanal makinesine [STATIK bir özel IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) atama
- [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineleri oluşturma hakkında daha fazla bilgi edinin