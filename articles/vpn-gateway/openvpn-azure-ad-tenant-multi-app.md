---
title: 'VPN Gateway: farklı Kullanıcı grupları için Azure AD kiracısı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulaması kullanarak sanal ağınıza bağlanmak için P2S VPN kullanabilirsiniz
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 12/20/2019
ms.author: alzam
ms.openlocfilehash: 73a47b20010be0b4eadec790dd8fb254f912ff2a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75477180"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>P2S OpenVPN Protokolü bağlantıları için Azure Active Directory kiracısı oluşturma

Sanal ağınıza bağlanırken sertifika tabanlı kimlik doğrulama veya RADIUS kimlik doğrulaması kullanabilirsiniz. Ancak, açık VPN protokolünü kullandığınızda Azure Active Directory kimlik doğrulamasını da kullanabilirsiniz. Farklı kullanıcı kümesinin farklı VPN ağ geçitlerine bağlanmasını istiyorsanız, AD 'de birden çok uygulamayı kaydedebilir ve bunları farklı VPN ağ geçitlerine bağlayabilirsiniz. Bu makale, P2S OpenVPN kimlik doğrulaması için bir Azure AD kiracısı ayarlamanıza ve farklı kullanıcılar ve gruplar için farklı erişime izin vermek üzere Azure AD 'de birden çok uygulama oluşturup kaydetmenize yardımcı olur.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® Protokolü bağlantılarında desteklenir.
>

## <a name="tenant"></a>1. Azure AD kiracısı oluşturma

[Yeni kiracı oluşturma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) makalesindeki adımları kullanarak BIR Azure AD kiracısı oluşturun:

* Kuruluş adı
* İlk etki alanı adı

Örnek:

   ![Yeni Azure AD kiracısı](./media/openvpn-azure-ad-tenant-multi-app/newtenant.png)

## <a name="users"></a>2. Azure AD kiracı kullanıcıları oluşturun

Sonra, iki kullanıcı hesabı oluşturun. Bir genel yönetici hesabı ve bir ana Kullanıcı hesabı oluşturun. Ana Kullanıcı hesabı, ana ekleme hesabınız (hizmet hesabı) olarak kullanılır. Bir Azure AD Kiracı Kullanıcı hesabı oluşturduğunuzda, oluşturmak istediğiniz kullanıcı türü için Dizin rolünü ayarlayın.

Azure AD kiracınız için en az iki kullanıcı oluşturmak üzere [Bu makaledeki](../active-directory/fundamentals/add-users-azure-active-directory.md) adımları kullanın. Hesap türlerini oluşturmak için **Dizin rolünü** değiştirdiğinizden emin olun:

* Genel Yönetici
* Kullanıcı

## <a name="enable-authentication"></a>3. Azure AD kiracısında Azure VPN Istemcisini kaydetme

1. Kimlik doğrulaması için kullanmak istediğiniz dizinin dizin KIMLIĞINI bulun. Active Directory sayfasının Özellikler bölümünde listelenir.

    ![Dizin KIMLIĞI](./media/openvpn-azure-ad-tenant-multi-app/directory-id.png)

2. Dizin KIMLIĞINI kopyalayın.

3. Azure portal, **genel yönetici** rolüne atanan bir kullanıcı olarak oturum açın.

