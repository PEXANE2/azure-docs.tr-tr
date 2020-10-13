---
title: FortiGate dağıtım kılavuzu | Microsoft Docs
description: Fortinet FortiGate yeni nesil güvenlik duvarı ürününü kurun ve bunlarla çalışın.
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
ms.openlocfilehash: 357eb0a60e6246996de9ab75337ecc213d845ae7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91273339"
---
# <a name="fortigate-deployment-guide"></a>FortiGate dağıtım kılavuzu

Bu dağıtım kılavuzunu kullanarak, Fortinet FortiGate yeni nesil güvenlik duvarı ürününü ayarlamayı ve bunlarla çalışmayı öğreneceksiniz.

## <a name="redeem-the-fortigate-license"></a>FortiGate lisansını kullanma

Fortinet FortiGate yeni nesil güvenlik duvarı ürünü, Azure hizmet olarak altyapı (IaaS) içinde bir sanal makine olarak kullanılabilir. Bu sanal makine için iki lisans modu vardır: Kullandıkça öde ve kendi lisansını getir.

Fortinet, Azure Active Directory (Azure AD) "üretime yönelik güvenli hibrit erişimi (SHA)" ekibine lisanslarla lisans verebilir. Hiçbir lisansın sağlanmadığı durumlarda, Kullandıkça Öde dağıtımı da çalışacaktır.

Bir lisans verildiyse, Fortinet çevrimiçi olarak kullanılması gereken bir kayıt kodu sağlar.

![FortiGate SSL VPN kayıt kodunun ekran görüntüsü.](registration-code.png)

1. Kaydolun https://support.fortinet.com/ .
2. Kayıttan sonra oturum açın https://support.fortinet.com/ .
3. **Varlık**  >  **kaydetme/etkinleştirme**' ye gidin.
4. Fortinet tarafından sunulan kayıt kodunu girin.
5. Kayıt kodunu belirtin, **ürünü kamu dışı bir kullanıcı tarafından kullanılacak şekilde**seçin ve **İleri**' yi seçin.
6. Bir ürün açıklaması girin (örneğin, Fortigate), Fortinet iş ortağını **diğer**  >  **Microsoft**olarak ayarlayın ve **İleri**' yi seçin.
7. **Fortinet ürün kayıt sözleşmesi**'ni kabul edin ve **İleri**' yi seçin.
8. **Koşulları** kabul edin ve **Onayla**' yı seçin.
9. **Lisans dosyası indirmeyi**seçin ve lisansı daha sonra kaydedin.


## <a name="download-firmware"></a>Üretici yazılımını indirme

Fortinet FortiGate Azure VM Şu anda SAML kimlik doğrulaması için gereken bellenim sürümü ile birlikte sunulmamaktadır. En son sürüm, Fortinet 'ten alınmalıdır.

1. ' Da oturum açın https://support.fortinet.com/ .
2. **Download**  >  **Bellenim görüntülerini**indirme bölümüne gidin.
3. **Sürüm notlarının**sağında **İndir**' i seçin.
4. V6 ' i seçin **.** > **inç.** > **6,4.**.
5. Aynı satırdaki **https** bağlantısını seçerek **FGT_VM64_AZURE-v6-build1637-FORTINET. out** öğesini indirin.
6. Dosyayı daha sonra kaydedin.


## <a name="deploy-the-fortigate-vm"></a>FortiGate sanal makinesini dağıtma

1. Adresine gidin https://portal.azure.com ve FortiGate sanal makinesini dağıtmak istediğiniz abonelikte oturum açın.
2. Yeni bir kaynak grubu oluşturun veya FortiGate sanal makinesini dağıtmak istediğiniz kaynak grubunu açın.
3. **Ekle**’yi seçin.
4. **Markette ara**bölümünde *FORTI*girin. **Fortinet FortiGate yeni nesil güvenlik duvarı**' nı seçin.
5. Yazılım planını seçin (lisansınız varsa, kendi lisansını getir veya yoksa Kullandıkça öde). **Oluştur**’u seçin.
6. VM yapılandırmasını doldurun.

    ![Sanal makine oluşturma ekranının ekran görüntüsü.](virtual-machine.png)

7. **Kimlik doğrulama türünü** **parola**olarak ayarlayın ve VM için yönetici kimlik bilgilerini sağlayın.
8. **Gözden geçir ve oluştur** > **Oluştur**'u seçin.
9. VM dağıtımının tamamlanmasını bekleyin.


