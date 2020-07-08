---
title: 'VPN Gateway: P2S VPN bağlantıları için Azure AD kiracısı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulaması kullanarak sanal ağınıza bağlanmak için P2S VPN kullanabilirsiniz
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/17/2020
ms.author: alzam
ms.openlocfilehash: 2dda6cb84fc881b4ca628ff1cecdec7c00555e8b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85414325"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>P2S OpenVPN protokolü bağlantıları için Azure Active Directory kiracısı oluşturma

Sanal ağınıza bağlanırken sertifika tabanlı kimlik doğrulama veya RADIUS kimlik doğrulaması kullanabilirsiniz. Ancak, açık VPN protokolünü kullandığınızda Azure Active Directory kimlik doğrulamasını da kullanabilirsiniz. Bu makale, P2S Open VPN kimlik doğrulaması için bir Azure AD kiracısı ayarlamanıza yardımcı olur.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® Protokolü bağlantılarında desteklenir.
>


## <a name="1-verify-azure-ad-tenant"></a><a name="tenant"></a>1. Azure AD kiracısını doğrulama

Bir Azure AD kiracınız olduğunu doğrulayın. Bir Azure AD kiracınız yoksa, [Yeni bir kiracı oluşturma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) makalesindeki adımları kullanarak bir tane oluşturabilirsiniz:

* Kuruluş adı
* İlk etki alanı adı

Örnek:

   ![Yeni Azure AD kiracısı](./media/openvpn-create-azure-ad-tenant/newtenant.png)

## <a name="2-create-azure-ad-tenant-users"></a><a name="users"></a>2. Azure AD kiracı kullanıcıları oluşturun

Azure AD kiracınız aşağıdaki hesaplara ihtiyaç duyuyor: genel yönetici hesabı ve bir ana Kullanıcı hesabı. Ana Kullanıcı hesabı, ana ekleme hesabınız (hizmet hesabı) olarak kullanılır. Bir Azure AD Kiracı Kullanıcı hesabı oluşturduğunuzda, oluşturmak istediğiniz kullanıcı türü için Dizin rolünü ayarlayın.

Azure AD kiracınız için en az iki kullanıcı oluşturmak üzere [Bu makaledeki](../active-directory/fundamentals/add-users-azure-active-directory.md) adımları kullanın. Hesap türlerini oluşturmak için **Dizin rolünü** değiştirdiğinizden emin olun:

* Genel Yönetici
* Kullanıcı

## <a name="3-enable-azure-ad-authentication-on-the-vpn-gateway"></a><a name="enable-authentication"></a>3. VPN Gateway 'de Azure AD kimlik doğrulamasını etkinleştirme

1. Kimlik doğrulaması için kullanmak istediğiniz dizinin dizin KIMLIĞINI bulun. Active Directory sayfasının Özellikler bölümünde listelenir.

    ![Dizin KIMLIĞI](./media/openvpn-create-azure-ad-tenant/directory-id.png)

2. Dizin kimliği'ni kopyalayın.

3. Azure portal, **genel yönetici** rolüne atanan bir kullanıcı olarak oturum açın.

4. Sonra, yönetici onayı verin. Tarayıcınızın adres çubuğuna dağıtım konumunuza ait URL 'YI kopyalayıp yapıştırın:

    Ortak

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Kamu

    ```
   https://login.microsoftonline.us/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Almanya Microsoft Bulut

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure China 21Vianet

    ```
    https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. İstenirse **genel yönetici** hesabını seçin.

    ![Dizin KIMLIĞI](./media/openvpn-create-azure-ad-tenant/pick.png)

6. İstendiğinde **kabul et** ' i seçin.

    ![Kabul Et](./media/openvpn-create-azure-ad-tenant/accept.jpg)

7. Azure AD 'nizin altında, **Kurumsal uygulamalarda**LISTELENEN **Azure VPN** ' yi görürsünüz.

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azurevpn.png)
    
8. Zaten çalışan bir noktadan siteye ortamınız yoksa, bir tane oluşturmak için yönergeyi izleyin. Noktadan siteye VPN ağ geçidi oluşturmak ve yapılandırmak için bkz. [noktadan sıteye VPN oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md) . 

    > [!IMPORTANT]
    > Temel SKU, OpenVPN için desteklenmez.

9. **Noktadan siteye yapılandırmaya** giderek ve **tünel türü**olarak **OpenVPN (SSL)** seçerek VPN ağ geçidinde Azure AD kimlik doğrulamasını etkinleştirin. **Kimlik doğrulaması türü** olarak **Azure Active Directory** öğesini seçin, ardından **Azure Active Directory** bölümünün altındaki bilgileri girin.

    ![Azure VPN](./media/openvpn-create-azure-ad-tenant/azure-ad-auth-portal.png)


   > [!NOTE]
   > Değerin sonuna eğik çizgi eklediğinizden emin olun `AadIssuerUri` . Aksi takdirde bağlantı başarısız olabilir.

10. **VPN Istemcisini indir** bağlantısına tıklayarak profili oluşturun ve indirin.

11. İndirilen ZIP dosyasını ayıklayın.

12. Sıkıştırılmış olmayan "AzureVPN" klasörüne gidin.

13. "azurevpnconfig.xml" dosyasının konumunu bir yere göz önünde alın. azurevpnconfig.xml, VPN bağlantısı ayarını içerir ve doğrudan Azure VPN Istemci uygulamasına aktarılabilir. Ayrıca, bu dosyayı e-posta veya başka yollarla bağlanması gereken tüm kullanıcılara dağıtabilirsiniz. Kullanıcının başarıyla bağlanması için geçerli bir Azure AD kimlik bilgilerine ihtiyacı olacak.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. [P2S VPN bağlantıları IÇIN VPN Istemcisi yapılandırma](openvpn-azure-ad-client.md).