4. Sonra, yönetici onayı verin. Tarayıcınızın adres çubuğuna dağıtım konumunuza ait URL 'YI kopyalayıp yapıştırın:

    Genel

    ```
    https://login.microsoftonline.com/common/oauth2/authorize?client_id=41b23e61-6c1e-4545-b367-cd054e0ed4b4&response_type=code&redirect_uri=https://portal.azure.com&nonce=1234&prompt=admin_consent
    ````

    Azure Devlet Kurumları

    ```
    https://login-us.microsoftonline.com/common/oauth2/authorize?client_id=51bb15d4-3a4f-4ebf-9dca-40096fe32426&response_type=code&redirect_uri=https://portal.azure.us&nonce=1234&prompt=admin_consent
    ````

    Microsoft Bulut Almanya

    ```
    https://login-us.microsoftonline.de/common/oauth2/authorize?client_id=538ee9e6-310a-468d-afef-ea97365856a9&response_type=code&redirect_uri=https://portal.microsoftazure.de&nonce=1234&prompt=admin_consent
    ````

    Azure Çin 21Vianet

    ```
    https://https://login.chinacloudapi.cn/common/oauth2/authorize?client_id=49f817b6-84ae-4cc0-928c-73f27289b3aa&response_type=code&redirect_uri=https://portal.azure.cn&nonce=1234&prompt=admin_consent
    ```

5. İstenirse **genel yönetici** hesabını seçin.

    ![Dizin KIMLIĞI](./media/openvpn-azure-ad-tenant-multi-app/pick.png)

6. İstendiğinde **kabul et** ' i seçin.

    ![Kabul et](./media/openvpn-azure-ad-tenant-multi-app/accept.jpg)

7. Azure AD 'nizin altında, **Kurumsal uygulamalarda**, **Azure VPN** ' yi listelenmiş görürsünüz.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/azurevpn.png)

## <a name="enable-authentication"></a>4. çeşitli kullanıcılar/gruplar için ek uygulamalar kaydetme

1. Azure Active Directory altında **uygulama kayıtları** ' a ve ardından **+ Yeni kayıt** ' a tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app1.png)

2. **Uygulama kaydet** dikey penceresinde **adı** girin ve istediğiniz **Desteklenen hesap türlerini** seçin ve **Kaydet** ' e tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app2.png)

3. Yeni uygulama kaydedildikten sonra uygulama dikey penceresinde **BIR API 'Yi kullanıma** sunma ' ya tıklayın.

4. **+ Kapsam Ekle** ' ye tıklayın
5. Varsayılan **uygulama KIMLIĞI URI** 'sini bırakın ve Kaydet ' e tıklayın **ve devam edin**

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app3.png)
6. Gerekli alanları doldurup **durumunun** **etkinleştirildiğinden**emin olun. **Kapsam Ekle** 'ye tıklayın

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app4.png)
7. **BIR API 'Yi kullanıma** sunma ve ardından **bir istemci uygulaması ekleme**' ye tıklayın.  **ISTEMCI kimliği**için buluta bağlı olarak aşağıdaki değerleri girin:
    -   Azure **genel** için **41b23e61-6c1e-4545-b367-cd054e0ed4b4** girin
    -   Azure **kamu** için **51bb15d4-3a4f-4EBF-9dca-40096fe32426** girin
    -   Azure **Almanya** için **538ee9e6-310A-468d-afef-ea97365856a9** girin
    -   Azure **Çin 21Vianet** için **49f817b6-84ae-4cc0-928c-73f27289b3aa** girin


8. **Uygulama Ekle** ' ye tıklayın

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app5.png)
9. **Genel bakış** sayfasından **uygulama (istemci) kimliğini** kopyalayın. VPN ağ geçidlerinizi yapılandırmak için buna ihtiyacınız olacak

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/app6.png)

10. Güvenlik gereksiniminize gereken birçok uygulama oluşturmak için bu bölümdeki adımları yineleyin (4). Her uygulama, bir VPN ağ geçidiyle ilişkilendirilir ve farklı bir kullanıcı kümesine sahip olabilir. Bir ağ geçidiyle yalnızca bir uygulama ilişkilendirilebilir.

## <a name="enable-authentication"></a>5. uygulamalarınıza Kullanıcı atama

1. Azure AD 'de **Kurumsal uygulamalar**' ın altında, yeni kayıtlı uygulamayı seçin ve **Özellikler**' e tıklayın. **Gerekli Kullanıcı atamasının** **Evet**olarak ayarlandığından emin olun. **Kaydet**’e tıklayın

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user2.png)

2. Uygulama sayfasında, **Kullanıcılar ve gruplar** ' a ve ardından **Kullanıcı Ekle** ' ye tıklayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user3.png)
3. **Atama Ekle**altında **Kullanıcılar ve gruplar**' a tıklayın. Bu VPN uygulamasına erişebilmesini istediğiniz kullanıcıları seçin. **Seç**'e tıklayın

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/user4.png)

## <a name="enable-authentication"></a>6. VPN Gateway 'de Azure AD kimlik doğrulamasını etkinleştirme

1. Aşağıdaki komutları çalıştırarak VPN Gateway 'de Azure AD kimlik doğrulamasını etkinleştirin ve komutu kendi ortamınızı yansıtacak şekilde değiştirdiğinizden emin olun:

    ```azurepowershell-interactive
    $gw = Get-AzVirtualNetworkGateway -Name <name of VPN gateway> -ResourceGroupName <Resource group>
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientRootCertificates @()
    Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -AadTenantUri "https://login.microsoftonline.com/<your Directory ID>" -AadAudienceId "application ID from previous section" -AadIssuerUri "https://sts.windows.net/<your Directory ID>/" -VpnClientAddressPool 192.168.0.0/24
    ```
> [!NOTE]
> Tüm kullanıcılara VPN ağ geçidine erişim izni verecek şekilde yukarıdaki komutlarda Azure VPN istemcisinin uygulama KIMLIĞINI kullanmayın. Kaydettiğiniz uygulamaların KIMLIĞINI kullanın.

2. Aşağıdaki komutları çalıştırarak profili oluşturun ve indirin. -ResourcGroupName ve-Name değerlerini kendi değerlerinizle eşleşecek şekilde değiştirin.

    ```azurepowershell-interactive
    $profile = New-AzVpnClientConfiguration -Name <name of VPN gateway> -ResourceGroupName <Resource group> -AuthenticationMethod "EapTls"
    $PROFILE.VpnProfileSASUrl
    ```

3. Komutları çalıştırdıktan sonra, aşağıdakine benzer bir sonuç görürsünüz. Profil ZIP dosyasını indirmek için sonuç URL 'sini tarayıcınıza kopyalayın.

    ![Azure VPN](./media/openvpn-azure-ad-tenant-multi-app/profile.png)

4. İndirilen ZIP dosyasını ayıklayın.

5. Sıkıştırılmış olmayan "AzureVPN" klasörüne gidin.

6. "Azurevpnconfig. xml" dosyasının konumunu bir yere unutmayın. Azurevpnconfig. xml, VPN bağlantısı ayarını içerir ve doğrudan Azure VPN Istemci uygulamasına aktarılabilir. Ayrıca, bu dosyayı e-posta veya başka yollarla bağlanması gereken tüm kullanıcılara dağıtabilirsiniz. Kullanıcının başarıyla bağlanması için geçerli bir Azure AD kimlik bilgilerine ihtiyacı olacak.

## <a name="next-steps"></a>Sonraki adımlar

Sanal ağınıza bağlanmak için bir VPN istemci profili oluşturmanız ve yapılandırmanız gerekir. Bkz. [P2S VPN bağlantıları IÇIN VPN Istemcisi yapılandırma](openvpn-azure-ad-client.md).
