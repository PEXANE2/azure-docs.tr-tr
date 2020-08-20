---
title: FortiGate dağıtım kılavuzu | Microsoft Docs
description: Azure Active Directory ve FortiGate SSL VPN arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 18a3d9d5-d81c-478c-be7e-ef38b574cb88
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/11/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 999e19ffad1d18e163881c844cbf30f8b7fef574
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88658483"
---
# <a name="fortigate-deployment-guide"></a>FortiGate dağıtım kılavuzu

## <a name="contents"></a>İçindekiler

- FortiGate lisansını benimseme
- Üretici yazılımını indirme
- FortiGate sanal makinesini dağıtma
   - Bir kara genel IP adresi ayarlayın ve tam etki alanı adı atayın
   - TCP bağlantı noktası için yeni bir gelen ağ güvenlik grubu kuralı oluştur
- FortiGate için özel bir Azure uygulaması oluşturma
- Grup eşleştirme için hazırlanma
   - Kullanıcılar için Grup oluşturma
- FortiGate VM 'sini yapılandırma
   - Lisansı yükler
   - Üretici yazılımını güncelleştirme
   - Yönetim bağlantı noktasını TCP olarak değiştirme
   - Azure Active Directory SAML Imzalama sertifikasını karşıya yükleyin
   - Özel bir SSL sertifikası yükleme ve yapılandırma
   - Komut satırı yapılandırması gerçekleştir
   - VPN portalları ve güvenlik duvarı Ilkesi oluşturma
- Azure kullanarak oturum açmayı test etme

## <a name="redeeming-the-fortigate-license"></a>FortiGate lisansını benimseme

Fortinet FortiGate yeni nesil güvenlik duvarı ürünü, Azure IaaS 'de bir sanal makine olarak sunulmaktadır. Bu sanal makine için iki lisans modu vardır:

- Kullandıkça Öde (PAYG)
- Kendi lisansınızı getirin (KLG)

Güvenli karma erişim (SHA) Kılavuzu sağlamak için Fortinet ile ortaklık yaparken, Fortinet, Azure AD üyelerine lisanslarla üretim SHA ekibine ulaşın. Lisansın sağlanmadığı durumlarda, PAYG dağıtımı da çalışır.

Bir Lisansın verildiği durumlarda, Fortinet çevrimiçi olarak kullanılması gereken bir kayıt kodu sağlar

![Fortigate SSL VPN](registration-code.png)

1. Kayıt https://support.fortinet.com/
2. Kayıt sonrasında, oturum açma https://support.fortinet.com/
3. **Varlık** -> **Kaydet/etkinleştir** 'e gidin
4. Fortinet tarafından sunulan kayıt kodunu girin
5. Kayıt kodunu belirtin, **ürünü kamu dışı bir kullanıcı tarafından kullanılacak şekilde** seçin ve **İleri** ' ye tıklayın.
6. Bir ürün açıklaması girin (ör. FortiGate), Fortinet Iş ortağını **diğer** > **Microsoft** olarak ayarlayın ve **İleri** ' ye tıklayın.
7. **Fortinet ürün kayıt anlaşmasını** kabul edin ve **İleri** ' ye tıklayın.
8. **Koşulları** kabul edin ve **Onayla** ' ya tıklayın.
9. **Lisans dosyasına** tıklayın ve lisansı daha sonra kaydedin


## <a name="download-firmware"></a>Üretici yazılımını indirme

Yazma sırasında, Fortinet FortiGate Azure VM 'si SAML kimlik doğrulaması için gerekli olan bellenim sürümü ile birlikte gelmez. En son sürüm, Fortinet 'ten alınmalıdır.

1. Oturum açma: https://support.fortinet.com/
2. **İndirme** -> **bellenim görüntülerini** gezin
3. **Sürüm notlarının** sağında **İndir** ' e tıklayın
4. V6 ' e tıklayın **.**
5. 6 ' ya tıklayın **.**
6. 6,4 ' e tıklayın **.**
7. Aynı satırdaki **https** bağlantısına tıklayarak **FGT_VM64_AZURE-v6-build1637-FORTINET. out** öğesini indirin
8. Dosyayı daha sonra Kaydet


