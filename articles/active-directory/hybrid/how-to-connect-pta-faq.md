---
title: 'Azure AD Connect: Geçiş Kimlik Doğrulama - Sık sorulan sorular | Microsoft Dokümanlar'
description: Azure Active Directory Pass-through Authentication hakkında sık sorulan soruların yanıtları
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
ms.date: 03/09/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 661d4f622dce45aeca1d41ead60f05ccdcfbc9c9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406873"
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>Azure Active Directory Pass-through Authentication: Sık sorulan sorular

Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması hakkında sık sorulan sorular ele alınıyor. Güncelleştirilmiş içeriği kontrol etmeye devam edin.

## <a name="which-of-the-methods-to-sign-in-to-azure-ad-pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs-should-i-choose"></a>Azure AD, Geçiş Kimlik Doğrulaması, parola karma senkronizasyonu ve Active Directory Federation Services (AD FS) oturum açma yöntemlerinden hangisini seçmeliyim?

Çeşitli Azure AD oturum açma yöntemlerinin karşılaştırılması ve kuruluşunuz için doğru oturum açma yönteminin nasıl seçilecek için [bu kılavuzu](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn) gözden geçirin.

## <a name="is-pass-through-authentication-a-free-feature"></a>Pass-through Kimlik Doğrulama ücretsiz bir özellik midir?

Pass-through Kimlik Doğrulama ücretsiz bir özelliktir. Kullanmak için Azure AD'nin ücretli sürümlerine ihtiyacınız yoktur.

## <a name="is-pass-through-authentication-available-in-the-microsoft-azure-germany-cloud-and-the-microsoft-azure-government-cloud"></a>Geçiş Kimlik Doğrulaması Microsoft [Azure Almanya bulutu](https://www.microsoft.de/cloud-deutschland) ve [Microsoft Azure Devlet bulutu](https://azure.microsoft.com/features/gov/)kullanılabilir mi?

Hayır. Geçiş Kimlik Doğrulaması yalnızca dünya çapındaki Azure AD örneğinde kullanılabilir.

## <a name="does-conditional-access-work-with-pass-through-authentication"></a>[Koşullu Erişim](../active-directory-conditional-access-azure-portal.md) Geçiş Kimlik Doğrulamasıyla çalışır mı?

Evet. Azure Çok Faktörlü Kimlik Doğrulama dahil tüm Koşullu Erişim özellikleri, Geçiş Kimlik Doğrulaması ile çalışır.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Geçiş Kimlik Doğrulama, "userPrincipalName" yerine kullanıcı adı olarak "Alternatif Kimlik"i destekliyor mu?
Alternatif bir e-posta gibi UPN olmayan bir değeri kullanarak oturum açma, şu anda hem geçiş kimlik doğrulaması (PTA) hem de parola karma eşitleme (PHS) için özel önizlemede test ediliyor.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Parola karma eşitleme, Geçiş Kimlik Doğrulaması'na bir geri dönüş görevi mi veriyor?

Hayır. Geçiş Kimlik Doğrulama otomatik olarak parola karma senkronizasyonu na _geçmiyor._ Kullanıcı oturum açma hatalarını önlemek için, [yüksek kullanılabilirlik](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)için Geçiş Kimlik Doğrulaması'nı yapılandırmanız gerekir.

## <a name="what-happens-when-i-switch-from-password-hash-synchronization-to-pass-through-authentication"></a>Parola karma senkronizasyonundan Geçiş Kimlik Doğrulaması'na geçtiğimde ne olur?

Oturum açma yöntemini parola karma senkronizasyonundan Geçiş Kimlik Doğrulaması'na geçirmek için Azure AD Connect'i kullandığınızda, Geçiş Kimlik Doğrulaması yönetilen etki alanlarındaki kullanıcılarınız için birincil oturum açma yöntemi haline gelir. Daha önce parola karma eşitlemesi ile senkronize edilmiş tüm kullanıcıların parola karmalarının Azure AD'de depolandığını lütfen unutmayın.

## <a name="can-i-install-an-azure-ad-application-proxy-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Bir Azure [AD Application Proxy](../manage-apps/application-proxy.md) bağlayıcısını Geçiş Kimlik Doğrulaması Aracısı ile aynı sunucuya yükleyebilir miyim?

Evet. Pass-through Authentication Agent, sürüm 1.5.193.0 veya sonraki rebranded sürümleri, bu yapılandırmayı destekler.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Azure AD Connect ve Pass-through Authentication Agent'ın hangi sürümlerine ihtiyacınız var?

Bu özelliğin işe yaraması için Azure AD Connect ve 1.5.193.0 veya daha sonra Geçiş Kimlik Doğrulama Aracısı için sürüm 1.1.750.0 veya daha sonra sınamanız gerekir. Tüm yazılımları Windows Server 2012 R2 veya sonraki sunucularda yükleyin.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication"></a>Kullanıcımın parolasının süresi dolduysa ve Geçiş Kimlik Doğrulaması'nı kullanarak oturum açmaya çalışırlarsa ne olur?

Belirli bir kullanıcı için [parola yazmayı](../authentication/concept-sspr-writeback.md) yeniden yapılandırmışsanız ve kullanıcı Geçiş Kimlik Doğrulaması'nı kullanarak giriş yaparsa, parolalarını değiştirebilir veya sıfırlayabilir. Parolalar beklendiği gibi şirket içi Active Directory'ye geri yazılır.

Belirli bir kullanıcı için parola yazmayı yapılandırmadıysanız veya kullanıcının atanmış geçerli bir Azure AD lisansı yoksa, kullanıcı parolasını bulutta güncelleştiremez. Parolalarının süresi dolmuş olsa bile parolalarını güncelleştiremezler. Kullanıcı bunun yerine şu iletiyi görür: "Kuruluşunuz bu sitede parolanızı güncelleştirmenize izin vermez. Kuruluşunuz tarafından önerilen yönteme göre güncelleyin veya yöneticinize yardıma ihtiyacınız olup olmadığını sorun." Kullanıcı veya yönetici, parolalarını şirket içi Active Directory'de sıfırlamalıdır.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Pass-through Kimlik Doğrulama sizi kaba kuvvet parola saldırılarına karşı nasıl korur?

[Akıllı Lokavt hakkında bilgi edinin.](../authentication/howto-password-smart-lockout.md)

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>Geçiş Kimlik Doğrulama Aracıları 80 ve 443 bağlantı noktaları üzerinden ne iletişim kurar?

- Kimlik Doğrulama Aracıları, tüm özellik işlemleri için bağlantı noktası 443 üzerinden HTTPS isteklerinde bulunur.
- Kimlik Doğrulama Aracıları, TLS/SSL sertifika iptal listelerini (CRLs) indirmek için 80 bağlantı noktası üzerinden HTTP isteklerinde bulunur.

     >[!NOTE]
     >Son güncelleştirmeler özelliğin gerektirdiği bağlantı noktası sayısını azalttı. Azure AD Connect veya Kimlik Doğrulama Aracısı'nın eski sürümlerivarsa, bu bağlantı noktalarını da açık tutun: 5671, 8080, 9090, 9091, 9350, 9352 ve 10100-10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Pass-through Kimlik Doğrulama Aracıları giden bir web proxy sunucusu üzerinden iletişim kurabilir mi?

Evet. Web Proxy Otomatik Bulma (WPAD) şirket içi ortamınızda etkinleştirilirse, Kimlik Doğrulama Aracıları ağda bir web proxy sunucusu bulmaya ve kullanmaya çalışır.

Ortamınızda WPAD yoksa, bir Geçiş Kimlik Doğrulama Aracısı'nın Azure AD ile iletişim kurmasına izin vermek için proxy bilgileri (aşağıda gösterildiği gibi) ekleyebilirsiniz:
- Geçiş Kimlik Doğrulama Aracısını sunucuya yüklemeden önce Internet Explorer'da proxy bilgilerini yapılandırın. Bu, Kimlik Doğrulama Aracısı yüklemesini tamamlamanızı sağlar, ancak yine de Yönetici portalında **Etkin olmayan** olarak gösterecektir.
- Sunucuda "C:\Program Files\Microsoft Azure AD Connect Authentication Agent" adresine gidin.
- "AzureADConnectAuthenticationAgentService" yapılandırma dosyasını düzenleme ve aşağıdaki satırları\:ekleyin (gerçek proxy adresinizle "http //contosoproxy.com:8080" değiştirin):

```
   <system.net>
      <defaultProxy enabled="true" useDefaultCredentials="true">
         <proxy
            usesystemdefault="true"
            proxyaddress="http://contosoproxy.com:8080"
            bypassonlocal="true"
         />
     </defaultProxy>
   </system.net>
```

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Aynı sunucuya iki veya daha fazla Geçiş Kimlik Doğrulama Aracısı yükleyebilir miyim?

Hayır, tek bir sunucuya yalnızca bir Geçiş Kimlik Doğrulama Aracısı yükleyebilirsiniz. Yüksek kullanılabilirlik için Geçiş Kimlik Doğrulamasını yapılandırmak istiyorsanız, [buradaki yönergeleri izleyin.](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)

## <a name="do-i-have-to-manually-renew-certificates-used-by-pass-through-authentication-agents"></a>Pass-through Kimlik Doğrulama Aracıları tarafından kullanılan sertifikaları el ile yenilemek zorunda mıyım?

Her Geçiş Kimlik Doğrulama Aracısı ve Azure AD arasındaki iletişim, sertifika tabanlı kimlik doğrulaması kullanılarak güvenli hale gelir. Bu [sertifikalar Azure AD tarafından birkaç ayda bir otomatik olarak yenilenir.](how-to-connect-pta-security-deep-dive.md#operational-security-of-the-authentication-agents) Bu sertifikaları el ile yenilemenize gerek yoktur. Eski süresi dolmuş sertifikaları gerektiği gibi temizleyebilirsiniz.

## <a name="how-do-i-remove-a-pass-through-authentication-agent"></a>Geçiş Kimlik Doğrulama Aracısını nasıl kaldıracağım?

Geçiş Kimlik Doğrulama Aracısı çalışıyor sayılsa da, etkin kalır ve kullanıcı oturum açma isteklerini sürekli olarak işler. Bir Kimlik Doğrulama Aracısını kaldırmak istiyorsanız, **Programları ve Özellikleri > Denetim Masası'> na** gidin ve hem Microsoft Azure AD Connect Kimlik **Doğrulaması** Aracısı'nı hem de **Microsoft Azure AD Bağlantı Aracısı Güncelleyici** programlarını kaldırın.

Önceki adımı tamamladıktan sonra [Azure Active Directory yönetici merkezindeki](https://aad.portal.azure.com) Geçiş Kimlik Doğrulama bıçağını kontrol ederseniz, Kimlik Doğrulama Aracısı'nın Etkin **Olmayan**olarak gösterdiğini görürsünüz. Bu _bekleniyor._ Kimlik Doğrulama Aracısı birkaç gün sonra otomatik olarak listeden çıkar.

## <a name="i-already-use-ad-fs-to-sign-in-to-azure-ad-how-do-i-switch-it-to-pass-through-authentication"></a>Azure AD'de oturum açabilmek için AD FS'yi zaten kullanıyorum. Geçiş Kimlik Doğrulamasına nasıl geçiş yapabilirim?

AD FS'den (veya diğer federasyon teknolojilerinden) Geçiş Kimlik Doğrulamasına geçiş yapıyorsunuz, [burada](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx?raw=true)yayınlanan ayrıntılı dağıtım kılavuzumuzu izlemenizi öneririz.

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-active-directory-environment"></a>Pass-through Kimlik Doğrulamasını çok ormanlı Active Directory ortamında kullanabilir miyim?

Evet. Etkin Dizin ormanlarınız arasında orman güvenleri (iki yönlü) varsa ve ad sonekleri yönlendirme doğru şekilde yapılandırılırsa, çok ormanlı ortamlar desteklenir.

## <a name="does-pass-through-authentication-provide-load-balancing-across-multiple-authentication-agents"></a>Geçiş Kimlik Doğrulaması, birden çok Kimlik Doğrulama Aracısı arasında yük dengelemesi sağlar mı?

Hayır, birden çok Geçiş Kimlik Doğrulama Aracısı yüklemek yalnızca [yüksek kullanılabilirlik](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)sağlar. Kimlik Doğrulama Aracıları arasında deterministik yük dengelemesağlamaz. Herhangi bir Kimlik Doğrulama Aracısı (rastgele) belirli bir kullanıcı oturum açma isteğini işleyebilir.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Kaç Tane Geçiş Kimlik Doğrulama Aracısı yüklemem gerekiyor?

Birden çok Geçiş Kimlik Doğrulama Aracısı yüklemek [yüksek kullanılabilirlik](how-to-connect-pta-quick-start.md#step-4-ensure-high-availability)sağlar. Ancak, Kimlik Doğrulama Aracıları arasında deterministic yük dengeleme sağlamaz.

Kiracınızda görmeyi beklediğiniz en yüksek ve ortalama oturum açma istekleri yükünü göz önünde bulundurun. Bir ölçüt olarak, tek bir Kimlik Doğrulama Aracısı standart 4 çekirdekli işlemci, 16 GB RAM sunucusunda saniyede 300 ila 400 kimlik doğrulaması işleyebilir.

Ağ trafiğini tahmin etmek için aşağıdaki boyutlandırma kılavuzunu kullanın:
- Her istek (0,5K + 1K * num_of_agents) bayt taşıma yükü boyutuna sahiptir; örneğin, Azure AD'den Kimlik Doğrulama Aracısı'na gelen veriler. Burada "num_of_agents", kiracınızda kayıtlı Kimlik Doğrulama Aracılarının sayısını gösterir.
- Her yanıtın yük boyutu 1K baytdır; örneğin, Kimlik Doğrulama Aracısı'ndan Azure AD'ye kadar olan veriler.

Çoğu müşteri için, toplam iki veya üç Kimlik Doğrulama Aracısı yüksek kullanılabilirlik ve kapasite için yeterlidir. Oturum açma gecikmesini iyileştirmek için etki alanı denetleyicilerinize yakın Kimlik Doğrulama Aracıları yüklemeniz gerekir.

>[!NOTE]
>Kiracı başına 40 Kimlik Doğrulama Aracısı sistem sınırı vardır.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Azure AD Connect'i çalıştıran sunucudan başka bir sunucuya ilk Geçiş Kimlik Doğrulama Aracısını yükleyebilir miyim?

Hayır, bu senaryo _desteklenmiyor._

## <a name="why-do-i-need-a-cloud-only-global-administrator-account-to-enable-pass-through-authentication"></a>Pass-through Kimlik Doğrulaması'nı etkinleştirmek için neden yalnızca buluta özel bir Global Administrator hesabına ihtiyacım var?

Yalnızca buluta özel Global Administrator hesabı kullanarak Geçiş Kimlik Doğrulaması'nı etkinleştirmeniz veya devre dışı bırakmanız önerilir. Yalnızca [buluta özel Global Administrator hesabı ekleme](../active-directory-users-create-azure-portal.md)hakkında bilgi edinin. Bu şekilde yapmak, kiracınızın dışında kalmamanızı sağlar.

## <a name="how-can-i-disable-pass-through-authentication"></a>Geçiş Kimlik Doğrulaması'nı nasıl devre dışı dışı şıştırabilirim?

Azure AD Connect sihirbazını yeniden çalıştırın ve kullanıcı oturum açma yöntemini Geçiş Kimlik Doğrulama'dan başka bir yönteme değiştirin. Bu değişiklik, kiracıdaki Geçiş Kimlik Doğrulaması'nı devre dışı kılabilir ve Kimlik Doğrulama Aracısını sunucudan yükler. Kimlik Doğrulama Aracılarını diğer sunuculardan el ile kaldırmanız gerekir.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>Geçiş Kimlik Doğrulama Aracısını kaldırdığımda ne olur?

Bir sunucudan Geçiş Kimlik Doğrulama Aracısını yüklerseniz, sunucunun oturum açma isteklerini kabul etmeyi durdurmasına neden olur. Kiracınızdaki kullanıcı oturum açma özelliğini kırmamak için, Geçiş Kimlik Doğrulama Aracısını kaldırmadan önce başka bir Kimlik Doğrulama Aracısı çalıştırdığınızdan emin olun.

## <a name="i-have-an-older-tenant-that-was-originally-setup-using-ad-fs--we-recently-migrated-to-pta-but-now-are-not-seeing-our-upn-changes-synchronizing-to-azure-ad--why-are-our-upn-changes-not-being-synchronized"></a>Ben aslında AD FS kullanarak kurulum eski bir kiracı var.  Yakın zamanda PTA'ya geçiş yaptık, ancak şimdi AZURE AD ile senkronize olan UPN değişikliklerimizi görmüyoruz.  UPN değişikliklerimiz neden senkronize edilmiyor?

C: Aşağıdaki durumlarda şirket içi UPN değişiklikleriniz aşağıdaki durumlarda eşitlemeyebilir:

- Azure AD kiracınız 15 Haziran 2015 tarihinden önce oluşturuldu
- Başlangıçta kimlik doğrulaması için AD FS'yi kullanarak Azure AD kiracınızdan bıkmış oldunuz
- Kimlik doğrulaması olarak PTA'yı kullanarak yönetilen kullanıcılara geçtiniz

Bunun nedeni, kiracıların 15 Haziran 2015 tarihinden önce oluşturulan varsayılan davranışının UPN değişikliklerini engellemek olmasıdır.  UPN değişikliklerini kaldırmanız gerekiyorsa, aşağıdaki PowerShell cmdlt'yi çalıştırmanız gerekir:  

`Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $True`

15 Haziran 2015'ten sonra oluşturulan kiracılar, UPN değişikliklerini eşitleme varsayılan davranışına sahiptir.   



## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md): Hangi senaryoların desteklenip hangilerinin desteklenmediğini öğrenin.
- [Hızlı başlangıç](how-to-connect-pta-quick-start.md): Azure AD Pass-through Authentication'da çalışmaya başlayın.
- [AD FS'den Geçiş Kimlik Doğrulamasına geçiş](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx?raw=true) - AD FS'den (veya diğer federasyon teknolojilerinden) Geçiş Kimlik Doğrulaması'na geçiş için ayrıntılı bir kılavuz.
- [Akıllı Kilitleme](../authentication/howto-password-smart-lockout.md): Kullanıcı hesaplarını korumak için kiracınızdaki Akıllı Kilitleme özelliğini nasıl yapılandırılabildiğini öğrenin.
- [Teknik derin dalış](how-to-connect-pta-how-it-works.md): Geçiş Kimlik Doğrulama özelliğinin nasıl çalıştığını anlayın.
- [Sorun Giderme](tshoot-connect-pass-through-authentication.md): Geçiş Kimlik Doğrulama özelliğiyle ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [Güvenlik derin dalış](how-to-connect-pta-security-deep-dive.md): Pass-through Kimlik Doğrulama özelliği hakkında derin teknik bilgi alın.
- [Azure AD Sorunsuz SSO](how-to-connect-sso.md): Bu tamamlayıcı özellik hakkında daha fazla bilgi edinin.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Yeni özellik isteklerini dosyalamak için Azure Active Directory Forum'u kullanın.

