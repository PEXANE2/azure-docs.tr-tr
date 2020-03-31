---
title: Azure ağ arabirimi için IP adreslerini yapılandırma | Microsoft Dokümanlar
description: Ağ arabirimi için özel ve genel IP adreslerini nasıl ekleyeceğinizi, değiştireceğinizi ve kaldırmayı öğrenin.
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
ms.date: 01/22/2020
ms.author: kumud
ms.openlocfilehash: a2a85d98bf29e78d58bf0c578ce79943bae21fc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244972"
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Azure ağ arabirimleri için IP adresi ekleme, değiştirme veya kaldırma

Ağ arabirimi için genel ve özel IP adreslerini nasıl ekleyeceğinizi, değiştireceğinizi ve kaldırmayı öğrenin. Ağ arabirimine atanan özel IP adresleri, sanal bir makinenin Azure sanal ağındaki diğer kaynaklarla ve bağlı ağlarla iletişim kurmasını sağlar. Özel bir IP adresi de öngörülemeyen bir IP adresi kullanarak Internet'e giden iletişim sağlar. Ağ arabirimine atanan [genel IP adresi,](virtual-network-public-ip-address.md) Internet'ten sanal makineye gelen iletişimi sağlar. Adres ayrıca öngörülebilir bir IP adresi kullanarak sanal makineden Internet'e giden iletişimi de sağlar. Ayrıntılar için [Azure'da giden bağlantıları anlama 'ya](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

