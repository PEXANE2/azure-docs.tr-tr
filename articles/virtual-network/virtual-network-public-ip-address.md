---
title: Azure genel IP adresi oluşturma, değiştirme veya silme | Microsoft Dokümanlar
description: Herkese açık bir IP adresini nasıl oluştureceğinizi, değiştireceğinizi veya silecek yapılacağını öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/06/2019
ms.author: kumud
ms.openlocfilehash: 38ff40b537730418fe9f0f8295884dae98a2fe0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244920"
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Genel IP adresi oluşturma, değiştirme veya silme

Herkese açık bir IP adresi ve nasıl oluşturulacak, değiştirilip silecek hakkında bilgi edinin. Ortak IP adresi, kendi yapılandırılabilir ayarları olan bir kaynaktır. Ortak IP adreslerini destekleyen bir Azure kaynağına ortak BIR IP adresi atamak şunları sağlar:
- Azure Sanal Makineler (VM), Azure Uygulama Ağ Geçitleri, Azure Yük Dengeleyicileri, Azure VPN Ağ Geçitleri ve diğerleri gibi Internet'ten kaynağa gelen iletişim. VM yük dengeleyici arka uç havuzunun bir parçası olduğu ve yük bakiyeleyiciye ortak bir IP adresi atandığı sürece, VM'nin kendisine atanmış genel bir IP adresi yoksa, Internet'ten VM'ler gibi bazı kaynaklarla iletişim kurmaya devam edebilirsiniz. Belirli bir Azure hizmetiiçin bir kaynağın genel bir IP adresi ne zaman atanabileceğini veya farklı bir Azure kaynağının genel IP adresi aracılığıyla iletilip iletilemeyeceğini belirlemek için hizmetin belgelerine bakın.
- Öngörülebilir bir IP adresi kullanarak Internet'e giden bağlantı. Örneğin, sanal bir makine internete giden, herkese açık bir IP adresi olmadan iletişim kurabilir, ancak adresi varsayılan olarak Azure tarafından öngörülemeyen bir genel adrese çevrilen ağ adresidir. Bir kaynağa ortak bir IP adresi atamak, giden bağlantı için hangi IP adresinin kullanıldığını bilmenizi sağlar. Tahmin edilebilir olsa da, seçilen atama yöntemine bağlı olarak adres değişebilir. Daha fazla bilgi için [bkz.](#create-a-public-ip-address) Azure kaynaklarından giden bağlantılar hakkında daha fazla bilgi edinmek için [bkz.](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin herhangi bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, ücretsiz deneme [hesabı](https://azure.microsoft.com/free)için kaydolun.
- Portalı kullanıyorsanız, https://portal.azure.comAzure hesabınızla açın ve oturum açın.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanıyorsanız, [Azure Bulut Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure Komut satırı arabirimi (CLI) komutlarını kullanıyorsanız, [komutları Azure Bulut Kabuğu'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Bu öğretici, Azure CLI sürüm 2.0.31 veya sonraki sürüm gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` Azure ile bağlantı oluşturmak için de çalıştırmanız gerekir.