## <a name="deploy-the-fortigate-vm"></a>FortiGate sanal makinesini dağıtma

1. https://portal.azure.comFortiGate sanal makinesini dağıtmak istediğiniz aboneliğe gidin ve oturum açın
2. Yeni bir kaynak grubu oluşturun veya FortiGate sanal makinesini dağıtmak istediğiniz kaynak grubunu açın
3. **Ekle** 'ye tıklayın
4. **Market** iletişim kutusuna "FORTI" yazın ve **Fortinet Fortigate yeni** **nesil güvenlik duvarını** seçin
5. Yazılım planını seçin (KLG bir lisansınız varsa veya bu lisans varsa, **Oluştur** ' a tıklayın).
6. VM yapılandırmasını doldurma

    ![Fortigate SSL VPN](virtual-machine.png)

7. Kimlik doğrulama türünü **parola** olarak AYARLAYıN ve VM için yönetici kimlik bilgilerini belirtin
8. **Gözden geçir + oluştur** 'a tıklayın
9. **Oluştur** seçeneğine tıklayın
10. VM dağıtımının tamamlanmasını bekle


### <a name="set-a-statuc-public-ip-address-and-assign-a-fully-qualified-domain-name"></a>Bir kara genel IP adresi ayarlayın ve tam etki alanı adı atayın

Tutarlı bir kullanıcı deneyimi için, FortiGate sanal makinesine atanan genel IP adresini statik olarak atanmak üzere ayarlamanız tercih edilir. Buna ek olarak, tam etki alanı adıyla eşleme aynı nedenlerle da yararlıdır.

_Statik bir genel IP adresi ayarlama_

1. https://portal.azure.comFortiGate VM 'sine gidin ve ayarları açın
2. **Genel bakış** EKRANıNDA genel IP adresine tıklayın

    ![Fortigate SSL VPN](public-ip-address.png)

3. **Statik** ' a ve ardından **Kaydet** ' e tıklayın

_Tam etki alanı adı ata_

FortiGate VM 'nin dağıtıldığı ortam için genel olarak yönlendirilebilir bir etki alanı adı sahibiyseniz, sanal makine için, yukarıda statik olarak atanan genel IP adresiyle eşleşen bir ana bilgisayar (A) kaydı oluşturun.

### <a name="create-a-new-inbound-network-security-group-rule-for-tcp-port"></a>TCP bağlantı noktası için yeni bir gelen ağ güvenlik grubu kuralı oluştur

1. https://portal.azure.comFortiGate VM 'sine gidin ve ayarları açın
2. Sol taraftaki menüde **ağ** ' a tıklayın. Ağ arabirimi listelenecektir ve gelen bağlantı noktası kuralları görüntülenir
3. **Gelen bağlantı noktası kuralı ekle** ' ye tıklayın
4. TCP 8443 için yeni bir gelen bağlantı noktası kuralı oluşturma

    ![Fortigate SSL VPN](port-rule.png)

5. **Ekle** 'ye tıklayın


## <a name="create-a-custom-azure-app-for-fortigate"></a>FortiGate için özel bir Azure uygulaması oluşturma

1. https://portal.azure.comFortiGate oturum açma işlemleri Için kimlik sağlayacak olan kiracı için Azure Active Directory dikey penceresine gidin ve açın
2. Sol taraftaki menüde **Kurumsal uygulamalar** ' a tıklayın
3. **Yeni uygulama** ' ya tıklayın
4. **Galeri dışı uygulamaya** tıklayın
5. Bir ad sağlayın (ör. FortiGate) ve **Ekle** ' ye tıklayın.
6. Sol taraftaki menüdeki **Kullanıcılar ve gruplar** ' a tıklayın
7. Oturum açıp **ata** 'ya tıklalayabilecek kullanıcıları ekleyin
8. Sol taraftaki menüden **Çoklu oturum açma** ' ya tıklayın
9. **SAML** 'ye tıklayın
10. **Temel SAML yapılandırması** altında, yapılandırmayı düzenlemek için kurşun kaleme tıklayın
11. Yapılandırma
    - Tanımlayıcı (varlık KIMLIĞI) `https://<address>/remote/saml/metadata`
    - Yanıt URL 'SI (onaylama tüketici hizmeti URL 'SI)  `https://<address>/remote/saml/login`
    - Oturum kapatma URL 'SI `https://<address>/remote/saml/logout`

    Burada, `address` FortiGate sanal makinesine atanan FQDN veya genel IP adresi

    Bu URL 'lerin her birini daha sonra kullanılmak üzere Kaydet –

    - Varlık KIMLIĞI
    - Yanıt URL'si
    - Oturum kapatma URL 'SI
12. **Kaydet**’e tıklayın
13. Temel SAML yapılandırmasını kapatma
14. **3 – SAML Imzalama sertifikası** ' nın altında, **sertifikayı indirin (base64)** ve daha sonra kaydedin
15. **4 – ayarla (uygulama adı)** altında Azure oturum açma URL 'sini, Azure AD tanımlayıcıyı ve Azure oturum açma URL 'sini kopyalayın ve daha sonra kaydedin
    - Azure oturum açma URL 'SI
    - Azure AD tanımlayıcısı
    - Azure oturum kapatma URL 'SI
16. **2 – Kullanıcı öznitelikleri ve talepler** altında, yapılandırmayı düzenlemek için kurşun kaleme tıklayın
17. **Yeni talep Ekle** 'ye tıklayın
18. Adı **Kullanıcı** adı olarak ayarla
19. Kaynak özniteliğini **User. UserPrincipalName** olarak ayarla
20. **Kaydet**’e tıklayın
21. **Grup talebi ekle** ' ye tıklayın
22. **Tüm grupları** Seç
23. **Grup talebinin adını özelleştirmeyi** denetle
24. Adı **Grup** olarak ayarla
25. **Kaydet**’e tıklayın


## <a name="prepare-for-group-matching"></a>Grup eşleştirme için hazırlanma

FortiGate, grup üyeliğine göre oturum açtıktan sonra farklı Kullanıcı Portalı deneyimlerine izin verir. Örneğin, pazarlama grubu ve finans grubu için bir deneyim olabilir.

Bunu aşağıdaki şekilde yapılandırın:

### <a name="create-groups-for-users"></a>Kullanıcılar için Grup oluşturma

1. https://portal.azure.comFortiGate oturum açma işlemleri Için kimlik sağlayacak olan kiracı için Azure Active Directory dikey penceresine gidin ve açın
2. **Gruplar** 'a tıklayın
3. **Yeni Grup** ' a tıklayın
4. İle grup oluşturma
    - Grup türü = güvenlik
    - Grup adı = `a meaningful name`
    - Grup açıklaması = `a meaningful description for the group`
    - Üyelik türü = atandı
    - Üyeler = `users for the user experience that will map to this group`
5. Ek kullanıcı deneyimleri için 3 ve 4 numaralı adımları tekrarlayın
6. Gruplar oluşturulduktan sonra her bir grubu seçin ve her biri için nesne KIMLIĞINI kaydedin
7. Daha sonra bu nesne kimliklerini ve grup adlarını Kaydet


## <a name="configure-the-fortigate-vm"></a>FortiGate VM 'sini yapılandırma

### <a name="install-the-license"></a>Lisansı yükler

1. `https://<address>` sayfasına gidin

    `address`FortiGate sanal makinesine atanan FQDN veya genel IP adresi aşağıda verilmiştir

2. Tüm sertifika hatalarını devam ettirmek
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın
4. Dağıtım KLG modelini kullanıyorsa, lisans yükleme istemi görüntülenir. Daha önce oluşturulan lisans dosyasını seçin ve karşıya yükleyin, **Tamam** ' a tıklayın ve FortiGate sanal makinesini yeniden başlatın:

    ![Fortigate SSL VPN](license.png)

5. Yeniden başlatmadan sonra, lisansı doğrulamak için yönetici kimlik bilgileriyle tekrar oturum açın

### <a name="update-firmware"></a>Üretici yazılımını güncelleştirme

1. `https://<address>` sayfasına gidin

    `address`FortiGate sanal makinesine atanan FQDN veya genel IP adresi aşağıda verilmiştir

