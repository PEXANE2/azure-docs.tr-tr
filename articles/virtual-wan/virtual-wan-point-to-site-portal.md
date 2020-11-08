---
title: 'Öğretici: Azure sanal WAN kullanarak Azure ile noktadan siteye bağlantı oluşturma'
description: Bu öğreticide Azure Sanal WAN kullanarak Azure'a Noktadan Siteye bağlantı oluşturmayı öğreneceksiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 11/06/2020
ms.author: cherylmc
ms.openlocfilehash: 0671e41703ecc4b8580c8439eec5bac954b4dbe0
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2020
ms.locfileid: "94368840"
---
# <a name="tutorial-create-a-user-vpn-connection-using-azure-virtual-wan"></a>Öğretici: Azure sanal WAN kullanarak bir kullanıcı VPN bağlantısı oluşturma

Bu öğreticide Sanal WAN kullanarak Azure'daki kaynaklarınıza bir IPsec/IKE (IKEv2) veya OpenVPN VPN bağlantısı üzerinden bağlanmayı öğreneceksiniz. Bu tür bir bağlantı, istemci bilgisayarda VPN istemcisinin yapılandırılmasını gerektirir. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Sanal WAN oluşturma
> * P2S yapılandırması oluşturma
> * Sanal hub oluşturma
> * DNS sunucularını belirtme
> * VPN istemci profili yapılandırma paketini oluştur ve Kur
> * Sanal WAN'ınızı görüntüleme

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Before beginning](../../includes/virtual-wan-before-include.md)]

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Sanal WAN oluşturma

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-create-vwan-include.md)]

## <a name="create-a-p2s-configuration"></a><a name="p2sconfig"></a>P2S yapılandırması oluşturma

Noktadan siteye (P2S) yapılandırması, uzak istemcileri bağlamaya yönelik parametreleri tanımlar.

[!INCLUDE [Create P2S configuration](../../includes/virtual-wan-p2s-configuration-include.md)]

## <a name="create-virtual-hub-and-gateway"></a><a name="hub"></a>Sanal hub ve ağ geçidi oluşturma

[!INCLUDE [Create hub](../../includes/virtual-wan-p2s-hub-include.md)]

## <a name="specify-dns-server"></a><a name="dns"></a>DNS sunucusunu belirtin

Bu ayarı, hub 'ı oluştururken yapılandırabilir veya daha sonra değiştirebilirsiniz. Değiştirmek için sanal hub 'ı bulun. **Kullanıcı VPN (site üzerine gelin)** altında **Yapılandır** ' ı SEÇIN ve DNS sunucusu IP adreslerini **özel DNS sunucuları** metin kutusuna (es) girin. En fazla 5 DNS sunucusu belirtebilirsiniz.

   :::image type="content" source="media/virtual-wan-point-to-site-portal/custom-dns.png" alt-text="özel DNS" lightbox="media/virtual-wan-point-to-site-portal/custom-dns-expand.png":::

## <a name="generate-and-install-client-profile-package"></a><a name="download"></a>İstemci profili paketini oluştur ve Kur

VPN istemcilerinizi yapılandırmak için VPN istemci profili paketini oluşturun.

[!INCLUDE [Download profile](../../includes/virtual-wan-p2s-download-profile-include.md)]

### <a name="configure-vpn-clients"></a>VPN istemcilerini yapılandırma

Uzaktan erişim VPN istemcilerini yapılandırmak için indirilen profili kullanın. Her işletim sistemi için yordam farklıdır. Sisteminize uygulanan yönergeleri izleyin.

[!INCLUDE [Configure clients](../../includes/virtual-wan-p2s-configure-clients-include.md)]

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
1. **Genel bakış** sayfasında, haritadaki her bir nokta bir hub 'ı temsil eder.
1. Hub **'lar ve bağlantılar** bölümünde, hub durumu, site, bölge, VPN bağlantı durumu ve baytları içinde ve dışarı izleyebilirsiniz.

## <a name="clean-up-resources"></a><a name="cleanup"></a>Kaynakları temizleme

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Sonra, sanal WAN hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> * [Sanal WAN hakkında SSS](virtual-wan-faq.md)