### <a name="set-a-static-public-ip-address-and-assign-a-fully-qualified-domain-name"></a>Statik bir genel IP adresi ayarlayın ve tam etki alanı adı atayın

Tutarlı bir kullanıcı deneyimi için, FortiGate sanal makinesine atanan genel IP adresini statik olarak atanacak şekilde ayarlayın. Ayrıca, tam etki alanı adı (FQDN) ile eşleyin.

1. Adresine gidin https://portal.azure.com ve FortiGate sanal makinesi için ayarları açın.
2. **Genel bakış** EKRANıNDA genel IP adresini seçin.

    ![Fortigate SSL VPN ekran görüntüsü.](public-ip-address.png)

3. **Statik**  >  **Kaydet**' i seçin.

FortiGate VM 'nin dağıtıldığı ortam için genel olarak yönlendirilebilir bir etki alanı adınız varsa, VM için bir konak (A) kaydı oluşturun. Bu kayıt, statik olarak atanan önceki genel IP adresiyle eşleşir.

### <a name="create-a-new-inbound-network-security-group-rule-for-tcp-port"></a>TCP bağlantı noktası için yeni bir gelen ağ güvenlik grubu kuralı oluştur

1. Adresine gidin https://portal.azure.com ve FortiGate sanal makinesi için ayarları açın.
2. Soldaki menüden **ağ iletişimi**' ni seçin. Ağ arabirimi listelenir ve gelen bağlantı noktası kuralları gösterilir.
3. **Gelen bağlantı noktası kuralı ekle**' yi seçin.
4. TCP 8443 için yeni bir gelen bağlantı noktası kuralı oluşturun.

    ![Gelen güvenlik kuralı Ekle ekran görüntüsü.](port-rule.png)

5. **Ekle**’yi seçin.


## <a name="create-a-custom-azure-app-for-fortigate"></a>FortiGate için özel bir Azure uygulaması oluşturma

1. ' A gidin https://portal.azure.com ve kiracı için, FortiGate oturum açma kimliğini sağlayacak Azure AD bölmesini açın.
2. Sol taraftaki menüden **Kurumsal uygulamalar**' ı seçin.
3. **Yeni uygulama**  >  **Galeri olmayan uygulama**' yı seçin.
4. Bir ad girin (örneğin, FortiGate) ve **Ekle**' yi seçin.
5. Sol taraftaki menüden **Kullanıcılar ve gruplar**' ı seçin.
6. Oturum açabiliyor ve **ata**' yı seçebilecek kullanıcıları ekleyin.
7. Sol taraftaki menüden **Çoklu oturum açma**' yı seçin.
8. **SAML**'yi seçin.
9. **Temel SAML yapılandırması**altında, yapılandırmayı düzenlemek için kalem simgesini seçin.
10. Aşağıdakini yapılandırın:
    - **Tanımlayıcı (VARLıK kimliği)** `https://<address>/remote/saml/metadata` .
    - **Yanıt URL 'si (onaylama tüketici hizmeti URL 'si)**  `https://<address>/remote/saml/login` .
    - **Oturum kapatma URL 'si** `https://<address>/remote/saml/logout` .

    `<address>` , FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

11. Bu URL 'lerin her birini daha sonra kullanmak üzere Kaydet: varlık KIMLIĞI, yanıt URL 'SI ve oturum kapatma URL 'SI.
12. **Kaydet**' i seçin ve **temel SAML yapılandırması**' nı kapatın.
13. **3 – SAML Imza sertifikası**' nın altında, **sertifikayı indirin (base64)** ve daha sonra kaydedin.
14. **4 – ayarla (uygulama adı)** altında, Azure oturum açma URL 'sini, Azure AD tanımlayıcıyı ve Azure Logout URL 'yi kopyalayın ve daha sonra için kaydedin.
15. **2 – Kullanıcı öznitelikleri ve talepler**altında, yapılandırmayı düzenlemek için kalem simgesini seçin.
16. **Yeni talep Ekle**' yi seçin ve ad ' ı **Kullanıcı**adı olarak ayarlayın.
17. Kaynak özniteliğini **User. UserPrincipalName**olarak ayarlayın.
18. **Save**  >  **Grup Ekle talep iste**  >  **tüm gruplar**' ı seçin.
19. **Grup talebinin adını Özelleştir**' i seçin ve adı **Grup**olarak ayarlayın.
20. **Kaydet**’i seçin.


## <a name="prepare-for-group-matching"></a>Grup eşleştirme için hazırlanma

FortiGate, grup üyeliğine göre oturum açmadan sonra farklı Kullanıcı Portalı deneyimlerine izin verir. Örneğin, pazarlama grubu ve finans grubu için bir deneyim olabilir. Kullanıcılar için Grup oluşturma adımları aşağıda verilmiştir:

1. ' A gidin https://portal.azure.com ve kiracı için, FortiGate oturum açma kimliğini sağlayacak Azure AD bölmesini açın.
2. **Gruplar**  >  **Yeni Grup**' u seçin.
3. Aşağıdaki ayrıntılarla bir grup oluşturun:
    - Grup türü = güvenlik
    - Grup adı = `a meaningful name`
    - Grup açıklaması = `a meaningful description for the group`
    - Üyelik türü = atandı
    - Üyeler = `users for the user experience that will map to this group`
4. Ek kullanıcı deneyimleri için 3 ve 4 numaralı adımları tekrarlayın.
5. Grupları oluşturduktan sonra her bir grubu seçin ve her biri için **nesne kimliğini** kaydedin.
6. Daha sonra bu nesne kimliklerini ve grup adlarını kaydedin.


## <a name="configure-the-fortigate-vm"></a>FortiGate VM 'sini yapılandırma

Aşağıdaki bölümler, FortiGate sanal makinesini ayarlama konusunda size rehberlik sağlar.

### <a name="install-the-license"></a>Lisansı yükler

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Dağıtım kendi lisansını getir modelini kullanıyorsa, lisans yüklemek için bir istem görürsünüz. Daha önce oluşturulan lisans dosyasını seçin ve karşıya yükleyin. **Tamam** ' ı seçin ve FortiGate sanal makinesini yeniden başlatın.

    ![FortiGate VM lisansının ekran görüntüsü.](license.png)

5. Yeniden başlatma işleminden sonra, lisansı doğrulamak için yönetici kimlik bilgileriyle yeniden oturum açın.

### <a name="update-firmware"></a>Üretici yazılımını güncelleştirme

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Sol menüde **sistem**  >  **bellenimi**' nu seçin.
5. **Üretici yazılımı yönetimi**' nde, **Araştır**' ı seçin ve daha önce indirilen bellenim dosyasını seçin.
6. Uyarıyı yoksayın ve **yedekleme yapılandırması ve yükseltme '** yi seçin.

    ![Üretici yazılımı yönetiminin ekran görüntüsü.](backup-configure-upgrade.png)

7. **Devam**’ı seçin.
8. FortiGate yapılandırmasını (. conf dosyası olarak) kaydetmeniz istendiğinde **Kaydet**' i seçin.
9. Üretici yazılımının karşıya yüklenmesini ve uygulanmasını bekleyin. FortiGate sanal makinesinin yeniden başlatılmasını bekleyin.
10. FortiGate sanal makinesi yeniden başlatıldıktan sonra, yönetici kimlik bilgileriyle tekrar oturum açın.
11. Panoyu ayarlamanız istendiğinde **daha sonra**öğesini seçin.
12. Öğretici videosu başladığında **Tamam**' ı seçin.

### <a name="change-the-management-port-to-tcp"></a>Yönetim bağlantı noktasını TCP olarak değiştirme

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Sol menüde **sistem**' i seçin.
5. **Yönetim ayarları**altında HTTPS bağlantı noktasını **8443**olarak değiştirin ve **Uygula**' yı seçin.
6. Değişiklik uygulandıktan sonra tarayıcı, yönetim sayfasını yeniden yüklemeye çalışır, ancak başarısız olur. Şu andan itibaren yönetim sayfası adresi `https://<address>` .

    ![Uzak sertifika yükleme ekran görüntüsü.](certificate.png)

### <a name="upload-the-azure-ad-saml-signing-certificate"></a>Azure AD SAML imzalama sertifikasını karşıya yükleme

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Sol menüde **sistem**  >  **sertifikaları**' nı seçin.
5. Uzak **sertifikayı içeri aktar**' ı seçin  >  **Remote Certificate**.
6. Azure kiracısındaki FortiGate özel uygulama dağıtımından indirilen sertifikaya gidin. Seçin ve **Tamam**' ı seçin.

### <a name="upload-and-configure-a-custom-ssl-certificate"></a>Özel bir SSL sertifikası yükleme ve yapılandırma