2. Tüm sertifika hatalarını devam ettirmek
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın
4. Sol taraftaki menüde **sistem** ' e tıklayın.
5. Sol taraftaki menüde sistem altında, **bellenim** ' a tıklayın.
6. Bellenim yönetimi sayfasında, **Araştır** ' a tıklayın ve daha önce indirilen bellenim dosyasını seçin
7. Uyarıyı yoksayın ve **yedekleme yapılandırması ve yükseltme** ' ye tıklayın.

    ![Fortigate SSL VPN](backup-configure-upgrade.png)

8. **Devam** ' a tıklayın
9. FortiGate yapılandırmasını (. conf dosyası olarak) kaydetmeniz istendiğinde **Kaydet** ' e tıklayın.
10. Üretici yazılımının uygulanması için ve FortiGate VM 'nin yeniden başlatılması için bekleyin
11. FortiGate sanal makinesi yeniden başlatıldıktan sonra, yönetici kimlik bilgileriyle tekrar oturum açın
12. Pano kurulumunu gerçekleştirmek isteyip istemediğiniz sorulduğunda, **İleri** ' ye tıklayın.
13. Öğretici videosu başladığında **Tamam** ' a tıklayın.

### <a name="change-the-management-port-to-tcp"></a>Yönetim bağlantı noktasını TCP olarak değiştirme

1. `https://<address>` sayfasına gidin

    `address`FortiGate sanal makinesine atanan FQDN veya genel IP adresi aşağıda verilmiştir

2. Tüm sertifika hatalarını devam ettirmek
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın
4. Sol taraftaki menüde **sistem** ' e tıklayın.
5. Yönetim ayarları altında HTTPS bağlantı noktasını **8443** olarak değiştirin
6. **Uygula** ' ya tıklayın
7. Değişiklik uygulandıktan sonra tarayıcı, yönetim sayfasını yeniden yüklemeye çalışır, ancak başarısız olur. Bu andan itibaren yönetim sayfası adresi `https://<address>`

    ![Fortigate SSL VPN](certificate.png)

### <a name="upload-the-azure-active-directory-saml-signing-certificate"></a>Azure Active Directory SAML Imzalama sertifikasını karşıya yükleyin

1. `https://<address>` sayfasına gidin

    `address`FortiGate sanal makinesine atanan FQDN veya genel IP adresi aşağıda verilmiştir

2. Tüm sertifika hatalarını devam ettirmek
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın
4. Sol taraftaki menüde **sistem** ' e tıklayın.
5. Sistem altında **Sertifikalar** ' a tıklayın.
6. > **uzak sertifikayı** **içeri aktar** ' a tıklayın
7. Azure kiracısındaki FortiGate özel uygulama dağıtımından indirilen sertifikaya gidin, seçin ve **Tamam 'a** tıklayın

### <a name="upload-and-configure-a-custom-ssl-certificate"></a>Özel bir SSL sertifikası yükleme ve yapılandırma

FortiGate sanal makinesini, kullanmakta olduğunuz FQDN 'yi destekleyen kendi SSL sertifikanız ile yapılandırmak isteyebilirsiniz. İçindeki özel anahtarla paketlenmiş bir SSL sertifikasına erişiminiz varsa. PFX biçimi, bu amaçla kullanılıyor olabilir

1. `https://<address>` sayfasına gidin

    `address`FortiGate sanal makinesine atanan FQDN veya genel IP adresi aşağıda verilmiştir

2. Tüm sertifika hatalarını devam ettirmek
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın
4. Sol taraftaki menüde **sistem** ' e tıklayın.
5. Sistem altında **Sertifikalar** ' a tıklayın.
6. **İçeri** > **yerel sertifika** ' e tıklayın
7. **PKCS #12 sertifikası** ' na tıklayın
8. Öğesine gidin. SSL sertifikasını ve özel anahtarı içeren PFX dosyası
9. Sağlayın. PFX parolası
10. Sertifika için anlamlı bir ad sağlayın
11. **Tamam 'a** tıklayın
12. Sol taraftaki menüde **sistem** ' e tıklayın.
13. Sistem altında **Ayarlar** ' a tıklayın.
14. Yönetim ayarları altında, HTTPS sunucu sertifikası ' nın yanındaki açılan ayarı genişletin ve yukarıya içeri aktarılan SSL sertifikasını seçin
15. **Uygula** ' ya tıklayın
16. Tarayıcı penceresini kapatın ve sonra yeniden şuraya gidin `https://<address>`
17. FortiGate yönetici kimlik bilgileriyle oturum açın ve kullanımda olan doğru SSL sertifikasını gözlemleyin


