---
title: 'VPN Ağ Geçidi: Farklı kullanıcı grupları için Azure AD kiracı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulamasını kullanarak VNet'inize bağlanmak için P2S VPN'i kullanabilirsiniz
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: alzam
ms.openlocfilehash: 118ea21cbdd2e0527659c7c1beb40d8e42fa1d10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77485719"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>P2S OpenVPN iletişim kuralı bağlantıları için Azure Etkin Dizin kiracısı oluşturma

VNet'inize bağlanırken, sertifika tabanlı kimlik doğrulamasını veya RADIUS kimlik doğrulamasını kullanabilirsiniz. Ancak, Açık VPN iletişim kuralını kullandığınızda, Azure Etkin Dizin kimlik doğrulamasını da kullanabilirsiniz. Farklı kullanıcı kümesinin farklı VPN ağ geçitlerine bağlanabilmesini istiyorsanız, AD'ye birden fazla uygulama kaydedebilir ve bunları farklı VPN ağ geçitlerine bağlayabilirsiniz. Bu makale, P2S OpenVPN kimlik doğrulaması için bir Azure AD kiracısı kurmanıza ve farklı kullanıcılar ve gruplar için farklı erişime izin vermek için Azure AD'de birden çok uygulama oluşturmanıza ve kaydetmenize yardımcı olur.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® iletişim iletişim bağlantıları için desteklenir.
>

[!INCLUDE [create](../../includes/openvpn-azure-ad-tenant-multi-app.md)]

## <a name="6-enable-authentication-on-the-gateway"></a><a name="enable-authentication"></a>6. Ağ geçidinde kimlik doğrulamayı etkinleştirme

Bu adımda, VPN ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirin.

1. Aşağıdaki komutları çalıştırarak VPN ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirin. Komutları kendi ortamınızı yansıtacak şekilde değiştirdiğinizden emin olun:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of VPN gateway> -ResourceGroupName <Resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -AadTenantUri "https://login.microsoftonline.com/<your Directory ID>" -AadAudienceId "application ID from previous section" -AadIssuerUri "https://sts.windows.net/<your Directory ID>/" -VpnClientAddressPool 192.168.0.0/24
    ```
    > [!NOTE]
    > Yukarıdaki komutlarda Azure VPN istemcisinin uygulama kimliğini kullanmayın: Tüm kullanıcılara VPN ağ geçidine erişim hakkı verir. Kaydolduğunuz başvuru(lar)ın kimliğini kullanın.

2. Aşağıdaki komutları çalıştırarak profili oluşturun ve indirin. -ResourcGroupName ve -Ad değerlerini kendi kiyle eşleşecek şekilde değiştirin.

    ```azurepowershell-interactive
    $profile = New-AzVpnClientConfiguration -Name <name of VPN gateway> -ResourceGroupName <Resource group> -AuthenticationMethod "EapTls"
    $PROFILE.VpnProfileSASUrl
    ```

3. Komutları çalıştırdıktan sonra, aşağıdakine benzer bir sonuç görürsünüz. Profil zip dosyasını indirmek için sonuç URL'sini tarayıcınıza kopyalayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/profile.png)

4. İndirilen zip dosyasını ayıklayın.

5. Fermuarsız "AzureVPN" klasörüne göz atın.

6. "azurevpnconfig.xml" dosyasının konumuna dikkat edin. Azurevpnconfig.xml VPN bağlantısı için ayar içerir ve doğrudan Azure VPN İstemci uygulamasına içe aktarılabilir. Ayrıca, bu dosyayı e-posta veya başka yollarla bağlanması gereken tüm kullanıcılara da dağıtabilirsiniz. Kullanıcının başarılı bir şekilde bağlanmak için geçerli Azure REKLAM kimlik bilgilerine ihtiyacı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. [P2S VPN bağlantıları için vpn istemcisini yapılandırın.](openvpn-azure-ad-client.md)
