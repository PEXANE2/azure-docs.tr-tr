---
title: IPsec tünelini yeniden kurmak için Azure VPN ağ geçidini sıfırlama
description: Bu makale, IPsec tünellerini yeniden kurmak için Azure VPN Ağ Geçidinizi sıfırlamanızda size yol açar. Makale, hem klasik hem de Kaynak Yöneticisi dağıtım modellerinde VPN ağ geçitleri için geçerlidir.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/09/2020
ms.author: cherylmc
ms.openlocfilehash: e3a5807a0ccfa39cc80acacedaa5fb4d3afaaed3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244426"
---
# <a name="reset-a-vpn-gateway"></a>VPN Gateway sıfırlama

Bir veya daha fazla Siteden Siteye VPN tünelinde şirketler arası VPN bağlantısını kaybederseniz bir Azure VPN ağ geçidinin sıfırlanması yararlıdır. Bu durumda şirket içi VPN cihazlarınızın tümü düzgün çalışır, ancak Azure VPN ağ geçitleriyle IPsec tünelleri kuramaz. Bu makale, VPN ağ geçidinizi sıfırlamanıza yardımcı olur.

### <a name="what-happens-during-a-reset"></a>Sıfırlama sırasında ne olur?

VPN ağ geçidi, etkin bekleme yapılandırmasında çalışan iki VM örneğinden oluşur. Ağ geçidini sıkarken, ağ geçidini yeniden başlatır ve daha sonra bu ağ geçidine çapraz bina yapılandırmalarını yeniden uygular. Ağ geçidi, sahip olduğu genel IP adresini tutar. Bu durum VPN yönlendirici yapılandırmasını Azure VPN ağ geçidi için yeni bir ortak IP adresi ile güncelleştirmenizin gerekli olmadığı anlamına gelir.

Ağ geçidini sıfırlama komutunu verdiyseniz, Azure VPN ağ geçidinin geçerli etkin örneği hemen yeniden başlatılır. Etkin örnekten (yeniden başlatılıyor) arıza sırasında bekleme örneğine kısa bir boşluk olacaktır. Boşluk bir dakikadan az olmalıdır.

İlk yeniden başlatma sonrasında bağlantı geri kazanılmazsa ikinci sanal makine örneğini (yeni etkin ağ geçidi) yeniden başlatmak için aynı komutu yeniden verin. Arka arkaya iki yeniden başlatma istenirse her iki sanal makine örneğinin (etkin ve bekleme) yeniden başlatılma süresi biraz daha uzun olur. Bu, VPN'lerin yeniden başlatmaları tamamlaması için 30 ila 45 dakikaya kadar VPN bağlantısında daha uzun bir boşluğa neden olur.

İki yeniden başlatmadan sonra, hala binalar arası bağlantı sorunları yaşıyorsanız, lütfen Azure portalından bir destek isteği açın.

## <a name="before-you-begin"></a><a name="before"></a>Başlamadan önce

Ağ geçidinizi sıfırlamadan önce her bir IPsec Siteden Siteye (S2S) VPN tüneli için aşağıda listelenen anahtar öğeleri doğrulayın. Öğelerdeki uyuşmazlık S2S VPN tünellerinin bağlantısının kesilmesiyle sonuçlanır. Şirket içi ve Azure VPN ağ geçitleriniz için yapılandırmaları doğrulamak ve düzeltmek, sizi ağ geçitlerinde çalışan diğer bağlantılar için gereksiz yeniden başlatmalardan ve kesintilerden kurtarır.

Ağ geçidinizi sıfırlamadan önce aşağıdaki öğeleri doğrulayın:

* Hem Azure VPN ağ geçidi hem de şirket içi VPN ağ geçidi için İnternet IP adresleri (VIP) hem Azure hem de şirket içi VPN ilkelerinde doğru şekilde yapılandırılmıştır.
* Önceden paylaşılan anahtar Azure ve şirket içi VPN ağ geçitlerinde aynı olmalıdır.
* Şifreleme, karma algoritmaları ve PFS (Kusursuz İletme Gizliliği) gibi belirli IPsec/IKE yapılandırmalarını uygularsanız hem Azure hem de şirket içi VPN ağ geçitlerinin aynı yapılandırmalara sahip olduğundan emin olun.

## <a name="azure-portal"></a><a name="portal"></a>Azure portalında