FortiGate sanal makinesini, kullanmakta olduğunuz FQDN 'yi destekleyen kendi SSL sertifikanız ile yapılandırmak isteyebilirsiniz. PFX biçiminde özel anahtarla paketlenmiş bir SSL sertifikasına erişiminiz varsa, bu amaçla kullanılabilir.

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. Herhangi bir sertifika hatasını izlemeye devam edin.
3. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
4. Sol menüde **sistem**  >  **sertifikaları**' nı seçin.
5. Yerel **sertifikayı içeri aktar**  >  **Local Certificate**  >  **PKCS #12 sertifikası**' nı seçin.
6. Öğesine gidin. SSL sertifikasını ve özel anahtarı içeren PFX dosyası.
7. Sağlayın. PFX parolası ve sertifika için anlamlı bir ad. Ardından **Tamam**'ı seçin.
8. Sol menüde **sistem**  >  **ayarları**' nı seçin.
9. **Yönetim ayarları**altında, **https sunucu sertifikası**' nın yanındaki listeyi genişletin ve daha önce içeri aktarılan SSL sertifikasını seçin.
10. **Uygula**’yı seçin.
11. Tarayıcı penceresini kapatın ve adresine gidin `https://<address>` .
12. FortiGate yönetici kimlik bilgileriyle oturum açın. Artık kullanımda olan doğru SSL sertifikasını görmeniz gerekir.


### <a name="perform-command-line-configuration"></a>Komut satırı yapılandırması gerçekleştirme

Aşağıdaki bölümlerde, komut satırını kullanarak çeşitli yapılandırmalara yönelik adımlar sağlanmaktadır.

#### <a name="for-saml-authentication"></a>SAML kimlik doğrulaması için

1. Adresine gidin https://portal.azure.com ve FortiGate sanal makinesi için ayarları açın.
2. Sol taraftaki menüden **seri konsolu**' nu seçin.
3. Kimliği, sanal makine yöneticisi kimlik bilgileriyle seri konsolunda oturum açın. Sonraki adımda, daha önce kaydettiğiniz URL 'Ler gereklidir:
    - Varlık KIMLIĞI
    - Yanıt URL'si
    - Oturum kapatma URL 'SI
    - Azure oturum açma URL 'SI
    - Azure AD tanımlayıcısı
    - Azure oturum kapatma URL 'SI
4. Seri konsolunda aşağıdaki komutları çalıştırın:

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
    > Azure oturum kapatma URL 'SI bir `?` karakter içeriyor. Bu, FortiGate seri konsoluna doğru bir şekilde sağlanması için özel bir anahtar sırası gerektirir. URL genellikle olur `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1` . Bunu seri konsolunda sağlamak için şunu yazın:
        ```
        set idp-single-logout-url https://login.microsoftonline.com/common/wsfederation
        ```.
    Sonra CTRL + V yazın ve satırı doldurmak için içindeki URL 'nin geri kalanını yapıştırın: 
        ```
        set idp-single-logout-url
        https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.
        ```

5. Yapılandırmayı onaylamak için aşağıdakileri çalıştırın:

    ```
    show user saml
    ```

#### <a name="for-group-matching"></a>Grup eşleştirme için

1. Adresine gidin https://portal.azure.com ve FortiGate sanal makinesi için ayarları açın.
2. Sol taraftaki menüden **seri konsolu**' nu seçin.
3. Kimliği, sanal makine yöneticisi kimlik bilgileriyle seri konsolunda oturum açın.
4. Seri konsolunda aşağıdaki komutları çalıştırın:

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

    FortiGate 'te farklı bir portal deneyimine sahip olacak her ek grup için, bu komutları tekrarlayın (Kodun ikinci satırıyla itibaren).

#### <a name="for-authentication-timeout"></a>Kimlik doğrulama zaman aşımı için

1. Adresine gidin https://portal.azure.com ve FortiGate sanal makinesi için ayarları açın.
2. Sol taraftaki menüden **seri konsolu**' nu seçin.
3. Kimliği, sanal makine yöneticisi kimlik bilgileriyle seri konsolunda oturum açın.
4. Seri konsolunda aşağıdaki komutları çalıştırın:

    ```
    config system global
    set remoteauthtimeout 60
    end
    ```
### <a name="create-vpn-portals-and-firewall-policy"></a>VPN portalları ve güvenlik duvarı ilkesi oluşturma

