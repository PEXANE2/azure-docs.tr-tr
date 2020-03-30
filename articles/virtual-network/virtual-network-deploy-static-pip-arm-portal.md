---
title: Statik genel IP adresine sahip bir VM oluşturma - Azure portalı | Microsoft Dokümanlar
description: Azure portalını kullanarak statik genel IP adresine sahip bir VM'yi nasıl oluşturabilirsiniz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76043549"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-the-azure-portal"></a>Azure portalını kullanarak statik genel IP adresine sahip sanal bir makine oluşturma

Statik genel IP adresine sahip sanal bir makine oluşturabilirsiniz. Genel BIR IP adresi, internetten sanal bir makineyle iletişim kurmanızı sağlar. Adresin hiçbir zaman değişmemesini sağlamak için dinamik bir adres yerine statik bir genel IP adresi atayın. [Statik genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method)hakkında daha fazla bilgi edinin. Varolan bir sanal makineye atanan genel bir IP adresini dinamikten statike değiştirmek veya özel IP adresleriyle çalışmak için [bkz.](virtual-network-network-interface-addresses.md) Ortak IP adreslerinin [nominal ücreti](https://azure.microsoft.com/pricing/details/ip-addresses)vardır ve abonelik başına kullanabileceğiniz genel IP adreslerinin sayısı için bir [sınır](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vardır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **Compute'u**seçin ve ardından **Windows Server 2016 VM'yi**veya seçtiğiniz başka bir işletim sistemini seçin.
3. Aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Tamam**’ı seçin:

    |Ayar|Değer|
    |---|---|
    |Adı|myVM|
    |Kullanıcı adı| Seçtiğiniz bir kullanıcı adını girin.|
    |Parola| Seçtiğiniz bir parolayı girin. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
    |Abonelik| Aboneliğinizi seçin.|
    |Kaynak grubu| **Mevcut olanı kullan**’ı seçin ve **myResourceGroup** seçeneğini belirleyin.|
    |Konum| **Doğu ABD'yi** seçin|

4. Sanal makine için bir boyut seçin ve **Seç** seçeneğini belirleyin.
5. **Ayarlar** **altında, Genel IP adresini**seçin.
6. *PublicIpAddress'imi*girin , **Statik'i**seçin ve aşağıdaki resimde gösterildiği gibi **Tamam'ı**seçin:

   ![Statik'i seçin](./media/virtual-network-deploy-static-pip-arm-portal/select-static.png)

   Genel IP adresi standart bir SKU olması gerekiyorsa, **SKU**altında **Standart'ı** seçin. [Genel IP adresi SK'ler](virtual-network-ip-addresses-overview-arm.md#sku)hakkında daha fazla bilgi edinin. Sanal makine ortak bir Azure Yük Dengeleyicisinin arka uç havuzuna eklenecekse, sanal makinenin genel IP adresinin SKU'su yük bakiyesi ortak IP adresinin SKU'suyla eşleşmelidir. Ayrıntılar için Azure [Yük Dengeleyicisi'ne](../load-balancer/concepts-limitations.md#skus)bakın.

6. Genel **gelen bağlantı noktalarını seç'in**altında bir bağlantı noktası seçin veya bağlantı noktası seçmeyin. Portal 3389, Internet'ten Windows Server sanal makineye uzaktan erişim sağlamak için seçilir. 3389 bağlantı noktasının internetten açılması, üretim iş yükleri için önerilmez.

   ![Bağlantı noktası seçin](./media/virtual-network-deploy-static-pip-arm-portal/select-port.png)

7. Kalan varsayılan ayarları kabul edin ve **Tamam'ı**seçin.
8. **Özet** sayfasında **Oluştur**'u seçin. Sanal makinenin dağıtılması birkaç dakika sürer.
9. Sanal makine dağıtıldıktan sonra, portalın üst kısmındaki arama kutusuna *PublicIpAddress'igirini* girin. **PublicIpAddress** arama sonuçlarında göründüğünde, onu seçin.
10. Atanan genel IP adresini ve adresin aşağıdaki resimde gösterildiği gibi **myVM** sanal makinesine atandığını görüntüleyebilirsiniz:

    ![Genel IP adresini görüntüleme](./media/virtual-network-deploy-static-pip-arm-portal/public-ip-overview.png)

    Azure, sanal makineyi oluşturduğunuz bölgede kullandığınız adreslerden genel bir IP adresi atadı. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz.

11. Atamanın **Statik**olduğunu doğrulamak için **Yapılandırma'yı** seçin.

    ![Genel IP adresini görüntüleme](./media/virtual-network-deploy-static-pip-arm-portal/public-ip-configuration.png)

> [!WARNING]
> Sanal makinenin işletim sistemindeki IP adresi ayarlarını değiştirmeyin. İşletim sistemi Azure genel IP adreslerinden habersizdir. İşletim sistemine özel IP adresi ayarları ekleyebilmenize rağmen, gerekli olmadıkça bunu yapmamanızı ve bir [işletim sistemine özel bir IP adresi eklemeyi](virtual-network-network-interface-addresses.md#private)okuyana kadar yapmamanızı öneririz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu ve içerdiği tüm kaynakları silin:

1. Portalın üst kısmındaki **Ara** kutusuna *myResourceGroup* değerini girin. Arama sonuçlarında **myResourceGroup** seçeneğini gördüğünüzde bunu seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** için *myResourceGroup* girin ve **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Azure'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin
- Tüm genel [IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address) hakkında daha fazla bilgi edinin
- Azure sanal makinesine statik özel [IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) atama ve [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) hakkında daha fazla bilgi edinin
- [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineleri oluşturma hakkında daha fazla bilgi edinin