---
title: 'Kullanıcı VPN bağlantısı için Azure AD kimlik doğrulamasını yapılandırma: sanal WAN'
description: Kullanıcı VPN için Azure Active Directory kimlik doğrulamasını yapılandırma hakkında bilgi edinin.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 10/14/2020
ms.author: alzam
ms.openlocfilehash: 9cc68eb60096c4431acfc988c87ca9bf99f1f045
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93043407"
---
# <a name="configure-azure-active-directory-authentication-for-user-vpn"></a>Kullanıcı VPN için Azure Active Directory kimlik doğrulamasını yapılandırma

Bu makalede, Azure 'da bir OpenVPN VPN bağlantısı üzerinden kaynaklarınıza bağlanmak üzere sanal WAN 'daki Kullanıcı VPN 'si için Azure AD kimlik doğrulamasını yapılandırma hakkında yönergeler verilmektedir. Azure Active Directory kimlik doğrulaması yalnızca OpenVPN Protokolü ve Windows çalıştıran istemciler kullanan ağ geçitleri için kullanılabilir.

Bu tür bir bağlantı, istemci bilgisayarda bir istemcinin yapılandırılmış olmasını gerektirir. Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).

Bu makalede şunları öğreneceksiniz:

* Sanal WAN oluşturma
* Sanal hub oluşturma
* Kullanıcı VPN yapılandırması oluşturma
* Sanal WAN Kullanıcı VPN profilini indir
* Kullanıcı VPN yapılandırmasını bir sanal hub 'a Uygula
* VNet 'i sanal hub 'a bağlama
* Kullanıcı VPN istemci yapılandırmasını indirme ve uygulama
* Sanal WAN 'nizi görüntüleme

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Başlamadan önce

Yapılandırmanıza başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Bağlanmak istediğiniz bir sanal ağınız var. Şirket içi ağlarınızın alt ağlarının hiçbirinin, bağlanmak istediğiniz sanal ağlarla çakışmadığından emin olun. Azure portal bir sanal ağ oluşturmak için [hızlı](../virtual-network/quick-create-portal.md)başlangıca bakın.

* Sanal ağınızda sanal ağ geçidi yok. Sanal ağınızda bir ağ geçidi (VPN veya ExpressRoute) varsa, tüm ağ geçitlerini kaldırmanız gerekir. Bu yapılandırma, sanal ağın bunun yerine sanal WAN hub ağ geçidine bağlanmasını gerektirir.

* Hub bölgenizden bir IP adresi aralığı edinin. Hub, sanal WAN tarafından oluşturulan ve kullanılan bir sanal ağ. Hub için belirttiğiniz adres aralığı, bağlandığınız mevcut sanal ağlarınızla çakışamaz. Ayrıca bağlandığınız şirket içi adres aralıklarıyla da çakışamaz. Şirket içi ağ yapılandırmanızda bulunan IP adresi aralıklarını tanımıyorsanız, sizin için bu ayrıntıları sağlayabilecek biriyle koordine edebilirsiniz.

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Sanal WAN oluşturma

Bir tarayıcıdan [Azure portalına](https://portal.azure.com) gidin ve Azure hesabınızla oturum açın.

1. Sanal WAN sayfasına gidin. Portalda **+Kaynak oluştur**’a tıklayın. Arama kutusuna **sanal WAN** yazın ve ENTER ' u seçin.
2. Sonuçlardan **sanal WAN** ' ı seçin. Sanal WAN sayfasında, **Oluştur** ' a tıklayarak WAN sayfası oluştur sayfasını açın.
3. **WAN oluştur** sayfasında, **temel bilgiler** sekmesinde aşağıdaki alanları girin:

   ![Sanal WAN](./media/virtual-wan-point-to-site-azure-ad/vwan.png)

   * **Abonelik**: Kullanmak istediğiniz aboneliği seçin.
   * **Kaynak grubu** -yeni oluştur veya var olanı kullan.
   * **Kaynak grubu konumu** -açılan listeden bir kaynak konumu seçin. WAN, global bir kaynaktır ve belirli bir bölgeyle sınırlı değildir. Ancak oluşturduğunuz WAN kaynağını daha kolay yönetmek ve bulmak için bir bölge seçmeniz gerekir.
   * **Ad** -WAN 'nizi çağırmak istediğiniz adı yazın.
   * **Şunu yazın:** Stand. Temel bir WAN oluşturursanız, yalnızca temel bir hub oluşturabilirsiniz. Temel hub 'lar yalnızca VPN siteden siteye bağlantı özelliğine sahiptir.
4. Alanları doldurmayı tamamladıktan sonra, **gözden geçir + oluştur**' u seçin.
5. Doğrulama geçtikten sonra, sanal WAN oluşturmak için **Oluştur** ' u seçin.

## <a name="create-an-empty-virtual-hub"></a><a name="site"></a>Boş bir sanal hub oluşturun

1. Sanal WAN 'ınız altında, hub ' ı seçin ve **+ yeni merkez**' e tıklayın.

   ![Ekran görüntüsü, yeni hub seçiliyken hub yapılandırması iletişim kutusunu gösterir.](media/virtual-wan-point-to-site-azure-ad/hub1.jpg)
2. Sanal hub Oluştur sayfasında aşağıdaki alanları girin.

   **Bölge** -sanal hub 'ı dağıtmak istediğiniz bölgeyi seçin.

   **Ad** -sanal hub 'ınızı çağırmak istediğiniz adı girin.

   **Hub özel adres alanı** -hub 'ın CIDR gösteriminde adres aralığı.

   ![Ekran görüntüsü, değer girebileceğiniz sanal hub oluştur bölmesini gösterir.](media/virtual-wan-point-to-site-azure-ad/hub2.jpg)  
3. **Gözden geçir ve oluştur**’a tıklayın.
4. **Doğrulama başarılı** sayfasında **Oluştur**' a tıklayın.

## <a name="create-a-new-user-vpn-configuration"></a><a name="site"></a>Yeni bir kullanıcı VPN yapılandırması oluştur

Bir kullanıcı VPN yapılandırması, uzak istemcileri bağlamaya yönelik parametreleri tanımlar.

1. Sanal WAN ' ın altında, **Kullanıcı VPN yapılandırması**' nı seçin.

   ![Ekran görüntüsü Kullanıcı V P N konfigürasyonlar menü öğesinin seçili olduğunu gösterir.](media/virtual-wan-point-to-site-azure-ad/aadportal1.jpg)

2. **+ Kullanıcı VPN yapılandırması oluştur**' a tıklayın.

   ![Ekran görüntüsü Kullanıcı oluşturma V P N yapılandırma bağlantısını gösterir.](media/virtual-wan-point-to-site-azure-ad/aadportal2.jpg)

3. Bilgileri girin ve **Oluştur**' a tıklayın.

   * **Yapılandırma adı** -Kullanıcı VPN yapılandırmanızı çağırmak istediğiniz adı girin.
   * **Tünel türü** -OpenVPN öğesini seçin.
   * **Kimlik doğrulama yöntemi** -Azure Active Directory seçin.
   * **Hedef kitle** -Azure AD kiracınızda KAYıTLı [Azure VPN](openvpn-azure-ad-tenant.md) kurumsal uygulamasının uygulama kimliğini yazın. 
   * **Enden** - `https://sts.windows.net/<your Directory ID>/`
   * **AAD kiracısı** - `https://login.microsoftonline.com/<your Directory ID>`
  
   ![Ekran görüntüsünde, değerleri girebileceğiniz yeni Kullanıcı V P N yapılandırma bölmesi görüntülenir.](media/virtual-wan-point-to-site-azure-ad/aadportal3.jpg)

## <a name="edit-hub-assignment"></a><a name="hub"></a>Hub atamasını düzenleme

1. Sanal WAN altındaki **hub** + dikey penceresine gidin.
2. VPN sunucusu yapılandırmasını ilişkilendirmek istediğiniz hub 'ı seçin ve üç nokta (...) simgesine tıklayın.

   ![Ekran görüntüsü, menüden sanal hub 'ı Düzenle ' nin seçili olduğunu gösterir.](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. **Sanal hub 'ı Düzenle**' ye tıklayın.
4. **Noktadan siteye ağ geçidini dahil** et onay kutusunu işaretleyin ve Istediğiniz **ağ geçidi ölçek birimini** seçin.

   ![Ekran görüntüsü, ağ geçidi ölçek birimini seçebileceğiniz sanal hub 'ı Düzenle iletişim kutusunu gösterir.](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. VPN istemcilerine atanan IP adreslerine ait **adres havuzunu** girin.
6. **Onayla**'ya tıklayın.
7. İşlemin tamamlanması 30 dakika kadar sürebilir.

## <a name="download-user-vpn-profile"></a><a name="device"></a>Kullanıcı VPN profilini indir

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN 'nizin sayfasında, **Kullanıcı VPN yapılandırması**' na tıklayın.
2. Sayfanın üst kısmında, **Kullanıcı VPN yapılandırması 'Nı indir**' e tıklayın.
3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.
4. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.

## <a name="configure-user-vpn-clients"></a>Kullanıcı VPN istemcilerini yapılandırma

Bağlanmak için, Azure VPN Istemcisini indirmeniz ve sanal ağa bağlanmak isteyen her bilgisayarda önceki adımlarda indirilen VPN istemci profilini içeri aktarmanız gerekir.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN &reg; Protokolü bağlantıları için desteklenir.
>

#### <a name="to-download-the-azure-vpn-client"></a>Azure VPN istemcisini indirmek için

Azure VPN Istemcisini indirmek için bu [bağlantıyı](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab) kullanın.

#### <a name="to-import-a-client-profile"></a><a name="import"></a>İstemci profilini içeri aktarmak için

1. Sayfasında **Içeri aktar**' ı seçin.

    ![Ekran görüntüsü artı menüsünden Seçili Içeri aktarmayı gösterir.](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Profil XML dosyasına gidin ve seçin. Dosya seçili olduğunda **Aç**' ı seçin.

    ![Ekran görüntüsü, bir dosyayı seçebileceğiniz açık bir iletişim kutusu gösterir.](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Profilin adını belirtin ve **Kaydet**' i seçin.

    ![Ekran görüntüsünde, eklenen bağlantı adı ve Kaydet düğmesi seçili görüntülenir.](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. VPN 'ye bağlanmak için **Bağlan** ' ı seçin.

    ![Ekran görüntüsü, yeni oluşturduğunuz bağlantı için Bağlan düğmesini gösterir.](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Bağlandıktan sonra simge yeşil ve **bağlı** olarak görünür.

    ![Ekran görüntüsü, bağlantıyı kesme seçeneğiyle bağlı bir durumda gösterir.](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="to-delete-a-client-profile"></a><a name="delete"></a>İstemci profilini silmek için

1. Silmek istediğiniz istemci profilinin yanındaki üç nokta (...) simgesini seçin. Ardından **Kaldır**' ı seçin.

    ![Ekran görüntüsü menüden Seçileni kaldır 'ı gösterir.](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Silmek için **Kaldır** ' ı seçin.

    ![Ekran görüntüsü, kaldırma veya Iptal etme seçeneğiyle bir onay iletişim kutusu gösterir.](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose-connection-issues"></a><a name="diagnose"></a>Bağlantı sorunlarını Tanıla

1. Bağlantı sorunlarını tanılamak için **Tanılama** aracını kullanabilirsiniz. Menüyü açığa çıkarmak için tanılamak istediğiniz VPN bağlantısının yanındaki üç nokta (...) simgesini seçin. Ardından **Tanıla**' yı seçin.

    ![Ekran görüntüsü, menüden seçili tanılamayı gösterir.](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. **Bağlantı özellikleri** sayfasında, **tanı Çalıştır**' ı seçin.

    ![Ekran görüntüsü, bir bağlantı için tanılama Çalıştır düğmesini gösterir.](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Kimlik bilgilerinizle oturum açın.

    ![Ekran görüntüsü bu eylem için oturum açma iletişim kutusunu gösterir.](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. Tanılama sonuçlarını görüntüleyin.

    ![Ekran görüntüsü, tanılamada sonuçları gösterir.](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Sanal WAN'ınızı görüntüleme

1. Sanal WAN'a gidin.
2. Genel bakış sayfasında haritadaki her bir nokta bir hub'ı temsil eder.
3. Hub'lar ve bağlantılar bölümünde herhangi bir hub'ın durumunu, sitesini, bölgesini, VPN bağlantısı durumunu ve gelen/giden baytları görüntüleyebilirsiniz.

## <a name="clean-up-resources"></a><a name="cleanup"></a>Kaynakları temizleme

Bu kaynaklara artık ihtiyacınız kalmadığında, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
