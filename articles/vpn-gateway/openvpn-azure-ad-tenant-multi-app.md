---
title: 'VPN Gateway: farklı Kullanıcı grupları için Azure AD kiracısı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulaması kullanarak sanal ağınıza bağlanmak için P2S VPN kullanabilirsiniz
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: cherylmc
ms.openlocfilehash: 1305ca603aef63dafcc7b055d55e3f0fe281f4fc
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91819681"
---
# <a name="create-an-active-directory-ad-tenant-for-p2s-openvpn-protocol-connections"></a>P2S OpenVPN Protokolü bağlantıları için Active Directory (AD) kiracısı oluşturma

Sanal ağınıza bağlanırken sertifika tabanlı kimlik doğrulama veya RADIUS kimlik doğrulaması kullanabilirsiniz. Ancak, açık VPN protokolünü kullandığınızda Azure Active Directory kimlik doğrulamasını da kullanabilirsiniz. Farklı kullanıcı kümesinin farklı VPN ağ geçitlerine bağlanmasını istiyorsanız, AD 'de birden çok uygulamayı kaydedebilir ve bunları farklı VPN ağ geçitlerine bağlayabilirsiniz. Bu makale, P2S OpenVPN kimlik doğrulaması için bir Azure AD kiracısı ayarlamanıza ve farklı kullanıcılar ve gruplar için farklı erişime izin vermek üzere Azure AD 'de birden çok uygulama oluşturup kaydetmenize yardımcı olur.

[!INCLUDE [Windows 10 and OpenVPN note](../../includes/vpn-gateway-openvpn-auth-include.md)]

[!INCLUDE [create](../../includes/openvpn-azure-ad-tenant-multi-app.md)]

## <a name="6-enable-authentication-on-the-gateway"></a><a name="enable-authentication"></a>6. ağ geçidinde kimlik doğrulamasını etkinleştirin

Bu adımda, VPN ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirecektir.

1. **Noktadan siteye yapılandırmaya** giderek ve **tünel türü**olarak **OpenVPN (SSL)** seçerek VPN ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirin. **Kimlik doğrulaması türü** olarak **Azure Active Directory** öğesini seçin, ardından **Azure Active Directory** bölümünün altındaki bilgileri girin.

    ![Azure portal görünümü](./media/openvpn-azure-ad-tenant-multi-app/azure-ad-auth-portal.png)

    > [!NOTE]
    > Azure VPN istemcisinin uygulama KIMLIĞINI kullanmayın: tüm kullanıcılara VPN Gateway erişimi verir. Kaydettiğiniz uygulamaların KIMLIĞINI kullanın.

2. **VPN Istemcisini indir** bağlantısına tıklayarak profili oluşturun ve indirin.

3. İndirilen ZIP dosyasını ayıklayın.

4. Sıkıştırılmış olmayan "AzureVPN" klasörüne gidin.

5. "azurevpnconfig.xml" dosyasının konumunu bir yere göz önünde alın. azurevpnconfig.xml, VPN bağlantısı ayarını içerir ve doğrudan Azure VPN Istemci uygulamasına aktarılabilir. Ayrıca, bu dosyayı e-posta veya başka yollarla bağlanması gereken tüm kullanıcılara dağıtabilirsiniz. Kullanıcının başarıyla bağlanması için geçerli bir Azure AD kimlik bilgilerine ihtiyacı olacak.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. [P2S VPN bağlantıları IÇIN VPN Istemcisi yapılandırma](openvpn-azure-ad-client.md).