### <a name="perform-command-line-configuration"></a>Komut satırı yapılandırması gerçekleştir

_SAML kimlik doğrulaması için komut satırı yapılandırması gerçekleştir_

1. https://portal.azure.comFortiGate VM 'sine gidin ve ayarları açın
2. Sol taraftaki menüden **seri konsol** ' e tıklayın.
3. Sanal Makine Yöneticisi kimlik bilgileriyle seri konsolundaki oturum açma

    Sonraki adımda, daha önce kaydedilen URL 'Ler gerekecektir. Yani

    - Varlık KIMLIĞI
    - Yanıt URL'si
    - Oturum kapatma URL 'SI
    - Azure oturum açma URL 'SI
    - Azure AD tanımlayıcısı
    - Azure oturum kapatma URL 'SI
4. Seri konsolunda aşağıdaki komutları yürütün –

    ```
    config user saml
    edit azure
    set entity-id <Entity ID>
    set single-sign-on-url <Reply URL>
    set single-logout-url <Logout URL>
    set idp-single-sign-on-url <Azure Login URL>
    set idp-entity-id <Azure AD Identifier>
    set idp-single-logout-url <Azure Logout URL>
    set idp-cert REMOTE_Cert_
    set user-name username
    set group-name group
    end
    ```
    > [!NOTE]
    > Azure oturum kapatma URL 'SI var mı? inde. Bu, FortiGate seri konsoluna doğru bir şekilde sağlanması için özel bir anahtar sırası gerektirir. URL genellikle-

    `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1`

    Bunu seri konsolunda sağlamak için, şunu yazarak ilerleyin

    ```
    set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation
    ```
    Sonra CTRL + V yazın,

    Sonra satırı doldurmak için içindeki URL 'nin geri kalanını yapıştırın

    ```
    set idp-single-logout-url
    https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.
    ```

5. Yapılandırmayı onaylamak için, yürütün –

    ```
    show user saml
    ```

_Grup eşleştirme için komut satırı yapılandırması gerçekleştir_

1. https://portal.azure.comFortiGate VM 'sine gidin ve ayarları açın
2. Sol taraftaki menüden **seri konsol** ' e tıklayın.
3. Sanal Makine Yöneticisi kimlik bilgileriyle seri konsolundaki oturum açma
4. Seri konsolunda aşağıdaki komutları yürütün –

    ```
    config user group
    edit <group 1 name>
    set member azure
    config match
    edit 1
    set server-name azure
    set group-name <group 1 Object Id>
    next
    end
    next
    ```

    `group 1 name`FortiGate 'te farklı bir portal deneyimine sahip olan her ek grup için Düzenle ' den bu komutu yineleyin

_Kimlik doğrulama zaman aşımı için komut satırı yapılandırması gerçekleştir_

1. https://portal.azure.comFortiGate VM 'sine gidin ve ayarları açın
2. Sol taraftaki menüden **seri konsol** ' e tıklayın.
3. Sanal Makine Yöneticisi kimlik bilgileriyle seri konsolundaki oturum açma
4. Seri konsolunda aşağıdaki komutları yürütün –

    ```
    config system global
    set remoteauthtimeout 60
    end
    ```
### <a name="create-vpn-portals-and-firewall-policy"></a>VPN portalları ve güvenlik duvarı Ilkesi oluşturma

1. `https://<address>` sayfasına gidin

    `address`FortiGate sanal makinesine atanan FQDN veya genel IP adresi aşağıda verilmiştir

2. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın
3. Sol taraftaki menüden **VPN** ' ye tıklayın.
4. VPN altında **SSL-VPN portalları** ' na tıklayın.
5. **Yeni oluştur** ' a tıklayın
6. Bir ad belirtin (genellikle özel Portal deneyimi sağlamak için kullanılan Azure grubuyla eşleşiyor)
7. **+** Kaynak IP havuzları ' nın yanındaki artı işaretine () tıklayın, varsayılan havuzu seçin ve **Kapat** ' a tıklayın.
8. Bu grup için deneyimi özelleştirin. Test için, bu portal Iletisi ve temasının özelleştirmesi olabilir. Bu Ayrıca, kullanıcıların iç kaynaklara yönlendirme özel yer işaretlerini oluşturabileceğiniz yerdir.
9. **Tamam 'a** tıklayın
10. Özel Portal deneyimine sahip olacak her bir Azure grubu için 5 ile 9 arasındaki adımları yineleyin
11. VPN altında **SSL-VPN ayarları** ' na tıklayın.
12. **+** Arabirimleri dinlemek için yanındaki artı işaretine () tıklayın
13. **PORT1** seçin ve **Kapat** 'a tıklayın


14. Özel bir SSL sertifikası daha önce yüklenmişse, sunucu sertifikasını açılan menüdeki Özel SSL sertifikasını kullanacak şekilde değiştirin
15. Kimlik doğrulama/Portal eşleme altında **Yeni oluştur** ' a tıklayın.
16. İlk Azure grubunu seçin ve aynı ada sahip portalla eşleştirin
17. **Tamam 'a** tıklayın
18. Her Azure grubu/Portal çifti için 15 ile 17 arasındaki adımları yineleyin
19. Kimlik doğrulama/Portal eşleme altında **diğer tüm kullanıcıları/grupları** Düzenle
20. Portalı **tam erişime** ayarla
21. **Tamam 'a** tıklayın
22. **Uygula** ' ya tıklayın
23. SSL-VPN ayar sayfasının en üstüne gidin ve **SSL yok-VPN ilkeleri**var seçeneğine tıklayın 
     **. Bu ayarları kullanarak yeni bir SSL-VPN ilkesi oluşturmak için buraya tıklayın**
24. **VPN GRP** gibi bir ad sağlayın
25. Giden arabirimi **bağlantı noktasına** ayarla
26. **Kaynak** tıklama
27. Adres altında **Tümü** ' nü seçin
28. Kullanıcı altında, ilk Azure grubunu seçin
29. **Kapat** 'a tıklayın
30. **Hedef** tıklama
31. Adres altında bu durum genellikle iç ağ olur. Test için login.microsoft.com seçin
32. **Kapat** 'a tıklayın
33. **Hizmet** 'e tıklayın
34. **Tümüne** tıklayın
35. **Kapat** 'a tıklayın
36. **Tamam 'a** tıklayın
37. Sol taraftaki menüde **ilke & nesneler** ' e tıklayın.
38. Ilke & nesneleri altında **güvenlik duvarı ilkesi** ' ne tıklayın.
39. **SSL-VPN tünel arabirimi (SSL. root)-> bağlantı noktası**
40. Daha önce oluşturulan ( **VPN GRP 1** ) VPN ilkesine sağ tıklayın ve **Kopyala** ' yı seçin.
41. VPN ilkesinin altına sağ tıklayın ve **aşağıdaki** > **Yapıştır** ' ı seçin
42. Yeni ilkeyi farklı bir adla ( **VPN Grp2** söyleyin) ve grubun geçerli olduğunu (başka bir Azure grubu) belirterek düzenleyin.
43. Yeni ilkeye sağ tıklayıp durumu **etkin** olarak ayarla


## <a name="test-sign-in-using-azure"></a>Azure kullanarak oturum açmayı test etme

1. Özel bir tarayıcı oturumu kullanarak şuraya gidin `https://<address>` 
2. Oturum açma, oturum açma için Azure Active Directory yeniden yönlendirmelidir
3. Azure kiracısındaki FortiGate uygulamasına atanan bir kullanıcının kimlik bilgilerini sağladıktan sonra, uygun Kullanıcı portalının gösterilmesi gerekir

    ![Fortigate SSL VPN](test-sign-in.png)
