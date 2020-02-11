---
title: 'VPN Gateway: OpenVPN Protokolü P2S bağlantıları için VPN istemcisi: Azure AD kimlik doğrulaması'
description: Azure AD kimlik doğrulaması kullanarak sanal ağınıza bağlanmak için P2S VPN kullanabilirsiniz
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 02/07/2020
ms.author: alzam
ms.openlocfilehash: 3072fc3a82cfe85649cf080c9def69cc4869b7ec
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77122509"
---
# <a name="configure-a-vpn-client-for-p2s-openvpn-protocol-connections-azure-ad-authentication"></a>P2S OpenVPN Protokolü bağlantıları için bir VPN istemcisi yapılandırma: Azure AD kimlik doğrulaması

Bu makale, bir VPN istemcisini Noktadan siteye VPN ve Azure Active Directory kimlik doğrulaması kullanarak bir sanal ağa bağlanacak şekilde yapılandırmanıza yardımcı olur. Azure AD 'yi kullanarak bağlanabilmek ve kimlik doğrulayabilmeniz için önce Azure AD kiracınızı yapılandırmanız gerekir. Daha fazla bilgi için bkz. [Azure AD kiracısı yapılandırma](openvpn-azure-ad-tenant.md).

> [!NOTE]
> Azure AD kimlik doğrulaması yalnızca OpenVPN® Protokolü bağlantılarında desteklenir.
>

## <a name="profile"></a>İstemci profilleriyle çalışma

Bağlanmak için, Azure VPN Istemcisini indirmeniz ve sanal ağa bağlanmak isteyen her bilgisayarda bir VPN istemci profili yapılandırmanız gerekir. Bir bilgisayarda istemci profili oluşturabilir, dışarı aktarabilir ve daha sonra ek bilgisayarlara aktarabilirsiniz.

### <a name="to-download-the-azure-vpn-client"></a>Azure VPN istemcisini indirmek için

Azure VPN Istemcisini indirmek için bu [bağlantıyı](https://go.microsoft.com/fwlink/?linkid=2117554) kullanın.

### <a name="cert"></a>Sertifika tabanlı bir istemci profili oluşturmak için

Sertifika tabanlı bir profille çalışırken, istemci bilgisayarda uygun sertifikaların yüklü olduğundan emin olun. Sertifikalar hakkında daha fazla bilgi için bkz. [istemci sertifikalarını Install](point-to-site-how-to-vpn-client-install-azure-cert.md).

  ![CERT](./media/openvpn-azure-ad-client/create/create-cert1.jpg)

### <a name="radius"></a>RADIUS istemci profili oluşturmak için

  ![La](./media/openvpn-azure-ad-client/create/create-radius1.jpg)
  
> [!NOTE]
> Sunucu parolası P2S VPN istemci profiline aktarılabilir.  İstemci profilini dışarı aktarma yönergeleri [burada](about-vpn-profile-download.md)bulunabilir.
>

### <a name="export"></a>Bir istemci profilini dışarı ve dağıtım

Çalışma profiliniz varsa ve diğer kullanıcılara dağıtmanız gerekiyorsa, aşağıdaki adımları kullanarak dışarı aktarabilirsiniz:

1. Dışarı aktarmak istediğiniz VPN istemci profilini vurgulayın, **...** öğesini seçin ve ardından **dışarı aktar**' ı seçin.

    ![işlemi](./media/openvpn-azure-ad-client/export/export1.jpg)

2. Bu profili kaydetmek istediğiniz konumu seçin, dosya adını olduğu gibi bırakın ve **Kaydet** ' i seçerek XML dosyasını kaydedin.

    ![işlemi](./media/openvpn-azure-ad-client/export/export2.jpg)

### <a name="import"></a>İstemci profilini içeri aktarmak için

1. Sayfasında **Içeri aktar**' ı seçin.

    ![aktarmaya](./media/openvpn-azure-ad-client/import/import1.jpg)

2. Profil XML dosyasına gidin ve seçin. Dosya seçili olduğunda **Aç**' ı seçin.

    ![aktarmaya](./media/openvpn-azure-ad-client/import/import2.jpg)

3. Profilin adını belirtin ve **Kaydet**' i seçin.

    ![aktarmaya](./media/openvpn-azure-ad-client/import/import3.jpg)

4. VPN 'ye bağlanmak için **Bağlan** ' ı seçin.

    ![aktarmaya](./media/openvpn-azure-ad-client/import/import4.jpg)

5. Bağlandıktan sonra simge yeşil ve **bağlı**olarak görünür.

    ![aktarmaya](./media/openvpn-azure-ad-client/import/import5.jpg)

### <a name="delete"></a>İstemci profilini silmek için

1. Silmek istediğiniz istemci profilinin yanındaki üç nokta simgesini seçin. Ardından **Kaldır**' ı seçin.

    ![delete](./media/openvpn-azure-ad-client/delete/delete1.jpg)

2. Silmek için **Kaldır** ' ı seçin.

    ![delete](./media/openvpn-azure-ad-client/delete/delete2.jpg)

## <a name="connection"></a>Bağlantı oluştur

1. Sayfada **+** ve ardından **+ Ekle**' yi seçin.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create1.jpg)

2. Bağlantı bilgilerini doldurun. Değerlerden emin değilseniz yöneticinize başvurun. Değerleri doldurduktan sonra **Kaydet**' i seçin.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create2.jpg)

3. VPN 'ye bağlanmak için **Bağlan** ' ı seçin.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create3.jpg)

