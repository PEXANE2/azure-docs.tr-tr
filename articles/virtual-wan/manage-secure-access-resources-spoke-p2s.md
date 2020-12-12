---
title: P2S istemcileri için bağlı olan sanal ağlarda bulunan kaynaklara güvenli erişimi yönetme
titleSuffix: Azure Virtual WAN
description: Bu makale, kullanıcı VPN (Noktadan siteye) istemcileri için sanal ağlara güvenli erişimi yönetmek üzere Azure sanal WAN ve Azure Güvenlik duvarı kurallarını kullanmanıza yardımcı olur.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 12/11/2020
ms.author: cherylmc
ms.openlocfilehash: b0937bbd72460b1d46ce0394af1933e858424966
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97360331"
---
# <a name="manage-secure-access-to-resources-in-spoke-vnets-for-user-vpn-clients"></a>Kullanıcı VPN istemcileri için bağlı olan sanal ağlarda bulunan kaynaklara güvenli erişimi yönetme

Bu makalede, sanal WAN ve Azure Güvenlik Duvarı kurallarının ve filtrelerinin Azure 'da Noktadan siteye Ikev2 'e veya açık VPN bağlantılarına yönelik güvenli erişimi yönetmek için nasıl kullanılacağı gösterilmektedir. Bu yapılandırma, Azure kaynaklarına erişimi kısıtlamak istediğiniz uzak kullanıcılarınız veya Azure 'da kaynaklarınızın güvenliğini sağlamak için yararlıdır.

Bu makaledeki adımlar, kullanıcı VPN istemcilerinin sanal hub 'a bağlı olan bir bağlı olan VNet 'teki belirli bir kaynağa (VM1) erişmesine izin vermek için aşağıdaki diyagramda mimariyi oluşturmanıza yardımcı olur (VM2). Bu mimari örneği temel bir kılavuz olarak kullanın.

:::image type="content" source="./media/manage-secure-access-resources-spoke-p2s/diagram.png" alt-text="Diyagram: güvenli sanal hub" :::

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Prerequisites](../../includes/virtual-wan-before-include.md)]

* Kullanmak istediğiniz kimlik doğrulama yapılandırması için kullanılabilir değerler vardır. Örneğin, bir RADIUS sunucusu, Azure Active Directory kimlik doğrulaması veya [sertifika oluşturma ve dışarı aktarma](../vpn-gateway/vpn-gateway-certificates-point-to-site.md).

## <a name="create-a-virtual-wan"></a>Sanal WAN oluşturma

[!INCLUDE [Create virtual WAN](../../includes/virtual-wan-create-vwan-include.md)]

## <a name="define-p2s-configuration-parameters"></a><a name= "p2s-config"></a>P2S yapılandırma parametrelerini tanımlayın

Noktadan siteye (P2S) yapılandırması, uzak istemcileri bağlamaya yönelik parametreleri tanımlar. Bu bölüm, P2S yapılandırma parametreleri tanımlamanıza yardımcı olur ve ardından VPN istemci profili için kullanılacak yapılandırmayı oluşturur. İzleyeceğiniz yönergeler, kullanmak istediğiniz kimlik doğrulama yöntemine bağlıdır.

### <a name="authentication-methods"></a>Kimlik doğrulama yöntemleri

Kimlik doğrulama yöntemini seçerken üç seçeneğiniz vardır. Her yöntemin belirli gereksinimleri vardır. Aşağıdaki yöntemlerden birini seçin ve ardından adımları uygulayın.

* **Azure Active Directory kimlik doğrulaması:** Aşağıdakileri edinin:

   * Azure AD kiracınızda kayıtlı Azure VPN kurumsal uygulamasının **uygulama kimliği** .
   * **Veren**. Örnek: `https://sts.windows.net/your-Directory-ID`.
   * **Azure AD kiracısı**. Örnek: `https://login.microsoftonline.com/your-Directory-ID`.

* **RADIUS tabanlı kimlik doğrulaması:** RADIUS sunucusu IP 'sini, RADIUS sunucu gizli anahtarını ve sertifika bilgilerini alın.

* **Azure sertifikaları:** Bu yapılandırma için sertifikalar gereklidir. Sertifika oluşturmanız veya edinmeniz gerekir. Her istemci için bir istemci sertifikası gereklidir. Ayrıca, kök sertifika bilgilerinin (ortak anahtar) yüklenmesi gerekir. Gerekli sertifikalar hakkında daha fazla bilgi için bkz. [sertifikaları oluşturma ve dışarı aktarma](../vpn-gateway/vpn-gateway-certificates-point-to-site.md).

