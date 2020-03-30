---
title: 'Kullanıcı VPN bağlantısı için Azure AD kimlik doğrulamasını yapılandırma: Virtual WAN'
description: Kullanıcı VPN'i için Azure Active Directory kimlik doğrulamasını nasıl yapılandırılabildiğini öğrenin.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: alzam
ms.openlocfilehash: 703b832d58f2374eac131cfd380ba27f2c890618
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80059493"
---
# <a name="configure-azure-active-directory-authentication-for-user-vpn"></a>Kullanıcı VPN'i için Azure Active Directory kimlik doğrulamasını yapılandırma

Bu makalede, Virtual WAN'daki Kullanıcı VPN'i için Azure AD kimlik doğrulaması nın OpenVPN VPN bağlantısı üzerinden Azure'daki kaynaklarınıza bağlanmak için nasıl yapılandırılacağı gösterilmektedir. Azure Active Directory kimlik doğrulaması yalnızca OpenVPN iletişim kuralını kullanan ağ geçitleri ve Windows çalıştıran istemciler için kullanılabilir.

Bu tür bir bağlantı, istemci bilgisayarda bir istemcinin yapılandırılmış olmasını gerektirir. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * WAN oluşturma
> * Hub oluşturma
> * P2S yapılandırması oluşturma
> * VPN istemci profili indirin
> * P2S yapılandırmasını bir hub'a uygulama
> * Bir sanal ağı bir hub'a bağlama
> * VPN istemci yapılandırmasını indirme ve uygulama
> * Sanal WAN'ınızı görüntüleme

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bağlanmak istediğiniz bir sanal ağınız var. Şirket içi ağlarınızın hiçbirinin bağlanmak istediğiniz sanal ağlarla örtüşmediğini doğrulayın. Azure portalında sanal ağ oluşturmak için [Hızlı Başlangıç'a](../virtual-network/quick-create-portal.md)bakın.

* Sanal ağınızın herhangi bir sanal ağ ağ geçidi yok. Sanal ağınızın bir ağ geçidi (VPN veya ExpressRoute) varsa, tüm ağ geçitlerini kaldırmanız gerekir. Bu yapılandırma, sanal ağların bunun yerine Sanal WAN hub ağ geçidine bağlanmasını gerektirir.

* Hub bölgenizden bir IP adresi aralığı edinin. Hub, Virtual WAN tarafından oluşturulan ve kullanılan sanal bir ağdır. Hub için belirttiğiniz adres aralığı, bağlandığınız varolan sanal ağlarınizle çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını bilmiyorsanız, bu ayrıntıları sizin için sağlayabilecek biriyle işbirliği yapabilirsiniz.

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Sanal WAN oluşturma

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

1. Sanal WAN sayfasına gidin. Portalda **+Kaynak oluştur**’a tıklayın. Arama kutusuna **Sanal WAN** yazın ve Enter'u seçin.
2. Sonuçlardan **Sanal WAN'ı** seçin. Sanal WAN sayfasında, WAN Oluştur sayfasını açmak için **Oluştur'u** tıklatın.
3. WAN **Oluştur** sayfasında, **Temel Bilgiler** sekmesinde aşağıdaki alanları doldurun:

   ![Sanal WAN](./media/virtual-wan-point-to-site-azure-ad/vwan.png)

   * **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu** - Yeni oluşturun veya varolanları kullanın.
   * **Kaynak grubu konumu** - Açılır kaynak konumundan bir kaynak konumu seçin. WAN, global bir kaynaktır ve belirli bir bölgeyle sınırlı değildir. Ancak oluşturduğunuz WAN kaynağını daha kolay yönetmek ve bulmak için bir bölge seçmeniz gerekir.
   * **Ad** - WAN adını vermek istediğiniz Adı yazın.
   * **Türü:** Standart. Temel WAN oluşturursanız, yalnızca bir Temel hub oluşturabilirsiniz. Temel hub'lar yalnızca VPN'den siteye bağlantı yeteneğine sahiptir.
4. Alanları doldurmayı bitirdikten sonra **Gözden Geçir +Oluştur'u**seçin.
5. Doğrulama geçtikten sonra, sanal WAN oluşturmak için **Oluştur'u** seçin.

## <a name="create-an-empty-virtual-hub"></a><a name="site"></a>Boş bir sanal hub oluşturma

1. Sanal WAN'ınızın altında Hub'ları seçin ve **+Yeni Hub'ı**tıklatın.

   ![yeni site](media/virtual-wan-point-to-site-azure-ad/hub1.jpg)
2. Sanal hub oluştur sayfasında aşağıdaki alanları doldurun.

   **Bölge** - Sanal hub'ı dağıtmak istediğiniz bölgeyi seçin.

   **Ad** - Sanal hub'ınızı aramak istediğiniz adı girin.

   **Hub özel adres alanı** - CIDR gösteriminde hub'ın adres aralığı.

   ![yeni site](media/virtual-wan-point-to-site-azure-ad/hub2.jpg)  
3. **Gözden geçir ve oluştur**’a tıklayın.
4. Geçirilen **doğrulama** sayfasında **oluştur'u**tıklatın.

## <a name="create-a-new-p2s-configuration"></a><a name="site"></a>Yeni bir P2S yapılandırması oluşturma

P2S yapılandırması, uzak istemcilerin bağlanmasına yönelik parametreleri tanımlar.

1. Sanal WAN'ınız altında **Kullanıcı VPN yapılandırmalarını**seçin.

   ![yeni config](media/virtual-wan-point-to-site-azure-ad/aadportal1.jpg)

2. **+Kullanıcı VPN config oluştur'a**tıklayın.

   ![yeni config](media/virtual-wan-point-to-site-azure-ad/aadportal2.jpg)

3. Bilgileri girin ve **Oluştur'u** tıklatın

   ![yeni config](media/virtual-wan-point-to-site-azure-ad/aadportal3.jpg)

## <a name="edit-hub-assignment"></a><a name="hub"></a>Hub atamasını düzenleme

1. Sanal WAN altında **Hubs** bıçak gidin.
2. Vpn sunucusu yapılandırmasını ilişkilendirmek istediğiniz hub'ı seçin ve elipsleri (...) tıklatın.

   ![yeni site](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. **Sanal hub'ı edit'i**tıklatın.
4. **Yer-iş geçidi ekle onay** kutusunu işaretleyin ve istediğiniz **Ağ Geçidi ölçeği birimini** seçin.

   ![yeni site](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. VPN istemcilerine IP adreslerinin atanacağı **Adres havuzunu** girin.
6. **Onayla**'ya tıklayın.
7. İşlemin tamamlanması 30 dakika kadar sürebilir.

## <a name="download-vpn-profile"></a><a name="device"></a>VPN profili indirme

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN'ınız için sayfada **Kullanıcı VPN yapılandırmalarını**tıklatın.
2. Sayfanın üst kısmında, **kullanıcı VPN config indir'i**tıklatın.
3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.
4. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.

## <a name="configure-user-vpn-clients"></a>Kullanıcı VPN istemcilerini yapılandırma

Bağlanmak için Azure VPN İstemci'sini indirmeniz ve VNet'e bağlanmak isteyen her bilgisayarda önceki adımlarda indirilen VPN istemcisi profilini almanız gerekir.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca&reg; OpenVPN iletişim iletişim bağlantıları için desteklenir.
>

#### <a name="to-download-the-azure-vpn-client"></a>Azure VPN istemcisini indirmek için

Azure VPN İstemcisini indirmek için bu [bağlantıyı](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab) kullanın.

#### <a name="to-import-a-client-profile"></a><a name="import"></a>İstemci profili almak için

1. Sayfada, **İçe Aktar'ı**seçin.

    ![içeri aktar](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Profil xml dosyasına göz atın ve seçin. Dosya seçilirken **Aç'ı**seçin.

    ![içeri aktar](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Profilin adını belirtin ve **Kaydet'i**seçin.

    ![içeri aktar](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. VPN'e bağlanmak için **Bağlan'ı** seçin.

    ![içeri aktar](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Bağlandıktan sonra simge yeşile döner ve **Bağlı**der.

    ![içeri aktar](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="to-delete-a-client-profile"></a><a name="delete"></a>İstemci profilini silmek için

1. Silmek istediğiniz istemci profilinin yanındaki elipsleri (...) seçin. Ardından **Kaldır'ı**seçin.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Silmek için **Kaldır'ı** seçin.

    ![delete](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose-connection-issues"></a><a name="diagnose"></a>Bağlantı sorunlarını tanılama

1. Bağlantı sorunlarını tanılamak için **Tanılama** aracını kullanabilirsiniz. Menüyü ortaya çıkarmak için tanılamak istediğiniz VPN bağlantısının yanındaki elipsleri (...) seçin. Ardından **Tanıla'yı**seçin.

    ![Tanılamak](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. Bağlantı **Özellikleri** sayfasında Tanıyı **Çalıştır'ı**seçin.

    ![Tanılamak](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Kimlik bilgilerinizle oturum açın.

    ![Tanılamak](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. Tanı sonuçlarını görüntüleyin.

    ![Tanılamak](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
2. Genel bakış sayfasında haritadaki her bir nokta bir hub'ı temsil eder.
3. Hub'lar ve bağlantılar bölümünde herhangi bir hub'ın durumunu, sitesini, bölgesini, VPN bağlantısı durumunu ve gelen/giden baytları görüntüleyebilirsiniz.


## <a name="clean-up-resources"></a><a name="cleanup"></a>Kaynakları temizleme

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
