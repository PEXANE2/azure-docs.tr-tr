---
title: Azure rota tablosu oluşturma, değiştirme veya silme
titlesuffix: Azure Virtual Network
description: Rota tablosunu nasıl oluştureceğinizi, değiştireceğinizi veya sildiğini öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/19/2020
ms.author: kumud
ms.openlocfilehash: 79310ddf121d6ada10755b198b515fdc9c1114d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247070"
---
# <a name="create-change-or-delete-a-route-table"></a>Rota tablosu oluşturma, değiştirme veya silme

Azure, Azure alt ağları, sanal ağlar ve şirket içi ağlar arasındaki trafiği otomatik olarak yönlendirir. Azure'un varsayılan yönlendirmelerinden herhangi birini değiştirmek istiyorsanız, bunu bir rota tablosu oluşturarak yaparsınız. Sanal ağlarda yönlendirmede yeniyseniz, [sanal ağ trafiği yönlendirmesinde](virtual-networks-udr-overview.md) veya bir [öğreticiyi](tutorial-create-route-table-portal.md)tamamlayarak bu konuda daha fazla bilgi edinebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Hesabınız yoksa, etkin bir abonelikle bir Azure hesabı ayarlayın. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Ardından, bu makalenin herhangi bir bölümündeki adımları başlatmadan önce bu görevlerden birini tamamlayın:

- **Portal kullanıcıları**: Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

