---
title: Azure ağ arabirimi oluşturma, değiştirme veya silme
titlesuffix: Azure Virtual Network
description: Ağ arabiriminin ne olduğunu ve nasıl oluşturulup ayarları değiştireceğinizi ve bir arabiriminnasıl değiştireceğinizi öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/22/2020
ms.author: kumud
ms.openlocfilehash: 11e6285ef70ffde5344add951801997f8541eaad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244946"
---
# <a name="create-change-or-delete-a-network-interface"></a>Ağ arabirimi oluşturma, değiştirme veya silme

Ağ arabirimi oluşturma, ayarları değiştirme ve silme yi öğrenin. Ağ arabirimi, Azure Sanal Makinesi'nin Internet, Azure ve şirket içi kaynaklarla iletişim kurmasını sağlar. Azure portalını kullanarak sanal bir makine oluştururken, portal sizin için varsayılan ayarlara sahip bir ağ arabirimi oluşturur. Bunun yerine özel ayarlara sahip ağ arabirimleri oluşturmayı ve oluşturduğunuzda sanal bir makineye bir veya daha fazla ağ arabirimi eklemeyi seçebilirsiniz. Ayrıca, varolan bir ağ arabirimi için varsayılan ağ arabirimi ayarlarını değiştirmek isteyebilirsiniz. Bu makalede, özel ayarlara sahip bir ağ arabirimi oluşturma, ağ filtresi (ağ güvenlik grubu) ataması, alt ağ ataması, DNS sunucu ayarları ve IP iletme gibi varolan ayarları nasıl değiştireceği ve ağ arabirimini silme yi açıklamaktadır.

Ağ arabirimi için IP adresleri eklemeniz, değiştirmeniz veya kaldırmanız gerekiyorsa, [ip adreslerini yönet'e](virtual-network-network-interface-addresses.md)bakın. Sanal makinelerden ağ arabirimlerine ağ arabirimleri eklemeniz veya kaldırmanız gerekiyorsa, [bkz.](virtual-network-network-interface-vm.md)

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin herhangi bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, ücretsiz deneme [hesabı](https://azure.microsoft.com/free)için kaydolun.
- Portalı kullanıyorsanız, https://portal.azure.comAzure hesabınızla açın ve oturum açın.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanıyorsanız, [Azure Bulut Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure Komut satırı arabirimi (CLI) komutlarını kullanıyorsanız, [komutları Azure Bulut Kabuğu'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Bu öğretici, Azure CLI sürümü 2.0.28 veya daha sonra gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` Azure ile bağlantı oluşturmak için de çalıştırmanız gerekir.

Oturum açtığınız veya Azure'a bağlandığınız hesabın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya [İzinler'de](#permissions)listelenen uygun eylemlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

## <a name="create-a-network-interface"></a>Ağ arabirimi oluşturma

Azure portalını kullanarak sanal bir makine oluştururken, portal sizin için varsayılan ayarlara sahip bir ağ arabirimi oluşturur. Tüm ağ arabirimi ayarlarınızı belirtmek isterseniz, özel ayarlara sahip bir ağ arabirimi oluşturabilir ve sanal makineoluştururken (PowerShell veya Azure CLI kullanarak) ağ arabirimini sanal bir makineye ekleyebilirsiniz. Ayrıca bir ağ arabirimi oluşturabilir ve varolan bir sanal makineye (PowerShell veya Azure CLI kullanarak) ekleyebilirsiniz. Varolan bir ağ arabirimine sahip sanal bir makinenin nasıl oluşturularak oluşturulmasını veya varolan sanal makinelerden ağ arabirimlerine nasıl ekleyeceğini veya kaldırdığını öğrenmek için [bkz.](virtual-network-network-interface-vm.md) Bir ağ arabirimi oluşturmadan önce, içinde ağ arabirimi oluşturduğunuz aynı konumda ve abonelikte varolan bir [sanal ağa](manage-virtual-network.md) sahip olmalısınız.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Select + **Ağ arabirimleri**altında **ekle.**
3. Aşağıdaki ayarlar için değerleri girin veya seçin, ardından **Oluştur'u**seçin:

    |Ayar|Gerekli mi?|Ayrıntılar|
    |---|---|---|
    |Adı|Evet|Ad seçtiğiniz kaynak grubunda benzersiz olmalıdır. Zamanla Azure aboneliğinizde büyük olasılıkla birkaç ağ arabirimi olur. Birkaç ağ arabirimini yönetmeyi kolaylaştırmak için bir adlandırma kuralı oluştururken öneriler için [Bkz. Adlandırma kuralları.](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming) Ağ arabirimi oluşturulduktan sonra ad değiştirilemez.|
    |Sanal ağ|Evet|Ağ arabirimi için sanal ağı seçin. Ağ arabirimiyle aynı abonelik te ve konumda bulunan sanal ağa yalnızca ağ arabirimi atayabilirsiniz. Bir ağ arabirimi oluşturulduktan sonra, atandığı sanal ağı değiştiremezsiniz. Ağ arabirimine eklediğiniz sanal makine, ağ arabirimiyle aynı konumda ve abonelikte de bulunmalıdır.|
    |Alt ağ|Evet|Seçtiğiniz sanal ağ içinde bir alt ağ seçin. Ağ arabiriminin oluşturulduktan sonra atandığı alt ağı değiştirebilirsiniz.|
    |Özel IP adresi ataması|Evet| Bu ayarda, IPv4 adresi için atama yöntemini seçmiyoruz. Aşağıdaki atama yöntemlerinden birini seçin: **Dinamik:** Bu seçeneği seçerken Azure, seçtiğiniz alt ağın adres alanından otomatik olarak kullanılabilir bir sonraki adresi atar. **Statik:** Bu seçeneği seçerken, seçtiğiniz alt ağın adres alanı içinden kullanılabilir bir IP adresini el ile atamanız gerekir. Statik ve dinamik adresler, siz değiştirene veya ağ arabirimi silinene kadar değişmez. Ağ arabirimi oluşturulduktan sonra atama yöntemini değiştirebilirsiniz. Azure DHCP sunucusu bu adresi sanal makinenin işletim sistemi içindeki ağ arabirimine atar.|
    |Ağ güvenlik grubu|Hayır| Kümesi **Yok'a**bırakın, varolan bir [ağ güvenlik grubu](security-overview.md)seçin veya bir ağ [güvenlik grubu oluşturun.](tutorial-filter-network-traffic.md) Ağ güvenlik grupları, ağ arabiriminin içinde ve dışında ağ trafiğini filtrelemenizi sağlar. Ağ arabirimine sıfır veya bir ağ güvenlik grubu uygulayabilirsiniz. Ağ arabiriminin atandığı alt ağa sıfır veya bir ağ güvenlik grubu da uygulanabilir. Bir ağ güvenlik grubu bir ağ arabirimine ve ağ arabirimine atanan alt ağa uygulandığında, bazen beklenmeyen sonuçlar oluşur. Ağ arabirimleri ve alt ağlarına uygulanan ağ güvenlik gruplarını sorun gidermek için [Sorun Giderme ağı güvenlik gruplarına](diagnose-network-traffic-filter-problem.md)bakın.|
    |Abonelik|Evet|Azure [aboneliklerinizden](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription)birini seçin. Ağ arabirimini iliştirdiğiniz sanal makine ve bağladığınız sanal ağ aynı abonelikte bulunmalıdır.|
    |Özel IP adresi (IPv6)|Hayır| Bu onay kutusunu seçerseniz, ağ arabirimine atanan IPv4 adresine ek olarak ağ arabirimine bir IPv6 adresi atanır. Ağ arabirimleri ile IPv6 kullanımı hakkında önemli bilgiler için bu makalenin IPv6 bölümüne bakın. IPv6 adresi için bir atama yöntemi seçemezsiniz. Bir IPv6 adresi atamayı seçerseniz, dinamik yöntemle atanır.
    |IPv6 adı (yalnızca **Özel IP adresi (IPv6)** onay kutusu işaretlendiğinde görünür) |Evet, **Özel IP adresi (IPv6)** onay kutusu işaretlenirse.| Bu ad, ağ arabirimi için ikincil bir IP yapılandırmasına atanır. IP yapılandırmaları hakkında daha fazla bilgi edinmek için [ağ arabirimi ayarlarını görüntüle'ye](#view-network-interface-settings)bakın.|
    |Kaynak grubu|Evet|Varolan bir [kaynak grubu](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) seçin veya bir tane oluşturun. Ağ arabirimi, iliştirdiğiniz sanal makineden veya bağladığınız sanal ağdan farklı veya farklı kaynak grubunda bulunabilir.|
    |Konum|Evet|Ağ arabirimine iliştirdiğiniz sanal makine ve bağlandığınız sanal ağ, bölge olarak da adlandırılan aynı [konumda](https://azure.microsoft.com/regions)bulunmalıdır.|

Portal, oluşturduğunuzda ağ arabirimine genel bir IP adresi atama seçeneği sağlamaz, ancak portal ortak bir IP adresi oluşturur ve portalı kullanarak sanal bir makine oluşturduğunuzda ağ arabirimine atar. Oluşturduktan sonra ağ arabirimine genel bir IP adresi eklemeyi öğrenmek için IP [adreslerini yönet'e](virtual-network-network-interface-addresses.md)bakın. Ortak BIR IP adresine sahip bir ağ arabirimi oluşturmak istiyorsanız, ağ arabirimini oluşturmak için CLI veya PowerShell'i kullanmanız gerekir.

Portal, ağ arabirimi oluştururken ağ arabirimini uygulama güvenlik gruplarına atama seçeneği sağlamaz, ancak Azure CLI ve PowerShell yapar. Ancak, ağ arabirimi sanal bir makineye bağlı olduğu sürece, portalı kullanarak varolan bir ağ arabirimi bir uygulama güvenlik grubuna atayabilirsiniz. Bir uygulama güvenlik grubuna ağ arabirimi nasıl atayacağız öğrenmek için uygulama [güvenlik gruplarına ekle veya uygulama güvenlik gruplarından kaldır'a](#add-to-or-remove-from-application-security-groups)bakın.

>[!Note]
> Azure, ağ arabirimi sanal bir makineye bağlandıktan ve sanal makine ilk kez başlatıldıktan sonra ağ arabirimine bir MAC adresi atar. Azure'un ağ arabirimine atadığı MAC adresini belirtemezsiniz. Ağ arabirimi silinene veya birincil ağ arabiriminin birincil IP yapılandırmasına atanan özel IP adresi değiştirilene kadar MAC adresi ağ arabirimine atanmaya devam eder. IP adresleri ve IP yapılandırmaları hakkında daha fazla bilgi edinmek için ip [adreslerini yönet'e](virtual-network-network-interface-addresses.md) bakın

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az network nic create](/cli/azure/network/nic)|
|PowerShell|[Yeni-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface)|

## <a name="view-network-interface-settings"></a>Ağ arabirimi ayarlarını görüntüleme

Bir ağ arabirimi için çoğu ayarı oluşturulduktan sonra görüntüleyebilir ve değiştirebilirsiniz. Portal, ağ arabirimi için DNS soneki veya uygulama güvenlik grubu üyeliğini görüntülemez. DNS soneki ve uygulama güvenliği grup üyeliğini görüntülemek için PowerShell veya Azure CLI [komutlarını](#view-settings-commands) kullanabilirsiniz.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Listeden görüntülemek veya ayarları değiştirmek istediğiniz ağ arabirimini seçin.
3. Seçtiğiniz ağ arabirimi için aşağıdaki öğeler listelenir:
   - **Genel Bakış:** Ağaatanan IP adresleri, ağ arabiriminin atandığı sanal ağ/alt ağ ve ağ arabiriminin bağlı olduğu sanal makine (birine bağlıysa) gibi ağ arabirimi hakkında bilgi sağlar. Aşağıdaki resim **mywebserver256**adlı bir ağ arabirimi ![için genel bakış ayarlarını gösterir : Ağ arabirimi genel bakış](./media/virtual-network-network-interface/nic-overview.png)

     **Kaynak grubunun** veya **Abonelik adının**yanındaki **(değiştir)** seçeneğini seçerek ağ arabirimini farklı bir kaynak grubuna veya aboneliğe taşıyabilirsiniz. Ağ arabirimini taşırsanız, ağ arabirimiyle ilgili tüm kaynakları onunla birlikte taşımanız gerekir. Ağ arabirimi sanal bir makineye bağlıysa, örneğin, sanal makineyi ve diğer sanal makineyle ilgili kaynakları da taşımanız gerekir. Ağ arabirimini taşımak [için bkz: Kaynağı yeni bir kaynak grubuna veya aboneye taşı.](../azure-resource-manager/management/move-resource-group-and-subscription.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-the-portal) Makalede ön koşullar ve Azure portalı, PowerShell ve Azure CLI'yi kullanarak kaynakların nasıl taşınır ı listeleyeceğiz.
   - **IP yapılandırmaları:** IP yapılandırmalarına atanan genel ve özel IPv4 ve IPv6 adresleri burada listelenmiştir. Bir IP yapılandırmasına bir IPv6 adresi atanırsa, adres görüntülenmez. IP yapılandırmaları ve IP adreslerinin nasıl ekleyip kaldırılış ları hakkında daha fazla bilgi edinmek [için, bir Azure ağ arabirimi için IP adreslerini yapılandırın'a](virtual-network-network-interface-addresses.md)bakın. IP yönlendirme ve alt ağ atamada da bu bölümde yapılandırılır. Bu ayarlar hakkında daha fazla bilgi edinmek için [IP iletmesini etkinleştir mecanda](#enable-or-disable-ip-forwarding) ve [alt ağ atamasını değiştir'e](#change-subnet-assignment)bakın.
   - **DNS sunucuları:** Azure DHCP sunucuları tarafından hangi DNS sunucusuna ağ arabirimi atandığını belirtebilirsiniz. Ağ arabirimi, ağ arabiriminin atandığı sanal ağdan ayarı devralabilir veya atandığı sanal ağın ayarını geçersiz kılan özel bir ayarına sahip olabilir. Görüntülenenleri değiştirmek için [DNS sunucularını değiştir'e](#change-dns-servers)bakın.
   - **Ağ güvenlik grubu (NSG):** NSG'nin ağ arabirimiyle ilişkili olduğu görüntüler (varsa). NSG, ağ arabirimi için ağ trafiğini filtrelemek için gelen ve giden kurallar içerir. Bir NSG ağ arabirimiyle ilişkilendirilirse, ilişkili NSG'nin adı görüntülenir. Görüntülenenleri değiştirmek için, [bkz.](#associate-or-dissociate-a-network-security-group)
   - **Özellikleri:** MAC adresi (ağ arabirimi sanal bir makineye bağlı değilse boş) ve içinde bulunduğu abonelik de dahil olmak üzere ağ arabirimi yle ilgili önemli ayarları görüntüler.
   - **Etkili güvenlik kuralları:**  Ağ arabirimi çalışan bir sanal makineye bağlıysa ve bir NSG ağ arabirimiyle, atandığı alt ağla veya her ikisiyle ilişkiliyse güvenlik kuralları listelenir. Görüntülenenler hakkında daha fazla bilgi edinmek için [etkili güvenlik kurallarını görüntüleyin.](#view-effective-security-rules) NSG'ler hakkında daha fazla bilgi edinmek için [Ağ güvenlik gruplarına](security-overview.md)bakın.
   - **Etkili rotalar:** Ağ arabirimi çalışan bir sanal makineye bağlıysa rotalar listelenir. Rotalar, Azure varsayılan yollarının, kullanıcı tanımlı yolların ve ağ arabiriminin atandığı alt ağ için var olabilecek BGP yollarının bir birleşimidir. Görüntülenenler hakkında daha fazla bilgi edinmek için [etkili yolları Görüntüle'ye](#view-effective-routes)bakın. Azure varsayılan yolları ve kullanıcı tanımlı rotalar hakkında daha fazla bilgi edinmek için [Yönlendirme'ye genel bakış'a](virtual-networks-udr-overview.md)bakın.
Ortak Azure Kaynak Yöneticisi ayarları: Ortak Azure Kaynak Yöneticisi ayarları hakkında daha fazla bilgi edinmek için [Etkinlik günlüğü,](../azure-monitor/platform/platform-logs-overview.md) [Erişim denetimi (IAM)](../role-based-access-control/overview.md), [Etiketler](../azure-resource-manager/management/tag-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Kilitler](../azure-resource-manager/management/lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)ve [Otomasyon komut dosyasına](../azure-resource-manager/templates/export-template-portal.md)bakın.

<a name="view-settings-commands"></a>**Komut**

Bir Ağ arabirimine bir IPv6 adresi atanmışsa, PowerShell çıkışı adresin atanmış olduğu gerçeğini döndürür, ancak atanan adresi döndürmez. Benzer şekilde, CLI adresatanmış olduğu gerçeğini döndürür, ancak adres için çıktısında *null* döndürür.

|Araç|Komut|
|---|---|
|CLI|[az ağ nic listesi](/cli/azure/network/nic) abonelik ağ arabirimleri görüntülemek için; [az ağ nic göstermek](/cli/azure/network/nic) bir ağ arabirimi için ayarları görüntülemek için|
|PowerShell|Ağ arabiriminin abonelik veya görünüm ayarlarındaki ağ arabirimlerini görüntülemek için [Get-AzNetworkInterface](/powershell/module/az.network/get-aznetworkinterface)|

## <a name="change-dns-servers"></a>DNS sunucularını değiştirme

DNS sunucusu, Azure DHCP sunucusu tarafından sanal makine işletim sistemi içindeki ağ arabirimine atanır. Atanan DNS sunucusu, ağ arabirimi için DNS sunucu ayarı ne olursa olsun. Ağ arabiriminin ad çözümleme ayarları hakkında daha fazla bilgi edinmek [için sanal makineler için Ad çözünürlüğüne](virtual-networks-name-resolution-for-vms-and-role-instances.md)bakın. Ağ arabirimi ayarları sanal ağdan devralabilir veya sanal ağ ayarını geçersiz kılacak kendi benzersiz ayarlarını kullanabilir.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Listeden bir DNS sunucusunu değiştirmek istediğiniz ağ arabirimini seçin.
3. **AYARLAR**altında **DNS sunucularını** seçin.
4. Birini seçin:
   - **Sanal ağdan devralma**: Ağ arabiriminin atandığı sanal ağ için tanımlanan DNS sunucu ayarını devralmak için bu seçeneği seçin. Sanal ağ düzeyinde, özel bir DNS sunucusu veya Azure tarafından sağlanan DNS sunucusu tanımlanır. Azure tarafından sağlanan DNS sunucusu, aynı sanal ağa atanan kaynakların ana bilgisayar adlarını çözümleyebilir. FQDN farklı sanal ağlara atanan kaynaklar için çözmek için kullanılmalıdır.
   - **Özel**: Birden çok sanal ağdaki adları çözümlemek için kendi DNS sunucunuzu yapılandırabilirsiniz. DNS sunucusu olarak kullanmak istediğiniz sunucunun IP adresini girin. Belirttiğiniz DNS sunucu adresi yalnızca bu ağ arabirimine atanır ve ağ arabiriminin atandığı sanal ağ için herhangi bir DNS ayarını geçersiz kılar.
     >[!Note]
     >VM kullanılabilirlik kümesinin bir parçası olan bir NIC kullanıyorsa, kullanılabilirlik kümesinin bir parçası olan tüm NIC'lerden her bir VM için belirtilen tüm DNS sunucuları devralınır.
5. **Kaydet'i**seçin.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ nic güncelleme](/cli/azure/network/nic)|
|PowerShell|[Set-AzNetworkInterface](/powershell/module/az.network/set-aznetworkinterface)|

## <a name="enable-or-disable-ip-forwarding"></a>IP iletmesini etkinleştirme veya devre dışı

IP iletme, bir ağ arabiriminin bağlı olduğu sanal makineyi sağlar:
- Ağ arabirimine atanan IP yapılandırmalarından herhangi birine atanan IP adreslerinden birine yönelik ağ trafiği alma.
- Ağ trafiğini, ağ arabiriminin IP yapılandırmalarından birine atanandan farklı bir kaynak IP adresiyle gönderin.

Ayarı, sanal makinenin iletmesi gereken trafiği alan sanal makineye bağlı her ağ arabirimi için etkinleştirilmelidir. Sanal bir makine, birden çok ağ arabirimi veya bağlı tek bir ağ arabirimi olup olmadığını trafiği iletebilir. IP yönlendirme bir Azure ayarı olsa da, sanal makinenin güvenlik duvarı, WAN optimizasyonu ve yük dengeleme uygulamaları gibi trafiği iletebilecek bir uygulama da çalıştırması gerekir. Sanal bir makine ağ uygulamalarını çalıştırırken, sanal makine genellikle ağ sanal cihazı olarak adlandırılır. [Azure](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances)Marketi'nde ağ sanal cihazlarını dağıtmaya hazır bir liste görüntüleyebilirsiniz. IP yönlendirme genellikle kullanıcı tanımlı rotalar ile kullanılır. Kullanıcı tanımlı rotalar hakkında daha fazla bilgi edinmek için [Kullanıcı tanımlı rotalara](virtual-networks-udr-overview.md)bakın.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Etkinleştirmek istediğiniz ağ arabirimini seçin veya IP yönlendirmesini devre dışı kılabilir.
3. **AYARLAR** bölümünde **IP yapılandırmalarını** seçin.
4. Ayarı değiştirmek için **Etkin** veya **Devre Dışı (varsayılan** ayar) seçeneğini belirleyin.
5. **Kaydet'i**seçin.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ nic güncelleme](/cli/azure/network/nic)|
|PowerShell|[Set-AzNetworkInterface](/powershell/module/az.network/set-aznetworkinterface)|

## <a name="change-subnet-assignment"></a>Alt ağ atamayı değiştirme

Ağ arabiriminin atandığı alt ağı değiştirebilirsiniz, ancak sanal ağı değiştiremezsiniz.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Alt ağ atamasını değiştirmek istediğiniz ağ arabirimini seçin.
3. **AYARLAR**altında **IP yapılandırmalarını** seçin. Listelenen herhangi bir IP yapılandırması için herhangi bir özel IP adresi varsa **, (Statik)** yanlarında varsa, izleyen adımları tamamlayarak IP adresi atama yöntemini dinamik olarak değiştirmeniz gerekir. Tüm özel IP adresleri, ağ arabirimi için alt ağ atamasını değiştirmek için dinamik atama yöntemiyle atanmalıdır. Adresler dinamik yöntemle atanırsa, beşinci adımı devam edin. Statik atama yöntemiyle herhangi bir IPv4 adresi atanmışsa, atama yöntemini dinamik olarak değiştirmek için aşağıdaki adımları tamamlayın:
   - IP yapılandırmaları listesinden IPv4 adres atama yöntemini değiştirmek istediğiniz IP yapılandırmasını seçin.
   - Özel IP adresi **Atama** yöntemi için **Dinamik'i** seçin. Statik atama yöntemiyle bir IPv6 adresi atayamazsınız.
   - **Kaydet'i**seçin.
4. Ağ arabirimini **Subnet** açılır listesinden taşımak istediğiniz alt ağı seçin.
5. **Kaydet'i**seçin. Yeni alt ağ için alt net adres aralığından yeni dinamik adresler atanır. Ağ arabirimini yeni bir alt ağa atadıktan sonra, isterseniz yeni alt net adres aralığından statik bir IPv4 adresi atayabilirsiniz. Ağ arabirimi için IP adresleri ekleme, değiştirme ve kaldırma hakkında daha fazla bilgi edinmek için IP [adreslerini yönet'e](virtual-network-network-interface-addresses.md)bakın.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ nic ip-config güncelleme](/cli/azure/network/nic/ip-config)|
|PowerShell|[Set-AzNetworkInterfaceIpConfig](/powershell/module/az.network/set-aznetworkinterfaceipconfig)|

## <a name="add-to-or-remove-from-application-security-groups"></a>Uygulama güvenlik gruplarına ekleme veya uygulama güvenlik gruplarından kaldırma

Ağ arabirimi sanal bir makineye bağlıysa, portalı kullanarak bir ağ arabirimine yalnızca ağ arabirimi ekleyebilir veya bir uygulama güvenlik grubundan ağ arabirimi kaldırabilirsiniz. PowerShell'i veya Azure CLI'yi, ağ arabirimi sanal bir makineye bağlı olsun veya olmasın, bir uygulama güvenlik grubuna ağ arabirimi eklemek veya bir ağ arabirimi kaldırmak için kullanabilirsiniz. [Uygulama güvenlik grupları](security-overview.md#application-security-groups) ve uygulama [güvenlik grubu oluşturma](manage-network-security-group.md)hakkında daha fazla bilgi edinin.

1. Portalın üst kısmındaki *Arama kaynakları, hizmetler ve dokümanlar* kutusunda, bir uygulama güvenlik grubuna eklemek veya kaldırmak istediğiniz bir ağ arabirimi olan sanal bir makinenin adını yazmaya başlayın. VM'nizin adı arama sonuçlarında göründüğünde, onu seçin.
2. **AYARLAR**'ın altında **Ağ İletişimi**’ni seçin.  **Uygulama Güvenlik Grupları'nı** seçin, ardından uygulama **güvenlik gruplarını yapılandırın,** ağ arabirimini eklemek istediğiniz uygulama güvenlik gruplarını seçin veya ağ arabirimini kaldırmak istediğiniz uygulama güvenlik gruplarını seçin ve sonra **Kaydet'i**seçin. Aynı sanal ağda bulunan ağ arabirimleri aynı uygulama güvenlik grubuna eklenebilir. Uygulama güvenlik grubu ağ arabirimi ile aynı konumda olmalıdır.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ nic güncelleme](/cli/azure/network/nic)|
|PowerShell|[Set-AzNetworkInterface](/powershell/module/az.network/set-aznetworkinterface)|

## <a name="associate-or-dissociate-a-network-security-group"></a>Bir ağ güvenlik grubunu ilişkilendirme veya ayırma

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *ağ arabirimlerini* girin. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Bir ağ güvenlik grubunu ilişkilendirmek istediğiniz listedeki ağ arabirimini seçin veya ağ güvenlik grubunu ayırın.
3. **AYARLAR**altında **Ağ güvenlik grubunu** seçin.
4. **Edit'i**seçin.
5. **Ağ güvenlik grubunu** seçin ve ardından ağ arabirimiyle ilişkilendirmek istediğiniz ağ güvenlik grubunu seçin veya ağ güvenlik grubunu ayırmak için **Yok'u**seçin.
6. **Kaydet'i**seçin.

**Komutlar**

- Azure CLI: [az ağ nic güncelleştirmesi](/cli/azure/network/nic#az-network-nic-update)
- PowerShell: [Set-AzNetworkInterface](/powershell/module/az.network/set-aznetworkinterface)

## <a name="delete-a-network-interface"></a>Ağ arabirimini silme

Sanal bir makineye bağlı olmadığı sürece ağ arabirimini silebilirsiniz. Bir ağ arabirimi sanal bir makineye bağlıysa, önce sanal makineyi durdurulan (ayrılan) duruma yerleştirmeniz, ardından ağ arabirimini sanal makineden ayırmanız gerekir. Bir ağ arabirimini sanal bir makineden ayırmak için, [sanal bir makineden ağ arabirimini ayırma](virtual-network-network-interface-vm.md#remove-a-network-interface-from-a-vm)adımlarını tamamlayın. Ancak, sanal makineye bağlı tek ağ arabirimi ise, bir ağ arabirimini sanal makineden ayıramazsınız. Sanal bir makinenin her zaman en az bir ağ arabirimi olmalıdır. Sanal bir makinenin silinmesi, bağlı tüm ağ arabirimlerini ayırır, ancak ağ arabirimlerini silmez.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuda *ağ arabirimleri*yazın. **Ağ arabirimleri** arama sonuçlarında göründüğünde, onu seçin.
2. Silmek istediğiniz listedeki ağ arabirimini seçin.
3. **Genel Bakış** Altında **Sil'i**seçin.
4. Ağ arabiriminin silinmesini onaylamak için **Evet'i** seçin.

Bir ağ arabirimini sildiğinizde, ona atanan mac veya IP adresleri serbest bırakılır.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ nic silme](/cli/azure/network/nic)|
|PowerShell|[Remove-AzNetworkInterface](/powershell/module/az.network/remove-aznetworkinterface)|

## <a name="resolve-connectivity-issues"></a>Bağlantı sorunlarını çözme

Sanal bir makineyle veya sanal makineden iletişim kuramıyorsanız, ağ güvenlik grubu güvenlik kuralları veya ağ arabirimi için etkili rotalar soruna neden olabilir. Sorunu çözmeye yardımcı olmak için aşağıdaki seçeneklere sahipsiniz:

### <a name="view-effective-security-rules"></a>Etkili güvenlik kurallarını görüntüleme

Sanal makineye bağlı her ağ arabirimi için etkili güvenlik kuralları, ağ güvenlik grubunda oluşturduğunuz kuralların ve [varsayılan güvenlik kurallarının](security-overview.md#default-security-rules)bir birleşimidir. Ağ arabiriminin etkili güvenlik kurallarını anlamak, sanal bir makineyle neden iletişim kuramadığınızı belirlemenize yardımcı olabilir. Çalışan bir sanal makineye bağlı herhangi bir ağ arabirimi için etkili kuralları görüntüleyebilirsiniz.

1. Portalın üst kısmındaki arama kutusuna, etkili güvenlik kurallarını görüntülemek istediğiniz sanal bir makinenin adını girin. Sanal makinenin adını bilmiyorsanız, arama kutusuna *sanal makineleri* girin. **Sanal makineler** arama sonuçlarında göründüğünde, onu seçin ve ardından listeden sanal bir makine seçin.
2. **AYARLAR**altında **Ağ'ı** seçin.
3. Ağ arabiriminin adını seçin.
4. **SUPPORT + SORUN GIDERME**altında Etkili güvenlik **kurallarını** seçin.
5. Gerekli gelen ve giden iletişiminiz için doğru kuralların var olup olmadığını belirlemek için etkili güvenlik kuralları listesini gözden geçirin. [Ağ güvenlik grubuna genel bakış](security-overview.md)listesinde gördükleriniz hakkında daha fazla bilgi edinin.

Azure Ağ İzleyicisi'nin IP akışı doğrulama özelliği, güvenlik kurallarının sanal makine ile bitiş noktası arasındaki iletişimi engelleyip engellemediğinizi belirlemenize de yardımcı olabilir. Daha fazla bilgi için [IP akışını niçin doğrulayabın.](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

**Komutlar**

- Azure CLI: [az ağ nic listesi-etkili-nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg)
- PowerShell: [Get-AzEffectiveNetworkSecurityGroup](/powershell/module/az.network/get-azeffectivenetworksecuritygroup)

### <a name="view-effective-routes"></a>Etkili rotaları görüntüleme

Sanal makineye bağlı ağ arabirimlerinin etkili yolları varsayılan yolların, oluşturduğunuz yolların ve BGP üzerinden bgp üzerinden yayılan yolların bir birleşimidir. Ağ arabiriminin etkili yollarını anlamak, sanal bir makineyle neden iletişim kuramadığınızı belirlemenize yardımcı olabilir. Çalışan bir sanal makineye bağlı herhangi bir ağ arabirimi için etkili yolları görüntüleyebilirsiniz.

1. Portalın üst kısmındaki arama kutusuna, etkili güvenlik kurallarını görüntülemek istediğiniz sanal bir makinenin adını girin. Sanal makinenin adını bilmiyorsanız, arama kutusuna *sanal makineleri* girin. **Sanal makineler** arama sonuçlarında göründüğünde, onu seçin ve ardından listeden sanal bir makine seçin.
2. **AYARLAR**altında **Ağ'ı** seçin.
3. Ağ arabiriminin adını seçin.
4. SUPPORT + **SORUN GIDERME**altında **Etkili rotalar** seçin.
5. Gerekli gelen ve giden iletişiminiz için doğru yolların var olup olmadığını belirlemek için etkili yolların listesini gözden geçirin. [Yönlendirme genel bakışı](virtual-networks-udr-overview.md)listesinde gördükleriniz hakkında daha fazla bilgi edinin.

Azure Ağ İzleyicisi'nin bir sonraki atlama özelliği, yolların sanal makine ile bitiş noktası arasındaki iletişimi engelleyip engellemediğinizi belirlemenize de yardımcı olabilir. Daha fazla bilgi için [Sonraki hop'a](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

**Komutlar**

- Azure CLI: [az ağ nic show-etkili-rota-tablo](/cli/azure/network/nic#az-network-nic-show-effective-route-table)
- PowerShell: [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable)

## <a name="permissions"></a>İzinler

Ağ arabirimlerinde görevleri gerçekleştirmek için, hesabınızın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya aşağıdaki tabloda listelenen uygun izinlere atanan [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir role atanması gerekir:

| Eylem                                                                     | Adı                                                      |
| ---------                                                                  | -------------                                             |
| Microsoft.Network/networkInterfaces/read                                   | Ağ arabirimini alın                                     |
| Microsoft.Network/networkInterfaces/write                                  | Ağ arabirimi oluşturma veya güncelleştirme                        |
| Microsoft.Network/networkInterfaces/join/action                            | Sanal makineye ağ arabirimi ekleme           |
| Microsoft.Network/networkInterfaces/delete                                 | Ağ arabirimini silme                                  |
| Microsoft.Network/networkInterfaces/joinViaPrivateIp/action                | Bir servi üzerinden ağ arabirimine bir kaynak katılın ...     |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action             | Ağ arabirimini etkili rota tablosuna alın               |
| Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action  | Ağ arabirimini etkili güvenlik gruplarına edin           |
| Microsoft.Network/networkInterfaces/loadBalancers/read                     | Ağ arabirimi yük dengeleyicileri alın                      |
| Microsoft.Network/networkInterfaces/serviceAssociations/read               | Servis derneği alın                                   |
| Microsoft.Network/networkInterfaces/serviceAssociations/write              | Hizmet ilişkilendirme oluşturma veya güncelleştirme                    |
| Microsoft.Network/networkInterfaces/serviceAssociations/delete             | Hizmet ilişkilendirme silme                                |
| Microsoft.Network/networkInterfaces/serviceAssociations/validate/action    | Hizmet ilişkilendirmedoğru                              |
| Microsoft.Network/networkInterfaces/ipconfigurations/read                  | Ağ arabirimi IP yapılandırması alın                    |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [PowerShell'i](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanarak birden fazla NIC içeren bir VM oluşturma
- [Azure CLI](virtual-network-multiple-ip-addresses-cli.md) veya [PowerShell'i](virtual-network-multiple-ip-addresses-powershell.md) kullanarak birden çok IPv4 adresi içeren tek bir NIC VM oluşturun
- [Azure CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)veya [Azure Kaynak Yöneticisi şablonu](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanarak özel bir IPv6 adresiyle (Azure Yük Bakiyeleyicisinin arkasında) tek bir NIC VM oluşturun
- [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyalarını kullanarak veya Azure [Kaynak Yöneticisi şablonu](template-samples.md) kullanarak ağ arabirimi oluşturma
- Sanal ağlar için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama
