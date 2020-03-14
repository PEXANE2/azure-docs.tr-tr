---
title: Azure AD geçişli kimlik doğrulaması-hızlı başlangıç | Microsoft Docs
description: Bu makalede Azure Active Directory (Azure AD) geçişli kimlik doğrulaması ile çalışmaya başlama açıklanmaktadır.
services: active-directory
keywords: Azure AD Connect geçişli kimlik doğrulaması, Active Directory yüklemesi, Azure AD, SSO, çoklu oturum açma için gerekli bileşenler
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
ms.openlocfilehash: be6a6e9231b13c47d1421543464c720f6283b5f9
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79261248"
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Azure Active Directory geçişli kimlik doğrulaması: hızlı başlangıç

## <a name="deploy-azure-ad-pass-through-authentication"></a>Azure AD geçişli kimlik doğrulaması dağıtma

Azure Active Directory (Azure AD) geçişli kimlik doğrulaması, kullanıcılarınızın aynı parolaları kullanarak hem şirket içi hem de bulut tabanlı uygulamalarda oturum açmasına olanak tanır. Geçişli kimlik doğrulaması, kullanıcıların parolalarını doğrudan şirket içi Active Directory karşı doğrulayarak imzalamalarını ister.

>[!IMPORTANT]
>AD FS (veya diğer Federasyon teknolojileri) üzerinden doğrudan kimlik doğrulamaya geçiş yapıyorsanız, [burada](https://aka.ms/adfstoPTADPDownload)yayımlanan ayrıntılı dağıtım kılavuzumuzu izlemenizi kesinlikle öneririz.

Kiracınızda geçişli kimlik doğrulaması dağıtmak için aşağıdaki yönergeleri izleyin:

## <a name="step-1-check-the-prerequisites"></a>1\. Adım: önkoşulları denetleme

Aşağıdaki önkoşulların yerinde olduğundan emin olun.

### <a name="in-the-azure-active-directory-admin-center"></a>Azure Active Directory Yönetim merkezinde

1. Azure AD kiracınızda yalnızca bulutta yer alan bir genel yönetici hesabı oluşturun. Bu şekilde, şirket içi hizmetleriniz başarısız olması veya kullanılamaz hale gelmesi için kiracınızın yapılandırmasını yönetebilirsiniz. [Yalnızca bulut genel yönetici hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu adımın tamamlanması, kiracınızdan kilitlenmemesini sağlamak açısından önemlidir.
2. Azure AD kiracınıza bir veya daha fazla [özel etki alanı adı](../active-directory-domains-add-azure-portal.md) ekleyin. Kullanıcılarınız bu etki alanı adlarından biriyle oturum açabilir.

### <a name="in-your-on-premises-environment"></a>Şirket içi ortamınızda

1. Azure AD Connect çalıştırmak için Windows Server 2012 R2 veya üstünü çalıştıran bir sunucu belirler. Henüz etkinleştirilmemişse, [sunucuda TLS 1,2](./how-to-connect-install-prerequisites.md#enable-tls-12-for-azure-ad-connect)' ı etkinleştirin. Parolasını doğrulamanız gereken kullanıcılarla aynı Active Directory ormanına ekleyin.
2. Önceki adımda tanımlanan sunucuya [Azure AD Connect en son sürümünü](https://www.microsoft.com/download/details.aspx?id=47594) yükler. Zaten Azure AD Connect çalışıyorsa, sürümün 1.1.750.0 veya üzeri olduğundan emin olun.

    >[!NOTE]
    >Azure AD Connect sürümleri 1.1.557.0, 1.1.558.0, 1.1.561.0 ve 1.1.614.0, Parola karması eşitlemeyle ilgili bir sorun var. Parola karması eşitlemesini doğrudan kimlik doğrulamasıyla birlikte _kullanmayı düşünmüyorsanız_ [Azure AD Connect sürüm notlarını](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#116470)okuyun.

3. Tek başına kimlik doğrulama aracılarını çalıştırabileceğiniz bir veya daha fazla ek sunucuyu (TLS 1,2 etkin Windows Server 2012 R2 veya üstünü çalıştıran) belirler. Bu ek sunucular, oturum açma isteklerinin yüksek kullanılabilirliğini sağlamak için gereklidir. Parolaları doğrulamanız gereken kullanıcılarla aynı Active Directory ormanına ekleyin.

    >[!IMPORTANT]
    >Üretim ortamlarında, kiracınızda en az 3 kimlik doğrulama aracınız çalışıyor olması önerilir. Kiracı başına 40 kimlik doğrulama aracısından oluşan bir sistem limiti vardır. En iyi yöntem olarak, kimlik doğrulama aracılarını çalıştıran tüm sunucuları katman 0 sistemleri olarak değerlendirin (bkz. [başvuru](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

4. Sunucularınız ve Azure AD arasında bir güvenlik duvarı varsa, aşağıdaki öğeleri yapılandırın:
   - Kimlik doğrulama aracılarının Azure AD 'ye *giden* istekleri aşağıdaki bağlantı noktaları üzerinden yapabildiğinden emin olun:

     | Bağlantı noktası numarası | Nasıl kullanılır? |
     | --- | --- |
     | **80** | SSL sertifikası doğrulanırken sertifika iptal listelerini (CRL 'Ler) indirir |
     | **443** | Hizmetle tüm giden iletişimi işler |
     | **8080** (isteğe bağlı) | Bağlantı noktası 443 kullanılamıyorsa, kimlik doğrulama aracıları, 8080 numaralı bağlantı noktası üzerinden her on dakikada bir durum bildirir. Bu durum Azure AD portalında görüntülenir. 8080 numaralı bağlantı noktası Kullanıcı oturum açma işlemleri _için kullanılmaz._ |
     
     Güvenlik duvarınız, kaynak kullanıcılara göre kuralları zorlarsa, ağ hizmeti olarak çalışan Windows hizmetlerinden gelen trafik için bu bağlantı noktalarını açın.
   - Güvenlik duvarınız veya proxy 'niz DNS beyaz listeye izin veriyorsa, **\*. msappproxy.net** ve **\*. ServiceBus.Windows.net**ile bağlantıları güvenilir bir şekilde listeleyin. Aksi takdirde, haftalık olarak güncellenen [Azure veri MERKEZI IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişime izin verin.
   - Kimlik doğrulama aracılarınızın ilk kayıt için **login.Windows.net** ve **login.microsoftonline.com** 'e erişmesi gerekir. Bu URL 'Ler için güvenlik duvarınızı da açın.
   - Sertifika doğrulaması için şu URL 'Leri engellemeyi kaldırın: **mscrl.Microsoft.com:80**, **CRL.Microsoft.com:80**, **OCSP.msocsp.com:80**ve **www\.Microsoft.com:80**. Bu URL 'Ler diğer Microsoft ürünleriyle sertifika doğrulaması için kullanıldığından, bu URL 'Lerin engeli kaldırılmış olabilir.

## <a name="step-2-enable-the-feature"></a>2\. Adım: özelliği etkinleştirme

[Azure AD Connect](whatis-hybrid-identity.md)aracılığıyla doğrudan kimlik doğrulamasını etkinleştirin.

>[!IMPORTANT]
>Azure AD Connect birincil veya hazırlama sunucusu üzerinde doğrudan kimlik doğrulamasını etkinleştirebilirsiniz. Birincil sunucudan etkinleştirmeniz önemle önerilir. Gelecekte bir Azure AD Connect hazırlama sunucusu ayarlıyorsanız, oturum açma seçeneği olarak geçişli kimlik doğrulaması ' nı seçerek devam etmeniz **gerekir** ; başka bir seçenek belirlendiğinde, Kiracıdaki geçişli kimlik doğrulaması **devre dışı** bırakılır ve birincil sunucudaki ayar geçersiz kılınır.

İlk kez Azure AD Connect yüklüyorsanız, [özel yükleme yolunu](how-to-connect-install-custom.md)seçin. **Kullanıcı oturum açma** sayfasında, **oturum açma yöntemi**olarak **geçişli kimlik doğrulaması** ' nı seçin. Başarılı bir şekilde tamamlandığında, Azure AD Connect ile aynı sunucuya bir geçişli kimlik doğrulama Aracısı yüklenir. Ek olarak, kiracınızda doğrudan kimlik doğrulama özelliği etkinleştirilmiştir.

![Azure AD Connect: Kullanıcı oturumu açma](./media/how-to-connect-pta-quick-start/sso3.png)

Azure AD Connect, [hızlı yükleme](how-to-connect-install-express.md) veya [özel yükleme](how-to-connect-install-custom.md) yolunu kullanarak zaten yüklediyseniz, Azure AD Connect **Kullanıcı oturum açma görevini Değiştir** ' i seçin ve ardından **İleri**' yi seçin. Ardından oturum açma yöntemi olarak **geçişli kimlik doğrulaması** ' nı seçin. Başarılı bir şekilde tamamlandığında, bir geçişli kimlik doğrulama Aracısı, Azure AD Connect aynı sunucuya yüklenir ve bu özellik kiracınızda etkinleştirilir.

![Azure AD Connect: Kullanıcı oturumunu değiştirme](./media/how-to-connect-pta-quick-start/changeusersignin.png)

>[!IMPORTANT]
>Geçişli kimlik doğrulaması, kiracı düzeyinde bir özelliktir. ' In açık olması, kiracınızdaki _Tüm_ yönetilen etki alanlarında bulunan kullanıcılar için oturum açma özelliğini etkiler. Active Directory Federasyon Hizmetleri (AD FS) (AD FS) 'den geçişli kimlik doğrulamaya geçiş yapıyorsanız AD FS altyapınızı kapatmadan önce en az 12 saat beklemeniz gerekir. Bu bekleme süresi, kullanıcıların geçiş sırasında Exchange ActiveSync 'de oturum açabildiğinden emin olmak için kullanılır. AD FS 'ten geçişli kimlik doğrulamaya geçiş hakkında daha fazla yardım için, [burada](https://aka.ms/adfstoptadpdownload)yayımlanan ayrıntılı dağıtım planına göz atın.

## <a name="step-3-test-the-feature"></a>3\. Adım: özelliği test etme

Doğrudan kimlik doğrulamayı doğru şekilde etkinleştirdiğinizi doğrulamak için aşağıdaki yönergeleri izleyin:

1. [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) kiracınızın genel yönetici kimlik bilgileriyle oturum açın.
2. Sol bölmedeki **Azure Active Directory** seçin.
3. **Azure AD Connect**seçin.
4. **Geçişli kimlik doğrulaması** özelliğinin **etkin**olarak göründüğünü doğrulayın.
5. **Doğrudan kimlik doğrulaması**' nı seçin. **Geçişli kimlik doğrulaması** bölmesi, kimlik doğrulama aracılarınızın yüklendiği sunucuları listeler.

![Azure Active Directory Yönetim Merkezi: Azure AD Connect bölmesi](./media/how-to-connect-pta-quick-start/pta7.png)

![Azure Active Directory Yönetim Merkezi: geçişli kimlik doğrulama bölmesi](./media/how-to-connect-pta-quick-start/pta8.png)

Bu aşamada, kiracınızdaki tüm yönetilen etki alanlarından kullanıcılar doğrudan kimlik doğrulaması kullanarak oturum açabilir. Ancak, Federasyon etki alanlarının kullanıcıları daha önce yapılandırdığınız AD FS veya başka bir Federasyon sağlayıcısı kullanarak oturum açmaya devam eder. Bir etki alanını federe 'dan yönetilene dönüştürürseniz, bu etki alanındaki tüm kullanıcılar doğrudan kimlik doğrulaması kullanarak oturum açmaya başlar. Doğrudan kimlik doğrulama özelliği yalnızca bulutta kullanıcıları etkilemez.

## <a name="step-4-ensure-high-availability"></a>4\. Adım: yüksek kullanılabilirlik sağlayın

Doğrudan kimlik doğrulamayı bir üretim ortamında dağıtmayı planlıyorsanız, ek bağımsız kimlik doğrulama aracılarını yüklemelisiniz. Bu kimlik doğrulama aracılarını, Azure AD Connect çalıştıran sunucu (ler) i _yerine sunucuya_ yükler. Bu kurulum, Kullanıcı oturum açma istekleri için yüksek kullanılabilirlik sağlar.

>[!IMPORTANT]
>Üretim ortamlarında, kiracınızda en az 3 kimlik doğrulama aracınız çalışıyor olması önerilir. Kiracı başına 40 kimlik doğrulama aracısından oluşan bir sistem limiti vardır. En iyi yöntem olarak, kimlik doğrulama aracılarını çalıştıran tüm sunucuları katman 0 sistemleri olarak değerlendirin (bkz. [başvuru](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

Birden çok geçişli kimlik doğrulama Aracısı yükleme, yüksek kullanılabilirlik sağlar, ancak kimlik doğrulama aracıları arasında belirleyici yük dengeleyiciyi sağlamaz. Kiracınız için kaç kimlik doğrulama Aracısı gerektiğini öğrenmek üzere kiracınızda görmeyi düşündüğünüz oturum açma isteklerinin tepe ve ortalama yükünü göz önünde bulundurun. Bir kıyaslama olarak, tek bir kimlik doğrulama Aracısı standart 4 çekirdekli bir CPU, 16 GB RAM sunucusunda 300 ila 400 kimlik doğrulaması işleyebilir.

Ağ trafiğini tahmin etmek için aşağıdaki boyutlandırma kılavuzunu kullanın:
- Her isteğin yük boyutu (0,5 K + 1K * num_of_agents) bayt) vardır. Yani, Azure AD 'den kimlik doğrulama aracısına yönelik veriler. Burada, "num_of_agents" kiracınızda kayıtlı kimlik doğrulama aracılarının sayısını gösterir.
- Her yanıtın yük boyutu 1K bayttır; Yani, kimlik doğrulama aracısından Azure AD 'ye ait veriler.

Çoğu müşteri için toplamda üç kimlik doğrulama Aracısı yüksek kullanılabilirlik ve kapasite için yeterlidir. Oturum açma gecikmesini geliştirmek için, kimlik doğrulama aracılarını etki alanı denetleyicilerinize yakın bir şekilde yüklemelisiniz.

Başlamak için, kimlik doğrulama Aracısı yazılımını indirmek için aşağıdaki yönergeleri izleyin:

1. Kimlik doğrulama aracısının en son sürümünü (sürüm 1.5.193.0 veya üzeri) indirmek için, kiracınızın genel yönetici kimlik bilgilerinizle [Azure Active Directory Yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. Sol bölmedeki **Azure Active Directory** seçin.
3. **Azure AD Connect**' yi seçin, **geçişli kimlik doğrulaması**' nı seçin ve ardından **aracıyı indir**' i seçin.
4. **Koşulları kabul & İndir** düğmesini seçin.

![Azure Active Directory Yönetim Merkezi: kimlik doğrulama aracısını Indir düğmesi](./media/how-to-connect-pta-quick-start/pta9.png)

![Azure Active Directory Yönetim Merkezi: aracı bölmesini Indir](./media/how-to-connect-pta-quick-start/pta10.png)

>[!NOTE]
>Ayrıca [, kimlik doğrulama Aracısı yazılımını doğrudan indirebilirsiniz](https://aka.ms/getauthagent). Yükleme _yapmadan önce_ kimlik doğrulama aracısının [hizmet koşullarını](https://aka.ms/authagenteula) gözden geçirin ve kabul edin.

Tek başına kimlik doğrulama aracısını dağıtmanın iki yolu vardır:

İlk olarak, yalnızca indirilen kimlik doğrulama Aracısı yürütülebilir dosyasını çalıştırarak ve istendiğinde kiracınızın genel yönetici kimlik bilgilerini sağlayarak bunu etkileşimli olarak yapabilirsiniz.

İkincisi, katılımsız dağıtım betiği oluşturup çalıştırabilirsiniz. Bu, aynı anda birden fazla kimlik doğrulama Aracısı dağıtmak veya Kullanıcı arabirimi etkinleştirilmemiş olan veya uzak masaüstü ile erişemeyen Windows Server 'lar üzerinde kimlik doğrulama aracıları yüklemek istediğinizde yararlıdır. Bu yaklaşımın nasıl kullanılacağına ilişkin yönergeler aşağıda verilmiştir:

1. Kimlik doğrulama Aracısı yüklemek için şu komutu çalıştırın: `AADConnectAuthAgentSetup.exe REGISTERCONNECTOR="false" /q`.
2. Windows PowerShell 'i kullanarak hizmetimizi kullanarak kimlik doğrulama aracısını kaydedebilirsiniz. Kiracınız için genel Yönetici Kullanıcı adı ve parolası içeren bir PowerShell kimlik bilgileri nesnesi `$cred` oluşturun. *\<Kullanıcı adı\>* ve *\<parola\>* değiştirerek aşağıdaki komutu çalıştırın:

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $SecurePassword
3. **C:\Program Files\Microsoft Azure AD Connect kimlik doğrulama aracısına** gidin ve oluşturduğunuz `$cred` nesnesini kullanarak aşağıdaki betiği çalıştırın:

        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft Azure AD Connect Authentication Agent\Modules\" -moduleName "PassthroughAuthPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature PassthroughAuthentication

>[!IMPORTANT]
>Bir sanal makinede bir kimlik doğrulama Aracısı yüklüyse, başka bir kimlik doğrulama Aracısı kurmak için sanal makineyi klonlamamanız gerekir. Bu yöntem **desteklenmiyor**.

## <a name="step-5-configure-smart-lockout-capability"></a>5\. Adım: akıllı kilitleme özelliğini yapılandırma

Akıllı kilitleme, kullanıcılarınızın parolalarını tahmin etmeye çalışan kötü aktörlerin kilitlenmeye veya alma için deneme yanılma yöntemlerinin kullanılmasına yardımcı olur. Azure AD 'de akıllı kilitleme ayarlarını ve/veya şirket içi Active Directory uygun kilitleme ayarlarını yapılandırarak saldırıları Active Directory erişmeden önce bu saldırılara karşı filtrelenebilir. Kullanıcı hesaplarınızı korumak üzere kiracınızda akıllı kilitleme ayarlarını yapılandırma hakkında daha fazla bilgi edinmek için [Bu makaleyi](../authentication/howto-password-smart-lockout.md) okuyun.

## <a name="next-steps"></a>Sonraki adımlar
- [AD FS 'den geçişli kimlik doğrulamaya geçiş](https://aka.ms/adfstoptadp) -AD FS (veya diğer Federasyon teknolojileri) üzerinden doğrudan kimlik doğrulamaya geçiş yapmak için ayrıntılı kılavuz.
- [Akıllı kilitleme](../authentication/howto-password-smart-lockout.md): Kullanıcı hesaplarını korumak için kiracınızda akıllı kilitleme özelliğini nasıl yapılandıracağınızı öğrenin.
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md): doğrudan kimlik doğrulamasıyla hangi senaryoların desteklendiğini ve hangilerinin hangilerinin desteklenmediğini öğrenin.
- [Teknik derinlemesine](how-to-connect-pta-how-it-works.md)bakış: geçişli kimlik doğrulama özelliğinin nasıl çalıştığını anlayın.
- [Sık sorulan sorular](how-to-connect-pta-faq.md): sık sorulan soruların yanıtlarını bulun.
- [Sorun giderme](tshoot-connect-pass-through-authentication.md): doğrudan kimlik doğrulama özelliğiyle yaygın sorunları çözmeyi öğrenin.
- [Güvenlik derinlemesine](how-to-connect-pta-security-deep-dive.md)bakış: doğrudan kimlik doğrulama özelliği hakkında teknik bilgiler alın.
- [Azure AD sorunsuz SSO](how-to-connect-sso.md): Bu tamamlayıcı özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): yeni özellik isteklerini dosya olarak yüklemek Için Azure Active Directory forumunu kullanın.