- **PowerShell kullanıcıları**: Azure Bulut [Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Azure Bulut BulutU tarayıcı sekmesinde, çevre açılır **listesini seçin** ve daha önce seçilmemişse **PowerShell'i** seçin.

    PowerShell'i yerel olarak çalıştırıyorsanız, Azure PowerShell modül sürüm 1.0.0 veya sonrası kullanın. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az.Network` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). Azure `Connect-AzAccount` ile bağlantı oluşturmak için de çalıştırın.

- **Azure Komut satırı arabirimi (CLI) kullanıcıları**: [Azure Bulut BulutU'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Azure CLI sürümünü yerel olarak çalıştırıyorsanız Azure CLI sürüm 2.0.31 veya sonraki sürümlerini kullanın. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure `az login` ile bağlantı oluşturmak için de çalıştırın.

Oturum açtığınız veya Azure'a bağlandığınız hesabın [Ağ katılımcısı rolüne](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) veya [İzinler'de](#permissions)listelenen uygun eylemlere atanmış bir [Özel role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Azure konumu ve aboneliği başına kaç rota masası oluşturabileceğinizin bir sınırı vardır. Ayrıntılar için [Ağ sınırları - Azure Kaynak Yöneticisi'](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)ne bakın.

1. Azure [portalı](https://portal.azure.com) menüsünde veya **Ana** sayfadan **kaynak oluştur'u**seçin.

1. Arama kutusuna *Rota tablosunu*girin. Arama sonuçlarında **Rota tablosu** göründüğünde, onu seçin.

1. Rota **tablosu** sayfasında **Oluştur'u**seçin.

1. Rota **oluştur tablosu** iletişim kutusunda:

    1. Rota tablosu için bir **Ad** girin.
    1. **Aboneliğinizi**seçin.
    1. Varolan bir **Kaynak grubu** seçin veya yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin.
    1. **Konum**seçin.
    1. Rota tablosunu, bir VPN ağ geçidi aracılığıyla şirket içi ağınıza bağlı sanal bir ağdaki bir alt ağla ilişkilendirmek istiyorsanız ve şirket içi rotalarınızı alt ağdaki ağ arabirimlerine yaymak istemiyorsanız, **Sanal ağ ağ geçidi rotası yayılmasını** Devre Dışı **Bırakılmış**olarak ayarlayın.

1. Yeni rota tablonuzu oluşturmak için **Oluştur'u** seçin.

### <a name="create-route-table---commands"></a>Rota tablosu oluşturma - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create) |
| PowerShell | [Yeni-AzRouteTable](/powershell/module/az.network/new-azroutetable) |

## <a name="view-route-tables"></a>Rota tablolarını görüntüleme

Sanal ağınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin. Aboneliğinizde bulunan rota tabloları listelenir.

### <a name="view-route-table---commands"></a>Rota tablosunu görüntüle - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ rota-tablo listesi](/cli/azure/network/route-table#az-network-route-table-list) |
| PowerShell | [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable) |

## <a name="view-details-of-a-route-table"></a>Rota tablosunun ayrıntılarını görüntüleme

1. Sanal ağınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablosu listesinde, ayrıntıları görüntülemek istediğiniz rota tablosunu seçin.

1. Rota tablosu sayfasında, **Ayarlar**altında, rota tablosundaki **Yolları** veya rota tablosunun ilişkili olduğu **Alt Ağları** görüntüleyin.

Ortak Azure ayarları hakkında daha fazla bilgi edinmek için aşağıdaki bilgilere bakın:

- [Etkinlik günlüğü](../azure-monitor/platform/platform-logs-overview.md)
- [Erişim denetimi (IAM)](../role-based-access-control/overview.md)
- [Etiketler](../azure-resource-manager/management/tag-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Kilitler](../azure-resource-manager/management/lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Otomasyon betiği](../azure-resource-manager/templates/export-template-portal.md)

### <a name="view-details-of-route-table---commands"></a>Rota tablosunun ayrıntılarını görüntüleyin - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ rota-tablo gösterisi](/cli/azure/network/route-table#az-network-route-table-show) |
| PowerShell | [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable) |

## <a name="change-a-route-table"></a>Rota tablosunu değiştirme

1. Sanal ağınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablosu listesinde, değiştirmek istediğiniz rota tablosunu seçin.

En yaygın değişiklikler rota [lar eklemek,](#create-a-route) yolları [kaldırmak,](#delete-a-route) rota tablolarını alt ağlara [ilişkilendirmek](#associate-a-route-table-to-a-subnet) veya rota tablolarını alt ağlardan [ayırmaktır.](#dissociate-a-route-table-from-a-subnet)

### <a name="change-a-route-table---commands"></a>Rota tablosunu değiştirme - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ rota tablosu güncelleştirmesi](/cli/azure/network/route-table#az-network-route-table-update) |
| PowerShell | [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) |

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

İsteğe bağlı olarak bir rota tablosunu bir alt ağla ilişkilendirebilirsiniz. Bir rota tablosu sıfır veya daha fazla alt ağlarla ilişkilendirilebilir. Rota tabloları sanal ağlarla ilişkili olmadığından, bir rota tablosunu ilişkili rota tablosunu istediğiniz her alt ağla ilişkilendirmeniz gerekir. Azure, sanal ağ bir Azure sanal ağ ağ geçidine (ExpressRoute veya VPN) bağlıysa, rota tabloları, [varsayılan rotalar](virtual-networks-udr-overview.md#default)ve şirket içi ağdan yayılan rotalar içinde oluşturduğunuz rotaları temel alan alt ağdan ayrılan tüm trafiği yönlendirir. Bir rota tablosunu yalnızca aynı Azure konumunda bulunan sanal ağlardaki alt ağlarla ilişkilendirebilirsiniz ve rota tablosuyla aynı abonelikte.

1. Sanal ağınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Sanal ağları**arayın ve seçin.

1. Sanal ağ listesinde, bir rota tablosunu ilişkilendirmek istediğiniz alt ağı içeren sanal ağı seçin.

1. Sanal ağ menüsü çubuğunda **Alt Ağlar'ı**seçin.

1. Rota tablosunu ilişkilendirmek istediğiniz alt ağı seçin.

1. **Rota tablosunda,** alt ağla ilişkilendirmek istediğiniz rota tablosunu seçin.

1. **Kaydet'i**seçin.

Sanal ağınız bir Azure VPN ağ geçidine bağlıysa, *0.0.0.0/0*hedefi olan bir rota içeren [ağ geçidi alt ağına](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) bir rota tablosu bağlamayın. Bunun yapılması, ağ geçidinin düzgün çalışmasını engelleyebilir. Bir rotada *0.0.0.0/0* kullanma hakkında daha fazla bilgi için Sanal ağ trafiği yönlendirmesi'ne bakın. [Virtual network traffic routing](virtual-networks-udr-overview.md#default-route)

### <a name="associate-a-route-table---commands"></a>Rota tablosunu ilişkilendirme - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) |
| PowerShell | [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) |

## <a name="dissociate-a-route-table-from-a-subnet"></a>Bir rota tablosunu alt ağdan ayırma

Bir rota tablosunu bir alt ağdan ayırdığınızda, Azure trafiği [varsayılan rotalarına](virtual-networks-udr-overview.md#default)göre yönlendirir.

1. Sanal ağınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Sanal ağları**arayın ve seçin.

1. Sanal ağ listesinde, rota tablosunu birbirinden ayırmak istediğiniz alt ağı içeren sanal ağı seçin.

1. Sanal ağ menüsü çubuğunda **Alt Ağlar'ı**seçin.

1. Rota tablosunu ayrıştırmak istediğiniz alt ağı seçin.

1. **Rota tablosunda** **Yok'u**seçin.

1. **Kaydet'i**seçin.

### <a name="dissociate-a-route-table---commands"></a>Bir rota tablosunu ayrıştırın - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) |
| PowerShell | [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) |

## <a name="delete-a-route-table"></a>Rota tablosunu silme

Alt ağlarla ilişkili bir rota tablosunu silemezsiniz. [Dissociate](#dissociate-a-route-table-from-a-subnet) Bir rota tablosunu silmeye çalışmadan önce tüm alt ağlardan ayırın.

1. Rota tablolarınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablosu listesinde, silmek istediğiniz rota tablosunu seçin.

1. **Sil'i**ve ardından onay iletişim kutusunda **Evet'i** seçin.

### <a name="delete-a-route-table---commands"></a>Rota tablosunu silme - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ rota-tablo silme](/cli/azure/network/route-table#az-network-route-table-delete) |
| PowerShell | [Remove-AzRouteTable](/powershell/module/az.network/remove-azroutetable) |

## <a name="create-a-route"></a>Yönlendirme oluşturma

Azure konumu ve abonelik başına rota tablosu başına kaç rota oluşturabileceğinin bir sınırı vardır. Ayrıntılar için [Ağ sınırları - Azure Kaynak Yöneticisi'](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)ne bakın.

1. Rota tablolarınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablosu listesinde, rota eklemek istediğiniz rota tablosunu seçin.

1. Rota tablosu menü çubuğundan > **Rotaekle'yi**seçin. **Routes**

1. Rota tablosuiçindeki rota için benzersiz bir **Rota adı** girin.

1. Trafiği yönlendirmek istediğiniz Sınıfsız Etki Alanları Yönlendirme (CIDR) notasına **Adres önekini**girin. Önek, rota tablosu içinde birden fazla rotada çoğaltılamaz, ancak önek başka bir önek içinde olabilir. Örneğin, *10.0.0.0/16'yı* bir rotada önek olarak tanımladıysanız, yine de *10.0.0.22* adresi önekiyle başka bir rota tanımlayabilirsiniz. Azure, en uzun önek eşleşmesi temel alınerek trafik için bir rota seçer. Daha fazla bilgi edinmek için [Azure'un rotayı nasıl seçtiğine](virtual-networks-udr-overview.md#how-azure-selects-a-route)bakın.

1. Sonraki **atlama türünü**seçin. Sonraki atlama türleri hakkında daha fazla bilgi edinmek için [Sanal ağ trafiği yönlendirmesine](virtual-networks-udr-overview.md)bakın.

1. Bir Sonraki **sanal** **cihaz**türü seçtiyseniz, Sonraki **atlama adresi**için bir IP adresi girin.

1. **Tamam'ı**seçin.

### <a name="create-a-route---commands"></a>Rota oluşturma - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create) |
| PowerShell | [Yeni-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig) |

## <a name="view-routes"></a>Rotaları görüntüleme

Rota tablosu sıfır veya daha fazla yol içerir. Rotaları görüntülerken listelenen bilgiler hakkında daha fazla bilgi edinmek için [Sanal ağ trafiği yönlendirmesine](virtual-networks-udr-overview.md)bakın.

1. Rota tablolarınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablosu listesinde, rotaları görüntülemek istediğiniz rota tablosunu seçin.

1. Rota tablosu menüsü çubuğunda, rota listesini görmek için **Rotalar'ı** seçin.

### <a name="view-routes---commands"></a>Yolları görüntüleyin - komutları

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ rota-tablo rota listesi](/cli/azure/network/route-table/route#az-network-route-table-route-list) |
| PowerShell | [Al-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig) |

## <a name="view-details-of-a-route"></a>Bir yolun ayrıntılarını görüntüleme

1. Rota tablolarınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablosu listesinde, ayrıntıları görüntülemek istediğiniz rotayı içeren rota tablosunu seçin.

1. Rota tablosu menüsü çubuğunda, rota listesini görmek için **Rotalar'ı** seçin.

1. Ayrıntıları görüntülemek istediğiniz rotayı seçin.

### <a name="view-details-of-a-route---commands"></a>Rotanın ayrıntılarını görüntüleyin - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ rota-tablo rota gösterisi](/cli/azure/network/route-table/route#az-network-route-table-route-show) |
| PowerShell | [Al-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig) |

## <a name="change-a-route"></a>Rotayı değiştirme

1. Rota tablolarınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablosu listesinde, değiştirmek istediğiniz rotayı içeren rota tablosunu seçin.

1. Rota tablosu menüsü çubuğunda, rota listesini görmek için **Rotalar'ı** seçin.

1. Değiştirmek istediğiniz rotayı seçin.

1. Varolan ayarları yeni ayarlarıyla değiştirin ve ardından **Kaydet'i**seçin.

### <a name="change-a-route---commands"></a>Rotayı değiştirme - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ rota tablosu rota güncelleştirmesi](/cli/azure/network/route-table/route#az-network-route-table-route-update) |
| PowerShell | [Set-AzRouteConfig](/powershell/module/az.network/set-azrouteconfig) |

## <a name="delete-a-route"></a>Rotayı silme

1. Rota tablolarınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Rota tablolarını**arayın ve seçin.

1. Rota tablosu listesinde, silmek istediğiniz rotayı içeren rota tablosunu seçin.

1. Rota tablosu menüsü çubuğunda, rota listesini görmek için **Rotalar'ı** seçin.

1. Silmek istediğiniz rotayı seçin.

1. **Sil'i**seçin, ardından onay iletişim kutusunda **Evet'i** seçin.

### <a name="delete-a-route---commands"></a>Rotayı silme - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ rota-tablo rota silme](/cli/azure/network/route-table/route#az-network-route-table-route-delete) |
| PowerShell | [Kaldır-AzRouteConfig](/powershell/module/az.network/remove-azrouteconfig) |

## <a name="view-effective-routes"></a>Etkili rotaları görüntüleme

VM'ye bağlı her ağ arabirimi için etkili rotalar, oluşturduğunuz rota tabloları, Azure'un varsayılan yolları ve bir Azure sanal ağı üzerinden Sınır Geçidi Protokolü (BGP) aracılığıyla şirket içi ağlardan yayılan tüm yolların bir birleşimidir. Ağ geçidi. Bir ağ arabirimi için etkili yolları anlamak, yönlendirme sorunlarını giderme de yararlıdır. Çalışan bir VM'ye bağlı herhangi bir ağ arabiriminin etkili rotalarını görüntüleyebilirsiniz.

1. VM'lerinizi yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

1. Sanal makine listesinde, etkili rotaları görüntülemek istediğiniz VM'yi seçin.

1. VM menü çubuğunda **Ağ'ı**seçin.

1. Ağ arabiriminin adını seçin.

1. Ağ arabirimi menü çubuğunda **Etkili yolları**seçin.

1. Trafiği yönlendirmek istediğiniz yer için doğru rotanın bulunup bulunmadığınızı görmek için etkili yolların listesini gözden geçirin. [Sanal ağ trafiği yönlendirmesinde](virtual-networks-udr-overview.md)bu listede gördüğünüz sonraki atlama türleri hakkında daha fazla bilgi edinin.

### <a name="view-effective-routes---commands"></a>Etkili yolları görüntüleyin - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ nic show-etkili-rota-tablo](/cli/azure/network/nic#az-network-nic-show-effective-route-table) |
| PowerShell | [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable) |

## <a name="validate-routing-between-two-endpoints"></a>İki uç nokta arasındaki yönlendirmeyi doğrulama

Sanal makine ile başka bir Azure kaynağının IP adresi, şirket içi kaynak veya Internet'teki bir kaynak arasındaki bir sonraki atlama türünü belirleyebilirsiniz. Azure'un yönlendirmesi belirlemek, yönlendirme sorunlarını giderme de yararlıdır. Bu görevi tamamlamak için varolan bir ağ izleyiciniz olmalıdır. Varolan bir ağ izleyiciniz yoksa, [Ağ İzleyicisi Oluştur örneğindeki](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)adımları tamamlayarak bir tane oluşturun.

1. Ağ izleyicilerinizi yönetmek için [Azure portalına](https://portal.azure.com) gidin. **Ağ İzleyicisini**arayın ve seçin.

1. Ağ izleyicisi menü çubuğunda **Sonraki atlama'yı**seçin.

1. Ağ **Watcher | Sonraki hop** sayfası:

    1. **Aboneliğinizi** ve yönlendirmeyi doğrulamak istediğiniz kaynak VM'nin **Kaynak grubunu** seçin.

    1. Sanal **makineyi** ve VM'ye bağlı **Ağ arabirimini** seçin.
    
    1. Yönlendirmeyi doğrulamak istediğiniz ağ arabirimine atanmış bir **Kaynak IP adresi** girin.

    1. Yönlendirmeyi doğrulamak istediğiniz bir **Hedef IP adresi** girin.

1. **Sonraki atlama'yı**seçin.

Kısa bir bekleyişten sonra Azure, trafiği yönlendiren bir sonraki atlama türünü ve rotanın kimliğini söyler. [Sanal ağ trafiği yönlendirmesinde](virtual-networks-udr-overview.md)döndürülen sonraki atlama türleri hakkında daha fazla bilgi edinin.

### <a name="validate-routing-between-two-endpoints---commands"></a>İki uç nokta arasındaki yönlendirmeyi doğrulayın - komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ izleyici show-next-hop](/cli/azure/network/watcher#az-network-watcher-show-next-hop) |
| PowerShell | [Get-AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop) |

## <a name="permissions"></a>İzinler

Rota tabloları ve güzergahlar üzerinde görev yapmak için, hesabınızın [Ağ katılımcısı rolüne](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) veya aşağıdaki tabloda listelenen uygun eylemlere atanmış özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir:

| Eylem                                                          |   Adı                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/okuma                              |   Rota tablosunu okuma                                    |
| Microsoft.Network/routeTables/yazma                             |   Rota tablosu oluşturma veya güncelleştirme                        |
| Microsoft.Network/routeTables/silme                            |   Rota tablosunu silme                                  |
| Microsoft.Network/routeTables/join/action                       |   Yönlendirme tablosunu bir alt ağ ile ilişkilendirme                   |
| Microsoft.Network/routeTables/routes/read                       |   Rotayı okuma                                          |
| Microsoft.Network/routeTables/routes/write                      |   Rota oluşturma veya güncelleştirme                              |
| Microsoft.Network/routeTables/routes/delete                     |   Rotayı silme                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   Ağ arabirimi için etkili rota tablosunu alın |
| Microsoft.Network/networkWatchers/nextHop/action                |   Bir VM'den bir sonraki atlamayı alır                           |

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyalarını veya Azure [Kaynak Yöneticisi şablonlarını](template-samples.md) kullanarak bir rota tablosu oluşturma
- Sanal ağlar için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama
