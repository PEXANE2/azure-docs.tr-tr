---
title: Self servis parola sıfırlama SSS - Azure Etkin Dizin
description: Azure AD self servis parola sıfırlama hakkında sık sorulan sorular
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/15/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: d1246b5b980f60c2f3e65aa5b32a7d79dd6efc7d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81407169"
---
# <a name="password-management-frequently-asked-questions"></a>Parola yönetimi sık sorulan sorular

Parola sıfırlama ile ilgili her şey için sık sorulan bazı sorular (SSS) aşağıda veda edebilirsiniz.

Azure Etkin Dizini (Azure AD) ve self servis parola sıfırlama (SSPR) hakkında burada yanıtlanmamış genel bir sorunuz varsa, [Azure AD forumunda](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD)toplulukten yardım isteyebilirsiniz. Topluluğun üyeleri arasında mühendisler, ürün yöneticileri, MVP'ler ve diğer BT profesyonelleri yer almaktadır.

Bu SSS aşağıdaki bölümlere ayrılmıştır:

* [Parola sıfırlama kaydı ile ilgili sorular](#password-reset-registration)
* [Parola sıfırlama ile ilgili sorular](#password-reset)
* [Parola değişikliği ile ilgili sorular](#password-change)
* [Parola yönetimi raporları yla ilgili sorular](#password-management-reports)
* [Şifre yazma ile ilgili sorular](#password-writeback)

## <a name="password-reset-registration"></a>Parola sıfırlama kaydı

* **S: Kullanıcılarım kendi parola sıfırlama verilerini kaydedebilir mi?**

  > **Y:** Evet. Parola sıfırlama etkinleştirildikleri ve lisans aldıkları sürece, kullanıcılar kimlik doğrulama bilgilerinihttps://aka.ms/ssprsetup) kaydetmek için parola sıfırlama kayıt portalına gidebilirler. Kullanıcılar ayrıca Access Panelhttps://myapps.microsoft.com)(. Access Paneli üzerinden kaydolmak için profil resimlerini seçmeleri, **Profil'i**seçmeleri ve ardından parola sıfırlama seçeneği **için Kaydol** seçeneğini seçmeleri gerekir.
  >
  >
* **S: Bir grup için parola sıfırlamayı etkinleştirip herkes için etkinleştirmeye karar verirsem kullanıcılarımın yeniden kaydolması gerekir mi?**

  > **A:** №. Kimlik doğrulama verilerini dolduran kullanıcıların yeniden kaydolması gerekmez.
  >
  >
* **S: Kullanıcılarım adına parola sıfırlama verilerini tanımlayabilir miyim?**

  > **A:** Evet, bunu Azure AD Connect, PowerShell, [Azure portalı](https://portal.azure.com)veya Microsoft [365 yönetici merkezi](https://admin.microsoft.com)ile yapabilirsiniz. Daha fazla bilgi için Azure [AD self servis parola sıfırlama tarafından kullanılan Verilere](howto-sspr-authenticationdata.md)bakın.
  >
  >
* **S: Şirket içi güvenlik soruları için verileri senkronize edebilir miyim?**

  > **A:** Hayır, bugün bu mümkün değil.
  >
  >
* **S: Kullanıcılarım verileri diğer kullanıcıların bu verileri göremeyecek şekilde kaydedebilir mi?**

  > **Y:** Evet. Kullanıcılar parola sıfırlama kayıt portalını kullanarak verileri kaydettirdiğinde, veriler yalnızca genel yöneticiler ve kullanıcı tarafından görülebilen özel kimlik doğrulama alanlarına kaydedilir.
  >
  >
* **S: Kullanıcılarımın parola sıfırlamayı kullanabilmeleri için kaydolmaları gerekiyor mu?**

  > **A:** №. Onların adına yeterli kimlik doğrulama bilgisi tanımlarsanız, kullanıcıların kaydolması gerekmez. Parola sıfırlama, dizindeki uygun alanlarda depolanan verileri düzgün biçimlendirdiğiniz sürece çalışır.
  >
  >
* **S: Kullanıcılarım adına kimlik doğrulama telefonunu, kimlik doğrulama e-postasını veya alternatif kimlik doğrulama telefon alanlarını senkronize edebilir veya ayarlayabilir miyim?**

  > **A:** Bir Global Yönetici tarafından ayarlanabilen alanlar, [SSPR Veri gereksinimleri](howto-sspr-authenticationdata.md)makalesinde tanımlanır.
  >
  >
* **S: Kayıt portalı kullanıcılarımı hangi seçenekleri göstereceğimi nasıl belirler?**

  > **A:** Parola sıfırlama kayıt portalı yalnızca kullanıcılarınız için etkinleştirdiğiniz seçenekleri gösterir. Bu seçenekler, dizininizin **Yapılandırma** sekmesinin **Kullanıcı Parolası Sıfırlama İlkesi** bölümünde bulunur. Örneğin, güvenlik sorularını etkinleştirmezseniz, kullanıcılar bu seçenek için kaydolamaz.
  >
  >
* **S: Bir kullanıcı ne zaman kayıtlı olarak kabul edilir?**

  > **A:** Bir kullanıcı, [Azure portalında](https://portal.azure.com)belirlediğiniz bir parolayı **sıfırlamak için gereken yöntem sayısını** en az kaydetmişse SSPR'ye kayıtlı olarak kabul edilir.
  >
  >

## <a name="password-reset"></a>Parola sıfırlama

* **S: Kullanıcıların kısa bir süre içinde parolayı sıfırlamak için birden çok denemeyapmasını engelliyor musunuz?**

  > **A:** Evet, kötüye kullanımdan korumak için parola sıfırlama yerleşik güvenlik özellikleri vardır. 
  >
  > Kullanıcılar, 24 saat boyunca kilitlendirilmeden önce 24 saatlik bir süre içinde yalnızca beş parola sıfırlama denemesi deneyebilir. 
  >
  > Kullanıcılar bir telefon numarasını doğrulamayı, SMS göndermeyi veya güvenlik sorularını ve yanıtlarını 24 saat boyunca kilitlendirilmeden önce yalnızca beş kez doğrulamayı deneyebilir. 
  >
  > Kullanıcılar, 24 saat boyunca kilitlendirilmeden önce 10 dakikalık bir süre içinde en fazla 10 kez e-posta gönderebilir.
  >
  > Sayaçlar, kullanıcı parolasını sıyrıkla sıfırlanır.
  >
  >
* **S: Parola sıfırlamadan e-posta, SMS veya telefon görüşmesi almak için ne kadar beklemeliyim?**

  > **A:** E-postalar, SMS mesajları ve telefon görüşmeleri bir dakikadan kısa bir süre içinde gelmelidir. Normal durum 5 ila 20 saniye.
  > Bu zaman diliminde bildirimi almazsanız:
  > * Önemsiz klasörünüzü kontrol edin.
  > * İletişim e-postayla iletişime geçilen numaranın veya e-postanın beklediğiniz numara olup olmadığını kontrol edin.
  > * Dizindeki kimlik doğrulama verilerinin doğru biçimlendirilmiş olup olmadığını denetleyin, örneğin, +1 4255551234 veya *\@kullanıcı contoso.com.* 
* **S: Parola sıfırlama ile hangi diller desteklenir?**

  > **A:** Parola sıfırlama UI, SMS iletileri ve sesli aramalar Office 365'te desteklenen dillerde yerelleştirilmiştir.
  >
  >
* **S: Dizinin yapılandırma sekmesinde kuruluş marka öğelerini ayarladığınızda parola sıfırlama deneyiminin hangi bölümleri markalanır?**

  > **A:** Parola sıfırlama portalı kuruluşunuzun logosunu gösterir ve özel bir e-posta veya URL'ye işaret etmek için "Yöneticinizle iletişim kurun" bağlantısını yapılandırmanıza olanak tanır. Parola sıfırlama yoluyla gönderilen tüm e-postalar, kuruluşunuzun logosunu, renklerini ve adını e-postanın gövdesine içerir ve bu ada ait ayarlardan özelleştirilmiştir.
  >
  >
* **S: Kullanıcılarımı parolalarını sıfırlamak için nereye gidecekleri konusunda nasıl eğitebilirim?**

  > **A:** [SSPR dağıtım](howto-sspr-deployment.md#plan-communications) makalemizdeki önerilerden bazılarını deneyin.
  >
  >
* **S: Bu sayfayı bir mobil cihazdan kullanabilir miyim?**

  > **A:** Evet, bu sayfa mobil cihazlarda çalışır.
  >
  >
* **S: Kullanıcılar parolalarını sıyrırken yerel Active Directory hesaplarının kilidini açmayı destekliyor musunuz?**

  > **Y:** Evet. Bir kullanıcı parolalarını sıfırladığında, Azure AD Connect üzerinden parola yazma dağıtımı yapıldıysa, parolalarını sıyrırken kullanıcının hesabıotomatik olarak açılır.
  >
  >
* **S: Parola sıfırlamayı doğrudan kullanıcımın masaüstü oturum açma deneyimine nasıl entegre edebilirim?**

  > **A:** Azure AD Premium müşterisiyseniz, Microsoft Identity Manager'ı ek ücret ödemeden yükleyebilir ve şirket içi parola sıfırlama çözümlerini dağıtabilirsiniz.
  >
  >
* **S: Farklı yerel ayarlar için farklı güvenlik soruları ayarlayabilir miyim?**

  > **A:** Hayır, bugün bu mümkün değil.
  >
  >
* **S: Güvenlik soruları kimlik doğrulama seçeneği için kaç soru yapılandırabilirim?**

  > **A:** [Azure portalında](https://portal.azure.com)en fazla 20 özel güvenlik sorusu yapılandırabilirsiniz.
  >
  >
* **S: Güvenlik soruları ne kadar süreyle olabilir?**

  > **A:** Güvenlik soruları 3 ila 200 karakter uzunluğunda olabilir.
  >
  >
* **S: Güvenlik sorularının yanıtları ne kadar olabilir?**

  > **A:** Yanıtlar 3 ila 40 karakter uzunluğunda olabilir.
  >
  >
* **S: Güvenlik sorularının yinelenen yanıtları reddedildi mi?**

  > **A:** Evet, güvenlik sorularının yinelenen yanıtlarını reddediyoruz.
  >
  >
* **S: Bir kullanıcı aynı güvenlik sorusunu birden fazla kez kaydedebilir mi?**

  > **A:** №. Bir kullanıcı belirli bir soruyu kaydettikten sonra, bu soruya ikinci kez kaydolamaz.
  >
  >
* **S: Kayıt ve sıfırlama için minimum güvenlik sorusu sınırı belirlemek mümkün mü?**

  > **A:** Evet, bir sınır kayıt için ayarlanabilir ve diğeri sıfırlamak için. Kayıt için üç ila beş güvenlik sorusu, sıfırlamak için ise üç ila beş soru gerekebilir.
  >
  >
* **S: İlkemi, kullanıcıların sıfırlama kullanabilirsiniz güvenlik sorularını kullanmasını gerektirecek şekilde yapılandırıldım, ancak Azure yöneticileri farklı şekilde yapılandırılmış gibi görünüyor.**

  > **A:** Bu beklenen davranıştır. Microsoft, tüm Azure yöneticisi rolleri için varsayılan olarak güçlü ve iki aşamalı parola sıfırlama ilkesi uygular. Bu, yöneticilerin güvenlik sorularını kullanmasını engeller. Bu ilke hakkında daha fazla bilgiyi [Azure Active Directory makalesinde Parola ilkeleri ve kısıtlamalarında](concept-sspr-policy.md) bulabilirsiniz.
  >
  >
* **S: Bir kullanıcı sıfırlamak için gereken maksimum soru sayısından daha fazla sayıda niçin kaydettirdiyse, sıfırlama sırasında güvenlik soruları nasıl seçilir?**

  > **C:** *N* sayısı, kullanıcının kaydolduğu toplam soru sayısından rastgele seçilir ve *N,* sıfırlama seçeneği için **gereken soru sayısı** için ayarlanan tutardır. Örneğin, bir kullanıcı beş güvenlik sorusu kaydettiyse, ancak parolayı sıfırlamak için yalnızca üç soru gerekiyorsa, beş sorudan üçü rasgele seçilir ve sıfırlama sırasında sunulur. Soru çekiçleme önlemek için, kullanıcı yanlış soruların yanıtlarını alırsa seçim süreci baştan başlar.
  >
  >
* **S: E-posta ve SMS tek seferlik şifreler ne kadar süreyle geçerlidir?**

  > **A:** Parola sıfırlama için oturum ömrü 15 dakikadır. Parola sıfırlama işleminin başlangıcından itibaren, kullanıcının parolasını sıfırlamak için 15 dakikası vardır. Parola sıfırlama oturumu sırasında e-posta ve SMS tek seferlik parola 5 dakika süreyle geçerlidir.
  >
  >
* **S: Kullanıcıların parolalarını sıfırlamalarını engelleyebilir miyim?**

  > **A:** Evet, SSPR'yi etkinleştirmek için bir grup kullanıyorsanız, tek bir kullanıcıyı kullanıcıların parolalarını sıfırlamasına izin veren gruptan kaldırabilirsiniz. Kullanıcı Bir Global Administrator ise parolalarını sıfırlama yeteneğini korur ve bu devre dışı bırakılamaz.
  >
  >

## <a name="password-change"></a>Parola değiştirme

* **S: Kullanıcılarım parolalarını değiştirmek için nereye gitmeli?**

  > **A:** Kullanıcılar parolalarını profil resimlerini veya simgelerini gördükleri her [yerde(Office 365](https://portal.office.com) portalının veya [Access Panel](https://myapps.microsoft.com) deneyimlerinin sağ üst köşesinde olduğu gibi) değiştirebilirler. Kullanıcılar Parolalarını [Access Panel Profili sayfasından](https://account.activedirectory.windowsazure.com/r#/profile)değiştirebilirler. Kullanıcılardan, parolalarının süresi dolmuşsa, Azure AD oturum açma sayfasında parolalarını otomatik olarak değiştirmeleri de istenebilir. Son olarak, kullanıcılar parolalarını değiştirmek istiyorlarsa doğrudan [Azure AD parola değiştirme portalına](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) göz atabilir.
  >
  >
* **S: Kullanıcılarım şirket içi parolalarının süresi dolduğunda Office portalında bilgilendirilebilir mi?**

  > **A:** Evet, Active Directory Federation Services (AD FS) kullanıyorsanız, bu bugün mümkündür. AD FS kullanıyorsanız, AD FS makalesi [ile parola ilke talepleri gönderme](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396) yönergelerini izleyin. Parola karma eşitleme kullanıyorsanız, bu bugün mümkün değildir. Parola ilkelerini şirket içi dizinlerden eşitlemiyorum, bu nedenle bulut deneyimlerine son kullanma bildirimleri yayınlamamız mümkün değildir. Her iki durumda da, [parolaları PowerShell üzerinden sona ermek üzere olan kullanıcıları bilgilendirmek de mümkündür.](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx)
  >
  >
* **S: Kullanıcıların parolalarını değiştirmelerini engelleyebilir miyim?**

  > **A:** Yalnızca bulut kullanıcıları için parola değişiklikleri engellenemez. Şirket içi kullanıcılar **için, Kullanıcı'nın parola** seçeneğini seçili olarak değiştiremeyeceğini ayarlayabilirsiniz. Seçili kullanıcılar parolalarını değiştiremez.
  >
  >

## <a name="password-management-reports"></a>Parola yönetimi raporları

* **S: Verilerin parola yönetimi raporlarında gösterilmesi ne kadar sürer?**

  > **A:** Veriler 5 ila 10 dakika içinde parola yönetimi raporlarında görünmelidir. Bazı durumlarda, görünmesi bir saat kadar sürebilir.
  >
  >
* **S: Parola yönetimi raporlarını nasıl filtreleyebilirim?**

  > **A:** Parola yönetimi raporlarını filtrelemek için, raporun üst kısmındaki sütun etiketlerinin en sağındaki küçük büyüteç'i seçin. Daha zengin filtreleme yapmak istiyorsanız, raporu Excel'e indirebilir ve bir özet tablo oluşturabilirsiniz.
  >
  >
* **S: Parola yönetimi raporlarında depolanan maksimum olay sayısı nedir?**

  > **A:** 75.000'e kadar parola sıfırlama veya parola sıfırlama kaydı olayları, 30 güne kadar uzatır ve parola yönetim raporlarında depolanır. Bu sayıyı daha fazla etkinlik içerecek şekilde genişletmek için çalışıyoruz.
  >
  >
* **S: Parola yönetimi raporları ne kadar geriye gider?**

  > **A:** Parola yönetimi raporları, son 30 gün içinde gerçekleşen işlemleri gösterir. Şimdilik, bu verileri arşivlemeniz gerekiyorsa, raporları düzenli aralıklarla indirebilir ve ayrı bir konuma kaydedebilirsiniz.
  >
  >
* **S: Parola yönetimi raporlarında görünebilecek en fazla satır sayısı var mı?**

  > **Y:** Evet. UI'de gösterilse ler veya karşıdan yüklenirler.
  >
  >
* **S: Parola sıfırlama veya kayıt raporlama verilerine erişmek için bir API var mı?**

  > **Y:** Evet. Parola sıfırlama raporlama verilerine nasıl erişebileceğinizi öğrenmek için [Azure Log Analytics REST API Başvurusu'na](/rest/api/loganalytics/)bakın.
  >
  >

## <a name="password-writeback"></a>Parola geri yazma

* **S: Parola geri yazma arka planda nasıl çalışır?**

  > **A:** Makaleye bakın Parola yazma, parola geri yazmayı etkinleştirdiğinizde ne olacağını ve verilerin sistem içinde şirket içi ortamınıza nasıl aktığını açıklamak için [nasıl çalışır?](howto-sspr-writeback.md)
  >
  >
* **S: Parola geri yazma çalışması ne kadar sürer? Parola karma eşitleme ile olduğu gibi bir eşitleme gecikmesi var mı?**

  > **A:** Şifre yazma anında. Parola karma eşitlemeden temelde farklı çalışan eşzamanlı bir ardışık ardışık ardışık ardışık yapıdır. Parola yazma, kullanıcıların parola sıfırlama veya değiştirme işlemlerinin başarısı hakkında gerçek zamanlı geri bildirim almalarına olanak tanır. Bir parolanın başarılı bir şekilde yazılması için ortalama süre 500 ms'nin altındadır.
  >
  >
* **S: Şirket içi hesabım devre dışı bırakılırsa, bulut hesabım ve erişimim nasıl etkilenir?**

  > **A:** Şirket içi kimliğiniz devre dışı bırakılırsa, Azure AD Connect aracılığıyla bir sonraki eşitleme aralığında bulut kimliğiniz ve erişiminiz de devre dışı bırakılır. Varsayılan olarak, bu eşitleme her 30 dakikada bir.
  >
  >
* **S: Şirket içi hesabım şirket içi Active Directory parola ilkesiyle kısıtlanmışsa, parolamı değiştirdiğimde SSPR bu ilkeye uyuyor mu?**

  > **A:** Evet, SSPR şirket içi Active Directory parola ilkesine dayanır ve buna uyar. Bu ilke, tipik Active Directory etki alanı parola ilkesinin yanı sıra bir kullanıcıyı hedefleyen tanımlanmış, ince taneli parola ilkelerini de içerir.
  >
  >
* **S: Parola geri yazma ne tür hesaplar için çalışır?**

  > **A:** Parola yazma, şirket içi Active Directory'den Azure AD'ye eşitlenen kullanıcı hesapları için federe, parola karma senkronize edilmiş ve Geçiş Autentication Kullanıcıları gibi çalışır.
  >
  >
* **S: Parola geri yazma etki alanımdaki parola ilkelerini zorlar mı?**

  > **Y:** Evet. Parola geri yazma, parola yaşını, geçmişini, karmaşıklığı, filtreleri ve yerel etki alanınızdaki parolalara koyabileceğiniz diğer tüm kısıtlamaları zorlar.
  >
  >
* **S: Parola yazma güvenli midir?  Saldırıya uğramayacağımdan nasıl emin olabilirim?**

  > **A:** Evet, şifre yazma güvenlidir. Parola yazma hizmeti tarafından uygulanan birden çok güvenlik katmanı hakkında daha fazla bilgi için, [Parola geri yazma genel bakış](howto-sspr-writeback.md) makalesindeki Parola geri [yazma güvenlik](concept-sspr-writeback.md#password-writeback-security) bölümüne göz atın.
  >
  >

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md)
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md)
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’de etkinliği nasıl bildirebilirim?](howto-sspr-reporting.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Sanırım bir şey kırıldı. SSPR'yi nasıl giderebilirim?](active-directory-passwords-troubleshoot.md)