Bir ağ arabirimi oluşturmanız, değiştirmeniz veya silmeniz gerekiyorsa, [ağ arabirimini yönet](virtual-network-network-interface.md) makalesini okuyun. Sanal bir makineden ağ arabirimlerine ağ arabirimleri eklemeniz veya kaldırmanız gerekiyorsa, [Ağ arabirimleri ekle](virtual-network-network-interface-vm.md) makalesini okuyun veya kaldırın.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin herhangi bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, ücretsiz deneme [hesabı](https://azure.microsoft.com/free)için kaydolun.
- Portalı kullanıyorsanız, https://portal.azure.comAzure hesabınızla açın ve oturum açın.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanıyorsanız, [Azure Bulut Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure Komut satırı arabirimi (CLI) komutlarını kullanıyorsanız, [komutları Azure Bulut Kabuğu'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Bu öğretici, Azure CLI sürüm 2.0.31 veya sonraki sürüm gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` Azure ile bağlantı oluşturmak için de çalıştırmanız gerekir.

Oturum açtığınız veya Azure'a bağlandığınız hesabın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya [Ağ arabirimi izinlerinde](virtual-network-network-interface.md#permissions)listelenen uygun eylemlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

## <a name="add-ip-addresses"></a>IP adresleri ekleme

[Azure sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesinde listelenen sınırlar içinde, ağ arabirimine gerektiği kadar [özel](#private) ve [genel](#public) [IPv4](#ipv4) adresi ekleyebilirsiniz. Varolan bir ağ arabirimi için bir [ikincil IP yapılandırmasına](#secondary) (varolan ikincil IP yapılandırmaları olmadığı sürece) özel bir IPv6 adresi ekleyebilirsiniz. Her ağ arabiriminde en fazla bir IPv6 özel adresi olabilir. İsteğe bağlı olarak bir IPv6 ağ arabirimi yapılandırmasına genel bir IPv6 adresi ekleyebilirsiniz. IPv6 adreslerini kullanma hakkında ayrıntılı bilgi için [IPv6'ya](#ipv6) bakın.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Listeden iPv4 adresi eklemek istediğiniz ağ arabirimini seçin.
3. **AYARLAR**altında **IP yapılandırmalarını**seçin.
4. **IP yapılandırmaları**altında , seçin **+ Ekle**.
5. Aşağıdakileri belirtin, sonra **Tamam'ı**seçin:

   |Ayar|Gerekli mi?|Ayrıntılar|
   |---|---|---|
   |Adı|Evet|Ağ arabirimi için benzersiz olmalıdır|
   |Tür|Evet|Varolan bir ağ arabirimine BIR IP yapılandırması ekliyorsanız ve her ağ arabiriminin [birincil](#primary) IP yapılandırması olması gerektiğinden, tek seçeneğiniz **İkincil'dir.**|
   |Özel IP adresi atama yöntemi|Evet|[**Dinamik**](#dynamic): Azure, ağ arabiriminin dağıtılan alt net adresi aralığının bir sonraki kullanılabilir adresini atar. [**Statik**](#static): Ağ arabiriminin dağıtılan alt net adresi aralığı için kullanılmayan bir adres atarsınız.|
   |Genel IP adresi|Hayır|**Devre dışı bırakılmış:** Şu anda IP yapılandırması ile ilişkili ortak IP adresi kaynağı bulunmamaktadır. **Etkinleştirilmiş:** Varolan bir IPv4 Genel IP adresi seçin veya yeni bir tane oluşturun. Genel BIR IP adresi oluşturmayı öğrenmek için [Genel IP adresleri](virtual-network-public-ip-address.md#create-a-public-ip-address) makalesini okuyun.|
6. Sanal makine işletim sistemleri makalesine birden fazla IP adresi atamatalimatını tamamlayarak sanal makine işletim sistemine otomatik özel IP [adreslerini](virtual-network-multiple-ip-addresses-portal.md#os-config) el ile ekleyin. Sanal makine işletim sistemine IP adreslerini el ile eklemeden önce özel hususlar için [özel](#private) IP adreslerine bakın. Sanal makine işletim sistemine ortak IP adresleri eklemeyin.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az network nic ip-config create](/cli/azure/network/nic/ip-config)|
|PowerShell|[Ekle-AzNetworkInterfaceIpConfig](/powershell/module/az.network/add-aznetworkinterfaceipconfig)|

## <a name="change-ip-address-settings"></a>IP adresi ayarlarını değiştirme

Bir IPv4 adresinin atama yöntemini değiştirmeniz, statik IPv4 adresini değiştirmeniz veya ağ arabirimine atanan genel IP adresini değiştirmeniz gerekebilir. Sanal bir makinedeki ikincil ağ arabirimiyle ilişkili ikincil bir IP yapılandırmasının özel IPv4 adresini değiştiriyorsanız [(birincil ve ikincil ağ arabirimleri](virtual-network-network-interface-vm.md)hakkında daha fazla bilgi edinin), aşağıdaki adımları tamamlamadan önce sanal makineyi durdurulan (ayrılmış) duruma yerleştirin:

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Listeden IP adresi ayarlarını görüntülemek veya değiştirmek istediğiniz ağ arabirimini seçin.
3. **AYARLAR**altında **IP yapılandırmalarını**seçin.
4. Listeden değiştirmek istediğiniz IP yapılandırmasını seçin.
5. [IP yapılandırması ekle'nin](#add-ip-addresses)5.
6. **Kaydet'i**seçin.

>[!NOTE]
>Birincil ağ arabiriminde birden çok IP yapılandırması varsa ve birincil IP yapılandırmasının özel IP adresini değiştirirseniz, birincil ve ikincil IP adreslerini Windows içindeki ağ arabirimine el ile yeniden atamanız gerekir (Linux için gerekli değildir) . Bir işletim sistemi içindeki bir ağ arabirimine IP adreslerini el ile atamak [için](virtual-network-multiple-ip-addresses-portal.md#os-config)bkz. Sanal makine işletim sistemine IP adreslerini el ile eklemeden önce özel hususlar için [özel](#private) IP adreslerine bakın. Sanal makine işletim sistemine ortak IP adresleri eklemeyin.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ nic ip-config güncelleme](/cli/azure/network/nic/ip-config)|
|PowerShell|[Set-AzNetworkInterfaceIpConfig](/powershell/module/az.network/set-aznetworkinterfaceipconfig)|

## <a name="remove-ip-addresses"></a>IP adreslerini kaldırma

[Özel](#private) ve [genel](#public) IP adreslerini ağ arabiriminden kaldırabilirsiniz, ancak ağ arabiriminde her zaman en az bir özel IPv4 adresi atanmış olmalıdır.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. IP adreslerini listeden kaldırmak istediğiniz ağ arabirimini seçin.
3. **AYARLAR**altında **IP yapılandırmalarını**seçin.
4. Silmek istediğiniz [ikincil](#secondary) bir IP yapılandırmasını [(birincil](#primary) yapılandırmayı silemezsiniz) sağ seçin, silme işlemini onaylamak için **Sil'i**seçin, ardından **Evet'i**seçin. Yapılandırmada bununla ilişkili ortak bir IP adresi kaynağı varsa, kaynak IP yapılandırmasından ayrıştırılır, ancak kaynak silinmez.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ nic ip-config silme](/cli/azure/network/nic/ip-config)|
|PowerShell|[Kaldır-AzNetworkInterfaceIpConfig](/powershell/module/az.network/remove-aznetworkinterfaceipconfig)|

## <a name="ip-configurations"></a>IP yapılandırmaları

[Özel](#private) ve (isteğe bağlı olarak) [genel](#public) IP adresleri, ağ arabirimine atanan bir veya daha fazla IP yapılandırmasına atanır. İki tür IP yapılandırması vardır:

### <a name="primary"></a>Birincil

Her ağ arabirimine bir birincil IP yapılandırması atanır. Birincil IP yapılandırması:

- [Özel](#private) bir [IPv4](#ipv4) adresi atanmış. Birincil IP yapılandırmasına özel bir [IPv6](#ipv6) adresi atamazsınız.
- Ayrıca, kendisine atanmış [genel](#public) bir IPv4 adresi de olabilir. Birincil (IPv4) IP yapılandırmasına ortak bir IPv6 adresi atamazsınız. 

### <a name="secondary"></a>İkincil

Birincil IP yapılandırmasına ek olarak, bir ağ arabiriminde sıfır veya daha fazla ikincil IP yapılandırması atanmış olabilir. İkincil bir IP yapılandırması:

- Özel bir IPv4 veya IPv6 adresi atanmış olmalıdır. Adres IPv6 ise, ağ arabiriminde yalnızca bir ikincil IP yapılandırması olabilir. Adres IPv4 ise, ağ arabiriminde birden çok ikincil IP yapılandırması atanmış olabilir. Bir ağ arabirimine kaç özel ve genel IPv4 adresi atanabileceği hakkında daha fazla bilgi edinmek için [Azure sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesine bakın.
- Ayrıca, genel bir IPv4 veya IPv6 adresi de atanmış olabilir. Bir ağ arabirimine birden çok IPv4 adresi atamak aşağıdaki gibi senaryolarda yararlıdır:
  - Tek bir sunucuda farklı IP adreslerine ve SSL sertifikalarına sahip birden fazla web sitesi veya hizmetin barındırılması.
  - Güvenlik duvarı veya yük dengeleyicisi gibi ağ sanal cihazı olarak hizmet veren sanal bir makine.
  - Herhangi bir ağ arabirimi için özel IPv4 adreslerinden herhangi birini Azure Yük Dengeleyici arka uç havuzuna ekleme özelliği. Geçmişte, yalnızca birincil ağ arabirimi için birincil IPv4 adresi bir arka uç havuzuna eklenebilir. Birden çok IPv4 yapılandırmasını nasıl yükleyeceğimiz hakkında daha fazla bilgi edinmek [için, birden çok IP yapılandırmasını dengeleme](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) konusuna bakın. 
  - Bir ağ arabirimine atanan bir IPv6 adresini bakiyeyükleme yeteneği. Özel bir IPv6 adresine bakiye yükleme hakkında daha fazla bilgi edinmek için [Yük bakiyesi IPv6 adresleri](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesine bakın.

## <a name="address-types"></a>Adres türleri

Aşağıdaki IP adresleri türlerini bir IP [yapılandırmasına](#ip-configurations)atayabilirsiniz:

### <a name="private"></a>Özel

Özel [IPv4](#ipv4) veya IPv6 adresleri, sanal bir makinenin sanal ağdaki veya diğer bağlı ağlardaki diğer kaynaklarla iletişim kurmasını sağlar. 

Varsayılan olarak, Azure DHCP sunucuları Azure ağ arabiriminin [birincil IP yapılandırması](#primary) için özel IPv4 adresini sanal makine işletim sistemi içindeki ağ arabirimine atar. Gerekli olmadıkça, sanal makinenin işletim sistemi içinde bir ağ arabiriminin IP adresini el ile ayarlamamalısınız.

> [!WARNING]
> Sanal makinenin işletim sistemindeki bir ağ arabiriminin birincil IP adresi olarak ayarlanan IPv4 adresi, sanal bir makineye bağlı birincil ağ arabiriminin birincil IP yapılandırmasına atanan özel IPv4 adresinden hiç farklıysa Azure'da sanal makineye bağlantınızı kaybedersiniz.

Sanal makinenin işletim sisteminde bir ağ arabiriminin IP adresini el ile ayarlamanın gerekli olduğu senaryolar vardır. Örneğin, bir Azure sanal makinesine birden çok IP adresi eklerken bir Windows işletim sisteminin birincil ve ikincil IP adreslerini el ile ayarlamanız gerekir. Bir Linux sanal makinesi için yalnızca ikincil IP adreslerini el ile ayarlamanız gerekebilir. Ayrıntılar için [BIR VM işletim sistemine IP adresleri ekle'ye](virtual-network-multiple-ip-addresses-portal.md#os-config) bakın. IP yapılandırmasına atanan adresi değiştirmeniz gerekirse, aşağıdakileri yapmanız önerilir:

1. Sanal makinenin Azure DHCP sunucularından bir adres aldığından emin olun. Bunu yaptıktan sonra, IP adresinin atamasını işletim sistemi içinde DHCP olarak değiştirin ve sanal makineyi yeniden başlatın.
2. Sanal makineyi durdurun (anlaşma yeri) durdurun.
3. Azure içindeki IP yapılandırmasının IP adresini değiştirin.
4. Sanal makineyi başlatın.
5. İşletim sistemi içindeki ikincil IP adreslerini (ve windows içindeki birincil IP adresini) Azure'da ayarladığınız la eşleşecek şekilde [el ile yapılandırın.](virtual-network-multiple-ip-addresses-portal.md#os-config)

Önceki adımları izleyerek, Azure içindeki ağ arabirimine ve sanal makinenin işletim sisteminde atanan özel IP adresi aynı kalır. Aboneliğinizde, bir işletim sistemi içinde IP adreslerini el ile ayarladığınız sanal makineleri izlemek için, sanal makinelere bir Azure [etiketi](../azure-resource-manager/management/tag-resources.md) eklemeyi düşünün. Örneğin "IP adresi atama: Statik" kullanabilirsiniz. Bu şekilde, aboneliğinizdeki IP adresini işletim sistemi için el ile ayarladığınız sanal makineleri kolayca bulabilirsiniz.

Özel bir IP adresi, sanal bir makinenin aynı veya bağlı sanal ağlar içindeki diğer kaynaklarla iletişim kurmasını sağlamanın yanı sıra, sanal bir makinenin Internet'e giden iletişim kurmasını da sağlar. Giden bağlantılar, Azure tarafından öngörülemeyen genel bir IP adresine çevrilmiş kaynak ağ adresidir. Azure giden Internet bağlantısı hakkında daha fazla bilgi edinmek için [Azure giden Internet bağlantısı](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesini okuyun. Internet'ten sanal makinenin özel IP adresine gelen iletişim kuramazsınız. Giden bağlantılarınız öngörülebilir bir genel IP adresi gerektiriyorsa, ortak ip adresi kaynağını ağ arabirimiyle ilişkilendirin.

### <a name="public"></a>Genel

Genel IP adresi kaynağı üzerinden atanan genel IP adresleri, Internet'ten sanal bir makineye gelen bağlantıyı sağlar. Internet'e giden bağlantılar öngörülebilir bir IP adresi kullanır. Ayrıntılar için [Azure'da giden bağlantıları anlama](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bilgisine bakın. Bir IP yapılandırmasına genel bir IP adresi atayabilirsiniz, ancak atamanız gerekmez. Genel bir IP adresi kaynağını ilişkilendirerek sanal bir makineye genel bir IP adresi atadıysanız, sanal makine internete giden iletişim kurmaya devam edebilir. Bu durumda, özel IP adresi, Azure tarafından öngörülemeyen bir genel IP adresine çevrilmiş kaynak ağ adresidir. Genel IP adresi kaynakları hakkında daha fazla bilgi edinmek için [Genel IP adresi kaynağına](virtual-network-public-ip-address.md)bakın.

Ağ arabirimine atayabileceğiniz özel ve genel IP adreslerinin sayısının sınırları vardır. Ayrıntılar için [Azure sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini okuyun.

> [!NOTE]
> Azure, sanal makinenin özel IP adresini herkese açık bir IP adresine çevirir. Sonuç olarak, sanal bir makinenin işletim sistemi kendisine atanan herhangi bir genel IP adresinden habersizdir, bu nedenle işletim sistemi içinde genel bir IP adresi nin el ile atanmasına gerek yoktur.

## <a name="assignment-methods"></a>Atama yöntemleri

Genel ve özel IP adresleri aşağıdaki atama yöntemlerinden biri kullanılarak atanır:

### <a name="dynamic"></a>Dinamik

Dinamik özel IPv4 ve IPv6 (isteğe bağlı) adresleri varsayılan olarak atanır.

- **Yalnızca herkese açık**: Azure adresi her Azure bölgesine özgü bir aralıktan atar. Her bölgeye hangi aralıkların atandığını öğrenmek için [Bkz. Microsoft Azure Veri Merkezi IP Aralıkları.](https://www.microsoft.com/download/details.aspx?id=41653) Sanal bir makine durdurulduğunda (devre dışı bırakıldığında) adres değişebilir, sonra yeniden başlatılabilir. Her iki atama yöntemini kullanarak bir IP yapılandırmasına genel bir IPv6 adresi atamazsınız.
- **Yalnızca Özel**: Azure, her alt net adres aralığındaki ilk dört adresi saklı tutar ve adresleri atamaz. Azure, alt ağ adres aralığından sonraki kullanılabilir adresi bir kaynağa atar. Örneğin, alt ağ adres aralığının 10.0.0.0/16 olduğunu varsayarsak ve 10.0.0.0.4-10.0.0.14 adresleri zaten atanmışsa (.0-.3 ayrılmıştır), Azure kaynağa 10.0.0.15 adresini atar. Dinamik, varsayılan ayırma yöntemidir. Dinamik IP adresleri bir kez atandıktan sonra, ancak ağ arabirimi silinirse, aynı sanal ağ içinde farklı bir alt ağa atanırsa veya ayırma yöntemi statik olarak değiştirilip farklı bir IP adresi belirtilirse serbest bırakılır. Varsayılan olarak, dinamik olan ayırma yöntemini statik olarak değiştirdiğinizde Azure dinamik olarak atanmış önceki adresi statik adres olarak atar. 

### <a name="static"></a>Statik

Bir IP yapılandırmasına ortak veya özel statik IPv4 veya IPv6 adresi atayabilirsiniz (isteğe bağlı olarak). Azure'un statik genel IPv4 adreslerini nasıl atadığı hakkında daha fazla bilgi edinmek için [genel IP adreslerine](virtual-network-public-ip-address.md)bakın.

- **Yalnızca herkese açık**: Azure adresi her Azure bölgesine özgü bir aralıktan atar. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz. Atandığı genel IP adresi kaynağı silinene veya atama yöntemi dinamik olarak değiştirilene kadar adres değişmez. Genel IP adresi kaynağı bir IP yapılandırması ile ilişkiliyse, atama yöntemini değiştirmeden önce IP yapılandırmasından ayrıştırılmalıdır.
- **Yalnızca Özel**: Alt netin adres aralığından bir adres seçip atarsınız. Alt ağ adres aralığından atadığınız adres, alt ağa adres aralığının ilk dört adresi dışında kalan ve şu anda alt ağda başka bir kaynağa atanmamış olan herhangi bir adres olabilir. Statik adresler ancak ağ arabirimi silindiğinde serbest bırakılır. Ayırma yöntemini statik olarak değiştirirseniz, Azure, alt netin adres aralığındaki bir sonraki kullanılabilir adres olmasa bile, önceden atanmış dinamik IP adresini statik adres olarak dinamik olarak atar. Ağ arabirimi aynı sanal ağ içinde farklı bir alt ağa atandığında da adres değişir; ama ağ arabirimini farklı bir alt ağa atamak için, önce statik ayırma yöntemini dinamik olarak değiştirmeniz gerekir. Ağ arabirimini farklı bir alt ağa atadıktan sonra, ayırma yöntemini yine statik yapabilir ve yeni alt ağ adres aralığından bir IP adresi atayabilirsiniz.

## <a name="ip-address-versions"></a>IP adresi sürümleri

Adres atarken aşağıdaki sürümleri belirtebilirsiniz:

### <a name="ipv4"></a>IPv4

Her ağ arabiriminde atanmış özel [private](#private) [Bir IPv4](#ipv4) adresiolan bir [birincil](#primary) IP yapılandırması olmalıdır. Her biri IPv4 özel ve (isteğe bağlı olarak) bir IPv4 [genel](#public) IP adresi ne sahip bir veya daha fazla [ikincil](#secondary) IP yapılandırmaları ekleyebilirsiniz.

### <a name="ipv6"></a>IPv6

Bir ağ arabiriminin ikincil IP yapılandırmasına sıfır veya bir özel [IPv6](#ipv6) adresi atayabilirsiniz. Ağ arabiriminde varolan ikincil IP yapılandırmaları olamaz. Her ağ arabiriminde en fazla bir IPv6 özel adresi olabilir. İsteğe bağlı olarak bir IPv6 ağ arabirimi yapılandırmasına genel bir IPv6 adresi ekleyebilirsiniz.

> [!NOTE]
> Portalı kullanarak IPv6 adresine sahip bir ağ arabirimi oluşturabiliyor olsanız da, portalı kullanarak yeni veya varolan sanal makineye varolan bir ağ arabirimi ekemezsiniz. Özel bir IPv6 adresine sahip bir ağ arabirimi oluşturmak için PowerShell veya Azure CLI'yi kullanın ve ardından sanal bir makine oluştururken ağ arabirimini takın. Varolan bir sanal makineye atanmış özel bir IPv6 adresi olan bir ağ arabirimi takamazsınız. Herhangi bir araç (portal, CLI veya PowerShell) kullanarak sanal bir makineye bağlı herhangi bir ağ arabirimi için bir IP yapılandırmasına özel bir IPv6 adresi ekleyemezsiniz.

Birincil veya ikincil IP yapılandırmasına genel bir IPv6 adresi atayamazsınız.

## <a name="skus"></a>SKU'lar

Temel veya standart SKU ile ortak bir IP adresi oluşturulur. SKU farklılıkları hakkında daha fazla bilgi için genel [IP adreslerini yönet'e](virtual-network-public-ip-address.md)bakın.

> [!NOTE]
> Standart bir SKU genel IP adresini bir sanal makinenin ağ arabirimine atadığınızda amaçlanan trafiğe bir [ağ güvenlik grubuyla](security-overview.md#network-security-groups) açıkça izin vermeniz gerekir. Bir ağ güvenlik grubu oluşturup ilişkilendirene ve istenen trafiğe açıkça izin verene kadar kaynakla erişim kurma girişimleri başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar
Farklı IP yapılandırmalarına sahip sanal bir makine oluşturmak için aşağıdaki makaleleri okuyun:

|Görev|Araç|
|---|---|
|Birden çok ağ arabirimi ile VM oluşturma|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Birden çok IPv4 adresi olan tek bir NIC VM oluşturma|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Özel IPv6 adresi olan tek bir NIC VM oluşturun (Azure Yük Bakiyeleyicisi'nin arkasında)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Kaynak Yöneticisi şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