Oturum açtığınız veya Azure'a bağlandığınız hesabın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya [İzinler'de](#permissions)listelenen uygun eylemlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

Genel IP adreslerinin nominal ücreti vardır. Fiyatlandırmayı görüntülemek için [IP adresi fiyatlandırma](https://azure.microsoft.com/pricing/details/ip-addresses) sayfasını okuyun.

## <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
2. *Marketi Ara* kutusuna *genel ip adresini* girin. Arama sonuçlarında **Genel IP adresi** göründüğünde, adresi seçin.
3. Genel IP adresi altında **Oluştur'u**seçin. **Create**
4. **Genel IP adresi oluştur**altında aşağıdaki ayarlar için değerleri girin veya seçin , ardından **Oluştur'u**seçin:

   |Ayar|Gerekli mi?|Ayrıntılar|
   |---|---|---|
   |IP Sürümü|Evet| IPv4 veya IPv6 veya Her Ikisini seçin. Her ikisinin de seçilmesi, 1 IPv4 adresi ve 1 IPv6 adresi olmak üzere 2 Genel IP adresinin oluşturulmasına neden olur. [Azure VNET'lerde IPv6](../virtual-network/ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)hakkında daha fazla bilgi edinin.|
   |SKU|Evet|SKU'ların piyasaya sürülmesinden önce oluşturulan tüm genel IP adresleri **Temel** SKU genel IP adresleridir. Ortak IP adresi oluşturulduktan sonra SKU'yu değiştiremezsiniz. Tek başına bir sanal makine, kullanılabilirlik kümesi içindeki sanal makineler veya sanal makine ölçek kümeleri Temel veya Standart SUS'ları kullanabilir. Kullanılabilirlik kümeleri veya ölçek setleri veya bağımsız VM'ler içinde sanal makineler arasında SK'lerin karıştırılmasına izin verilmez. **Temel** SKU: Kullanılabilirlik bölgelerini destekleyen bir bölgede genel bir IP adresi oluşturuyorsanız, **Kullanılabilirlik bölgesi** ayarı varsayılan olarak *Yok* olarak ayarlanır. Temel Genel IP'ler Kullanılabilirlik bölgelerini desteklemez. **Standart** SKU: Standart SKU genel IP sanal bir makine veya yük dengeleyici ön uç ilişkili olabilir. Kullanılabilirlik bölgelerini destekleyen bir bölgede genel bir IP adresi oluşturuyorsanız, **Kullanılabilirlik bölgesi** ayarı varsayılan olarak *Bölge yedekli* olarak ayarlanır. Kullanılabilirlik bölgeleri hakkında daha fazla bilgi için **Kullanılabilirlik bölgesi** ayarına bakın. Adresi standart yük dengeleyiciyle ilişkilendirirseniz standart SKU gereklidir. Standart yük dengeleyicileri hakkında daha fazla bilgi edinmek için [Azure yük dengeleyici sku](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)standardına bakın. Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.|
   |Adı|Evet|Ad seçtiğiniz kaynak grubunda benzersiz olmalıdır.|
   |IP adresi ataması|Evet|**Dinamik:** Dinamik adresler, yalnızca ortak bir IP adresi Azure kaynağıyla ilişkilendirilip kaynak ilk kez başlatıldıktan sonra atanır. Dinamik adresler, sanal makine gibi bir kaynağa atanmışsa ve sanal makine durdurulursa (ayrılırsa) ve sonra yeniden başlatılırsa değişebilir. Sanal bir makine yeniden başlatılırsa veya durdurulursa (ancak tahsis edilmezse) adres aynı kalır. Dinamik adresler, ortak bir IP adresi kaynağı ilişkili olduğu bir kaynaktan ayrıştırıldığında serbest bırakılır. **Statik:** Ortak bir IP adresi oluşturulduğunda statik adresler atanır. Statik adresler, ortak bir IP adresi kaynağı silinene kadar serbest bırakılmaz. Adres bir kaynakla ilişkilendirilmezse, adres oluşturulduktan sonra atama yöntemini değiştirebilirsiniz. Adres bir kaynakla ilişkiliyse, atama yöntemini değiştiremeyebilirsiniz. **IP sürümü**için *IPv6'yı* seçerseniz, atama yöntemi Temel SKU için *Dinamik* olmalıdır.  Standart SKU adresleri hem IPv4 hem de IPv6 için *Statiktir.* |
   |Boşta zaman/zaman ası (dakika)|Hayır|Bir TCP veya HTTP bağlantısını, istemcilerin canlı tutma iletileri göndermesine güvenmeden açık tutmanın kaç dakika sı. **IP Sürümü**için IPv6'yı seçerseniz, bu değer değiştirilemez. |
   |DNS ad etiketi|Hayır|Adını oluşturduğunuz Azure konumu içinde benzersiz olmalıdır (tüm aboneliklerde ve tüm müşterilerde). Azure, ad ve IP adresini DNS'sine otomatik olarak kaydeder, böylece ada sahip bir kaynağa bağlanabilirsiniz. Azure, tam nitelikli DNS adını oluşturmak için sağladığınız ada *location.cloudapp.azure.com* (konum seçtiğiniz konum konum) gibi varsayılan bir alt ağı ekler. Her iki adres sürümü de oluşturmayı seçerseniz, aynı DNS adı hem IPv4 hem de IPv6 adreslerine atanır. Azure'un varsayılan DNS'si hem IPv4 A hem de IPv6 AAAA ad kayıtlarını içerir ve DNS adı arandığında her iki kayıtla da yanıt verir. İstemci hangi adresle iletişim kureceğini (IPv4 veya IPv6) seçer. Varsayılan son ek ile DNS ad etiketini kullanmak yerine veya buna ek olarak, genel IP adresine çözümlenen bir özel son ek ile DNS adını yapılandırmak için Azure DNS hizmetini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure genel IP adresiyle Azure DNS kullanma](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|
   |Ad (Yalnızca **her ikisinin**DE IP Sürümünü seçerseniz görünür)|Evet, **her ikisinin** DE IP Sürümünü seçerseniz|Ad, bu listedeki ilk **Ad** için girdiğiniz addan farklı olmalıdır. Hem IPv4 hem de IPv6 adresi oluşturmayı seçerseniz, portal iki ayrı genel IP adresi kaynağı oluşturur ve her IP adresi sürümünde ona atanmış bir sürümüdür.|
   |IP adresi ataması (Yalnızca her **ikisinin**DE IP Sürümünü seçerseniz görünür )|Evet, **her ikisinin** DE IP Sürümünü seçerseniz|Yukarıdaki IP Adresi Ataması ile aynı kısıtlamalar|
   |Abonelik|Evet|Genel IP'leri ilişkilendireceğiniz kaynakla aynı [abonelikte](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) bulunmalıdır.|
   |Kaynak grubu|Evet|Ortak IP'leri ilişkilendireceğiniz kaynakla aynı veya farklı [kaynak grubunda](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) bulunabilir.|
   |Konum|Evet|Ortak IP'leri ilişkilendireceğiniz kaynak olarak bölge olarak da adlandırılan aynı [konumda](https://azure.microsoft.com/regions)bulunmanız gerekir.|
   |Kullanılabilirlik alanı| Hayır | Bu ayar yalnızca desteklenen bir konum seçtiğinizde görüntülenir. Desteklenen konumların listesi için [Kullanılabilirlik bölgelerine genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bölümüne bakın. **Temel** SKU'yu seçtiyseniz, *Hiçbiri* sizin için otomatik olarak seçilir. Belirli bir bölgeyi garanti etmeyi tercih ederseniz, belirli bir bölge seçebilirsiniz. Her iki seçenek de bölge gereksiz değildir. **Standart** SKU'yu seçtiyseniz: Bölge yedekliotomatik olarak sizin için seçilir ve veri yolunuzu bölge arızasına karşı esnek hale getirir. Bölge arızasına dayanıklı olmayan belirli bir bölgeyi garanti etmeyi tercih ederseniz, belirli bir bölge seçebilirsiniz.

**Komutlar**

Portal iki genel IP adresi kaynağı (bir IPv4 ve bir IPv6) oluşturma seçeneği sağlasa da, aşağıdaki CLI ve PowerShell komutları bir IP sürümü veya diğeri için adresiçeren bir kaynak oluşturur. Her IP sürümü için bir tane olmak üzere iki genel IP adresi kaynağı istiyorsanız, genel IP adresi kaynakları için farklı adlar ve IP sürümleri belirterek komutu iki kez çalıştırmanız gerekir.

|Araç|Komut|
|---|---|
|CLI|[az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create)|
|PowerShell|[Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Genel bir IP adresini görüntüleme, değiştirme veya silme

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuya genel *ip adresi*yazın. **Ortak IP adresleri** arama sonuçlarında göründüğünde, bu adresleri seçin.
2. Görüntülemek, ayarları değiştirmek veya listeden silmek istediğiniz genel IP adresinin adını seçin.
3. Genel IP adresini görüntülemek, silmek veya değiştirmek isteyip istemediğiniz önemli olmak üzere aşağıdaki seçeneklerden birini tamamlayın.
   - **Görünüm**: **Genel Bakış** bölümü, ilişkili olduğu ağ arabirimi gibi genel IP adresinin anahtar ayarlarını gösterir (adres bir ağ arabirimiyle ilişkiliyse). Portal adresin sürümünü (IPv4 veya IPv6) görüntülemez. Sürüm bilgilerini görüntülemek için, genel IP adresini görüntülemek için PowerShell veya CLI komutunu kullanın. IP adresi sürümü IPv6 ise, atanan adres portal, PowerShell veya CLI tarafından görüntülenmez.
   - **Sil**: Genel IP adresini silmek için **Genel Bakış** bölümünde **Sil'i** seçin. Adres şu anda bir IP yapılandırması ile ilişkiliyse, silinemez. Adres şu anda bir yapılandırmayla ilişkiliyse, adresi IP yapılandırmasından çıkarmak için **Ayrıştırmayı** seçin.
   - **Change**: **Configuration'ı**seçin. [Ortak IP adresi oluşturma'nın](#create-a-public-ip-address)4. Bir IPv4 adresinin atamasını statikten dinamik'e değiştirmek için, öncelikle ortak IPv4 adresini ilişkili olduğu IP yapılandırmasından ayırmanız gerekir. Daha sonra atama yöntemini dinamik olarak değiştirebilir ve IP adresini aynı IP yapılandırması, farklı bir yapılandırma yla ilişkilendirmek için **Ilişkilendir'i** seçebilir veya ayrı bırakabilirsiniz. Genel **Bakış** bölümünde genel bir IP adresini ayrıştırmak için **Dissociate'i**seçin.

   >[!WARNING]
   >Atama yöntemini statikten dinamik'e değiştirdiğinizde, genel IP adresine atanan IP adresini kaybedersiniz. Azure genel DNS sunucuları statik veya dinamik adresler ile herhangi bir DNS ad etiketi (birini tanımladıysanız) arasında bir eşleme tutarken, sanal makine durduruldu (ayrılan) durumda olduktan sonra başlatıldığında dinamik bir IP adresi değişebilir. Adresin değişmesini önlemek için statik bir IP adresi atayın.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ public-ip listesi](/cli/azure/network/public-ip#az-network-public-ip-list) genel IP adreslerini listelemek için, [az ağ public-ip göstermek](/cli/azure/network/public-ip#az-network-public-ip-show) ayarları göstermek için; [az ağ public-ip güncelleme](/cli/azure/network/public-ip#az-network-public-ip-update) güncellemek için; [az ağ public-ip silme](/cli/azure/network/public-ip#az-network-public-ip-delete)|
|PowerShell|Genel bir IP adresi nesnesini almak ve ayarlarını görüntülemek için [Get-AzPublicIpAddress,](/powershell/module/az.network/get-azpublicipaddress) Ayarları güncelleştirmek için [Set-AzPublicIpAddress;](/powershell/module/az.network/set-azpublicipaddress) [Sil-AzPublicIpAddress](/powershell/module/az.network/remove-azpublicipaddress) sil|

## <a name="assign-a-public-ip-address"></a>Herkese açık bir IP adresi atama

Genel bir IP adresini aşağıdaki kaynaklara nasıl atayacaklarınız öğrenin:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM (oluştururken) veya varolan bir [VM'ye](virtual-network-network-interface-addresses.md#add-ip-addresses)
- [Internet'e bakan Yük Dengeleyici](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Application Gateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure VPN Ağ Geçidi kullanarak siteden siteye bağlantı](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Sanal Makine Ölçeği Seti](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>İzinler

Ortak IP adreslerindeki görevleri gerçekleştirmek için, hesabınızın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya aşağıdaki tabloda listelenen uygun eylemleri atanan [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir role atanması gerekir:

| Eylem                                                             | Adı                                                           |
| ---------                                                          | -------------                                                  |
| Microsoft.Network/publicIPAddresses/read                           | Herkese açık bir IP adresini okuma                                          |
| Microsoft.Network/publicIPAddresses/write                          | Herkese açık bir IP adresi oluşturma veya güncelleştirme                           |
| Microsoft.Network/publicIPAddresses/delete                         | Genel bir IP adresini silme                                     |
| Microsoft.Network/publicIPAddresses/join/action                    | Ortak bir IP adresini bir kaynakla ilişkilendirme                    |

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyalarını kullanarak veya Azure [Kaynak Yöneticisi şablonlarını](template-samples.md) kullanarak ortak bir IP adresi oluşturun
- Genel IP adresleri için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama
