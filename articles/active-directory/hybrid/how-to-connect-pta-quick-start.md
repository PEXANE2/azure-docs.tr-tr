---
title: Azure AD Pass-through Kimlik Doğrulama - Hızlı başlangıç | Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması ile nasıl başlatılanın.
services: active-directory
keywords: Azure AD Connect Pass-through Authentication, yükleme Active Directory, Azure AD, SSO, Tek Oturum açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6fc45033cdf1bdaa6d4ecd6ab58cc7f90ff9c1ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331426"
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory Pass-through Authentication: Hızlı başlangıç

## <a name="deploy-azure-ad-pass-through-authentication"></a>Azure AD Geçiş Kimlik Doğrulaması dağıtma

Azure Active Directory (Azure AD) Geçiş Kimlik Doğrulaması, kullanıcılarınızın aynı parolaları kullanarak hem şirket içinde hem de bulut tabanlı uygulamalarda oturum açmalarına olanak tanır. Geçiş Kimlik Doğrulama, parolalarını doğrudan şirket içi Active Directory'ye karşı doğrulayarak kullanıcıları işaretler.

>[!IMPORTANT]
>AD FS'den (veya diğer federasyon teknolojilerinden) Geçiş Kimlik Doğrulamasına geçiş yapıyorsunuz, [burada](https://aka.ms/adfstoPTADPDownload)yayınlanan ayrıntılı dağıtım kılavuzumuzu izlemenizi öneririz.

Kiracınıza Geçiş Kimlik Doğrulaması dağıtmak için aşağıdaki yönergeleri izleyin:

## <a name="step-1-check-the-prerequisites"></a>Adım 1: Ön koşulları kontrol edin

Aşağıdaki ön koşulların yerinde olduğundan emin olun.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Etkin Dizin yönetici merkezinde

