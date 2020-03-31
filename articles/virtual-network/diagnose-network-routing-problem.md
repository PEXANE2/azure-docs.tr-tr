---
title: Azure sanal makine yönlendirme sorununu tanıla | Microsoft Dokümanlar
description: Sanal makinenin etkili rotalarını görüntüleyerek sanal makine yönlendirme sorununu nasıl tanılayın öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2018
ms.author: kumud
ms.openlocfilehash: 13d74fbb4a7c133ca2365fd2cbfce4b3d2bea72e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75350583"
---
# <a name="diagnose-a-virtual-machine-routing-problem"></a>Sanal makine yönlendirme sorununu tanılama

Bu makalede, sanal bir makinede (VM) ağ arabirimi için etkili olan yolları görüntüleyerek yönlendirme sorununu nasıl tanıladığınızı öğrenirsiniz. Azure, her sanal ağ alt ağı için birkaç varsayılan yol oluşturur. Bir rota tablosunda rotalar tanımlayarak ve ardından rota tablosunu bir alt ağa ilişkilendirerek Azure'un varsayılan rotalarını geçersiz kılabilir. Oluşturduğunuz yolların, Azure'un varsayılan yollarının ve şirket içi ağınızdan bir Azure VPN ağ geçidi (sanal ağınız şirket içi ağınıza bağlıysa) sınır ağ geçidi protokolü (BGP) aracılığıyla yayılan yolların birleşimi bir alt ağdaki tüm ağ arabirimleri için etkili rotalar. Sanal ağ, ağ arabirimi veya yönlendirme kavramlarını bilmiyorsanız, [Sanal ağa genel bakış,](virtual-networks-overview.md)Ağ [arabirimi](virtual-network-network-interface.md)ve [Yönlendirme genel bakışına](virtual-networks-udr-overview.md)bakın.

## <a name="scenario"></a>Senaryo

Bir VM'ye bağlanmayı denersiniz, ancak bağlantı başarısız olur. VM'ye neden bağlanamadığınızı belirlemek için, Azure [portalı](#diagnose-using-azure-portal) [PowerShell](#diagnose-using-powershell)veya [Azure CLI'yi](#diagnose-using-azure-cli)kullanarak ağ arabiriminin etkili yollarını görüntüleyebilirsiniz.

İzleyen adımlar, etkili yolları görüntülemek için varolan bir VM'niz olduğunu varsayar. Varolan bir VM'inyoksa, bu makaledeki görevleri tamamlamak için önce bir [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM dağıtın. Bu makaledeki örnekler *myVMNic1*adlı bir ağ arabirimi ile *myVM* adlı bir VM içindir. VM ve ağ arabirimi *myResourceGroup*adlı bir kaynak grubundadır ve *Doğu ABD* bölgesindedir. Sorunu tanıyağınızdaki VM için adımlardaki değerleri uygun şekilde değiştirin.

## <a name="diagnose-using-azure-portal"></a>Azure portalını kullanarak tanılama

1. [Gerekli izinlere](virtual-network-network-interface.md#permissions)sahip bir Azure hesabıyla Azure [portalına](https://portal.azure.com) giriş yapın.
2. Azure portalının üst kısmında, çalışma durumundaki bir VM'nin adını arama kutusuna girin. Arama sonuçlarında VM'nin adı göründüğünde, onu seçin.
3. Soldaki **Ayarlar'ın** altında **Ağ'ı**seçin ve adını seçerek ağ arabirimi kaynağına gidin.
     ![Ağ arabirimlerini görüntüleme](./media/diagnose-network-routing-problem/view-nics.png)
4. Solda, Etkili **yolları**seçin. **myVMNic1** adlı bir ağ arabiriminin etkili yolları aşağıdaki ![resimde gösterilmiştir: Etkili yolları görüntüleyin](./media/diagnose-network-routing-problem/view-effective-routes.png)

    VM'ye bağlı birden çok ağ arabirimi varsa, seçerek herhangi bir ağ arabiriminin etkili yollarını görüntüleyebilirsiniz. Her ağ arabirimi farklı bir alt ağda olabileceğinden, her ağ arabiriminin farklı etkili yolları olabilir.

    Önceki resimde gösterilen örnekte, listelenen yollar Azure'un her alt ağ için oluşturduğu varsayılan yollardır. Listenizin en azından bu yolları vardır, ancak sanal ağınız için etkinleştirmiş olabileceğiniz özelliklere bağlı olarak başka bir sanal ağla bakılması veya bir Azure VPN ağ geçidi aracılığıyla şirket içi ağınıza bağlanması gibi ek rotalar olabilir. Ağ arabiriminiz için görebileceğiniz yolların her biri ve diğer [rotalar](virtual-networks-udr-overview.md)hakkında daha fazla bilgi edinmek için Sanal ağ trafiği yönlendirmesine bakın. Listenizin çok sayıda yolu varsa, .csv dosyasını rota listesiyle birlikte indirmek için **İndir'i**seçmeniz daha kolay olabilir.

Önceki adımlarda Etkin rotalar VM üzerinden görüntülenmiş olsa da, etkili yolları aşağıdaki yollarla da görüntüleyebilirsiniz:
- **Bireysel ağ arabirimi**: [Ağ arabirimini](virtual-network-network-interface.md#view-network-interface-settings)nasıl görüntüleyeceklerini öğrenin.
- **Tek tek rota tablosu**: Rota tablosunu nasıl [görüntüleyizlemeyi](manage-route-table.md#view-details-of-a-route-table)öğrenin.

## <a name="diagnose-using-powershell"></a>PowerShell kullanarak tanılama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure Bulut Kabuğu'nda](https://shell.azure.com/powershell)veya bilgisayarınızdan PowerShell çalıştırarak takip eden komutları çalıştırabilirsiniz. Azure Bulut Kabuğu ücretsiz bir etkileşimli kabuktır. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell'i bilgisayarınızdan çalıştırAcaksanız, Azure PowerShell modülüne, sürüm 1.0.0 veya daha sonrasına ihtiyacınız vardır. Yüklü `Get-Module -ListAvailable Az` sürümü bulmak için bilgisayarınızda çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, `Connect-AzAccount` [gerekli izinlere](virtual-network-network-interface.md#permissions)sahip bir hesapla Azure'da oturum açmak için de çalışmanız gerekir.

[Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable)ile bir ağ arayüzü için etkili yolları alın. Aşağıdaki örnek *myVMNic1*adlı bir ağ arabirimi için etkili yolları alır , *myResourceGroup*adlı bir kaynak grubunda:

```azurepowershell-interactive
Get-AzEffectiveRouteTable `
  -NetworkInterfaceName myVMNic1 `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Çıktıda döndürülen bilgileri anlamak için [Yönlendirme genel görünümüne](virtual-networks-udr-overview.md)bakın. Çıktı yalnızca VM çalışan durumdaysa döndürülür. VM'ye bağlı birden çok ağ arabirimi varsa, her ağ arabirimi için etkili yolları gözden geçirebilirsiniz. Her ağ arabirimi farklı bir alt ağda olabileceğinden, her ağ arabiriminin farklı etkili yolları olabilir. Hala bir iletişim sorununuz varsa, [ek tanı](#additional-diagnosis) ve dikkat edilmesi [gerekenlere](#considerations)bakın.

Bir ağ arabiriminin adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM'nin adını biliyorsanız, aşağıdaki komutlar VM'ye bağlı tüm ağ arabirimlerinin adlarını döndürer:

```azurepowershell-interactive
$VM = Get-AzVM -Name myVM `
  -ResourceGroupName myResourceGroup
$VM.NetworkProfile
```

Aşağıdaki örneğe benzer çıktı alırsınız:

```powershell
NetworkInterfaces
-----------------
{/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMNic1
```

Önceki çıktıda, ağ arabirimi adı *myVMNic1'dir.*

## <a name="diagnose-using-azure-cli"></a>Azure CLI kullanarak tanılama

[Azure Bulut Kabuğu'nda](https://shell.azure.com/bash)takip eden komutları veya CLI'yi bilgisayarınızdan çalıştırarak çalıştırabilirsiniz. Bu makale, Azure CLI sürümü 2.0.32 veya daha sonra gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` [gerekli izinlere](virtual-network-network-interface.md#permissions)sahip bir hesapla Azure'da da çalıştırıp oturum açmanız gerekir.

[Az ağ nic show-effective-route-table](/cli/azure/network/nic#az-network-nic-show-effective-route-table)ile bir ağ arabirimi için etkili yolları alın. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda bulunan *myVMNic1* adlı bir ağ arabiriminin etkili yollarını alır:

```azurecli-interactive
az network nic show-effective-route-table \
  --name myVMNic1 \
  --resource-group myResourceGroup
```

Çıktıda döndürülen bilgileri anlamak için [Yönlendirme genel görünümüne](virtual-networks-udr-overview.md)bakın. Çıktı yalnızca VM çalışan durumdaysa döndürülür. VM'ye bağlı birden çok ağ arabirimi varsa, her ağ arabirimi için etkili yolları gözden geçirebilirsiniz. Her ağ arabirimi farklı bir alt ağda olabileceğinden, her ağ arabiriminin farklı etkili yolları olabilir. Hala bir iletişim sorununuz varsa, [ek tanı](#additional-diagnosis) ve dikkat edilmesi [gerekenlere](#considerations)bakın.

Bir ağ arabiriminin adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM'nin adını biliyorsanız, aşağıdaki komutlar VM'ye bağlı tüm ağ arabirimlerinin adlarını döndürer:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

## <a name="resolve-a-problem"></a>Bir sorunu çözme

Yönlendirme sorunlarını çözme genellikle aşağıdakilerden oluşur:

- Azure'un varsayılan yollarından birini geçersiz kılmak için özel bir rota ekleme. [Özel bir rota eklemeyi](manage-route-table.md#create-a-route)öğrenin.
- İstenmeyen bir konuma yönlendirmeye neden olabilecek özel bir rotayı değiştirin veya kaldırın. Özel bir rotayı nasıl [değiştireceğinizi](manage-route-table.md#change-a-route) veya [sildiğinizi](manage-route-table.md#delete-a-route) öğrenin.
- Tanımladığınız özel yolları içeren rota tablosunun ağ arabiriminin bulunduğu alt ağla ilişkili olmasını sağlamak. [Rota tablosunu bir alt ağla](manage-route-table.md#associate-a-route-table-to-a-subnet)nasıl ilişkilendireceklerini öğrenin.
- Azure VPN ağ geçidi veya dağıttığınız ağ sanal cihazları gibi aygıtların çalışabilir olmasını sağlamak. Azure VPN ağ geçidiyle ilgili sorunları belirlemek için Ağ İzleyicisi'nin [VPN tanılama](../network-watcher/diagnose-communication-problem-between-networks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) özelliğini kullanın.

İletişim sorunlarınız devam ediyorsanız, [Bkz.](#considerations)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

İletişim sorunlarını giderirken aşağıdaki noktaları göz önünde bulundurun:

- Yönlendirme, tanımladığınız rotalar, kenarlık ağ geçidi protokolü (BGP) ve sistem yolları arasındaki en uzun önek eşleşmesi (LPM) temel alınarak yapılır. Aynı LPM eşleşmesine sahip birden fazla rota varsa, [yönlendirme genel bakışta](virtual-networks-udr-overview.md#how-azure-selects-a-route)listelenen sırada kaynağına göre bir rota seçilir. Etkili rotalarla, yalnızca kullanılabilir tüm rotalara bağlı olarak LPM eşleşmesi olan etkili yolları görebilirsiniz. Yolların bir ağ arabirimi için nasıl değerlendirildiğini görmek, VM'nizden gelen iletişimi etkileyebilecek belirli yolları gidermeyi çok daha kolay hale getirir.
- Bir sonraki atlama türü olarak *Sanal Aygıt'ı içeren* bir ağ sanal cihazına (NVA) özel rotalar tanımladıysanız, trafiği alan NVA'da IP yönlendirmesinin etkin olduğundan veya paketlerin bırakıldığından emin olun. [Ağ arabirimi için IP iletmesini etkinleştirme](virtual-network-network-interface.md#enable-or-disable-ip-forwarding)hakkında daha fazla bilgi edinin. Ayrıca, işletim sistemi veya NVA içindeki uygulama da ağ trafiğini iletmek ve bunu yapmak için yapılandırılabilir gerekir.
- 0.0.0.0/0 rotası oluşturduysanız, giden tüm internet trafiği, NVA veya VPN ağ geçidi gibi belirttiğiniz sonraki atlamaya yönlendirilir. Böyle bir rota oluşturmak genellikle zorunlu tünel leme olarak adlandırılır. Internet'ten VM'nize KADAR RDP veya SSH protokollerini kullanan uzak bağlantılar, bir sonraki atlamanın trafiği nasıl işlediğine bağlı olarak bu rotayla çalışmayabilir. Zorunlu tünelleme etkinleştirilebilir:
    - Siteden siteye VPN kullanırken, *VPN Ağ Geçidi*sonraki hop türü ile bir rota oluşturarak. [Zorunlu tünel yapılandırma](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)hakkında daha fazla bilgi edinin.
    - Bir siteden siteye VPN veya ExpressRoute devresi kullanırken sanal ağ ağ ağ geçidi üzerinden BGP üzerinden 0.0.0.0/0 (varsayılan rota) reklamı yapılırsa. [Siteden siteye VPN](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ip-addresses-used-for-azure-private-peering)ile BGP kullanma hakkında daha fazla bilgi edinin.
- Sanal ağ izleme trafiğinin doğru çalışması için, bir sonraki *atlama VNet eşleme* türüne sahip bir sistem rotasının, bakan sanal ağın öneki aralığı için bulunması gerekir. Böyle bir rota yoksa ve sanal ağ izleme bağlantısı **Bağlıysa:**
    - Birkaç saniye bekle ve tekrar dene. Yeni kurulan bir eşleme bağlantısıysa, bazen bir alt ağdaki tüm ağ arabirimlerine rota lar yaymak daha uzun sürer. Sanal ağ eşleme hakkında daha fazla bilgi edinmek için [Sanal ağ ağına genel bakış alabilmem](virtual-network-peering-overview.md) ve sanal ağ [eşlemeyi yönetme'ye](virtual-network-manage-peering.md)bakın.
    - Ağ güvenliği grubu kuralları iletişimi etkiliyor olabilir. Daha fazla bilgi için [bkz.](diagnose-network-traffic-filter-problem.md)
- Azure her Azure ağ arabirimine varsayılan yollar atasa da, VM'ye bağlı birden çok ağ arabirimi varsa, VM'nin işletim sistemi içinde yalnızca birincil ağ arabirimi varsayılan bir rota (0,0.0.0/0) veya ağ geçidi atanır. [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) veya [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) VM'ye bağlı ikincil ağ arabirimleri için varsayılan bir rota oluşturmayı öğrenin. [Birincil ve ikincil ağ arabirimleri](virtual-network-network-interface-vm.md#constraints)hakkında daha fazla bilgi edinin.

## <a name="additional-diagnosis"></a>Ek tanı

* Bir konuma giden trafik için bir sonraki atlama türünü belirlemek için hızlı bir test çalıştırmak için Azure Ağ İzleyicisinin [Sonraki atlama](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) özelliğini kullanın. Sonraki atlama, belirli bir konuma giden trafik için bir sonraki atlama türünün ne olduğunu söyler.
* Bir VM'nin ağ iletişiminin başarısız olmasına neden olan bir yol yoksa, sorun VM'nin işletim sistemi içinde çalışan güvenlik duvarı yazılımından kaynaklanıyor olabilir
* Vpn ağ geçidi veya NVA aracılığıyla şirket içi bir aygıta tünel kazma trafiğini [zorluyorsanız,](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) aygıtlar için yönlendirmeyi nasıl yapılandırdığınıza bağlı olarak internetten bir VM'ye bağlanamayabilirsiniz. Aygıt için yapılandırdığınız yönlendirmenin trafiği VM için genel veya özel bir IP adresine yönlendirmesi olduğunu doğrulayın.
* Giden iletişim sorunlarının yönlendirme, filtreleme ve işletim sistemi içi nedenlerini belirlemek için Ağ İzleyicisinin [bağlantı sorun giderme](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) özelliğini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir rota tablosu ve rotalar](manage-route-table.md)için tüm görevler, özellikler ve ayarlar hakkında bilgi edinin.
- Sonraki tüm [atlama türleri, sistem yolları ve Azure'un bir rotayı nasıl seçtiği](virtual-networks-udr-overview.md)hakkında bilgi edinin.
