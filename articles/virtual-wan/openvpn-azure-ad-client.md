---
title: 'VPN Ağ Geçidi: OpenVPN protokolü P2S bağlantıları için VPN istemcisi: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulamasını kullanarak VNet'inize bağlanmak için P2S VPN'i kullanabilirsiniz
services: vpn-gateway
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/27/2020
ms.author: alzam
ms.openlocfilehash: edb509d43742aeecf74107ae8cb625aeafbccb9f
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385629"
---
# <a name="configure-a-vpn-client-for-p2s-openvpn-protocol-connections-azure-ad-authentication"></a>P2S OpenVPN iletişim kuralı bağlantıları için VPN istemcisini yapılandırma: Azure AD kimlik doğrulaması

Bu makale, Bir VPN istemcisini, Site'ye Nokta VPN ve Azure Active Directory kimlik doğrulamasını kullanarak sanal bir ağa bağlanmak üzere yapılandırmanıza yardımcı olur. Azure AD kullanarak bağlanıp kimliğini doğrulamadan önce azure AD kiracınızı yapılandırmanız gerekir. Daha fazla bilgi için [bkz.](openvpn-azure-ad-tenant.md)

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® iletişim iletişim bağlantıları için desteklenir.
>

## <a name="working-with-client-profiles"></a><a name="profile"></a>İstemci profilleriyle çalışma

Bağlanmak için Azure VPN İstemci'sini indirmeniz ve VNet'e bağlanmak isteyen her bilgisayarda bir VPN istemcisi profili yapılandırmanız gerekir. Bilgisayarda istemci profili oluşturabilir, dışa aktarabilir ve sonra ek bilgisayarlara aktarabilirsiniz.

### <a name="to-download-the-azure-vpn-client"></a>Azure VPN istemcisini indirmek için

Azure VPN İstemcisini indirmek için bu [bağlantıyı](https://go.microsoft.com/fwlink/?linkid=2117554) kullanın. Lütfen Azure VPN İstemcinin arka planda çalışma izni olduğundan emin olun. İznin denetimini/etkinleştirmek için aşağıdaki adımları izleyin:

1. Başlat'a gidin, ardından Arka Plan uygulamalarını > Gizlilik > Ayarları'nı seçin.
2. Arka Plan Uygulamaları altında, **uygulamaların arka planda çalışmasına izin** verin'in açık olduğundan emin olun.
3. Hangi uygulamaların arka planda çalıştırılabildiği ne var' ı seçin altında, Azure VPN İstemci ayarlarını **Aç'a çevirin.**

  ![Izni](./media/openvpn-azure-ad-client/backgroundpermission.png)

### <a name="to-create-a-certificate-based-client-profile"></a><a name="cert"></a>Sertifika tabanlı istemci profili oluşturmak için

Sertifika tabanlı bir profille çalışırken, istemci bilgisayara uygun sertifikaların yüklendiğinden emin olun. Sertifikalar hakkında daha fazla bilgi için [bkz.](certificates-point-to-site.md)

  ![cert](./media/openvpn-azure-ad-client/create/create-cert1.jpg)

### <a name="to-create-a-radius-client-profile"></a><a name="radius"></a>RADIUS istemci profili oluşturmak için

  ![Radıus](./media/openvpn-azure-ad-client/create/create-radius1.jpg)
  
> [!NOTE]
> Server Secret, P2S VPN istemci profilinde dışa aktarılabilir.  İstemci profilinin nasıl dışa aktarılaacağına ilişkin talimatları [burada](about-vpn-profile-download.md)bulabilirsiniz.
>

### <a name="to-export-and-distribute-a-client-profile"></a><a name="export"></a>İstemci profilini dışa aktarmak ve dağıtmak için

Çalışan bir profiliniz olduğunda ve diğer kullanıcılara dağıtmanız gerektiğinde, aşağıdaki adımları kullanarak bu profili dışa aktarabilirsiniz:

1. Dışa aktarmak istediğiniz VPN istemciprofilini vurgulayın, **... 'u**seçin, sonra **Dışa Aktar'ı**seçin.

    ![dışarı aktar](./media/openvpn-azure-ad-client/export/export1.jpg)

2. Bu profili kaydetmek istediğiniz konumu seçin, dosya adını olduğu gibi bırakın, ardından xml dosyasını kaydetmek için **Kaydet'i** seçin.

    ![dışarı aktar](./media/openvpn-azure-ad-client/export/export2.jpg)

### <a name="to-import-a-client-profile"></a><a name="import"></a>İstemci profili almak için

1. Sayfada, **İçe Aktar'ı**seçin.

    ![içeri aktar](./media/openvpn-azure-ad-client/import/import1.jpg)

2. Profil xml dosyasına göz atın ve seçin. Dosya seçilirken **Aç'ı**seçin.

    ![içeri aktar](./media/openvpn-azure-ad-client/import/import2.jpg)

3. Profilin adını belirtin ve **Kaydet'i**seçin.

    ![içeri aktar](./media/openvpn-azure-ad-client/import/import3.jpg)

4. VPN'e bağlanmak için **Bağlan'ı** seçin.

    ![içeri aktar](./media/openvpn-azure-ad-client/import/import4.jpg)

5. Bağlandıktan sonra simge yeşile döner ve **Bağlı**der.

    ![içeri aktar](./media/openvpn-azure-ad-client/import/import5.jpg)

### <a name="to-delete-a-client-profile"></a><a name="delete"></a>İstemci profilini silmek için

1. Silmek istediğiniz istemci profilinin yanındaki elipsleri seçin. Ardından **Kaldır'ı**seçin.

    ![delete](./media/openvpn-azure-ad-client/delete/delete1.jpg)

2. Silmek için **Kaldır'ı** seçin.

    ![delete](./media/openvpn-azure-ad-client/delete/delete2.jpg)

## <a name="create-a-connection"></a><a name="connection"></a>Bağlantı oluşturma

1. Sayfada , sonra **+** **+ Ekle'yi**seçin.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create1.jpg)

2. Bağlantı bilgilerini doldurun. Değerlerden emin değilseniz, yöneticinize başvurun. Değerleri doldurduktan sonra **Kaydet'i**seçin.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create2.jpg)

3. VPN'e bağlanmak için **Bağlan'ı** seçin.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create3.jpg)

4. Uygun kimlik bilgilerini seçin ve ardından **Devam et'i**seçin.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create4.jpg)

5. Başarılı bir şekilde bağlandıktan sonra simge yeşile döner ve **Bağlı**olduğunu söyler.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create5.jpg)

### <a name="to-connect-automatically"></a><a name="autoconnect"></a>Otomatik olarak bağlanmak için

Bu adımlar, bağlantınızı Her Zaman A'ya otomatik olarak bağlanmak üzere yapılandırmanıza yardımcı olur.

1. VPN istemcinizin ana sayfasında **VPN Ayarları'nı**seçin.

    ![auto](./media/openvpn-azure-ad-client/auto/auto1.jpg)

2. Geçiş uygulamaları iletişim kutusunda **Evet'i** seçin.

    ![auto](./media/openvpn-azure-ad-client/auto/auto2.jpg)

3. Ayarlamak istediğiniz bağlantının zaten bağlı olmadığından emin olun, ardından profili vurgulayın ve Connect onay kutusunu **otomatik olarak** işaretleyin.

    ![auto](./media/openvpn-azure-ad-client/auto/auto3.jpg)

4. VPN bağlantısını başlatmak için **Bağlan'ı** seçin.

    ![auto](./media/openvpn-azure-ad-client/auto/auto4.jpg)

## <a name="diagnose-connection-issues"></a><a name="diagnose"></a>Bağlantı sorunlarını tanılama

