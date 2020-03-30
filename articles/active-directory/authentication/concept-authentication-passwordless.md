---
title: Azure Active Directory şifresiz oturum açma (önizleme)
description: FIDO2 güvenlik anahtarlarını veya Microsoft Authenticator uygulamasını kullanarak Azure Active Directory'de parolasız oturum açma seçenekleri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07ba38a5d7e8e8a89ba122efb1734c1f13a94d48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332179"
---
# <a name="passwordless-authentication-options-for-azure-active-directory"></a>Azure Etkin Dizin için parolasız kimlik doğrulama seçenekleri

Çok faktörlü kimlik doğrulama (MFA) kuruluşunuzun güvenliğini sağlamak için harika bir yoldur, ancak kullanıcılar genellikle parolalarını hatırlamak zorunda üstüne ek güvenlik katmanı ile sinirli olsun. Parola kaldırılır ve sahip olduğunuz bir şey, artı bir şey ya da bildiğiniz bir şey ile değiştirilir, çünkü parolasız kimlik doğrulama yöntemleri daha uygundur.

|   | Sahip olduğun bir şey | Olduğunuz veya bildiğiniz bir şey |
| --- | --- | --- |
| Parolasız | Windows 10 Aygıt, telefon veya güvenlik anahtarı | Biyometrik veya PIN |

Kimlik doğrulama söz konusu olduğunda her kuruluşun farklı gereksinimleri vardır. Microsoft, Azure Active Directory (Azure AD) ile tümleşen aşağıdaki üç parolasız kimlik doğrulama seçeneği sunar:

- İş İçin Windows Hello
- Microsoft Authenticator uygulaması
- FIDO2 güvenlik anahtarları

![Kimlik doğrulama: Güvenliğe karşı kolaylık](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

## <a name="windows-hello-for-business"></a>İş İçin Windows Hello

Windows Hello for Business, kendi belirlediği Windows PC'si olan bilgi çalışanları için idealdir. Biyometrik ve PIN doğrudan kullanıcının bilgisayarına bağlıdır ve bu da sahibinden başka birinin erişimini engeller. Ortak anahtar altyapısı (PKI) tümleştirmesi ve tek oturum açma (SSO) için yerleşik desteğiyle, Windows Hello for Business şirket içi ve bulutta kurumsal kaynaklara sorunsuz bir şekilde erişmek için kullanışlı bir yöntem sağlar.

![İşletmeler için Windows Hello ile kullanıcı oturum açma örneği](./media/concept-authentication-passwordless/windows-hellow-sign-in.jpeg)

Aşağıdaki adımlar, Oturum açma işleminin Azure Etkin Dizini ile nasıl çalıştığını gösterir.

![Windows Hello for Business ile kullanıcı oturum açma adımlarını özetleyen diyagram](./media/concept-authentication-passwordless/windows-hello-flow.png)

1. Kullanıcı, biyometrik veya PIN hareketi kullanarak Windows'a giriş yapar. Bu hareket, Windows Hello for Business özel anahtarının kilidini açar ve *Bulut AP sağlayıcısı*olarak adlandırılan Bulut Kimlik Doğrulama güvenlik destek sağlayıcısına gönderilir.
1. Cloud AP sağlayıcısı Azure AD'den bir nonce ister.
1. Azure AD, 5 dakika boyunca geçerli olan bir nonce döndürür.
1. Cloud AP sağlayıcısı, kullanıcının özel anahtarını kullanarak nonce'yi imzalar ve imzalı nonce'yi Azure AD'ye döndürür.
1. Azure AD, kullanıcının güvenli bir şekilde kaydedilmiş ortak anahtarını nonce imzasına karşı kullanarak imzalı nonce'yi doğrular. İmzayı doğruladıktan sonra Azure AD, döndürülen imzalı nonce'yi doğrular. Nonce doğrulandığında, Azure AD, aygıtın aktarım anahtarına şifrelenmiş oturum anahtarıyla birincil yenileme belirteci (PRT) oluşturur ve bunu Bulut AP sağlayıcısına döndürür.
1. Cloud AP sağlayıcısı oturum anahtarı ile şifreli PRT alır. Bulut AP sağlayıcısı, aygıtın özel aktarım anahtarını kullanarak oturum anahtarının şifresini çözer ve aygıtın Güvenilir Platform Modülü'ni (TPM) kullanarak oturum anahtarını korur.
1. Cloud AP sağlayıcısı, Windows'a başarılı bir kimlik doğrulama yanıtı döndürür. Kullanıcı daha sonra yeniden kimlik doğrulamaya gerek kalmadan Windows'a, bulutve şirket içi uygulamalara erişebilir.

Windows Hello for Business [planlama kılavuzu,](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) İşletmeler için Windows Hello türü ve göz önünde bulundurmanız gereken seçenekler hakkında karar almanıza yardımcı olmak için kullanılabilir.

## <a name="microsoft-authenticator-app"></a>Microsoft Authenticator Uygulaması

Çalışanınızın telefonunun parolasız kimlik doğrulama yöntemi olmasına izin verin. Parolaya ek olarak kullanışlı bir çok faktörlü kimlik doğrulama seçeneği olarak Microsoft Authenticator Uygulamasını zaten kullanıyor olabilirsiniz. Kimlik Doğrulayıcı Uygulamasını parolasız bir seçenek olarak da kullanabilirsiniz.

![Microsoft Authenticator uygulamasıyla Microsoft Edge'de oturum açma](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

Authenticator Uygulaması, herhangi bir iOS veya Android telefonu güçlü, şifresiz bir kimlik bilgisine dönüştürür. Kullanıcılar, telefonlarına bir bildirim alarak, ekranda görüntülenen bir numarayı telefonlarındaki yle eşleştirerek ve onaylamak için biyometrik (dokunma veya yüz) veya PIN'lerini kullanarak herhangi bir platformda veya tarayıcıda oturum açabilir. Yükleme ayrıntıları için lütfen [Microsoft Authenticator uygulamasını karşıdan yükleyin ve yükleyin.](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install)

Kimlik Doğrulayıcı Uygulamasını kullanarak parolasız kimlik doğrulaması, Windows Hello for Business ile aynı temel deseni izler. Azure AD'nin microsoft authenticator uygulaması sürümünün kullanıldığını bulabilmesi için kullanıcının tanımlanması gerektiğinden, bu durum biraz daha karmaşıktır:

![Microsoft Authenticator Uygulaması ile kullanıcı oturum açma adımlarını özetleyen diyagram](./media/concept-authentication-passwordless/authenticator-app-flow.png)

1. Kullanıcı kullanıcı adını girer.
1. Azure AD, kullanıcının güçlü bir kimlik bilgisine sahip olduğunu algılar ve Güçlü Kimlik Bilgisi akışını başlatır.
1. Uygulamaya apple push bildirim hizmeti (APNS) aracılığıyla iOS cihazlarda veya Android cihazlarda Firebase Cloud Messaging (FCM) aracılığıyla bir bildirim gönderilir.
1. Kullanıcı anında iletme bildirimini alır ve uygulamayı açar.
1. Uygulama Azure AD'yi çağırır ve bir kanıt-durum mücadelesi alır ve nonce.
1. Kullanıcı, özel anahtarın kilidini açmak için biyometrik veya PIN'ini girerek meydan okumayı tamamlar.
1. Nonce özel anahtarla imzalanır ve Azure AD'ye geri gönderilir.
1. Azure AD ortak/özel anahtar doğrulaması gerçekleştirir ve bir belirteç döndürür.

## <a name="fido2-security-keys"></a>FIDO2 güvenlik anahtarları

FIDO2 güvenlik anahtarları, herhangi bir form faktöründe gelebilecek, su altı standartlara dayalı parolasız kimlik doğrulama yöntemidir. Hızlı Kimlik Çevrimiçi (FIDO) şifresiz kimlik doğrulama için açık bir standarttır. FIDO, kullanıcıların ve kuruluşların harici bir güvenlik anahtarı veya bir aygıtta yerleşik bir platform anahtarı kullanarak kullanıcı adı veya parola olmadan kaynaklarında oturum açma standartlarından yararlanmalarına olanak tanır.

Genel önizleme için çalışanlar, Azure AD'lerinde oturum açtırmak veya Windows 10'a katılan karma Azure AD aygıtlarında oturum açabilmek için güvenlik anahtarlarını kullanabilir ve bulut ve şirket içi kaynaklarında tek oturum açabilir. Kullanıcılar desteklenen tarayıcılarda da oturum açabilir. FIDO2 güvenlik anahtarları, çok güvenlik duyarlı olan veya senaryoları olan veya telefonlarını ikinci bir faktör olarak kullanmak istemeyen veya kullanamayan çalışanlar alabilen işletmeler için harika bir seçenektir.

![Güvenlik anahtarıyla Microsoft Edge'de oturum açma](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

Bir kullanıcı FIDO2 güvenlik anahtarıyla giriş yaptığında aşağıdaki işlem kullanılır:

![FIDO2 güvenlik anahtarıyla kullanıcı oturum açma adımlarını özetleyen diyagram](./media/concept-authentication-passwordless/fido2-security-key-flow.png)

1. Kullanıcı FIDO2 güvenlik anahtarını bilgisayarına bağlar.
2. Windows FIDO2 güvenlik anahtarını algılar.
3. Windows bir kimlik doğrulama isteği gönderir.
4. Azure AD bir nonce geri gönderir.
5. Kullanıcı, FIDO2 güvenlik anahtarının güvenli yerleşim bölgesinde depolanan özel anahtarın kilidini açmak için jestini tamamlar.
6. FIDO2 güvenlik anahtarı nonce'yi özel anahtarla işaretler.
7. İmzalı nonce ile birincil yenileme belirteci (PRT) belirteç isteği Azure AD'ye gönderilir.
8. Azure AD, FIDO2 ortak anahtarını kullanarak imzalı nonce'yi doğrular.
9. Azure AD, şirket içi kaynaklara erişimi etkinleştirmek için PRT'yi döndürür.

FIDO2 tarafından onaylanan birçok anahtar olsa da, Microsoft maksimum güvenlik ve en iyi güvenliği sağlamak için satıcı tarafından uygulanacak FIDO2 İstemci-Kimlik Doğrulayıcı Protokolü (CTAP) belirtiminin bazı isteğe bağlı uzantılarını gerektirir Deneyim.

Bir güvenlik anahtarı Microsoft uyumlu olması için FIDO2 CTAP protokolünden aşağıdaki özellikleri ve uzantıları uygulamak **GEREKIR:**

| # | Özellik / Uzantı güveni | Bu özellik veya uzantı neden gereklidir? |
| --- | --- | --- |
| 1 | Yerleşik anahtar | Bu özellik, kimlik bilgilerinizin güvenlik anahtarında depolandığı güvenlik anahtarının taşınabilir olmasını sağlar. |
| 2 | İstemci pini | Bu özellik, kimlik bilgilerinizi ikinci bir faktörle korumanızı sağlar ve kullanıcı arabirimi olmayan güvenlik anahtarları için geçerlidir. |
| 3 | hmac-gizli | Bu uzantı, cihazınız çevrimdışı yken veya uçak modunda yken oturum açabilmesini sağlar. |
| 4 | RP başına birden fazla hesap | Bu özellik, Microsoft Hesabı ve Azure Etkin Dizini gibi birden çok hizmette aynı güvenlik anahtarını kullanabilmesini sağlar. |

Aşağıdaki sağlayıcılar, parolasız deneyimle uyumlu olduğu bilinen farklı form etkenlerinin FIDO2 güvenlik anahtarlarını sunar. FIDO Alliance'ın yanı sıra satıcıyla da iletişime geçerek bu anahtarların güvenlik özelliklerini değerlendirmenizi öneririz.

| Sağlayıcı | İletişim |
| --- | --- |
| Yubico | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feityen | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| Sakladı | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Düşmanlık | [https://www.ensurity.com/contact](https://www.ensurity.com/contact) |
| eWBM | [https://www.ewbm.com/support](https://www.ewbm.com/support) |
| AuthenTrend | [https://authentrend.com/about-us/#pg-35-3](https://authentrend.com/about-us/#pg-35-3) |
| Gemalto (Thales Grubu) | [https://safenet.gemalto.com/multi-factor-authentication/authenticators/passwordless-authentication/](https://safenet.gemalto.com/multi-factor-authentication/authenticators/passwordless-authentication/) |
| OneSpan A.Ş. | [https://www.onespan.com/sites/default/files/2019-08/Digipass-SecureClick_datasheet.pdf](https://www.onespan.com/sites/default/files/2019-08/Digipass-SecureClick_datasheet.pdf) |
| IDmelon Teknolojileri A.Ş. | [https://www.idmelon.com/#idmelon](https://www.idmelon.com/#idmelon) | 

> [!NOTE]
> NFC tabanlı güvenlik anahtarlarını satın alıp kullanmayı planlıyorsanız, güvenlik anahtarı için desteklenen bir NFC okuyucusuna ihtiyacınız olur. NFC okuyucusu bir Azure gereksinimi veya sınırlaması değildir. Desteklenen NFC okuyucularının listesi için NFC tabanlı güvenlik anahtarınız için satıcıya danışın.

Satıcıysanız ve cihazınızı desteklenen aygıtlar listesine almak istiyorsanız, [Fido2Request@Microsoft.com](mailto:Fido2Request@Microsoft.com)şu bağlantı kurun.

## <a name="what-scenarios-work-with-the-preview"></a>Önizlemede hangi senaryolar çalışır?

- Yöneticiler, kiracıları için parolasız kimlik doğrulama yöntemlerini etkinleştirebilir
- Yöneticiler, her yöntem için kiracıları içindeki tüm kullanıcıları hedefleyebilir veya kullanıcıları/grupları seçebilir
- Son kullanıcılar bu parolasız kimlik doğrulama yöntemlerini hesap portallarında kaydedebilir ve yönetebilir
- Son kullanıcılar bu parolasız kimlik doğrulama yöntemleriyle oturum açabilir
   - Microsoft Authenticator Uygulaması: Windows 10 Out Of Box (OOBE) kurulumu sırasında ve tüm işletim sistemindeki tüm mobil uygulamalar da dahil olmak üzere Azure AD kimlik doğrulamasının kullanıldığı senaryolarda çalışır.
   - Güvenlik anahtarları: Microsoft Edge (hem eski hem de yeni Edge) gibi desteklenen tarayıcılarda Windows 10 ve web için kilit ekranında çalışın.

## <a name="choose-a-passwordless-method"></a>Parolasız bir yöntem seçin

Bu üç parolasız seçenek arasındaki seçim şirketinizin güvenliğine, platformuna ve uygulama gereksinimlerine bağlıdır.

Microsoft şifresiz teknolojiyi seçerken göz önünde bulundurmanız gereken bazı etkenler şunlardır:

||**İş İçin Windows Hello**|**Microsoft Authenticator uygulamasıyla parolasız oturum açma**|**FIDO2 güvenlik anahtarları**|
|:-|:-|:-|:-|
|**Ön koşul**| Windows 10, sürüm 1809 veya sonrası<br>Azure Active Directory| Microsoft Authenticator uygulaması<br>Telefon (Android 6.0 veya üzeri çalışan iOS ve Android cihazlar.)|Windows 10, sürüm 1809 veya sonrası<br>Azure Active Directory|
|**Mod**|Platform|Yazılım|Donanım|
|**Sistemler ve cihazlar**|Yerleşik Güvenilir Platform Modülüne (TPM) sahip PC<br>PIN ve biyometri tanıma |Telefonda PIN ve biyometrik tanıma|Microsoft uyumlu FIDO2 güvenlik aygıtları|
|**Kullanıcı deneyimleri**|Windows aygıtlarıyla PIN veya biyometrik tanıma (yüz, iris veya parmak izi) kullanarak oturum açın.<br>Windows Hello kimlik doğrulaması aygıta bağlıdır; kullanıcının şirket kaynaklarına erişmek için hem cihaza hem de PIN veya biyometrik faktör gibi oturum açma bileşenine ihtiyacı vardır.|Parmak izi taraması, yüz veya iris tanıma veya PIN içeren bir cep telefonu kullanarak oturum açın.<br>Kullanıcılar, bilgisayarlarında veya cep telefonlarından çalışmak veya kişisel hesap için oturum açın.|FIDO2 güvenlik cihazını kullanarak oturum açma (biyometrik, PIN ve NFC)<br>Kullanıcı cihaza kuruluş denetimlerine dayalı olarak erişebilir ve USB, USB güvenlik anahtarları ve NFC özellikli akıllı kartlar, anahtarlar veya giyilebilir cihazlar gibi aygıtları kullanarak biyometriye dayalı olarak kimlik doğrulaması yapabilir.|
|**Etkin senaryolar**| Windows aygıtıyla parolasız deneyim.<br>Cihaz ve uygulamalarda tek oturum açma özelliğine sahip özel iş pc için geçerlidir.|Cep telefonu kullanarak şifresiz her yerde çözüm.<br>Web'deki iş veya kişisel uygulamalara herhangi bir cihazdan erişmek için geçerlidir.|Biyometrik, PIN ve NFC kullanan çalışanlar için parolasız deneyim.<br>Paylaşılan bilgisayarlar için geçerli olan ve cep telefonunun uygun bir seçenek olmadığı durumlarda (yardım masası personeli, genel kiosk veya hastane ekibi gibi)|

Gereksinimlerinizi ve kullanıcılarınızı hangi yöntemin destekleyeceğini seçmek için aşağıdaki tabloyu kullanın.

|Persona|Senaryo|Ortam|Şifresiz teknoloji|
|:-|:-|:-|:-|
|**Yönetici**|Yönetim görevleri için aygıta güvenli erişim|Atanmış Windows 10 cihazı|Windows Hello for Business ve/veya FIDO2 güvenlik anahtarı|
|**Yönetici**|Windows dışı aygıtlarda yönetim görevleri| Mobil veya windows olmayan aygıt|Microsoft Authenticator uygulamasıyla parolasız oturum açma|
|**Bilgi çalışanı**|Verimlilik çalışmaları|Atanmış Windows 10 cihazı|Windows Hello for Business ve/veya FIDO2 güvenlik anahtarı|
|**Bilgi çalışanı**|Verimlilik çalışmaları| Mobil veya windows olmayan aygıt|Microsoft Authenticator uygulamasıyla parolasız oturum açma|
|**Cephe çalışanı**|Fabrika, fabrika, perakende veya veri girişindekiosk'lar|Paylaşılan Windows 10 aygıtları|FIDO2 Güvenlik tuşları|

## <a name="next-steps"></a>Sonraki adımlar

[Kuruluşunuzdaki FIDO2 güvenlik anahtarı şifresiz seçenekleri etkinleştirme](howto-authentication-passwordless-security-key.md)

[Kuruluşunuzdaki telefon tabanlı şifresiz seçenekleri etkinleştirme](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Dış Bağlantılar

[FIDO İttifakı](https://fidoalliance.org/)

[FIDO2 CTAP belirtimi](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