1. Azure AD kiracınızda yalnızca buluta özel bir global yönetici hesabı oluşturun. Bu şekilde, şirket içi hizmetleriniz başarısız olursa veya kullanılamıyorsa kiracınızın yapılandırmasını yönetebilirsiniz. Yalnızca [buluta özel genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımı tamamlamak, kiracınızın kilitsiz kaldığınızdan emin olmak için çok önemlidir.
2. Azure AD kiracınıza bir veya daha fazla [özel alan adı](../active-directory-domains-add-azure-portal.md) ekleyin. Kullanıcılarınız bu alan adlarından biriyle oturum açabilir.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. Azure AD Connect'i çalıştırmak için Windows Server 2012 R2 veya daha sonra çalışan bir sunucu tanımlayın. Zaten etkinleştirildiyse, [sunucuda TLS 1.2'yi etkinleştirin.](./how-to-connect-install-prerequisites.md#enable-tls-12-for-azure-ad-connect) Sunucuyu, parolalarını doğrulamanız gereken kullanıcılarla aynı Etkin Dizin ormanına ekleyin.
2. Azure [AD Connect'in en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) önceki adımda tanımlanan sunucuya yükleyin. Azure AD Connect zaten çalışıyorsa, sürümün 1.1.750.0 veya daha sonra olduğundan emin olun.

    >[!NOTE]
    >Azure AD Connect sürümleri 1.1.557.0, 1.1.558.0, 1.1.561.0 ve 1.1.614.0 sürümlerinde parola karma eşitlemeyle ilgili bir sorun vardır. Parola _don't_ karma senkronizasyonunu Pass-through Authentication ile birlikte kullanmak istemiyorsanız, [Azure AD Connect sürüm notlarını](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#116470)okuyun.

3. Bağımsız Kimlik Doğrulama Aracıları çalıştırabileceğiniz bir veya daha fazla ek sunucu (TLS 1.2 etkinken Windows Server 2012 R2 veya daha sonra çalıştırarak) tanımlayın. Bu ek sunucular, oturum açma isteklerinin yüksek kullanılabilirliğini sağlamak için gereklidir. Sunucuları, parolalarını doğrulamanız gereken kullanıcılarla aynı Etkin Dizin ormanına ekleyin.

    >[!IMPORTANT]
    >Üretim ortamlarında, kiracınızda çalışan en az 3 Kimlik Doğrulama Aracısı'na sahip olduğunuzu öneririz. Kiracı başına 40 Kimlik Doğrulama Aracısı sistem sınırı vardır. Ve en iyi yöntem olarak, Kimlik Doğrulama Aracıları çalıştıran tüm sunucuları Katman 0 sistemleri olarak ele abakın [(bkz. başvuru).](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)

4. Sunucularınız ile Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğeleri yapılandırın:
   - Kimlik Doğrulama Aracıları'nın Azure AD'ye aşağıdaki bağlantı noktaları üzerinden *giden* isteklerde bulunabilmesini sağlayın:

     | Bağlantı noktası numarası | Nasıl kullanılır? |
     | --- | --- |
     | **80** | TLS/SSL sertifikasını doğrularken sertifika iptal listelerini (CRLs) karşıdan yükler |
     | **443** | Hizmetle ilgili tüm giden iletişimi işler |
     | **8080** (isteğe bağlı) | Kimlik doğrulama aracıları, bağlantı noktası 443 kullanılamıyorsa, 8080 portu üzerinden her on dakikada bir durumlarını bildirir. Bu durum Azure AD portalında görüntülenir. Port 8080 kullanıcı oturum açma için _kullanılmaz._ |
     
     Güvenlik duvarınız kuralları oluşturan kullanıcılara göre zorluyorsa, bu bağlantı noktalarını ağ hizmeti olarak çalışan Windows hizmetlerinden gelen trafiğe açın.
   - Güvenlik duvarınız veya proxy'niz DNS'nin beyaz listeye alınmasına izin veriyorsa, ** \*beyaz** liste bağlantıları .msappproxy.net ve ** \*.servicebus.windows.net.** Değilse, haftalık olarak güncelleştirilen [Azure veri merkezi IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişin.
   - Kimlik Doğrulama Aracılarınızın ilk kayıt için **login.windows.net** ve **login.microsoftonline.com** erişimi gerekir. Bu URL'ler için de güvenlik duvarınızı açın.
   - Sertifika doğrulama için aşağıdaki URL'lerin engelini kaldırın: **mscrl.microsoft.com:80,** **crl.microsoft.com:80,** **ocsp.msocsp.com:80**ve **\.www microsoft.com:80.** Bu URL'ler diğer Microsoft ürünleriyle sertifika doğrulama için kullanıldığından, bu URL'lerin engelini kaldırmış olabilirsiniz.

## <a name="step-2-enable-the-feature"></a>Adım 2: Özelliği etkinleştirme

[Azure AD Connect](whatis-hybrid-identity.md)üzerinden Geçiş Kimlik Doğrulaması'nı etkinleştirin.

>[!IMPORTANT]
>Azure AD Connect birincil veya evreleme sunucusunda Geçiş Kimlik Doğrulaması'nı etkinleştirebilirsiniz. Birincil sunucudan etkinleştirmeniz önerilir. Gelecekte bir Azure AD Connect evreleme sunucusu kuruyorsanız, oturum açma seçeneği olarak Geçiş Kimlik Doğrulama'yı seçmeye devam **etmelisiniz;** başka bir seçenek seçmek, kiracıdaki Geçiş Kimlik Doğrulaması'nı **devre dışı** bırakırsa ve birincil sunucudaki ayarı geçersiz kılar.

Azure AD Connect'i ilk kez yüklüyorsanız, [özel yükleme yolunu](how-to-connect-install-custom.md)seçin. Kullanıcı **oturum açma** sayfasında, **Oturum Açma yöntemi**olarak Geçiş Kimlik **Doğrulaması'nı** seçin. Başarılı bir şekilde tamamlandığında, Azure AD Connect ile aynı sunucuya bir Geçiş Kimlik Doğrulama Aracısı yüklenir. Ayrıca, kiracınızda Geçiş Kimlik Doğrulaması özelliği etkinleştirilir.

![Azure AD Connect: Kullanıcı oturum açma](./media/how-to-connect-pta-quick-start/sso3.png)

[Ekspres yüklemeyi](how-to-connect-install-express.md) veya [özel yükleme](how-to-connect-install-custom.md) yolunu kullanarak Azure AD Connect'i zaten yüklediyseniz, Azure AD Connect'te kullanıcı **oturum açma** görevini değiştir'i seçin ve **ardından İleri'yi**seçin. Ardından oturum açma yöntemi olarak **Geçiş Kimlik Doğrulaması'nı** seçin. Başarılı bir şekilde tamamlandığında, Azure AD Connect ile aynı sunucuya bir Geçiş Kimlik Doğrulama Aracısı yüklenir ve özellik kiracınızda etkinleştirilir.

![Azure AD Bağlantısı: Kullanıcı oturum açma](./media/how-to-connect-pta-quick-start/changeusersignin.png)

>[!IMPORTANT]
>Geçiş Kimlik Doğrulaması kiracı düzeyinde bir özelliktir. Bunu açmak, kiracınızdaki _tüm_ yönetilen etki alanlarındaki kullanıcılar için oturum açma'yı etkiler. Active Directory Federation Services 'dan (AD FS) Geçiş Kimlik Doğrulaması'na geçiyorsanız, AD FS altyapınızı kapatmadan önce en az 12 saat beklemeniz gerekir. Bu bekleme süresi, kullanıcıların geçiş sırasında Exchange ActiveSync'de oturum açmaya devam edebilmelerini sağlamak içindir. AD FS'den Geçiş Kimlik Doğrulamasına geçiş konusunda daha fazla yardım [için, burada](https://aka.ms/adfstoptadpdownload)yayınlanan ayrıntılı dağıtım planımıza göz atın.

## <a name="step-3-test-the-feature"></a>Adım 3: Özelliği test edin

Geçiş Kimlik Doğrulaması'nı doğru şekilde etkinleştirdiğinizi doğrulamak için aşağıdaki yönergeleri izleyin:

1. Kiracınız için genel yönetici kimlik bilgileriyle [Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
2. Sol bölmede **Azure Etkin Dizini'ni** seçin.
3. **Azure AD Connect'i**seçin.
4. **Geçiş kimlik doğrulama** özelliğinin **Etkin**olarak göründüğünü doğrulayın.
5. **Geçiş kimlik doğrulaması'nı**seçin. **Geçiş kimlik doğrulama** bölmesi, Kimlik Doğrulama Aracılarınızın yüklendiği sunucuları listeler.

![Azure Active Directory yönetici merkezi: Azure AD Connect bölmesi](./media/how-to-connect-pta-quick-start/pta7.png)

![Azure Active Directory yönetici merkezi: Geçiş Kimlik Doğrulama bölmesi](./media/how-to-connect-pta-quick-start/pta8.png)

Bu aşamada, kiracınızdaki tüm yönetilen etki alanlarından kullanıcılar Geçiş Kimlik Doğrulaması'nı kullanarak oturum açabilir. Ancak, federe etki alanlarından kullanıcılar, daha önce yapılandırdığınız AD FS'yi veya başka bir federasyon sağlayıcısını kullanarak oturum açmaya devam eder. Bir etki alanını federeden yönetilene dönüştürürseniz, bu etki alanından tüm kullanıcılar Geçiş Kimlik Doğrulaması'nı kullanarak otomatik olarak oturum açmaya başlar. Geçiş Kimlik Doğrulama özelliği yalnızca bulut kullanıcıları etkilemez.

## <a name="step-4-ensure-high-availability"></a>Adım 4: Yüksek kullanılabilirlik sağlayın

Geçiş Kimlik Doğrulaması'nı bir üretim ortamında dağıtmayı planlıyorsanız, ek bağımsız Kimlik Doğrulama Aracıları yüklemeniz gerekir. Bu Authentication Agent(lar)'ı, Azure AD Connect çalıştıran sunucu(lar) _dışında_ sunucuya yükleyin. Bu kurulum, kullanıcı oturum açma istekleri için yüksek kullanılabilirlik sağlar.

>[!IMPORTANT]
>Üretim ortamlarında, kiracınızda çalışan en az 3 Kimlik Doğrulama Aracısı'na sahip olduğunuzu öneririz. Kiracı başına 40 Kimlik Doğrulama Aracısı sistem sınırı vardır. Ve en iyi yöntem olarak, Kimlik Doğrulama Aracıları çalıştıran tüm sunucuları Katman 0 sistemleri olarak ele abakın [(bkz. başvuru).](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)

Birden çok Geçiş Kimlik Doğrulama Aracısı yüklemek, Kimlik Doğrulama Aracıları arasında yüksek kullanılabilirlik sağlar, ancak belirleyici yük dengelemesini sağlar. Kiracınız için kaç Kimlik Doğrulama Aracısı'na ihtiyacınız olduğunu belirlemek için, kiracınızda görmeyi beklediğiniz en yüksek ve ortalama oturum açma istekleri yükünü göz önünde bulundurun. Bir ölçüt olarak, tek bir Kimlik Doğrulama Aracısı standart 4 çekirdekli işlemci, 16 GB RAM sunucusunda saniyede 300 ila 400 kimlik doğrulaması işleyebilir.

Ağ trafiğini tahmin etmek için aşağıdaki boyutlandırma kılavuzunu kullanın:
- Her istek (0,5K + 1K * num_of_agents) bayt taşıma yükü boyutuna sahiptir; örneğin, Azure AD'den Kimlik Doğrulama Aracısı'na gelen veriler. Burada "num_of_agents", kiracınızda kayıtlı Kimlik Doğrulama Aracılarının sayısını gösterir.
- Her yanıtın yük boyutu 1K baytdır; örneğin, Kimlik Doğrulama Aracısı'ndan Azure AD'ye kadar olan veriler.

Çoğu müşteri için, yüksek kullanılabilirlik ve kapasite için toplam üç Kimlik Doğrulama Aracısı yeterlidir. Oturum açma gecikmesini iyileştirmek için etki alanı denetleyicilerinize yakın Kimlik Doğrulama Aracıları yüklemeniz gerekir.

Başlangıç olarak, Kimlik Doğrulama Aracısı yazılımını indirmek için aşağıdaki yönergeleri izleyin:

1. Kimlik Doğrulama Aracısı'nın (sürüm 1.5.193.0 veya sonraki sürüm) en son sürümünü indirmek için, kiracınızın genel yönetici kimlik bilgileriyle [Azure Active Directory yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
2. Sol bölmede **Azure Etkin Dizini'ni** seçin.
3. **Azure AD Connect'i**seçin, Geçiş **kimlik doğrulaması'nı**seçin ve ardından **Aracıyı İndir'i**seçin.
4. İndirme düğmesini **& kabul terimlerini** seçin.

![Azure Active Directory yönetici merkezi: Kimlik Doğrulama Aracısı düğmesini indir](./media/how-to-connect-pta-quick-start/pta9.png)

![Azure Active Directory yönetici merkezi: Aracı bölmesine indirin](./media/how-to-connect-pta-quick-start/pta10.png)

>[!NOTE]
>Ayrıca doğrudan [Authentication Agent yazılımı indirebilirsiniz.](https://aka.ms/getauthagent) Kimlik Doğrulama Aracısı'nı yüklemeden _önce_ gözden geçirin ve kabul [edin.](https://aka.ms/authagenteula)

Bağımsız bir Kimlik Doğrulama Aracısı dağıtmanın iki yolu vardır:

İlk olarak, indirilen Kimlik Doğrulama Aracısını çalıştırarak ve istendiğinde kiracınızın genel yönetici kimlik bilgilerini sağlayarak etkileşimli olarak yapabilirsiniz.

İkinci olarak, katılımsız bir dağıtım komut dosyası oluşturabilir ve çalıştırabilirsiniz. Bu, aynı anda birden çok Kimlik Doğrulama Aracısı dağıtmak veya kullanıcı arabirimi etkin olmayan Windows sunucularına Kimlik Doğrulama Aracıları yüklemek veya Uzak Masaüstü ile erişemediğiniz zaman kullanışlıdır. Bu yaklaşımın nasıl kullanılacağına ilişkin talimatlar aşağıda veda edilmiştir:

1. Kimlik Doğrulama Aracısı yüklemek için `AADConnectAuthAgentSetup.exe REGISTERCONNECTOR="false" /q`aşağıdaki komutu çalıştırın: .
2. Kimlik Doğrulama Aracısını Windows PowerShell'i kullanarak hizmetimize kaydedebilirsiniz. Kiracınız için genel `$cred` yönetici kullanıcı adı ve parola içeren bir PowerShell Kimlik Bilgileri nesnesi oluşturun. * \<Kullanıcı\> adı* ve * \<parolayerine\>* aşağıdaki komutu çalıştırın:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $SecurePassword
3. **C:\Program Files\Microsoft Azure AD Connect Authentication Agent'a** `$cred` gidin ve oluşturduğunuz nesneyi kullanarak aşağıdaki komut dosyasını çalıştırın:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft Azure AD Connect Authentication Agent\Modules\" -moduleName "PassthroughAuthPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature PassthroughAuthentication

>[!IMPORTANT]
>Sanal Makine'ye bir Kimlik Doğrulama Aracısı yüklenmişse, sanal makineyi başka bir Kimlik Doğrulama Aracısı kurmak için klonlayamazsınız. Bu yöntem **desteklenmez.**

## <a name="step-5-configure-smart-lockout-capability"></a>Adım 5: Akıllı Kilitleme özelliğini yapılandırın

Smart Lockout, kullanıcılarınızın parolalarını tahmin etmeye çalışan veya içeri girmek için kaba kuvvet yöntemlerini kullanan kötü aktörleri kilitlemeye yardımcı olur. Azure AD'deki Akıllı Kilitleme ayarlarını ve/veya şirket içi Active Directory'de uygun kilitleme ayarlarını yapılandırarak, saldırılar Active Directory'ye ulaşmadan önce filtrelenebilir. Kullanıcı hesaplarınızı korumak için kiracınızdaki Akıllı Kilit ayarlarının nasıl yapılandırılabildiğini öğrenmek için [bu makaleyi](../authentication/howto-password-smart-lockout.md) okuyun.

## <a name="next-steps"></a>Sonraki adımlar
- [AD FS'den Geçiş Kimlik Doğrulamasına geçiş](https://aka.ms/adfstoptadp) - AD FS'den (veya diğer federasyon teknolojilerinden) Geçiş Kimlik Doğrulaması'na geçiş için ayrıntılı bir kılavuz.
- [Akıllı Kilitleme](../authentication/howto-password-smart-lockout.md): Kullanıcı hesaplarını korumak için kiracınızdaki Akıllı Kilitleme özelliğini nasıl yapılandırılabildiğini öğrenin.
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md): Geçiş Kimlik Doğrulaması ile hangi senaryoların desteklenmediğini ve hangilerinin desteklenmediğini öğrenin.
- [Teknik derin dalış](how-to-connect-pta-how-it-works.md): Geçiş Kimlik Doğrulama özelliğinin nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-pta-faq.md): Sık sorulan soruların yanıtlarını bulun.
- [Sorun Giderme](tshoot-connect-pass-through-authentication.md): Geçiş Kimlik Doğrulama özelliğiyle ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [Güvenlik derin dalış](how-to-connect-pta-security-deep-dive.md): Pass-through Kimlik Doğrulama özelliği hakkında teknik bilgi alın.
- [Azure AD Sorunsuz SSO](how-to-connect-sso.md): Bu tamamlayıcı özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik isteklerini dosyalamak için Azure Active Directory Forum'u kullanın.