1. `https://<address>` öğesine gidin. Burada, `<address>` FortiGate sanal makinesine atanan FQDN veya genel IP adresidir.

2. FortiGate VM dağıtımı sırasında belirtilen yönetici kimlik bilgilerini kullanarak oturum açın.
3. Sol taraftaki menüden **VPN**  >  **SSL-VPN portalları**  >  **Yeni oluştur**' u seçin.
6. Bir ad belirtin (genellikle bu, özel Portal deneyimi sağlamak için kullanılan Azure grubuyla eşleşiyor).
7. **+** **Kaynak IP havuzları**' nın yanındaki artı işaretini () seçin, varsayılan havuzu seçin ve ardından **Kapat**' ı seçin.
8. Bu grup için deneyimi özelleştirin. Test için, bu portal iletisi ve temasının özelleştirmesi olabilir. Bu Ayrıca, kullanıcıların iç kaynaklara yönlendirme özel yer işaretleri oluşturabileceğiniz yerdir.
9. **Tamam**’ı seçin.
10. Özel Portal deneyimine sahip olan her bir Azure grubu için 5-9 arasındaki adımları yineleyin.
11. VPN altında **SSL-VPN ayarları**' nı seçin.
12. **+** **Arabirimleri dinle**' ın yanındaki artı işaretini () seçin, **PORT1**öğesini seçin ve ardından **Kapat**' ı seçin.
14. Daha önce özel bir SSL sertifikası yüklediyseniz, **sunucu sertifikasını** , açılan MENÜDEKI özel SSL sertifikasını kullanacak şekilde değiştirin.
15. **Kimlik doğrulama/Portal eşleme**altında **Yeni oluştur**' u seçin. İlk Azure grubunu seçin ve aynı ada sahip portalla eşleştirin. Ardından **Tamam**'ı seçin.
18. Azure grubunun ve portalının her eşleştirmesi için 15-17 arasındaki adımları yineleyin.
19. **Kimlik doğrulama/Portal eşleme**altında **diğer tüm kullanıcıları/grupları**düzenleyin.
20. Portalı **tam erişim**olarak ayarlayın ve **Tamam**' ı seçin  >  **Apply**.
23. **SSL-VPN ayar** sayfasının en üstüne gıdın ve SSL yok ' u seçin **. Bu ayarları kullanarak yeni bir SSL-VPN ilkesi oluşturmak için buraya tıklayın.**
24. **VPN GRP**gibi bir ad sağlayın. Sonra **giden arabirimi** **bağlantı noktası**olarak ayarlayın ve **kaynak**' ı seçin.
27. **Adres**altında **Tümü**' nü seçin.
28. **Kullanıcı**altında, ilk Azure grubunu seçin.
29. Hedefi **Kapat**' ı seçin  >  **Destination**. **Adres**altında bu genellikle iç ağ olur. Test için **login.Microsoft.com** öğesini seçin.
32. **Close**  >  **Hizmeti**  >  **Tümünü**Kapat ' ı seçin. Ardından **Close**  >  **Tamam tamam**' ı seçin.
37. Sol menüde **ilke & nesneler**  >  **güvenlik duvarı ilkesi**' ni seçin.
39. **SSL-VPN tünel arabirimi (SSL. root)**  >  **bağlantı noktasını**genişletin.
40. Daha önce oluşturulan VPN ilkesine (**VPN GRP 1**) sağ tıklayın ve **Kopyala**' yı seçin.
41. VPN ilkesinin altına sağ tıklayın ve aşağıdaki **Yapıştır**' ı seçin  >  **Below**.
42. Yeni ilkeyi, farklı bir adla (örneğin, **VPN Grp2**) belirterek düzenleyin. Ayrıca, uygulandığı grubu (başka bir Azure grubuna) de değiştirin.
43. Yeni ilkeye sağ tıklayın ve durumu **etkin**olarak ayarlayın.


## <a name="test-sign-in-by-using-azure"></a>Azure kullanarak oturum açmayı test etme

1. Özel bir tarayıcı oturumu kullanarak adresine gidin `https://<address>` .  
2. Oturum açma, oturum açma için Azure AD 'ye yeniden yönlendirmelidir.
3. Azure kiracısındaki FortiGate uygulamasına atanan bir kullanıcı için kimlik bilgileri sağladıktan sonra, uygun Kullanıcı Portalı görüntülenmelidir.

    ![FortiGate SSL VPN ekran görüntüsü](test-sign-in.png)