Azure portalını kullanarak Bir Kaynak Yöneticisi VPN ağ geçidini sıfırlayabilirsiniz. Klasik bir ağ geçidini sıfırlamak istiyorsanız [PowerShell](#resetclassic) adımlarına bakın.

### <a name="resource-manager-deployment-model"></a>Resource Manager dağıtım modeli

1. Azure [portalını](https://portal.azure.com) açın ve sıfırlamak istediğiniz Kaynak Yöneticisi sanal ağ ağ geçidine gidin.
2. Sanal ağ ağ geçidi için bıçak üzerinde , 'Sıfırla'yı tıklatın.

   ![VPN Ağ Geçidi bıçağını sıfırla](./media/vpn-gateway-howto-reset-gateway/reset-vpn-gateway-portal.png)
3. Sıfırla çubuğunda, **Sıfırla** düğmesini tıklatın.

## <a name="powershell"></a><a name="ps"></a>Powershell

### <a name="resource-manager-deployment-model"></a>Resource Manager dağıtım modeli

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir ağ geçidisıfırlama küçlükiçin cmdlet **Sıfırlama-AzVirtualNetworkGateway**olduğunu. Sıfırlama yapmadan önce [PowerShell Az cmdlets'in](https://docs.microsoft.com/powershell/module/az.network)en son sürümüne sahip olduğunuzdan emin olun. Aşağıdaki örnek, TestRG1 kaynak grubunda VNet1GW adlı bir sanal ağ ağ ağ geçidini sıfırlar:

```powershell
$gw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Sonuç:

Bir iade sonucu aldığınızda, ağ geçidi sıfırlamanın başarılı olduğunu varsayabilirsiniz. Ancak, geri dönüş sonucu açıkça sıfırlama başarılı olduğunu gösteren hiçbir şey yoktur. Ağ geçidi sıfırlamanın tam olarak ne zaman oluştuğunu görmek için geçmişe yakından bakmak istiyorsanız, bu bilgileri [Azure portalında](https://portal.azure.com)görüntüleyebilirsiniz. Portalda, **'GatewayName' -> Kaynak Sağlığı'na**gidin.

### <a name="classic-deployment-model"></a><a name="resetclassic"></a>Klasik dağıtım modeli

Bir ağ geçidini sıfırlamak için cmdlet **Sıfırlama-AzureVNetAğ Ağ Geçidi'** dir. Hizmet Yönetimi için Azure PowerShell cmdlets masaüstünüze yerel olarak yüklenmelidir. Azure Bulut Su Şur'u kullanamazsınız. Sıfırlama gerçekleştirmeden önce, [Service Management (SM) PowerShell cmdlets'in](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0#azure-service-management-cmdlets)en son sürümüne sahip olduğunuzdan emin olun. Bu komutu kullanırken, sanal ağın tam adını kullandığınızdan emin olun. Portal kullanılarak oluşturulan klasik VNets PowerShell için gerekli olan uzun bir adı var. 'Get-AzureVNetConfig -ExportToFile C:\Myfoldername\NetworkConfig.xml' kullanarak uzun adı görüntüleyebilirsiniz.

Aşağıdaki örnek, "Grup TestRG1 TestVNet1" (portalda basitçe "TestVNet1" olarak gösterir) adlı sanal bir ağ için ağ geçidini sıfırlar:

```powershell
Reset-AzureVNetGateway –VnetName 'Group TestRG1 TestVNet1'
```

Sonuç:

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

## <a name="azure-cli"></a><a name="cli"></a>Azure CLI

Ağ geçidini sıfırlamak için [az ağı vnet ağ geçidi sıfırlama](https://docs.microsoft.com/cli/azure/network/vnet-gateway) komutunu kullanın. Aşağıdaki örnek, TestRG5 kaynak grubunda VNet5GW adlı bir sanal ağ ağ ağ geçidini sıfırlar:

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Sonuç:

Bir iade sonucu aldığınızda, ağ geçidi sıfırlamanın başarılı olduğunu varsayabilirsiniz. Ancak, geri dönüş sonucu açıkça sıfırlama başarılı olduğunu gösteren hiçbir şey yoktur. Ağ geçidi sıfırlamanın tam olarak ne zaman oluştuğunu görmek için geçmişe yakından bakmak istiyorsanız, bu bilgileri [Azure portalında](https://portal.azure.com)görüntüleyebilirsiniz. Portalda, **'GatewayName' -> Kaynak Sağlığı'na**gidin.