1. Bağlantı sorunlarını tanılamak için **Tanılama** aracını kullanabilirsiniz. Menüyü ortaya çıkarmak için tanılamak istediğiniz VPN bağlantısının yanındaki **...** seçeneğini belirleyin. Ardından **Tanıla'yı**seçin.

    ![Tanılamak](./media/openvpn-azure-ad-client/diagnose/diagnose1.jpg)

2. Bağlantı **Özellikleri** sayfasında Tanıyı **Çalıştır'ı**seçin.

    ![Tanılamak](./media/openvpn-azure-ad-client/diagnose/diagnose2.jpg)

3. Kimlik bilgilerinizle oturum açın.

    ![Tanılamak](./media/openvpn-azure-ad-client/diagnose/diagnose3.jpg)

4. Tanı sonuçlarını görüntüleyin.

    ![Tanılamak](./media/openvpn-azure-ad-client/diagnose/diagnose4.jpg)

## <a name="faq"></a>SSS

### <a name="how-do-i-add-dns-suffixes-to-the-vpn-client"></a>VPN istemcisine DNS soneklerini nasıl eklerim?

İndirilen profil XML dosyasını değiştirebilir ve ** \<dnsssonfix \<>dnsssufix> \< \</dnssufix>/dnsssonefixs>** etiketleri ekleyebilirsiniz

```
<azvpnprofile>
<clientconfig>

    <dnssuffixes>
          <dnssuffix>.mycorp.com</dnssuffix>
          <dnssuffix>.xyz.com</dnssuffix>
          <dnssuffix>.etc.net</dnssuffix>
    </dnssuffixes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="how-do-i-add-custom-dns-servers-to-the-vpn-client"></a>VPN istemcisine özel DNS sunucularını nasıl ekleyebilirim?

İndirilen profil XML dosyasını değiştirebilir ve ** \<dnsserver \<>\<dnsserver> \</dnsserver>/dnsservers>** etiketleri ekleyebilirsiniz

```
<azvpnprofile>
<clientconfig>

    <dnsservers>
        <dnsserver>x.x.x.x</dnsserver>
        <dnsserver>y.y.y.y</dnsserver>
    </dnsservers>
    
</clientconfig>
</azvpnprofile>
```

> [!NOTE]
> OpenVPN Azure AD istemcisi DNS Ad Çözümleme İlke Tablosu (NRPT) girişlerini kullanır, `ipconfig /all`bu da DNS sunucularının çıktıaltında listelenmeyeceği anlamına gelir. Kullanım içi DNS ayarlarınızı onaylamak için lütfen PowerShell'deki [Get-DnsClientNrptPolicy'ye](https://docs.microsoft.com/powershell/module/dnsclient/get-dnsclientnrptpolicy?view=win10-ps) başvurun.
>

### <a name="how-do-i-add-custom-routes-to-the-vpn-client"></a>VPN istemcisine özel yolları nasıl eklerim?

İndirilen profil XML dosyasını değiştirebilir ve ** \<rota>\<rotayı \<>\<hedef>\< \<maskesi> \</hedef>/maske>/rota>\</includeroutes>** etiketleri ekleyebilirsiniz

```
<azvpnprofile>
<clientconfig>

    <includeroutes>
        <route>
            <destination>x.x.x.x</destination><mask>24</mask>
        </route>
    </includeroutes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="how-do-i-block-exclude-routes-from-the-vpn-client"></a>VPN istemcisinden yolları nasıl engellerim (hariç tutabilirim)

İndirilen profil XML dosyasını değiştirebilir ve ** \<dışlanan rotaları>\<rotayı>\<hedef>\<maske \<> /hedef>\</maske>\</rota>\</dışlama rotaları>** etiketleri ekleyebilirsiniz

```
<azvpnprofile>
<clientconfig>

    <excluderoutes>
        <route>
            <destination>x.x.x.x</destination><mask>24</mask>
        </route>
    </excluderoutes>
    
</clientconfig>
</azvpnprofile>
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [bkz.](openvpn-azure-ad-tenant.md)
