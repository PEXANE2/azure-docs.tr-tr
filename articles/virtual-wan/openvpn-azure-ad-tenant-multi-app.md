---
title: 'VPN Gateway: farklı Kullanıcı grupları için Azure AD kiracısı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulaması kullanarak sanal ağınıza bağlanmak için P2S VPN kullanabilirsiniz
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 1/10/2020
ms.author: alzam
ms.openlocfilehash: 90244b9dcf30c2ef01d4e57c9d8e35fa1d71f434
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76985654"
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

    Almanya Microsoft Bulut

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

    ![Ettiğinizde](./media/openvpn-azure-ad-tenant-multi-app/accept.jpg)

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

## <a name="site"></a>6. yeni bir P2S yapılandırması oluşturma

P2S yapılandırması, uzak istemcilerin bağlanmasına yönelik parametreleri tanımlar.

1. Ortamınız için gereken değerleri değiştirerek aşağıdaki değişkenleri ayarlayın.

   ```powershell
   $aadAudience = "00000000-abcd-abcd-abcd-999999999999"
   $aadIssuer = "https://sts.windows.net/00000000-abcd-abcd-abcd-999999999999/"
   $aadTenant = "https://login.microsoftonline.com/00000000-abcd-abcd-abcd-999999999999"    
   ```

2. Yapılandırmayı oluşturmak için aşağıdaki komutları çalıştırın:

   ```powershell
   $aadConfig = New-AzVpnServerConfiguration -ResourceGroupName <ResourceGroup> -Name newAADConfig -VpnProtocol OpenVPN -VpnAuthenticationType AAD -AadTenant $aadTenant -AadIssuer $aadIssuer -AadAudience $aadAudience -Location westcentralus
   ```

> [!NOTE]
> Tüm kullanıcılara VPN ağ geçidine erişim izni verecek şekilde yukarıdaki komutlarda Azure VPN istemcisinin uygulama KIMLIĞINI kullanmayın. Kaydettiğiniz uygulamaların KIMLIĞINI kullanın.

## <a name="hub"></a>7. hub atamasını Düzenle

1. Sanal WAN altındaki **hub** + dikey penceresine gidin.
2. VPN sunucusu yapılandırmasını ilişkilendirmek istediğiniz hub 'ı seçin ve üç nokta (...) simgesine tıklayın.

   ![yeni site](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. **Sanal hub 'ı Düzenle**' ye tıklayın.
4. **Noktadan siteye ağ geçidini dahil** et onay kutusunu işaretleyin ve Istediğiniz **ağ geçidi ölçek birimini** seçin.

   ![yeni site](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. VPN istemcilerine atanan IP adreslerine ait **adres havuzunu** girin.
6. **Onayla**'ya tıklayın.
7. İşlemin tamamlanması 30 dakika kadar sürebilir.

## <a name="device"></a>8. VPN profilini indir

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN 'nizin sayfasında, **Kullanıcı VPN yapılandırması**' na tıklayın.
2. Sayfanın üst kısmında, **Kullanıcı VPN yapılandırması 'Nı indir**' e tıklayın.
3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.
4. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.

4. İndirilen ZIP dosyasını ayıklayın.

5. Sıkıştırılmış olmayan "AzureVPN" klasörüne gidin.

6. "Azurevpnconfig. xml" dosyasının konumunu bir yere unutmayın. Azurevpnconfig. xml, VPN bağlantısı ayarını içerir ve doğrudan Azure VPN Istemci uygulamasına aktarılabilir. Ayrıca, bu dosyayı e-posta veya başka yollarla bağlanması gereken tüm kullanıcılara dağıtabilirsiniz. Kullanıcının başarıyla bağlanması için geçerli bir Azure AD kimlik bilgilerine ihtiyacı olacak.
## <a name="configure-user-vpn-clients"></a>Kullanıcı VPN istemcilerini yapılandırma

Bağlanmak için, Azure VPN Istemcisini (Önizleme) indirmeniz ve sanal ağa bağlanmak isteyen her bilgisayarda önceki adımlarda indirilen VPN istemci profilini içeri aktarmanız gerekir.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® Protokolü bağlantılarında desteklenir.
>

#### <a name="to-download-the-azure-vpn-client"></a>Azure VPN istemcisini indirmek için

Azure VPN Istemcisini (Önizleme) indirmek için bu [bağlantıyı](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab) kullanın.

#### <a name="import"></a>İstemci profilini içeri aktarmak için

1. Sayfasında **Içeri aktar**' ı seçin.

    ![aktarmaya](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Profil XML dosyasına gidin ve seçin. Dosya seçili olduğunda **Aç**' ı seçin.

    ![aktarmaya](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Profilin adını belirtin ve **Kaydet**' i seçin.

    ![aktarmaya](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. VPN 'ye bağlanmak için **Bağlan** ' ı seçin.

    ![aktarmaya](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Bağlandıktan sonra simge yeşil ve **bağlı**olarak görünür.

    ![aktarmaya](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="delete"></a>İstemci profilini silmek için

1. Silmek istediğiniz istemci profilinin yanındaki üç nokta (...) simgesini seçin. Ardından **Kaldır**' ı seçin.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Silmek için **Kaldır** ' ı seçin.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose"></a>Bağlantı sorunlarını Tanıla

1. Bağlantı sorunlarını tanılamak için **Tanılama** aracını kullanabilirsiniz. Menüyü açığa çıkarmak için tanılamak istediğiniz VPN bağlantısının yanındaki üç nokta (...) simgesini seçin. Ardından **Tanıla**' yı seçin.

    ![tanın](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. **Bağlantı özellikleri** sayfasında, **tanı Çalıştır**' ı seçin.

    ![tanın](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Kimlik bilgilerinizle oturum açın.

    ![tanın](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. Tanılama sonuçlarını görüntüleyin.

    ![tanın](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="viewwan"></a>Sanal WAN 'nizi görüntüleme

1. Sanal WAN'a gidin.
2. Genel bakış sayfasında haritadaki her bir nokta bir hub'ı temsil eder. Hub sistem durumu özetini görüntülemek için noktalardan birinin üzerine gidin.
3. Hub'lar ve bağlantılar bölümünde herhangi bir hub'ın durumunu, sitesini, bölgesini, VPN bağlantısı durumunu ve gelen/giden baytları görüntüleyebilirsiniz.

## <a name="viewhealth"></a>Kaynak sistem durumunu görüntüleme

1. WAN'ınıza gidin.
2. WAN sayfanızın **Destek ve sorun giderme** bölümünde **Sistem durumu**'na tıklayın ve kaynağınızı görüntüleyin.


## <a name="cleanup"></a>Kaynakları Temizleme

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
