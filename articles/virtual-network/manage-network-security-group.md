---
title: Azure ağ güvenlik grubu oluşturma, değiştirme veya silme
titlesuffix: Azure Virtual Network
description: Ağ güvenlik grubu oluşturmayı, değiştirmeyi veya silmeyi öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/13/2020
ms.author: kumud
ms.openlocfilehash: a22adef5510e24c2dc07ffb39c9687d500644f8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066446"
---
# <a name="create-change-or-delete-a-network-security-group"></a>Ağ güvenlik grubu oluşturma, değiştirme veya silme

Ağ güvenlik gruplarındaki güvenlik kuralları, sanal ağ alt ağlarına ve ağ arabirimlerine girip çıkabilen ağ trafiği türünü filtrelemenize olanak tanır. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek için [Ağ güvenlik grubuna genel bakış'a](security-overview.md)bakın. Ardından, ağ güvenlik gruplarıyla ilgili deneyim kazanmak için [Filtre ağı trafiği](tutorial-filter-network-traffic.md) eğitimini tamamlayın.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Hesabınız yoksa, etkin bir abonelikle bir Azure hesabı ayarlayın. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) Bu makalenin geri kalanını başlatmadan önce bu görevlerden birini tamamlayın:

- **Portal kullanıcıları**: Azure hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.

- **PowerShell kullanıcıları**: Azure Bulut [Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Azure Bulut BulutU tarayıcı sekmesinde, çevre açılır **listesini seçin** ve daha önce seçilmemişse **PowerShell'i** seçin.

    PowerShell'i yerel olarak çalıştırıyorsanız, Azure PowerShell modül sürüm 1.0.0 veya sonrası kullanın. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az.Network` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.

- **Azure Komut satırı arabirimi (CLI) kullanıcıları**: [Azure Bulut BulutU'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Azure CLI sürümünü yerel olarak çalıştırıyorsanız Azure CLI sürüm 2.0.28 veya daha sonra kullanın. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure ile bağlantı oluşturmak için `az login` komutunu çalıştırın.

Oturum açtığınız veya Azure'a bağlandığınız hesabın [Ağ katılımcısı rolüne](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) veya [İzinler'de](#permissions)listelenen uygun eylemlere atanmış bir [Özel role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

## <a name="work-with-network-security-groups"></a>Ağ güvenlik grupları ile çalışma

Ağ güvenlik grubu oluşturabilir, [görüntüleyebilir,](#view-all-network-security-groups) [ayrıntıları görüntüleyebilir,](#view-details-of-a-network-security-group) [değiştirebilir](#change-a-network-security-group)ve [silebilirsiniz.](#delete-a-network-security-group) Ayrıca, bir ağ güvenlik grubunu ağ arabiriminden veya alt ağdan [ilişkilendirebilir](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface) veya ayırabilirsiniz.

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Her Azure konumu ve aboneliği için kaç ağ güvenlik grubu oluşturabileceğinizin bir sınırı vardır. Daha fazla bilgi edinmek için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bölümüne bakın.

1. Azure [portalı](https://portal.azure.com) menüsünde veya **Ana** sayfadan **kaynak oluştur'u**seçin.

2. **Ağ'ı**seçin, ardından **Ağ güvenlik grubunu**seçin.

3. **Temel ayarlar** sekmesi altında ağ güvenlik **grubu oluştur** sayfasında, aşağıdaki ayarlar için değerleri ayarlayın:

    | Ayar | Eylem |
    | --- | --- |
    | **Abonelik** | Aboneliğinizi seçin. |
    | **Kaynak grubu** | Varolan bir kaynak grubu seçin veya yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin. |
    | **Adı** | Kaynak grubuna benzersiz bir metin dizesi girin. |
    | **Bölge** | İstediğiniz konumu seçin. |

4. **İncele ve oluştur**’u seçin.

5. **Doğrulama geçti** iletisini gördünkten sonra **Oluştur'u**seçin.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network nsg create](/cli/azure/network/nsg#az-network-nsg-create) |
| PowerShell | [Yeni-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) |

### <a name="view-all-network-security-groups"></a>Tüm ağ güvenlik gruplarını görüntüleme

Ağ güvenlik gruplarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Ağ güvenlik **gruplarını**arayın ve seçin. Aboneliğiniz için ağ güvenlik grupları listesi görüntülenir.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ nsg listesi](/cli/azure/network/nsg#az-network-nsg-list) |
| PowerShell | [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) |

### <a name="view-details-of-a-network-security-group"></a>Ağ güvenlik grubunun ayrıntılarını görüntüleme

1. Ağ güvenlik gruplarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Ağ güvenlik **gruplarını**arayın ve seçin.

2. Ağ güvenlik grubunuzun adını seçin.

Ağ güvenlik grubunun menü çubuğunda, **Ayarlar**altında, **Gelen güvenlik kurallarını,** Giden **güvenlik kurallarını,** Ağ **arabirimlerini**ve ağ güvenlik grubunun ilişkili olduğu **Alt Ağları** görüntüleyebilirsiniz.

**İzleme**altında **Tanılama ayarlarını**etkinleştirebilir veya devre dışı kullanabilirsiniz. **Destek + sorun giderme** **altında, Etkili güvenlik kurallarını**görüntüleyebilirsiniz. Daha fazla bilgi edinmek [için, ağ güvenlik grubu için Tanılama günlüğe kaydetme](virtual-network-nsg-manage-log.md) ve [VM ağ trafiği filtresi sorununu tanıla.](diagnose-network-traffic-filter-problem.md)

Listelenen ortak Azure ayarları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Etkinlik günlüğü](../azure-monitor/platform/platform-logs-overview.md)
- [Erişim denetimi (IAM)](../role-based-access-control/overview.md)
- [Etiketler](../azure-resource-manager/management/tag-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Kilitler](../azure-resource-manager/management/lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Otomasyon betiği](../azure-resource-manager/templates/export-template-portal.md)

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ nsg göstermek](/cli/azure/network/nsg#az-network-nsg-show) |
| PowerShell | [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) |

### <a name="change-a-network-security-group"></a>Ağ güvenlik grubunu değiştirme

1. Ağ güvenlik gruplarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Ağ güvenlik **gruplarını**arayın ve seçin.

2. Değiştirmek istediğiniz ağ güvenlik grubunun adını seçin.

En yaygın değişiklikler [bir güvenlik kuralı eklemek,](#create-a-security-rule) [bir kuralı kaldırmak](#delete-a-security-rule)ve bir ağ güvenlik grubunu bir alt ağ veya ağ arabirimine veya bir ağ [arabirimine ilişkilendirmek veya ayırmaktır.](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface)

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ nsg güncelleme](/cli/azure/network/nsg#az-network-nsg-update) |
| PowerShell | [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup) |

### <a name="associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface"></a>Bir ağ güvenlik grubunu bir alt ağ veya ağ arabirimine ilişkilendirme veya ayırma

Bir ağ güvenlik grubunu ağ arabiriminden ilişkilendirmek veya ayırmak [için, bir ağ güvenlik grubunu ağ arabiriminden ilişkilendirme veya ayrıştırmak](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group)için bkz. Bir ağ güvenlik grubunu bir alt ağdan ilişkilendirmek veya ayırmak için [bkz.](virtual-network-manage-subnet.md#change-subnet-settings)

### <a name="delete-a-network-security-group"></a>Ağ güvenlik grubunu silme

Bir ağ güvenlik grubu herhangi bir alt ağ veya ağ arabirimleriyle ilişkilendirilirse, silinemez. Bir ağ güvenlik grubunu silmeye çalışmadan önce tüm alt ağlardan ve ağ arabirimlerinden ayırın.

1. Ağ güvenlik gruplarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Ağ güvenlik **gruplarını**arayın ve seçin.

2. Silmek istediğiniz ağ güvenlik grubunun adını seçin.

3. Ağ güvenlik grubunun araç çubuğunda **Sil'i**seçin. Ardından onay iletişim kutusunda **Evet'i** seçin.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ nsg silme](/cli/azure/network/nsg#az-network-nsg-delete) |
| PowerShell | [Remove-AzNetworkSecurityGroup](/powershell/module/az.network/remove-aznetworksecuritygroup) |

## <a name="work-with-security-rules"></a>Güvenlik kurallarıyla çalışma

Ağ güvenlik grubu sıfır veya daha fazla güvenlik kuralı içerir. Güvenlik kuralını oluşturabilir, [görüntüleyebilir, ayrıntıları](#view-all-security-rules) [görüntüleyebilir,](#view-details-of-a-security-rule) [değiştirebilir](#change-a-security-rule)ve [silebilirsiniz.](#delete-a-security-rule)

### <a name="create-a-security-rule"></a>Güvenlik kuralı oluşturma

Her Azure konumu ve aboneliği için ağ güvenlik grubu başına kaç kural oluşturabileceğinizin bir sınırı vardır. Daha fazla bilgi edinmek için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bölümüne bakın.

1. Ağ güvenlik gruplarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Ağ güvenlik **gruplarını**arayın ve seçin.

2. Güvenlik kuralı eklemek istediğiniz ağ güvenlik grubunun adını seçin.

3. Ağ güvenlik grubunun menü çubuğunda, Gelen güvenlik kuralları veya **Giden güvenlik kurallarını**seçin. **Inbound security rules**

    Bazılarını eklememiş olabilirsiniz dahil olmak üzere varolan birkaç kural listelenir. Bir ağ güvenlik grubu oluşturduğunuzda, içinde birkaç varsayılan güvenlik kuralı oluşturulur. Daha fazla bilgi edinmek için [varsayılan güvenlik kurallarına](security-overview.md#default-security-rules)bakın.  Varsayılan güvenlik kurallarını silemezsiniz, ancak bunları daha yüksek önceliğe sahip kurallarla geçersiz kılabilirsiniz.

4. <a name="security-rule-settings"></a>**Ekle'yi**seçin. Aşağıdaki ayarlar için değerleri seçin veya ekleyin ve sonra **Tamam'ı**seçin:

    | Ayar | Değer | Ayrıntılar |
    | ------- | ----- | ------- |
    | **Kaynak** | Biri:<ul><li>**Tümü**</li><li>**IP Adresleri**</li><li>**Hizmet Etiketi** (gelen güvenlik kuralı) veya **VirtualNetwork** (giden güvenlik kuralı)</li><li>**Uygulama&nbsp;&nbsp;güvenlik grubu**</li></ul> | <p>**IP Adresleri**seçerseniz, Kaynak **IP adreslerini/CIDR aralıklarını**da belirtmeniz gerekir.</p><p>**Hizmet Etiketi'ni**seçerseniz, **kaynak hizmet etiketi**de seçebilirsiniz.</p><p>**Uygulama güvenlik grubunu**seçerseniz, varolan bir uygulama güvenlik grubu da seçmeniz gerekir. **Hem Kaynak** hem de **Hedef**için Uygulama **güvenlik grubu** seçerseniz, her iki uygulama güvenlik grubundaki ağ arabirimleri aynı sanal ağda olmalıdır.</p> |
    | **Kaynak IP adresleri/CIDR aralıkları** | IP adreslerinin ve Sınıfsız Etki Alanı Yönlendirme (CIDR) aralıklarının virgülle sınırlandırılma listesi | <p>**Kaynak'ı** **IP Adresleri'ne**değiştirirseniz bu ayar görüntülenir. Birden çok değerden oluşan tek bir değer veya virgülle ayrılmış bir liste belirtmeniz gerekir. Birden çok değere `10.0.0.0/16, 192.188.1.1`örnek olarak . Belirtebileceğiniz değer sayısının sınırları vardır. Daha fazla ayrıntı için [Azure sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bakın.</p><p>Belirttiğiniz IP adresi bir Azure VM'ye atanmışsa, herkese açık IP adresini değil, özel IP adresini belirtin. Azure, genel IP adresini gelen güvenlik kuralları için özel bir IP adresine çevirdikten sonra güvenlik kurallarını işler, ancak giden kurallar için özel bir IP adresini genel bir IP adresine çevirmeden önce. Azure'daki genel ve özel IP adresleri hakkında daha fazla bilgi edinmek için [IP adresi türlerine](virtual-network-ip-addresses-overview-arm.md)bakın.</p> |
    | **Kaynak servis etiketi** | Açılır listesinden bir hizmet etiketi | Gelen güvenlik kuralı için **Kaynak'ı** **Hizmet Etiketine** ayarlarsanız, bu isteğe bağlı ayar görüntülenir. Hizmet etiketi, IP adresleri kategorisi için önceden tanımlanmış bir tanımlayıcıdır. Kullanılabilir hizmet etiketleri ve her etiketin temsil leri hakkında daha fazla bilgi edinmek için [Hizmet etiketlerine](security-overview.md#service-tags)bakın. |
    | **Kaynak uygulama güvenlik grubu** | Varolan bir uygulama güvenlik grubu | **Kaynak'ı** **Uygulama güvenlik grubuna**ayarlarsanız bu ayar görüntülenir. Ağ arabirimiyle aynı bölgede bulunan bir uygulama güvenlik grubu seçin. Uygulama [güvenlik grubu oluşturmayı](#create-an-application-security-group)öğrenin. |
    | **Kaynak bağlantı noktası aralıkları** | Biri:<ul><li>Tek bir bağlantı noktası, örneğin`80`</li><li>Gibi bir dizi bağlantı noktası`1024-65535`</li><li>Tek bağlantı noktaları ve/veya bağlantı noktası aralıklarının virgülden ayrılmış bir listesi, örneğin`80, 1024-65535`</li><li>Herhangi bir bağlantı`*`noktasında trafiğe izin veren bir yıldız işareti</li></ul> | Bu ayar, kuralın trafiğe izin verdiği veya reddettiği bağlantı noktalarını belirtir. Belirtebileceğiniz bağlantı noktası sayısının sınırları vardır. Daha fazla ayrıntı için [Azure sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bakın. |
    | **Hedef** | Biri:<ul><li>**Tümü**</li><li>**IP Adresleri**</li><li>**Hizmet Etiketi** (giden güvenlik kuralı) veya **VirtualNetwork** (gelen güvenlik kuralı)</li><li>**Uygulama&nbsp;&nbsp;güvenlik grubu**</li></ul> | <p>**IP adreslerini**seçerseniz, **Hedef IP adreslerini/CIDR aralıklarını**da belirtin.</p><p>**VirtualNetwork'u**seçerseniz, sanal ağın adres alanı içindeki tüm IP adreslerine trafik izni verilir. **VirtualNetwork** bir hizmet etiketidir.</p><p>**Uygulama güvenlik grubunu**seçerseniz, varolan bir uygulama güvenlik grubu seçmeniz gerekir. Uygulama [güvenlik grubu oluşturmayı](#create-an-application-security-group)öğrenin.</p> |
    | **Hedef IP adresleri/CIDR aralıkları** | IP adresleri nin ve CIDR aralıklarının virgülle sınırlandırılma listesi | <p>**Hedef'i** **IP Adresleriolarak**değiştirirseniz bu ayar görüntülenir. **Kaynak** ve **Kaynak IP adresleri/CIDR aralıklarına**benzer şekilde, tek veya birden çok adres veya aralık belirtebilirsiniz. Belirtebileceğiniz sayının sınırları vardır. Daha fazla ayrıntı için [Azure sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bakın.</p><p>Belirttiğiniz IP adresi bir Azure VM'ye atanmışsa, herkese açık IP adresini değil, özel IP adresini belirttiğinizi belirtin. Azure, genel IP adresini gelen güvenlik kuralları için özel bir IP adresine çevirdikten sonra güvenlik kurallarını işler, ancak Azure giden kurallar için özel bir IP adresini genel bir IP adresine çevirmeden önce. Azure'daki genel ve özel IP adresleri hakkında daha fazla bilgi edinmek için [IP adresi türlerine](virtual-network-ip-addresses-overview-arm.md)bakın.</p> |
    | **Hedef hizmet etiketi** | Açılır listesinden bir hizmet etiketi | Giden bir güvenlik kuralı için **Hedef'i** **Hizmet Etiketi** olarak değiştirirseniz, bu isteğe bağlı ayar görüntülenir. Hizmet etiketi, IP adresleri kategorisi için önceden tanımlanmış bir tanımlayıcıdır. Kullanılabilir hizmet etiketleri ve her etiketin temsil leri hakkında daha fazla bilgi edinmek için [Hizmet etiketlerine](security-overview.md#service-tags)bakın. |
    | **Hedef uygulama güvenlik grubu** | Varolan bir uygulama güvenlik grubu | **Hedef'i** **Uygulama güvenlik grubuna**ayarlarsanız bu ayar görüntülenir. Ağ arabirimiyle aynı bölgede bulunan bir uygulama güvenlik grubu seçin. Uygulama [güvenlik grubu oluşturmayı](#create-an-application-security-group)öğrenin. |
    | **Hedef bağlantı noktası aralıkları** | Biri:<ul><li>Tek bir bağlantı noktası, örneğin`80`</li><li>Gibi bir dizi bağlantı noktası`1024-65535`</li><li>Tek bağlantı noktaları ve/veya bağlantı noktası aralıklarının virgülden ayrılmış bir listesi, örneğin`80, 1024-65535`</li><li>Herhangi bir bağlantı`*`noktasında trafiğe izin veren bir yıldız işareti</li></ul> | Kaynak **bağlantı noktası aralıklarında**olduğu gibi, tek veya birden çok bağlantı noktası ve aralığı belirtebilirsiniz. Belirtebileceğiniz sayının sınırları vardır. Daha fazla ayrıntı için [Azure sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bakın. |
    | **Protokolü** | **Herhangi bir**, **TCP**, **UDP**veya **ICMP** | Kuralı Aktarım Denetim Protokolü (TCP), Kullanıcı Datagram Protokolü (UDP) veya Internet Control Message Protocol (ICMP) ile sınırlandırabilirsiniz. Varsayılan kural tüm protokoller için geçerlidir. |
    | **Eylem** | **İzin Ver** veya **Reddet** | Bu ayar, bu kuralın sağlanan kaynak ve hedef yapılandırmasına erişime izin verip vermeyeceğini belirtir. |
    | **Öncelik** | Ağ güvenlik grubundaki tüm güvenlik kuralları için benzersiz olan 100 ile 4096 arasındaki değer | Azure, güvenlik kurallarını öncelik sırasına göre işler. Sayı ne kadar düşükse, öncelik de o kadar yüksektir. 100, 200 ve 300 gibi kurallar oluştururken öncelikli sayılar arasında boşluk bırakmanızı öneririz. Boşluklar bırakmak, gelecekte kural eklemeyi kolaylaştırır, böylece onlara varolan kurallardan daha yüksek veya daha düşük öncelik verebilirsiniz. |
    | **Adı** | Ağ güvenlik grubu içindeki kural için benzersiz bir ad | Ad en fazla 80 karakter olabilir. Bir harf veya sayı ile başlamalı ve bir harf, sayı veya alt puan ile bitmelidir. Ad yalnızca harfler, sayılar, alt çizerler, dönemler veya tireler içerebilir. |
    | **Açıklama** | Metin açıklaması | Güvenlik kuralı için isteğe bağlı olarak bir metin açıklaması belirtebilirsiniz. |

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network nsg rule create](/cli/azure/network/nsg/rule#az-network-nsg-rule-create) |
| PowerShell | [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) |

### <a name="view-all-security-rules"></a>Tüm güvenlik kurallarını görüntüleme

Ağ güvenlik grubu sıfır veya daha fazla kural içerir. Kuralları görüntülerken listelenen bilgiler hakkında daha fazla bilgi edinmek için [Ağ güvenlik grubuna genel bakış'a](security-overview.md)bakın.

1. Ağ güvenlik grubunun kurallarını görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Ağ güvenlik **gruplarını**arayın ve seçin.

2. Kuralları görüntülemek istediğiniz ağ güvenlik grubunun adını seçin.

3. Ağ güvenlik grubunun menü çubuğunda, Gelen güvenlik kuralları veya **Giden güvenlik kurallarını**seçin. **Inbound security rules**

Liste, oluşturduğunuz tüm kuralları ve ağ güvenlik grubunun [varsayılan güvenlik kurallarını](security-overview.md#default-security-rules)içerir.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network nsg rule list](/cli/azure/network/nsg/rule#az-network-nsg-rule-list) |
| PowerShell | [Get-AzNetworkSecurityRuleConfig](/powershell/module/az.network/get-aznetworksecurityruleconfig) |

### <a name="view-details-of-a-security-rule"></a>Bir güvenlik kuralının ayrıntılarını görüntüleme

1. Ağ güvenlik grubunun kurallarını görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Ağ güvenlik **gruplarını**arayın ve seçin.

2. Bir kuralın ayrıntılarını görüntülemek istediğiniz ağ güvenlik grubunun adını seçin.

3. Ağ güvenlik grubunun menü çubuğunda, Gelen güvenlik kuralları veya **Giden güvenlik kurallarını**seçin. **Inbound security rules**

4. Ayrıntıları görüntülemek istediğiniz kuralı seçin. Tüm ayarların açıklaması için [Güvenlik kuralı ayarlarına](#security-rule-settings)bakın.

    > [!NOTE]
    > Bu yordam yalnızca özel güvenlik kuralı için geçerlidir. Varsayılan güvenlik kuralını seçerseniz çalışmaz.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ nsg kural göstermek](/cli/azure/network/nsg/rule#az-network-nsg-rule-show) |
| PowerShell | [Get-AzNetworkSecurityRuleConfig](/powershell/module/az.network/get-aznetworksecurityruleconfig) |

### <a name="change-a-security-rule"></a>Güvenlik kuralını değiştirme

1. [Güvenlik kuralının ayrıntılarını görüntüle'deki](#view-details-of-a-security-rule)adımları tamamlayın.

2. Ayarları gerektiği gibi değiştirin ve ardından **Kaydet'i**seçin. Tüm ayarların açıklaması için [Güvenlik kuralı ayarlarına](#security-rule-settings)bakın.

    > [!NOTE]
    > Bu yordam yalnızca özel güvenlik kuralı için geçerlidir. Varsayılan güvenlik kuralını değiştirmenize izin verilmez.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az network nsg rule update](/cli/azure/network/nsg/rule#az-network-nsg-rule-update) |
| PowerShell | [Set-AzNetworkSecurityRuleConfig](/powershell/module/az.network/set-aznetworksecurityruleconfig) |

### <a name="delete-a-security-rule"></a>Güvenlik kuralını silme

1. [Güvenlik kuralının ayrıntılarını görüntüle'deki](#view-details-of-a-security-rule)adımları tamamlayın.

2. **Sil**'i ve sonra da **Evet**'i seçin.

    > [!NOTE]
    > Bu yordam yalnızca özel güvenlik kuralı için geçerlidir. Varsayılan güvenlik kuralını silmenize izin verilmez.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ nsg kuralı silme](/cli/azure/network/nsg/rule#az-network-nsg-rule-delete) |
| PowerShell | [Kaldır-AzNetworkSecurityRuleConfig](/powershell/module/az.network/remove-aznetworksecurityruleconfig) |

## <a name="work-with-application-security-groups"></a>Uygulama güvenlik gruplarıyla çalışma

Uygulama güvenlik grubu sıfır veya daha fazla ağ arabirimi içerir. Daha fazla bilgi edinmek için [uygulama güvenlik gruplarına](security-overview.md#application-security-groups)bakın. Bir uygulama güvenlik grubundaki tüm ağ arabirimleri aynı sanal ağda bulunmalıdır. Bir uygulama güvenlik grubuna ağ arabirimi nasıl ekleyeceğinizi öğrenmek [için](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups)bkz.

### <a name="create-an-application-security-group"></a>Uygulama güvenlik grubu oluşturma

1. Azure [portalı](https://portal.azure.com) menüsünde veya **Ana** sayfadan **kaynak oluştur'u**seçin.

2. Arama kutusuna *Uygulama güvenlik grubunu*girin.

3. Uygulama **güvenlik grubu** sayfasında **Oluştur'u**seçin.

4. **Temel bilgiler** sekmesi altında bir uygulama güvenlik **grubu oluştur** sayfasında, aşağıdaki ayarlar için değerleri ayarlayın:

    | Ayar | Eylem |
    | --- | --- |
    | **Abonelik** | Aboneliğinizi seçin. |
    | **Kaynak grubu** | Varolan bir kaynak grubu seçin veya yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin. |
    | **Adı** | Kaynak grubuna benzersiz bir metin dizesi girin. |
    | **Bölge** | İstediğiniz konumu seçin. |

5. **İncele ve oluştur**’u seçin.

6. Gözden **Geçir + oluştur** sekmesinin altında, **Doğrulama geçti** iletisini gördünden sonra **Oluştur'u**seçin.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ asg oluşturmak](/cli/azure/network/asg#az-network-asg-create) |
| PowerShell | [Yeni-AzApplicationSecurityGroup](/powershell/module/az.network/new-azapplicationsecuritygroup) |

### <a name="view-all-application-security-groups"></a>Tüm uygulama güvenlik gruplarını görüntüleme

Uygulama güvenlik gruplarınızı görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Uygulama güvenlik **gruplarını**arayın ve seçin. Azure portalı, uygulama güvenlik gruplarınızın bir listesini görüntüler.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ asg listesi](/cli/azure/network/asg#az-network-asg-list) |
| PowerShell | [Get-AzApplicationSecurityGroup](/powershell/module/az.network/get-azapplicationsecuritygroup) |

### <a name="view-details-of-a-specific-application-security-group"></a>Belirli bir uygulama güvenlik grubunun ayrıntılarını görüntüleme

1. Uygulama güvenlik grubunu görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Uygulama güvenlik **gruplarını**arayın ve seçin.

2. Ayrıntılarını görüntülemek istediğiniz uygulama güvenlik grubunun adını seçin.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ asg göstermek](/cli/azure/network/asg#az-network-asg-show) |
| PowerShell | [Get-AzApplicationSecurityGroup](/powershell/module/az.network/get-azapplicationsecuritygroup) |

### <a name="change-an-application-security-group"></a>Uygulama güvenlik grubunu değiştirme

1. Uygulama güvenlik grubunu görüntülemek için [Azure portalına](https://portal.azure.com) gidin. Uygulama güvenlik **gruplarını**arayın ve seçin.

2. Değiştirmek istediğiniz uygulama güvenlik grubunun adını seçin.

3. Değiştirmek istediğiniz ayarın yanında **değiştir'i** seçin. Örneğin, **Etiketler**ekleyebilir veya kaldırabilirsiniz veya **Kaynak grubunu** veya **Aboneliği**değiştirebilirsiniz.

    > [!NOTE]
    > Konumu değiştiremezsin.

    Menü çubuğunda Access denetimi **(IAM)** seçeneğini de seçebilirsiniz. Access **denetimi (IAM)** sayfasında, uygulama güvenlik grubuna izinler atayabilir veya kaldırabilirsiniz.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ asg güncelleme](/cli/azure/network/asg#az-network-asg-update) |
| PowerShell | PowerShell cmdlet yok |

### <a name="delete-an-application-security-group"></a>Uygulama güvenlik grubunu silme

Herhangi bir ağ arabirimi içeriyorsa uygulama güvenlik grubunu silemezsiniz. Uygulama güvenlik grubundaki tüm ağ arabirimlerini kaldırmak için ağ arabirimi ayarlarını değiştirin veya ağ arabirimlerini silin. Daha fazla bilgi edinmek için uygulama [güvenlik gruplarına ekle veya uygulama güvenlik gruplarından kaldır](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups) veya [ağ arabirimini sil'e](virtual-network-network-interface.md#delete-a-network-interface)bakın.

1. Uygulama güvenlik gruplarınızı yönetmek için [Azure portalına](https://portal.azure.com) gidin. Uygulama güvenlik **gruplarını**arayın ve seçin.

2. Silmek istediğiniz uygulama güvenlik grubunun adını seçin.

3. **Sil'i**seçin ve ardından uygulama güvenlik grubunu silmek için **Evet'i** seçin.

#### <a name="commands"></a>Komutlar

| Araç | Komut |
| ---- | ------- |
| Azure CLI | [az ağ asg silme](/cli/azure/network/asg#az-network-asg-delete) |
| PowerShell | [Remove-AzApplicationSecurityGroup](/powershell/module/az.network/remove-azapplicationsecuritygroup) |

## <a name="permissions"></a>İzinler

Ağ güvenlik grupları, güvenlik kuralları ve uygulama güvenlik gruplarıyla ilgili görevleri yapmak için, hesabınızın [Ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya aşağıdaki tablolarda listelenen uygun izinlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir:

### <a name="network-security-group"></a>Ağ güvenlik grubu

| Eylem                                                        |   Adı                                                                |
|-------------------------------------------------------------- |   -------------------------------------------                         |
| Microsoft.Network/networkSecurityGroups/oku                  |   Ağ güvenlik grubunu alın                                          |
| Microsoft.Network/networkSecurityGroups/write                 |   Ağ güvenlik grubu oluşturma veya güncelleştirme                             |
| Microsoft.Network/networkSecurityGroups/delete                |   Ağ güvenlik grubunu silme                                       |
| Microsoft.Network/networkSecurityGroups/join/action           |   Ağ güvenlik grubunu bir alt ağ veya ağ arabirimiyle ilişkilendirme 

### <a name="network-security-group-rule"></a>Ağ güvenliği grubu kuralı

| Eylem                                                        |   Adı                                                                |
|-------------------------------------------------------------- |   -------------------------------------------                         |
| Microsoft.Network/networkSecurityGroups/rules/read            |   Kuralı alın                                                            |
| Microsoft.Network/networkSecurityGroups/rules/write           |   Oluşturma veya güncelleştirme kuralı                                               |
| Microsoft.Network/networkSecurityGroups/rules/delete          |   Kuralı silme                                                         |

### <a name="application-security-group"></a>Uygulama güvenlik grubu

| Eylem                                                                     | Adı                                                     |
| --------------------------------------------------------------             | -------------------------------------------              |
| Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action     | Bir UYGULAMA güvenlik grubuna IP yapılandırmasına katılma|
| Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Uygulama güvenlik grubuna güvenlik kuralını katılma    |
| Microsoft.Network/applicationSecurityGroups/oku                           | Uygulama güvenlik grubu alma                        |
| Microsoft.Network/applicationSecurityGroups/write                          | Uygulama güvenlik grubu oluşturma veya güncelleştirme           |
| Microsoft.Network/applicationSecurityGroups/delete                         | Uygulama güvenlik grubunu silme                     |

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyalarını veya Azure [Kaynak Yöneticisi şablonlarını](template-samples.md) kullanarak ağ veya uygulama güvenlik grubu oluşturun
- Sanal ağlar için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama
