---
title: Azure genel IP adresi öneki oluşturma, değiştirme veya silme
titlesuffix: Azure Virtual Network
description: Genel bir IP adresi önekioluşturmayı, değiştireceğinizi veya silemeyi öğrenin.
services: virtual-network
documentationcenter: na
author: anavinahar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/13/2019
ms.author: anavin
ms.openlocfilehash: 5f0c2d9757f3652b0f83b8c36d89c855f7a92fdd
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383859"
---
# <a name="create-change-or-delete-a-public-ip-address-prefix"></a>Genel IP adresi ön eki oluşturma, değiştirme veya silme

Genel bir IP adresi öneki ve nasıl oluşturulacak, değiştirilip silineceği hakkında bilgi edinin. Genel IP adresi öneki, belirttiğiniz ortak IP adreslerinin sayısına bağlı olarak bitişik bir adres aralığıdır. Adresler aboneliğinize atanır. Genel bir IP adresi kaynağı oluşturduğunuzda, önek teki statik bir genel IP adresi atayabilir ve internet bağlantısını etkinleştirmek için adresi sanal makinelere, yük dengeleyicilerine veya diğer kaynaklara ilişkilendirebilirsiniz. Genel IP adresi önekleri hakkında bilginiz yoksa, [Genel IP adresi önekine genel bakış](public-ip-address-prefix.md)

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalenin herhangi bir bölümündeki adımları tamamlamadan önce aşağıdaki görevleri tamamlayın:

- Zaten bir Azure hesabınız yoksa, ücretsiz deneme [hesabı](https://azure.microsoft.com/free)için kaydolun.
- Portalı kullanıyorsanız, https://portal.azure.comAzure hesabınızla açın ve oturum açın.
- Bu makaledeki görevleri tamamlamak için PowerShell komutlarını kullanıyorsanız, [Azure Bulut Shell'deki](https://shell.azure.com/powershell)komutları çalıştırın veya bilgisayarınızdan PowerShell çalıştırın. Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın Azure araçları, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bu öğretici, Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.
- Bu makaledeki görevleri tamamlamak için Azure Komut satırı arabirimi (CLI) komutlarını kullanıyorsanız, [komutları Azure Bulut Kabuğu'ndaki](https://shell.azure.com/bash)komutları çalıştırın veya CLI'yi bilgisayarınızdan çalıştırın. Bu öğretici, Azure CLI sürümü 2.0.41 veya sonrası gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). Azure CLI'yi yerel olarak çalıştırıyorsanız, `az login` Azure ile bağlantı oluşturmak için de çalıştırmanız gerekir.

Oturum açtığınız veya Azure'a bağlandığınız hesabın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya [İzinler'de](#permissions)listelenen uygun eylemlere atanan özel bir [role](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atanması gerekir.

Genel IP adresi önekleri bir ücrete sahiptir. Ayrıntılar için [fiyatlandırmaya](https://azure.microsoft.com/pricing/details/ip-addresses)bakın.

## <a name="create-a-public-ip-address-prefix"></a>Genel IP adresi öneki oluşturma

1. Portalın üst, sol köşesinde + **Kaynak oluştur'u**seçin.
2. *Marketi Ara* kutusuna genel ip *önekini* girin. Arama sonuçlarında **Genel IP adresi öneki** göründüğünde, bunu seçin.
3. **Genel IP adresi öneki**altında **Oluştur'u**seçin.
4. **Genel IP adresi öneki oluştur**altında aşağıdaki ayarlar için değerleri girin veya seçin , ardından **Oluştur'u**seçin:

   |Ayar|Gerekli mi?|Ayrıntılar|
   |---|---|---|
   |Abonelik|Evet|Ortak IP adresini ilişkilendirmek istediğiniz kaynakla aynı [abonelikte](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) bulunmanız gerekir.|
   |Kaynak grubu|Evet|Ortak IP adresini ilişkilendirmek istediğiniz [kaynakla](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) aynı veya farklı kaynak grubunda bulunabilir.|
   |Adı|Evet|Ad seçtiğiniz kaynak grubunda benzersiz olmalıdır.|
   |Bölge|Evet|Aralıktan adres ataacağınız ortak IP adresleriyle aynı [bölgede](https://azure.microsoft.com/regions)bulunmalıdır.|
   |Önek boyutu|Evet| İhtiyacınız olan önek boyutu. Bir /28 veya 16 IP adresleri varsayılandır.

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ public-ip öneki oluşturmak](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-create)|
|PowerShell|[Yeni-AzPublicIpPrefix](/powershell/module/az.network/new-azpublicipprefix)|

## <a name="create-a-static-public-ip-address-from-a-prefix"></a>Önek itibaren statik bir genel IP adresi oluşturma
Bir önek oluşturduktan sonra önekten statik IP adresleri oluşturmanız gerekir. Bunu yapmak için aşağıdaki adımları izleyin.

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuya genel ip *adresi öneki*yazın. Arama sonuçlarında **Genel IP adresi önekleri** göründüğünde, bunu seçin.
2. Ortak IP'ler oluşturmak istediğiniz önek'i seçin.
3. Arama sonuçlarında göründüğünde, seçin ve Genel Bakış **bölümündeki +IP adresi ekle'ye** tıklayın.
4. **Genel IP adresi oluştur**altında aşağıdaki ayarlar için değerleri girin veya seçin. Önek Standart SKU, IPv4 ve statik için olduğundan, yalnızca aşağıdaki bilgileri sağlamanız gerekir:

   |Ayar|Gerekli mi?|Ayrıntılar|
   |---|---|---|
   |Adı|Evet|Ortak IP adresinin adı seçtiğiniz kaynak grubunda benzersiz olmalıdır.|
   |Boşta zaman/zaman ası (dakika)|Hayır|Bir TCP veya HTTP bağlantısını, istemcilerin canlı tutma iletileri göndermesine güvenmeden açık tutmanın kaç dakika sı. |
   |DNS ad etiketi|Hayır|Adını oluşturduğunuz Azure bölgesinde (tüm aboneliklerde ve tüm müşterilerde) benzersiz olmalıdır. Azure, ad ve IP adresini DNS'sine otomatik olarak kaydeder, böylece ada sahip bir kaynağa bağlanabilirsiniz. Azure, tam nitelikli DNS adını oluşturmak için sağladığınız ada *location.cloudapp.azure.com* (konum seçtiğiniz konum konum) gibi varsayılan bir alt ağı ekler. Daha fazla bilgi için bkz. Azure [genel IP adresiyle Azure DNS kullan.](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address)|

Alternatif olarak, genel ip adresi kaynağı oluşturmak için aşağıdaki CLI ve PS komutlarını --public-ip-prefix (CLI) ve -PublicIpPrefix (PS) parametreleri ile kullanabilirsiniz. 

|Araç|Komut|
|---|---|
|CLI|[az network public-ip create](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create)|
|PowerShell|[Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress?view=azps-2.0.0)|

## <a name="view-or-delete-a-prefix"></a>Önek görüntüleme veya silme

1. Azure portalının üst kısmındaki metin *Arama kaynaklarını* içeren kutuya genel ip *adresi öneki*yazın. Arama sonuçlarında **Genel IP adresi önekleri** göründüğünde, bunu seçin.
2. Görüntülemek, ayarları değiştirmek veya listeden silmek istediğiniz genel IP adresi önekinin adını seçin.
3. Genel IP adresi önekini görüntülemek, silmek veya değiştirmek isteyip istemediğiniz önemli olmak üzere aşağıdaki seçeneklerden birini tamamlayın.
   - **Görünüm**: **Genel Bakış** bölümü, öneki gibi genel IP adresi önekinin anahtar ayarlarını gösterir.
   - **Sil**: Genel IP adresi önekini silmek için **Genel Bakış** bölümünde **Sil'i** seçin. Önek içindeki adresler genel IP adresi kaynaklarıyla ilişkiliyse, öncelikle ortak IP adresi kaynaklarını silmeniz gerekir. Bkz. [genel bir IP adresini sil.](virtual-network-public-ip-address.md#view-change-settings-for-or-delete-a-public-ip-address)

**Komutlar**

|Araç|Komut|
|---|---|
|CLI|[az ağ public-ip öneki listesi](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-list) genel IP adreslerini listelemek için, [az ağ public-ip öneki](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-show) ayarları göstermek için göster; [az ağ public-ip öneki](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-update) güncelleştirmesi; [az ağ public-ip öneki silmek](/cli/azure/network/public-ip/prefix#az-network-public-ip-prefix-delete) için silme|
|PowerShell|Ortak bir IP adresi nesnesini almak ve ayarlarını görüntülemek için [Get-AzPublicIpPrefix,](/powershell/module/az.network/get-azpublicipprefix) Ayarları güncelleştirmek için [Set-AzPublicIpPrefix;](/powershell/module/az.network/set-azpublicipprefix) [Sil-AzPublicIpPrefix'i](/powershell/module/az.network/remove-azpublicipprefix) sil|

## <a name="permissions"></a>İzinler

Genel IP adresi öneklerinde görevleri gerçekleştirmek için, hesabınızın [ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolüne veya aşağıdaki tabloda listelenen uygun eylemlere atanan [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir role atanması gerekir:

| Eylem                                                            | Adı                                                           |
| ---------                                                         | -------------                                                  |
| Microsoft.Network/publicIPPrefixes/read                           | Genel IP adresi önekini okuma                                |
| Microsoft.Network/publicIPPrefixes/write                          | Herkese açık bir IP adresi öneki oluşturma veya güncelleştirme                    |
| Microsoft.Network/publicIPPrefixes/delete                         | Genel IP adresi önekini silme                              |
|Microsoft.Network/publicIPPrefixes/join/action                     | Önek itibaren genel bir IP adresi oluşturma |

## <a name="next-steps"></a>Sonraki adımlar

- [Genel BIR IP öneki](public-ip-address-prefix.md) kullanmanın senaryoları ve yararları hakkında bilgi edinin
