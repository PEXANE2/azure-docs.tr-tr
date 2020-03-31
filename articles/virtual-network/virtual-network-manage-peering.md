---
title: Bir Azure sanal ağ eşlemi oluşturma, değiştirme veya silme | Microsoft Dokümanlar
description: Sanal ağ eşlemesini nasıl oluştureceğinizi, değiştireceğinizi veya sileceyeceğinizi öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/01/2019
ms.author: anavin
ms.openlocfilehash: 97acac61d0397a4e13fb64d39a6aba92e4de2afd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80123298"
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Sanal ağ eşlemi oluşturma, değiştirme veya silme

Sanal ağ eşlemesini nasıl oluştureceğinizi, değiştireceğinizi veya sileceyeceğinizi öğrenin. Sanal ağ eşleme, aynı bölgedeki ve bölgeler arasında (Global VNet Peering olarak da bilinir) Azure omurga ağı üzerinden sanal ağları bağlamanızı sağlar. Bir kez bakıldıktan sonra, sanal ağlar hala ayrı kaynaklar olarak yönetilir. Sanal ağ da yeniyseniz, [sanal ağ da](virtual-network-peering-overview.md) bu konuda daha fazla bilgi edinebilirsiniz genel bakış veya bir [öğretici](tutorial-connect-virtual-networks-portal.md)tamamlayarak.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin herhangi bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, ücretsiz deneme [hesabı](https://azure.microsoft.com/free)için kaydolun.
- Portalı kullanıyorsanız, https://portal.azure.comeşlemelerle çalışmak için gerekli [izinlere](#permissions) sahip bir hesapla açın ve oturum açın.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanıyorsanız, [Azure Bulut Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, `Connect-AzAccount` Azure ile bağlantı oluşturmak için gözlerle çalışmak için [gerekli izinlere](#permissions) sahip bir hesapla da çalışmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure Komut satırı arabirimi (CLI) komutlarını kullanıyorsanız, [komutları Azure Bulut Kabuğu'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Bu öğretici, Azure CLI sürüm 2.0.31 veya sonraki sürüm gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` Azure ile bağlantı oluşturmak için gözlerle çalışmak için [gerekli izinlere](#permissions) sahip bir hesapla da çalışmanız gerekir.

Oturum açtığınız veya Azure'a bağlandığınız hesabın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya [İzinler'de](#permissions)listelenen uygun eylemlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

## <a name="create-a-peering"></a>Bir eşleme oluşturma

Bir eşleme oluşturmadan önce, gereksinimleri ve kısıtlamaları ve [gerekli izinleri](#permissions)kendinizi tanımak.

1. Azure portalının üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin. Klasik dağıtım modeli nde dağıtılan sanal bir ağdan bir görünüm oluşturamadığınız için, listede görünüyorsa **Sanal ağları (klasik)** seçmeyin.
2. Bir eşleme oluşturmak istediğiniz listedesanal ağı seçin.
3. **AYARLAR** **altında, Eşler'i**seçin.
4. **+ Ekle** öğesini seçin. 
5. <a name="add-peering"></a>Aşağıdaki ayarlar için değerleri girin veya seçin:
    - **Adı:** Eşlemenin adı sanal ağ içinde benzersiz olmalıdır.
    - **Sanal ağ dağıtım modeli:** Bakmak istediğiniz sanal ağın hangi dağıtım modeli aracılığıyla dağıtıldığını seçin.
    - **Kaynak kimliğimi biliyorum:** Eşlemek istediğiniz sanal ağa erişimi okuduysanız, bu onay kutusunu işaretsiz bırakın. Eşlemek istediğiniz sanal ağa veya aboneye okuma erişiminiz yoksa, bu kutuyu işaretleyin. Kutuyu işaretlediğinizde ortaya çıkan **Kaynak Kimliği** kutusuna bakmak istediğiniz sanal ağın tam kaynak kimliğini girin. Girdiğiniz kaynak kimliği, bu sanal ağla aynı veya [desteklenen farklı](#requirements-and-constraints) Azure [bölgesinde](https://azure.microsoft.com/regions) bulunan bir sanal ağ için olmalıdır. Tam kaynak kimliği ' `/subscriptions/<Id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<virtual-network-name>`ye benzer. Sanal bir ağın özelliklerini görüntüleyerek sanal bir ağ için kaynak kimliği alabilirsiniz. Sanal bir ağın özelliklerini nasıl görüntüleyebilirsiniz öğrenmek için [bkz.](manage-virtual-network.md#view-virtual-networks-and-settings) Abonelik, baktığınız sanal ağa sahip abonelikten farklı bir Azure Etkin Dizin kiracısıyla ilişkiliyse, önce her kiracıdan bir [kullanıcıyı](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) karşı kiracıya konuk kullanıcı olarak ekleyin.
    - **Abonelik:** Eşlemek istediğiniz sanal ağın [aboneliğini](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) seçin. Hesabınızın kaç abonelik okuduğuna bağlı olarak bir veya daha fazla abonelik listelenir. **Kaynak Kimliği** onay kutusunu işaretlediyseniz, bu ayar kullanılamaz.
    - **Sanal ağ:** Eşlemek istediğiniz sanal ağı seçin. Azure dağıtım modeli aracılığıyla oluşturulmuş bir sanal ağ seçebilirsiniz. Farklı bir bölgede sanal ağ seçmek istiyorsanız, desteklenen bir [bölgede](#cross-region)sanal ağ seçmeniz gerekir. Listede görünebilmesi için sanal ağa erişimi okumuş olmalısınız. Sanal ağ listelenmiş, ancak gri renkteyse, bunun nedeni sanal ağın adres alanının bu sanal ağın adres alanıyla çakışması olabilir. Sanal ağ adresi boşlukları çakışıyorsa, bunlara bakılamaz. **Kaynak Kimliği** onay kutusunu işaretlediyseniz, bu ayar kullanılamaz.
    - **Sanal ağ erişimine izin verin:** İki sanal ağ arasındaki iletişimi etkinleştirmek istiyorsanız **Etkin** (varsayılan) seçeneğini belirleyin. Sanal ağlar arasındaki iletişimi etkinleştirmek, sanal ağa bağlı kaynakların aynı sanal ağa bağlıymış gibi aynı bant genişliği ve gecikme siyle birbirleriyle iletişim kurmasına olanak tanır. İki sanal ağdaki kaynaklar arasındaki tüm iletişim Azure özel ağı üzerindendir. Ağ güvenliği grupları için **VirtualNetwork** hizmet etiketi sanal ağı ve eşlenen sanal ağı kapsar. Ağ güvenliği grubu hizmet etiketleri hakkında daha fazla bilgi edinmek için [Ağ güvenlik gruplarına genel bakış'a](security-overview.md#service-tags)bakın. Trafiğin karşıdan bakan sanal ağa akmasını istemiyorsanız **Devre Dışı'yı** seçin. Başka bir sanal ağa sahip bir sanal ağa baktıysanız, ancak bazen iki sanal ağ arasındaki trafik akışını devre dışı kakmak istiyorsanız **Devre Dışı Seçeneğini** belirleyebilirsiniz. Etkinleştirme/devre dışı bırakmanın, eşlemeleri silerek yeniden oluşturmaktan daha uygun olduğunu görebilirsiniz. Bu ayar devre dışı bırakıldığında, trafik bakan sanal ağlar arasında akmaz.
    - **İlileli trafiğe izin verin:** Sanal ağdaki bir ağsanal cihaz tarafından *iletilen* trafiğin (sanal ağdan kaynaklanmamış) bir gözleme yoluyla bu sanal ağa akmasını sağlamak için bu kutuyu işaretleyin. Örneğin, Spoke1, Spoke2 ve Hub adlı üç sanal ağı göz önünde bulundurun. Her konuşan sanal ağ ile Hub sanal ağı arasında bir eşleme vardır, ancak konuşan sanal ağlar arasında eşlemeler yoktur. Hub sanal ağında bir ağ sanal cihaz dağıtılır ve ağ sanal cihazı üzerinden alt ağlar arasındaki trafiği yönlendiren her kollu sanal ağa kullanıcı tanımlı rotalar uygulanır. Bu onay kutusu, her bir konuşan sanal ağ ile hub sanal ağ arasındaki eşleme için işaretlenmezse, hub sanal ağlar arasındaki trafiği iletmediğinden, konuşan sanal ağlar arasında trafik akmaz. Bu özelliği etkinleştirmek, iletilen trafiğin eşleme yoluyla yapılmasına izin verirken, kullanıcı tarafından tanımlanan herhangi bir rota veya ağ sanal beyaz eşyası oluşturmaz. Kullanıcı tanımlı rotalar ve ağ sanal cihazları ayrı ayrı oluşturulur. Kullanıcı [tanımlı rotalar](virtual-networks-udr-overview.md#user-defined)hakkında bilgi edinin. Bir Azure VPN Ağ Geçidi üzerinden sanal ağlar arasında trafik iletilirse bu ayarı denetlemeniz gerekmez.
    - **Ağ geçidi geçişine izin verin:** Bu sanal ağa bağlı bir sanal ağ ağ geçidiniz varsa ve gözlenen sanal ağdan gelen trafiğin ağ geçidinden akmasına izin vermek istiyorsanız bu kutuyu işaretleyin. Örneğin, bu sanal ağ bir sanal ağ ağ geçidi aracılığıyla şirket içi ağa bağlanabilir. Ağ geçidi bir ExpressRoute veya VPN ağ geçidi olabilir. Bu kutuyu denetlemek, bakan sanal ağdaki trafiğin bu sanal ağa bağlı ağ geçidinden şirket içi ağa akmasını sağlar. Bu kutuyu işaretleseniz, bakan sanal ağ bir ağ geçidi yapılandırılamaz. Diğer sanal ağdan bu sanal ağa bakmayı ayarlarken, eşlenen sanal **ağ, uzak ağ geçitlerini kullan** onay kutusunun işaretli olması gerekir. Bu kutuyu işaretsiz (varsayılan) bırakırsanız, eşlenen sanal ağdan gelen trafik yine de bu sanal ağa akar, ancak bu sanal ağa bağlı bir sanal ağ ağ geçidinden akmaz. Eşleme bir sanal ağ (Kaynak Yöneticisi) ve sanal ağ (klasik) arasındaysa, ağ geçidi sanal ağda (Kaynak Yöneticisi) olmalıdır.

       Bir vpn ağ geçidi, trafiği şirket içi ağa iletmeye ek olarak, ağ geçidinin içinde olduğu sanal ağla, sanal ağların birbiriyle bakılması gerekmeden, bakılmış sanal ağlar arasındaki ağ trafiğini iletebilir. Trafiği iletmek için VPN ağ geçidi kullanmak, bir hub'da vpn ağ geçidi kullanmak istediğinizde **(iletilen trafiğe izin vermek**için açıklanan hub ve konuşma örneğine bakın) sanal ağ ile birbiriyle bakmayan konuşan sanal ağlar arasındaki trafiği yönlendirmek için yararlıdır. Geçiş için ağ geçidi kullanımına izin verme hakkında daha fazla bilgi edinmek için sanal [ağ eşlemesinde geçiş için VPN ağ geçidini yapılandır'a](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın. Bu senaryo, bir sonraki atlama türü olarak sanal ağ ağ ağ geçidini belirten kullanıcı tanımlı yolların uygulanmasını gerektirir. Kullanıcı [tanımlı rotalar](virtual-networks-udr-overview.md#user-defined)hakkında bilgi edinin. Vpn ağ geçidini yalnızca kullanıcı tanımlı bir rotada bir sonraki atlama türü olarak belirtebilirsiniz, kullanıcı tanımlı bir rotada bir sonraki atlama türü olarak ExpressRoute ağ geçidi belirtemezsiniz.

    - **Uzak ağ geçitlerini kullanın:** Bu sanal ağdaki trafiğin, baktığınız sanal ağa bağlı bir sanal ağ ağ geçidinden akmasına izin vermek için bu kutuyu işaretleyin. Örneğin, baktığınız sanal ağın şirket içi bir ağa iletişim iman etmesini sağlayan bir VPN ağ geçidi vardır.  Bu kutuyu denetlemek, bu sanal ağdaki trafiğin, eşlenen sanal ağa bağlı VPN ağ geçidinden akmasını sağlar. Bu kutuyu işaretleseniz, bakan sanal ağ da ona bağlı bir sanal ağ ağ geçidi ne olmalıdır ve **geçiş geçidi geçiş** onay kutusunu işaretlemelidir. Bu kutuyu işaretsiz (varsayılan) bırakırsanız, bakan sanal ağdan gelen trafik yine de bu sanal ağa akabilir, ancak bu sanal ağa bağlı bir sanal ağ ağ geçidinden akamaz.
    
      Bu sanal ağ için yalnızca bir eşleyen bu ayarı etkinleştirebilir.

      Sanal ağınızda zaten yapılandırılmış bir ağ geçidiniz varsa uzak ağ geçitlerini kullanamazsınız. Geçiş için ağ geçidi kullanma hakkında daha fazla bilgi edinmek için sanal [ağ eşlemesinde geçiş için VPN ağ geçidini yapılandırın](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        
    > [!NOTE]
    > Bir sanal ağ ağ ağ geçidi ni, şirket içi trafiği, eşlenmiş bir VNet'e geçişli olarak göndermek için kullanıyorsanız, şirket içi VPN aygıtının eşlenen VNet IP aralığı 'ilginç' bir trafiğe ayarlanmalıdır. Aksi takdirde, şirket içi kaynaklarınız, bakan VNet'teki kaynaklarla iletişim kuramaz.

6. Seçtiğiniz sanal ağa eşlemi eklemek için **Tamam'ı** seçin.

Farklı aboneliklerde ve dağıtım modellerinde sanal ağlar arasında eşleme uygulamak için adım adım yönergeler için [sonraki adımlara](#next-steps)bakın.

### <a name="commands"></a>Komutlar

- **Azure CLI**: [az ağ vnet eşleme oluşturma](/cli/azure/network/vnet/peering)
- **PowerShell**: [Ekle-AzVirtualNetworkPeering](/powershell/module/az.network/add-azvirtualnetworkpeering)

## <a name="view-or-change-peering-settings"></a>Eşleme ayarlarını görüntüleme veya değiştirme

Bir bakış değiştirmeden önce, gereksinimleri ve kısıtlamaları ve [gerekli izinleri](#permissions)kendinizi tanımak.

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin. Klasik dağıtım modeli nde dağıtılan sanal bir ağdan bir görünüm oluşturamadığınız için, listede görünüyorsa **Sanal ağları (klasik)** seçmeyin.
2. Eşleme ayarlarını değiştirmek istediğiniz listedesanal ağı seçin.
3. **AYARLAR** **altında, Eşler'i**seçin.
4. Görüntülemek istediğiniz eşlemi seçin veya ayarları değiştirin.
5. Uygun ayarı değiştirin. Bir eş oluşturma adım [5'teki](#add-peering) her ayarın seçenekleri hakkında bilgi edinin.
6. **Kaydet'i**seçin.

**Komutlar**

- **Azure CLI**: sanal ağ için eşlemeleri listelemek için [az ağ vnet eşleme listesi,](/cli/azure/network/vnet/peering) belirli bir eşleme nin ayarlarını göstermek için [az ağ vnet eşleme gösterisi](/cli/azure/network/vnet/peering) ve eşleme ayarlarını değiştirmek için az ağ [vnet eşleme](/cli/azure/network/vnet/peering) güncelleştirmesi.|
- **PowerShell**: Görünüm ayarları almak için [Get-AzVirtualNetworkPeering](/powershell/module/az.network/get-azvirtualnetworkpeering) ve ayarları değiştirmek için [Set-AzVirtualNetworkPeering.](/powershell/module/az.network/set-azvirtualnetworkpeering)

## <a name="delete-a-peering"></a>Bir eşleme silme

Bir eşlemi silmeden önce, hesabınızın [gerekli izinlere](#permissions)sahip olduğundan emin olun.

Bir eşleme silindiğinde, sanal ağdaki trafik artık bakan sanal ağa akmaz. Kaynak Yöneticisi aracılığıyla dağıtılan sanal ağlara bakıldığında, her sanal ağ diğer sanal ağa bakar. Bir sanal ağdan eşleme silme, sanal ağlar arasındaki iletişimi devre dışı bıraksa da, diğer sanal ağdaki eşlemi silmez. Diğer sanal ağda bulunan eşlemenin eşleme durumu **Kesildi.** İlk sanal ağdaki eşlemi ve her iki sanal ağ için de Bağlı'ya yapılan *Connected*eşleme durumunu yeniden oluşturmadan eşlemi yeniden oluşturamazsınız.

Sanal ağların bazen değil, her zaman değil, bir eşlemeyi silerek iletişim kurmasını istiyorsanız, **sanal ağ erişimine izin verme** ayarını Devre Dışı **Bırak'a** ayarlayabilirsiniz. Nasıl yapılacağını öğrenmek için, bu makalenin bir eşleme bölümü oluştur'un 6. Ağ erişimini devre dışı bırakmak ve etkinleştirmek, eşlemeleri silip yeniden oluşturmaktan daha kolay olabilir.

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin. Klasik dağıtım modeli nde dağıtılan sanal bir ağdan bir görünüm oluşturamadığınız için, listede görünüyorsa **Sanal ağları (klasik)** seçmeyin.
2. Bir eşlemi silmek istediğiniz listedeki sanal ağı seçin.
3. **AYARLAR** **altında, Eşler'i**seçin.
4. Silmek istediğiniz eşlemenin sağ tarafında, sil seçeneğini seçin **... ,** **Sil'i**seçin, ardından ilk sanal ağdan eşlemi silmek için **Evet'i** seçin.
5. Diğer sanal ağdan gelen eşlemi silmek için önceki adımları tamamlayın.

**Komutlar**

- **Azure CLI**: [az ağ vnet eşleme silme](/cli/azure/network/vnet/peering)
- **PowerShell**: [Kaldır-AzVirtualNetworkPeering](/powershell/module/az.network/remove-azvirtualnetworkpeering)

## <a name="requirements-and-constraints"></a>Gereksinimler ve kısıtlamalar

- <a name="cross-region"></a>Aynı bölgedeki veya farklı bölgelerdeki sanal ağları eşleyebilirsiniz. Farklı bölgelerdeki sanal ağları niçin eşleme, *Global VNet Peering*olarak da adlandırılır. 
- Genel bir bakış oluştururken, bakan sanal ağlar herhangi bir Azure genel bulut bölgesinde veya Çin bulut bölgelerinde veya Devlet bulutu bölgelerinde bulunabilir. Bulutların üzerinden bakamazsın. Örneğin, Azure genel bulutundaki bir VNet,Azure Çin bulutundaki bir VNet'e bakılamaz.
- Bir sanal ağ içindeki kaynaklar, genel olarak eşlenen sanal ağın Temel iç yük dengeleyicisinin ön uç IP adresiyle iletişim kuramaz. Temel Load Balancer için yalnızca aynı bölge içinde destek vardır. Hem Sanal Ağ Eşleme hem de Genel Sanal Ağ Eşleme için Standart Load Balancer desteği vardır. Global VNet Peering üzerinde çalışmayan temel yük dengeleyicisi kullanan hizmetler [burada belgelenmiştir.](virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)
- Uzak ağ geçitleri kullanabilir veya genel olarak bakan sanal ağlarda ve yerel olarak bakan sanal ağlarda ağ geçidi geçişine izin verebilirsiniz.
- Sanal ağlar aynı veya farklı aboneliklerde olabilir. Sanal ağlara farklı aboneliklerde baktığınızda, her iki abonelik de aynı veya farklı Azure Etkin Dizin kiracısı ile ilişkilendirilebilir. Zaten bir AD kiracınız yoksa, [bir tane oluşturabilirsiniz.](../active-directory/develop/quickstart-create-new-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json-a-new-azure-ad-tenant) Farklı Azure Etkin Dizin kiracılarına ilişkili aboneliklerden sanal ağlarda bakma desteği Portal'da kullanılamaz. CLI, PowerShell veya Şablonlar'ı kullanabilirsiniz.
- Eşlediğiniz sanal ağlar, çakışmayan IP adresi boşluklarına sahip olmalıdır.
- Sanal ağ başka bir sanal ağla bakıldıktan sonra adres aralıklarını sanal ağın adres alanından ekalamaz veya adres aralıklarını silemezsiniz. Adres aralıkları eklemek veya kaldırmak için, eşlemi silin, adres aralıklarını ekleyin veya kaldırın ve ardından eşlemi yeniden oluşturun. Adres aralıkları eklemek veya sanal ağlardaki adres aralıklarını kaldırmak için [bkz.](manage-virtual-network.md)
- Kaynak Yöneticisi aracılığıyla dağıtılan iki sanal ağı veya kaynak yöneticisi aracılığıyla dağıtılan sanal ağ ile klasik dağıtım modeli aracılığıyla dağıtılan bir sanal ağ eşleyebilirsiniz. Klasik dağıtım modeli aracılığıyla oluşturulan iki sanal ağı eşleyemezsiniz. Azure dağıtım modellerini bilmiyorsanız, Azure [dağıtım modellerini anlayın](../azure-resource-manager/management/deployment-models.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesini okuyun. Klasik dağıtım modeliyle oluşturulan iki sanal ağı bağlamak için [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) kullanabilirsiniz.
- Resource Manager ile oluşturulmuş olan iki sanal ağı eşlerken eşlemedeki her sanal ağ için bir eşleme yapılandırılması gerekir. Eşleme durumu için aşağıdaki türlerden birini görürsünüz: 
  - *Başlatılan:* İlk sanal ağdan ikinci sanal ağa eşleme oluşturduğunuzda, eşleme durumu *Başlatılır.* 
  - *Bağlı:* İkinci sanal ağdan ilk sanal ağa eşleme oluşturduğunuzda, onun eşleme durumu *Bağlanır.* İlk sanal ağın eşleme durumunu görüntülerseniz, durumunun *Başlat'tan* *Bağlı'ya*değiştirildiğini görürsünüz. Her iki sanal ağ eşlemeleri için eşleme durumu *Bağlanıncaya*kadar eşleme başarıyla kurulamaz.
- Klasik dağıtım modeli aracılığıyla oluşturulan sanal ağ ile Kaynak Yöneticisi aracılığıyla oluşturulan sanal bir ağa bakarken, yalnızca Kaynak Yöneticisi aracılığıyla dağıtılan sanal ağ için bir eşleme yapılandırırsınız. Sanal ağ (klasik) veya klasik dağıtım modeli aracılığıyla dağıtılan iki sanal ağ arasında eşlemi yapılandıramazsınız. Sanal ağdan (Kaynak Yöneticisi) sanal ağa (Klasik) eşleme oluşturduğunuzda, eşleme durumu *Güncelleniyor,* ardından kısa bir süre *Bağlı*olarak değişir.
- İki sanal ağ arasında bir eşleme kurulur. Bakaralar geçişli değildir. Aralarında eşlemeler oluşturursanız:
  - VirtualNetwork1 & VirtualNetwork2
  - VirtualNetwork2 & VirtualNetwork3

  VirtualNetwork2 üzerinden VirtualNetwork1 ve VirtualNetwork3 arasında bir bakış yoktur. VirtualNetwork1 ve VirtualNetwork3 arasında sanal bir ağ oluşturmak istiyorsanız, VirtualNetwork1 ve VirtualNetwork3 arasında bir eşleme oluşturmanız gerekir.
- Varsayılan Azure ad çözümlemesi kullanarak eşlenen sanal ağlardaki adları çözümleyemezsiniz. Diğer sanal ağlardaki adları çözümlemek için, özel etki alanları veya özel bir DNS sunucusu [için Azure DNS](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanmanız gerekir. Kendi DNS sunucunuzu nasıl kurup kurup kurup kurygöreceğinizi öğrenmek için, [kendi DNS sunucunuzu kullanarak Ad çözünürlüğüne](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)bakın.
- Aynı bölgedeki eşlenen sanal ağlardaki kaynaklar, aynı sanal ağda yatmış gibi aynı bant genişliği ve gecikme siyle birbirleriyle iletişim kurabilir. Ancak her sanal makine boyutu kendi maksimum ağ bant genişliğine sahiptir. Farklı sanal makine boyutları için maksimum ağ bant genişliği hakkında daha fazla bilgi edinmek için [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutlarına bakın.
- Sanal ağ başka bir sanal ağa bakılabilir ve Azure sanal ağ ağ geçidine sahip başka bir sanal ağa bağlanabilir. Sanal ağlar hem eşleme hem de ağ geçidi üzerinden bağlandığında, sanal ağlar arasındaki trafik ağ geçidi yerine bakan yapılandırması üzerinden akar.
- Yeni yolların istemciye indirilmesini sağlamak için sanal ağ eşlemebaşarıyla yapılandırıldıktan sonra Site'ye nokta vpn istemcileri yeniden indirilmelidir.
- Sanal ağ eşlemesi kullanan girdi ve çıktı trafiği için nominal bir ücret uygulanır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="permissions"></a>İzinler

Sanal ağ eşlemesiyle çalışmak için kullandığınız hesaplar aşağıdaki rollere atanmalıdır:

- [Ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor): Kaynak Yöneticisi aracılığıyla dağıtılan sanal ağ için.
- [Klasik Ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor): Klasik dağıtım modeli aracılığıyla dağıtılan sanal ağ için.

Hesabınız önceki rollerden birine atanmamışsa, aşağıdaki tablodan gerekli eylemleri atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanmalıdır:

| Eylem                                                          | Adı |
|---                                                              |---   |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write  | Sanal ağ A'dan sanal ağa bir bakış oluşturmak için gereklidir B. Sanal ağ A sanal ağ olmalıdır (Kaynak Yöneticisi)          |
| Microsoft.Network/virtualNetworks/peer/action                   | Sanal ağ B (Kaynak Yöneticisi) sanal ağ A bir peering oluşturmak için gerekli                                                       |
| Microsoft.ClassicNetwork/virtualNetworks/peer/action                   | Sanal ağ B (klasik) sanal ağ A bir peering oluşturmak için gerekli                                                                |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/oku   | Sanal ağ eşlemeokuma   |
| Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Sanal ağ eşlemelerini silme |

## <a name="next-steps"></a>Sonraki adımlar

- Aynı veya farklı aboneliklerde, aynı veya farklı dağıtım modelleriyle oluşturulmuş sanal ağlar arasında, bir sanal ağ eşlemesi oluşturulur. Aşağıdaki senaryolardan biri için öğreticiyi tamamlayın:

  |Azure dağıtım modeli             | Abonelik  |
  |---------                          |---------|
  |Her ikisi de Resource Manager              |[Aynı](tutorial-connect-virtual-networks-portal.md)|
  |                                   |[Farklı](create-peering-different-subscriptions.md)|
  |Biri Resource Manager, diğeri klasik  |[Aynı](create-peering-different-deployment-models.md)|
  |                                   |[Farklı](create-peering-different-deployment-models-subscriptions.md)|

- [Hub ve konuşan ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json) oluşturmayı öğrenin
- [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyalarını kullanarak veya Azure [Kaynak Yöneticisi şablonlarını](template-samples.md) kullanarak sanal ağ eşleme oluşturma
- Sanal ağlar için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama
