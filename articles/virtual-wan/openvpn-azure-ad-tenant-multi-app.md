---
title: 'Sanal WAN: farklı Kullanıcı grupları için Azure AD kiracısı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulaması kullanarak sanal ağınıza bağlanmak için P2S VPN kullanabilirsiniz
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: alzam
ms.openlocfilehash: 4e667dcccd612873e8633c375bc3ce611e11c962
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77485957"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>P2S OpenVPN Protokolü bağlantıları için Azure Active Directory kiracısı oluşturma

Sanal ağınıza bağlanırken sertifika tabanlı kimlik doğrulama veya RADIUS kimlik doğrulaması kullanabilirsiniz. Ancak, açık VPN protokolünü kullandığınızda Azure Active Directory kimlik doğrulamasını da kullanabilirsiniz. Farklı kullanıcı kümesinin farklı VPN ağ geçitlerine bağlanmasını istiyorsanız, AD 'de birden çok uygulamayı kaydedebilir ve bunları farklı VPN ağ geçitlerine bağlayabilirsiniz.

Bu makale, P2S OpenVPN kimlik doğrulaması için bir Azure AD kiracısı ayarlamanıza ve farklı kullanıcılara ve gruplara farklı erişim sağlamak için Azure AD 'de birden çok uygulama oluşturup kaydetmenize yardımcı olur.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® Protokolü bağlantılarında desteklenir.
>

[!INCLUDE [create](../../includes/openvpn-azure-ad-tenant-multi-app.md)]

## <a name="site"></a>6. yeni bir P2S yapılandırması oluşturma

P2S yapılandırması, uzak istemcilerin bağlanmasına yönelik parametreleri tanımlar.

1. Ortamınız için gereken değerleri değiştirerek aşağıdaki değişkenleri ayarlayın.

   ```azurepowershell-interactive
   $aadAudience = "00000000-abcd-abcd-abcd-999999999999"
   $aadIssuer = "https://sts.windows.net/00000000-abcd-abcd-abcd-999999999999/"
   $aadTenant = "https://login.microsoftonline.com/00000000-abcd-abcd-abcd-999999999999"    
   ```

2. Yapılandırmayı oluşturmak için aşağıdaki komutları çalıştırın:

   ```azurepowershell-interactive
   $aadConfig = New-AzVpnServerConfiguration -ResourceGroupName <ResourceGroup> -Name newAADConfig -VpnProtocol OpenVPN -VpnAuthenticationType AAD -AadTenant $aadTenant -AadIssuer $aadIssuer -AadAudience $aadAudience -Location westcentralus
   ```

   > [!NOTE]
   > Yukarıdaki komutlarda Azure VPN istemcisinin uygulama KIMLIĞINI kullanmayın: tüm kullanıcılara VPN Gateway erişimi verir. Kaydettiğiniz uygulamaların KIMLIĞINI kullanın.

## <a name="hub"></a>7. hub atamasını Düzenle

1. Sanal WAN altındaki **hub** + dikey penceresine gidin.

2. VPN sunucusu yapılandırmasını ilişkilendirmek istediğiniz hub 'ı seçin ve üç nokta (...) simgesine tıklayın.

    ![yeni site](media/openvpn-azure-ad-tenant-multi-app/p2s4.jpg)

3. **Sanal hub 'ı Düzenle**' ye tıklayın.

4. **Noktadan siteye ağ geçidini dahil** et onay kutusunu işaretleyin ve Istediğiniz **ağ geçidi ölçek birimini** seçin.

    ![yeni site](media/openvpn-azure-ad-tenant-multi-app/p2s2.jpg)

5. VPN istemcilerine atanan IP adreslerine ait **adres havuzunu** girin.

6. **Onayla**'ya tıklayın.

7. İşlemin tamamlanması 30 dakika kadar sürebilir.

## <a name="device"></a>8. VPN profilini indir

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN 'nizin sayfasında, **Kullanıcı VPN yapılandırması**' na tıklayın.

2. Sayfanın üst kısmında, **Kullanıcı VPN yapılandırması 'Nı indir**' e tıklayın.

3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.

4. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.

5. İndirilen ZIP dosyasını ayıklayın.

6. Sıkıştırılmış olmayan "AzureVPN" klasörüne gidin.

7. "Azurevpnconfig. xml" dosyasının konumunu bir yere unutmayın. Azurevpnconfig. xml, VPN bağlantısı ayarını içerir ve doğrudan Azure VPN Istemci uygulamasına aktarılabilir. Ayrıca, bu dosyayı e-posta veya başka yollarla bağlanması gereken tüm kullanıcılara dağıtabilirsiniz. Kullanıcının başarıyla bağlanması için geçerli bir Azure AD kimlik bilgilerine ihtiyacı olacak.

## <a name="9-configure-user-vpn-clients"></a>9. Kullanıcı VPN istemcilerini yapılandırma

Bağlanmak için, Azure VPN Istemcisini indirmeniz ve sanal ağa bağlanmak isteyen her bilgisayarda önceki adımlarda indirilen VPN istemci profilini içeri aktarmanız gerekir.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® Protokolü bağlantılarında desteklenir.
>

#### <a name="to-download-the-azure-vpn-client"></a>Azure VPN istemcisini indirmek için

Azure VPN Istemcisini indirmek için bu [bağlantıyı](https://go.microsoft.com/fwlink/?linkid=2117554) kullanın.

#### <a name="import"></a>İstemci profilini içeri aktarmak için

1. Sayfasında **Içeri aktar**' ı seçin.

    ![aktarmaya](./media/openvpn-azure-ad-tenant-multi-app/import/import1.jpg)

2. Profil XML dosyasına gidin ve seçin. Dosya seçili olduğunda **Aç**' ı seçin.

    ![aktarmaya](./media/openvpn-azure-ad-tenant-multi-app/import/import2.jpg)

3. Profilin adını belirtin ve **Kaydet**' i seçin.

    ![aktarmaya](./media/openvpn-azure-ad-tenant-multi-app/import/import3.jpg)

4. VPN 'ye bağlanmak için **Bağlan** ' ı seçin.

    ![aktarmaya](./media/openvpn-azure-ad-tenant-multi-app/import/import4.jpg)

5. Bağlandıktan sonra simge yeşil ve **bağlı**olarak görünür.

    ![aktarmaya](./media/openvpn-azure-ad-tenant-multi-app/import/import5.jpg)

#### <a name="delete"></a>İstemci profilini silmek için

1. Silmek istediğiniz istemci profilinin yanındaki üç nokta (...) simgesini seçin. Ardından **Kaldır**' ı seçin.

    ![delete](./media/openvpn-azure-ad-tenant-multi-app/delete/delete1.jpg)

2. Silmek için **Kaldır** ' ı seçin.

    ![delete](./media/openvpn-azure-ad-tenant-multi-app/delete/delete2.jpg)

#### <a name="diagnose"></a>Bağlantı sorunlarını tanılamak için

1. Bağlantı sorunlarını tanılamak için **Tanılama** aracını kullanabilirsiniz. Menüyü açığa çıkarmak için tanılamak istediğiniz VPN bağlantısının yanındaki üç nokta (...) simgesini seçin. Ardından **Tanıla**' yı seçin.

    ![tanın](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose1.jpg)

2. **Bağlantı özellikleri** sayfasında, **tanı Çalıştır**' ı seçin.

    ![tanın](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose2.jpg)

3. Kimlik bilgilerinizle oturum açın.

    ![tanın](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose3.jpg)

4. Tanılama sonuçlarını görüntüleyin.

    ![tanın](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose4.jpg)

## <a name="viewwan"></a>10. sanal WAN 'nizi görüntüleyin

1. Sanal WAN'a gidin.

2. Genel bakış sayfasında haritadaki her bir nokta bir hub'ı temsil eder. Hub sistem durumu özetini görüntülemek için noktalardan birinin üzerine gidin.

3. Hub'lar ve bağlantılar bölümünde herhangi bir hub'ın durumunu, sitesini, bölgesini, VPN bağlantısı durumunu ve gelen/giden baytları görüntüleyebilirsiniz.

## <a name="viewhealth"></a>11. kaynak sistem durumunu görüntüleyin

1. WAN'ınıza gidin.

2. WAN sayfanızın **Destek ve sorun giderme** bölümünde **Sistem durumu**'na tıklayın ve kaynağınızı görüntüleyin.

## <a name="cleanup"></a>Kaynakları Temizleme

Bu kaynaklar artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz. "myResourceGroup" yerine kaynak grubunuzun adını yazın ve aşağıdaki PowerShell komutunu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
