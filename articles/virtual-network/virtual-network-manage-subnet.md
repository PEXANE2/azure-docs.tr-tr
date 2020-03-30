---
title: Azure sanal ağ alt ağı ekleme, değiştirme veya silme
titlesuffix: Azure Virtual Network
description: Azure'da sanal ağ alt ağı eklemeyi, değiştirmeyi veya silmeyi öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2020
ms.author: kumud
ms.openlocfilehash: e5b78969f6b4315bb02b3f4152c6eeab94adddb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80246951"
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Sanal ağ alt ağını ekleme, değiştirme veya silme

Sanal ağ alt ağı eklemeyi, değiştirmeyi veya silmeyi öğrenin. Sanal ağa dağıtılan tüm Azure kaynakları, sanal ağ içindeki bir alt ağa dağıtılır. Sanal ağlarda yeniyseniz, [Sanal ağa genel bakışta](virtual-networks-overview.md) veya hızlı bir [başlangıç](quick-create-portal.md)yaparak bunlar hakkında daha fazla bilgi edinebilirsiniz. Sanal ağı yönetme hakkında daha fazla bilgi edinmek için sanal [ağ oluştur, değiştirme veya silme](manage-virtual-network.md)hakkında bilgi edinin.

## <a name="before-you-begin"></a>Başlamadan önce

Hesabınız yoksa, etkin bir abonelikle bir Azure hesabı ayarlayın. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Ardından, bu makalenin herhangi bir bölümündeki adımları başlatmadan önce bu görevlerden birini tamamlayın: 

- **Portal kullanıcıları**: Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

