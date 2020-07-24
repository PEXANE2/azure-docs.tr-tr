---
title: Kimlik doğrulama yöntemleri ve özellikleri-Azure Active Directory
description: Oturum açma olaylarının artırılmasına ve güvenliğinin sağlanmasına yardımcı olmak için Azure Active Directory 'de bulunan farklı kimlik doğrulama yöntemleri ve özellikleri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.collection: M365-identity-device-management
ms.custom: contperfq4
ms.openlocfilehash: 4c224f40996625ead4592a3bf50e02f0877c76db
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87051314"
---
# <a name="what-authentication-and-verification-methods-are-available-in-azure-active-directory"></a>Azure Active Directory'de mevcut olan kimlik doğrulaması ve doğrulama yöntemleri

Azure Active Directory (Azure AD) hesapları için oturum açma deneyiminin bir parçası olarak, bir kullanıcının kimliklerini doğrulayabilmesi için farklı yollar vardır. Kullanıcı adı ve parola, bir kullanıcının geçmişe ait kimlik bilgilerini sağlaması için en yaygın yoldur. Azure AD 'de Modern kimlik doğrulama ve güvenlik özellikleriyle, bu temel parola ek kimlik doğrulama yöntemleriyle takıma girebilir veya değiştirilebilir.

Azure AD 'deki bir Kullanıcı, aşağıdaki kimlik doğrulama yöntemlerinden birini kullanarak kimlik doğrulaması seçebilir:

* Geleneksel Kullanıcı adı ve parola
* Microsoft Authenticator uygulama parolasız oturum açma
* OATH donanım belirteci veya FIDO2 güvenlik anahtarı
* SMS tabanlı parolasız oturum açma

Azure AD 'deki pek çok hesap self servis parola sıfırlama (SSPR) veya Azure Multi-Factor Authentication için etkinleştirilmiştir. Bu özellikler, telefon araması veya güvenlik soruları gibi ek doğrulama yöntemlerini içerir. Kullanıcıların birden çok doğrulama yöntemini kaydetmesini gerektirmesini öneririz. Bir kullanıcı için bir yöntem kullanılamadığında, başka bir yöntemle kimlik doğrulamayı seçebilirler.

Aşağıdaki tabloda, birincil veya ikincil kimlik doğrulaması için hangi yöntemlerin kullanılabildiği özetlenmektedir:

| Yöntem | Birincil kimlik doğrulama | İkincil kimlik doğrulaması |
| --- | --- | --- |
| [Parola](#password) | Yes | |
| [Microsoft Authenticator uygulaması](#microsoft-authenticator-app) | Evet (Önizleme) | MFA ve SSPR |
| [FIDO2 güvenlik anahtarları (Önizleme)](#fido2-security-keys) | Yes | Yalnızca MFA |
| [OATH yazılım belirteçleri](#oath-software-tokens) | Hayır | MFA |
| [OATH Donanım belirteçleri (Önizleme)](#oath-hardware-tokens-preview) | Yes | MFA |
| [SMS](#phone-options) | Evet (Önizleme) | MFA ve SSPR |
| [Sesli arama](#phone-options) | Hayır | MFA ve SSPR |
| [Güvenlik soruları](#security-questions) | Hayır | Yalnızca SSPR |
| [E-posta adresi](#email-address) | Hayır | Yalnızca SSPR |
| [Uygulama parolaları](#app-passwords) | Hayır | Yalnızca belirli durumlarda MFA |

Bu makalede, Azure AD 'de bulunan bu farklı kimlik doğrulama ve doğrulama yöntemleri ve belirli sınırlamalar veya kısıtlamalar özetlenmektedir.

![Oturum açma ekranında kullanılan kimlik doğrulama yöntemleri](media/concept-authentication-methods/overview-login.png)

## <a name="password"></a>Parola

Azure AD parolası genellikle birincil kimlik doğrulama yöntemlerinden biridir. Parola kimlik doğrulama yöntemini devre dışı bırakamıyorum.

Kullanıcı imza için parolasını kullanmazsa [SMS tabanlı oturum açma](howto-authentication-sms-signin.md) gibi bir kimlik doğrulama yöntemi kullanıyor olsanız bile, bir parola kullanılabilir kimlik doğrulama yöntemi olarak kalır.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator uygulaması

Kimlik doğrulayıcı uygulaması, Azure AD iş veya okul hesabınız veya Microsoft hesabı için ek bir güvenlik düzeyi sağlar ve [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594)ve [Windows Phone](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6)için kullanılabilir. Microsoft Authenticator uygulamayla, kullanıcılar oturum açma sırasında parolasız bir şekilde veya self servis parola sıfırlama (SSPR) veya Azure Multi-Factor Authentication olayları sırasında ek bir doğrulama seçeneği olarak kimlik doğrulaması yapabilir.

Kullanıcılar, bir oturum açma arabirimine girilebilecek bir OATH doğrulama kodu oluşturmak için Mobil uygulamayla ilgili bir bildirim alabilir veya bu uygulamayı kullanabilir. Hem bildirim hem de doğrulama kodunu etkinleştirirseniz, Authenticator uygulamasını kaydeden kullanıcılar kimliklerini doğrulamak için her iki yöntemi de kullanabilir.

Kimlik doğrulayıcı uygulamasını Kullanıcı adı ve parola birleşimi yerine bir oturum açma isteminde kullanmak için bkz. [Microsoft Authenticator App (Önizleme) ile passwordless oturum açmayı etkinleştirme](howto-authentication-passwordless-phone.md).

> [!NOTE]
> Kullanıcılar SSPR 'yi etkinleştirdiklerinde mobil uygulamalarını kaydetme seçeneğine sahip değildir. Bunun yerine, kullanıcılar mobil uygulamalarını [https://aka.ms/mfasetup](https://aka.ms/mfasetup) konumundaki Birleşik güvenlik bilgileri kaydının bir parçası olarak veya ' de kaydedebilir [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) .

### <a name="notification-through-mobile-app"></a>Mobil uygulama aracılığıyla bildirim

Authenticator uygulaması, Smartphone 'a veya tabletinize bir bildirim göndererek, hesaplara yetkisiz erişimin önlenmesine ve sahte işlemleri durdurmasına yardımcı olabilir. Kullanıcılar bildirimi görüntüler ve meşru ise **Doğrula**' yı seçin. Aksi takdirde, **Reddet**' i seçebilir.

![Oturum açma işlemini tamamlamaya yönelik kimlik doğrulayıcı uygulama bildirimi için Web tarayıcı istemi örnek ekran görüntüsü](media/tutorial-enable-azure-mfa/azure-multi-factor-authentication-browser-prompt.png)

> [!NOTE]
> Kuruluşunuzda Çin 'de çalışan veya Çin 'e geçiş yapan personel varsa, Android cihazlarda *mobil uygulama yöntemi ile Ilgili bildirim* söz konusu ülkede/bölgede çalışmaz. Diğer kimlik doğrulama yöntemleri bu kullanıcılar için kullanılabilir duruma getirilmelidir.

### <a name="verification-code-from-mobile-app"></a>Mobil uygulamadaki doğrulama kodu

Kimlik doğrulayıcı uygulaması bir OATH doğrulama kodu oluşturmak için yazılım belirteci olarak kullanılabilir. Kullanıcı adınızı ve parolanızı girdikten sonra, kimlik doğrulayıcı uygulaması tarafından belirtilen kodu oturum açma arabirimine girersiniz. Doğrulama kodu, ikinci bir kimlik doğrulama biçimi sağlar.

Kullanıcılar, her zaman kullanılmak üzere yapılandırılmış Microsoft Authenticator uygulaması gibi beş OATH donanım belirtecinin veya Authenticator uygulamasının bir birleşimine sahip olabilir.

> [!WARNING]
> Yalnızca bir yöntem sıfırlama için gerekli olduğunda self servis parola sıfırlamasının en yüksek düzeyde güvenlik sağlamak için, kullanıcılara sunulan tek seçenek bir doğrulama kodu olur.
>
> İki yöntem gerektiğinde, kullanıcılar diğer etkin yöntemlerin yanı sıra bir bildirim ya da doğrulama kodu kullanarak sıfırlayabilir.

## <a name="fido2-security-keys"></a>FIDO2 güvenlik anahtarları

FIDO (hızlı kimlik çevrimiçi) Birliği, açık kimlik doğrulama standartlarını yükseltmenize ve parolaların Kullanıcı kimliğini bir kimlik doğrulama formu olarak azaltmaya yardımcı olur. FIDO2, Web kimlik doğrulaması (WebAuthn) standardını içeren en son standarttır.

FIDO2 güvenlik anahtarlarını bir Kullanıcı adı ve parola birleşimi yerine bir oturum açma isteminde kullanmak için bkz. [passwordless FIDO2 güvenlik anahtarı oturum açma 'Yı etkinleştirme (Önizleme)](howto-authentication-passwordless-security-key.md).

Kullanıcılar, kimlik doğrulama yöntemi olarak oturum açma arabiriminde bir FIDO2 güvenlik anahtarı kaydedebilir ve seçebilir. Bu FIDO2 güvenlik anahtarları genellikle USB cihazlarıdır, ancak Bluetooth veya NFC de kullanabilir. Kimlik doğrulamasını işleyen bir donanım aygıtıyla, açığa çıkarılan veya tahmin edilecek bir parola olmadığından hesabın güvenliği artar.

Azure AD 'de FIDO2 güvenlik anahtarları şu anda önizlemededir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="oath-tokens"></a>OATH belirteçleri

OATH TOTP (zaman tabanlı bir kerelik parola), bir kerelik parola (OTP) kodlarının nasıl oluşturulduğunu belirten bir açık standarttır. OATH TOTP, kodları oluşturmak için yazılım veya donanım kullanılarak uygulanabilir. Azure AD, farklı bir kod oluşturma standardı olan OATH HOTP 'yi desteklemez.

### <a name="oath-software-tokens"></a>OATH yazılım belirteçleri

Yazılım OATH belirteçleri genellikle Microsoft Authenticator uygulaması ve diğer kimlik doğrulayıcı uygulamaları gibi uygulamalardır. Azure AD, uygulamaya giriş yapan ve her OTP oluşturmak için kullanılan, gizli anahtar veya çekirdek oluşturur.

Kimlik doğrulayıcı uygulaması, bir kullanıcının cihazının bağlantısı olmasa bile bir yedeklemesi olması için, anında iletme bildirimleri için ayarlandığında, otomatik olarak kodlar üretir. Kod oluşturmak için OATH TOTP kullanan üçüncü taraf uygulamalar da kullanılabilir.

Bazı OATH TOTP Donanım belirteçleri programlanabilir, yani gizli anahtar veya çekirdek önceden programlanabilir olarak gelmeyecektir. Bu programlanabilir donanım belirteçleri, yazılım belirteci kurulum akışından alınan gizli anahtar veya çekirdek kullanılarak ayarlanabilir. Müşteriler bu belirteçleri kendi tercih ettiği satıcıdan satın alabilir ve kendi satıcısının kurulum sürecinde gizli anahtarı veya kaynağı kullanabilir.

### <a name="oath-hardware-tokens-preview"></a>OATH Donanım belirteçleri (Önizleme)

Azure AD, her 30 veya 60 saniyede bir kodu yenileyen OATH-TOTP SHA-1 belirteçleri kullanımını destekler. Müşteriler bu belirteçleri kendi tercih ettiği satıcıdan satın alabilir.

OATH TOTP Donanım belirteçleri genellikle, belirteçte önceden programlanabilir bir gizli anahtar veya çekirdek ile gelir. Bu anahtarların aşağıdaki adımlarda açıklandığı gibi Azure AD 'ye giriş olması gerekir. Gizli anahtarlar 128 karakterle sınırlıdır ve bu, tüm belirteçlerle uyumlu olmayabilir. Gizli anahtar yalnızca *a-z* veya *a-z* karakterleri ve *1-7*rakamları içerebilir ve *Base32*içinde kodlanmalıdır.

Yeniden kullanılabilen programlanabilir OATH TOTP Donanım belirteçleri, yazılım belirteci kurulum akışında Azure AD ile de ayarlanabilir.

OATH Donanım belirteçleri, genel önizlemenin bir parçası olarak desteklenir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

![OATH belirteçlerini MFA OATH belirteçleri dikey penceresine yükleme](media/concept-authentication-methods/mfa-server-oath-tokens-azure-ad.png)

Belirteçler alındıktan sonra, aşağıdaki örnekte gösterildiği gibi, UPN, seri numarası, gizli anahtar, zaman aralığı, üretici ve model dahil olmak üzere bir virgülle ayrılmış değerler (CSV) dosya biçiminde karşıya yüklenmelidir:

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567abcdef1234567abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Üst bilgi satırını CSV dosyanıza eklediğinizden emin olun.

Bir CSV dosyası olarak düzgün biçimlendirildikten sonra, bir yönetici Azure portal oturum açabilir, **Azure Active Directory > güvenlik > MFA > Oath belirteçleri**' ne gidebilir ve elde edilen CSV dosyasını karşıya yükleyebilir.

CSV dosyasının boyutuna bağlı olarak, işlem birkaç dakika sürebilir. Geçerli durumu almak için **Yenile** düğmesini seçin. Dosyada herhangi bir hata varsa, çözmeniz için herhangi bir hatayı listeleyen bir CSV dosyası indirebilirsiniz. İndirilen CSV dosyasındaki alan adları karşıya yüklenen sürümden farklı.

Herhangi bir hata çözüldükten sonra, yönetici belirteç için **Etkinleştir** ' i seçip BELIRTEÇTE görünen OTP 'yi girerek her anahtarı etkinleştirebilir.

Kullanıcılar, her zaman kullanılmak üzere yapılandırılmış Microsoft Authenticator uygulaması gibi beş OATH donanım belirtecinin veya Authenticator uygulamasının bir birleşimine sahip olabilir.

## <a name="phone-options"></a>Telefon seçenekleri

Metin iletisini kullanarak doğrudan kimlik doğrulama için, [SMS tabanlı kimlik doğrulaması (Önizleme) için kullanıcıları yapılandırabilir ve etkinleştirebilirsiniz](howto-authentication-sms-signin.md). SMS tabanlı oturum açma, ön satır çalışanları için harika. SMS tabanlı oturum açma sayesinde, kullanıcıların uygulama ve hizmetlere erişmek için bir Kullanıcı adı ve parola bilmeleri gerekmez. Bunun yerine Kullanıcı kayıtlı cep telefonu numarasını girer, doğrulama kodu içeren bir kısa mesaj alır ve oturum açma arabirimine girer.

Kullanıcılar ayrıca, Azure Multi-Factor Authentication veya self servis parola sıfırlama (SSPR) sırasında kullanılan ikincil kimlik doğrulama biçimi olarak bir cep telefonu veya ofis telefonu kullanarak kendilerini doğrulayabilirler.

Doğru şekilde çalışmak için telefon numaralarının *+ CountryCode PhoneNumber*biçiminde olması gerekir, örneğin *+ 1 4251234567*.

> [!NOTE]
> Ülke/bölge kodu ve telefon numarası arasında bir boşluk olması gerekir.
>
> Parola sıfırlama, telefon uzantılarını desteklemez. *+ 1 4251234567X12345* biçiminde bile, çağrı yerleştirilmadan önce uzantılar kaldırılır.

### <a name="mobile-phone-verification"></a>Cep telefonu doğrulama

Azure Multi-Factor Authentication veya SSPR için, kullanıcılar oturum açma arabirimine girmek üzere doğrulama kodu içeren bir kısa mesaj almayı seçebilir veya tanımlı PIN kodunu girmek için bir istem içeren bir telefon araması alabilir.

Kullanıcılar cep telefonu numarasının dizinde görünmesini istemiyor, ancak parola sıfırlama için kullanmak istiyorsanız, Yöneticiler dizindeki telefon numarasını doldurmamalıdır. Bunun yerine, kullanıcılar, **kimlik doğrulama telefonu** özniteliğini, Birleşik güvenlik bilgileri kaydı aracılığıyla doldurmalıdır [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo) . Yöneticiler bu bilgileri kullanıcının profilinde görebilir, ancak başka bir yerde yayımlanmaz.

![Bir telefon numarası doldurulmuş kimlik doğrulama yöntemlerini gösteren Azure portal ekran görüntüsü](media/concept-authentication-methods/user-authentication-methods.png)

Microsoft, tutarlı SMS veya sesli tabanlı Azure Multi-Factor Authentication istem teslimini aynı numarayla garanti etmez. Kullanıcılarımıza ilişkin olarak, SMS teslimat yeteneğini geliştirmek üzere rota ayarlamaları yaptığımız için istediğiniz zaman kısa kodlar ekleyebilir veya kaldırabiliriz. Microsoft, Birleşik Devletler ve Kanada yanı sıra ülkeler/bölgeler için kısa kodları desteklemez.

#### <a name="text-message-verification"></a>Kısa mesaj doğrulaması

SSPR veya Azure Multi-Factor Authentication sırasında kısa mesaj doğrulamayla birlikte, bir doğrulama kodu içeren cep telefonu numarasına SMS gönderilir. Oturum açma işlemini gerçekleştirmek için, girilen doğrulama kodu oturum açma arabirimine girilir.

#### <a name="phone-call-verification"></a>Telefon araması doğrulaması

SSPR veya Azure Multi-Factor Authentication sırasında telefon araması doğrulaması ile, Kullanıcı tarafından kaydedilen telefon numarasına otomatik bir sesli çağrı yapılır. Oturum açma işlemini tamamlamaya yönelik olarak, kullanıcıdan PIN numarasını girmesi istenir ve ardından tuş takımında # gelmelidir.

### <a name="office-phone-verification"></a>Office telefon doğrulaması

Office Phone özniteliği Azure AD yöneticisi tarafından yönetilir ve Kullanıcı tarafından kaydedilemez.

SSPR veya Azure Multi-Factor Authentication sırasında telefon araması doğrulaması ile, Kullanıcı tarafından kaydedilen telefon numarasına otomatik bir sesli çağrı yapılır. Oturum açma işlemini tamamlamaya yönelik olarak, kullanıcıdan PIN numarasını girmesi istenir ve ardından tuş takımında # gelmelidir.

### <a name="troubleshooting-phone-options"></a>Telefon seçenekleriyle ilgili sorunları giderme

Azure AD için telefon kimlik doğrulamasıyla ilgili sorunlar yaşıyorsanız, aşağıdaki sorun giderme adımlarını gözden geçirin:

* Tek bir cihazda engellenen arayan KIMLIĞI.
   * Cihazda yapılandırılan tüm engellenen numaraları gözden geçirin.
* Yanlış telefon numarası veya yanlış ülke/bölge kodu ya da kişisel telefon numarası ile iş telefonu numarası arasında karışıklık.
   * Kullanıcı nesnesi ve yapılandırılmış kimlik doğrulama yöntemleri sorunlarını giderin. Doğru telefon numaralarının kaydedildiğinden emin olun.
* Yanlış PIN girildi.
   * Kullanıcının, hesabı için kayıtlı doğru PIN 'ı kullandığını onaylayın.
* Sesli mesaj ile iletilen çağrı.
   * Kullanıcının telefonu açık olduğundan ve bu hizmetin kendi alanında kullanılabilir olduğundan emin olun veya alternatif yöntemi kullanın.
* Kullanıcı engellendi
   * Azure portal kullanıcının engellemesini kaldırmak için bir Azure AD yöneticisi vardır.
* Cihazda SMS abone değil.
   * Kullanıcının bir yöntemi değiştirmesini veya cihazda SMS 'yi etkinleştirmesini sağlamak.
* Telefon girişi yok, eksik DTMF tonları sorunları, birden çok cihazda engellenen çağıran KIMLIĞI veya birden çok cihazda SMS tarafından engellenen hatalı Telekom sağlayıcıları.
   * Microsoft, kimlik doğrulaması için telefon çağrılarını ve SMS iletilerini yönlendirmek üzere birden çok Telekom sağlayıcısı kullanır Yukarıdaki sorunlardan herhangi birini görürseniz, bir kullanıcının yöntemini 5 dakika içinde en az beş kez kullanmayı denemesi ve Microsoft desteği ile iletişim kurulurken kullanıcının bilgilerinin kullanılabilir olması gerekir.

## <a name="security-questions"></a>Güvenlik soruları

Güvenlik soruları, oturum açma olayında kimlik doğrulama yöntemi olarak kullanılmaz. Bunun yerine, güvenlik soruları, kim olduğunu onaylamak için self servis parola sıfırlama (SSPR) işlemi sırasında kullanılabilir. Yönetici hesapları, SSPR ile doğrulama yöntemi olarak güvenlik sorularını kullanamaz.

Kullanıcılar SSPR 'ye kaydolduklarında, kullanılacak kimlik doğrulama yöntemlerini seçmeleri istenir. Güvenlik sorularını kullanmayı tercih ederseniz, kendi yanıtlarını isteyecek ve sonra kendi yanıtlarını sağlamaları için bir soru kümesinden seçim yapılır.

![Güvenlik soruları için kimlik doğrulama yöntemlerini ve seçeneklerini gösteren Azure portal ekran görüntüsü](media/concept-authentication-methods/security-questions-authentication-method.png)

> [!NOTE]
> Güvenlik soruları, dizindeki bir kullanıcı nesnesi üzerinde özel olarak ve güvenli bir şekilde depolanır ve yalnızca kayıt sırasında kullanıcılar tarafından yanıtlanabilecek. Bir yöneticinin bir kullanıcının sorularını veya yanıtlarını okuması veya değiştirmesi için bir yol yoktur.

Bazı kullanıcılar başka bir Kullanıcı sorusunun yanıtlarını bilebileceğinden, güvenlik soruları diğer yöntemlerden daha az güvenli olabilir. SSPR ile güvenlik soruları kullanıyorsanız, bunları başka bir yöntemle birlikte kullanmanız önerilir. Bir kullanıcıdan SSPR işlemi sırasında kimliklerini doğrulamak için Microsoft Authenticator uygulama veya telefon kimlik doğrulamasını kullanması istenebilir ve yalnızca telefon veya kayıtlı cihazları yoksa güvenlik soruları ' nı seçin.

### <a name="predefined-questions"></a>Önceden tanımlanmış sorular

Aşağıdaki önceden tanımlanmış güvenlik soruları, SSPR ile doğrulama yöntemi olarak kullanılabilir. Bu güvenlik sorularının tümü, kullanıcının tarayıcı yerel ayarlarına bağlı olarak Office 365 dillerinin tam kümesine çevrilir ve yerelleştirilir:

* İlk eşiniz/iş ortağınızı hangi şehirle karşıladınız?
* Ebeveynleriniz hangi şehirle buluşmış?
* En yakın eşdüzey ağınız hangi şehirde etkin?
* Babalar hangi şehirde doğdu?
* İlk işiniz hangi şehirdir?
* Anneniz hangi şehirde doğdu?
* Yeni yıl 2000 ' de hangi şehirdeydiniz?
* Yüksek okulda en sevdiğiniz öğretmenin soyadı nedir?
* Uyguladığınız bir üniversite adı nedir, ancak katılmadınız mı?
* İlk evlilik alıyeninizi dağıttığınız yerin adı nedir?
* Babalar ikinci adı nedir?
* En sevdiğiniz yemek nedir?
* Anneannenizin en Annenizin adı ve soyadı nedir?
* Annenizin ikinci adı nedir?
* En eski eşdüzey Doğum günün ayı ve yılı nedir? (örneğin, Kasım 1985)
* En eski eşdüzey öğenin ikinci adı nedir?
* Babanızın babalar adı ve soyadı nedir?
* Kardeşinizin eşdüzey ortaınızın ikinci adı nedir?
* Altıncı sınıf için hangi okula katıldınız?
* Çocukluğunuzdaki en iyi arkadaşınızın adı ve soyadı neydi?
* İlk önemli olan ilk ve soyadı neydi?
* En sevdiğiniz sınıf okul öğretmeninizin soyadı neydi?
* İlk arabalarınızın veya otodöngünüzün marka ve modeli neydi?
* Katıldığınız ilk okulunuzun adı neydi?
* Doğmakta olduğunuz hastanın adı neydi?
* İlk çocukluk evin sokak adı neydi?
* Çocukluğunuzdaki kahraman 'ın adı neydi?
* En sevdiğiniz peluş ya da bir hayvan adı neydi?
* İlk evcil hayvanınızın adı neydi?
* Çocukluk takma ad nedir?
* Yüksek okulda en sevdiğiniz spor nedir?
* İlk işiniz neydi?
* Çocukluk telefon numaranız için son dört basamak neydi?
* Küçükken, büyüyken ne yapmak istiyorsunuz?
* Şimdiye kadar tanıştığınız en ünlü kişi kim?

### <a name="custom-security-questions"></a>Özel güvenlik soruları

Ek esneklik için kendi özel güvenlik sorularınızı tanımlayabilirsiniz. Özel bir güvenlik sorusunun en fazla uzunluğu 200 karakterdir.

Özel güvenlik soruları, varsayılan güvenlik soruları gibi otomatik olarak yerelleştirilmez. Kullanıcının tarayıcı yerel ayarı farklı olsa bile, tüm özel sorular yönetim kullanıcı arabirimine girildikleri dilde görüntülenir. Yerelleştirilmiş sorulara ihtiyacınız varsa, önceden tanımlanmış soruları kullanmanız gerekir.

### <a name="security-question-requirements"></a>Güvenlik sorusu gereksinimleri

Hem varsayılan hem de özel güvenlik soruları için aşağıdaki gereksinimler ve sınırlamalar geçerlidir:

* En küçük yanıt karakter sınırı üç karakterdir.
* En fazla yanıt karakter sınırı 40 karakterdir.
* Kullanıcılar aynı sorudan birden çok kez yanıt vermez.
* Kullanıcılar, birden fazla soruya aynı yanıtı sağlayamaz.
* Herhangi bir karakter kümesi, Unicode karakterler de dahil olmak üzere soruları ve yanıtları tanımlamak için kullanılabilir.
* Tanımlanan soruların sayısı, kaydolmak için gereken soru sayısına eşit veya ondan büyük olmalıdır.

## <a name="email-address"></a>E-posta adresi

Bir e-posta adresi doğrudan kimlik doğrulama yöntemi olarak kullanılamaz. E-posta adresi yalnızca self servis parola sıfırlama (SSPR) için doğrulama seçeneği olarak kullanılabilir. SSPR sırasında e-posta adresi seçildiğinde, kimlik doğrulama/doğrulama işlemini tamamlaması için kullanıcıya bir e-posta gönderilir.

SSPR için kayıt sırasında, Kullanıcı kullanılacak e-posta adresini sağlar. SSPR sırasında bunlara erişebildiklerinden emin olmak için kurumsal hesabından farklı bir e-posta hesabı kullanmaları önerilir.

## <a name="app-passwords"></a>Uygulama parolaları

Bazı eski, tarayıcı olmayan uygulamalar, kimlik doğrulama işleminde duraklamaları veya kesmeleri anlamıyor. Bir Kullanıcı Multi-Factor Authentication için etkinleştirilmişse ve bu eski, tarayıcı olmayan uygulamalardan birini kullanmaya çalışırsa, genellikle kimlik doğrulaması başarılı olur. Uygulama parolası, kullanıcıların kesintiye uğramadan daha eski, tarayıcı olmayan uygulamalarla kimlik doğrulaması yapmaya devam etmesine olanak tanır.

Varsayılan olarak, kullanıcılar uygulama parolaları oluşturamaz. Kullanıcıların uygulama parolaları oluşturmalarına izin vermeniz gerekiyorsa, kullanıcının Azure Multi-Factor Authentication özellikleri için *hizmet ayarları* altında **tarayıcı olmayan uygulamalarda oturum açmak için kullanıcıların uygulama parolaları oluşturmasına izin ver** ' i seçin.

![Uygulama parolalarının kullanıcısına izin vermek için çok faktörlü kimlik doğrulamasının hizmet ayarlarını gösteren Azure portal ekran görüntüsü](media/concept-authentication-methods/app-password-authentication-method.png)

Azure Multi-Factor Authentication, koşullu erişim ilkelerini kullanarak zorunlu kılabilir ve Kullanıcı başına MFA aracılığıyla değil, uygulama parolaları oluşturamazsınız. Erişimi denetlemek için koşullu erişim ilkelerini kullanan modern uygulamaların uygulama parolaları gerekmez.

Kuruluşunuz Azure AD ile çoklu oturum açma (SSO) için federe ise ve Azure Multi-Factor Authentication kullanıyorsanız aşağıdaki noktalar geçerlidir:

* Uygulama parolası Azure AD tarafından doğrulandığından, Federasyonu atlar. Federasyon yalnızca uygulama parolaları ayarlanırken kullanılır. Federasyon (SSO) kullanıcıları için, parolalar kuruluş KIMLIĞINDE depolanır. Kullanıcı şirketten ayrılırsa, bu bilgi DirSync kullanarak kuruluş KIMLIĞINE akacaktır. Hesap devre dışı bırakma veya silme olaylarının eşitlenmesi üç saate kadar sürebilir. Bu, Azure AD 'de uygulama parolalarının devre dışı bırakılmasını/silinmesini erteler.
* Şirket içi Istemci Access Control ayarları uygulama parolaları tarafından kabul edilmez.
* Uygulama parolaları için şirket içi kimlik doğrulaması günlüğü veya denetim yeteneği kullanılamaz.
* Bazı gelişmiş mimari tasarımları, kimlik doğrulamasının bulunduğu yere bağlı olarak Multi-Factor Authentication kullanırken kurumsal Kullanıcı adı ve parolaların ve uygulama parolalarının bir birleşimini kullanmanızı gerektirebilir.
    * Şirket içi altyapıya karşı kimlik doğrulaması yapan istemciler için bir kuruluş Kullanıcı adı ve parolası kullanırsınız.
    * Azure AD kimlik doğrulaması yapan istemciler için uygulama parolasını kullanırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [self servis parola sıfırlama (SSPR)][tutorial-sspr] ve [Azure Multi-Factor Authentication][tutorial-azure-mfa]öğreticisine bakın.

SSPR kavramları hakkında daha fazla bilgi edinmek için bkz. [Azure AD self servis parola sıfırlamasının nasıl çalıştığı][concept-sspr].

MFA kavramları hakkında daha fazla bilgi edinmek için bkz. [Azure Multi-Factor Authentication nasıl çalıştığı][concept-mfa].

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md