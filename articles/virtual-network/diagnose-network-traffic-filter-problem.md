---
title: Sanal makine ağı trafik filtresi sorununu tanıla | Microsoft Dokümanlar
description: Sanal makinenin etkili güvenlik kurallarını görüntüleyerek sanal makine ağı trafik filtresi sorununu nasıl tanılayarak nasıl tanılamayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2018
ms.author: kumud
ms.openlocfilehash: 6939ea2497a9f12321e1a6dfb9bf9fbb353bc7db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240769"
---
# <a name="diagnose-a-virtual-machine-network-traffic-filter-problem"></a>Sanal makine ağı trafik filtresi sorununu tanılama

Bu makalede, sanal makine (VM) için etkili olan ağ güvenlik grubu (NSG) güvenlik kurallarını görüntüleyerek ağ trafiği filtresi sorununu nasıl tanıladığınızı öğrenirsiniz.

NSG'ler, VM'ye girip çıkan trafik türlerini kontrol etmenizi sağlar. Bir NSG'yi Azure sanal ağındaki bir alt ağla, VM'ye bağlı bir ağ arabirimiyle veya her ikisiyle ilişkilendirebilirsiniz. Ağ arabirimine uygulanan etkili güvenlik kuralları, ağ arabirimiyle ilişkili NSG'de bulunan kuralların ve ağ arabiriminin bulunduğu alt ağdaki kuralların bir araya toplanmasıdır. Farklı NSG'lerde kurallar bazen birbiriyle çakışabilir ve VM'nin ağ bağlantısını etkileyebilir. VM ağ arabirimlerinde uygulanan NSG'lerin tüm etkili güvenlik kurallarını görüntüleyebilirsiniz. Sanal ağ, ağ arabirimi veya NSG kavramlarını bilmiyorsanız, [Sanal ağa genel bakış,](virtual-networks-overview.md) [Ağ arabirimi](virtual-network-network-interface.md)ve [Ağ güvenlik gruplarına genel bakış](security-overview.md)alabilirsiniz.

## <a name="scenario"></a>Senaryo

Bir VM'ye internetten 80 bağlantı noktası üzerinden bağlanmayı denersiniz, ancak bağlantı başarısız olur. Bağlantı noktası 80'e Internet'ten neden erişemediğinizi belirlemek için, Azure [portalı](#diagnose-using-azure-portal) [PowerShell](#diagnose-using-powershell)veya [Azure CLI'yi](#diagnose-using-azure-cli)kullanarak ağ arabiriminin etkili güvenlik kurallarını görüntüleyebilirsiniz.

İzleyen adımlar, etkili güvenlik kurallarını görüntülemek için varolan bir VM'niz olduğunu varsayar. Varolan bir VM'inyoksa, bu makaledeki görevleri tamamlamak için önce bir [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM dağıtın. Bu makaledeki örnekler *myVMVMNic*adlı bir ağ arabirimi ile *myVM* adlı bir VM içindir. VM ve ağ arabirimi *myResourceGroup*adlı bir kaynak grubundadır ve *Doğu ABD* bölgesindedir. Sorunu tanıyağınızdaki VM için adımlardaki değerleri uygun şekilde değiştirin.

## <a name="diagnose-using-azure-portal"></a>Azure portalını kullanarak tanılama

1. [Gerekli izinlere](virtual-network-network-interface.md#permissions)sahip bir Azure hesabıyla Azure [portalına](https://portal.azure.com) giriş yapın.
2. Azure portalının üst kısmında, arama kutusuna VM'nin adını girin. Arama sonuçlarında VM'nin adı göründüğünde, onu seçin.
3. **SETTINGS**altında, aşağıdaki resimde gösterildiği gibi **Ağ'ı**seçin:

   ![Güvenlik kurallarını görüntüleme](./media/diagnose-network-traffic-filter-problem/view-security-rules.png)

   Önceki resimde listelenen gördüğünüz kurallar **myVMVMNic**adlı bir ağ arabirimi içindir. İki farklı ağ güvenlik grubundan ağ arabirimi için **GELEN PORT KURALLARI** olduğunu görürsünüz:
   
   - **mySubnetNSG**: Ağ arabiriminin içinde olduğu alt ağla ilişkilidir.
   - **myVMNSG**: **MyVMVMNic**adlı VM ağ arabirimi ile ilişkilidir.

   **DenyAllInBound** adlı [kural, senaryoda](#scenario)açıklandığı gibi, internetten, port 80 üzerinden VM gelen iletişimi engelleyen şeydir. Kural, interneti içeren **SOURCE**için *0.0.0.0/0* listeler. Daha yüksek önceliğe (daha düşük sayıya) sahip başka hiçbir kural 80 giriş noktasısağlar. Internet'ten VM'ye gelen bağlantı noktası 80'e izin vermek için [bkz.](#resolve-a-problem) Güvenlik kuralları ve Azure'un bunları nasıl uyguladığı hakkında daha fazla bilgi edinmek için [Ağ güvenlik gruplarına](security-overview.md)bakın.

   Resmin alt kısmında, GIDEN **PORT**KURALLARI'nı da görürsünüz. Bunun altında ağ arabirimi için giden bağlantı noktası kuralları vardır. Resim her NSG için yalnızca dört gelen kural gösterse de, NSG'lerinizin dörtten fazla kuralı olabilir. Resimde, SOURCE **ve** **DESTINATION** altında **VirtualNetwork** ve **SOURCE**altında **AzureLoadBalancer** bakın. **VirtualNetwork** ve **AzureLoadBalancer** [hizmet etiketleridir.](security-overview.md#service-tags) Hizmet etiketleri, güvenlik kuralı oluşturma karmaşıklığını en aza indirmeye yardımcı olmak için bir IP adresi önekleri grubunu temsil eder.

4. VM'nin çalışan durumda olduğundan emin olun ve ardından aşağıdaki resimde gösterilen etkili güvenlik kurallarını görmek için önceki resimde gösterildiği gibi **Etkili güvenlik kurallarını**seçin:

   ![Etkili güvenlik kurallarını görüntüleme](./media/diagnose-network-traffic-filter-problem/view-effective-security-rules.png)

   Listelenen kurallar, ağ arabirimi ve alt ağla ilişkili NSG için farklı sekmeler olmasına rağmen, adım 3'te gördüğünüzle aynıdır. Resimde gördüğünüz gibi, yalnızca ilk 50 kural gösterilir. Tüm kuralları içeren bir .csv dosyasını indirmek için **İndir'i**seçin.

   Her hizmet etiketinin hangi öneklerin temsil olduğunu görmek **için, AllowAzureLoadBalancerInbound**adlı kural gibi bir kural seçin. Aşağıdaki resim, **AzureLoadBalancer** hizmet etiketinin önekleri gösterir:

   ![Etkili güvenlik kurallarını görüntüleme](./media/diagnose-network-traffic-filter-problem/address-prefixes.png)

   **AzureLoadBalancer** hizmet etiketi yalnızca bir önek temsil etse de, diğer hizmet etiketleri birkaç önek temsil eder.

5. Önceki adımlar **myVMVMNic**adlı bir ağ arabirimi için güvenlik kuralları gösterdi, ama aynı zamanda önceki resimlerin bazılarında **myVMVMNic2** adlı bir ağ arabirimi gördüm. Bu örnekte VM'nin bağlı iki ağ arabirimi vardır. Etkili güvenlik kuralları her ağ arabirimi için farklı olabilir.

   **myVMVMNic2** ağ arabiriminin kurallarını görmek için seçin. Aşağıdaki resimde gösterildiği gibi, ağ arabirimi alt ağıyla ilişkili kurallara sahip, **çünkü** her iki ağ arabirimi de aynı alt ağdadır. Bir NSG'yi bir alt ağla ilişkilendirdiğinizde, kuralları alt ağdaki tüm ağ arabirimlerine uygulanır.

   ![Güvenlik kurallarını görüntüleme](./media/diagnose-network-traffic-filter-problem/view-security-rules2.png)

   **myVMVMNic** ağ arabiriminin aksine, **myVMVMNic2** ağ arabiriminde bir ağ güvenlik grubu yoktur. Her ağ arabirimi ve alt net sıfır veya bir, NSG ilişkili olabilir. Her ağ arabirimi veya alt ağ ile ilişkili NSG aynı veya farklı olabilir. Aynı ağ güvenlik grubunu istediğiniz kadar ağ arabirimi ve alt ağla ilişkilendirebilirsiniz.

Etkin güvenlik kuralları VM aracılığıyla görüntülenmiş olsa da, etkili güvenlik kurallarını bir birey aracılığıyla da görüntüleyebilirsiniz:
- **Ağ arabirimi**: [Ağ arabirimini](virtual-network-network-interface.md#view-network-interface-settings)nasıl görüntüleyeceklerini öğrenin.
- **NSG**: [NSG'yi](manage-network-security-group.md#view-details-of-a-network-security-group)nasıl görüntüleyebilirsiniz öğrenin.

## <a name="diagnose-using-powershell"></a>PowerShell kullanarak tanılama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure Bulut Kabuğu'nda](https://shell.azure.com/powershell)veya bilgisayarınızdan PowerShell çalıştırarak takip eden komutları çalıştırabilirsiniz. Azure Bulut Kabuğu ücretsiz bir etkileşimli kabuktır. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. PowerShell'i bilgisayarınızdan çalıştırAcaksanız, Azure PowerShell modülüne, sürüm 1.0.0 veya daha sonrasına ihtiyacınız vardır. Yüklü `Get-Module -ListAvailable Az` sürümü bulmak için bilgisayarınızda çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, `Connect-AzAccount` [gerekli izinlere](virtual-network-network-interface.md#permissions)sahip bir hesapla Azure'da oturum açmak için de çalışmanız gerekir.]

[Get-AzEffectiveNetworkSecurityGroup](/powershell/module/az.network/get-azeffectivenetworksecuritygroup)ile bir ağ arabirimi için etkili güvenlik kurallarını alın. Aşağıdaki örnek *myVMVMNic*adlı bir ağ arabirimi için etkili güvenlik kuralları alır , *myResourceGroup*adlı bir kaynak grubunda:

```azurepowershell-interactive
Get-AzEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVMVMNic `
  -ResourceGroupName myResourceGroup
```

Çıktı json formatında döndürülür. Çıktıyı anlamak için [komut çıktısını yorumla'ya](#interpret-command-output)bakın.
Çıktı yalnızca bir NSG ağ arabirimiyle, ağ arabiriminin içinde olduğu alt ağla veya her ikisiyle ilişkiliyse döndürülür. VM çalışan durumda olmalıdır. Bir VM'de farklı NSG'ler uygulanmış birden çok ağ arabirimi olabilir. Sorun giderme de, her ağ arabirimi için komutu çalıştırın.

Bağlantı sorunu hala yaşıyorsanız, ek [tanı](#additional-diagnosis) ve [dikkat edilmesi gerekenleri](#considerations)görün.

Bir ağ arabiriminin adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM'nin adını biliyorsanız, aşağıdaki komutlar VM'ye bağlı tüm ağ arabirimlerinin adlarını döndürer:

```azurepowershell-interactive
$VM = Get-AzVM -Name myVM -ResourceGroupName myResourceGroup
$VM.NetworkProfile
```

Aşağıdaki örneğe benzer çıktı alırsınız:

```output
NetworkInterfaces
-----------------
{/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic
```

Önceki çıktıda, ağ arabirimi adı *myVMVMNic*olduğunu.

## <a name="diagnose-using-azure-cli"></a>Azure CLI kullanarak tanılama

Bu makaledeki görevleri tamamlamak için Azure Komut satırı arabirimi (CLI) komutlarını kullanıyorsanız, [komutları Azure Bulut Kabuğu'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Bu makale, Azure CLI sürümü 2.0.32 veya daha sonra gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` [gerekli izinlere](virtual-network-network-interface.md#permissions)sahip bir hesapla Azure'da da çalıştırıp oturum açmanız gerekir.

[az ağ nic listesi-etkili-nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg)ile bir ağ arabirimi için etkili güvenlik kuralları alın. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda bulunan *myVMVMNic* adlı bir ağ arabirimi için etkili güvenlik kurallarını alır:

```azurecli-interactive
az network nic list-effective-nsg \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Çıktı json formatında döndürülür. Çıktıyı anlamak için [komut çıktısını yorumla'ya](#interpret-command-output)bakın.
Çıktı yalnızca bir NSG ağ arabirimiyle, ağ arabiriminin içinde olduğu alt ağla veya her ikisiyle ilişkiliyse döndürülür. VM çalışan durumda olmalıdır. Bir VM'de farklı NSG'ler uygulanmış birden çok ağ arabirimi olabilir. Sorun giderme de, her ağ arabirimi için komutu çalıştırın.

Bağlantı sorunu hala yaşıyorsanız, ek [tanı](#additional-diagnosis) ve [dikkat edilmesi gerekenleri](#considerations)görün.

Bir ağ arabiriminin adını bilmiyorsanız, ancak ağ arabiriminin bağlı olduğu VM'nin adını biliyorsanız, aşağıdaki komutlar VM'ye bağlı tüm ağ arabirimlerinin adlarını döndürer:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

Döndürülen çıktıiçinde, aşağıdaki örneğe benzer bilgiler görürsünüz:

```output
"networkProfile": {
    "additionalProperties": {},
    "networkInterfaces": [
      {
        "additionalProperties": {},
        "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
        "primary": true,
        "resourceGroup": "myResourceGroup"
      },
```

Önceki çıktıda, ağ arabirimi adı *myVMVMNic arabirimidir.*

## <a name="interpret-command-output"></a>Komut çıktısını yorumlama

Sorunu tanılamak için [PowerShell'i](#diagnose-using-powershell)veya [Azure CLI'yi](#diagnose-using-azure-cli) kullanıp kullanmadığınıza bakılmaksızın, aşağıdaki bilgileri içeren çıktı alırsınız:

- **NetworkSecurityGroup**: Ağ güvenlik grubunun kimliği.
- **İlişkilendirme**: Ağ güvenlik grubunun Bir *Ağ Arabirimi* veya *Subnet*ile ilişkili olup olmadığı. Bir NSG her ikisiyle de ilişkiliyse, çıkış her NSG için **NetworkSecurityGroup**, **Association**ve **EffectiveSecurityRules**ile döndürülür. Etkili güvenlik kurallarını görüntülemek için komutu çalıştırmadan hemen önce NSG ilişkiliyse veya ilişkilendirilirse, değişikliğin komut çıkışına yansıması için birkaç saniye beklemeniz gerekebilir.
- **EffectiveSecurityRules**: Her özelliğin [açıklaması bir güvenlik kuralı oluştur'da](manage-network-security-group.md#create-a-security-rule)ayrıntılı olarak açıklanır. *Varsayılan Güvenlik Kuralları/* ile önceden karşılaşılan kural adları, her NSG'de bulunan varsayılan güvenlik kurallarıdır. *Güvenlik Kuralları/* oluşturduğun kurallarla önceden gelen kural adları. **HedefAdresPrefix** veya **sourceAddressPrefix** özellikleri için **Internet,** **VirtualNetwork**ve **AzureLoadBalancer** gibi bir [hizmet etiketi](security-overview.md#service-tags)belirten kurallar da **genişletilmişDestinationAddressPrefix** özelliği için değerlere sahiptir. **GenişletilmişDestinationAddressPrefix** özelliği, hizmet etiketitarafından temsil edilen tüm adres önekleri listeler.

Çıktıda yinelenen kurallar listelenirse, bunun nedeni nsg'nin hem ağ arabirimi yle hem de alt ağla ilişkili olmasıdır. Her iki NSG'de de aynı varsayılan kurallar vardır ve her iki NSG'de de aynı olan kendi kurallarınızı oluşturduysanız, ek yinelenen kurallarınız olabilir.

**DefaultSecurityRules/DenyAllInBound** adlı kural, [senaryoda](#scenario)açıklandığı gibi, internetten 80 no'lu bağlantı noktası üzerinden VM'ye gelen iletişimi engelleyen kuraldır. Daha yüksek bir öncelik (düşük sayı) ile başka bir kural bağlantı noktası 80 internetten gelen sağlar.

## <a name="resolve-a-problem"></a>Bir sorunu çözme

Bu makalede [senaryoda](#scenario) sunulan sorunu tanılamak için Azure [portalını](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell)veya [Azure CLI'yi](#diagnose-using-azure-cli) kullanın, çözüm aşağıdaki özelliklere sahip bir ağ güvenliği kuralı oluşturmaktır:

| Özellik                | Değer                                                                              |
|---------                |---------                                                                           |
| Kaynak                  | Herhangi biri                                                                                |
| Kaynak bağlantı noktası aralıkları      | Herhangi biri                                                                                |
| Hedef             | VM'nin IP adresi, bir dizi IP adresi veya alt ağdaki tüm adresler. |
| Hedef bağlantı noktası aralıkları | 80                                                                                 |
| Protokol                | TCP                                                                                |
| Eylem                  | İzin Ver                                                                              |
| Öncelik                | 100                                                                                |
| Adı                    | İzin Ver-HTTP-Tümler                                                                     |

Kuralı oluşturduktan sonra, kuralın önceliği trafiği reddeden *DenyAllInBound*adlı varsayılan güvenlik kuralından daha yüksek olduğundan, bağlantı noktası 80'in internetten girmesine izin verilir. [Güvenlik kuralını](manage-network-security-group.md#create-a-security-rule)nasıl oluşturabilirsiniz öğrenin. Farklı NSG'ler hem ağ arabirimi yle hem de alt ağla ilişkiliyse, her iki NSG'de de aynı kuralı oluşturmanız gerekir.

Azure gelen trafiği işlediğinde, alt ağla ilişkili NSG'deki kuralları işler (ilişkili bir NSG varsa) ve ardından ağ arabirimiyle ilişkili NSG'deki kuralları işler. Ağ arabirimi ve alt ağile ilişkili bir NSG varsa, trafiğin VM'ye ulaşabilmesi için bağlantı noktasının her iki NSG'de de açık olması gerekir. Yönetim ve iletişim sorunlarını hafifletmek için, tek tek ağ arabirimleri yerine bir NSG'yi bir alt ağla ilişkilendirmenizi öneririz. Bir alt ağdaki VM'lerin farklı güvenlik kurallarına ihtiyacı varsa, ağ arabirimlerini bir uygulama güvenlik grubunun (ASG) üyeleri yapabilir ve bir güvenlik kuralının kaynağı ve hedefi olarak bir ASG belirtebilirsiniz. [Uygulama güvenlik grupları](security-overview.md#application-security-groups)hakkında daha fazla bilgi edinin.

İletişim sorunlarınız devam ediyorsanız, [Bkz.](#considerations)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bağlantı sorunlarını giderirken aşağıdaki noktaları göz önünde bulundurun:

* Varsayılan güvenlik kuralları Internet'ten gelen erişimi engeller ve yalnızca sanal ağdan gelen trafiğe izin verir. Internet'ten gelen trafiğe izin vermek için, varsayılan kurallardan daha yüksek önceliğe sahip güvenlik kuralları ekleyin. [Varsayılan güvenlik kuralları](security-overview.md#default-security-rules)veya güvenlik [kuralının](manage-network-security-group.md#create-a-security-rule)nasıl eklenöğretilen hakkında daha fazla bilgi edinin.
* Sanal ağlara baktıysanız, varsayılan **olarak, VIRTUAL_NETWORK** hizmet etiketi, eşlenen sanal ağlar için önekleri içerecek şekilde otomatik olarak genişler. Sanal ağ eşlemeyle ilgili sorunları gidermek için **ExpandedAddressPrefix** listesindeönekleri görüntüleyebilirsiniz. [Sanal ağ izleme](virtual-network-peering-overview.md) ve [hizmet etiketleri](security-overview.md#service-tags)hakkında daha fazla bilgi edinin.
* Etkili güvenlik kuralları yalnızca VM'nin ağ arabirimi yle ilişkili bir NSG ve veya alt ağ varsa ve VM çalışan durumdaysa ağ arabirimi için gösterilir.
* Ağ arabirimi veya alt ağıyla ilişkili NSG'ler yoksa ve VM'ye atanmış genel bir [IP adresiniz](virtual-network-public-ip-address.md) varsa, tüm bağlantı noktaları her yerden gelen ve giden erişim için açıktır. VM'nin genel bir IP adresi varsa, ağ arabiriminin alt ağına bir NSG uygulamanızı öneririz.

## <a name="additional-diagnosis"></a>Ek tanı

* Trafiğin bir VM'ye izin verilip verilmediğini belirlemek için hızlı bir test çalıştırmak için Azure Ağ İzleyicisinin [IP akışını doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) özelliğini kullanın. IP akışı doğrulaması, trafiğe izin verilip verilmediğini veya reddedilip reddedildiğini bildirir. Reddedilirse, IP akışı hangi güvenlik kuralının trafiği reddettiğini doğrular.
* VM'nin ağ bağlantısının başarısız olmasına neden olan güvenlik kuralları yoksa, sorun şu neden olabilir:
  * VM işletim sistemi içinde çalışan güvenlik duvarı yazılımı
  * Sanal cihazlar veya şirket içi trafik için yapılandırılan rotalar. İnternet [trafiği, zorunlu tünel leme](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)yoluyla şirket içi ağınıza yönlendirilebilir. Tünel internet trafiğini sanal bir cihaza veya şirket içinde zorlarsanız, Internet'ten VM'ye bağlanamayabilirsiniz. VM'den trafik akışını engelleyebilecek rota sorunlarını nasıl tanılayacağımhakkında [bilgi](diagnose-network-routing-problem.md)edinmek için bkz.

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ güvenlik grubu](manage-network-security-group.md#work-with-network-security-groups) ve güvenlik [kurallarının](manage-network-security-group.md#work-with-security-rules)tüm görevleri, özellikleri ve ayarları hakkında bilgi edinin.
- Varsayılan [güvenlik kuralları,](security-overview.md#default-security-rules) [hizmet etiketleri](security-overview.md#service-tags)ve Bir VM [için gelen ve giden trafik için Azure'un güvenlik kurallarını nasıl işlediği](security-overview.md#network-security-groups) hakkında bilgi edinin.