- **PowerShell kullanıcıları**: Azure Bulut [Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Azure Bulut BulutU tarayıcı sekmesinde, çevre açılır **listesini seçin** ve daha önce seçilmemişse **PowerShell'i** seçin.

    PowerShell'i yerel olarak çalıştırıyorsanız, Azure PowerShell modül sürüm 1.0.0 veya sonrası kullanın. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az.Network` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). Azure `Connect-AzAccount` ile bağlantı oluşturmak için de çalıştırın.

- **Azure Komut satırı arabirimi (CLI) kullanıcıları**: [Azure Bulut BulutU'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Azure CLI sürümünü yerel olarak çalıştırıyorsanız Azure CLI sürüm 2.0.31 veya sonraki sürümlerini kullanın. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure `az login` ile bağlantı oluşturmak için de çalıştırın.

Oturum açtığınız veya Azure'a bağlandığınız hesabın [Ağ katılımcısı rolüne](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) veya [İzinler'de](#permissions)listelenen uygun eylemlere atanmış özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

## <a name="add-a-subnet"></a>Alt ağ ekleme

1. Sanal ağlarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. **Sanal ağları**arayın ve seçin.

2. Alt ağ eklemek istediğiniz sanal ağın adını seçin.

3. **Ayarlar'dan** **Subnets** > **Subnet'i**seçin.

4. Alt **ağ** ekle iletişim kutusuna, aşağıdaki ayarlar için değerleri girin:

    | Ayar | Açıklama |
    | --- | --- |
    | **Adı** | Ad, sanal ağ içinde benzersiz olmalıdır. Diğer Azure hizmetleriyle maksimum uyumluluk için, adın ilk karakteri olarak bir harf kullanmanızı öneririz. Örneğin, Azure Uygulama Ağ Geçidi, bir numarayla başlayan bir adı olan bir alt ağda dağıtılmaz. |
    | **Adres aralığı** | <p>Aralık, sanal ağ için adres alanı içinde benzersiz olmalıdır. Aralık, sanal ağdaki diğer alt ağ adres aralıklarıyla çakışamaz. Adres alanı Sınıfsız Etki Alanları Yönlendirme (CIDR) gösterimi kullanılarak belirtilmelidir.</p><p>Örneğin, adres alanı *10.0.0.0/16*olan bir sanal *ağda, 10.0.0.0/22'lik*bir alt net adres alanı tanımlayabilirsiniz. Belirtebileceğiniz en küçük aralık ,alt ağ için sekiz IP adresi sağlayan */29'dur.* Azure, protokol uygunluğu için her alt ağdaki ilk ve son adresi saklı tutar. Azure hizmet kullanımı için üç ek adres ayrılmıştır. Sonuç olarak, */29* adres aralığına sahip bir alt ağ tanımlamak, alt ağda kullanılabilir üç IP adresiyle sonuçlanır.</p><p>Sanal bir ağı VPN ağ geçidine bağlamayı planlıyorsanız, bir ağ geçidi alt ağı oluşturmanız gerekir. [Ağ geçidi alt ağları için belirli adres aralığı hususları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub)hakkında daha fazla bilgi edinin. Alt ağ eklendikten sonra, belirli koşullar altında adres aralığını değiştirebilirsiniz. Alt net adres aralığını nasıl değiştireceğinizi öğrenmek için [bkz.](#change-subnet-settings)</p> |
    | **Ağ güvenlik grubu** | Alt ağ için gelen ve giden ağ trafiğini filtrelemek için, varolan bir ağ güvenlik grubunu bir alt ağla ilişkilendirebilirsiniz. Ağ güvenlik grubu, sanal ağla aynı abonelikte ve konumda bulunmalıdır. [Ağ güvenlik grupları](security-overview.md) ve ağ [güvenlik grubu oluşturma](tutorial-filter-network-traffic.md)hakkında daha fazla bilgi edinin. |
    | **Rota tablosu** | Ağ trafiği yönlendirmesini diğer ağlara denetlemek için, varolan bir rota tablosunu isteğe bağlı olarak bir alt ağla ilişkilendirebilirsiniz. Rota tablosu, sanal ağla aynı abonelikte ve konumda bulunmalıdır. [Azure yönlendirmesi](virtual-networks-udr-overview.md) ve [rota tablosu oluşturma](tutorial-create-route-table-portal.md)hakkında daha fazla bilgi edinin. |
    | **Hizmet uç noktaları** | <p>Bir alt ağ isteğe bağlı olarak bir veya daha fazla hizmet uç noktası nın etkinleştirilmesine neden olabilir. Bir hizmet için hizmet bitiş noktasını etkinleştirmek için **Hizmetler** listesinden hizmet bitiş noktalarını etkinleştirmek istediğiniz hizmet veya hizmetleri seçin. Azure, bitiş noktası için konumu otomatik olarak yapılandırır. Varsayılan olarak, Azure sanal ağın bölgesi için hizmet bitiş noktalarını yapılandırır. Azure, bölgesel başarısız senaryoları desteklemek için, Azure Depolama için bitiş noktalarını otomatik olarak [Azure eşleştirilmiş bölgelere](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions) yapılandırır.</p><p>Bir hizmet bitiş noktasını kaldırmak için, hizmet bitiş noktasını kaldırmak istediğiniz hizmeti seçin. Hizmet bitiş noktaları ve etkinleştirilebilecekleri hizmetler hakkında daha fazla bilgi edinmek için [Sanal ağ hizmeti uç noktalarına](virtual-network-service-endpoints-overview.md)bakın. Bir hizmet için bir hizmet bitiş noktasını etkinleştirdikten sonra, hizmetle oluşturulan bir kaynak için alt ağ için ağ erişimini de etkinleştirmeniz gerekir. Örneğin, **Microsoft.Storage**için hizmet bitiş noktasını etkinleştiriyorsanız, ağ erişimi vermek istediğiniz tüm Azure Depolama hesaplarına ağ erişimini de etkinleştirmeniz gerekir. Hizmet bitiş noktasının etkinleştirilen alt ağlara ağ erişimini etkinleştirmek için, hizmet bitiş noktasını etkinleştirdiğiniz tek tek hizmetin belgelerine bakın.</p><p>Bir alt ağ için bir hizmet bitiş noktasının etkin olduğunu doğrulamak için, alt ağdaki herhangi bir ağ arabirimi için [etkili yolları](diagnose-network-routing-problem.md) görüntüleyin. Bir bitiş noktası yapılandırdığınızda, hizmetin adres önekleri ile *varsayılan* bir rota ve **VirtualNetworkServiceEndpoint**sonraki atlama türü bakın. Yönlendirme hakkında daha fazla bilgi edinmek için [Sanal ağ trafiği yönlendirmesine](virtual-networks-udr-overview.md)bakın.</p> |
    | **Alt ağ temsilcisi** | Bir alt ağ isteğe bağlı olarak bir veya daha fazla delegasyon için etkin olabilir. Alt ağ delegasyonu, hizmet dağıtımı sırasında benzersiz bir tanımlayıcı kullanarak alt ağda hizmete özgü kaynaklar oluşturmak için hizmete açık izinler verir. Bir hizmet için temsilci vermek **için, Hizmetler** listesinden devretmek istediğiniz hizmeti seçin. |

5. Alt ağı seçtiğiniz sanal ağa eklemek için **Tamam'ı**seçin.

### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) |
| PowerShell | [Ekle-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) |

## <a name="change-subnet-settings"></a>Alt ağ ayarlarını değiştirme

1. Sanal ağlarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. **Sanal ağları**arayın ve seçin.

2. Değiştirmek istediğiniz alt ağı içeren sanal ağın adını seçin.

3. **Ayarlar'dan** **Alt Ağlar'ı**seçin.

4. Alt ağlar listesinde, ayarları değiştirmek istediğiniz alt ağı seçin.

5. Alt ağ sayfasında, aşağıdaki ayarlardan herhangi birini değiştirin:

    | Ayar | Açıklama |
    | --- | --- |
    | **Adres aralığı** | Alt ağ içinde hiçbir kaynak dağıtılmazsa, adres aralığını değiştirebilirsiniz. Alt ağda herhangi bir kaynak varsa, kaynakları başka bir alt ağa taşımanız veya önce alt ağdan silmeniz gerekir. Bir kaynağı taşımak veya silmek için attığınız adımlar kaynağa bağlı olarak değişir. Alt ağlardaki kaynakları nasıl taşıyup silenleri öğrenmek için, bu kaynak türlerinin her biri için belgeleri okuyun. Alt ağ ekle'nin adım 4'ünde **Adres aralığıiçin** [kısıtlamalara](#add-a-subnet)bakın. |
    | **Kullanıcılar** | Yerleşik rolleri veya kendi özel rollerinizi kullanarak alt ağa erişimi denetleyebilirsiniz. Alt ağa erişmek için rol atama ve kullanıcı atama hakkında daha fazla bilgi edinmek için [bkz.](../role-based-access-control/role-assignments-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-role-assignment) |
    | **Ağ güvenlik grubu** ve **Yol tablosu** | [Bkz. alt ağ ekle'nin](#add-a-subnet)4. |
    | **Hizmet uç noktaları** | <p>[Alt ağ ekle'nin](#add-a-subnet)4. Varolan bir alt ağ için bir hizmet bitiş noktası etkinleştirirken, alt ağdaki herhangi bir kaynakta kritik görevlerin çalışmadığından emin olun. Servis uç noktaları alt ağdaki her ağ arabiriminde rotaları değiştirir. Hizmet bitiş noktaları *0.0.0.0/0* adresi öneki ve *internet*sonraki atlama türü ile varsayılan rota kullanarak gitmek , hizmetin adres önekleri ve *VirtualNetworkServiceEndpoint*sonraki atlama türü ile yeni bir rota kullanarak.</p><p>Geçiş sırasında, açık TCP bağlantıları sonlandırılabilir. Tüm ağ arabirimleri için hizmete trafik akışları yeni rota ile güncelleştirilene kadar hizmet bitiş noktası etkinleştirilir. Yönlendirme hakkında daha fazla bilgi edinmek için [Sanal ağ trafiği yönlendirmesine](virtual-networks-udr-overview.md)bakın.</p> |
    | **Alt ağ temsilcisi** | [Alt ağ ekle'nin](#add-a-subnet)4. Alt ağ delegasyonu sıfır veya bunun için etkin birden çok delegasyon olarak değiştirilebilir. Bir hizmet için kaynak zaten alt ağda dağıtılmışsa, hizmetin tüm kaynakları kaldırılıncaya kadar alt ağ delegasyonu eklenemez veya kaldırılamaz. Farklı bir hizmet için temsilci vermek **için, Hizmetler** listesinden devretmek istediğiniz hizmeti seçin. |

6. **Kaydet'i**seçin.

### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network vnet subnet update](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-update) |
| PowerShell | [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) |

## <a name="delete-a-subnet"></a>Bir alt ağı silme

Bir alt ağı yalnızca alt ağda kaynak yoksa silebilirsiniz. Kaynaklar alt ağdaysa, alt ağı silmeden önce bu kaynakları silmeniz gerekir. Bir kaynağı silmek için attığınız adımlar kaynağa bağlı olarak değişir. Alt ağlardaki kaynakları nasıl silerken öğrenince, bu kaynak türlerinin her biri için belgeleri okuyun.

1. Sanal ağlarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. **Sanal ağları**arayın ve seçin.

2. Silmek istediğiniz alt ağı içeren sanal ağın adını seçin.

3. **Ayarlar'dan** **Alt Ağlar'ı**seçin.

4. Alt ağlar listesinde, silmek istediğiniz alt ağı seçin.

5. **Sil'i**ve ardından onay iletişim kutusunda **Evet'i** seçin.

### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ vnet subnet silme](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-delete) |
| PowerShell | [Kaldır-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/remove-azvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json) |

## <a name="permissions"></a>İzinler

Alt ağlarda görev yapmak için, hesabınızın [Ağ katılımcısı rolüne](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) veya aşağıdaki tabloda uygun eylemleri atanmış özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir:

|Eylem                                                                   |   Adı                                       |
|-----------------------------------------------------------------------  |   -----------------------------------------  |
|Microsoft.Network/virtualNetworks/subnets/read                           |   Sanal ağ alt netini okuma              |
|Microsoft.Network/virtualNetworks/subnets/write                          |   Sanal ağ alt ağı oluşturma veya güncelleştirme  |
|Microsoft.Network/virtualNetworks/subnets/delete                         |   Sanal ağ alt ünü silme            |
|Microsoft.Network/virtualNetworks/subnets/join/action                    |   Sanal ağa katılma                     |
|Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action  |   Bir alt ağ için hizmet bitiş noktasını etkinleştirme     |
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read           |   Sanal makineleri bir alt ağda alın       |

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyalarını kullanarak veya Azure [Kaynak Yöneticisi şablonlarını](template-samples.md) kullanarak sanal ağ ve alt ağlar oluşturun
- Sanal ağlar için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama
