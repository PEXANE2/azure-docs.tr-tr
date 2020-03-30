---
title: 'Sanal WAN: Farklı kullanıcı grupları için Azure AD kiracı: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulamasını kullanarak VNet'inize bağlanmak için P2S VPN'i kullanabilirsiniz
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: alzam
ms.openlocfilehash: af5ff5817ee9ae7e6d7432fe281ecb440bf25b9a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060711"
---
# <a name="create-an-azure-active-directory-tenant-for-p2s-openvpn-protocol-connections"></a>P2S OpenVPN iletişim kuralı bağlantıları için Azure Etkin Dizin kiracısı oluşturma

VNet'inize bağlanırken, sertifika tabanlı kimlik doğrulamasını veya RADIUS kimlik doğrulamasını kullanabilirsiniz. Ancak, Açık VPN iletişim kuralını kullandığınızda, Azure Etkin Dizin kimlik doğrulamasını da kullanabilirsiniz. Farklı kullanıcı kümesinin farklı ağ geçitlerine bağlanabilmesini istiyorsanız, AD'ye birden fazla uygulama kaydedebilir ve bunları farklı ağ geçitlerine bağlayabilirsiniz.

Bu makale, P2S OpenVPN kimlik doğrulaması için bir Azure AD kiracısı kurmanıza ve farklı kullanıcılar ve gruplar için farklı erişime izin vermek için Azure AD'de birden çok uygulama oluşturmanıza ve kaydetmenize yardımcı olur.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca&reg; OpenVPN iletişim iletişim bağlantıları için desteklenir.
>

[!INCLUDE [create](../../includes/openvpn-azure-ad-tenant-multi-app.md)]

## <a name="6-create-a-new-p2s-configuration"></a><a name="site"></a>6. Yeni bir P2S yapılandırması oluşturma

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
   > Yukarıdaki komutlarda Azure VPN istemcisinin uygulama kimliğini kullanmayın: Tüm kullanıcılara ağ geçidine erişim hakkı tanıyacaktır. Kaydolduğunuz başvuru(lar)ın kimliğini kullanın.

## <a name="7-edit-hub-assignment"></a><a name="hub"></a>7. Hub atamayı edin

1. Sanal WAN altında **Hubs** bıçak gidin.

2. Vpn sunucusu yapılandırmasını ilişkilendirmek istediğiniz hub'ı seçin ve elipsleri (...) tıklatın.

    ![yeni site](media/openvpn-azure-ad-tenant-multi-app/p2s4.jpg)

3. **Sanal hub'ı edit'i**tıklatın.

4. **Yer-iş geçidi ekle onay** kutusunu işaretleyin ve istediğiniz **Ağ Geçidi ölçeği birimini** seçin.

    ![yeni site](media/openvpn-azure-ad-tenant-multi-app/p2s2.jpg)

5. VPN istemcilerine IP adreslerinin atanacağı **Adres havuzunu** girin.

6. **Onayla**'ya tıklayın.

7. İşlemin tamamlanması 30 dakika kadar sürebilir.

## <a name="8-download-vpn-profile"></a><a name="device"></a>8. VPN profilini indirin

İstemcilerinizi yapılandırmak için VPN profilini kullanın.

1. Sanal WAN'ınız için sayfada **Kullanıcı VPN yapılandırmalarını**tıklatın.

2. Sayfanın üst kısmında, **kullanıcı VPN config indir'i**tıklatın.

3. Dosya oluşturulduktan sonra bağlantıya tıklayarak indirebilirsiniz.

4. VPN istemcilerini yapılandırmak için profil dosyasını kullanın.

5. İndirilen zip dosyasını ayıklayın.

6. Fermuarsız "AzureVPN" klasörüne göz atın.

7. "azurevpnconfig.xml" dosyasının konumuna dikkat edin. Azurevpnconfig.xml VPN bağlantısı için ayar içerir ve doğrudan Azure VPN İstemci uygulamasına içe aktarılabilir. Ayrıca, bu dosyayı e-posta veya başka yollarla bağlanması gereken tüm kullanıcılara da dağıtabilirsiniz. Kullanıcının başarılı bir şekilde bağlanmak için geçerli Azure REKLAM kimlik bilgilerine ihtiyacı olacaktır.

## <a name="9-configure-user-vpn-clients"></a>9. Kullanıcı VPN istemcilerini yapılandırın

Bağlanmak için Azure VPN İstemci'sini indirmeniz ve VNet'e bağlanmak isteyen her bilgisayarda önceki adımlarda indirilen VPN istemcisi profilini almanız gerekir.

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca&reg; OpenVPN iletişim iletişim bağlantıları için desteklenir.
>

#### <a name="to-download-the-azure-vpn-client"></a>Azure VPN istemcisini indirmek için

Azure VPN İstemcisini indirmek için bu [bağlantıyı](https://go.microsoft.com/fwlink/?linkid=2117554) kullanın.

#### <a name="to-import-a-client-profile"></a><a name="import"></a>İstemci profili almak için

1. Sayfada, **İçe Aktar'ı**seçin.

    ![içeri aktar](./media/openvpn-azure-ad-tenant-multi-app/import/import1.jpg)

2. Profil xml dosyasına göz atın ve seçin. Dosya seçilirken **Aç'ı**seçin.

    ![içeri aktar](./media/openvpn-azure-ad-tenant-multi-app/import/import2.jpg)

3. Profilin adını belirtin ve **Kaydet'i**seçin.

    ![içeri aktar](./media/openvpn-azure-ad-tenant-multi-app/import/import3.jpg)

4. VPN'e bağlanmak için **Bağlan'ı** seçin.

    ![içeri aktar](./media/openvpn-azure-ad-tenant-multi-app/import/import4.jpg)

5. Bağlandıktan sonra simge yeşile döner ve **Bağlı**der.

    ![içeri aktar](./media/openvpn-azure-ad-tenant-multi-app/import/import5.jpg)

#### <a name="to-delete-a-client-profile"></a><a name="delete"></a>İstemci profilini silmek için

1. Silmek istediğiniz istemci profilinin yanındaki elipsleri (...) seçin. Ardından **Kaldır'ı**seçin.

    ![delete](./media/openvpn-azure-ad-tenant-multi-app/delete/delete1.jpg)

2. Silmek için **Kaldır'ı** seçin.

    ![delete](./media/openvpn-azure-ad-tenant-multi-app/delete/delete2.jpg)

#### <a name="to-diagnose-connection-issues"></a><a name="diagnose"></a>Bağlantı sorunlarını tanılamak için

1. Bağlantı sorunlarını tanılamak için **Tanılama** aracını kullanabilirsiniz. Menüyü ortaya çıkarmak için tanılamak istediğiniz VPN bağlantısının yanındaki elipsleri (...) seçin. Ardından **Tanıla'yı**seçin.

    ![Tanılamak](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose1.jpg)

2. Bağlantı **Özellikleri** sayfasında Tanıyı **Çalıştır'ı**seçin.

    ![Tanılamak](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose2.jpg)

3. Kimlik bilgilerinizle oturum açın.

    ![Tanılamak](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose3.jpg)

4. Tanı sonuçlarını görüntüleyin.

    ![Tanılamak](./media/openvpn-azure-ad-tenant-multi-app/diagnose/diagnose4.jpg)

## <a name="10-view-your-virtual-wan"></a><a name="viewwan"></a>10. Sanal WAN'ınızı görüntüleyin

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
