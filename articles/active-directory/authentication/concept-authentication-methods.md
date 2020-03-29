---
title: Kimlik doğrulama yöntemleri - Azure Etkin Dizini
description: MFA ve SSPR için Azure AD'da kullanılabilir kimlik doğrulama yöntemleri
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5a82c69575e82a7cf397955f08c3f114e449ba6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968783"
---
# <a name="what-are-authentication-methods"></a>Kimlik doğrulaması yöntemleri nelerdir?

Bir yönetici olarak, Azure Çok Faktörlü Kimlik Doğrulama ve self servis parola sıfırlama (SSPR) için kimlik doğrulama yöntemlerini seçerken, kullanıcıların birden çok kimlik doğrulama yöntemini kaydetmesini beklemeniz önerilir. Bir kimlik doğrulama yöntemi bir kullanıcı için kullanılamıyorsa, başka bir yöntemle kimlik doğrulaması yapmayı seçebilirler.

Yöneticiler ilkede hangi kimlik doğrulama yöntemlerinin SSPR ve MFA kullanıcıları tarafından kullanılabildiği açıklanabilir. Bazı kimlik doğrulama yöntemleri tüm özellikler için kullanılamayabilir. İlkelerinizi yapılandırma hakkında daha fazla bilgi için makalelere bakın Self servis parola sıfırlama yı [başarıyla nasıl kullanıma sunulursunuz](howto-sspr-deployment.md) ve [bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması Planlama](howto-mfa-getstarted.md)

Microsoft, Yöneticilerin, kullanıcıların bir tanesine erişemedikleri durumlarda gereken en az sayıdan daha fazla kimlik doğrulama yöntemi seçmelerini sağlamalarını şiddetle önerir.

|Kimlik Doğrulama Yöntemi|Kullanım|
| --- | --- |
| Parola | MFA ve SSPR |
| Güvenlik soruları | Yalnızca SSPR |
| E-posta adresi | Yalnızca SSPR |
| Microsoft Authenticator uygulaması | MFA ve SSPR |
| OATH Donanım belirteci | MFA ve SSPR için genel önizleme |
| SMS | MFA ve SSPR |
| Sesli arama | MFA ve SSPR |
| Uygulama parolaları | MFA sadece bazı durumlarda |

![Oturum açma ekranında kullanılan kimlik doğrulama yöntemleri](media/concept-authentication-methods/overview-login.png)

|     |
| --- |
| MFA ve SSPR için OATH Donanım belirteçleri Azure Active Directory'nin genel önizleme özellikleridir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın|
|     |

## <a name="password"></a>Parola

Azure AD parolanız kimlik doğrulama yöntemi olarak kabul edilir. **Devre dışı edilemeyen**tek yöntemdir.

## <a name="security-questions"></a>Güvenlik soruları

Güvenlik soruları yalnızca **Azure AD self servis parola sıfırlamada** yönetici olmayan hesaplarda kullanılabilir.

Güvenlik sorularını kullanıyorsanız, bunları başka bir yöntemle birlikte kullanmanızı öneririz. Bazı kişiler başka bir kullanıcının sorularının yanıtlarını bilebilir, çünkü güvenlik soruları diğer yöntemlere göre daha az güvenli olabilir.

> [!NOTE]
> Güvenlik soruları dizindeki bir kullanıcı nesnesinde özel ve güvenli bir şekilde depolanır ve yalnızca kayıt sırasında kullanıcılar tarafından yanıtlanabilir. Bir yöneticinin kullanıcının sorularını veya yanıtlarını okuması veya değiştirmesi hiçbir yol yoktur.
>

### <a name="predefined-questions"></a>Önceden tanımlanmış sorular

* İlk eşinizle/eşinizle hangi şehirde tanıştınız?
* Ailen hangi şehirde tanıştı?
* En yakın kardeşiniz hangi şehirde yaşıyor?
* Baban hangi şehirde doğdu?
* İlk işin hangi şehirdeydi?
* Annen hangi şehirde doğdu?
* 2000'de hangi şehirdeydin?
* Lisedeki en sevdiğin öğretmenin soyadı ne?
* Başvurduğunuz ama katılmadığınız bir üniversitenin adı nedir?
* İlk düğün ünüzü yaptığınız yerin adı nedir?
* Babanın göbek adı ne?
* En sevdiğin yemek nedir?
* Anneannenin adı ve soyadı nedir?
* Annenin göbek adı ne?
* En büyük kardeşinin doğum günü ayı ve yılı nedir? (örneğin Kasım 1985)
* En büyük kardeşinin göbek adı ne?
* Babanızın adı ve soyadı nedir?
* En küçük kardeşinin göbek adı ne?
* Altıncı sınıfta hangi okula gittin?
* Çocukluk en iyi arkadaşının adı neydi?
* İlk önemli diğerinin adı ve soyadı neydi?
* En sevdiğin ilkokul öğretmeninin soyadı neydi?
* İlk arabanızın veya motosikletinizin matve tiyabı neydi?
* Gittiğin ilk okulun adı neydi?
* Doğduğun hastanenin adı neydi?
* İlk çocukluk evinizin sokağının adı neydi?
* Çocukluk kahramanının adı neydi?
* En sevdiğin doldurulmuş hayvanın adı neydi?
* İlk evcil hayvanınızın adı neydi?
* Çocukluk lakabın neydi?
* Lisede en sevdiğin spor hangisiydi?
* İlk işin neydi?
* Çocukluk telefon numaranızın son dört hanesi neydi?
* Gençken, büyüyünce ne olmak isterdin?
* Tanıştığın en ünlü insan kim?

Önceden tanımlanmış tüm güvenlik soruları, kullanıcının tarayıcı yerellesine göre tam Office 365 dil kümesine çevrilir ve yerelleştirilir.

### <a name="custom-security-questions"></a>Özel güvenlik soruları

Özel güvenlik soruları yerelleştirilmez. Tüm özel sorular, kullanıcının tarayıcı yerel bilgisayarı farklı olsa bile, yönetim kullanıcı arabirimine girilen lerle aynı dilde görüntülenir. Yerelleştirilmiş sorulara ihtiyacınız varsa, önceden tanımlanmış soruları kullanmalısınız.

Özel güvenlik sorusunun maksimum uzunluğu 200 karakterdir.

### <a name="security-question-requirements"></a>Güvenlik sorusu gereksinimleri

* Minimum yanıt karakter sınırı üç karakterdir.
* En yüksek yanıt karakter sınırı 40 karakterdir.
* Kullanıcılar aynı soruya birden fazla kez yanıt veremez.
* Kullanıcılar birden fazla soruya aynı yanıtı veremez.
* Herhangi bir karakter kümesi, Unicode karakterleri de dahil olmak üzere soruları ve yanıtları tanımlamak için kullanılabilir.
* Tanımlanan soru sayısı, kaydolmak için gereken soru sayısından büyük veya eşit olmalıdır.

## <a name="email-address"></a>E-posta adresi

E-posta adresi **yalnızca Azure AD self servis parola sıfırlama da**kullanılabilir.

Microsoft, kullanıcının Azure AD parolasına erişmesi gerekmeyen bir e-posta hesabı kullanılmasını önerir.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması

Microsoft Authenticator uygulaması, Azure REKLAM çalışmanıza veya okul hesabınıza veya Microsoft hesabınıza ek bir güvenlik düzeyi sağlar.

Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) ve [Windows Phone](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6)'da kullanılabilir.

> [!NOTE]
> Kullanıcılar self servis şifre sıfırlama için kayıt olurken mobil uygulamalarını kaydetme seçeneğine sahip olmayacaktır. Bunun yerine, kullanıcılar mobil [https://aka.ms/mfasetup](https://aka.ms/mfasetup) uygulamalarını güvenlik bilgileri kaydı [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo)önizlemesinde veya adresinden kaydedebilirler.
>

### <a name="notification-through-mobile-app"></a>Mobil uygulama üzerinden bildirim

Microsoft Authenticator uygulaması, akıllı telefonunuza veya tabletinize bir bildirim göndererek hesaplara yetkisiz erişimi önlemeye ve sahte işlemleri durdurmaya yardımcı olabilir. Kullanıcılar bildirimi görüntüleyin ve yasalsa Doğrula'yı seçin. Aksi takdirde, Reddet'i seçebilirler.

> [!WARNING]
> Sıfırlama için yalnızca bir yöntem gerektiğinde self servis parola sıfırlama için doğrulama kodu, en **yüksek güvenlik düzeyini sağlamak için**kullanıcılar için kullanılabilen tek seçenektir.
>
> İki yöntem gerektiğinde, kullanıcılar diğer etkin yöntemlere ek olarak **YA** bildirim **veya** doğrulama kodunu kullanarak sıfırlanabilirler.
>

Mobil uygulamadan hem bildirim hem de doğrulama kodu aracılığıyla bildirimin kullanılmasını etkinleştiriseniz, microsoft authenticator uygulamasını bir bildirim kullanarak kaydeden kullanıcılar kimliklerini doğrulamak için hem bildirimi hem de kodu kullanabilir.

> [!NOTE]
> Kuruluşunuzun Çin'de çalışan veya Çin'e seyahat eden personeli varsa, **Android cihazlardaki** **mobil uygulama yöntemiyle bildirim** o ülkede çalışmaz. Bu kullanıcılar için alternatif yöntemler kullanılabilir hale getirilmelidir.

### <a name="verification-code-from-mobile-app"></a>Mobil uygulamadan doğrulama kodu

Microsoft Authenticator uygulaması veya diğer üçüncü taraf uygulamaları, bir OATH doğrulama kodu oluşturmak için yazılım belirteci olarak kullanılabilir. Kullanıcı adınızı ve şifrenizi girdikten sonra, uygulamatarafından sağlanan kodu oturum açma ekranına girersiniz. Doğrulama kodu, ikinci bir kimlik doğrulama biçimi sağlar.

> [!WARNING]
> Sıfırlama doğrulama kodu için tek bir yöntem gerektiğinde self servis parola sıfırlama için en **yüksek güvenlik düzeyini sağlamak için**kullanıcılar için kullanılabilen tek seçenektir.
>

Kullanıcılar, herhangi bir zamanda kullanılmak üzere yapılandırılan Microsoft Authenticator uygulaması gibi en fazla beş OATH donanım belirteci veya kimlik doğrulayıcı uygulamasının bir birleşimine sahip olabilir.

## <a name="oath-hardware-tokens-public-preview"></a>OATH donanım belirteçleri (genel önizleme)

OATH, tek seferlik parola (OTP) kodlarının nasıl oluşturulduğunu belirten açık bir standarttır. Azure AD, 30 saniyelik veya 60 saniyelik çeşitli yemin-TOTP SHA-1 belirteçlerinin kullanımını destekler. Müşteriler bu belirteçleri kendi seçtikleri satıcıdan temin edebilirler. Gizli anahtarlar, tüm belirteçlerle uyumlu olmayan 128 karakterle sınırlıdır. Gizli anahtar yalnızca *a-z* veya *A-Z* ve *1-7*rakamlarını içerebilir ve Base32'de kodlanmalıdır.

![MFA OATH belirteçleri bıçak yemin belirteçleri yükleme](media/concept-authentication-methods/mfa-server-oath-tokens-azure-ad.png)

OATH donanım belirteçleri genel önizlemenin bir parçası olarak desteklenir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın

Belirteçler alındıktan sonra, aşağıdaki örnekte gösterildiği gibi UPN, seri numarası, gizli anahtar, zaman aralığı, üretici ve modeli içeren virgülle ayrılmış değerler (CSV) dosya biçiminde yüklenmelidir:

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567abcdef1234567abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Üstbilgi satırını CSV dosyanıza eklediğinizden emin olun.

Bir csv dosyası olarak düzgün biçimlendirdikten sonra, yönetici Azure portalında oturum açabilir, **Azure Active Directory** > **Security** > **MFA** > **OATH belirteçlerine**gidebilir ve ortaya çıkan CSV dosyasını yükleyebilir.

CSV dosyasının boyutuna bağlı olarak, işlem birkaç dakika sürebilir. Geçerli durumu almak için **Yenile** düğmesini tıklatın. Dosyada herhangi bir hata varsa, çözmeniz gereken hataları listeleyen bir CSV dosyasını indirme seçeneğiniz olacaktır. İndirilen CSV dosyasındaki alan adları yüklenen sürümden farklıdır.

Herhangi bir hata giderildikten sonra, yönetici belirteç etkinleştirmek için **Etkinleştir'i** tıklatarak ve belirteç üzerinde görüntülenen OTP'yi girerek her anahtarı etkinleştirebilir.

Kullanıcılar, herhangi bir zamanda kullanılmak üzere yapılandırılan Microsoft Authenticator uygulaması gibi en fazla beş OATH donanım belirteci veya kimlik doğrulayıcı uygulamasının bir birleşimine sahip olabilir.

## <a name="phone-options"></a>Telefon seçenekleri

### <a name="mobile-phone"></a>Cep telefonu

Cep telefonu olan kullanıcılar için iki seçenek mevcuttur.

Kullanıcılar cep telefonu numaralarının dizinde görünür olmasını istemiyorsa, ancak parola sıfırlama için kullanmak istiyorlarsa, yöneticiler bu numarayı dizinde doldurmamalıdır. Kullanıcılar **kimlik doğrulama telefon** özniteliğini [parola sıfırlama kayıt portalı](https://aka.ms/ssprsetup)üzerinden doldurmalıdır. Yöneticiler bu bilgileri kullanıcının profilinde görebilir, ancak başka bir yerde yayınlanmaz.

Düzgün çalışabilmek için telefon numaraları *+CountryCode Telefon Numarası +1*4255551234 biçiminde olmalıdır.

> [!NOTE]
> Ülke kodu ile telefon numarası arasında bir boşluk olması gerekiyor.
>
> Parola sıfırlama telefon uzantılarını desteklemez. +1 4255551234X12345 biçiminde bile, arama yerleştirilmeden önce uzantılar kaldırılır.

Microsoft, aynı numarayla tutarlı SMS veya Ses tabanlı Çok Faktörlü Kimlik Doğrulama istemi teslimini garanti etmez. Kullanıcılarımızın yararına, SMS gönderimini geliştirmek için rota ayarlamaları yaptığımızda Microsoft, herhangi bir zamanda Kısa kodlar ekleyebilir veya kaldırabilir. Microsoft, ABD ve Kanada dışında ülkeler/bölgeler için kısa kodları desteklemez.

#### <a name="text-message"></a>Kısa mesaj

Doğrulama kodu içeren cep telefonu numarasına BIR SMS gönderilir. Devam etmek için oturum açma arabiriminde sağlanan doğrulama kodunu girin.

#### <a name="phone-call"></a>Telefon görüşmesi

Sağladığınız telefon numarasına otomatik sesli arama yapılır. Aramayı yanıtlayın ve kimlik doğrulaması için telefon tuş takımıyla # tuşuna basın

> [!IMPORTANT]
> Mart 2019'dan itibaren telefon görüşmesi seçenekleri, ücretsiz/deneme Azure AD kiracılarında MFA ve SSPR kullanıcıları tarafından kullanılamaz. SMS mesajları bu değişiklikle etkilenmez. Telefon görüşmesi, ücretli Azure AD kiracılarında bulunan kullanıcılar tarafından kullanılabilir olmaya devam edecektir. Bu değişiklik yalnızca ücretsiz/deneme Azure AD kiracılarını etkiler.

### <a name="office-phone"></a>Ofis telefonu

Sağladığınız telefon numarasına otomatik sesli arama yapılır. Aramayı yanıtlayın ve doğrulamak için telefon tuş takımı # tuşuna basın.

Düzgün çalışabilmek için telefon numaraları *+CountryCode Telefon Numarası +1*4255551234 biçiminde olmalıdır.

Ofis telefonu özniteliği yöneticiniz tarafından yönetilir.

> [!IMPORTANT]
> Mart 2019'dan itibaren telefon görüşmesi seçenekleri, ücretsiz/deneme Azure AD kiracılarında MFA ve SSPR kullanıcıları tarafından kullanılamaz. SMS mesajları bu değişiklikle etkilenmez. Telefon görüşmesi, ücretli Azure AD kiracılarında bulunan kullanıcılar tarafından kullanılabilir olmaya devam edecektir. Bu değişiklik yalnızca ücretsiz/deneme Azure AD kiracılarını etkiler.

> [!NOTE]
> Ülke kodu ile telefon numarası arasında bir boşluk olması gerekiyor.
>
> Parola sıfırlama telefon uzantılarını desteklemez. +1 4255551234X12345 biçiminde bile, arama yerleştirilmeden önce uzantılar kaldırılır.

### <a name="troubleshooting-phone-options"></a>Telefon seçenekleriyle ilgili sorunları giderme

Telefon numarası kullanarak kimlik doğrulama yöntemleriyle ilgili sık karşılaşılan sorunlar:

* Tek bir cihazda engellenen arayan kimliği
   * Sorun giderme aygıtı
* Yanlış telefon numarası, yanlış ülke kodu, ev telefonu numarası karşı iş telefon numarası
   * Kullanıcı nesnesi ve yapılandırılmış kimlik doğrulama yöntemlerisorun giderme. Doğru telefon numaralarının kayıtlı olduğundan emin olun.
* Yanlış PIN girilen
   * Kullanıcının Azure MFA Server'da kayıtlı doğru PIN'i kullandığını onaylayın.
* Sesli mesaja yönlendirilen çağrı
   * Kullanıcının telefonun açık olduğundan ve bu hizmetin kendi alanında kullanılabildiğinden veya alternatif yöntem kullandığından emin olun.
* Kullanıcı engellendi
   * Yöneticinin Azure portalındaki kullanıcının engelini kaldırmasını sağlar.
* SMS aygıtta abone değil
   * Kullanıcının yöntemleri değiştirmesini veya cihazdaki SMS'i etkinleştirmesini sağlar.
* Hatalı telekom sağlayıcıları (Telefon girişi algılanmadı, DTMF tonları sorunları eksik, birden çok cihazda engellenen arayan kimliği veya birden çok cihazda engellenen SMS)
   * Microsoft, kimlik doğrulaması için telefon aramalarını ve SMS mesajlarını yönlendirmek için birden çok telekom sağlayıcısı kullanır. Yukarıdaki sorunlardan herhangi birini görüyorsanız, kullanıcının yöntemi 5 dakika içinde en az 5 kez kullanmaya kalkışması ve Microsoft desteğine başvururken bu kullanıcının bilgilerinin kullanılabilir olması gerekir.

## <a name="app-passwords"></a>Uygulama Şifreleri

Bazı tarayıcı dışı uygulamalar çok faktörlü kimlik doğrulamayı desteklemez, bir kullanıcı çok faktörlü kimlik doğrulama için etkinleştirildiyse ve tarayıcı dışı uygulamaları kullanmaya çalışıyorsa, kimlik doğrulaması yapamaz. Uygulama parolası, kullanıcıların kimlik doğrulamaya devam etmesini sağlar

Kullanıcı başına MFA aracılığıyla değil, Koşullu Erişim ilkeleri yle Çok Faktörlü Kimlik Doğrulama'yı uygularsanız, uygulama parolaları oluşturamazsınız. Erişimi denetlemek için Koşullu Erişim ilkelerini kullanan uygulamaların uygulama parolalarına ihtiyacı yoktur.

Kuruluşunuz Azure AD ile SSO için federe yse ve Azure MFA kullanıyor olacaksanız, aşağıdaki ayrıntılara dikkat edin:

* Uygulama parolası Azure AD tarafından doğrulanır ve bu nedenle federasyonu atlar. Federasyon yalnızca uygulama parolalarını ayarlarken kullanılır. Federe (SSO) kullanıcıları için parolalar kuruluş kimliğinde depolanır. Kullanıcı şirketten ayrılırsa, bu bilginin DirSync kullanarak kuruluş kimliğine akması gerekir. Hesabın devre dışı sayılması/silinmesi eşitleme için üç saat kadar sürebilir ve bu da Azure AD'deki uygulama parolalarının devre dışı kaldırılmasını/silinmesini geciktirir.
* Şirket için İstemci Erişimi Denetimi ayarları Uygulama Parolası tarafından onaylanmaz.
* Uygulama parolaları için şirket içinde kimlik doğrulama günlüğü/denetim özelliği yoktur.
* Bazı gelişmiş mimari tasarımlar, kimlik doğruladıkları yere bağlı olarak istemcilerle iki aşamalı doğrulama kullanırken kuruluş kullanıcı adı ve parolaları ile uygulama parolalarının bir birleşimini kullanmayı gerektirebilir. Şirket içi altyapıya karşı kimlik doğrulaması yapan istemciler için kuruluş kullanıcı adı ve parola kullanırsınız. Azure AD'ye karşı kimlik doğrulaması yapan istemciler için uygulama parolasını kullanırsınız.
* Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz. Kullanıcıların uygulama parolaları oluşturmasına izin vermeniz gerekiyorsa, hizmet ayarları altında **tarayıcı dışı uygulamalarda oturum açmaları için kullanıcıların uygulama parolaları oluşturmasına izin ver'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Kuruluşunuz için self servis parola sıfırlamayı etkinleştirme](quickstart-sspr.md)

[Kuruluşunuz için Azure Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirme](howto-mfa-getstarted.md)

[Kiracınızda birleşik kaydı etkinleştirme](howto-registration-mfa-sspr-combined.md)

[Son kullanıcı kimlik doğrulama yöntemi yapılandırma belgeleri](https://aka.ms/securityinfoguide)
