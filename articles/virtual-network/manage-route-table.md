---
title: Azure yol tablosu oluşturma, değiştirme veya silme
titlesuffix: Azure Virtual Network
description: Yol tablosu oluşturmayı, değiştirmeyi veya silmeyi öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: kumud
ms.openlocfilehash: fe8ea4dfb4de45a1e09648ac51fe8d74f93a6b9e
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78355682"
---
# <a name="create-change-or-delete-a-route-table"></a>Rota tablosu oluşturma, değiştirme veya silme

Azure, Azure alt ağları, sanal ağlar ve şirket içi ağlar arasındaki trafiği otomatik olarak yönlendirir. Azure 'un varsayılan yönlendirmesinde herhangi birini değiştirmek istiyorsanız, bir yol tablosu oluşturarak bunu yapabilirsiniz. Sanal ağlarda yönlendirmeye yeni başladıysanız, [yönlendirmeye genel bakış](virtual-networks-udr-overview.md) veya bir [öğreticiyi](tutorial-create-route-table-portal.md)tamamlama konusunda daha fazla bilgi edinebilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin herhangi bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri doldurun:

* Henüz bir Azure hesabınız yoksa [ücretsiz deneme hesabı](https://azure.microsoft.com/free)için kaydolun.<br>
* Portalı kullanıyorsanız, https://portal.azure.comaçın ve Azure hesabınızla oturum açın.<br>
* Bu makaledeki görevleri tamamlamaya yönelik PowerShell komutlarını kullanıyorsanız, [Azure Cloud Shell](https://shell.azure.com/powershell)komutları çalıştırın veya PowerShell 'i bilgisayarınızdan çalıştırarak çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiş ve hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya üstünü gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.<br>
* Bu makaledeki görevleri gerçekleştirmek için Azure komut satırı arabirimi (CLı) komutlarını kullanıyorsanız, [Azure Cloud Shell](https://shell.azure.com/bash)komutları çalıştırın ya da bilgisayarınızdan CLI 'yı çalıştırarak. Bu öğretici, Azure CLı sürüm 2.0.31 veya üstünü gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). Azure CLı 'yi yerel olarak çalıştırıyorsanız, Azure ile bağlantı oluşturmak için `az login` çalıştırmanız da gerekir.

Oturum açmak veya Azure 'a bağlanmak için kullandığınız hesap, [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya [izinlerde](#permissions)listelenen uygun eylemlere atanmış [özel bir role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanmalıdır.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Azure konumu ve abonelik başına kaç yol tablosunun oluşturabileceğiniz bir sınır vardır. Ayrıntılar için [Azure limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portalın sol üst köşesinde **+ kaynak oluştur**' u seçin.
1. **Ağ**' ı ve ardından **yol tablosu**' nu seçin.
1. Yol tablosu için bir **ad** girin, **aboneliğinizi**seçin, yeni bir **kaynak grubu**oluşturun veya var olan bir kaynak grubunu seçin, bir **konum**seçin ve ardından **Oluştur**' u seçin. Yönlendirme tablosunu bir VPN ağ geçidi aracılığıyla şirket içi ağınıza bağlı bir sanal ağda bir alt ağla ilişkilendirmeyi planlıyorsanız ve **sanal ağ geçidi yol yaymayı**devre dışı bıraktığınızda, şirket içi yollarınız alt ağdaki ağ arabirimlerine yayılmaz.

### <a name="create-route-table---commands"></a>Rota tablosu oluşturma-komutlar

* Azure CLı: [az Network Route-Table Create](/cli/azure/network/route-table/route)<br>
* PowerShell: [New-AzRouteTable](/powershell/module/az.network/new-azroutetable)

## <a name="view-route-tables"></a>Rota tablolarını görüntüleme

Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin. Aboneliğinizde mevcut olan yol tabloları listelenir.

### <a name="view-route-table---commands"></a>Rota tablosunu görüntüleme-komutlar

* Azure CLı: [az Network Route-Table List](/cli/azure/network/route-table/route)<br>
* PowerShell: [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)

## <a name="view-details-of-a-route-table"></a>Rota tablosunun ayrıntılarını görüntüleme

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin.
1. Ayrıntılarını görüntülemek istediğiniz listede yol tablosunu seçin. **Ayarlar**' ın altında yol tablosundaki **yolları** ve yol tablosunun ilişkilendirildiği **alt ağları** görüntüleyebilirsiniz.
1. Ortak Azure ayarları hakkında daha fazla bilgi edinmek için aşağıdaki bilgilere bakın:

    * [Etkinlik Günlüğü](../azure-monitor/platform/platform-logs-overview.md)<br>
    * [Erişim denetimi (ıAM)](../role-based-access-control/overview.md)<br>
    * [Etiketler](../azure-resource-manager/management/tag-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br>
    * [Kaynaktaki](../azure-resource-manager/management/lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br>
    * [Otomasyon betiği](../azure-resource-manager/templates/export-template-portal.md)

### <a name="view-details-of-route-table---commands"></a>Rota tablosunun ayrıntılarını görüntüleme-komutlar

* Azure CLı: [az Network Route-Table Show](/cli/azure/network/route-table/route)<br>
* PowerShell: [Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)

## <a name="change-a-route-table"></a>Yol tablosunu değiştirme

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin.
1. Değiştirmek istediğiniz yol tablosunu seçin. En yaygın değişiklikler, rotalar [ekleme](#create-a-route) veya [kaldırma](#delete-a-route) , rota tablolarını alt ağlardan [ilişkilendirme](#associate-a-route-table-to-a-subnet) ve rota tablolarının [ilişkilendirmesini](#dissociate-a-route-table-from-a-subnet) .

### <a name="change-a-route-table---commands"></a>Yol tablosunu değiştirme-komutlar

* Azure CLı: [az Network Route-Table Update](/cli/azure/network/route-table/route)<br>
* PowerShell: [set-AzRouteTable](/powershell/module/az.network/set-azroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir alt ağ ile ilişkili sıfır veya bir yol tablosu olabilir. Bir yol tablosu, sıfır veya birden çok alt ağ ile ilişkilendirilebilir. Rota tabloları sanal ağlarla ilişkili olmadığından, yol tablosunun ilişkilendirilmesini istediğiniz her alt ağ için bir rota tablosu ilişkilendirmeniz gerekir. Sanal ağ bir Azure sanal ağ geçidine (ExpressRoute veya VPN) bağlıysa, alt ağdan çıkan tüm trafik, rota tabloları, [varsayılan yollar](virtual-networks-udr-overview.md#default)ve şirket içi bir ağdan yayılan yollar temel alınarak yönlendirilir. Yönlendirme tablosu ile aynı Azure konumunda ve abonelikte bulunan sanal ağlardaki alt ağlarla yalnızca bir rota tablosu ilişkilendirebilirsiniz.

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. Arama sonuçlarında **sanal ağlar** görüntülendiğinde, bunu seçin.
1. Bir yol tablosunu ilişkilendirmek istediğiniz alt ağı içeren listedeki sanal ağı seçin.
1. **Ayarlar**altında **alt ağlar** ' ı seçin.
1. Yol tablosunu ilişkilendirmek istediğiniz alt ağı seçin.
1. **Rota tablosu**' nu seçin, alt ağla ilişkilendirmek istediğiniz yol tablosunu seçin ve ardından **Kaydet**' i seçin.

Sanal ağınız bir Azure VPN ağ geçidine bağlıysa, rota tablosunu 0.0.0.0/0 hedefine sahip bir rota içeren [ağ geçidi alt ağına](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) ilişkilendirmeyin. Bunun yapılması, ağ geçidinin düzgün çalışmasını engelleyebilir. Bir rotada 0.0.0.0/0 kullanma hakkında daha fazla bilgi için bkz. [sanal ağ trafiği yönlendirme](virtual-networks-udr-overview.md#default-route).

### <a name="associate-a-route-table---commands"></a>Rota tablosunu ilişkilendirme-komutlar

* Azure CLı: [az Network VNET subnet Update](/cli/azure/network/vnet/subnet?view=azure-cli-latest)<br>
* PowerShell: [set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Bir alt ağdan yol tablosunun ilişkilendirmesini kaldırma

Bir alt ağdan yol tablosunun ilişkilendirmesini kaldırdığınızda, Azure trafiği [varsayılan yollarına](virtual-networks-udr-overview.md#default)göre yönlendirir.

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. Arama sonuçlarında **sanal ağlar** görüntülendiğinde, bunu seçin.
1. Bir yol tablosunun ilişkilendirmesini kaldırmak istediğiniz alt ağı içeren sanal ağı seçin.
1. **Ayarlar**altında **alt ağlar** ' ı seçin.
1. Yol tablosunun ilişkilendirmesini kaldırmak istediğiniz alt ağı seçin.
1. **Yol tablosu**' nu seçin, **hiçbiri**' ni seçin ve **Kaydet**' i seçin

### <a name="dissociate-a-route-table---commands"></a>Rota tablosunun ilişkilendirmesini kaldırma-komutlar

* Azure CLı: [az Network VNET subnet Update](/cli/azure/network/vnet/subnet?view=azure-cli-latest)<br>
* PowerShell: [set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig)

## <a name="delete-a-route-table"></a>Yol tablosunu silme

Bir yol tablosu herhangi bir alt ağ ile ilişkilendirilmişse silinemez. Bir yol tablosunun silmeyi denemeden önce tüm alt ağlardan [ilişkilendirmesini](#dissociate-a-route-table-from-a-subnet) kaldırın.

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin.
1. Silmek istediğiniz yol tablosunun sağ tarafında **..** . seçeneğini belirleyin.
1. **Sil**' i seçin ve ardından **Evet**' i seçin.

### <a name="delete-a-route-table---commands"></a>Yol tablosunu silme-komutlar

* Azure CLı: [az Network Route-Table Delete](/cli/azure/network/route-table/route)<br>
* PowerShell: [Remove-AzRouteTable](/powershell/module/az.network/remove-azroutetable)

## <a name="create-a-route"></a>Yönlendirme oluşturma

Azure konumu ve aboneliği başına yol tablosu başına kaç yol oluşturabileceğiniz bir sınır vardır. Ayrıntılar için [Azure limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin.
1. Yol eklemek istediğiniz listeden yol tablosunu seçin.
1. **Ayarlar**altında **rotalar**' ı seçin.
1. **+ Ekle** öğesini seçin.
1. Yol tablosu içinde yol için benzersiz bir **ad** girin.
1. Trafiği yönlendirmek istediğiniz CıDR gösteriminde **Adres önekini**girin. Ön ek, yol tablosundaki birden fazla rotada yinelenemez, ancak ön ek başka bir ön ek içinde olabilir. Örneğin, bir rotada önek olarak 10.0.0.0/16 tanımladıysanız, 10.0.0.0/24 adres ön ekine sahip başka bir yol tanımlayabilirsiniz. Azure, en uzun ön ek eşleşmesi temelinde trafik için bir yol seçer. Azure 'un yolların nasıl seçtiği hakkında daha fazla bilgi edinmek için bkz. [yönlendirmeye genel bakış](virtual-networks-udr-overview.md#how-azure-selects-a-route).
1. **Sonraki atlama türünü**seçin. Tüm sonraki atlama türlerinin ayrıntılı açıklaması için bkz. [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).
1. **Sonraki atlama adresi**IÇIN bir IP adresi girin. Bir **sonraki atlama türü**için *Sanal Gereç* seçtiyseniz yalnızca bir adres girebilirsiniz.
1. **Tamam**’ı seçin.

### <a name="create-a-route---commands"></a>Rota oluşturma-komutlar

* Azure CLı: [az Network Route-Table Route Create](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [New-AzRouteConfig](/powershell/module/az.network/new-azrouteconfig)

## <a name="view-routes"></a>Rotaları görüntüle

Bir yol tablosu sıfır veya birden çok yol içeriyor. Yolları görüntülerken listelenen bilgiler hakkında daha fazla bilgi edinmek için bkz. [yönlendirmeye genel bakış](virtual-networks-udr-overview.md).

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin.
1. Yolları görüntülemek istediğiniz listeden yol tablosunu seçin.
1. **Ayarlar**altında **rotalar** ' ı seçin.

### <a name="view-routes---commands"></a>Yolları görüntüle-komutlar

* Azure CLı: [az Network Route-Table Route List](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig)

## <a name="view-details-of-a-route"></a>Bir yolun ayrıntılarını görüntüleme

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin.
1. Bir yolun ayrıntılarını görüntülemek istediğiniz yol tablosunu seçin.
1. **Rotalar**' ı seçin.
1. Ayrıntılarını görüntülemek istediğiniz yolu seçin.

### <a name="view-details-of-a-route---commands"></a>Yol komutlarının ayrıntılarını görüntüleme

* Azure CLı: [az Network Route-Table Route Show](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Get-AzRouteConfig](/powershell/module/az.network/get-azrouteconfig)

## <a name="change-a-route"></a>Rota değiştirme

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin.
1. Bir yolu değiştirmek istediğiniz yol tablosunu seçin.
1. **Rotalar**' ı seçin.
1. Değiştirmek istediğiniz yolu seçin.
1. Var olan ayarları yeni ayarlarına değiştirip **Kaydet**' i seçin.

### <a name="change-a-route---commands"></a>Rota değiştirme komutları

* Azure CLı: [az Network Route-Table Route Update](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [set-AzRouteConfig](/powershell/module/az.network/set-azrouteconfig)

## <a name="delete-a-route"></a>Bir yolu silme

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *Rota tabloları* ' nı girin. Arama sonuçlarında **yol tabloları** görüntülendiğinde, bunu seçin.
1. Bir yolu silmek istediğiniz yol tablosunu seçin.
1. **Rotalar**' ı seçin.
1. Yollar listesinden, silmek istediğiniz yolun sağ tarafındaki **...** seçeneğini belirleyin.
1. **Sil**' i ve ardından **Evet**' i seçin.

### <a name="delete-a-route---commands"></a>Rota silme-komutlar

* Azure CLı: [az Network Route-Table Route Delete](/cli/azure/network/route-table/route?view=azure-cli-latest)<br>
* PowerShell: [Remove-AzRouteConfig](/powershell/module/az.network/remove-azrouteconfig)

## <a name="view-effective-routes"></a>Geçerli yolları görüntüle

Bir sanal makineye bağlı her bir ağ arabirimine yönelik etkili rotalar, oluşturduğunuz rota tablolarının bir birleşimidir, Azure 'un varsayılan yolları ve şirket içi ağlardan gelen ve BGP aracılığıyla Azure sanal ağ geçidi üzerinden yayılan yollar. Bir ağ arabirimine yönelik geçerli yolların anlaşılmasında, yönlendirme sorunlarını giderirken yararlı olur. Çalışan bir sanal makineye bağlı herhangi bir ağ arabirimi için geçerli yolları görüntüleyebilirsiniz.

1. Portalın üst kısmındaki arama kutusuna, için geçerli yolları görüntülemek istediğiniz bir sanal makinenin adını girin. Bir sanal makinenin adını bilmiyorsanız, arama kutusuna *sanal makineler* girin. Arama sonuçlarında **sanal makineler** görüntülendiğinde, bunu seçin ve listeden bir sanal makine seçin.
1. **Ayarlar**altında **ağ** ' ı seçin.
1. Bir ağ arabiriminin adını seçin.
1. **Destek + sorun giderme**altında **geçerli rotalar** ' ı seçin.
1. Trafiği yönlendirmek istediğiniz yere doğru yolun mevcut olup olmadığını anlamak için etkin yolların listesini gözden geçirin. Bu listede, [yönlendirmeye genel bakış](virtual-networks-udr-overview.md)bölümünde gördüğünüz sonraki atlama türleri hakkında daha fazla bilgi edinin.

### <a name="view-effective-routes---commands"></a>Geçerli rotaları görüntüleme-komutlar

* Azure CLı: [az Network Nic Show-etkin-Route-Table](/cli/azure/network/nic?view=azure-cli-latest)<br>
* PowerShell: [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable)

## <a name="validate-routing-between-two-endpoints"></a>İki uç nokta arasındaki yönlendirmeyi doğrula

Bir sanal makine ile başka bir Azure kaynağının IP adresi, bir şirket içi kaynak veya Internet 'teki bir kaynak arasındaki sonraki atlama türünü belirleyebilirsiniz. Yönlendirme sorunlarını giderirken Azure 'un yönlendirmeyi belirleme yararlı olur. Bu görevi gerçekleştirmek için, var olan bir ağ izleyicisine sahip olmanız gerekir. Mevcut bir ağ izleyicisine sahip değilseniz, [bir Ağ İzleyicisi örneği oluşturma](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)' daki adımları tamamlayarak bir tane oluşturun.

1. Portalın üst kısmındaki arama kutusuna *Ağ İzleyicisi* ' ni arama kutusuna girin. **Ağ İzleyicisi**, arama sonuçlarında görüntülendiğinde seçin.
1. **Ağ tanılama araçları**altında **Ileri atlama ' yi** seçin.
1. **Aboneliğinizi** ve yönlendirmeyi doğrulamak istediğiniz kaynak sanal makinenin **kaynak grubunu** seçin.
1. **Sanal**makineyi, sanal makineye bağlı **ağ arabirimini** ve yönlendirmeyi doğrulamak Istediğiniz ağ arabirimine atanan **kaynak IP adresini** seçin.
1. Yönlendirmeyi doğrulamak istediğiniz **hedef IP adresini** girin.
1. **Sonraki durak**' ı seçin.
1. Kısa bir bekleme sonrasında, sonraki atlama türünü ve trafiği yönlendiren yolun KIMLIĞINI bildiren bilgiler döndürülür. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md)bölümünde döndürülen sonraki atlama türleri hakkında daha fazla bilgi edinin.

### <a name="validate-routing-between-two-endpoints---commands"></a>İki uç nokta arasındaki yönlendirmeyi doğrula-komutlar

* Azure CLı: [az Network izleyici Show-Next-Hop](/cli/azure/network/watcher?view=azure-cli-latest)<br>
* PowerShell: [Get-AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop)

## <a name="permissions"></a>İzinler

Rota tablolarında ve rotalarında görevler gerçekleştirmek için, hesabınız [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya aşağıdaki tabloda listelenen uygun eylemlere atanmış [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir role atanmalıdır:

| Eylem                                                          |   Adı                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft. Network/routeTables/Read                              |   Rota tablosu okuma                                    |
| Microsoft. Network/routeTables/Write                             |   Rota tablosu oluşturma veya güncelleştirme                        |
| Microsoft. Network/routeTables/Delete                            |   Yol tablosunu silme                                  |
| Microsoft. Network/routeTables/JOIN/Action                       |   Yönlendirme tablosunu bir alt ağ ile ilişkilendirme                   |
| Microsoft. Network/routeTables/rotalar/Read                       |   Bir yolu oku                                          |
| Microsoft. Network/routeTables/rotalar/Write                      |   Rota oluşturma veya güncelleştirme                              |
| Microsoft. Network/routeTables/rotalar/Delete                     |   Bir yolu silme                                        |
| Microsoft. Network/NetworkInterfaces/effectiveRouteTable/Action  |   Ağ arabirimi için geçerli yol tablosunu al |
| Microsoft. Network/networkWatchers/Sonrakii/eylem                |   Bir VM 'den sonraki atlamayı alır                           |

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek betikleri kullanarak veya Azure [Kaynak Yöneticisi şablonlarını](template-samples.md) kullanarak rota tablosu oluşturma<br>
* Sanal ağlar için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama
