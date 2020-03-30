---
title: Sorun Giderme Azure MFA NPS uzantısı - Azure Etkin Dizini
description: Azure Çok Faktörlü Kimlik Doğrulama için NPS uzantısı ile ilgili sorunları çözme konusunda yardım alın
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
ms.openlocfilehash: 3462ada0864d2d8321b1936e94f947c55c754879
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294512"
---
# <a name="resolve-error-messages-from-the-nps-extension-for-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme

Azure Çok Faktörlü Kimlik Doğrulama için NPS uzantısıile ilgili hatalarla karşılaşırsanız, bir çözüme daha hızlı ulaşmak için bu makaleyi kullanın. NPS uzantıgünlükleri, NPS Uzantısı'nın yüklendiği sunucuda **Özel Görünümler** > **Sunucu Rolleri** > **Ağ Politikası ve Erişim Hizmetleri** altında Olay Görüntüleyicisi'nde bulunur.

## <a name="troubleshooting-steps-for-common-errors"></a>Sık karşılaşılan hatalar için sorun giderme adımları

| Hata kodu | Sorun giderme adımları |
| ---------- | --------------------- |
| **CONTACT_SUPPORT** | [Desteğe başvurun](#contact-microsoft-support)ve günlükleri toplamak için adımların listesini belirtin. Kiracı kimliği ve kullanıcı ana adı (UPN) dahil olmak üzere hatadan önce ne olduğu hakkında olabildiğince fazla bilgi sağlayın. |
| **CLIENT_CERT_INSTALL_ERROR** | İstemci sertifikasının nasıl yüklendiğinde veya kiracınızla ilişkili olduğuyla ilgili bir sorun olabilir. İstemci sertifika sorunlarını araştırmak için [MFA NPS uzantısı sorun giderme](howto-mfa-nps-extension.md#troubleshooting) yönergelerini izleyin. |
| **ESTS_TOKEN_ERROR** | İstemci sertifikası ve ADAL belirteç sorunlarını araştırmak için [MFA NPS uzantısı](howto-mfa-nps-extension.md#troubleshooting) sorun giderme yönergelerini izleyin. |
| **HTTPS_COMMUNICATION_ERROR** | NPS sunucusu Azure MFA'dan yanıt alamıyor. Güvenlik duvarlarınızın gelen ve gelen trafik için çift yönlü olarak açık olduğunu doğrulayınhttps://adnotifications.windowsazure.com |
| **HTTP_CONNECT_ERROR** | NPS uzantısını çalıştıran sunucuda, `https://adnotifications.windowsazure.com` erişebileceğinizi `https://login.microsoftonline.com/`doğrulayın ve. Bu siteler yüklenmezse, bu sunucudaki bağlantı nın giderimi sorun giderin. |
| **Azure MFA için NPS Uzantısı:** <br> Azure MFA için NPS Uzantısı yalnızca AccessAccept Durumunda Yarıçap istekleri için İkincil Auth gerçekleştirir. Kullanıcı kullanıcı adı için yanıt durumu AccessReject ile alınan istek, isteği yoksayarak. | Bu hata genellikle AD'deki bir kimlik doğrulama hatasını veya NPS sunucusunun Azure AD'den yanıt alamadığını yansıtır. Güvenlik duvarlarınızın 80 `https://adnotifications.windowsazure.com` ve `https://login.microsoftonline.com` 443 bağlantı noktalarına girip çıkmak için çift yönlü olarak açık olduğunu doğrulayın. Ağ Erişim İzinleri'nin DIAL-IN sekmesinde ayarın "NPS Ağ Politikası üzerinden erişimi denetlemek" için ayarlı olup olmadığını da kontrol etmek önemlidir. Kullanıcıya lisans atanmamışsa, bu hata da tetiklenebilir. |
| **REGISTRY_CONFIG_ERROR** | Uygulamanın kayıt defterinde bir anahtar eksiktir, bu da [PowerShell komut dosyasının](howto-mfa-nps-extension.md#install-the-nps-extension) kurulumdan sonra çalıştırılmadığı için olabilir. Hata iletisi eksik anahtarı içermelidir. anahtarı n HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa altında olduğundan emin olun. |
| **REQUEST_FORMAT_ERROR** <br> Radius İsteği eksik zorunlu Radius userName\Tanımlayıcı özniteliği. NPS'nin RADIUS isteklerini aldığını doğrulayın | Bu hata genellikle bir yükleme sorununu yansıtır. NPS uzantısı RADIUS isteklerini alabilen NPS sunucularında yüklenmelidir. RDG ve RRAS gibi hizmetler için bağımlılık olarak yüklenen NPS sunucuları yarıçap isteklerialmaz. NPS Uzantı, kimlik doğrulama isteğindeki ayrıntıları okuyamadığı için bu tür yüklemeler ve hatalar üzerine yüklendiğinde çalışmaz. |
| **REQUEST_MISSING_CODE** | NPS ve NAS sunucuları arasındaki parola şifreleme protokolünün kullandığınız ikincil kimlik doğrulama yöntemini desteklediğinden emin olun. **PAP,** bulutta Azure MFA'nın tüm kimlik doğrulama yöntemlerini destekler: telefon görüşmesi, tek yönlü kısa mesaj, mobil uygulama bildirimi ve mobil uygulama doğrulama kodu. **CHAPV2** ve **EAP** destek telefon görüşmesi ve mobil uygulama bildirimi. |
| **USERNAME_CANONICALIZATION_ERROR** | Kullanıcının şirket içi Active Directory örneğinde bulunduğunu ve NPS Hizmetinin dizine erişmek için izinleri olduğunu doğrulayın. Ormanlar arası güvenler kullanıyorsanız, daha fazla yardım için [desteğe başvurun.](#contact-microsoft-support) |

### <a name="alternate-login-id-errors"></a>Alternatif giriş kimliği hataları

| Hata kodu | Hata iletisi | Sorun giderme adımları |
| ---------- | ------------- | --------------------- |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: userObjectSid arama başarısız oldu | Kullanıcının şirket içi Active Directory örneğinde var olduğunu doğrulayın. Ormanlar arası güvenler kullanıyorsanız, daha fazla yardım için [desteğe başvurun.](#contact-microsoft-support) |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: Alternatif Giriş Kimliği araması başarısız oldu | LDAP_ALTERNATE_LOGINID_ATTRIBUTE geçerli bir [etkin dizin özniteliği](https://msdn.microsoft.com/library/ms675090(v=vs.85).aspx)olarak ayarlı olduğunu doğrulayın. <br><br> LDAP_FORCE_GLOBAL_CATALOG True olarak ayarlanmışsa veya LDAP_LOOKUP_FORESTS boş olmayan bir değerle yapılandırılırsa, Bir Global Catalog yapılandırdığınızı ve buna AlternativeLoginId özniteliğinin eklendiğini doğrulayın. <br><br> LDAP_LOOKUP_FORESTS boş olmayan bir değerle yapılandırılırsa, değerin doğru olduğundan doğrulayın. Birden fazla orman adı varsa, adlar boşluklarla değil, yarı iki nokta ile ayrılmalıdır. <br><br> Bu adımlar sorunu çözmezse, daha fazla yardım için [desteğe başvurun.](#contact-microsoft-support) |
| **ALTERNATE_LOGIN_ID_ERROR** | Hata: Alternatif Giriş Değeri boş | AlternateLoginId özniteliğinin kullanıcı için yapılandırıldığından doğrulayın. |

## <a name="errors-your-users-may-encounter"></a>Kullanıcılarınızın karşılaşabileceği hatalar

| Hata kodu | Hata iletisi | Sorun giderme adımları |
| ---------- | ------------- | --------------------- |
| **Erişim Reddedildi** | Arayan kiracının kullanıcı için kimlik doğrulaması yapmak için erişim izinleri yok | Kiracı etki alanı ile kullanıcı ana adının (UPN) etki alanının aynı olup olmadığını kontrol edin. Örneğin, bunun user@contoso.com Contoso kiracısına kimlik doğrulamaya çalıştığından emin olun. UPN, Azure'daki kiracı için geçerli bir kullanıcıyı temsil eder. |
| **Kimlik DoğrulamaYöntemiNotConfigured** | Belirtilen kimlik doğrulama yöntemi kullanıcı için yapılandırılmamıştır | [İki aşamalı doğrulama için ayarlarınızı yönet'teki](../user-help/multi-factor-authentication-end-user-manage-settings.md)talimatlara göre kullanıcının doğrulama yöntemlerini eklemesini veya doğrulamasını bekleyin. |
| **Kimlik DoğrulamaYöntemiNotSupported** | Belirtilen kimlik doğrulama yöntemi desteklenmez. | Bu hatayı içeren tüm günlüklerinizi toplayın ve [desteğe başvurun.](#contact-microsoft-support) Desteğe başvurttunca, kullanıcı adını ve hatayı tetikleyen ikincil doğrulama yöntemini sağlayın. |
| **BecAccessDenied** | MSODS Bec arama geri döndü erişim reddedildi, muhtemelen kullanıcı adı kiracı tanımlı değildir | Kullanıcı, Etkin Dizin'de şirket içinde bulunur, ancak AD Connect tarafından Azure AD'ye eşitlenmez. Veya, kullanıcı kiracı için eksik. Kullanıcıyı Azure AD'ye ekleyin ve [iki aşamalı doğrulama için ayarlarınızı yönet'teki](../user-help/multi-factor-authentication-end-user-manage-settings.md)yönergelere göre doğrulama yöntemlerini eklemelerini isteyin. |
| **Geçersiz Format** veya **StrongAuthenticationServiceGeçersizParametre** | Telefon numarası tanınmaz bir biçimde | Kullanıcının doğrulama telefon numaralarını düzeltmesini sağlar. |
| **Geçersiz Oturum** | Belirtilen oturum geçersiz veya süresi dolmuş olabilir | Oturumun tamamlanması üç dakikadan fazla sürdü. Kimlik doğrulama isteğini başlattıktan sonraki üç dakika içinde, kullanıcının doğrulama kodunu girdiğini veya uygulama bildirimine yanıt verdiğini doğrulayın. Bu sorunu çözmezse, istemci, NAS Server, NPS Server ve Azure MFA bitiş noktası arasında ağ gecikmesi olmadığından denetleyin.  |
| **NoDefaultAuthenticationMethodYapılı** | Kullanıcı için varsayılan kimlik doğrulama yöntemi yapılandırılmadı | [İki aşamalı doğrulama için ayarlarınızı yönet'teki](../user-help/multi-factor-authentication-end-user-manage-settings.md)talimatlara göre kullanıcının doğrulama yöntemlerini eklemesini veya doğrulamasını bekleyin. Kullanıcının varsayılan kimlik doğrulama yöntemini seçtiğini ve bu yöntemi hesapları için yapılandırdığını doğrulayın. |
| **YeminCodePinIncorrect** | Yanlış kod ve pin girilen. | Bu hata NPS uzantısında bekgöremez. Kullanıcınız bununla karşılaşırsa, sorun giderme yardımı için [desteğe başvurun.](#contact-microsoft-support) |
| **ProofDataNotFound** | Kanıt verileri belirtilen kimlik doğrulama yöntemi için yapılandırılmamadı. | Kullanıcının farklı bir doğrulama yöntemi denemesini veya iki aşamalı [doğrulama için ayarlarınızı yönet'teki](../user-help/multi-factor-authentication-end-user-manage-settings.md)yönergelere göre yeni bir doğrulama yöntemi eklemesini bekleyin. Doğrulama yönteminin doğru ayarlı olduğunu doğruladıktan sonra kullanıcı bu hatayı görmeye devam ederse, [desteğe başvurun.](#contact-microsoft-support) |
| **SMSAuthFailedWrongCodePinEntered** | Yanlış kod ve pin girilen. (OneWaySMS) | Bu hata NPS uzantısında bekgöremez. Kullanıcınız bununla karşılaşırsa, sorun giderme yardımı için [desteğe başvurun.](#contact-microsoft-support) |
| **KiracıEngellendi** | Kiracı engellendi | Azure portalındaki Azure AD özellikleri sayfasından Dizin Kimliği ile [desteğe başvurun.](#contact-microsoft-support) |
| **Kullanıcı Bulunamadı** | Belirtilen kullanıcı bulunamadı | Kiracı artık Azure AD'de etkin olarak görünmüyor. Aboneliğinizin etkin olup olmadığını ve gerekli birinci taraf uygulamalara sahip olup olmadığınızı kontrol edin. Ayrıca, sertifika konusu kiracının beklendiği gibi olduğundan ve sertifikanın hala geçerli olduğundan ve hizmet sorumlusuna kayıtlı olduğundan emin olun. |

## <a name="messages-your-users-may-encounter-that-arent-errors"></a>Kullanıcılarınızın karşılaşabileceği ve hata olmayan iletiler

Bazen, kullanıcılarınızın kimlik doğrulama istekleri başarısız olduğu için Çok Faktörlü Kimlik Doğrulama'dan iletiler alabilir. Bunlar yapılandırma ürünündeki hatalar değildir, ancak kimlik doğrulama isteğinin neden reddedildiğini açıklayan kasıtlı uyarılardır.

| Hata kodu | Hata iletisi | Önerilen adımlar | 
| ---------- | ------------- | ----------------- |
| **Yemin KoduYanlış** | Yanlış kod girilen\YEMIN Kodu Yanlış | Kullanıcı yanlış kodu girdi. Yeni bir kod isteyerek veya yeniden oturum açmayaparak yeniden denemelerini isteyin. | 
| **SMSAuthFailedMaxAllowedCodeRetryUlaşıl** | İzin verilen maksimum kod yeniden deneme | Kullanıcı doğrulama meydan okuma çok kez başarısız oldu. Ayarlarınıza bağlı olarak, artık bir yönetici tarafından engelinin kaldırılması gerekebilir.  |
| **SMSAuthFailedWrongCodeEntered** | Yanlış kod girilen/Metin Mesajı OTP Yanlış | Kullanıcı yanlış kodu girdi. Yeni bir kod isteyerek veya yeniden oturum açmayaparak yeniden denemelerini isteyin. |

## <a name="errors-that-require-support"></a>Destek gerektiren hatalar

Bu hatalardan biriyle karşılaşırsanız, tanılama yardımı için [desteğe başvurmanızı](#contact-microsoft-support) öneririz. Bu hataları giderebilecek standart bir adım kümesi yoktur. Kişi desteği yaptığınızda, hataya yol açan adımlar ve kiracı bilgileriniz hakkında mümkün olduğunca çok bilgi eklediğinizden emin olun.

| Hata kodu | Hata iletisi |
| ---------- | ------------- |
| **GeçersizParametre** | İstek geçersiz olmamalıdır |
| **GeçersizParametre** | ObjectId, ReplicationScope için boş veya boş olmamalıdır:{0} |
| **GeçersizParametre** | CompanyName \{0}\ uzunluğu izin verilen maksimum uzunluktan daha uzundur{1} |
| **GeçersizParametre** | UserPrincipalName geçersiz veya boş olmamalıdır |
| **GeçersizParametre** | Sağlanan TenantId doğru biçimde değil |
| **GeçersizParametre** | SessionId boş veya boş olmamalıdır |
| **GeçersizParametre** | İstek veya Msods herhangi bir ProofData çözemedi. ProofData bilinmiyor |
| **InternalError** |  |
| **YeminCodePinIncorrect** |  |
| **SürümNotSupported** |  |
| **MFAPinNotSetup** |  |

## <a name="next-steps"></a>Sonraki adımlar

### <a name="troubleshoot-user-accounts"></a>Kullanıcı hesaplarını sorun giderme

Kullanıcılarınız [iki aşamalı doğrulama ile ilgili sorun yaşıyorsanız,](../user-help/multi-factor-authentication-end-user-troubleshoot.md)kendi kendine tanı sorunları yardımcı olur.

### <a name="health-check-script"></a>Sistem durumu denetimi komut dosyası

[Azure MFA NPS Uzantı denetim komut dosyası,](https://docs.microsoft.com/samples/azure-samples/azure-mfa-nps-extension-health-check/azure-mfa-nps-extension-health-check/) NPS uzantısında sorun giderildiğinde temel bir sistem durumu denetimi gerçekleştirir. Komut dosyasını çalıştırın ve 3 seçeneğini seçin.

### <a name="contact-microsoft-support"></a>Microsoft desteğine başvurma

Ek yardıma ihtiyacınız varsa, Azure [Çok Faktörlü Kimlik Doğrulama Sunucusu desteği](https://support.microsoft.com/oas/default.aspx?prid=14947)aracılığıyla bir destek uzmanına başvurun. Bizimle iletişim e-son ları yaparken, sorununuz hakkında mümkün olduğunca çok bilgi eklemeniz yararlı olacaktır. Verebileceğiniz bilgiler, hatayı gördüğünüz sayfayı, belirli hata kodunu, belirli oturum kimliğini, hatayı gören kullanıcının kimliğini ve hata ayıklama günlüklerini içerir.

Destek tanılama için hata ayıklama günlükleri toplamak için NPS sunucusunda aşağıdaki adımları kullanın:

1. Kayıt Defteri Düzenleyicisi'ni açın ve **VERBOSE_LOG** **TRUE'ya** ayarlanan HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa'ya göz atın
2. Yönetici komut istemini açın ve şu komutları çalıştırın:

   ```
   Mkdir c:\NPS
   Cd NPS
   netsh trace start Scenario=NetConnection capture=yes tracefile=c:\NPS\nettrace.etl
   logman create trace "NPSExtension" -ow -o c:\NPS\NPSExtension.etl -p {7237ED00-E119-430B-AB0F-C63360C8EE81} 0xffffffffffffffff 0xff -nb 16 16 -bs 1024 -mode Circular -f bincirc -max 4096 -ets
   logman update trace "NPSExtension" -p {EC2E6D3A-C958-4C76-8EA4-0262520886FF} 0xffffffffffffffff 0xff -ets
   ```

3. Sorunu yeniden oluşturun

4. Bu komutlarla izleme yi durdurun:

   ```
   logman stop "NPSExtension" -ets
   netsh trace stop
   wevtutil epl AuthNOptCh C:\NPS\%computername%_AuthNOptCh.evtx
   wevtutil epl AuthZOptCh C:\NPS\%computername%_AuthZOptCh.evtx
   wevtutil epl AuthZAdminCh C:\NPS\%computername%_AuthZAdminCh.evtx
   Start .
   ```

5. Kayıt Defteri Düzenleyicisi'ni açın ve **VERBOSE_LOG** **FALSE** olarak ayarlanan HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\AzureMfa'ya göz atın
6. C:\NPS klasörünün içeriğini zipleyin ve sıkıştırılan dosyayı destek talebine takın.
