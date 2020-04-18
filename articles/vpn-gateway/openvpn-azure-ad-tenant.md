---
title: 'VPN Ağ Geçidi: P2S VPN bağlantıları için Azure AD kiracı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulamasını kullanarak VNet'inize bağlanmak için P2S VPN'i kullanabilirsiniz
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 04/17/2020
ms.author: alzam
ms.openlocfilehash: 00db2ed05285a1637414aa1e3adbe3b047ff0568
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641341"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>P2S OpenVPN iletişim kuralı bağlantıları için Azure Etkin Dizin kiracısı oluşturma

VNet'inize bağlanırken, sertifika tabanlı kimlik doğrulamasını veya RADIUS kimlik doğrulamasını kullanabilirsiniz. Ancak, Açık VPN iletişim kuralını kullandığınızda, Azure Etkin Dizin kimlik doğrulamasını da kullanabilirsiniz. Bu makale, P2S Open VPN kimlik doğrulaması için bir Azure AD kiracısı ayarlamanıza yardımcı olur.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® iletişim iletişim bağlantıları için desteklenir.
>


## <a name="1-verify-azure-ad-tenant"></a><a name="tenant"></a>1. Azure AD kiracılarını doğrulayın

Azure AD kiracınız olduğunu doğrulayın. Azure AD kiracınız yoksa, [yeni bir kiracı oluştur](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) makalesindeki adımları kullanarak bir tane oluşturabilirsiniz:

* Kuruluş adı
* İlk etki alanı adı

Örnek:

   ![Yeni Azure AD kiracı](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Azure AD kiracı sıyrık kullanıcıları oluşturma

Azure AD kiracınızın aşağıdaki hesaplara ihtiyacı vardır: Bir Global Yönetici hesabı ve bir ana kullanıcı hesabı. Ana kullanıcı hesabı, ana gömme hesabınız (hizmet hesabı) olarak kullanılır. Bir Azure AD kiracı kullanıcı hesabı oluşturduğunuzda, oluşturmak istediğiniz kullanıcı türüne göre Dizin rolünü ayarlarsınız.

Azure AD kiracınız için en az iki kullanıcı oluşturmak için [bu makaledeki](../active-directory/fundamentals/add-users-azure-active-directory.md) adımları kullanın. Hesap türlerini oluşturmak için **Dizin Rolünü** değiştirdiğinden emin olun:

* Genel Yönetici
* Kullanıcı

## <a name="3-enable-azure-ad-authentication-on-the-vpn-gateway"></a><a name="enable-authentication"></a>3. VPN ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirme

1. Kimlik doğrulaması için kullanmak istediğiniz dizinin Dizin Kimliğini bulun. Active Directory sayfasının özellikleri bölümünde listelenir.

    ![Dizin Kimliği](./media/openvpn-create-azure-ad-tenant/directory-id.png)

2. Dizin kimliği'ni kopyalayın.

3. **Azure portalında, Global yönetici** rolüne atanmış bir kullanıcı olarak oturum açın.

4. Sonra, yönetici onayı verin. Tarayıcınızın adres çubuğunda dağıtım konumunuza ait URL'yi kopyalayın ve yapıştırın:

    Genel

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Kamu

    ```
   https://login.microsoftonline.us/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud Almanya

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure Çin 21Vianet

    ```
    https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. İstenirse **Global Admin** hesabını seçin.

    ![Dizin Kimliği](./media/openvpn-create-azure-ad-tenant/pick.png)

6. İstendiğinde **Kabul Et'i** seçin.

    ![Kabul Et](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. Azure REKLAM'ınız altında, **Kurumsal uygulamalarda** **Azure VPN'inin** listelenmiş olduğunu görürsünüz.

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)
    
8. Zaten işleyen bir noktadan siteye ortamınız yoksa, bir ortam oluşturmak için yönergeyi izleyin. Bkz. Yerel Azure sertifikası kimlik doğrulaması yla bir noktadan siteye VPN ağ geçidi oluşturmak ve yapılandırmak için bir noktadan siteye [VPN oluştur.](vpn-gateway-howto-point-to-site-resource-manager-portal.md) 

    > [!IMPORTANT]
    > Temel SKU OpenVPN için desteklenmez.

9. Aşağıdaki komutları çalıştırarak VPN ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirin ve komutu kendi ortamınızı yansıtacak şekilde değiştirdiğinizden emin olun:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of VPN gateway> -ResourceGroupName <Resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -AadTenantUri "https://login.microsoftonline.com/<your Directory ID>/" -AadAudienceId "41b23e61-6c1e-4545-b367-cd054e0ed4b4" -AadIssuerUri "https://sts.windows.net/<your Directory ID>/" -VpnClientAddressPool 192.168.0.0/24 -VpnClientProtocol OpenVPN
    ```

   > [!NOTE]
   > Değerin sonuna bir çizgi eklediğinizden `AadIssuerUri` emin olun. Aksi takdirde, komut başarısız olur.

10. Aşağıdaki komutları çalıştırarak profili oluşturun ve indirin. -ResourceGroupName ve -Ad değerlerini kendi kiyle eşleşecek şekilde değiştirin.

    ```azurepowershell-interactive
    $profile = New-AzVpnClientConfiguration -Name <name of VPN gateway> -ResourceGroupName <Resource group> -AuthenticationMethod "EapTls"
    $PROFILE.VpnProfileSASUrl
    ```

11. Komutları çalıştırdıktan sonra, aşağıdakine benzer bir sonuç görürsünüz. Profil zip dosyasını indirmek için sonuç URL'sini tarayıcınıza kopyalayın.

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/profile.png)

12. İndirilen zip dosyasını ayıklayın.

13. Fermuarsız "AzureVPN" klasörüne göz atın.

14. "azurevpnconfig.xml" dosyasının konumuna dikkat edin. Azurevpnconfig.xml VPN bağlantısı için ayar içerir ve doğrudan Azure VPN İstemci uygulamasına içe aktarılabilir. Ayrıca, bu dosyayı e-posta veya başka yollarla bağlanması gereken tüm kullanıcılara da dağıtabilirsiniz. Kullanıcının başarılı bir şekilde bağlanmak için geçerli Azure REKLAM kimlik bilgilerine ihtiyacı olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. [P2S VPN bağlantıları için vpn istemcisini yapılandırın.](openvpn-azure-ad-client.md)
