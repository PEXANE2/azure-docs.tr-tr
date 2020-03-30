---
title: NPS uzantısını kullanarak Azure MFA ile VPN - Azure Active Directory
description: Microsoft Azure için Ağ Politikası Sunucusu uzantısını kullanarak VPN altyapınızı Azure MFA ile tümleştirin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: f446f1549b3efcd5f27752fac972dfd80c8650d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75425394"
---
# <a name="integrate-your-vpn-infrastructure-with-azure-mfa-by-using-the-network-policy-server-extension-for-azure"></a>Azure için Ağ Politikası Sunucusu uzantısını kullanarak VPN altyapınızı Azure MFA ile tümleştirin

Azure için Ağ İlkesi Sunucusu (NPS) uzantısı, kuruluşların iki aşamalı doğrulama sağlayan bulut tabanlı [Azure Çok Faktörlü Kimlik Doğrulaması (MFA)](howto-mfaserver-nps-rdg.md)kullanarak Uzaktan Kimlik Doğrulama Çevirmeli Kullanıcı Hizmeti (RADIUS) istemci kimlik doğrulamasını korumasına olanak tanır.

Bu makalede, Azure için NPS uzantısını kullanarak NPS altyapısını MFA ile tümleştirmek için yönergeler sağlanmaktadır. Bu işlem, VPN kullanarak ağınıza bağlanmaya çalışan kullanıcılar için güvenli iki aşamalı doğrulama sağlar.

Ağ Politikası ve Erişim Hizmetleri, kuruluşlara aşağıdakileri yapma olanağı sağlar:

* Belirtmek üzere ağ isteklerinin yönetimi ve denetimi için merkezi bir konum atama:

  * Kimler bağlanabilir

  * Günün hangi saatlerinde bağlantılara izin verilir

  * Bağlantıların süresi

  * İstemcilerin bağlanmak için kullanması gereken güvenlik düzeyi

    Her VPN veya Uzak Masaüstü Ağ Geçidi sunucusunda ilkeler belirtmek yerine, bunu merkezi bir konuma geldikten sonra yapın. RADIUS protokolü merkezi Kimlik Doğrulama, Yetkilendirme ve Muhasebe (AAA) sağlamak için kullanılır.

* Aygıtlara ağ kaynaklarına sınırsız veya sınırlı erişim verilip verilmeyeceğini belirleyen Ağ Erişim Koruması (NAP) istemci sistem durumu ilkelerini belirleyin ve uygulayın.

* 802.1x özellikli kablosuz erişim noktalarına ve Ethernet anahtarlarına erişmek için kimlik doğrulamave yetkilendirmeyi zorlamanın bir yolunu sağlayın.
  Daha fazla bilgi için [Ağ İlkesi Sunucusu'na](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top)bakın.

Güvenliği artırmak ve yüksek düzeyde uyumluluk sağlamak için kuruluşlar, kullanıcıların VPN sunucusundaki sanal bağlantı noktasına bağlanmak için iki aşamalı doğrulama yı kullanmasını sağlamak için NPS'yi Azure Çok Faktörlü Kimlik Doğrulaması ile tümleştirebilir. Kullanıcılara erişim izni verebilmek için kullanıcı adı ve parola birleşimini ve kontrol ettikleri diğer bilgileri sağlamaları gerekir. Bu bilgilere güvenilmeli ve kolayca çoğaltılmamalıdır. Bir cep telefonu numarası, sabit hat numarası veya mobil cihazdaki bir uygulamayı içerebilir.

Azure için NPS uzantısı kullanılabilirliğinden önce, tümleşik NPS ve MFA ortamları için iki aşamalı doğrulama uygulamak isteyen müşterilerin şirket içi ortamda ayrı bir MFA sunucusu yapılandırması ve bakımını yapmak zorunda kaldığı. Bu tür kimlik doğrulama, RADIUS kullanılarak Uzak Masaüstü Ağ Geçidi ve Azure Çok Faktörlü Kimlik Doğrulama Sunucusu tarafından sunulur.

Azure için NPS uzantısı ile kuruluşlar, şirket içi tabanlı bir MFA çözümü veya bulut tabanlı MFA çözümü dağıtarak RADIUS istemci kimlik doğrulamasını güvence altına alabilir.

## <a name="authentication-flow"></a>Kimlik doğrulaması akışı

Kullanıcılar VPN sunucusundaki sanal bir bağlantı noktasına bağlandıklarında, önce çeşitli protokoller kullanarak kimlik doğrulamaları gerekir. Protokoller, kullanıcı adı ve parola ile sertifika tabanlı kimlik doğrulama yöntemlerinin bir birleşiminin kullanılmasına izin verir.

Kullanıcıların kimliklerini doğrulamanın ve doğrulamanın yanı sıra, uygun arama izinlerine sahip olmaları gerekir. Basit uygulamalarda, erişime izin veren arama izinleri doğrudan Active Directory kullanıcı nesnelerinde ayarlanır.

![Etkin Dizin Kullanıcıları ve Bilgisayarkullanıcı özelliklerinde çevirmeli numara](./media/howto-mfa-nps-extension-vpn/image1.png)

Basit uygulamalarda, her VPN sunucusu, her yerel VPN sunucusunda tanımlanan ilkelere dayalı olarak erişim sağlar veya reddeder.

Daha büyük ve daha ölçeklenebilir uygulamalarda, VPN erişimini veren veya reddeden ilkeler RADIUS sunucularında merkezileştirilmiştir. Bu gibi durumlarda, VPN sunucusu bağlantı isteklerini ve hesap iletilerini radius sunucusuna ileten bir erişim sunucusu (RADIUS istemcisi) olarak hareket eder. VPN sunucusundaki sanal bağlantı noktasına bağlanmak için kullanıcıların kimlik doğrulaması yapılmalı ve RADIUS sunucularında merkezi olarak tanımlanan koşulları karşılaması gerekir.

Azure için NPS uzantısı NPS ile tümleşik olduğunda, başarılı bir kimlik doğrulama akışı aşağıdaki gibi sonuçlanır:

1. VPN sunucusu, uzak masaüstü oturumu gibi bir kaynağa bağlanmak için kullanıcı adı ve parola içeren bir VPN kullanıcısından kimlik doğrulama isteği alır.
2. RADIUS istemcisi olarak hareket eden VPN sunucusu, isteği RADIUS *Access-Request* iletisine dönüştürür ve (şifreli bir parolayla) NPS uzantısının yüklü olduğu RADIUS sunucusuna gönderir.
3. Kullanıcı adı ve parola kombinasyonu Active Directory'de doğrulanır. Kullanıcı adı veya parola yanlışsa, RADIUS Sunucusu bir *Access-Reject* iletisi gönderir.
4. NPS Bağlantı İsteği ve Ağ İlkeleri'nde belirtildiği gibi tüm koşullar karşılanırsa (örneğin, günün saati veya grup üyeliği kısıtlamaları), NPS uzantısı Azure Çok Faktörlü Kimlik Doğrulaması ile ikincil kimlik doğrulaması isteğini tetikler.
5. Azure Çok Faktörlü Kimlik Doğrulama, Azure Active Directory ile iletişim kurar, kullanıcının ayrıntılarını alır ve kullanıcı tarafından yapılandırılan yöntemi (cep telefonu görüşmesi, kısa mesaj veya mobil uygulama) kullanarak ikincil kimlik doğrulamasını gerçekleştirir.
6. MFA mücadelesi başarılı olduğunda, Azure Çok Faktörlü Kimlik Doğrulama sonucu NPS uzantısına iletir.
7. Bağlantı denemesi hem doğrulanmış hem de yetkilendirilen bir sistemden sonra, uzantın yüklü olduğu NPS VPN sunucusuna (RADIUS istemcisi) radius *Erişim Kabul* iletisi gönderir.
8. Kullanıcı VPN sunucusunda sanal bağlantı noktasına erişim verilir ve şifreli bir VPN tüneli kurar.

## <a name="prerequisites"></a>Ön koşullar

Bu bölümde, MFA'yı VPN ile tümleştiremeden önce tamamlanması gereken ön koşullar ayrıntılı olarak açıklanır. Başlamadan önce, aşağıdaki ön koşullara sahip olmalısınız:

* VPN altyapısı
* Ağ Politikası ve Erişim Hizmetleri rolü
* Azure Çok Faktörlü Kimlik Doğrulama lisansı
* Windows Server yazılımı
* Kitaplıklar
* Azure Etkin Dizin (Azure AD) şirket içi Active Directory ile senkronize
* Azure Etkin Dizin GUID Kimliği

### <a name="vpn-infrastructure"></a>VPN altyapısı

Bu makalede, Microsoft Windows Server 2016 kullanan çalışan bir VPN altyapınız olduğu ve VPN sunucunuzun şu anda bir RADIUS sunucusuna bağlantı isteklerini iletmek için yapılandırılmamış olduğu varsayar. Makalede, VPN altyapısını merkezi bir RADIUS sunucusu kullanacak şekilde yapılandırabilirsiniz.

Yerinde çalışan bir VPN altyapınız yoksa, Microsoft ve üçüncü taraf sitelerinde bulabileceğiniz çok sayıda VPN kurulum öğreticisinde kılavuzu izleyerek hızlı bir şekilde bir tane oluşturabilirsiniz.

### <a name="the-network-policy-and-access-services-role"></a>Ağ Politikası ve Erişim Hizmetleri rolü

Ağ Politikası ve Erişim Hizmetleri RADIUS sunucusu ve istemci işlevselliği sağlar. Bu makalede, ağ ilkesi ve erişim hizmetleri rolünü ortamınızdaki bir üye sunucuya veya etki alanı denetleyicisinde yüklediğiniz varsayar. Bu kılavuzda, BIR VPN yapılandırması için RADIUS'u yapılandırabilirsiniz. Ağ İlkesi ve Erişim Hizmetleri rolünü VPN sunucunuz *dışındaki* bir sunucuya yükleyin.

Ağ İlkesi ve Erişim Hizmetleri rol hizmeti Windows Server 2012 veya sonraki yükleme hakkında bilgi [için](https://technet.microsoft.com/library/dd296890.aspx)bkz. NAP, Windows Server 2016'da amortismana hazırdır. NPS'yi etki alanı denetleyicisine yükleme önerisi de dahil olmak üzere NPS için en iyi uygulamaların açıklaması [için](https://technet.microsoft.com/library/cc771746)Bkz.

### <a name="azure-mfa-license"></a>Azure MFA Lisansı

Azure Çok Faktörlü Kimlik Doğrulama için bir lisans gereklidir ve Azure AD Premium, Enterprise Mobility + Security veya Çok Faktörlü Kimlik Doğrulama tek başına lisans aracılığıyla kullanılabilir. Kullanıcı başına veya kimlik doğrulama lisansları başına gibi Azure MFA'nın tüketim tabanlı lisansları NPS uzantısı ile uyumlu değildir. Daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama'yı nasıl edinirebilirsiniz.](concept-mfa-licensing.md) Sınama amacıyla deneme aboneliği kullanabilirsiniz.

### <a name="windows-server-software"></a>Windows Server yazılımı

NPS uzantısı, Ağ İlkesi ve Erişim Hizmetleri rolü yüklü olan Windows Server 2008 R2 SP1 veya daha sonra sını gerektirir. Bu kılavuzdaki tüm adımlar Windows Server 2016 ile gerçekleştirilmiştir.

### <a name="libraries"></a>Kitaplıklar

Aşağıdaki kitaplıklar NPS uzantısı ile otomatik olarak yüklenir:

-   [Visual Studio 2013 Için Visual C++ Yeniden Dağıtılabilir Paketler (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Windows PowerShell sürüm 1.1.166.0 için Microsoft Azure Active Directory Modülü](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Microsoft Azure Active Directory PowerShell Modülü zaten mevcut değilse, kurulum işleminin bir parçası olarak çalıştırdığınız bir yapılandırma komut dosyasıyla yüklenir. Modül zaten yüklenmediyse, modülü önceden yüklemenize gerek yoktur.

### <a name="azure-active-directory-synced-with-on-premises-active-directory"></a>Azure Etkin Dizini şirket içi Active Directory ile senkronize edildi

NPS uzantısını kullanmak için şirket içi kullanıcıların Azure Etkin Dizini ile senkronize edilmesi ve MFA için etkinleştirilmesi gerekir. Bu kılavuz, şirket içi kullanıcıların Azure AD Connect üzerinden Azure Active Directory ile senkronize edildiğini varsayar. MFA kullanıcıları etkinleştirme talimatları aşağıda verilmiştir.

Azure AD Connect hakkında daha fazla bilgi için [bkz.](../hybrid/whatis-hybrid-identity.md)

### <a name="azure-active-directory-guid-id"></a>Azure Etkin Dizin GUID Kimliği

NPS uzantısını yüklemek için Azure Etkin DizininIN GUID'ini bilmeniz gerekir. Azure Etkin DizininIN GUID'ini bulma yönergeleri sonraki bölümde verilmiştir.

## <a name="configure-radius-for-vpn-connections"></a>VPN bağlantıları için RADIUS'u yapılandırın

NPS rolünü bir üye sunucuya yüklediyseniz, VPN bağlantılarını isteyen VPN istemcisini doğrulamak ve yetkilendirmek için yapılandırmanız gerekir. 

Bu bölüm, Ağ İlkesi ve Erişim Hizmetleri rolünü yüklediğinizi, ancak altyapınızda kullanılmak üzere yapılandırmadığınızı varsayar.

> [!NOTE]
> Kimlik doğrulaması için merkezi bir RADIUS sunucusu kullanan çalışan bir VPN sunucunuz varsa, bu bölümü atlayabilirsiniz.
>

### <a name="register-server-in-active-directory"></a>Aktif Dizine Sunucu Kaydet

Bu senaryoda düzgün çalışabilmek için NPS sunucusunun Active Directory'de kaydedilmesi gerekir.

1. Sunucu Yöneticisi'ni açın.

2. Server Manager'da **Araçlar'ı**seçin ve ardından **Network Policy Server'ı**seçin.

3. Network Policy Server konsolunda **NPS (Yerel) düğmesine**sağ tıklayın ve ardından **Active Directory'de Kaydet sunucusunu**seçin. **Tamam'ı** iki kez seçin.

    ![Active Directory menüsünde sunucukaydetme seçeneği](./media/howto-mfa-nps-extension-vpn/image2.png)

4. Bir sonraki yordam için konsolu açık bırakın.

### <a name="use-wizard-to-configure-the-radius-server"></a>RADIUS sunucusunu yapılandırmak için sihirbazı kullanın

RADIUS sunucusunu yapılandırmak için standart (sihirbaz tabanlı) veya gelişmiş yapılandırma seçeneği kullanabilirsiniz. Bu bölüm, sihirbaz tabanlı standart yapılandırma seçeneğini kullandığınızı varsayar.

1. Network Policy Server konsolunda **NPS (Yerel)** seçeneğini belirleyin.

2. **Standart Yapılandırma**altında, **Çevirmeli Bağlantı veya VPN Bağlantıları için RADIUS Server'ı**seçin ve ardından VPN veya **Çevirmeli BağlantıYı Yapılandır'ı**seçin.

    ![Çevirmeli Veya VPN Bağlantıları için RADIUS Sunucusu'nun yapılandırılması](./media/howto-mfa-nps-extension-vpn/image3.png)

3. **Çevirmeli veya Sanal Özel Ağ Bağlantılarını Seç penceresinde** Sanal Özel Ağ **Bağlantıları'nı**seçin ve **ardından İleri'yi**seçin.

    ![Sanal özel ağ bağlantılarını yapılandırma](./media/howto-mfa-nps-extension-vpn/image4.png)

4. **Çevirmeli Veya VPN Server ekle** penceresinde **Ekle'yi**seçin.

5. Yeni **RADIUS istemci** penceresinde, dost bir ad girin, VPN sunucusunun çözülebilir adını veya IP adresini girin ve ardından paylaşılan gizli bir parola girin. Paylaşılan gizli parolayı uzun ve karmaşık hale getirin. Kaydedin, çünkü bir sonraki bölümde ihtiyacınız olacak.

    ![Yeni RADIUS istemci penceresi oluşturma](./media/howto-mfa-nps-extension-vpn/image5.png)

6. **Tamam'ı**seçin ve sonra **İleri'yi**seçin.

7. Kimlik **Doğrulama Yöntemlerini Yapılandırma** penceresinde, varsayılan seçimi **(Microsoft Şifreli Kimlik Doğrulama sürümü 2 [MS-CHAPv2])** kabul edin veya başka bir seçenek seçin ve **İleri'yi**seçin.

    > [!NOTE]
    > Genişletilebilir Kimlik Doğrulama Protokolü 'nü (EAP) yapılandırırsanız, Microsoft Challenge-Handshake Kimlik Doğrulama Protokolü (CHAPv2) veya Korumalı Genişletilebilir Kimlik Doğrulama Protokolü (PEAP) kullanmanız gerekir. Başka hiçbir EAP desteklenmez.

8. Kullanıcı **Grupları Belirt** penceresinde **Ekle'yi**seçin ve ardından uygun bir grup seçin. Grup yoksa, tüm kullanıcılara erişim sağlamak için seçimi boş bırakın.

    ![Erişime izin vermek veya reddetmek için Kullanıcı Grupları penceresini belirtin](./media/howto-mfa-nps-extension-vpn/image7.png)

9. **Sonraki'ni**seçin.

10. IP **Filtreleri Belirt** penceresinde **İleri'yi**seçin.

11. Şifreleme **Ayarlarını Belirt** penceresinde varsayılan ayarları kabul edin ve **sonra İleri'yi**seçin.

    ![Şifreleme Ayarlarını Belirt penceresi](./media/howto-mfa-nps-extension-vpn/image8.png)

12. Bölge **Adı Belirt** penceresinde, bölge adını boş bırakın, varsayılan ayarı kabul edin ve sonra **İleri'yi**seçin.

    ![Bir Bölge Adı penceresi belirt](./media/howto-mfa-nps-extension-vpn/image9.png)

13. Yeni **Çevirmeli Veya Sanal Özel Ağ Bağlantılarını Ve RADIUS istemcileri** penceresinde **Finish'i**seçin.

    ![Tamamlanan yapılandırma penceresi](./media/howto-mfa-nps-extension-vpn/image10.png)

### <a name="verify-the-radius-configuration"></a>RADIUS yapılandırmasını doğrulama

Bu bölümde sihirbazı kullanarak oluşturduğunuz yapılandırma ayrıntılı.

1. Ağ İlkesi Sunucusu'nda, NPS (yerel) konsolunda **RADIUS Istemcilerini**genişletin ve ardından **RADIUS İstemcilerini**seçin.

2. Ayrıntılar bölmesinde, oluşturduğunuz RADIUS istemcisini sağ tıklatın ve ardından **Özellikler'i**seçin. RADIUS istemcinizin (VPN sunucusu) özellikleri burada gösterilenler gibi olmalıdır:

    ![VPN özelliklerini ve yapılandırmasını doğrulayın](./media/howto-mfa-nps-extension-vpn/image11.png)

3. **İptal**’i seçin.

4. Ağ İlkesi Sunucusu'nda, NPS (yerel) **konsolunda, İlkeler'i**genişletin ve ardından **Bağlantı İstek İlkeleri'ni**seçin. VPN Bağlantıları ilkesi aşağıdaki resimde gösterildiği gibi görüntülenir:

    ![VPN bağlantı ilkesini gösteren bağlantı isteği ilkesi](./media/howto-mfa-nps-extension-vpn/image12.png)

5. **İlkeler** **altında, Ağ İlkeleri'ni**seçin. Aşağıdaki resimde gösterilen ilkeye benzeyen bir Sanal Özel Ağ (VPN) Bağlantılar ilkesi ni görmelisiniz:

    ![Sanal Özel Ağ Bağlantıları ilkesini gösteren Ağ İlkeleri](./media/howto-mfa-nps-extension-vpn/image13.png)

## <a name="configure-your-vpn-server-to-use-radius-authentication"></a>VPN sunucunuzu RADIUS kimlik doğrulamasını kullanacak şekilde yapılandırın

Bu bölümde, VPN sunucunuzu RADIUS kimlik doğrulamasını kullanacak şekilde yapılandırırsınız. Yönergeler, vpn sunucusunun çalışan bir yapılandırmasına sahip olduğunuzu varsayıyor, ancak radius kimlik doğrulaması kullanacak şekilde yapılandırmamış. VPN sunucusunu yapılandırdıktan sonra yapılandırmanızın beklendiği gibi çalıştığını onaylayın.

> [!NOTE]
> Radius kimlik doğrulaması kullanan çalışan bir VPN sunucu yapılandırmanız varsa, bu bölümü atlayabilirsiniz.
>

### <a name="configure-authentication-provider"></a>Kimlik doğrulama sağlayıcısını yapılandırma

1. VPN sunucusunda Server Manager'ı açın.

2. Server Manager'da **Araçlar'ı**seçin ve ardından **Yönlendirme ve Uzaktan Erişim'i**seçin.

3. Yönlendirme **ve Uzaktan Erişim** penceresinde, sunucu ** \<adı> (yerel)** sağ tıklatın ve ardından **Özellikler'i**seçin.

4. Sunucu **adı> (yerel) Özellikler penceresinde Güvenlik sekmesini seçin. \<** **Security**

5. **Güvenlik** sekmesinde, **Kimlik Doğrulama sağlayıcısı**altında RADIUS **Kimlik Doğrulama'yı**seçin ve sonra **Yapıla'yı**seçin.

    ![RADIUS Kimlik Doğrulama sağlayıcısını yapılandırın](./media/howto-mfa-nps-extension-vpn/image15.png)

6. RADIUS **Kimlik Doğrulama** penceresinde **Ekle'yi**seçin.

7. RADIUS **Server Ekle** penceresinde aşağıdakileri yapın:

    a. Sunucu **ad** kutusuna, önceki bölümde yapılandırdığınız RADIUS sunucusunun adını veya IP adresini girin.

    b. Paylaşılan **gizli**için **Değiştir'i**seçin ve daha önce oluşturduğunuz ve kaydettiğiniz paylaşılan gizli parolayı girin.

    c. **Zaman-Çıkış (saniye)** kutusuna **30**değerini girin.  
    Zaman ayırma değeri, ikinci kimlik doğrulama etkenini tamamlamak için yeterli zaman tanımak için gereklidir.

    ![Zaman çıkışını yapılandıran RADIUS Server penceresi ekleme](./media/howto-mfa-nps-extension-vpn/image16.png)

8. **Tamam'ı**seçin.

### <a name="test-vpn-connectivity"></a>Test VPN bağlantısı

Bu bölümde, VPN sanal bağlantı noktasına bağlanmaya çalıştığınızda VPN istemcisinin RADIUS sunucusu tarafından kimlik doğrulaması ve yetkilendirilmesini onaylarsınız. Yönergeler, Windows 10'u VPN istemcisi olarak kullandığınızı varsayar.

> [!NOTE]
> VPN sunucusuna bağlanmak için zaten bir VPN istemcisi yapılandırıldıysanız ve ayarları kaydettiyseniz, VPN bağlantı nesnesini yapılandırma ve kaydetme yle ilgili adımları atlayabilirsiniz.
>

1. VPN istemci bilgisayarınızda **Başlat** düğmesini seçin ve ardından **Ayarlar** düğmesini seçin.

2. Windows **Ayarları** penceresinde **Internet'& Ağ'ı**seçin.

3. **VPN'i**seçin.

4. **VPN bağlantısı ekle'yi**seçin.

5. VPN **bağlantı ekle** penceresinde, **VPN sağlayıcı** kutusunda, **Windows'u (yerleşik)** seçin, kalan alanları uygun şekilde tamamlayın ve sonra **Kaydet'i**seçin.

    !["VPN bağlantısı ekle" penceresi](./media/howto-mfa-nps-extension-vpn/image17.png)

6. Denetim **Masası'na**gidin ve ardından **Ağ ve Paylaşım Merkezi'ni**seçin.

7. **Bağdaştırıcı ayarlarını değiştir'i**seçin.

    ![Ağ ve Paylaşım Merkezi - Bağdaştırıcı ayarlarını değiştir](./media/howto-mfa-nps-extension-vpn/image18.png)

8. VPN ağ bağlantısını sağ tıklatın ve ardından **Özellikler'i**seçin.

9. VPN özellikleri penceresinde **Güvenlik** sekmesini seçin.

10. **Güvenlik** sekmesinde, yalnızca **Microsoft CHAP Sürüm 2'nin (MS-CHAP v2)** seçildiğinden emin olun ve ardından **Tamam'ı**seçin.

    !["Bu protokollere izin ver" seçeneği](./media/howto-mfa-nps-extension-vpn/image20.png)

11. VPN bağlantısını sağ tıklatın ve sonra **Bağlan'ı**seçin.

12. **Ayarlar** penceresinde **Bağlan'ı**seçin.  
    Burada gösterildiği gibi, Olay Kimliği 6272 olarak RADIUS sunucusunda Güvenlik günlüğünde başarılı bir bağlantı görüntülenir:

    ![Başarılı bir bağlantı gösteren Olay Özellikleri penceresi](./media/howto-mfa-nps-extension-vpn/image21.png)

## <a name="troubleshooting-radius"></a>Sorun Giderme RADIUS

VPN sunucunuzu kimlik doğrulama ve yetkilendirme için merkezi bir RADIUS sunucusu kullanacak şekilde yapılandırmadan önce VPN yapılandırmanızın çalıştığını varsayalım. Yapılandırma çalışıyorsa, soruna RADIUS sunucusunun yanlış yapılandırılması veya geçersiz bir kullanıcı adı veya parola kullanılması neden olabilir. Örneğin, kullanıcı adında alternatif UPN soneki kullanırsanız, oturum açma girişimi başarısız olabilir. En iyi sonuçlar için aynı hesap adını kullanın.

Bu sorunları gidermek için başlamak için ideal bir yer RADIUS sunucusundaki Güvenlik olay günlüklerini incelemektir. Olayları aramak için zaman kazanmak için, burada gösterildiği gibi Olay Görüntüleyicisi'nde rol tabanlı Ağ İlkesi ve Access Server özel görünümünü kullanabilirsiniz. "Event ID 6273", NPS'nin bir kullanıcıya erişimi engellendiği olayları gösterir.

![NPAS olaylarını gösteren olay görüntüleyicisi](./media/howto-mfa-nps-extension-vpn/image22.png)

## <a name="configure-multi-factor-authentication"></a>Çok Faktörlü Kimlik Doğrulamayı Yapılandırma

Çok Faktörlü Kimlik Doğrulama için kullanıcıları yapılandırmaya yardımcı olmak için [bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulama dağıtımı planlama makalelerine](howto-mfa-getstarted.md#create-conditional-access-policy) bakın ve [hesabımı iki aşamalı doğrulama için ayarlama](../user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="install-and-configure-the-nps-extension"></a>NPS uzantısını yükleme ve yapılandırma

Bu bölümde VPN sunucusu ile istemci kimlik doğrulaması için MFA kullanmak için VPN yapılandırma için talimatlar sağlar.

NPS uzantısını yükledikten ve yapılandırdıktan sonra, bu sunucu tarafından işlenen tüm RADIUS tabanlı istemci kimlik doğrulamasının MFA'yı kullanması gerekir. Tüm VPN kullanıcılarınız Azure Çok Faktörlü Kimlik Doğrulama'ya kayıtlı değilse, aşağıdakilerden birini yapabilirsiniz:

* MFA kullanmak üzere yapılandırılan kullanıcıların kimliğini doğrulamak için başka bir RADIUS sunucusu ayarlayın.

* Sorunlu kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama'ya kayıtlı olmaları durumunda ikinci bir kimlik doğrulama faktörü sağlamalarına olanak tanıyan bir kayıt defteri girişi oluşturun.

_HKLM\SOFTWARE\Microsoft\AzureMfa'da REQUIRE_USER_MATCH_adında yeni bir dize değeri oluşturun ve değeri *True* or *False*olarak ayarlayın.

!["Kullanıcı Eşleşmesi Gerektir" ayarı](./media/howto-mfa-nps-extension-vpn/image34.png)

Değer *True* olarak ayarlanmışsa veya boşsa, tüm kimlik doğrulama istekleri MFA zorluğuna tabidir. Değer *False*olarak ayarlanmışsa, MFA zorlukları yalnızca Azure Çok Faktörlü Kimlik Doğrulama'ya kayıtlı kullanıcılara verilir. *False* ayarını yalnızca test sırasında veya onboard döneminde üretim ortamlarında kullanın.

### <a name="obtain-the-azure-active-directory-guid-id"></a>Azure Active Directory GUID Kimliğini Edinin

NPS uzantısı yapılandırmasının bir parçası olarak, yönetici kimlik bilgilerini ve Azure AD kiracınızın kimliğini sağlamanız gerekir. Aşağıdakileri yaparak kimliği alın:

1. Azure kiracısının genel yöneticisi olarak [Azure portalında](https://portal.azure.com) oturum açın.

2. Azure portalı menüsünde **Azure Active Directory'yi**seçin veya herhangi bir sayfadan **Azure Active Directory'yi** arayın ve seçin.

3. **Özellikleri**seçin.

4. Azure AD Kimliğinizi kopyalamak için **Kopyala** düğmesini seçin.

    ![Azure portalında Azure AD Dizin Kimliği](./media/howto-mfa-nps-extension-vpn/azure-active-directory-id-in-azure-portal.png)

### <a name="install-the-nps-extension"></a>NPS uzantısını yükleme

NPS uzantısı, Ağ İlkesi ve Erişim Hizmetleri rolünün yüklü olduğu ve tasarımınızda RADIUS sunucusu olarak işlev görebilen bir sunucuya yüklenmelidir. VPN sunucunuza NPS *uzantısını* yüklemeyin.

1. NPS uzantısını [Microsoft Download Center'dan](https://aka.ms/npsmfa)indirin.

2. Kurulum yürütülebilir dosyasını *(NpsExtnForAzureMfaInstaller.exe) NPS*sunucusuna kopyalayın.

3. NPS sunucusunda, **NpsExtnForAzureMfaInstaller.exe'yi** çift tıklatın ve istenirse **Çalıştır'ı**seçin.

4. Azure **MFA Kurulumu için NPS Uzantısı** penceresinde, yazılım lisans koşullarını gözden geçirin, lisans hüküm ve koşulları onay kutusunu **kabul ediyorum'u** seçin ve sonra **Yükle'yi**seçin.

    !["Azure MFA Kurulumu için NPS Uzantısı" penceresi](./media/howto-mfa-nps-extension-vpn/image36.png)

5. Azure **MFA Kurulumu için NPS Uzantısı** penceresinde **Kapat'ı**seçin.  

    !["Kurulum Başarılı" onay penceresi](./media/howto-mfa-nps-extension-vpn/image37.png)

### <a name="configure-certificates-for-use-with-the-nps-extension-by-using-a-powershell-script"></a>PowerShell komut dosyası kullanarak NPS uzantısı ile kullanılacak sertifikaları yapılandırma

Güvenli iletişim ve güvence sağlamak için, Sertifikaları NPS uzantısı tarafından kullanılmak üzere yapılandırın. NPS bileşenleri, NPS ile kullanılmak üzere kendi imzalı bir sertifikayı yapılandıran bir Windows PowerShell komut dosyası içerir.

Komut dosyası aşağıdaki eylemleri gerçekleştirir:

* Kendi imzalı bir sertifika oluşturur.
* Sertifikanın ortak anahtarını Azure AD'deki hizmet sorumlusuyla ilişkilendirin.
* Sertifikayı yerel makine mağazasında saklar.
* Ağ kullanıcısına sertifikanın özel anahtarına erişim sağlar.
* NPS hizmetini yeniden başlatır.

Kendi sertifikalarınızı kullanmak istiyorsanız, sertifikanızın ortak anahtarını Azure AD'deki hizmet ilkesiyle ilişkilendirmeniz gerekir.

Komut dosyasını kullanmak için uzantıyı Azure Etkin Dizin yönetim kimlik bilgileriniz ve daha önce kopyaladığınız Azure Etkin Dizin kiracı kimliğinizi sağlayın. Komut dosyasını, NPS uzantısını yüklediğiniz her NPS sunucusunda çalıştırın.

1. Windows PowerShell'i yönetici olarak çalıştırın.

2. PowerShell komut isteminde **cd "c:\Program Files\Microsoft\AzureMfa\Config"** girin ve sonra Enter'u seçin.

3. Sonraki komut isteminde **.\AzureMfaNpsExtnConfigSetup.ps1**'i girin ve sonra Enter'u seçin. Komut dosyası, Azure AD PowerShell modülünün yüklü olup olmadığını denetler. Yüklü değilse, komut dosyası modülü sizin için yükler.

    ![AzureMfsNpsExtnConfigSetup.ps1 yapılandırma komut dosyasının çalıştırıl](./media/howto-mfa-nps-extension-vpn/image38.png)

    Komut dosyası PowerShell modülünün yüklenmesini doğruladıktan sonra Azure Active Directory PowerShell modülü oturum açma penceresini görüntüler.

4. Azure AD yönetici kimlik bilgilerinizi ve parolanızı girin ve ardından **Oturum Aç'ı**seçin.

    ![Azure AD PowerShell'e kimlik doğrulaması](./media/howto-mfa-nps-extension-vpn/image39.png)

5. Komut isteminde, daha önce kopyaladığınız kiracı kimliğini yapıştırın ve sonra Enter'u seçin.

    ![Azure AD Dizin Kimliği'ni daha önce kopyalanmış olarak girme](./media/howto-mfa-nps-extension-vpn/image40.png)

    Komut dosyası kendi imzalı bir sertifika oluşturur ve diğer yapılandırma değişikliklerini gerçekleştirir. Çıktı aşağıdaki resimde olduğu gibi:

    ![Kendi imzalı sertifikayı gösteren PowerShell penceresi](./media/howto-mfa-nps-extension-vpn/image41.png)

6. Sunucuyu yeniden başlatın.

### <a name="verify-the-configuration"></a>Yapılandırmayı doğrulama

Yapılandırmayı doğrulamak için VPN sunucusuyla yeni bir VPN bağlantısı kurmanız gerekir. Birincil kimlik doğrulama için kimlik bilgilerinizi başarıyla girdikten sonra, VPN bağlantısı aşağıda gösterildiği gibi bağlantı kurulmadan önce ikincil kimlik doğrulamasının başarılı olmasını bekler.

![Windows Ayarları VPN penceresi](./media/howto-mfa-nps-extension-vpn/image42.png)

Azure MFA'da daha önce yapılandırdığınız ikincil doğrulama yöntemiyle başarılı bir şekilde kimlik doğrulaması yaptıysanız, kaynağa bağlısınız. Ancak, ikincil kimlik doğrulaması başarısız olursa, kaynağa erişiminiz engellenir.

Aşağıdaki örnekte, Windows Phone'daki Microsoft Authenticator uygulaması ikincil kimlik doğrulamasını sağlar:

![Windows Phone'da Örnek MFA istemi](./media/howto-mfa-nps-extension-vpn/image43.png)

İkincil yöntemi kullanarak başarılı bir şekilde kimlik doğrulaması yaptıktan sonra, VPN sunucusundaki sanal bağlantı noktasına erişim hakkı verilir. Güvenilir bir cihazda mobil bir uygulama kullanarak ikincil bir kimlik doğrulama yöntemi kullanmanız gerektiğinden, oturum açma işlemi yalnızca kullanıcı adı ve parola kombinasyonu kullanmasından daha güvenlidir.

### <a name="view-event-viewer-logs-for-successful-sign-in-events"></a>Başarılı oturum açma etkinlikleri için Olay Görüntüleyicisi günlüğünü görüntüleme

Windows Event Viewer'daki başarılı oturum açma olaylarını görüntülemek için, Aşağıdaki PowerShell komutunu girerek NPS sunucusundaki Windows Security günlüklerini sorgular:

    `Get-WinEvent -Logname Security | where {$_.ID -eq '6272'} | FL`

![PowerShell güvenlik Olay Görüntüleyicisi](./media/howto-mfa-nps-extension-vpn/image44.png)

Güvenlik günlüğünü veya Ağ İlkesi ve Erişim Hizmetleri özel görünümünü de burada gösterildiği gibi görüntüleyebilirsiniz:

![Örnek Ağ İlkesi Sunucu günlüğü](./media/howto-mfa-nps-extension-vpn/image45.png)

Azure Çok Faktörlü Kimlik Doğrulama için NPS uzantısını yüklediğiniz sunucuda, *Uygulama ve Hizmet Günlükleri\Microsoft\AzureMfa'daki*uzantıya özgü Olay Görüntüleyicisi uygulama günlüklerini bulabilirsiniz.

    `Get-WinEvent -Logname Security | where {$_.ID -eq '6272'} | FL`

![Örnek Olay Görüntüleyici AuthZ günlükleri bölme](./media/howto-mfa-nps-extension-vpn/image46.png)

## <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Yapılandırma beklendiği gibi çalışmıyorsa, kullanıcının MFA kullanacak şekilde yapılandırıldığından doğrulayarak sorun gidermeye başlayın. Kullanıcının [Azure portalına](https://portal.azure.com)bağlanmasını sağlar. Kullanıcıdan ikincil kimlik doğrulaması istenirse ve başarıyla kimlik doğrulaması istenebilirse, sorun olarak MFA'nın yanlış yapılandırmasını ortadan kaldırabilirsiniz.

MFA kullanıcı için çalışıyorsa, ilgili Olay Görüntüleyicisi günlüklerini inceleyin. Günlükler, önceki bölümde tartışılan güvenlik olayını, Ağ Geçidi işletimini ve Azure Çok Faktörlü Kimlik Doğrulama günlüğünü içerir.

Başarısız oturum açma olayını (olay kimliği 6273) görüntüleyen bir güvenlik günlüğü örneği burada gösterilmiştir:

![Başarısız oturum açma olayını gösteren güvenlik günlüğü](./media/howto-mfa-nps-extension-vpn/image47.png)

Azure Çok Faktörlü Kimlik Doğrulama günlüğünden ilgili bir olay burada gösterilmiştir:

![Azure Çok Faktörlü Kimlik Doğrulama günlükleri](./media/howto-mfa-nps-extension-vpn/image48.png)

Gelişmiş sorun giderme yapmak için, NPS hizmetinin yüklü olduğu NPS veritabanı biçimi günlük dosyalarına başvurun. Günlük dosyaları _%SystemRoot%\System32\Logs_ klasöründe virgülle sınırlandırılmış metin dosyaları olarak oluşturulur. Günlük dosyalarının açıklaması için [NPS Veritabanı Biçim Günlüğü Dosyalarını Yorumla'ya](https://technet.microsoft.com/library/cc771748.aspx)bakın.

Bu günlük dosyalarındaki girişleri, bir elektronik tabloya veya veritabanına dışa aktarmadığınız sürece yorumlamak zordur. Günlük dosyalarını yorumlamada size yardımcı olmak için çevrimiçi birçok Internet Kimlik Doğrulama Hizmeti (IAS) ayrıştırma aracı bulabilirsiniz. Böyle bir indirilebilir [shareware uygulamaçıktı](https://www.deepsoftware.com/iasviewer) burada gösterilmiştir:

![Örnek Shareware uygulaması IAS parser](./media/howto-mfa-nps-extension-vpn/image49.png)

Ek sorun giderme yapmak için Wireshark veya Microsoft [İleti Çözümleyicisi](https://technet.microsoft.com/library/jj649776.aspx)gibi bir protokol çözümleyicisi kullanabilirsiniz. Wireshark'ın aşağıdaki görüntüsü VPN sunucusu ile NPS arasındaki RADIUS mesajlarını gösterir.

![Filtreuygulanmış trafiği gösteren Microsoft İleti Çözümleyicisi](./media/howto-mfa-nps-extension-vpn/image50.png)

Daha fazla bilgi için bkz: [Mevcut NPS altyapınızı Azure Çok Faktörlü Kimlik Doğrulaması ile tümleştirin.](howto-mfa-nps-extension.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Çok Faktörlü Kimlik Doğrulama'yı alın](concept-mfa-licensing.md)

[RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu](howto-mfaserver-nps-rdg.md)

[Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