4. Doğru kimlik bilgilerini seçip **devam**' ı seçin.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create4.jpg)

5. Başarılı bir şekilde bağlandıktan sonra simge yeşil ve **bağlı**olarak görünür.

    ![bağlantı](./media/openvpn-azure-ad-client/create/create5.jpg)

### <a name="autoconnect"></a>Otomatik olarak bağlanmak için

Bu adımlar, bağlantınızı her zaman açık ile otomatik olarak bağlanacak şekilde yapılandırmanıza yardımcı olur.

1. VPN istemcinizin ana sayfasında **VPN ayarları**' nı seçin.

    ![auto](./media/openvpn-azure-ad-client/auto/auto1.jpg)

2. Uygulamalar arasında geçiş yap iletişim kutusunda **Evet** ' i seçin.

    ![auto](./media/openvpn-azure-ad-client/auto/auto2.jpg)

3. Ayarlamak istediğiniz bağlantının zaten bağlı olmadığından emin olun, sonra profili vurgulayın ve **otomatik olarak bağlan** onay kutusunu işaretleyin.

    ![auto](./media/openvpn-azure-ad-client/auto/auto3.jpg)

4. VPN bağlantısını başlatmak için **Bağlan** ' ı seçin.

    ![auto](./media/openvpn-azure-ad-client/auto/auto4.jpg)

## <a name="diagnose"></a>Bağlantı sorunlarını Tanıla

1. Bağlantı sorunlarını tanılamak için **Tanılama** aracını kullanabilirsiniz. Menüyü açığa çıkarmak için tanılamak istediğiniz VPN bağlantısının yanındaki **...** seçeneğini belirleyin. Ardından **Tanıla**' yı seçin.

    ![tanın](./media/openvpn-azure-ad-client/diagnose/diagnose1.jpg)

2. **Bağlantı özellikleri** sayfasında, **tanı Çalıştır**' ı seçin.

    ![tanın](./media/openvpn-azure-ad-client/diagnose/diagnose2.jpg)

3. Kimlik bilgilerinizle oturum açın.

    ![tanın](./media/openvpn-azure-ad-client/diagnose/diagnose3.jpg)

4. Tanılama sonuçlarını görüntüleyin.

    ![tanın](./media/openvpn-azure-ad-client/diagnose/diagnose4.jpg)

## <a name="faq"></a>SSS

### <a name="how-do-i-add-dns-suffixes-to-the-vpn-client"></a>Nasıl yaparım? VPN istemcisine DNS sonekleri eklensin mi?

İndirilen profil XML dosyasını değiştirebilir ve **\<dnssuffixes >\<dnssudüzeltmesini > \</dnssudüzeltmesini >\</dnssuffixes >** etiketleri ekleyebilirsiniz

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

### <a name="how-do-i-add-custom-routes-to-the-vpn-client"></a>Nasıl yaparım? VPN istemcisine özel yollar eklensin mi?

İndirilen profil XML dosyasını değiştirebilir ve **\<route >\<ıncluderoutes >\<destination >\<mask > \</Route >\</ıncluderoutes >\</destionation >\</Mask >** etiketleri ekleyebilirsiniz

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

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure AD kimlik doğrulaması kullanan P2S Open VPN bağlantıları için Azure Active Directory kiracı oluşturma](openvpn-azure-ad-tenant.md).
