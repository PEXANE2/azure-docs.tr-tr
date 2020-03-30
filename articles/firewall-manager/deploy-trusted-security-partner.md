---
title: Azure Güvenlik Duvarı Yöneticisi güvenilen bir güvenlik iş ortağı dağıtma
description: Azure portalını kullanarak Azure Güvenlik Duvarı Yöneticisi güvenilen bir güvenliği nasıl dağıttığınızı öğrenin.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: victorh
ms.openlocfilehash: bcea9a8674e4b1979698b7d28eb4192172b0dc11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73931314"
---
# <a name="deploy-a-trusted-security-partner-preview"></a>Güvenilen güvenlik iş ortağı dağıtma (önizleme)

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Azure Güvenlik Duvarı Yöneticisi'ndeki *güvenilir güvenlik ortakları,* kullanıcılarınız için Internet erişimini korumak için tanıdık, en iyi nesil üçüncü taraf güvenlik (SECaaS) tekliflerinizi kullanmanıza olanak tanır.

Desteklenen senaryolar ve en iyi uygulama yönergeleri hakkında daha fazla bilgi edinmek için [bkz.](trusted-security-partners.md)

Desteklenen güvenlik ortakları **ZScaler** ve bu önizleme için **iboss** vardır. Desteklenen bölgeler WestCentralUS, NorthCentralUS, WestUS, WestUS2 ve EastUS'dur.

## <a name="prerequisites"></a>Ön koşullar

> [!IMPORTANT]
> Azure Güvenlik Duvarı Yöneticisi Önizlemesi, `Register-AzProviderFeature` PowerShell komutu kullanılarak açıkça etkinleştirilmelidir.

PowerShell komut isteminden aşağıdaki komutları çalıştırın:

```azure-powershell
connect-azaccount
Register-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network
```
Özellik kaydının tamamlanması 30 dakika kadar sürer. Kayıt durumunuzu kontrol etmek için aşağıdaki komutu çalıştırın:

`Get-AzProviderFeature -FeatureName AllowCortexSecurity -ProviderNamespace Microsoft.Network`

## <a name="deploy-a-third-party-security-provider-in-a-new-hub"></a>Yeni bir hub'da bir üçüncü taraf güvenlik sağlayıcısını dağıtma

1. https://portal.azure.com adresinden Azure portalında oturum açın.
2. Arama'da, **Güvenlik Duvarı Yöneticisi** yazın ve **Hizmetler**altında seçin. **Search**
3. **Başlarken**gidin. **Güvenli Sanal Hub Oluştur'u**seçin. 
4. Abonelik ve kaynak grubunu girin, desteklenen bir bölge seçin ve hub ve sanal WAN bilgilerinizi ekleyin. 
5. **VPN ağ geçidi dağıtma** varsayılan olarak etkinleştirilir. Hub'da güvenilir bir güvenlik ortağı dağıtmak için VPN Ağ Geçidi gerekir. 
6. **Sonrakini Seçin: Azure Güvenlik Duvarı**
   > [!NOTE]
   > Güvenilir güvenlik ortakları VPN Ağ Geçidi tünellerini kullanarak hub'ınıza bağlanır. VPN Ağ Geçidi'ni silerseniz, Güvenilen güvenlik ortaklarınızın bağlantıları kaybolur.
7. Internet trafiğini filtrelemek için üçüncü taraf hizmet sağlayıcısıyla birlikte özel trafiği filtrelemek için Azure Güvenlik Duvarı'nı dağıtmak istiyorsanız, Azure Güvenlik Duvarı için bir ilke seçin. Desteklenen [senaryolara](trusted-security-partners.md#key-scenarios)bakın.
8. Hub'da yalnızca bir üçüncü taraf güvenlik sağlayıcısı dağıtmak istiyorsanız, Azure Güvenlik Duvarı: **Devre Dışı Bırakılmış**olarak ayarlamak için **Etkin/Devre Dışı'** yı seçin. 
9. **Sonraki'ni Seçin: Güvenilir Güvenlik Ortakları.**
10. **Etkin**olarak ayarlamak için **Güvenilir Güvenlik Ortağı'nı** seçin. Bir ortak seçin. 
11. **Sonraki'ni**seçin. 
12. İçeriği gözden geçirin ve ardından **Oluştur'u**seçin.

VPN ağ geçidi dağıtımı 30 dakikadan fazla sürebilir.

Hub'ın oluşturulduğunu doğrulamak için Azure Güvenlik Duvarı Yöneticisi >Güvenli Hub'lara gidin. İş ortağının adını ve bekleyen **Güvenlik Bağlantısı**durumunu göstermek için hub >Genel Bakış sayfasını seçin.

Hub oluşturulduktan ve güvenlik ortağı kurulduktan sonra, güvenlik sağlayıcısını hub'a bağlamaya devam edin.

## <a name="deploy-a-third-party-security-provider-in-an-existing-hub"></a>Varolan bir hub'da bir üçüncü taraf güvenlik sağlayıcısıdağıtma

Ayrıca Sanal WAN'da varolan bir hub seçebilir ve bunu güvenli bir *sanal hub'a*dönüştürebilirsiniz.

1. **Başlarken,** **Varolan Hub'ları Dönüştür'ü**seçin.
2. Bir abonelik ve varolan bir hub seçin. Bir üçüncü taraf sağlayıcıyı yeni bir hub'a dağıtmak için diğer adımları izleyin.

Varolan bir hub'ı üçüncü taraf sağlayıcılarla güvenli hub'a dönüştürmek için vpn ağ geçidinin dağıtılması gerektiğini unutmayın.

