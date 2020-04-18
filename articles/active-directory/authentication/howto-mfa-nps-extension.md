---
title: NPS kullanarak Azure MFA özelliklerini sağlama - Azure Etkin Dizini
description: Mevcut kimlik doğrulama altyapınıza bulut tabanlı iki aşamalı doğrulama özellikleri ekleme
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
ms.openlocfilehash: cc1be4637d56d7205d50ebfc6f7d1d5d22e62edf
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617657"
---
# <a name="integrate-your-existing-nps-infrastructure-with-azure-multi-factor-authentication"></a>Mevcut NPS altyapınızı Azure Multi-Factor Authentication ile tümleştirme

Azure MFA için Ağ İlkesi Sunucusu (NPS) uzantısı, varolan sunucularınızı kullanarak kimlik doğrulama altyapınıza bulut tabanlı MFA özellikleri ekler. NPS uzantısı ile, yeni sunucular yüklemek, yapılandırmak ve korumak zorunda kalmadan mevcut kimlik doğrulama akışınıza telefon görüşmesi, kısa mesaj veya telefon uygulaması doğrulaması ekleyebilirsiniz. 

Bu uzantı, Azure MFA Sunucusu'nu dağıtmadan VPN bağlantılarını korumak isteyen kuruluşlar için oluşturulmuştur. NPS uzantısı, federal veya senkronize kullanıcılar için ikinci bir kimlik doğrulama faktörü sağlamak için RADIUS ve bulut tabanlı Azure MFA arasında bir bağdaştırıcı görevi görür.

Azure MFA için NPS uzantısını kullanırken, kimlik doğrulama akışı aşağıdaki bileşenleri içerir: 

1. **NAS/VPN Server** VPN istemcilerinden istekalır ve bunları RADIUS isteklerine dönüştürür. 
2. **NPS Server,** RADIUS istekleri için birincil kimlik doğrulamasını gerçekleştirmek için Active Directory'ye bağlanır ve başarılı olunda isteği yüklü tüm uzantılara geçirir.  
3. **NPS Uzantı,** ikincil kimlik doğrulaması için Azure MFA'ya bir istek tetikler. Uzantı yanıtı aldıktan ve MFA mücadelesi başarılı olursa, NPS sunucusuna Azure STS tarafından verilen Bir MFA talebi içeren güvenlik belirteçleri sağlayarak kimlik doğrulama isteğini tamamlar.  
4. **Azure MFA,** kullanıcının ayrıntılarını almak için Azure Active Directory ile iletişim kurar ve kullanıcıya yapılandırılan bir doğrulama yöntemini kullanarak ikincil kimlik doğrulamasını gerçekleştirir.

Aşağıdaki diyagram, bu üst düzey kimlik doğrulama isteği akışını göstermektedir: 

![Kimlik doğrulama akış diyagramı](./media/howto-mfa-nps-extension/auth-flow.png)

## <a name="plan-your-deployment"></a>Dağıtımınızı planlama

NPS uzantısı artıklığı otomatik olarak işler, böylece özel bir yapılandırmaya ihtiyacınız olmaz.

İhtiyacınız olduğu kadar Azure MFA özellikli NPS sunucusu oluşturabilirsiniz. Birden çok sunucu yüklerseniz, her biri için bir fark istemci sertifikası kullanmanız gerekir. Her sunucu için bir sertifika oluşturmak, her sertifikayı ayrı ayrı güncelleştirebileceğiniz ve tüm sunucularınızdaki kapalı kalma süresi konusunda endişelenmeyebileceğiniz anlamına gelir.

VPN sunucuları kimlik doğrulama isteklerini yönlendirir, bu nedenle yeni Azure MFA özellikli NPS sunucularından haberdar olmaları gerekir.

## <a name="prerequisites"></a>Ön koşullar

NPS uzantısı, varolan altyapınızla çalışmak içindir. Başlamadan önce aşağıdaki ön koşullara sahip olduğunuzdan emin olun.

### <a name="licenses"></a>Lisanslar

Azure MFA için NPS Uzantısı, [Azure Çok Faktörlü Kimlik Doğrulaması lisansına](multi-factor-authentication.md) sahip müşteriler tarafından kullanılabilir (Azure AD Premium, EMS veya MFA tek başına lisansile birlikte verilir). Kullanıcı başına veya kimlik doğrulama lisansları başına gibi Azure MFA'nın tüketim tabanlı lisansları NPS uzantısı ile uyumlu değildir. 

### <a name="software"></a>Yazılım

Windows Server 2008 R2 SP1 veya üzeri.

### <a name="libraries"></a>Kitaplıklar

Bu kitaplıklar uzantısı ile otomatik olarak yüklenir.

- [Visual Studio 2013 Için Visual C++ Yeniden Dağıtılabilir Paketler (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
- [Windows PowerShell sürüm 1.1.166.0 için Microsoft Azure Active Directory Modülü](https://www.powershellgallery.com/packages/MSOnline/1.1.166.0)

Windows PowerShell için Microsoft Azure Active Directory Modülü, kurulum işleminin bir parçası olarak çalıştırdığınız bir yapılandırma komut dosyası aracılığıyla zaten mevcut değilse yüklenir. Zaten yüklü değilse, bu modülü önceden yüklemenize gerek yoktur.

### <a name="azure-active-directory"></a>Azure Active Directory

NPS uzantısını kullanan herkesin Azure AD Connect kullanılarak Azure Etkin Dizin'e eşitlenmeleri ve MFA'ya kaydolması gerekir.

Uzantıyı yüklediğinizde, Azure AD kiracınız için dizin kimliğine ve yönetici kimlik bilgilerine ihtiyacınız var. Dizin kimliğinizi [Azure portalında](https://portal.azure.com)bulabilirsiniz. Yönetici olarak oturum açın. **Azure Etkin Dizini'ni**arayın ve seçin, ardından **Özellikler'i**seçin. GUID'i **Dizin Kimliği** kutusunda kopyalayın ve kaydedin. NPS uzantısını yüklerken bu GUID'yi kiracı kimliği olarak kullanırsınız.

![Azure Etkin Dizin özellikleri altında Dizin Kimliğinizi bulun](./media/howto-mfa-nps-extension/properties-directory-id.png)

### <a name="network-requirements"></a>Ağ gereksinimleri

NPS sunucusunun 80 ve 443 bağlantı noktaları üzerinden aşağıdaki URL'lerle iletişim kurabilmesi gerekir.

- https:\//adnotifications.windowsazure.com
- https:\//login.microsoftonline.com
- https:\//credentials.azure.com

Ayrıca, [sağlanan PowerShell komut dosyasını kullanarak bağdaştırıcının kurulumünü](#run-the-powershell-script) tamamlamak için aşağıdaki URL'lere bağlantı gereklidir

- https:\//login.microsoftonline.com
- https:\//provisioningapi.microsoftonline.com
- https:\//aadcdn.msauth.net

## <a name="prepare-your-environment"></a>Ortamınızı hazırlama

NPS uzantısını yüklemeden önce, kimlik doğrulama trafiğini işlemek için ortamınızı hazırlamak istersiniz.

### <a name="enable-the-nps-role-on-a-domain-joined-server"></a>Etki alanı birleştirilmiş sunucuda NPS rolünü etkinleştirme

NPS sunucusu Azure Etkin Dizini'ne bağlanır ve MFA isteklerini doğrular. Bu rol için bir sunucu seçin. NPS uzantısı RADIUS olmayan tüm istekler için hata lar attığından, diğer hizmetlerden gelen istekleri işlemeyen bir sunucu seçmenizi öneririz. NPS sunucusu, ortamınız için birincil ve ikincil kimlik doğrulama sunucusu olarak ayarlanmalıdır; RADIUS isteklerini başka bir sunucuya ekleyemez.

1. Sunucunuzda, Sunucu Yöneticisi Quickstart menüsünden **Rolleri ve Özellikleri Ekle Sihirbazı'nı** açın.
2. Yükleme türünüz için **Rol tabanlı veya özellik tabanlı yüklemeyi** seçin.
3. Ağ **İlkesi ve Erişim Hizmetleri** sunucu rolünü seçin. Bu rolü çalıştırmak için gerekli özellikler hakkında sizi bilgilendirmek için bir pencere açılabilir.
4. Onay sayfasına kadar sihirbaz aracılığıyla devam edin. **Yükle**’yi seçin.

Artık NPS için atanmış bir sunucunuz olduğuna göre, vpn çözümünden gelen RADIUS isteklerini işlemek için bu sunucuyu da yapılandırmanız gerekir.

### <a name="configure-your-vpn-solution-to-communicate-with-the-nps-server"></a>VPN çözümünüzü NPS sunucusuyla iletişim kurmak için yapılandırın

Kullandığınız VPN çözümüne bağlı olarak, RADIUS kimlik doğrulama ilkenizi yapılandırma adımları değişir. Bu ilkeyi RADIUS NPS sunucunuza işaret etmek için yapılandırın.

### <a name="sync-domain-users-to-the-cloud"></a>Etki alanı kullanıcılarını bulutla senkronize etme

Bu adım kiracınızda zaten tamamlanmış olabilir, ancak Azure AD Connect'in veritabanlarınızı son zamanlarda senkronize ettiğini iki kez kontrol etmek gerekir.

1. [Azure Portal](https://portal.azure.com)’da yönetici olarak oturum açın.
2. **Azure Etkin Dizini** > Azure**AD Bağlantısı'nı** seçin
3. Eşitleme durumunun **Etkin** olduğunu ve son eşitlemenizin bir saatten kısa bir süre önce olduğunu doğrulayın.

Yeni bir eşitleme turu başlatmanız gerekiyorsa, Azure AD Connect eşitlemesinde bize verilen [yönergeler: Zamanlayıcı.](../hybrid/how-to-connect-sync-feature-scheduler.md#start-the-scheduler)

### <a name="determine-which-authentication-methods-your-users-can-use"></a>Kullanıcılarınızın kullanabileceği kimlik doğrulama yöntemlerini belirleme

NPS uzantılı dağıtımda hangi kimlik doğrulama yöntemlerinin kullanılabileceğini etkileyen iki etken vardır:

1. RADIUS istemcisi (VPN, Netscaler sunucusu veya diğer) ve NPS sunucuları arasında kullanılan parola şifreleme algoritması.
   - **PAP,** bulutta Azure MFA'nın tüm kimlik doğrulama yöntemlerini destekler: telefon görüşmesi, tek yönlü kısa mesaj, mobil uygulama bildirimi, OATH donanım belirteçleri ve mobil uygulama doğrulama kodu.
   - **CHAPV2** ve **EAP** destek telefon görüşmesi ve mobil uygulama bildirimi.

      > [!NOTE]
      > NPS uzantısını dağıttığınızda, kullanıcılarınız için hangi yöntemlerin kullanılabilen yöntemleri değerlendirmek için bu etkenleri kullanın. RADIUS istemciniz PAP'ı destekliyorsa, ancak istemci UX'nin doğrulama kodu için giriş alanları yoksa, telefon görüşmesi ve mobil uygulama bildirimi desteklenen iki seçenektir.
      >
      > Buna ek olarak, VPN istemciniz UX giriş alanını destekliyorsa ve Ağ Erişim İlkesi'ni yapılandırmışsanız , kimlik doğrulama başarılı olabilir, ancak Ağ Politikası'nda yapılandırılan RADIUS özniteliklerinin hiçbiri Ne RRAS sunucusu ne de VPN istemcisi gibi Ağ Erişim Aygıtı'na uygulanmaz. Sonuç olarak, VPN istemcisi istenilenden daha fazla erişime sahip olabilir veya daha az erişim emeyebilir.
      >

2. İstemci uygulamasının (VPN, Netscaler sunucusu veya diğer) işleyebilir giriş yöntemleri. Örneğin, VPN istemcisinin kullanıcının bir metin veya mobil uygulamadan doğrulama kodu yazabilmesine izin vermek için bazı araçları var mı?

[Azure'da desteklenmeyen kimlik doğrulama yöntemlerini devre dışı kullanabilirsiniz.](howto-mfa-mfasettings.md#verification-methods)

### <a name="register-users-for-mfa"></a>Kullanıcıları MFA'ya kaydedin

NPS uzantısını dağıtmadan ve kullanmadan önce, iki aşamalı doğrulama gerçekleştirmesi gereken kullanıcıların MFA'ya kaydolması gerekir. Daha hemen, uzantıyı dağıtırken test etmek için, Multi-Factor Authentication için tam olarak kayıtlı en az bir test hesabına ihtiyacınız vardır.

Test hesabının başlatılmasını sağlamak için şu adımları kullanın:

1. Bir test [https://aka.ms/mfasetup](https://aka.ms/mfasetup) hesabı ile oturum açın.
2. Doğrulama yöntemi ayarlamak için istemleri izleyin.
3. Test hesabı için çok faktörlü kimlik doğrulaması gerektirecek [koşullu erişim ilkesi oluşturun.](howto-mfa-getstarted.md#create-conditional-access-policy)

> [!IMPORTANT]
>
> Kullanıcıların Azure Çok Faktörlü Kimlik Doğrulama'ya başarıyla kaydolduğundan emin olun. Kullanıcılar daha önce yalnızca self servis parola sıfırlama (SSPR) için kaydolmuşsa, *StrongAuthenticationMethods* hesapları için etkinleştirilir. Azure Çok Faktörlü Kimlik Doğrulama, kullanıcı yalnızca SSPR'ye kaydolsa bile *Strong AuthenticationMethods* yapılandırıldıklarında uygulanır.
>
> SSPR ve Azure Çok Faktörlü Kimlik Doğrulaması'nı aynı anda yapılandıran birleşik güvenlik kaydı etkinleştirilebilir. Daha fazla bilgi için azure [etkin dizininde birleşik güvenlik bilgi kaydını etkinleştir'e](howto-registration-mfa-sspr-combined.md)bakın.
>
> Ayrıca, kullanıcıları daha önce yalnızca SSPR'yi etkinleştirdilerse [kimlik doğrulama yöntemlerini yeniden kaydetmeye de](howto-mfa-userdevicesettings.md#manage-user-authentication-options) zorlayabilirsiniz.

## <a name="install-the-nps-extension"></a>NPS uzantısını yükleme

> [!IMPORTANT]
> NPS uzantısını VPN erişim noktasından farklı bir sunucuya yükleyin.

### <a name="download-and-install-the-nps-extension-for-azure-mfa"></a>Azure MFA için NPS uzantısını indirin ve yükleyin

1. [NPS Uzantısını](https://aka.ms/npsmfa) Microsoft İndirme Merkezi'nden indirin.
2. İkiliyi yapılandırmak istediğiniz Ağ İlkesi Sunucusu'na kopyalayın.
3. *setup.exe'yi* çalıştırın ve kurulum yönergelerini izleyin. Hatalarla karşılaşırsanız, ön koşul bölümündeki iki kitaplıkbaşarıyla yüklendiğinden iki kez kontrol edin.

#### <a name="upgrade-the-nps-extension"></a>NPS uzantısını yükseltme

Varolan bir NPS uzantısı yüklemesini yükseltirken, temel sunucunun yeniden başlatılmasını önlemek için aşağıdaki adımları tamamlayın:

1. Varolan sürümü kaldırma
1. Yeni yükleyiciyi çalıştırın
1. Ağ Politikası Sunucusu (IAS) hizmetini yeniden başlatın

### <a name="run-the-powershell-script"></a>PowerShell betiğini çalıştırma

Yükleyici bu konumda bir PowerShell komut `C:\Program Files\Microsoft\AzureMfa\Config` dosyası oluşturur: (C:\ yükleme sürücünüzdür). Bu PowerShell komut dosyası her çalıştırılında aşağıdaki eylemleri gerçekleştirir:

- Kendi imzalı bir sertifika oluşturun.
- Sertifikanın ortak anahtarını Azure AD'deki hizmet sorumlusuyla ilişkilendirin.
- Sertifikayı yerel makine sertifika deposunda saklayın.
- Sertifikanın özel anahtarına Network User'a erişim izni ver.
- NPS'yi yeniden başlatın.

Kendi sertifikalarınızı kullanmak istemiyorsanız (PowerShell komut dosyasının oluşturduğu kendi imzalı sertifikalar yerine), yüklemeyi tamamlamak için PowerShell Script'i çalıştırın. Uzantıyı birden çok sunucuya yüklerseniz, her birinin kendi sertifikası olmalıdır.

1. Windows PowerShell'i yönetici olarak çalıştırın.
2. Dizinleri değiştirin.

   `cd "C:\Program Files\Microsoft\AzureMfa\Config"`

3. Yükleyici tarafından oluşturulan PowerShell komut dosyasını çalıştırın.

   `.\AzureMfaNpsExtnConfigSetup.ps1`

4. Azure AD'de yönetici olarak oturum açın.
5. PowerShell, kiracı kimliğiniz için istemde kalır. Önkoşullar bölümünde Azure portalından kopyaladığınız Dizin Kimliği GUID'i kullanın.
6. PowerShell komut dosyası bittiğinde bir başarı iletisi gösterir.  

Yük dengeleme için ayarlamak istediğiniz ek NPS sunucularında bu adımları yineleyin.

Önceki bilgisayar sertifikanızın süresi dolduysa ve yeni bir sertifika oluşturulduysa, süresi dolmuş sertifikaları silmeniz gerekir. Süresi dolmuş sertifikalar, NPS Uzantısı'nın başlatılmasıyla ilgili sorunlara neden olabilir.

> [!NOTE]
> PowerShell komut dosyasıyla sertifika oluşturmak yerine kendi sertifikalarınızı kullanıyorsanız, bunların NPS adlandırma kuralına uygun olduğundan emin olun. Özne adı **CN=\<TenantID\>,OU=Microsoft NPS Uzantılı**olmalıdır. 

### <a name="microsoft-azure-government-additional-steps"></a>Microsoft Azure Kamu ek adımları

Azure Genel Bulutu kullanan müşteriler için her NPS sunucusunda aşağıdaki ek yapılandırma adımları gereklidir:

1. NPS sunucusunda Kayıt Defteri Düzenleyicisi'ni açın. **Registry Editor**
1. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa` sayfasına gidin. Aşağıdaki temel değerleri ayarlayın:

    | Kayıt defteri anahtarı       | Değer |
    |--------------------|-----------------------------------|
    | AZURE_MFA_HOSTNAME | adnotifications.windowsazure.us   |
    | STS_URL            | https://login.microsoftonline.us/ |

1. Her NPS sunucusu için kayıt defteri anahtar değerlerini ayarlamak için önceki iki adımı yineleyin.
1. Her NPS sunucusu için NPS hizmetini yeniden başlatın.

    Minimum etki için, her NPS sunucusunu Birer birer NLB dönüşünden çıkarın ve tüm bağlantıların boşalmasını bekleyin.

### <a name="certificate-rollover"></a>Sertifika devri

NPS uzantısı 1.0.1.32 sürümü ile, birden fazla sertifika okuma şimdi desteklenir. Bu özellik, son kullanma tarihinden önce sertifika güncelleştirmelerini kaydetmeyi kolaylaştıracaktır. Kuruluşunuz NPS uzantısının önceki bir sürümünü çalıştırıyorsa, sürüm 1.0.1.32 veya daha yüksek yükseltmeniz gerekir.

`AzureMfaNpsExtnConfigSetup.ps1` Komut dosyası tarafından oluşturulan sertifikalar 2 yıl süreyle geçerlidir. BT kuruluşları, sertifikaların sona erme tarihini izlemelidir. NPS uzantısına ait sertifikalar Kişisel sertifika deposuna yerleştirilir ve komut dosyasına verilen kiracı kimliğine verilir.

Bir sertifika son kullanma tarihine yaklaşırken, sertifikanın yerine yeni bir sertifika oluşturulmalıdır.  Bu `AzureMfaNpsExtnConfigSetup.ps1` işlem, yeniden çalıştırılarak ve istendiğinde aynı kiracı kimliğini tutarak gerçekleştirilir. Bu işlem ortamınızdaki her NPS sunucusunda yinelenmelidir.

## <a name="configure-your-nps-extension"></a>NPS uzantınızı yapılandırın

Bu bölümde, başarılı NPS uzantılı dağıtımlar için tasarım konuları ve öneriler yer almaktadır.

### <a name="configuration-limitations"></a>Yapılandırma sınırlamaları

- Azure MFA için NPS uzantısı, kullanıcıları ve ayarları MFA Server'dan buluta geçirmek için araçlar içermez. Bu nedenle, uzantıyı varolan dağıtım yerine yeni dağıtımlar için kullanmanızı öneririz. Uzantıyı varolan bir dağıtımda kullanırsanız, kullanıcılarınızın MFA ayrıntılarını bulutta doldurmak için yeniden prova yapması gerekir.  
- NPS uzantısı, Ikincil Auth'u gerçekleştirmek için Azure MFA'daki kullanıcıyı tanımlamak için şirket içi Etkin dizinindeki UPN'yi kullanır. Uzantı, upn dışında alternatif giriş kimliği veya özel Active Directory alanı gibi farklı bir tanımlayıcı kullanacak şekilde yapılandırılabilir. Daha fazla bilgi için, makaleye bakın, [Multi-Factor Authentication için NPS uzantısı için gelişmiş yapılandırma seçenekleri.](howto-mfa-nps-extension-advanced.md)
- Tüm şifreleme protokolleri tüm doğrulama yöntemlerini desteklemez.
   - **PAP** telefon görüşmesini, tek yönlü kısa mesajı, mobil uygulama bildirimini ve mobil uygulama doğrulama kodunu destekler
   - **CHAPV2** ve **EAP** destek telefon görüşmesi ve mobil uygulama bildirimi

### <a name="control-radius-clients-that-require-mfa"></a>MFA gerektiren RADIUS istemcilerini kontrol edin

NPS Uzantısı'nı kullanarak bir RADIUS istemcisi için MFA'yı etkinleştirdikten sonra, bu istemciiçin tüm kimlik doğrulamalarının MFA gerçekleştirmesi gerekir. Bazı RADIUS istemcileri için MFA'yı etkinleştirmek, ancak diğerleri için değil, iki NPS sunucusunu yapılandırabilir ve uzantıyı bunlardan yalnızca birine yükleyebilirsiniz. MFA'nın uzantıyla yapılandırılan NPS sunucusuna istek göndermesini, uzantıyla yapılandırılmayan NPS sunucusuna diğer RADIUS istemcilerini göndermesini istediğiniz RADIUS istemcilerini yapılandırın.

### <a name="prepare-for-users-that-arent-enrolled-for-mfa"></a>MFA'ya kaydolmamış kullanıcılar için hazırlanın

MFA'ya kayıtlı olmayan kullanıcılarınız varsa, kimlik doğrulaması yapmaya çalıştıklarında ne olacağını belirleyebilirsiniz. Özellik davranışını denetlemek için kayıt defteri *ayarı* REQUIRE_USER_MATCH *HKLM\Software\Microsoft\AzureMFA'daki* kayıt defteri ayarını kullanın. Bu ayarın tek bir yapılandırma seçeneği vardır:

| Anahtar | Değer | Varsayılan |
| --- | ----- | ------- |
| REQUIRE_USER_MATCH | DOĞRU/YANLıŞ | Ayarlanmıyor (TRUE'ya eşdeğer) |

Bu ayarın amacı, bir kullanıcı MFA'ya kaydolmadığında ne yapacağını belirlemektir. Anahtar yoksa, ayarlanmaz veya TRUE olarak ayarlanmışsa ve kullanıcı kaydedilmezse, uzantı MFA mücadelesinde başarısız olur. Anahtar FALSE olarak ayarlandığında ve kullanıcı kaydedilmediğinde, kimlik doğrulama MFA gerçekleştirmeden devam eder. Bir kullanıcı MFA'ya kayıtlıysa, REQUIRE_USER_MATCH FALSE olarak ayarlansa bile MFA ile kimlik doğrulaması gerekir.

Bu anahtarı oluşturmayı ve kullanıcılarınız binerken FALSE olarak ayarlayabilirsiniz ve tüm bunlar henüz Azure MFA'ya kaydolmayabilir. Ancak, anahtarı ayarlamak MFA'ya kaydolmamış kullanıcıların oturum açmasına izin verdiğinden, üretime gitmeden önce bu anahtarı kaldırmanız gerekir.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="nps-extension-health-check-script"></a>NPS uzantısı sistem durumu denetimi komut dosyası

NPS uzantısı sorun giderme temel sistem durumu denetimi adımları gerçekleştirmek için aşağıdaki komut dosyası kullanılabilir.

[MFA_NPS_Troubleshooter.ps1](https://docs.microsoft.com/samples/azure-samples/azure-mfa-nps-extension-health-check/azure-mfa-nps-extension-health-check/)

---

### <a name="how-do-i-verify-that-the-client-cert-is-installed-as-expected"></a>İstemci sertifikasının beklendiği gibi yüklenmiş olduğunu nasıl doğrulayabilirim?

Sertifika deposunda yükleyici tarafından oluşturulan kendi imzalı sertifikayı arayın ve özel anahtarın kullanıcı **NETWORK SERVICE'e**verilen izinlere sahip olup olmadığını kontrol edin. **Sertifikac, CN \<tenantid\>, OU = Microsoft NPS Uzantı** bir konu adı vardır

*AzureMfaNpsExtnConfigSetup.ps1* komut dosyası tarafından oluşturulan kendi imzalı sertifikaların da geçerlilik süresi iki yıldır. Sertifikanın yüklü olduğunu doğrularken, sertifikanın süresinin dolup olmadığını da denetlemeniz gerekir.

---

### <a name="how-can-i-verify-that-my-client-cert-is-associated-to-my-tenant-in-azure-active-directory"></a>Müşteri sertifikamın Azure Etkin Dizini'ndeki kiracımla ilişkili olduğunu nasıl doğrulayabilirim?

PowerShell komutunu açın ve aşağıdaki komutları çalıştırın:

``` PowerShell
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1
```

Bu komutlar, powershell oturumunuzdaki NPS uzantıörneğinizle kiracınızı ilişkilendiren tüm sertifikaları yazdırır. Özel anahtar olmadan "Base-64 kodlu X.509(.cer)" dosyası olarak müşteri sertifikanızı dışa aktararak sertifikanızı arayın ve PowerShell'in listesiyle karşılaştırın.

Aşağıdaki komut ,.cer biçiminde "C:" sürücünüzde "npscertificate" adlı bir dosya oluşturur.

``` PowerShell
import-module MSOnline
Connect-MsolService
Get-MsolServicePrincipalCredential -AppPrincipalId "981f26a1-7f43-403b-a875-f8b09b8cd720" -ReturnKeyValues 1 | select -ExpandProperty "value" | out-file c:\npscertificate.cer
```

Bu komutu çalıştırdıktan sonra, C sürücünüze gidin, dosyayı bulun ve üzerine çift tıklayın. Ayrıntılara gidin ve "parmak izi" olarak aşağı kaydırın, sunucuda yüklü olan sertifikanın parmak izini bununla karşılaştırın. Sertifika parmak izleri eşleşmelidir.

Geçerli ve Geçerli-Zaman damgaları, insan tarafından okunabilir şeklinde, komut birden fazla cert döndürür bariz uyumsuzluklar filtrelemek için kullanılabilir.

---

### <a name="why-cannot-i-sign-in"></a>Neden oturum açamıyorum?

Parolanızın süresinin dolup dolmadığından kontrol edin. NPS Uzantı, oturum açma iş akışının bir parçası olarak parolaların değiştirilmesini desteklemez. Daha fazla yardım için kuruluşunuzun BT Personeliile iletişime geçin.

---

### <a name="why-are-my-requests-failing-with-adal-token-error"></a>İsteklerim Neden ADAL belirteç hatasıyla başarısız oluyor?

Bu hata çeşitli nedenlerden biri nedeniyle olabilir. Sorun gidermeye yardımcı olmak için şu adımları kullanın:

1. NPS sunucunuzu yeniden başlatın.
2. İstemci sertifikasının beklendiği gibi yüklü olduğunu doğrulayın.
3. Azure AD'de sertifikanın kiracınızla ilişkili olduğunu doğrulayın.
4. Uzantıyı çalıştıran sunucudan `https://login.microsoftonline.com/` adresine erişilebildiğini doğrulayın.

---

### <a name="why-does-authentication-fail-with-an-error-in-http-logs-stating-that-the-user-is-not-found"></a>Kimlik doğrulama, kullanıcının bulunmadığını belirten HTTP günlüklerinde bir hatayla neden başarısız oluyor?

AD Connect'in çalıştığını ve kullanıcının hem Windows Etkin Dizini hem de Azure Etkin Dizini'nde bulunduğunu doğrulayın.

---

### <a name="why-do-i-see-http-connect-errors-in-logs-with-all-my-authentications-failing"></a>Neden tüm kimlik doğrulamalarım başarısız olan günlüklerde HTTP bağlantı hataları görüyorum?

NPS uzantısını çalıştıran sunucudan https://adnotifications.windowsazure.com adresine ulaşılabildiğini doğrulayın.

---

### <a name="why-is-authentication-not-working-despite-a-valid-certificate-being-present"></a>Geçerli bir sertifika olmasına rağmen kimlik doğrulama neden çalışmıyor?

Önceki bilgisayar sertifikanızın süresi dolduysa ve yeni bir sertifika oluşturulduysa, süresi dolmuş sertifikaları silmeniz gerekir. Süresi dolmuş sertifikalar, NPS Uzantısı'nın başlatılmasıyla ilgili sorunlara neden olabilir.

Geçerli bir sertifikanız olup olmadığını kontrol etmek için, MMC'yi kullanarak yerel Bilgisayar Hesabının Sertifika Deposu'nu kontrol edin ve sertifikanın son kullanma tarihini geçmediğinden emin olun. Yeni geçerli bir sertifika oluşturmak için "[PowerShell komut dosyasını çalıştır](#run-the-powershell-script)" bölümünün altındaki adımları yeniden

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>TLS/SSL Protokollerini ve Şifre Paketlerini yönetme

Kuruluşunuz gerektirmedikçe, daha eski ve daha zayıf şifreleme paketlerinin devre dışı bırakılmaları veya kaldırılması önerilir. Bu görevin nasıl gerçekleştirileceği hakkında bilgiler [AD FS için SSL/TLS Protokollerini ve Şifre Paketlerini Yönetme](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-protocols-in-ad-fs) makalesine bakın

### <a name="additional-troubleshooting"></a>Ek sorun giderme

Azure Çok Faktörlü Kimlik Doğrulama için [NPS uzantısından gelen hata iletilerini çözümle](howto-mfa-nps-extension-errors.md)makalesinde ek sorun giderme kılavuzu ve olası çözümler bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Windows Server'da Ağ Politikası Sunucusu'na genel bakış ve yapılandırma](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top)

- Oturum açma için alternatif kimlikleri yapılandırma veya [Çok Faktörlü Kimlik Doğrulama için NPS uzantısı için Gelişmiş yapılandırma seçeneklerinde](howto-mfa-nps-extension-advanced.md) iki aşamalı doğrulama gerçekleştirmemesi gereken IP'ler için bir özel durum listesi ayarlama

- NPS uzantısını kullanarak [Uzak Masaüstü Ağ Geçidi](howto-mfa-nps-extension-rdg.md) ve VPN [sunucularını](howto-mfa-nps-extension-vpn.md) nasıl entegre edebilirsiniz öğrenin

- [Azure Multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme](howto-mfa-nps-extension-errors.md)
