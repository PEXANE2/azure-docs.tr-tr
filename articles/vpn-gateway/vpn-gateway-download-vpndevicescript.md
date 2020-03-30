---
title: S2S VPN bağlantıları için VPN cihaz yapılandırma komut dosyalarını indirin
description: Bu makale, Azure Kaynak Yöneticisi'ni kullanarak Azure VPN Ağ Geçitleri ile S2S VPN bağlantıları için VPN aygıt yapılandırma komut dosyalarını indirmenize yardımcı olur.
titleSuffix: Azure VPN Gateway
services: vpn-gateway
author: yushwang
manager: rossort
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/09/2019
ms.author: yushwang
ms.openlocfilehash: f905e27f48a0bf9181625bbba07549a13d9420cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162145"
---
# <a name="download-vpn-device-configuration-scripts-for-s2s-vpn-connections"></a>S2S VPN bağlantıları için VPN cihaz yapılandırma komut dosyalarını indirin

Bu makale, Azure Kaynak Yöneticisi'ni kullanarak Azure VPN Ağ Geçitleri ile S2S VPN bağlantıları için VPN aygıt yapılandırma komut dosyalarını indirmenize yardımcı olur. Aşağıdaki diyagram, üst düzey iş akışını gösterir.

![download-script](./media/vpn-gateway-download-vpndevicescript/downloaddevicescript.png)

Aşağıdaki aygıtların kullanılabilir komut dosyaları vardır:

[!INCLUDE [scripts](../../includes/vpn-gateway-device-configuration-scripts.md)]

## <a name="about-vpn-device-configuration-scripts"></a><a name="about"></a>VPN aygıt yapılandırma komut dosyaları hakkında

Tesisler arası VPN bağlantısı, azure VPN ağ geçidi, şirket içi VPN aygıtı ve ikisini birbirine bağlayan bir IPsec S2S VPN tünelinden oluşur. Tipik iş akışı aşağıdaki adımları içerir:

1. Azure VPN ağ geçidi (sanal ağ ağ geçidi) oluşturma ve yapılandırma
2. Şirket içi ağınızı ve VPN aygıtınızı temsil eden bir Azure yerel ağ ağ geçidi oluşturma ve yapılandırma
3. Azure VPN ağ geçidi ile yerel ağ ağ geçidi arasında bir Azure VPN bağlantısı oluşturma ve yapılandırma
4. Azure VPN ağ geçidiile gerçek S2S VPN tünelini oluşturmak için yerel ağ ağ geçidi tarafından temsil edilen şirket içi VPN aygıtını yapılandırın

Azure [portalı](vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)veya [CLI'yi](vpn-gateway-howto-site-to-site-resource-manager-cli.md)kullanarak 1'den 3'e kadar olan adımları tamamlayabilirsiniz. Son adım, şirket içi VPN aygıtlarının Azure dışında yapılandırılmalarını içerir. Bu özellik, Azure VPN ağ geçidinizin, sanal ağınızın ve şirket içi ağ adresi önekleri ve VPN bağlantı özellikleri, vb. ilgili değerleri içeren VPN aygıtınız için bir yapılandırma komut dosyası indirmenize olanak tanır. Komut dosyasını başlangıç noktası olarak kullanabilir veya komut dosyasını yapılandırma konsolu aracılığıyla doğrudan şirket içi VPN aygıtlarınıza uygulayabilirsiniz.

> [!IMPORTANT]
> * Her VPN aygıt yapılandırma komut dosyası için sözdizimi farklıdır ve büyük ölçüde modellere ve firmware sürümlerine bağlıdır. Mevcut şablonlara karşı cihaz modelinize ve sürüm bilgilerinize özel önem verin.
> * Bazı parametre değerleri aygıtta benzersiz olmalıdır ve aygıta erişmeden belirlenemez. Azure tarafından oluşturulan yapılandırma komut dosyaları bu değerleri önceden doldurur, ancak sağlanan değerlerin cihazınızda geçerli olduğundan emin olmanız gerekir. Örnekler için:
>    * Arayüz numaraları
>    * Erişim denetim listesi numaraları
>    * İlke adları veya numaraları, vb.
> * Komut dosyasını uygulamadan önce doğrulamanız gereken parametreleri bulmak için komut dosyasına katıştırılmış "**REPLACE**" anahtar sözcük anahtar sözcüklerini arayın.
> * Bazı şablonlar, yapılandırmaları kaldırmak için uygulayabileceğiniz "**CLEANUP**" bölümünü içerir. Temizleme bölümleri varsayılan olarak yorumlanır.

## <a name="download-the-configuration-script-from-azure-portal"></a>Azure portalından yapılandırma komut dosyasını indirin

Azure VPN ağ geçidi, yerel ağ ağ geçidi ve ikisini birbirine bağlayan bir bağlantı kaynağı oluşturun. Aşağıdaki sayfa adımları size yol göstersin:

* [Azure portalında Siteden Siteye bağlantı oluşturma](vpn-gateway-howto-site-to-site-resource-manager-portal.md)

Bağlantı kaynağı oluşturulduktan sonra, VPN aygıt yapılandırma komut dosyalarını indirmek için aşağıdaki yönergeleri izleyin:

1. Bir tarayıcıdan Azure [portalına](https://portal.azure.com) gidin ve gerekirse Azure hesabınızla oturum açın
2. Oluşturduğunuz bağlantı kaynağına gidin. Tüm bağlantı kaynaklarının listesini "Tüm hizmetler", ardından "NETWORKING" ve "Connections"ı tıklatarak bulabilirsiniz.

    ![bağlantı listesi](./media/vpn-gateway-download-vpndevicescript/connectionlist.png)

3. Yapılandırmak istediğiniz bağlantıyı tıklatın.

    ![bağlantıya genel bakış](./media/vpn-gateway-download-vpndevicescript/connectionoverview.png)

4. Bağlantı genel bakış sayfasında kırmızı renkle vurgulanan "Yapılandırmayı İndir" bağlantısına tıklayın; bu "Yapılandırmayı karşıdan yükleme" sayfasını açar.

    ![indir-komut dosyası-1](./media/vpn-gateway-download-vpndevicescript/downloadscript-1.png)

5. VPN aygıtınız için model ailesini ve firmware sürümünü seçin ve ardından "Yapılandırmayı İndir" düğmesine tıklayın.

    ![download66-script-2](./media/vpn-gateway-download-vpndevicescript/downloadscript-2.PNG)

6. İndirilen komut dosyasını (metin dosyası) tarayıcınızdan kaydetmeniz istenir.
7. Yapılandırma komut dosyasını indirdikten sonra, bir metin düzenleyicisi ile açın ve değiştirilmesi gereken parametreleri tanımlamak ve incelemek için "REPLACE" anahtar sözcükte arama yapın.

    ![edit-script](./media/vpn-gateway-download-vpndevicescript/editscript.png)

## <a name="download-the-configuration-script-using-azure-powershell"></a>Azure PowerShell'i kullanarak yapılandırma komut dosyasını indirin



Aşağıdaki örnekte gösterildiği gibi, Azure PowerShell kullanarak yapılandırma komut dosyasını da indirebilirsiniz:

```azurepowershell-interactive
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite1"

# List the available VPN device models and versions
Get-AzVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration script for the connection
Get-AzVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG -DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="apply-the-configuration-script-to-your-vpn-device"></a>Yapılandırma komut dosyasını VPN cihazınıza uygulayın

Yapılandırma komut dosyasını indirip doğruladıktan sonra, bir sonraki adım komut dosyasını VPN cihazınıza uygulamaktır. Gerçek yordam VPN cihazınızın yapar ve modellerine göre değişir. VPN aygıtlarınız için kullanım kılavuzlarına veya talimat sayfalarına başvurun.

## <a name="next-steps"></a>Sonraki adımlar

[Siteden Siteye bağlantınızı yapılandırmaya](vpn-gateway-howto-site-to-site-resource-manager-portal.md)devam edin.