## <a name="configure-third-party-security-providers-to-connect-to-a-secured-hub"></a>Güvenli bir hub'a bağlanmak için üçüncü taraf güvenlik sağlayıcılarını yapılandırma

Sanal hub'ınızın VPN Ağ Geçidi'ne tüneller ayarlamak için üçüncü taraf sağlayıcıların hub'ınızın erişim haklarına ihtiyacı vardır. Bunu yapmak için, bir hizmet müdürünü aboneliğiniz veya kaynak grubunuzla ilişkilendirin ve erişim hakları tanıyın. Daha sonra bu kimlik bilgilerini portallarını kullanarak üçüncü tarafa vermeniz gerekir.

1. Azure Etkin Dizin (AD) hizmet ilkesi oluşturun: Yönlendirme URL'sini atlayabilirsiniz. 

   [Nasıl olunur: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)
2. Hizmet ilkesi için erişim hakları ve kapsam ekleyin.
   [Nasıl olunur: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)

   > [!NOTE]
   > Daha ayrıntılı denetim için erişimi yalnızca kaynak grubunuzla sınırlandırabilirsiniz.
3. [ZScaler'i izleyin: Microsoft Azure Sanal WAN Tümleştirme yönergelerini](https://help.zscaler.com/zia/configuring-microsoft-azure-virtual-wan-integration) şu şekilde yapılandırmak:

   - İş ortağı portalında oturum açın ve güvenilir iş ortağınıza güvenli hub'ınıza erişim sağlamak için kimlik bilgilerinizi ekleyin.
   - İş ortağı portalındaki sanal hub'ları eşitle ve tüneli sanal hub'a ayarlayın. Azure AD kimlik doğrulama kimlik bilgileriniz doğrulandıktan sonra bunu yapabilirsiniz.
   
4. Azure'daki Azure Sanal WAN portalında tünel oluşturma durumuna bakabilirsiniz. Tüneller hem Azure'da hem de iş ortağı portalında **bağlı** olarak gösterildikten sonra, hangi şubelerin ve VNet'lerin iş ortağına Internet trafiği göndermesi gerektiğini seçmek için rotalar ayarlamak için sonraki adımlara devam edin.

## <a name="configure-route-settings"></a>Rota ayarlarını yapılandırma

1. Azure Güvenlik Duvarı Yöneticisine (> Güvenli Hub'lara göz atın. 
2. Bir hub seçin. Hub durumu artık Bekleyen Güvenlik **Bağlantısı**yerine **Sağlanan** gösterilmelidir.

   Üçüncü taraf sağlayıcının hub'a bağlanabilmesini sağlayın. VPN ağ geçidindeki tüneller **Bağlı** durumda olmalıdır. Bu durum, hub ve üçüncü taraf iş ortağı arasındaki bağlantı durumunu önceki duruma göre daha fazla yansıtır.
3. Hub'ı seçin ve **Rota Ayarları'na**gidin.

   Hub'a bir üçüncü taraf sağlayıcı dağıttığınızda, hub'ı güvenli bir *sanal hub'a*dönüştürür. Bu, üçüncü taraf sağlayıcının hub'a 0.0.0.0/0 (varsayılan) bir rota nın reklamını yaptığını sağlar. Ancak, hangi bağlantıların bu varsayılan rotayı alması gerektiğini kabul etmedikçe, VNet bağlantıları ve hub'a bağlı siteler bu rotayı almaz.
4. **Internet trafiği**altında, **VNet-to-Internet** veya **Branch-to-Internet'i** seçin veya her ikisi de rotaların üçüncü taraf üzerinden gönderilen yapılandırıldığını seçin.

   Bu yalnızca hub'a hangi trafik türünün yönlendirilmesi gerektiğini gösterir, ancak henüz VNet'lerde veya dallardaki yolları etkilemez. Bu yollar varsayılan olarak hub'a bağlı tüm VNet/dallara yayılmaz.
5. **Güvenli bağlantıları** seçmeniz ve bu yolların ayarlandığı bağlantıları seçmeniz gerekir. Bu, hangi VNet'lerin/şubelerin üçüncü taraf sağlayıcıya Internet trafiği göndermeye başlayıp başlatabildiği anlamına gelen bir durumdur.
6. **Rota ayarlarından,** Internet trafiği altında **Güvenli bağlantıları** seçin ve ardından güvenli hale gelmek için VNet veya şubeleri (Virtual WAN'daki*siteler)* seçin. **Güvenli Internet trafiğini**seçin.
   ![Güvenli Internet trafiği](media/deploy-trusted-security-partner/secure-internet-traffic.png)
7. Hub'lar sayfasına geri gidin. Hub'ın **Güvenilen güvenlik ortağı** durumu artık **Güvenli**olmalıdır.

## <a name="branch-or-vnet-internet-traffic-via-third-party-service"></a>Üçüncü taraf hizmeti ile Şube veya VNet Internet trafiği

Ardından, VNet sanal makinelerinin veya şube sitesinin Internet'e erişip erişemediğini kontrol edebilir ve trafiğin üçüncü taraf hizmetine aktığını doğrulayabilirsiniz.

Rota belirleme adımlarını tamamladıktan sonra, VNet sanal makineleri ve şube siteleri üçüncü taraf servis yoluna 0/0 gönderilir. Bu sanal makinelere RDP veya SSH yapamazsınız. Oturum açabilmek için [Azure Bastion](../bastion/bastion-overview.md) hizmetini eşlenmiş bir VNet'te dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Bulut ağınızı Azure Güvenlik Duvarı Yöneticisi Önizlemeile güvenli hale](secure-cloud-network.md)




