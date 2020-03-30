---
title: Azure sanal ağı oluşturma, değiştirme veya silme
titlesuffix: Azure Virtual Network
description: Azure'da sanal bir ağı nasıl oluştureceğinizi, değiştireceğinizi veya sileceğinizi öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/10/2019
ms.author: kumud
ms.openlocfilehash: 70523dc12f3f20362fcf4a2c3cb456a182038e8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280241"
---
# <a name="create-change-or-delete-a-virtual-network"></a>Sanal ağ oluşturma, değiştirme veya silme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Varolan bir sanal ağ için sanal ağ oluşturma ve silme ve DNS sunucuları ve IP adresi alanları gibi ayarları nasıl değiştireceğinizi öğrenin. Sanal ağlarda yeniyseniz, [Sanal ağa genel bakışta](virtual-networks-overview.md) veya bir [öğreticiyi](quick-create-portal.md)tamamlayarak bunlar hakkında daha fazla bilgi edinebilirsiniz. Sanal ağ alt ağlar içerir. Alt ağları nasıl oluşturup değiştireceğinizi ve sileceğinizi öğrenmek için [bkz.](virtual-network-manage-subnet.md)

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalenin herhangi bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, ücretsiz deneme [hesabı](https://azure.microsoft.com/free)için kaydolun.
- Portalı kullanıyorsanız, https://portal.azure.comAzure hesabınızla açın ve oturum açın.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanıyorsanız, [Azure Bulut Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure Komut satırı arabirimi (CLI) komutlarını kullanıyorsanız, [komutları Azure Bulut Kabuğu'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Bu öğretici, Azure CLI sürüm 2.0.31 veya sonraki sürüm gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` Azure ile bağlantı oluşturmak için de çalıştırmanız gerekir.
- Oturum açtığınız veya Azure'a bağlandığınız hesabın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya [İzinler'de](#permissions)listelenen uygun eylemlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Seçin +**Sanal ağ****ağı ağı** >  **oluştur.** > 
2. Aşağıdaki ayarlar için değerleri girin veya seçin, ardından **Oluştur'u**seçin:
   - **Ad**: Adı sanal ağ oluşturmak için seçtiğiniz [kaynak grubunda](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) benzersiz olmalıdır. Sanal ağ oluşturulduktan sonra adı değiştiremezsiniz. Zaman içinde birden çok sanal ağ oluşturabilirsiniz. Adlandırma önerileri [için, adlandırma kurallarına](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#naming-and-tagging-resources)bakın. Bir adlandırma kuralını takip etmek, birden çok sanal ağı yönetmenin kolaylaşmasını sağlayabilir.
   - **Adres alanı**: Sanal bir ağın adres alanı, CIDR gösteriminde belirtilen bir veya daha fazla örtüşmeyen adres aralıklarından oluşur. Tanımladığınız adres aralığı herkese açık veya özel olabilir (RFC 1918). Adres aralığını genel veya özel olarak tanımlayın, adres aralığına yalnızca sanal ağ içinden, birbirine bağlı sanal ağlardan ve sanal ağa bağlandığınız şirket içi ağlardan ulaşılabilir. Aşağıdaki adres aralıklarını ekleyemezsiniz:
     - 224.0.0.0/4 (Çok Noktaya Yayın)
     - 255.255.255.255/32 (Yayın)
     - 127.0.0.0/8 (Geri Dönüş)
     - 169.254.0.0/16 (Bağlantı-yerel)
     - 168.63.129.16/32 (Dahili DNS, DHCP ve Azure Yük [Dengeleyicisi sağlık sondası)](../load-balancer/load-balancer-custom-probe-overview.md#probesource)

     Portalda sanal ağ oluşturduğunuzda yalnızca bir adres aralığı tanımlayabiliyor olsanız da, sanal ağ oluşturulduktan sonra adres alanına daha fazla adres aralığı ekleyebilirsiniz. Varolan bir sanal ağa adres aralığı nın nasıl ekleyeceğiniöğrenmek için adres [aralığı ekle veya kaldır'a](#add-or-remove-an-address-range)bakın.

     >[!WARNING]
     >Sanal bir ağ, başka bir sanal ağ veya şirket içi ağla çakışan adres aralıklarına sahipse, iki ağ bağlanamaz. Bir adres aralığı nı tanımlamadan önce, sanal ağı gelecekte diğer sanal ağlara veya şirket içi ağlara bağlamak isteyip istemediğinizgöz önünde bulundurun.
     >
     >

     - **Alt ağ adı**: Alt ağ adı sanal ağ içinde benzersiz olmalıdır. Alt ağ oluşturulduktan sonra alt net adını değiştiremezsiniz. Portal, sanal bir ağ için herhangi bir alt ağa sahip olmak gerekmese bile, sanal ağ oluştururken bir alt ağ tanımlamanızı gerektirir. Portalda, sanal ağ oluşturduğunuzda yalnızca bir alt ağ tanımlayabilirsiniz. Sanal ağ oluşturulduktan sonra sanal ağa daha fazla alt ağ ekleyebilirsiniz. Sanal ağa alt ağ eklemek için [bkz.](virtual-network-manage-subnet.md) Azure CLI veya PowerShell'i kullanarak birden çok alt ağa sahip bir sanal ağ oluşturabilirsiniz.

       >[!TIP]
       >Bazen yöneticiler, alt ağlar arasındaki trafik yönlendirmesini filtrelemek veya denetlemek için farklı alt ağlar oluşturur. Alt ağları tanımlamadan önce, alt ağlarınız arasındaki trafiği nasıl filtrelemek ve yönlendirmek isteyebileceğini düşünün. Alt ağlar arasındaki trafiği filtreleme hakkında daha fazla bilgi edinmek için [Ağ güvenlik gruplarına](security-overview.md)bakın. Azure, alt ağlar arasındaki trafiği otomatik olarak yönlendirir, ancak Azure varsayılan rotalarını geçersiz kılabilir. Azure'ların varsayılan alt ağ trafiği yönlendirmesi hakkında daha fazla bilgi edinmek için [Yönlendirme'ye genel bakış'a](virtual-networks-udr-overview.md)bakın.
       >

     - **Alt net adres aralığı**: Aralık, sanal ağ için girdiğiniz adres alanı içinde olmalıdır. Belirtebileceğiniz en küçük aralık,alt ağ için sekiz IP adresi sağlayan /29'dur. Azure, protokol uygunluğu için her alt ağdaki ilk ve son adresi saklı tutar. Azure hizmet kullanımı için üç ek adres ayrılmıştır. Sonuç olarak, /29 alt net adres aralığına sahip bir sanal ağ yalnızca üç kullanılabilir IP adresine sahiptir. Sanal bir ağı VPN ağ geçidine bağlamayı planlıyorsanız, bir ağ geçidi alt ağı oluşturmanız gerekir. [Ağ geçidi alt ağları için belirli adres aralığı hususları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub)hakkında daha fazla bilgi edinin. Alt ağ oluşturulduktan sonra, belirli koşullar altında adres aralığını değiştirebilirsiniz. Alt ağ adres aralığını nasıl değiştireceğinizi öğrenmek için [bkz.](virtual-network-manage-subnet.md)
     - **Abonelik**: Bir [abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription)seçin. Aynı sanal ağı birden fazla Azure aboneliğinde kullanamazsınız. Ancak, sanal ağ [eşleme](virtual-network-peering-overview.md)ile diğer aboneliklerde sanal ağlara bir abonelik bir sanal ağ bağlayabilirsiniz. Sanal ağa bağlandığınız herhangi bir Azure kaynağı, sanal ağla aynı abonelikte olmalıdır.
     - **Kaynak grubu**: Varolan bir kaynak grubu seçin veya yeni bir [kaynak grubu](../azure-resource-manager/management/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) oluşturun. Sanal ağa bağlandığınız bir Azure kaynağı, sanal ağla aynı kaynak grubunda veya farklı bir kaynak grubunda olabilir.
     - **Konum**: Bölge olarak da bilinen bir Azure [konumu](https://azure.microsoft.com/regions/)seçin. Sanal ağ yalnızca bir Azure konumunda olabilir. Ancak, vpn ağ geçidi kullanarak bir konumdaki sanal ağı başka bir konumdaki sanal ağa bağlayabilirsiniz. Sanal ağa bağlandığınız herhangi bir Azure kaynağı, sanal ağla aynı konumda olmalıdır.

**Komutlar**

- Azure CLI: [az ağ vnet oluşturmak](/cli/azure/network/vnet)
- PowerShell: [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)

## <a name="view-virtual-networks-and-settings"></a>Sanal ağları ve ayarları görüntüleme

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin.
2. Sanal ağlar listesinden, ayarları görüntülemek istediğiniz sanal ağı seçin.
3. Seçtiğiniz sanal ağ için aşağıdaki ayarlar listelenir:
   - **Genel Bakış**: Adres alanı ve DNS sunucuları da dahil olmak üzere sanal ağ hakkında bilgi sağlar. Aşağıdaki ekran görüntüsü **MyVNet**adlı bir sanal ağ için genel bakış ayarlarını gösterir:

     ![Ağ arabirimine genel bakış](./media/manage-virtual-network/vnet-overview.png)

     **Kaynak grubunun** veya **Abonelik adının**yanında **Değiştir'i** seçerek sanal ağı farklı bir aboneliğe veya kaynak grubuna taşıyabilirsiniz. Sanal ağı nasıl taşıyacağız öğrenmek [için kaynakları farklı bir kaynak grubuna veya aboneye taşıyın'](../azure-resource-manager/management/move-resource-group-and-subscription.md?toc=%2fazure%2fvirtual-network%2ftoc.json)ı 'na bakın. Makalede, Azure portalı, PowerShell ve Azure CLI'yi kullanarak ön koşulları ve kaynakların nasıl taşınır' ı listelemesi listelanmaktadır. Sanal ağa bağlı tüm kaynakların sanal ağla birlikte taşınması gerekir.
   - **Adres alanı**: Sanal ağa atanan adres boşlukları listelenir. Adres alanına adres aralığını nasıl ekleyeceğinizi ve kaldırmayı öğrenmek [için, Ekle'deki](#add-or-remove-an-address-range)adımları tamamlayın veya adres aralığını kaldırın.
   - **Bağlı aygıtlar**: Sanal ağa bağlı tüm kaynaklar listelenir. Önceki ekran görüntüsünde, üç ağ arabirimi ve bir yük dengeleyici sanal ağa bağlanır. Oluşturduğunuz ve sanal ağa bağlandığınız tüm yeni kaynaklar listelenir. Sanal ağa bağlı bir kaynağı silerseniz, artık listede görünmez.
   - **Alt ağlar**: Sanal ağ içinde bulunan alt ağların listesi gösterilir. Bir alt ağ eklemeyi ve kaldırmayı öğrenmek için [bkz.](virtual-network-manage-subnet.md)
   - **DNS sunucuları**: Azure dahili DNS sunucusunun veya özel bir DNS sunucusunun sanal ağa bağlı aygıtlar için ad çözünürlüğü sağlayıp sağlamadığını belirtebilirsiniz. Azure portalını kullanarak sanal bir ağ oluşturduğunuzda, Azure'un DNS sunucuları varsayılan olarak sanal ağ içinde ad çözümlemesi için kullanılır. DNS sunucularını değiştirmek için, bu makaledeki [DNS sunucularını değiştir](#change-dns-servers) adımları tamamlayın.
   - **Peerings**: Abonelikte var olan eşlemeler varsa, bunlar burada listelenir. Varolan görünümlerin ayarlarını görüntüleyebilir veya görünümler oluşturabilir, değiştirebilir veya silebilirsiniz. Eşlemeler hakkında daha fazla bilgi edinmek için [Sanal ağ eşlemesine](virtual-network-peering-overview.md)bakın.
   - **Özellikler**: Sanal ağın kaynak kimliği ve içinde bulunduğu Azure aboneliği de dahil olmak üzere sanal ağla ilgili ayarları görüntüler.
   - **Diyagram**: Diyagram, sanal ağa bağlı tüm aygıtların görsel bir temsilini sağlar. Diyagramda aygıtlar hakkında bazı önemli bilgiler bulunur. Bu görünümdeki bir aygıtı yönetmek için, diyagramda aygıtı seçin.
   - **Ortak Azure ayarları**: Ortak Azure ayarları hakkında daha fazla bilgi edinmek için aşağıdaki bilgilere bakın:
     - [Etkinlik günlüğü](../azure-monitor/platform/platform-logs-overview.md)
     - [Erişim denetimi (IAM)](../role-based-access-control/overview.md)
     - [Etiketler](../azure-resource-manager/management/tag-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
     - [Kilitler](../azure-resource-manager/management/lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
     - [Otomasyon betiği](../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates)

**Komutlar**

- Azure CLI: [az ağ vnet gösterisi](/cli/azure/network/vnet)
- PowerShell: [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork)

## <a name="add-or-remove-an-address-range"></a>Adres aralığı ekleme veya kaldırma

Sanal ağ için adres aralıkları ekleyebilir ve kaldırabilirsiniz. Bir adres aralığı CIDR gösteriminde belirtilmelidir ve aynı sanal ağdaki diğer adres aralıklarıyla çakışamaz. Tanımladığınız adres aralıkları genel veya özel olabilir (RFC 1918). Adres aralığını genel veya özel olarak tanımlayın, adres aralığına yalnızca sanal ağ içinden, birbirine bağlı sanal ağlardan ve sanal ağa bağlandığınız şirket içi ağlardan ulaşılabilir. 

İlişkili alt ağların aralıklarını içerdiği sürece sanal bir ağın adres aralığını azaltabilirsiniz. Ayrıca, adres aralığını genişletebilirsiniz, örneğin bir /16'yı /8 olarak değiştirerek. 

<!-- the above statement has been edited to reflect the most recent comments on the reopened issue: https://github.com/MicrosoftDocs/azure-docs/issues/20572 -->

Aşağıdaki adres aralıklarını ekleyemezsiniz:

- 224.0.0.0/4 (Çok Noktaya Yayın)
- 255.255.255.255/32 (Yayın)
- 127.0.0.0/8 (Geri Dönüş)
- 169.254.0.0/16 (Bağlantı-yerel)
- 168.63.129.16/32 (Dahili DNS, DHCP ve Azure Yük [Dengeleyicisi sağlık sondası)](../load-balancer/load-balancer-custom-probe-overview.md#probesource)

Adres aralığı eklemek veya kaldırmak için:

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin.
2. Sanal ağlar listesinden, adres aralığı eklemek veya kaldırmak istediğiniz sanal ağı seçin.
3. **AYARLAR**altında **Adres alanını**seçin.
4. Aşağıdaki seçeneklerden birini tamamlayın:
    - **Adres aralığı ekle**: Yeni adres aralığını girin. Adres aralığı, sanal ağ için tanımlanan varolan bir adres aralığıyla çakışamaz.
    - **Adres aralığını kaldırın**: Kaldırmak istediğiniz adres aralığının sağında , ... **seçin,** ardından **Kaldır'ı**seçin. Adres aralığında bir alt ağ varsa, adres aralığını kaldıramazsınız. Bir adres aralığını kaldırmak için, önce adres aralığında bulunan alt ağları (ve alt ağlardaki tüm kaynakları) silmeniz gerekir.
5. **Kaydet'i**seçin.

**Komutlar**

- Azure CLI: [az ağ vnet güncelleştirmesi](/cli/azure/network/vnet)
- PowerShell: [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork)

## <a name="change-dns-servers"></a>DNS sunucularını değiştirme

Sanal ağa bağlı tüm SANAL M'ler, sanal ağ için belirttiğiniz DNS sunucularına kaydolur. Ayrıca ad çözümlemesi için belirtilen DNS sunucusunu da kullanırlar. VM'deki her ağ arabirimi (NIC) kendi DNS sunucu ayarlarına sahip olabilir. Bir NIC'nin kendi DNS sunucu ayarları varsa, sanal ağ için DNS sunucu ayarlarını geçersiz kılar. NIC DNS ayarları hakkında daha fazla bilgi edinmek için [Ağ arabirimi görevleri ve ayarlarına](virtual-network-network-interface.md#change-dns-servers)bakın. Azure Bulut Hizmetleri'ndeki VM'ler ve rol örnekleri için ad çözümlemesi hakkında daha fazla bilgi edinmek [için, VM'ler ve rol örnekleri için Ad çözünürlüğüne](virtual-networks-name-resolution-for-vms-and-role-instances.md)bakın. Bir DNS sunucusu eklemek, değiştirmek veya kaldırmak için:

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin.
2. Sanal ağlar listesinden, DNS sunucularını değiştirmek istediğiniz sanal ağı seçin.
3. **AYARLAR**altında **DNS sunucularını**seçin.
4. Aşağıdaki seçeneklerden birini belirtin:
   - **Varsayılan (Azure sağlananı)**: Tüm kaynak adları ve özel IP adresleri otomatik olarak Azure DNS sunucularına kaydedilir. Aynı sanal ağa bağlı tüm kaynaklar arasındaki adları çözümleyebilirsiniz. Bu seçeneği, sanal ağlardaki adları çözümlemek için kullanamazsınız. Sanal ağlarda adları çözümlemek için özel bir DNS sunucusu kullanmanız gerekir.
   - **Özel**: Sanal ağ için Azure sınırına kadar bir veya daha fazla sunucu ekleyebilirsiniz. DNS sunucu sınırları hakkında daha fazla bilgi edinmek için [Azure sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic)bakın. Aşağıdaki seçenekleriniz vardır:
   - **Adres ekle**: Sunucuyu sanal ağınız DNS sunucular listenize ekler. Bu seçenek, DNS sunucusunu Azure'a da kaydeder. Azure'a zaten bir DNS sunucusu kaydettiyseniz, listedeki DNS sunucusunu seçebilirsiniz.
   - **Adresi kaldır**: Kaldırmak istediğiniz sunucunun yanında... **,** sonra **kaldır'** ı seçin. Sunucunun silmesi, sunucuyu yalnızca bu sanal ağ listesinden kaldırır. DNS sunucusu, diğer sanal ağlarınızın kullanması için Azure'da kayıtlı kalır.
   - **DNS sunucu adreslerini yeniden sıralayın : DNS**sunucularınızı ortamınız için doğru sırada listelediğinizi doğrulamanız önemlidir. DNS sunucu listeleri belirtilen sırayla kullanılır. Onlar bir round-robin kurulum olarak çalışmıyor. Listedeki ilk DNS sunucusuna ulaşılabiliyorsa, istemci, DNS sunucusunun düzgün çalışıp çalışmadığına bakılmaksızın bu DNS sunucusunu kullanır. Listelenen tüm DNS sunucularını kaldırın ve sonra bunları istediğiniz sıraya geri ekleyin.
   - **Adresi değiştir**: Listedeki DNS sunucusunu vurgulayın ve ardından yeni adresi girin.
5. **Kaydet'i**seçin.
6. Sanal ağa bağlı VM'leri yeniden başlatın, böylece yeni DNS sunucu ayarlarıa atanırlar. VM'ler yeniden başlatılana kadar geçerli DNS ayarlarını kullanmaya devam ederler.

**Komutlar**

- Azure CLI: [az ağ vnet güncelleştirmesi](/cli/azure/network/vnet)
- PowerShell: [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork)

## <a name="delete-a-virtual-network"></a>Sanal ağı silme

Sanal ağı yalnızca bağlı kaynak yoksa silebilirsiniz. Sanal ağdaki herhangi bir alt ağa bağlı kaynaklar varsa, öncelikle sanal ağdaki tüm alt ağlara bağlı kaynakları silmeniz gerekir. Bir kaynağı silmek için attığınız adımlar kaynağa bağlı olarak değişir. Alt ağlara bağlı kaynakları nasıl silersiniz öğrenmek için, silmek istediğiniz her kaynak türü için belgeleri okuyun. Sanal ağı silmek için:

1. Portalın üst kısmındaki arama kutusuna, arama kutusuna *sanal ağlar* girin. **Sanal ağlar** arama sonuçlarında göründüğünde, bu ağları seçin.
2. Sanal ağlar listesinden, silmek istediğiniz sanal ağı seçin.
3. **SETTINGS**altında **Bağlı aygıtları**seçerek sanal ağa bağlı aygıt olmadığını doğrulayın. Bağlı aygıtlar varsa, sanal ağı silmeden önce bunları silmeniz gerekir. Bağlı aygıt yoksa Genel **Bakış'ı**seçin.
4. **Sil**’i seçin.
5. Sanal ağın silinmesini onaylamak için **Evet'i**seçin.

**Komutlar**

- Azure CLI: [azure ağ vnet silme](/cli/azure/network/vnet)
- PowerShell: [Remove-azvirtualNetwork](/powershell/module/az.network/remove-azvirtualnetwork)

## <a name="permissions"></a>İzinler

Sanal ağlarda görevleri gerçekleştirmek için, hesabınızın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya aşağıdaki tabloda listelenen uygun eylemleri atanan [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir role atanması gerekir:

| Eylem                                  |   Adı                                |
|---------------------------------------- |   --------------------------------    |
|Microsoft.Network/virtualNetworks/read   |   Sanal Bir Ağ okuma              |
|Microsoft.Network/virtualNetworks/write  |   Sanal ağ oluşturma veya güncelleştirme  |
|Microsoft.Network/virtualNetworks/silme |   Sanal ağı silme            |

## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell](powershell-samples.md) veya [Azure CLI](cli-samples.md) örnek komut dosyalarını kullanarak veya Azure [Kaynak Yöneticisi şablonlarını](template-samples.md) kullanarak sanal ağ oluşturma
- Sanal ağlar için [Azure ilkesi](policy-samples.md) oluşturma ve uygulama
