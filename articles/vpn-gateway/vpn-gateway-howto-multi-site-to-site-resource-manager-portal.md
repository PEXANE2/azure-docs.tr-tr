---
title: Bir VNet:Azure Portalına birden çok VPN ağ geçidi Site'den Siteye bağlantı ekleme
description: Varolan bir bağlantıya sahip bir VPN ağ geçidine çok siteli S2S bağlantıları ekleme
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/09/2019
ms.author: cherylmc
ms.openlocfilehash: 7b438f2b966dc43d41b91a138b39193d230d5546
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75779697"
---
# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Varolan VPN ağ geçidi bağlantısına sahip bir VNet'e Siteden Siteye bağlantı ekleme

> [!div class="op_single_selector"]
> * [Azure portalında](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
> * [PowerShell (klasik)](vpn-gateway-multi-site.md)
>
> 

Bu makale, Azure portalını kullanarak varolan bir bağlantısı olan bir VPN ağ geçidine Site-To-Site (S2S) bağlantıları eklemenize yardımcı olur. Bu tür bir bağlantı genellikle "çok siteli" yapılandırma olarak adlandırılır. Zaten S2S bağlantısı, Noktadan Siteye bağlantısı olan bir VNet'e S2S bağlantısı veya VNet-to-VNet bağlantısı ekleyebilirsiniz. Bağlantı eklerken bazı sınırlamalar vardır. Yapılandırmanızı [başlatmadan](#before) önce doğrulamak için bu makaledeki başlamadan önce bölümü kontrol edin. 

Bu makale, RouteBased VPN ağ geçidi olan Kaynak Yöneticisi VNet'ler için geçerlidir. Bu adımlar, yeni ExpressRoute/Site-to-Site birlikte varolan bağlantı yapılandırmaları için geçerli değildir. Ancak, zaten var olan bir yapılandırmaya yalnızca yeni bir VPN bağlantısı ekliyorsanız, bu adımları kullanabilirsiniz. Birlikte varolan bağlantılar hakkında bilgi için [ExpressRoute/S2S birlikte varolan bağlantılara](../expressroute/expressroute-howto-coexist-resource-manager.md) bakın.

### <a name="deployment-models-and-methods"></a>Dağıtım modelleri ve yöntemleri
[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Bu tabloyu, bu yapılandırma için yeni makaleler ve ek araçlar kullanılabilir hale geldikçe güncelliyoruz. Bir makale kullanılabilir olduğunda, bu tablodan doğrudan bağlantı.

[!INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)]

## <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce
Aşağıdaki öğeleri doğrulayın:

* Birlikte var olan yeni bir ExpressRoute ve VPN Ağ Geçidi yapılandırmasını yapılandırmamanız.
* Varolan bir bağlantıile Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan bir sanal ağ var.
* VNet'inizin sanal ağ ağ geçidi RouteBased'dir. Politika Tabanlı VPN ağ geçidiniz varsa, sanal ağ ağ ağ geçidini silmeniz ve RouteBased olarak yeni bir VPN ağ geçidi oluşturmanız gerekir.
* Bu VNet'in bağlandığı VNet'lerin hiçbiri için adres aralıklarının hiçbiri çakışmaz.
* Uyumlu VPN aygıtınız ve onu yapılandırabilen birine sahipsiniz. Bkz. [VPN Cihazları Hakkında](vpn-gateway-about-vpn-devices.md). VPN cihazınızı yapılandırma konusuyla veya şirket içi ağ yapılandırmanızdaki IP adresi aralıklarıyla ilgili fazla bilginiz yoksa size bu ayrıntıları sağlayabilecek biriyle çalışmanız gerekir.
* VPN aygıtınız için harici olarak karşı karşıya olan bir genel IP adresiniz vardır. Bu IP adresi bir NAT’nin arkasında olamaz.

## <a name="part-1---configure-a-connection"></a><a name="part1"></a>Bölüm 1 - Bağlantıyı yapılandırma
1. Tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve gerekiyorsa Azure hesabınızda oturum açın.
2. **Tüm kaynakları** tıklatın ve kaynak listesinden sanal ağ ağ **ağ geçidinizi** bulun ve tıklatın.
3. Sanal **ağ ağ geçidi** **sayfasında, Bağlantılar'ı**tıklatın.
   
    ![Bağlantılar sayfası](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Bağlantılar sayfası")<br>
4. **Bağlantılar** sayfasında **+Ekle'yi**tıklatın.
   
    ![Bağlantı düğmesi ekle](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Bağlantı düğmesi ekle")<br>
5. Bağlantı **Ekle** sayfasında aşağıdaki alanları doldurun:
   
   * **Adı:** Bağlantı oluşturduğunuz siteye vermek istediğiniz ad.
   * **Bağlantı türü:** **Siteden siteye (IPsec)** seçin.
     
     ![Bağlantı sayfası ekleme](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Bağlantı sayfası ekleme")<br>

## <a name="part-2---add-a-local-network-gateway"></a><a name="part2"></a>Bölüm 2 - Yerel ağ ağ geçidi ekleme
1. **Yerel ağ ağ geçidini** tıklatın Yerel ağ ağ ağ geçidi ***seçin.*** Bu, **yerel ağ ağ geçidi** seç sayfasını açar.
   
    ![Yerel ağ ağ geçidini seçin](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Yerel ağ ağ geçidini seçin")<br>
2. Yerel ağ ağ **geçidi** oluştur sayfasını açmak için yeni **oluştur'u** tıklatın.
   
    ![Yerel ağ ağ geçidi sayfası oluşturma](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Yerel ağ ağ geçidi oluşturma")<br>
3. Yerel **ağ ağ geçidi oluştur** sayfasında aşağıdaki alanları doldurun:
   
   * **Adı:** Yerel ağ ağ geçidi kaynağına vermek istediğiniz ad.
   * **IP adresi:** Bağlanmak istediğiniz sitedeki VPN aygıtının genel IP adresi.
   * **Adres alanı:** Yeni yerel ağ sitesine yönlendirilmek istediğiniz adres alanı.
4. Değişiklikleri kaydetmek için **yerel ağ ağ geçidi** oluştur sayfasında **Tamam'ı** tıklatın.

## <a name="part-3---add-the-shared-key-and-create-the-connection"></a><a name="part3"></a>Bölüm 3 - Paylaşılan anahtarı ekleyin ve bağlantıyı oluşturun
1. Bağlantı **Ekle** sayfasında, bağlantınızı oluşturmak için kullanmak istediğiniz paylaşılan anahtarı ekleyin. Paylaşılan anahtarı VPN aygıtınızdan alabilir veya burada bir tane yapabilir ve VPN aygıtınızı aynı paylaşılan anahtarı kullanacak şekilde yapılandırabilirsiniz. Önemli olan anahtarların tamamen aynı olması.
   
    ![Paylaşılan anahtar](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Paylaşılan anahtar")<br>
2. Sayfanın alt kısmında, bağlantıyı oluşturmak için **Tamam'ı** tıklatın.

## <a name="part-4---verify-the-vpn-connection"></a><a name="part4"></a>Bölüm 4 - VPN bağlantısını doğrulayın


[!INCLUDE [vpn-gateway-verify-connection-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bağlantınız tamamlandıktan sonra sanal ağlarınıza sanal makineler ekleyebilirsiniz. Daha fazla bilgi için [sanal makinelerin öğrenme yolunu](/learn/paths/deploy-a-website-with-azure-virtual-machines/) görün.
