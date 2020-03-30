---
title: 'Kullanıcı VPN bağlantıları için Azure AD kiracı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulamasını kullanarak VNet'inize bağlanmak için Azure Virtual WAN Kullanıcı VPN'ini (noktadan siteye) kullanabilirsiniz
titleSuffix: Azure Virtual WAN
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: alzam
ms.openlocfilehash: 74347ce969b6a5ffd57f5ca8396517e78590f3f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80059452"
---
# <a name="create-an-azure-active-directory-tenant-for-user-vpn-openvpn-protocol-connections"></a>Kullanıcı VPN OpenVPN protokol bağlantıları için Azure Active Directory kiracısı oluşturma

VNet'inize bağlanırken, sertifika tabanlı kimlik doğrulamasını veya RADIUS kimlik doğrulamasını kullanabilirsiniz. Ancak, Açık VPN iletişim kuralını kullandığınızda, Azure Etkin Dizin kimlik doğrulamasını da kullanabilirsiniz. Bu makale, Sanal WAN Kullanıcı VPN (noktadan siteye) Açık VPN kimlik doğrulaması için bir Azure REKLAM kiracısı ayarlamanıza yardımcı olur.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca&reg; OpenVPN iletişim iletişim bağlantıları için desteklenir.
>

## <a name="1-create-the-azure-ad-tenant"></a><a name="tenant"></a>1. Azure AD kiracısını oluşturma

Yeni bir kiracı oluştur makalesindeki adımları kullanarak bir Azure AD [kiracısı oluşturun:](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

* Kuruluş adı
* İlk etki alanı adı

Örnek:

   ![Yeni Azure AD kiracı](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Azure AD kiracı sıyrık kullanıcıları oluşturma

Ardından, iki kullanıcı hesabı oluşturun. Bir Global Yönetici hesabı ve bir ana kullanıcı hesabı oluşturun. Ana kullanıcı hesabı, ana gömme hesabınız (hizmet hesabı) olarak kullanılır. Bir Azure AD kiracı kullanıcı hesabı oluşturduğunuzda, oluşturmak istediğiniz kullanıcı türüne göre Dizin rolünü ayarlarsınız.

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
    https://login-us.microsoftonline.com/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Cloud Almanya

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure Çin 21Vianet

    ```
    https://https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. İstenirse **Global Admin** hesabını seçin.

    ![Dizin Kimliği](./media/openvpn-create-azure-ad-tenant/pick.png)

6. İstendiğinde **Kabul Et'i** seçin.

    ![Kabul Et](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. Azure REKLAM'ınız altında, **Kurumsal uygulamalarda** **Azure VPN'inin** listelenmiş olduğunu görürsünüz.

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)

8. Kullanıcı VPN için Azure AD kimlik doğrulamasını yapılandırın ve [Azure'a Yer Noktası bağlantısı için Azure AD kimlik doğrulamasını yapılandırma](virtual-wan-point-to-site-azure-ad.md) adımlarını izleyerek sanal hub'a atayın

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız ve sanal hub ile ilişkilendirmeniz gerekir. Bkz. [Azure'a Nokta-To-Site bağlantısı için Azure AD kimlik doğrulaması yapılandırın.](virtual-wan-point-to-site-azure-ad.md)
