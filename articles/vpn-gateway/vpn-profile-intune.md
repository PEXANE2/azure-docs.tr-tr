---
title: Azure VPN istemcileri için bir Intune profili oluşturma
titleSuffix: Azure VPN Gateway
description: Azure VPN istemci profillerini dağıtmak için bir Intune özel profili oluşturmayı öğrenin
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/15/2020
ms.author: cherylmc
ms.openlocfilehash: 7105597ec34e804c2f2b85b01feb4824d63005c5
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578130"
---
# <a name="create-an-intune-profile-to-deploy-vpn-client-profiles"></a>VPN istemci profillerini dağıtmak için bir Intune profili oluşturma

Microsoft Intune kullanarak, Azure VPN istemcileri (Windows 10) için profilleri dağıtabilirsiniz. Bu makale, özel ayarları kullanarak bir Intune profili oluşturmanıza yardımcı olur.

> [!NOTE]
> Bu yöntem yalnızca, istemci kimlik doğrulaması için Azure Active Directory veya ortak bir sertifika kullanan profilleri dağıtmak üzere çalışır. Benzersiz istemci sertifikaları kullanılıyorsa, her kullanıcının Azure VPN Istemcisinde doğru sertifikayı el ile seçmeniz gerekir.
>

## <a name="prerequisites"></a>Önkoşullar

* Cihazlar zaten Intune MDM 'ye kayıtlı.
* Windows 10 için Azure VPN Istemcisi, istemci makinesine zaten dağıtıldı.
* Yalnızca Windows sürümü 19H2 veya üzeri desteklenir.

## <a name="modify-xml"></a><a name="xml"></a>XML 'yi Değiştir

Aşağıdaki adımlarda, Intune için özel bir OMA-URI profili için aşağıdaki ayarlarla örnek bir XML kullanıyoruz:

* Otomatik bağlan
* Güvenilen ağ algılama etkin.

Desteklenen diğer seçenekler için [VPNV2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) makalesine bakın.

1. Azure portal VPN profilini indirin ve *azurevpnconfig.xml* dosyasını paketten ayıklayın.
1. Aşağıdaki metni kopyalayıp yeni bir metin düzenleyici dosyasına yapıştırın.

   ```xml-interactive
    <VPNProfile>
      <!--<EdpModeId>corp.contoso.com</EdpModeId>-->
      <RememberCredentials>true</RememberCredentials>
      <AlwaysOn>true</AlwaysOn>
      <TrustedNetworkDetection>contoso.com,test.corp.contoso.com</TrustedNetworkDetection>
      <DeviceTunnel>false</DeviceTunnel>
      <RegisterDNS>false</RegisterDNS>
      <PluginProfile>
        <ServerUrlList>azuregateway-7cee0077-d553-4323-87df-069c331f58cb-053dd0f6af02.vpn.azure.com</ServerUrlList> 
        <CustomConfiguration>

        </CustomConfiguration>
        <PluginPackageFamilyName>Microsoft.AzureVpn_8wekyb3d8bbwe</PluginPackageFamilyName>
      </PluginProfile>
    </VPNProfile>
   ```
1. İle ve arasındaki girişi ```<ServerUrlList>``` , ```</ServerUrlList>``` indirilen profilinizdeki girdiyle değiştirin (azurevpnconfig.xml). "TrustedNetworkDetection" FQDN 'sini ortamınıza uyacak şekilde değiştirin.
1. Azure indirilen profilini açın (azurevpnconfig.xml) ve metni vurgulayarak ve (Ctrl) + C tuşlarına basarak tüm içeriği panoya kopyalayın. 
1. Önceki adımdaki kopyalanmış metni Etiketler arasında 2. adımda oluşturduğunuz dosyaya yapıştırın ```<CustomConfiguration>  </CustomConfiguration>``` . Dosyayı bir xml uzantısıyla kaydedin.
1. Etiketlerin içindeki değeri yazın ```<name>  </name>``` . Bu, profilin adıdır. Profili Intune 'da oluştururken bu ada ihtiyacınız olacaktır. Dosyayı kapatın ve kaydedildiği konumu hatırlayın.

## <a name="create-intune-profile"></a>Intune profili oluşturma

Bu bölümde, özel ayarlarla bir Microsoft Intune profili oluşturursunuz.

1. Intune 'da oturum açın ve **cihazlar-> yapılandırma profilleri**' ne gidin. **+ Profil oluştur**' u seçin.

   :::image type="content" source="./media/create-profile-intune/configuration-profile.png" alt-text="Yapılandırma profilleri":::
1. **Platform** olarak **Windows 10 ve üzeri**'ni seçin. **Profil**için **özel**' i seçin. Ardından **Oluştur**’u seçin.
1. Profile bir ad ve açıklama verin, ardından **İleri**' yi seçin.
1. **Yapılandırma ayarları** sekmesinde, **Ekle**' yi seçin.

    * **Ad:** Yapılandırma için bir ad girin.
    * **Açıklama:** İsteğe bağlı açıklama.
    * **OMA-URI:** ```./User/Vendor/MSFT/VPNv2/<name of your connection>/ProfileXML``` (Bu bilgiler <name> </name> etiketteki azurevpnconfig.xml dosyasında bulunabilir).
    * **Veri türü:** Dize (XML dosyası).

   Klasör simgesini seçin ve [XML](#xml) adımlarında adım 6 ' da kaydettiğiniz dosyayı seçin. **Add (Ekle)** seçeneğini belirleyin.

   :::image type="content" source="./media/create-profile-intune/configuration-settings.png" alt-text="Yapılandırma profilleri" lightbox="./media/create-profile-intune/configuration-settings.png":::
1. **İleri**’yi seçin.
1. **Atamalar**' ın altında, yapılandırmayı göndermek istediğiniz grubu seçin. Ardından **İleri**' yi seçin.
1. Uygulanabilirlik kuralları isteğe bağlıdır. Gerekirse herhangi bir kural tanımlayın ve ardından **İleri**' yi seçin.
1. **Gözden geçir + oluştur** sayfasında **Oluştur**' u seçin.

    :::image type="content" source="./media/create-profile-intune/create-profile.png" alt-text="Yapılandırma profilleri":::
1. Özel profiliniz artık oluşturuldu. Bu profili dağıtmaya yönelik Microsoft Intune adımlar için bkz. [Kullanıcı ve cihaz profilleri atama](https://docs.microsoft.com/mem/intune/configuration/device-profile-assign).
 
## <a name="next-steps"></a>Sonraki adımlar

Noktadan siteye hakkında daha fazla bilgi için bkz. [Noktadan siteye hakkında](point-to-site-about.md).