Aşağıdaki örnekte Azure sertifika kimlik doğrulaması gösterilmektedir.

[!INCLUDE [Define parameters](../../includes/virtual-wan-p2s-configuration-include.md)]

## <a name="create-the-hub-and-gateway"></a><a name="hub"></a>Hub ve ağ geçidini oluşturma

Bu bölümde, sanal hub 'ı Noktadan siteye ağ geçidi ile oluşturursunuz. Yapılandırırken, aşağıdaki örnek değerleri kullanabilirsiniz:

* **Hub özel IP adresi alanı:** 10.0.0.0/24
* **İstemci adres havuzu:** 10.5.0.0/16
* **Özel DNS sunucuları:** En fazla 5 DNS sunucusu listeleyebilirsiniz

[!INCLUDE [Create hub](../../includes/virtual-wan-p2s-hub-include.md)]

## <a name="generate-vpn-client-configuration-files"></a><a name="generate"></a>VPN istemcisi yapılandırma dosyaları oluştur

Bu bölümde, yapılandırma profili dosyalarını oluşturur ve indirirler. Bu dosyalar, istemci bilgisayarda yerel VPN istemcisini yapılandırmak için kullanılır. İstemci profili dosyalarının içeriği hakkında daha fazla bilgi için bkz. [Noktadan siteye yapılandırma-sertifikalar](../vpn-gateway/point-to-site-vpn-client-configuration-azure-cert.md).

[!INCLUDE [Download profile](../../includes/virtual-wan-p2s-download-profile-include.md)]

## <a name="configure-vpn-clients"></a><a name="clients"></a>VPN istemcilerini yapılandırma

Uzak erişim istemcilerini yapılandırmak için indirilen profili kullanın. Her işletim sistemi için yordam farklıdır, sisteminize uygulanan yönergeleri izleyin.

[!INCLUDE [Configure clients](../../includes/virtual-wan-p2s-configure-clients-include.md)]

## <a name="connect-the-spoke-vnet"></a><a name="connect-spoke"></a>Bağlı olan VNet 'i bağlama

Bu bölümde, bağlı olan sanal ağını sanal WAN hub 'ına iliştirebilirsiniz.

[!INCLUDE [Connect spoke virtual network](../../includes/virtual-wan-connect-vnet-hub-include.md)]

## <a name="create-virtual-machines"></a><a name="create-vm"></a>Sanal makineler oluşturma

Bu bölümde, VNet 'iniz, VM1 ve VM2 ' de iki VM oluşturursunuz. Ağ diyagramında 10.18.0.4 ve 10.18.0.5 kullanırız. VM 'lerinizi yapılandırırken oluşturduğunuz sanal ağı (Ağ sekmesinde bulunur) seçtiğinizden emin olun. VM oluşturma adımları için bkz. [hızlı başlangıç: VM oluşturma](../virtual-machines/windows/quick-create-portal.md).

## <a name="secure-the-virtual-hub"></a><a name="secure"></a>Sanal hub güvenliğini sağlama

Standart bir sanal hub, bağlı olan sanal ağlardaki kaynakları korumak için yerleşik güvenlik ilkelerine sahip değildir. Güvenli bir sanal hub, Azure 'da kaynaklarınızı korumak üzere gelen ve giden trafiği yönetmek için Azure Güvenlik Duvarı veya bir üçüncü taraf sağlayıcı kullanır.

