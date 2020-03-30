---
title: Azure Active Directory cihaz yönetimi SSS | Microsoft Dokümanlar
description: Azure Active Directory aygıt yönetimi SSS.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: troubleshooting
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: c238600d412e53ad665214492e292aa395655b78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79497525"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory cihaz yönetimi SSS

## <a name="general-faq"></a>Genel SSS

### <a name="q-i-registered-the-device-recently-why-cant-i-see-the-device-under-my-user-info-in-the-azure-portal-or-why-is-the-device-owner-marked-as-na-for-hybrid-azure-active-directory-azure-ad-joined-devices"></a>S: Cihazı yakın zamanda kaydettirdim. Azure portalında kullanıcı bilgilerimin altındaki cihazı neden göremiyorum? Veya aygıt sahibi neden karma Azure Etkin Dizin (Azure AD) için N/A olarak işaretlenmiştir?

**A:** Karma Azure AD'ye katılan Windows 10 **aygıtları KULLANICI aygıtları**altında görünmüyor.
Azure portalındaki **Tüm aygıtlar** görünümünü kullanın. PowerShell [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet de kullanabilirsiniz.

**User cihazları**altında yalnızca aşağıdaki cihazlar listelenir:

- Karma Azure AD olmayan tüm kişisel aygıtlar katıldı. 
- Windows 10 veya Windows Server 2016 olmayan tüm aygıtlar.
- Tüm Windows dışı aygıtlar. 

---

### <a name="q-how-do-i-know-what-the-device-registration-state-of-the-client-is"></a>S: İstemcinin cihaz kayıt durumunu nasıl bilebilirim?

**A:** Azure portalında Tüm **aygıtlara**gidin. Aygıt kimliğini kullanarak aygıtı arayın. Birleştirme türü sütunun altındaki değeri denetleyin. Bazen aygıt sıfırlanabilir veya yeniden görüntülenebilir. Bu nedenle cihazdaki cihaz kayıt durumunu da kontrol etmek gerekir:

- Windows 10 ve Windows Server 2016 `dsregcmd.exe /status`veya sonraki aygıtlar için çalıştırın.
- Alt düzey işletim sistemi `%programFiles%\Microsoft Workplace Join\autoworkplace.exe`sürümleri için çalıştırın.

**A:** Sorun giderme bilgileri için şu makalelere bakın:
- [dsregcmd komutunu kullanan sorun giderme cihazları](troubleshoot-device-dsregcmd.md)
- [Sorun giderme hibrit Azure Active Directory, Windows 10 ve Windows Server 2016 aygıtlarına katıldı](troubleshoot-hybrid-join-windows-current.md)
- [Karma Azure Etkin Dizini'nde sorun giderme, alt düzey aygıtları birleştirdi](troubleshoot-hybrid-join-windows-legacy.md)

---

### <a name="q-i-see-the-device-record-under-the-user-info-in-the-azure-portal-and-i-see-the-state-as-registered-on-the-device-am-i-set-up-correctly-to-use-conditional-access"></a>S: Azure portalındaki KULLANICI bilgisinin altında cihaz kaydını görüyorum. Ve durumu cihazda kayıtlı olarak görüyorum. Koşullu Erişim'i kullanmak için doğru şekilde ayarlandı mıyım?

**A:** **DeviceID**tarafından gösterilen aygıt birleştirme durumu, Azure AD'deki durumla eşleşmeli ve Koşullu Erişim için tüm değerlendirme ölçütlerini karşılamalıdır. Daha fazla bilgi için bkz: [Koşullu Erişim ile bulut uygulamasıerişimi için yönetilen aygıtları iste.](../conditional-access/require-managed-devices.md)

---

### <a name="q-why-do-my-users-see-an-error-message-saying-your-organization-has-deleted-the-device-or-your-organization-has-disabled-the-device-on-their-windows-10-devices"></a>S: Kullanıcılarım windows 10 cihazlarında neden "Kuruluşunuz aygıtı sildi" veya "Kuruluşunuz aygıtı devre dışı bıraktı" yazan bir hata iletisi görüyor?

**A:** Azure AD'ye katılan veya Azure AD'ye kayıtlı Olan Windows 10 aygıtlarında, kullanıcılara tek oturum açmaolanağı sağlayan birincil [yenileme belirteci (PRT)](concept-primary-refresh-token.md) verilir. PRT'nin geçerliliği aygıtın geçerliliğine bağlıdır. Kullanıcılar, aygıtın kendisinden eylem başlatmadan Azure AD'de silinmiş veya devre dışı bırakılmışsa bu iletiyi görür. Bir aygıt Azure AD'de silinebilir veya devre dışı bırakılmış olabilir aşağıdaki senaryolardan biri: 

- Kullanıcı cihazı Uygulamalarım portalından devre dışı kılabilir. 
- Bir yönetici (veya kullanıcı), Azure portalındaki veya PowerShell'i kullanarak aygıtı siler veya devre dışı eder
- Yalnızca Karma Azure AD katıldı: Bir yönetici, ou aygıtlarını eşitleme kapsamı dışında kaldırır ve bu da aygıtların Azure AD'den silinmesini sağlar
- Azure AD'yi yükseltme sürümüne 1.4.xx.x bağlayın. [Azure AD Connect 1.4.xx.x'i ve aygıtın kaybolmasını anlama](/azure/active-directory/hybrid/reference-connect-device-disappearance).


Bu eylemlerin nasıl düzeltilebileceği ne kadar aşağıya bakın.

---

### <a name="q-i-disabled-or-deleted-my-device-in-the-azure-portal-or-by-using-windows-powershell-but-the-local-state-on-the-device-says-its-still-registered-what-should-i-do"></a>S: Azure portalında veya Windows PowerShell kullanarak aygıtımı devre dışı bıraktım veya sildim. Ama cihazdaki yerel eyalet hala kayıtlı olduğunu söylüyor. Ne yapmalıyım?

**A:** Bu işlem tasarım gereğidir. Bu durumda, aygıtın buluttaki kaynaklara erişimi yoktur. Yöneticiler, yetkisiz erişimi önlemek için bu eylemi eski, kayıp veya çalıntı aygıtlar için gerçekleştirebilir. Bu eylem istemeden gerçekleştirildiyse, aygıtı aşağıda açıklandığı şekilde yeniden etkinleştirmeniz veya yeniden kaydetmeniz gerekir

- Aygıt Azure AD'de devre dışı bırakılmışsa, yeterli ayrıcalığa sahip bir yönetici bunu Azure AD portalından etkinleştirebilir  
  > [!NOTE]
  > Aygıtları Azure AD Connect'i kullanarak eşitlüyorsanız, karma Azure AD birleştirilmiş aygıtlar bir sonraki eşitleme döngüsü nde otomatik olarak yeniden etkinleştirilir. Bu nedenle, karma azure AD'ye katılan bir aygıtı devre dışı bilebilirseniz, aygıtı şirket içi AD'nizden devre dışı bmalısınız

 - Aygıt Azure AD'de silinirse, aygıtı yeniden kaydetmeniz gerekir. Yeniden kaydolmak için cihazüzerinde el ile işlem yapmak gerekir. Cihaz durumuna göre yeniden kayıt talimatları için aşağıya bakın. 

      Windows 10 ve Windows Server 2016/2019 aygıtlarına katılan karma Azure AD'yi yeniden kaydetmek için aşağıdaki adımları izleyin:

      1. Yönetici olarak komut istemini açın.
      1. `dsregcmd.exe /debug /leave` yazın.
      1. Aygıtı Azure AD'ye yeniden kaydeden zamanlanan görevi tetiklemek için oturum açın ve oturum açın. 

      Karma Azure AD'si olan alt düzey Windows işletim sistemi sürümleri için aşağıdaki adımları izleyin:

      1. Yönetici olarak komut istemini açın.
      1. `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"` yazın.
      1. `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"` yazın.

      Azure AD'nin birleştiği aygıtlar için Windows 10 cihazları aşağıdaki adımları izleyin:

      1. Yönetici olarak komut istemini açma
      1. Girin `dsregcmd /forcerecovery` (Not: Bu eylemi gerçekleştirmek için yönetici olmanız gerekir).
      1. Açılan iletişim kutusunda "Oturum aç"ı tıklatın ve oturum açma işlemine devam edin.
      1. Kurtarmayı tamamlamak için oturum açın ve cihaza geri oturum açın.

      Azure AD kayıtlı Windows 10 aygıtları için aşağıdaki adımları izleyin:

      1. **Ayarlar** > **Hesapları** > **Erişim İş veya Okul'a**gidin. 
      1. Hesabı seçin ve **Bağlantıyı Kes'i**seçin.
      1. "+ Bağlan"a tıklayın ve oturum açma işleminden geçerek cihazı yeniden kaydedin.

---

### <a name="q-why-do-i-see-duplicate-device-entries-in-the-azure-portal"></a>S: Azure portalında neden yinelenen aygıt girişlerini görüyorum?

**A:**

- Windows 10 ve Windows Server 2016 için, aynı aygıta katılmak ve yeniden katılmak için tekrarlanan denemeler yinelenen girişlere neden olabilir. 
- **İş Ekle veya Okul Hesabı'nı** kullanan her Windows kullanıcısı, aynı aygıt adına sahip yeni bir aygıt kaydı oluşturur.
- Şirket içinde Azure Dizin etki alanının birleştiği alt düzey Windows Işletim Sistemi sürümlerinde, otomatik kayıt, aygıtta giriş yapan her etki alanı kullanıcısı için aynı aygıt adına sahip yeni bir aygıt kaydı oluşturur. 
- Aynı ada sahip olan bir Azure AD, silinmiş, yeniden yüklenen ve yeniden birleştirilmiş makineye, aynı aygıt adına sahip başka bir kayıt olarak gösterilmektedir.

---

### <a name="q-does-windows-10-device-registration-in-azure-ad-support-tpms-in-fips-mode"></a>S: Azure AD'deki Windows 10 aygıt kaydı FIPS modunda TPM'leri destekliyor mu?

**A:** Windows 10 aygıt kaydı yalnızca FIPS uyumlu TPM 2.0 için desteklenir ve TPM 1.2 için desteklenmez. Aygıtlarınızda FIPS uyumlu TPM 1.2 varsa, Azure AD join veya Karma Azure AD join ile devam etmeden önce aygıtları devre dışı bırakmalısınız. Microsoft, TPM üreticisine bağlı olduğu için TPM'ler için FIPS modunu devre dışı bırakmak için herhangi bir araç sağlamaz. Destek için donanım OEM'inizle iletişime geçin. 

---

**S: Bir kullanıcı neden Azure portalında devre dışı bıraktırdığım bir cihazdan kaynaklara erişebiliyor?**

**A:** Azure AD aygıtıdevre dışı olarak işaretlendiği andan itibaren iptalin uygulanması bir saat kadar sürer.

>[!NOTE] 
>Kayıtlı aygıtlar için, kullanıcıların kaynaklara erişemediğinden emin olmak için aygıtı silmenizi öneririz. Daha fazla bilgi için [bkz: Aygıt kaydı nedir?](/mem/intune/user-help/use-managed-devices-to-get-work-done) 

---

### <a name="q-why-are-there-devices-marked-as-pending-under-the-registered-column-in-the-azure-portal"></a>S: Azure portalındaki KAYITLI sütunun altında neden "Beklemede" olarak işaretlenmiş aygıtlar var?

**C**: Beklemede aygıtın kayıtlı olmadığını gösterir. Bu durum, bir aygıtın Azure AD bağlantısı kullanılarak senkronize edildiğini ve aygıt kaydı için hazır olduğunu gösterir. Bu aygıtlar JOIN TYPE kümesine sahiptir ve "Karma Azure AD katıldı". [Karma Azure Etkin Dizinuygulamanızı nasıl planlayabildiğiniz](hybrid-azuread-join-plan.md)hakkında daha fazla bilgi edinin.

>[!NOTE]
>Bir aygıt, kayıtlı bir duruma sahip olmaktan "Beklemede" olarak da değişebilir
>* Bir aygıt önce Azure AD'den silinirse ve şirket içi bir AD'den yeniden senkronize edilirse.
>* Bir aygıt Azure AD Connect'te eşitleme kapsamından kaldırılır ve geri eklenirse.
>
>Her iki durumda da aygıtı bu aygıtların her birine el ile yeniden kaydetmeniz gerekir. Aygıtın daha önce kaydedilip kaydedilemediğini gözden geçirmek [için dsregcmd komutunu kullanarak aygıtları sorun giderebilirsiniz.](troubleshoot-device-dsregcmd.md)

---
## <a name="azure-ad-join-faq"></a>Azure AD sSS'ye katıldı

### <a name="q-how-do-i-unjoin-an-azure-ad-joined-device-locally-on-the-device"></a>S: Azure AD'ye katılan bir aygıta nasıl katılabilirim?

**A:** Saf Azure AD birleştirilmiş aygıtlar için çevrimdışı yerel yönetici hesabınız olduğundan veya bir tane oluşturduğundan emin olun. Azure AD kullanıcı kimlik bilgileriyle oturum açamazsınız. Ardından, **Ayarlar** > **Hesapları** > **Erişim Çalışması'na veya Okula**gidin. Hesabınızı seçin ve **Bağlantıyı Kes'i**seçin. İstendiğinde istemleri izleyin ve yerel yönetici kimlik bilgilerini sağlayın. Birebir olmayan işlemi tamamlamak için aygıtı yeniden başlatın.

---

### <a name="q-can-my-users-sign-in-to-azure-ad-joined-devices-that-are-deleted-or-disabled-in-azure-ad"></a>S: Kullanıcılarımın Azure AD'de oturum açmaları Azure AD'de silinen veya devre dışı bırakılan aygıtlara katılabilir mi?

**Y:** Evet. Windows, daha önce oturum açmış olan kullanıcıların ağ bağlantısı olmadan bile masaüstüne hızlı bir şekilde erişmesine olanak tanıyan önbelleğe alınmış bir kullanıcı adı ve parola özelliğine sahiptir. 

Bir aygıt Azure AD'de silindiğinde veya devre dışı bırakıldığında, Windows aygıtı tarafından bilinmemektedir. Bu nedenle, daha önce oturum açmış olan kullanıcılar önbelleğe alınmış kullanıcı adı ve parolayla masaüstüne erişmeye devam edin. Ancak aygıt silindikçe veya devre dışı bırakıldıkça, kullanıcılar aygıt tabanlı Koşullu Erişim tarafından korunan kaynaklara erişemez. 

Daha önce oturum açmamış olan kullanıcılar aygıta erişemez. Önbelleğe alınmış kullanıcı adı ve parola onlar için etkin değil. 

---

### <a name="q-can-a-disabled-or-deleted-user-sign-in-to-an-azure-ad-joined-devices"></a>S: Engelli veya silinmiş bir kullanıcı Azure AD'ye katılan aygıtlarda oturum açabilir mi?

**A:** Evet, ama sadece sınırlı bir süre için. Bir kullanıcı Azure AD'de silindiğinde veya devre dışı bırakıldığında, bu kullanıcı Windows aygıtı tarafından hemen bilinmemektedir. Böylece daha önce oturum açmış olan kullanıcılar önbelleğe alınmış kullanıcı adı ve parolayla masaüstüne erişebilirler. 

Genellikle, aygıt kullanıcı durumunu dört saatten kısa bir süre içinde fark edin. Ardından Windows, bu kullanıcıların masaüstüne erişimini engeller. Kullanıcı Azure AD'de silindiğinden veya devre dışı bırakıldığından, tüm belirteçleri iptal edilir. Böylece hiçbir kaynağa erişemezler. 

Daha önce oturum açmayan silinmiş veya devre dışı bırakılmış kullanıcılar aygıta erişemez. Önbelleğe alınmış kullanıcı adı ve parola onlar için etkin değil. 

---

### <a name="q-why-do-my-users-have-issues-on-azure-ad-joined-devices-after-changing-their-upn"></a>S: Kullanıcılarım UPN'lerini değiştirdikten sonra Azure AD'ye katılan aygıtlarda neden sorun sorunu var?

**A:** Şu anda, AZURE AD'ye katılan cihazlarda UPN değişiklikleri tam olarak desteklenmemektedir. Böylece Azure AD ile kimlik doğrulamaları UPN değişiklikleri nden sonra başarısız olur. Sonuç olarak, kullanıcıların cihazlarında SSO ve Koşullu Erişim sorunları vardır. Şu anda, kullanıcıların bu sorunu gidermek için yeni UPN'lerini kullanarak "Diğer kullanıcı" döşemesi aracılığıyla Windows'da oturum açmaları gerekir. Şu anda bu sorunu ele almak için çalışıyoruz. Ancak, Windows Hello for Business ile oturum açarak oturum açamayan kullanıcılar bu sorunla karşılaşmaz. 

---

### <a name="q-my-users-cant-search-printers-from-azure-ad-joined-devices-how-can-i-enable-printing-from-those-devices"></a>S: Kullanıcılarım Azure AD'ye katılan aygıtlardan yazıcılarda arama yapamaz. Bu cihazlardan yazdırmayı nasıl etkinleştirebilirim?

**A:** Azure AD'ye katılan aygıtlar için yazıcıdağıtmak için [bkz.](/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy) Karma bulut yazdırmayı dağıtmak için şirket içi bir Windows Server'a ihtiyacınız vardır. Şu anda bulut tabanlı yazdırma hizmeti kullanılamıyor. 

---

### <a name="q-how-do-i-connect-to-a-remote-azure-ad-joined-device"></a>S: Uzak bir Azure AD birleştirilmiş aygıta nasıl bağlanırım?

**A:** Bkz. [Uzak Azure Etkin Dizin bağlantılı bilgisayara bağlan.](/windows/client-management/connect-to-remote-aadj-pc)

---

### <a name="q-why-do-my-users-see-you-cant-get-there-from-here"></a>S: Kullanıcılarım neden *buradan oraya ulaşamadığınızı*görüyor?

**A:** Belirli Koşullu Erişim kurallarını belirli bir aygıt durumu gerektirecek şekilde yapılandırıldınız mı? Aygıt ölçütleri karşılamazsa, kullanıcılar engellenir ve bu iletiyi görürler. Koşullu Erişim ilkesi kurallarını değerlendirin. İletiden kaçınmak için aygıtın ölçütleri karşıladığından emin olun.

---

### <a name="q-why-dont-some-of-my-users-get-azure-multi-factor-authentication-prompts-on-azure-ad-joined-devices"></a>S: Bazı kullanıcılarımdan bazıları Azure AD'ye katılan cihazlarda Azure Çok Faktörlü Kimlik Doğrulama istemlerini neden alamıyor?

**A:** Bir kullanıcı, Çok Faktörlü Kimlik Doğrulama'yı kullanarak bir aygıtı Azure AD'ye katılabilir veya kaydedebilir. Daha sonra aygıtın kendisi bu kullanıcı için güvenilir bir ikinci faktör haline gelir. Aynı kullanıcı aygıtta giriş yaptığında ve bir uygulamaya erişse, Azure AD aygıtı ikinci bir faktör olarak kabul eder. Kullanıcının ek Çok Faktörlü Kimlik Doğrulama istemleri olmadan uygulamalara sorunsuz bir şekilde erişmesini sağlar. 

Bu davranış:

- Azure AD'ye katılan ve Azure AD'ye kayıtlı aygıtlar için geçerlidir, ancak karma Azure AD'ye katılan aygıtlar için geçerli değildir.
- Bu cihazda giriş yapan diğer kullanıcı için geçerli değildir. Böylece, bu cihaza erişen diğer tüm kullanıcılar Çok Faktörlü Kimlik Doğrulama sorunu yla karşı karşıya kalır. Daha sonra Çok Faktörlü Kimlik Doğrulama gerektiren uygulamalara erişebilirler.

---

### <a name="q-why-do-i-get-a-username-or-password-is-incorrect-message-for-a-device-i-just-joined-to-azure-ad"></a>S: Azure AD'ye yeni katıldığım bir cihaz için neden *kullanıcı adı veya parola yanlış* ileti alıyorum?

**A:** Bu senaryonun yaygın nedenleri şunlardır:

- Kullanıcı kimliğiniz artık geçerli değil.
- Bilgisayarınız Azure Etkin Dizini ile iletişim kuramıyor. Ağ bağlantısı sorunlarını denetleyin.
- Federe oturum açma lar, federasyon sunucunuzun etkin ve erişilebilir WS-Trust uç noktalarını desteklemesini gerektirir. 
- Geçiş kimlik doğrulamasını etkinleştirdin. Oturum açken geçici parolanızın değiştirilmesi gerekir.

---

### <a name="q-why-do-i-see-the-oops-an-error-occurred-dialog-when-i-try-to-azure-ad-join-my-pc"></a>S: Neden Oops görüyorum *... bir hata oluştu!* Azure AD'yi çalıştığımda bilgisayarıma katılmaya çalıştığımda iletişim mi?

**A:** Bu hata, Intune ile Azure Etkin Dizin kaydı ayarladığınızda ortaya çıkar. Azure AD'ye katılmaya çalışan kullanıcının doğru Intune lisansına sahip olduğundan emin olun. Daha fazla bilgi için bkz. Windows [aygıtları için kayıt ayarlama.](/intune/windows-enroll)  

---

### <a name="q-why-did-my-attempt-to-azure-ad-join-a-pc-fail-although-i-didnt-get-any-error-information"></a>S: Herhangi bir hata bilgisi almasam da Azure AD'ye katılma girişimim neden başarısız oldu?

**A:** Olası bir nedeni, yerel yerleşik yönetici hesabını kullanarak aygıtta oturum açmış olmasıdır. Kurulumu tamamlamak için Azure Active Directory join'i kullanmadan önce farklı bir yerel hesap oluşturun. 

---

### <a name="qwhat-are-the-ms-organization-p2p-access-certificates-present-on-our-windows-10-devices"></a>S: Windows 10 cihazlarımızda MS-Organization-P2P-Access sertifikaları nelerdir?

**A:** MS-Organization-P2P-Access sertifikaları Azure AD tarafından her ikisine de verilir, Azure AD katıldı ve azure AD'nin birleştiği aygıtları hibrit olarak adlandırılır. Bu sertifikalar, uzak masaüstü senaryoları için aynı kiracıdaki aygıtlar arasında güven sağlamak için kullanılır. Bir sertifika aygıta, diğeri kullanıcıya verilir. Aygıt sertifikası içinde `Local Computer\Personal\Certificates` bulunur ve bir gün için geçerlidir. Aygıt Azure AD'de hala etkinse, bu sertifika yenilenir (yeni bir sertifika vererek). Kullanıcı sertifikası bulunur `Current User\Personal\Certificates` ve bu sertifika bir gün için de geçerlidir, ancak bir kullanıcı uzak bir masaüstü oturumunu başka bir Azure AD'ye katılan aygıta denemeettiğinde isteğe bağlı olarak verilir. Son kullanma tarihinde yenilenmez. Her iki sertifika da bulunan MS-Organization-P2P-Access sertifikası `Local Computer\AAD Token Issuer\Certificates`kullanılarak verilir. Bu sertifika, aygıt kaydı sırasında Azure AD tarafından verilir. 

---

### <a name="qwhy-do-i-see-multiple-expired-certificates-issued-by-ms-organization-p2p-access-on-our-windows-10-devices-how-can-i-delete-them"></a>S: Windows 10 cihazlarımızda MS-Organization-P2P-Access tarafından verilen birden çok süresi dolmuş sertifikayı neden görüyorum? Bunları nasıl silebilirim?

**A:** Windows 10 sürüm 1709 ve daha düşük zaman aşımına geçmiş MS-Organization-P2P-Access sertifikaları şifreleme sorunları nedeniyle bilgisayar deposunda var olmaya devam tanımlanan bir sorun vardı. Süresi dolmuş çok sayıda sertifikayı işleyemeyen herhangi bir VPN istemcisi (örneğin Cisco AnyConnect) kullanıyorsanız, kullanıcılarınız ağ bağlantısıyla ilgili sorunlarla karşı karşıya kalabilir. Bu sorun, süresi dolmuş MS-Organization-P2P-Access sertifikalarını otomatik olarak silmek için Windows 10 1803 sürümünde düzeltildi. Aygıtlarınızı Windows 10 1803 olarak güncelleyerek bu sorunu çözebilirsiniz. Güncelleştiremiyorsanız, herhangi bir olumsuz etkisi olmadan bu sertifikaları silebilirsiniz.  

---

## <a name="hybrid-azure-ad-join-faq"></a>Karma Azure AD SSS'ye katıldı

### <a name="q-how-do-i-unjoin-a-hybrid-azure-ad-joined-device-locally-on-the-device"></a>S: Karma Azure AD'ye yerel olarak katılan bir Aygıta nasıl katılabilirim?

**A:** Karma Azure AD birleştirilmiş aygıtlar için otomatik kaydı kapattırın. Sonra zamanlanan görev aygıtı yeniden kaydetmez. Ardından, yönetici olarak bir komut `dsregcmd.exe /debug /leave`istemi açın ve girin. Veya toplu olarak katılmamak için bu komutu birkaç aygıtta komut dosyası olarak çalıştırın.

### <a name="q-where-can-i-find-troubleshooting-information-to-diagnose-hybrid-azure-ad-join-failures"></a>S: Karma Azure AD birleştirme hatalarını tanılamak için sorun giderme bilgilerini nerede bulabilirim?

**A:** Sorun giderme bilgileri için şu makalelere bakın:

- [Sorun giderme hibrit Azure Active Directory, Windows 10 ve Windows Server 2016 aygıtlarına katıldı](troubleshoot-hybrid-join-windows-current.md)
- [Karma Azure Etkin Dizini'nde sorun giderme, alt düzey aygıtları birleştirdi](troubleshoot-hybrid-join-windows-legacy.md)
 
### <a name="q-why-do-i-see-a-duplicate-azure-ad-registered-record-for-my-windows-10-hybrid-azure-ad-joined-device-in-the-azure-ad-devices-list"></a>S: Azure AD aygıtları listesinde windows 10 karma Azure AD'm'ın birleştiği aygıt için neden yinelenen bir Azure AD kayıtlı kaydı nı görüyorum?

**A:** Kullanıcılarınız hesaplarını etki alanıyla birleştirilmiş bir cihazdaki uygulamalara eklediğinde, Windows'a hesap ekle ile bu hesaplar istenebilebilir **mi?** İstemde **Evet** girerlerse, aygıt Azure AD'ye kaydolur. Güven türü Azure AD kayıtlı olarak işaretlenir. Kuruluşunuzdaki karma Azure AD'yi etkinleştirdikten sonra, aygıt karma Azure AD'ye de katılır. Sonra aynı aygıt için iki aygıt durumları gösterir. 

Karma Azure AD join, Azure AD kayıtlı durumundan önce gelir. Bu nedenle, cihazınız herhangi bir kimlik doğrulama ve Koşullu Erişim değerlendirmesi için karma Azure AD olarak kabul edilir. Azure AD kayıtlı aygıt kaydını Azure AD portalından güvenle silebilirsiniz. Windows [10 makinesinde bu çift durumu önlemeyi veya temizlemeyi](hybrid-azuread-join-plan.md#review-things-you-should-know)öğrenin. 

---

### <a name="q-why-do-my-users-have-issues-on-windows-10-hybrid-azure-ad-joined-devices-after-changing-their-upn"></a>S: Kullanıcılarım, UPN'lerini değiştirdikten sonra Windows 10 karma Azure AD'nin birleştiği cihazlarda neden sorun sorunu var?

**A:** Şu anda UPN değişiklikleri, karma Azure AD'ye katılan aygıtlarla tam olarak desteklenmemektedir. Kullanıcılar aygıtta oturum açabilir ve şirket içi uygulamalarına erişebilir, ancak Azure AD ile kimlik doğrulama bir UPN değişikliğinden sonra başarısız olur. Sonuç olarak, kullanıcıların cihazlarında SSO ve Koşullu Erişim sorunları vardır. Şu anda, Azure AD'den aygıta katılmanız (yüksek ayrıcalıklarla "dsregcmd /bırak" çalıştırın) ve sorunu çözmek için yeniden katılmanız (otomatik olarak gerçekleşir). Şu anda bu sorunu ele almak için çalışıyoruz. Ancak, Windows Hello for Business ile oturum açarak oturum açamayan kullanıcılar bu sorunla karşılaşmaz. 

---

### <a name="q-do-windows-10-hybrid-azure-ad-joined-devices-require-line-of-sight-to-the-domain-controller-to-get-access-to-cloud-resources"></a>S: Windows 10 karma Azure AD'ye katılan aygıtlar bulut kaynaklarına erişmek için etki alanı denetleyicisine görüş hattı gerektirir mi?

**A:** Hayır, kullanıcının parolasının değiştirilmesi dışında. Windows 10 karma Azure AD join tamamlandıktan ve kullanıcı en az bir kez oturum açtıktan sonra, aygıt bulut kaynaklarına erişmek için etki alanı denetleyicisine görüş hattı gerektirmez. Windows 10, parola değiştirilmediği sürece, Internet bağlantısı olan her yerden Azure AD uygulamalarında tek oturum açabilir. Windows Hello for Business ile oturum kuran kullanıcılar, etki alanı denetleyicilerine görüş sırası olmasa bile parola değişikliğinden sonra bile Azure AD uygulamalarında tek oturum açma hakkı almaya devam eder. 

---

### <a name="q-what-happens-if-a-user-changes-their-password-and-tries-to-login-to-their-windows-10-hybrid-azure-ad-joined-device-outside-the-corporate-network"></a>S: Bir kullanıcı parolasını değiştirir ve windows 10 karma Azure AD'si ile şirket ağı dışında birleştirilmiş aygıta oturum açmaya çalışırsa ne olur?

**A:** Bir parola şirket ağı dışında değiştirilirse (örneğin, Azure AD SSPR kullanılarak), yeni parolayla oturum açan kullanıcı başarısız olur. Karma Azure AD'nin birleştiği aygıtlar için şirket içi Active Directory birincil yetkilidir. Bir aygıt etki alanı denetleyicisine görüş hattı yoksa, yeni parolayı doğrulayamaz. Bu nedenle, kullanıcının yeni parolasıyla cihazda oturum açabilmesi için etki alanı denetleyicisiyle (VPN üzerinden veya şirket ağında olmak) bağlantı kurması gerekir. Aksi takdirde, yalnızca Windows'da önbelleğe alınmış oturum açma özelliği nedeniyle eski parolalarıyla oturum açabilirler. Ancak, eski parola belirteç istekleri sırasında Azure AD tarafından geçersiz kılınmaz ve bu nedenle tek oturum açmayı önler ve aygıt tabanlı Koşullu Erişim ilkelerinde başarısız olur. İşletmeler için Windows Hello kullanıyorsanız, bu sorun oluşmaz. 

---

## <a name="azure-ad-register-faq"></a>Azure AD kaydı SSS

### <a name="q-how-do-i-remove-an-azure-ad-registered-state-for-a-device-locally"></a>S: Bir aygıt için Azure AD kayıtlı durumunu yerel olarak nasıl kaldıracağım?

**A:** 
- Windows 10 Azure AD kayıtlı aygıtlar için **Ayarlar** > **Hesapları** > **Erişim İşi veya Okula**gidin. Hesabınızı seçin ve **Bağlantıyı Kes'i**seçin. Cihaz kaydı, Windows 10'daki kullanıcı profili başına gelir.
- iOS ve Android için Microsoft Authenticator uygulaması **Ayarlar** > **Aygıt Kaydı'nı** kullanabilir ve Kayıt **Dışı aygıtı**seçebilirsiniz.
- macOS için, aygıtı yönetimden kaldırmak ve herhangi bir kaydı kaldırmak için Microsoft Intune Company Portal uygulamasını kullanabilirsiniz. 

---
### <a name="q-how-can-i-block-users-from-adding-additional-work-accounts-azure-ad-registered-on-my-corporate-windows-10-devices"></a>S: Kullanıcıların kurumsal Windows 10 aygıtlarıma ek iş hesapları (Azure AD kayıtlı) eklemelerini nasıl engelleyebilirim?

**A:** Kullanıcılarınızın kurumsal etki alanınıza, Azure AD'ınbirleştiğiveya karma Azure AD'nin Windows 10 aygıtlarına ek iş hesapları eklemesini engellemek için aşağıdaki kayıt defterini etkinleştirin. Bu ilke, etki alanı birleşen makinelerin yanlışlıkla Azure AD'yi aynı kullanıcı hesabına kaydetmesini engellemek için de kullanılabilir. 

`HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin"=dword:00000001`

---
### <a name="q-can-i-register-android-or-ios-byod-devices"></a>S: Android veya iOS BYOD cihazlarını kaydedebilir miyim?

**A:** Evet, ancak yalnızca Azure aygıt kayıt hizmeti ve karma müşteriler için. Aktif Dizin Federasyon Hizmetleri'ndeki (AD FS) şirket içi cihaz kayıt hizmetiyle desteklenmez.

---
### <a name="q-how-can-i-register-a-macos-device"></a>S: MacOS cihazını nasıl kaydedebilirim?

**A:** Aşağıdaki adımları izleyin:

1.    [Uyumluluk ilkesi oluşturma](/intune/compliance-policy-create-mac-os)
1.    [macOS aygıtları için Koşullu Erişim ilkesi tanımlama](../active-directory-conditional-access-azure-portal.md) 

**Açıklamalar:**

- Koşullu Erişim ilkenizde yer alan kullanıcıların kaynaklara erişmek [için macOS'un desteklenen](../conditional-access/concept-conditional-access-conditions.md) bir Office sürümüne ihtiyacı vardır. 
- İlk erişim denemesi sırasında, kullanıcılarınızın şirket portalını kullanarak cihazı kaydetmesi istenir.

---
## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD kayıtlı cihazlar](concept-azure-ad-register.md) hakkında daha fazla bilgi edinin
- [Azure AD'ye katılan cihazlar](concept-azure-ad-join.md) hakkında daha fazla bilgi edinin
- [Karma Azure AD'ye katılan aygıtlar](concept-azure-ad-join-hybrid.md) hakkında daha fazla bilgi edinin