Aşağıdaki makaleyi kullanarak hub 'ı güvenli bir hub 'a dönüştürün: [bir sanal WAN hub 'ında Azure Güvenlik duvarını yapılandırma](howto-firewall.md).

## <a name="create-rules-to-manage-and-filter-traffic"></a><a name= "create-rules"></a> Trafiği yönetmek ve filtrelemek için kurallar oluşturma

Azure Güvenlik duvarının davranışını gösteren kurallar oluşturun. Hub 'ı güvenli hale getirerek, sanal hub 'ı girdiğiniz tüm paketlerin Azure kaynaklarınıza erişmeden önce güvenlik duvarı işlemeye tabi olduğundan emin veriyoruz.

Bu adımları tamamladıktan sonra, VPN kullanıcılarının 10.18.0.4 özel IP adresi ile VM **'ye erişmesine izin veren ancak özel** IP adresi 10.18.0.5 olan VM 'ye erişim izni veren bir mimari oluşturmuş olursunuz.

1. Azure portal, **güvenlik duvarı Yöneticisi**' ne gidin.
1. Güvenlik altında **Azure Güvenlik Duvarı ilkeleri**' ni seçin.
1. **Azure Güvenlik Duvarı Ilkesi oluştur**' u seçin.
1. **İlke ayrıntıları**' nın altına bir ad yazın ve sanal hub 'ınızın dağıtıldığı bölgeyi seçin.
1. **İleri ' yi seçin: DNS ayarları (Önizleme)**.
1. **Sonraki: kurallar**' ı seçin.
1. **Kurallar** sekmesinde **kural koleksiyonu Ekle**' yi seçin.
1. Koleksiyon için bir ad sağlayın. Türü **ağ** olarak ayarlayın. **100** öncelik değerini ekleyin.
1. Aşağıdaki örnekte gösterildiği gibi kural, kaynak türü, kaynak, protokol, hedef bağlantı noktası ve hedef türü adını girin. Ardından **Ekle**' yi seçin. Bu kural, VPN istemci havuzundaki herhangi bir IP adresinin 10.18.04 özel IP adresi olan VM 'ye erişmesini sağlar, ancak sanal hub 'a bağlı başka herhangi bir kaynak değildir. İstediğiniz mimari ve izin kurallarına uyacak şekilde istediğiniz kuralları oluşturun.

   :::image type="content" source="./media/manage-secure-access-resources-spoke-p2s/rules.png" alt-text="Güvenlik duvarı kuralları" :::

1. **İleri ' yi seçin: tehdit bilgileri**.
1. Ileri 'yi seçin **: hub**.
1. **Hub 'lar** sekmesinde **sanal hub 'ları ilişkilendir**' i seçin.
1. Daha önce oluşturduğunuz sanal hub 'ı seçin ve ardından **Ekle**' yi seçin.
1. **Gözden geçir ve oluştur**’u seçin.
1. **Oluştur**’u seçin.

Bu işlemin tamamlanması 5 dakika veya daha fazla sürebilir.

## <a name="route-traffic-through-azure-firewall"></a><a name="send"></a>Azure Güvenlik Duvarı aracılığıyla trafiği yönlendirme

Bu bölümde, trafiğin Azure Güvenlik Duvarı üzerinden yönlendirildiğinden emin olmanız gerekir.

1. Portalda, **güvenlik duvarı Yöneticisi**' nden **güvenli sanal hub 'lar**' ı seçin.
1. Oluşturduğunuz sanal hub 'ı seçin.
1. **Ayarlar** altında **Güvenlik Yapılandırması**' nı seçin.
1. **Özel trafik** altında **Azure Güvenlik Duvarı aracılığıyla gönder**' i seçin.
1. VNet bağlantısının ve şube bağlantısı özel trafiğinin Azure Güvenlik Duvarı tarafından güvenli olduğunu doğrulayın.
1. **Kaydet**’i seçin.

## <a name="validate"></a><a name="validate"></a>Doğrulama

Güvenli hub 'ınızın kurulumunu doğrulayın.

1. İstemci cihazınızdan VPN aracılığıyla **güvenli sanal hub** 'a bağlanın.
1. **10.18.0.4** IP adresini istemcinizden ping yapın. Yanıt görmeniz gerekir.
1. **10.18.0.5** IP adresini istemcinizden ping yapın. Yanıt görmeyebilirsiniz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Güvenli sanal hub 'daki **etkin rotalar tablosunun** , güvenlik duvarı tarafından özel trafik için bir sonraki atlamaya sahip olduğundan emin olun. Etkin rotalar tablosuna erişmek için **sanal hub** kaynağınız ' ne gidin. **Bağlantı** altında, **yönlendirme**' yi seçin ve ardından **geçerli rotalar**' ı seçin. Buradan, **varsayılan** yol tablosunu seçin.
* Kurallar [Oluştur](#create-rules) bölümünde kurallar oluşturduğunuza emin olun. Bu adımlar kaçırıldığında, oluşturduğunuz kurallar gerçekten hub ile ilişkilendirilmeyecektir ve yol tablosu ve paket akışı Azure Güvenlik duvarını kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [sanal WAN hakkında SSS](virtual-wan-faq.md).
* Azure Güvenlik Duvarı hakkında daha fazla bilgi için bkz. [Azure Güvenlik DUVARı SSS](../firewall/firewall-faq.md).
