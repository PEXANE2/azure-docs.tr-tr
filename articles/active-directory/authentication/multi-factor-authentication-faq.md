---
title: Azure Çok Faktörlü Kimlik Doğrulama SSS - Azure Etkin Dizini
description: Azure Çok Faktörlü Kimlik Doğrulama ile ilgili sık sorulan sorular ve yanıtlar.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 04/13/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8d28f93f316ac2a63be6b3a8eb0b80678bd7607f
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81271412"
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication hakkında sık sorulan sorular

Bu SSS, Azure Çok Faktörlü Kimlik Doğrulama ve Çok Faktörlü Kimlik Doğrulama hizmetini kullanma yla ilgili sık sorulan soruları yanıtlar. Genel olarak hizmet, faturalandırma modelleri, kullanıcı deneyimleri ve sorun giderme ile ilgili sorulara ayrılmıştır.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.
>
> Azure Çok Faktörlü Kimlik Doğrulama Sunucusu ile ilgili aşağıda paylaşılan bilgiler yalnızca MFA sunucusu çalışan kullanıcılar için geçerlidir.
>
> Tüketime dayalı lisanslama, 1 Eylül 2018 tarihinden itibaren yeni müşteriler tarafından kullanılamıyor.
> 1 Eylül 2018 tarihinden itibaren geçerli olmak üzere yeni auth sağlayıcıları oluşturulamaz. Mevcut auth sağlayıcıları kullanılmaya ve güncelliğe devam edebilir. Çok faktörlü kimlik doğrulama, Azure AD Premium lisanslarında kullanılabilir bir özellik olmaya devam eder.

## <a name="general"></a>Genel

* [Azure Çok Faktörlü Authentication Server kullanıcı verilerini nasıl işler?](#how-does-azure-multi-factor-authentication-server-handle-user-data)
* [Kullanıcılarıma SMS mesajları göndermek için hangi SMS kısa kodları kullanılır?](#what-sms-short-codes-are-used-for-sending-sms-messages-to-my-users)

### <a name="how-does-azure-multi-factor-authentication-server-handle-user-data"></a>Azure Çok Faktörlü Authentication Server kullanıcı verilerini nasıl işler?

Multi-Factor Authentication Server ile, kullanıcı verileri yalnızca şirket içi sunucularda depolanır. Kalıcı kullanıcı verileri bulutta depolanmaz. Kullanıcı iki aşamalı doğrulama gerçekleştirdiğinde, Çok Faktörlü Kimlik Doğrulama Sunucusu kimlik doğrulaması için Azure Çok Faktörlü Kimlik Doğrulama bulut hizmetine veri gönderir. Çok Faktörlü Kimlik Doğrulama Sunucusu ile Çok Faktörlü Kimlik Doğrulama bulut hizmeti arasındaki iletişim, 443 bağlantı noktası üzerinde Secure Sockets Layer (SSL) veya Transport Layer Security (TLS) kullanır.

Kimlik doğrulama istekleri bulut hizmetine gönderildiğinde, kimlik doğrulama ve kullanım raporları için veriler toplanır. Aşağıdaki veri alanları iki aşamalı doğrulama günlüklerine dahildir:

* **Benzersiz Kimlik** (kullanıcı adı veya şirket içi Çok Faktörlü Kimlik Doğrulama Sunucusu Kimliği)
* **Adı ve Soyadı** (isteğe bağlı)
* **E-posta Adresi** (isteğe bağlı)
* **Telefon Numarası** (sesli arama veya SMS kimlik doğrulaması kullanırken)
* **Aygıt Belirteci** (mobil uygulama kimlik doğrulaması kullanırken)
* **Kimlik Doğrulama Modu**
* **Kimlik Doğrulama Sonucu**
* **Çok Faktörlü Kimlik Doğrulama Sunucu Adı**
* **Çok Faktörlü Kimlik Doğrulama Sunucusu IP**
* **İstemci IP** (varsa)

İsteğe bağlı alanlar Çok Faktörlü Kimlik Doğrulama Sunucusu'nda yapılandırılabilir.

Doğrulama sonucu (başarı veya reddi) ve reddedilmiş olmasının nedeni kimlik doğrulama verileriyle birlikte depolanır. Bu veriler kimlik doğrulama ve kullanım raporlarında kullanılabilir.

### <a name="what-sms-short-codes-are-used-for-sending-sms-messages-to-my-users"></a>Kullanıcılarıma SMS mesajları göndermek için hangi SMS kısa kodları kullanılır?

Amerika Birleşik Devletleri'nde, aşağıdaki SMS kısa kodları kullanın:

* *97671*
* *69829*
* *51789*
* *99399*

Kanada'da aşağıdaki SMS kısa kodlarını kullanıyoruz:

* *759731*
* *673801*

Aynı numarayla tutarlı SMS veya ses tabanlı Çok Faktörlü Kimlik Doğrulama istemi teslimatının garantisi yoktur. Kullanıcılarımızın yararına, SMS gönderimini geliştirmek için rota ayarlamaları yaptığımızda herhangi bir zamanda kısa kodlar ekleyebilir veya kaldırabiliriz. Amerika Birleşik Devletleri ve Kanada dışında ülkeler veya bölgeler için kısa kodları desteklemeyiz.

## <a name="billing"></a>Faturalandırma

Çoğu faturalandırma [sorusu, Çok Faktörlü Kimlik Doğrulama Fiyatlandırması sayfasına](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) veya [Azure Çok Faktörlü Kimlik Doğrulama sürümleri ve tüketim planları](concept-mfa-licensing.md)için belgelere atıfta bulunarak yanıtlanabilir.

* [Kuruluşum, kimlik doğrulaması için kullanılan telefon aramaları ve kısa mesajları gönderdiği için ücretlendirilir mi?](#is-my-organization-charged-for-sending-the-phone-calls-and-text-messages-that-are-used-for-authentication)
* [Kullanıcı başına faturalandırma modeli, tüm etkin kullanıcılar için mi yoksa yalnızca iki aşamalı doğrulama gerçekleştirenler için mi ücret alıyor?](#does-the-per-user-billing-model-charge-me-for-all-enabled-users-or-just-the-ones-that-performed-two-step-verification)
* [Çok Faktörlü Kimlik Doğrulama faturalandırması nasıl çalışır?](#how-does-multi-factor-authentication-billing-work)
* [Azure Çok Faktörlü Kimlik Doğrulama'nın ücretsiz bir sürümü var mı?](#is-there-a-free-version-of-azure-multi-factor-authentication)
* [Kuruluşum herhangi bir zamanda kullanıcı başına ve kimlik doğrulama başına tüketim faturalandırma modelleri arasında geçiş yapabilir mi?](#can-my-organization-switch-between-per-user-and-per-authentication-consumption-billing-models-at-any-time)
* [Kuruluşum herhangi bir zamanda tüketim tabanlı faturalandırma ve abonelikler (lisans tabanlı model) arasında geçiş yapabilir mi?](#can-my-organization-switch-between-consumption-based-billing-and-subscriptions-a-license-based-model-at-any-time)
* [Kuruluşum, Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmak için kimlikleri kullanmak ve senkronize etmek zorunda mı?](#does-my-organization-have-to-use-and-synchronize-identities-to-use-azure-multi-factor-authentication)

### <a name="is-my-organization-charged-for-sending-the-phone-calls-and-text-messages-that-are-used-for-authentication"></a>Kuruluşum, kimlik doğrulaması için kullanılan telefon aramaları ve kısa mesajları gönderdiği için ücretlendirilir mi?

Hayır, Azure Çok Faktörlü Kimlik Doğrulaması aracılığıyla kullanıcılara gönderilen tek tek telefon görüşmeleri veya kısa mesajlar için ücretlendirilmezsiniz. Kimlik doğrulama MFA sağlayıcısı kullanıyorsanız, her kimlik doğrulama için faturalandırılırsınız, ancak kullanılan yöntem için faturalandırılamazsınız.

Kullanıcılarınız, kişisel telefon hizmetine göre, aldıkları telefon görüşmeleri veya kısa mesajlar için ücretlendirilebilir.

### <a name="does-the-per-user-billing-model-charge-me-for-all-enabled-users-or-just-the-ones-that-performed-two-step-verification"></a>Kullanıcı başına faturalandırma modeli, tüm etkin kullanıcılar için mi yoksa yalnızca iki aşamalı doğrulama gerçekleştirenler için mi ücret alıyor?

Faturalandırma, o ay iki aşamalı doğrulama yapıp yaptıklarına bakılmaksızın Çok Faktörlü Kimlik Doğrulama'yı kullanmak üzere yapılandırılan kullanıcı sayısına bağlıdır.

### <a name="how-does-multi-factor-authentication-billing-work"></a>Çok Faktörlü Kimlik Doğrulama faturalandırması nasıl çalışır?

Kullanıcı başına veya kimlik doğrulama MFA sağlayıcısı oluşturduğunuzda, kuruluşunuzun Azure aboneliği kullanıma göre aylık olarak faturalandırılır. Bu faturalandırma modeli, sanal makinelerin ve Web Uygulamalarının kullanımı için Azure faturalandırmalarına benzer.

Azure Çok Faktörlü Kimlik Doğrulama için bir abonelik satın aldığınızda, kuruluşunuz yalnızca her kullanıcı için yıllık lisans ücretini öder. MFA lisansları ve Office 365, Azure AD Premium veya Kurumsal Mobilite + Güvenlik paketleri bu şekilde faturalandırılır.

Daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama'yı nasıl edinirebilirsiniz.](concept-mfa-licensing.md)

### <a name="is-there-a-free-version-of-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama'nın ücretsiz bir sürümü var mı?

Azure AD Free katmanında güvenlik varsayılanları etkinleştirilebilir. Güvenlik varsayılanları ile, microsoft Authenticator uygulamasını kullanarak tüm kullanıcılar çok faktörlü kimlik doğrulaması için etkinleştirilir. Yalnızca Microsoft Authenticator uygulaması, güvenlik varsayılanları ile metin mesajı veya telefon doğrulaması kullanma olanağı yoktur.

Daha fazla bilgi için [bkz.](../fundamentals/concept-fundamentals-security-defaults.md)

### <a name="can-my-organization-switch-between-per-user-and-per-authentication-consumption-billing-models-at-any-time"></a>Kuruluşum herhangi bir zamanda kullanıcı başına ve kimlik doğrulama başına tüketim faturalandırma modelleri arasında geçiş yapabilir mi?

Kuruluşunuz tüketim tabanlı faturalandırmaile bağımsız bir hizmet olarak MFA satın alıyorsa, bir MFA sağlayıcısı oluşturduğunuzda bir faturalandırma modeli seçersiniz. Bir MFA sağlayıcısı oluşturulduktan sonra faturalandırma modelini değiştiremezsiniz. 

MFA sağlayıcınız bir Azure AD kiracısına bağlı *değilse* veya yeni MFA sağlayıcısını farklı bir Azure AD kiracısına bağlarsanız, kullanıcı ayarları ve yapılandırma seçenekleri aktarılmaz. Ayrıca, yeni MFA Sağlayıcısı ile oluşturulan etkinleştirme kimlik bilgileri kullanılarak mevcut Azure MFA Sunucularının yeniden etkinleştirilmesi gerekir. MFA Sunucularını yeni MFA Sağlayıcısına bağlamak için yeniden etkinleştirmek, telefon çağrısı ve kısa mesaj kimlik doğrulamasını etkilemez, ancak mobil uygulama etkinleştirilinceye kadar tüm kullanıcılar için mobil uygulama bildirimleri çalışmaz.

[Azure Çok Faktörlü Auth Sağlayıcısı'na başlarken](concept-mfa-authprovider.md)MFA sağlayıcıları hakkında daha fazla bilgi edinin.

### <a name="can-my-organization-switch-between-consumption-based-billing-and-subscriptions-a-license-based-model-at-any-time"></a>Kuruluşum herhangi bir zamanda tüketim tabanlı faturalandırma ve abonelikler (lisans tabanlı model) arasında geçiş yapabilir mi?

Bazı durumlarda, evet.

Dizininizin *kullanıcı başına* bir Azure Çok Faktörlü Kimlik Doğrulama sağlayıcısı varsa, MFA lisansları ekleyebilirsiniz. Lisansı olan kullanıcılar, kullanıcı başına tüketim tabanlı faturalandırmada dikkate alınmaz. Lisansı olmayan kullanıcılar MFA sağlayıcısı aracılığıyla MFA için etkinleştirilebilir. Çok Faktörlü Kimlik Doğrulama'yı kullanmak üzere yapılandırılan tüm kullanıcılarınız için lisans satın alıp atarsanız, Azure Çok Faktörlü Kimlik Doğrulama sağlayıcısını silebilirsiniz. Gelecekte lisanslardan daha fazla kullanıcınız varsa, kullanıcı başına her zaman başka bir MFA sağlayıcısı oluşturabilirsiniz.

Dizininizin *kimlik başına* bir Azure Çok Faktörlü Kimlik Doğrulama sağlayıcısı varsa, MFA sağlayıcısı aboneliğinize bağlı olduğu sürece her kimlik doğrulaması için her zaman faturalandırılırsınız. Kullanıcılara MFA lisansları atayabilirsiniz, ancak mfa lisansı atanmış veya atanmış biri tarafından gelsin, her iki aşamalı doğrulama isteği için faturalandırılırsınız.

### <a name="does-my-organization-have-to-use-and-synchronize-identities-to-use-azure-multi-factor-authentication"></a>Kuruluşum, Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmak için kimlikleri kullanmak ve senkronize etmek zorunda mı?

Kuruluşunuz tüketim tabanlı bir faturalandırma modeli kullanıyorsa, Azure Etkin Dizin isteğe bağlıdır, ancak gerekli değildir. MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse, Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'na yalnızca şirket içinde dağıtabilirsiniz.

Satın aldığınızda ve dizindeki kullanıcılara atadığınızda lisanslar Azure AD kiracısına eklendiğinden, lisanslar lisans modeli için Azure Etkin Dizin gereklidir.

## <a name="manage-and-support-user-accounts"></a>Kullanıcı hesaplarını yönetme ve destekleme

* [Kullanıcılarıma telefonlarında yanıt almazlarsa ne yapmaları gerektiğini söylemeliyim?](#what-should-i-tell-my-users-to-do-if-they-dont-receive-a-response-on-their-phone)
* [Kullanıcılarımdan biri hesabına giremezse ne yapmalıyım?](#what-should-i-do-if-one-of-my-users-cant-get-in-to-their-account)
* [Kullanıcılarımdan biri uygulama parolalarını kullanan bir telefonu kaybederse ne yapmalıyım?](#what-should-i-do-if-one-of-my-users-loses-a-phone-that-is-using-app-passwords)
* [Bir kullanıcı tarayıcı dışı uygulamalarda oturum açamazsa ne olur?](#what-if-a-user-cant-sign-in-to-non-browser-apps)
* [Kullanıcılarım bazen kısa mesaj veya doğrulama süreleri almazsınız söylüyorlar.](#my-users-say-that-sometimes-they-dont-receive-the-text-message-or-the-verification-times-out)
* [Kullanıcılarımın sistem çalışma saatleri bitmeden önce bir kısa mesajdan doğrulama kodunu girmeleri gereken süreyi değiştirebilir miyim?](#can-i-change-the-amount-of-time-my-users-have-to-enter-the-verification-code-from-a-text-message-before-the-system-times-out)
* [Azure Çok Faktörlü Kimlik Doğrulama Sunucusu ile donanım belirteçlerini kullanabilir miyim?](#can-i-use-hardware-tokens-with-azure-multi-factor-authentication-server)
* [Terminal Hizmetlerini güvence altına almak için Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nı kullanabilir miyim?](#can-i-use-azure-multi-factor-authentication-server-to-secure-terminal-services)
* [Arayan Kimliğini MFA Server'da yapılandırdım, ancak kullanıcılarım hala anonim bir arayandan Çok Faktörlü Kimlik Doğrulama çağrıları alır.](#i-configured-caller-id-in-mfa-server-but-my-users-still-receive-multi-factor-authentication-calls-from-an-anonymous-caller)
* [Neden kullanıcılarımdan güvenlik bilgilerini kaydetmeleri isteniyor?](#why-are-my-users-being-prompted-to-register-their-security-information)

### <a name="what-should-i-tell-my-users-to-do-if-they-dont-receive-a-response-on-their-phone"></a>Kullanıcılarıma telefonlarında yanıt almazlarsa ne yapmaları gerektiğini söylemeliyim?

Kullanıcılarınızın kimlik doğrulaması için bir telefon görüşmesi veya SMS almak için 5 dakikada en fazla beş kez denemesini bekleyin. Microsoft, arama ve SMS iletileri sunmak için birden çok sağlayıcı kullanır. Bu yaklaşım işe yaramazsa, daha fazla sorun gidermek için bir destek örneği açın.

Üçüncü taraf güvenlik uygulamaları da doğrulama kodu kısa mesaj veya telefon görüşmesi engelleyebilir. Bir üçüncü taraf güvenlik uygulaması kullanıyorsanız, korumayı devre dışı bırakmayı deneyin ve başka bir MFA doğrulama kodunun gönderilmesini isteyin.

Yukarıdaki adımlar işe yaramazsa, kullanıcıların birden fazla doğrulama yöntemi için yapılandırıp yapılandırılmadığını kontrol edin. Yeniden oturum açmayı deneyin, ancak oturum açma sayfasında farklı bir doğrulama yöntemi seçin.

Daha fazla bilgi için [son kullanıcı sorun giderme kılavuzuna](../user-help/multi-factor-authentication-end-user-troubleshoot.md)bakın.

### <a name="what-should-i-do-if-one-of-my-users-cant-get-in-to-their-account"></a>Kullanıcılarımdan biri hesabına giremezse ne yapmalıyım?

Kullanıcının hesabını yeniden kayıt işleminden geçirmelerini sağlayarak sıfırlayabilirsiniz. [Bulutta Azure Çok Faktörlü Kimlik Doğrulaması ile kullanıcı ve aygıt ayarlarını yönetme](howto-mfa-userdevicesettings.md)hakkında daha fazla bilgi edinin.

### <a name="what-should-i-do-if-one-of-my-users-loses-a-phone-that-is-using-app-passwords"></a>Kullanıcılarımdan biri uygulama parolalarını kullanan bir telefonu kaybederse ne yapmalıyım?

Yetkisiz erişimi önlemek için, tüm kullanıcının uygulama parolalarını silin. Kullanıcı değiştirme aygıtına sahip olduktan sonra parolaları yeniden oluşturabilir. [Bulutta Azure Çok Faktörlü Kimlik Doğrulaması ile kullanıcı ve aygıt ayarlarını yönetme](howto-mfa-userdevicesettings.md)hakkında daha fazla bilgi edinin.

### <a name="what-if-a-user-cant-sign-in-to-non-browser-apps"></a>Bir kullanıcı tarayıcı dışı uygulamalarda oturum açamazsa ne olur?

Kuruluşunuz hala eski istemciler kullanıyorsa ve [uygulama parolalarının kullanılmasına izin](howto-mfa-mfasettings.md#app-passwords)verdiyseniz, kullanıcılarınız bu eski istemcilerde kullanıcı adı ve parolalarıyla oturum açamayabilir. Bunun yerine, [uygulama parolaları ayarlamaları](../user-help/multi-factor-authentication-end-user-app-passwords.md)gerekir. Kullanıcılarınızın oturum açma bilgilerini temizlemesi (silmeleri), uygulamayı yeniden başlatmaları ve normal parolaları yerine kullanıcı adı ve *uygulama parolalarıyla* oturum açmaları gerekir.

Kuruluşunuzun eski istemcileri yoksa, kullanıcılarınızın uygulama parolaları oluşturmasına izin vermemelidir.

> [!NOTE]
> **Office 2013 istemcileri için modern kimlik doğrulaması**
>
> Uygulama parolaları yalnızca modern kimlik doğrulamasını desteklemeyen uygulamalar için gereklidir. Office 2013 istemcileri modern kimlik doğrulama protokollerini destekler, ancak yapılandırılması gerekir. Modern kimlik doğrulaması, Office 2013 için Mart 2015 veya daha sonra güncelleştirmeyi çalıştıran tüm müşteriler tarafından kullanılabilir. Daha fazla bilgi için, blog yazısı [Güncelleştirilmiş Office 365 modern kimlik doğrulama](https://www.microsoft.com/microsoft-365/blog/2015/11/19/updated-office-365-modern-authentication-public-preview/)bakın.

### <a name="my-users-say-that-sometimes-they-dont-receive-the-text-message-or-the-verification-times-out"></a>Kullanıcılarım bazen kısa mesaj veya doğrulama süreleri almazsınız söylüyorlar.

Hizmetin güvenilirliğini etkileyebilecek denetilemez etkenler olduğundan SMS iletilerinin teslimi garanti değildir. Bu faktörler hedef ülke veya bölge, cep telefonu operatörü ve sinyal gücü içerir.

Üçüncü taraf güvenlik uygulamaları da doğrulama kodu kısa mesaj veya telefon görüşmesi engelleyebilir. Bir üçüncü taraf güvenlik uygulaması kullanıyorsanız, korumayı devre dışı bırakmayı deneyin ve başka bir MFA doğrulama kodunun gönderilmesini isteyin.

Kullanıcılarınız genellikle güvenilir metin iletileri alma ile ilgili sorunlar yaşıyorsanız, bunun yerine Microsoft Authenticator uygulamasını veya telefon görüşmesi yöntemini kullanmalarını söyleyin. Microsoft Authenticator hem hücresel hem de Wi-Fi bağlantıları üzerinden bildirim alabilir. Buna ek olarak, mobil uygulama, aygıtın hiç sinyali olmasa bile doğrulama kodları oluşturabilir. Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?Linkid=825072), [iOS](https://go.microsoft.com/fwlink/?Linkid=825073) ve [Windows Phone](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6)'da kullanılabilir.

### <a name="can-i-change-the-amount-of-time-my-users-have-to-enter-the-verification-code-from-a-text-message-before-the-system-times-out"></a>Kullanıcılarımın sistem çalışma saatleri bitmeden önce bir kısa mesajdan doğrulama kodunu girmeleri gereken süreyi değiştirebilir miyim?

Bazı durumlarda, evet.

Azure MFA Server v7.0 veya üzeri ile tek yönlü SMS için, bir kayıt defteri anahtarı ayarlayarak zaman aşımını yapılandırabilirsiniz. MFA bulut hizmeti kısa mesajı gönderdikten sonra doğrulama kodu (veya bir kerelik parola) MFA Sunucusuna döndürülür. MFA Server varsayılan olarak kodu 300 saniye boyunca bellekte saklar. Kullanıcı 300 saniye geçmeden kodu girmezse, kimlik doğrulaması reddedilir. Varsayılan zaman ayarı değiştirmek için aşağıdaki adımları kullanın:

1. `HKLM\Software\Wow6432Node\Positive Networks\PhoneFactor` kısmına gidin.
2. *pfsvc_pendingSmsTimeoutSeconds* adında bir **DWORD** kayıt defteri anahtarı oluşturun ve Azure MFA Sunucusu'nun tek seferlik geçiş kodlarını depolamasını istediğiniz zamanı saniyeler içinde ayarlayın.

>[!TIP]
>
> Birden çok MFA Sunucunuz varsa, yalnızca özgün kimlik doğrulama isteğini işleyen kullanıcıya gönderilen doğrulama kodunu bilir. Kullanıcı kodu girdiğinde, doğrulama isteğinin aynı sunucuya gönderilmesi gerekir. Kod doğrulama sı farklı bir sunucuya gönderilirse, kimlik doğrulaması reddedilir.

Kullanıcılar SMS'e tanımlanan zaman dilimi içinde yanıt vermezlerse, kimlik doğrulamaları reddedilir.

Bulutta Azure MFA'sı olan tek yönlü SMS'lerde (AD FS bağdaştırıcısı veya Network Policy Server uzantısı dahil), zaman aşım ayarını yapılandıramazsınız. Azure AD doğrulama kodunu 180 saniye boyunca depolar.

### <a name="can-i-use-hardware-tokens-with-azure-multi-factor-authentication-server"></a>Azure Çok Faktörlü Kimlik Doğrulama Sunucusu ile donanım belirteçlerini kullanabilir miyim?

Azure Çok Faktörlü Kimlik Doğrulama Sunucusu kullanıyorsanız, üçüncü taraf Açık Kimlik Doğrulama (OATH) zaman tabanlı, tek kullanımlık parola (TOTP) belirteçlerini içe aktarabilir ve bunları iki aşamalı doğrulama için kullanabilirsiniz.

Gizli anahtarı bir CSV dosyasına koyup Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'na aktarıyorsanız, OATH TOTP belirteçleri olan ActiveIdentity belirteçlerini kullanabilirsiniz. Active Directory Federation Services (ADFS), Internet Information Server (IIS) formlarına dayalı kimlik doğrulama ve Uzaktan Kimlik Doğrulama Arama Kullanıcı Hizmeti (RADIUS) ile oath belirteçlerini, istemci sistemi kullanıcı girişini kabul edebileceği sürece kullanabilirsiniz.

Aşağıdaki biçimlerle üçüncü taraf OATH TOTP belirteçleri içe aktarabilirsiniz:  

- Taşınabilir Simetrik Anahtar Konteyneri (PSKC)
- Dosya bir seri numarası, Base 32 formatında gizli bir anahtar ve bir zaman aralığı içeriyorsa CSV

### <a name="can-i-use-azure-multi-factor-authentication-server-to-secure-terminal-services"></a>Terminal Hizmetlerini güvence altına almak için Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nı kullanabilir miyim?

Evet, ancak Windows Server 2012 R2 veya daha yeni bir süre kullanıyorsanız, Terminal Hizmetlerini yalnızca Uzak Masaüstü Ağ Geçidi (RD Ağ Geçidi) kullanarak güvence altına alabilirsiniz.

Windows Server 2012 R2'deki güvenlik değişiklikleri, Azure Çok Faktörlü Authentication Server'ın Windows Server 2012 ve önceki sürümlerde Yerel Güvenlik Otoritesi (LSA) güvenlik paketine bağlanma şeklini değiştirdi. Windows Server 2012 veya önceki Sürüm Hizmetleri sürümleri için, [Windows Kimlik Doğrulama ile bir uygulama güvenli](howto-mfaserver-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure)olabilir. Windows Server 2012 R2 kullanıyorsanız, RD Ağ Geçidi'ne ihtiyacınız vardır.

### <a name="i-configured-caller-id-in-mfa-server-but-my-users-still-receive-multi-factor-authentication-calls-from-an-anonymous-caller"></a>Arayan Kimliğini MFA Server'da yapılandırdım, ancak kullanıcılarım hala anonim bir arayandan Çok Faktörlü Kimlik Doğrulama çağrıları alır.

Çok Faktörlü Kimlik Doğrulama aramaları ortak telefon ağı üzerinden yerleştirildiğinde, bazen arayan kimliğini desteklemeyen bir operatör aracılığıyla yönlendirilir. Bu operatör davranışı nedeniyle, Çok Faktörlü Kimlik Doğrulama sistemi her zaman gönderse de arayan kimliği garanti etmez.

### <a name="why-are-my-users-being-prompted-to-register-their-security-information"></a>Neden kullanıcılarımdan güvenlik bilgilerini kaydetmeleri isteniyor?

Kullanıcıların güvenlik bilgilerini kaydetmelerinin istenmesi için çeşitli nedenler vardır:

- Kullanıcı, Azure AD'deki yöneticisi tarafından MFA için etkinleştirildi, ancak henüz hesabına kayıtlı güvenlik bilgisi yok.
- Kullanıcı, Azure AD'de self servis parola sıfırlama için etkinleştirildi. Güvenlik bilgileri, parolalarını unuturlarsa gelecekte sıfırlamalarına yardımcı olur.
- Kullanıcı, MFA gerektiren koşullu erişim ilkesine sahip ve daha önce MFA'ya kaydedilmemiş bir uygulamaya erişmiştir.
- Kullanıcı bir aygıtı Azure AD'ye kaydediyor (Azure AD Join dahil) ve kuruluşunuz cihaz kaydı için MFA gerektirir, ancak kullanıcı daha önce MFA'ya kaydolmamıştır.
- Kullanıcı Windows 10'da İşletmeler için Windows Hello üretiyor (MFA gerektirir) ve daha önce MFA'ya kaydolmadı.
- Kuruluş, kullanıcıya uygulanan bir MFA Kayıt ilkesi oluşturmuş ve etkinleştirmiştir.
- Kullanıcı daha önce MFA'ya kaydoldu, ancak yöneticinin devre dışı bırakıldığı bir doğrulama yöntemi seçti. Bu nedenle, yeni bir varsayılan doğrulama yöntemi seçmek için kullanıcının MFA kaydından tekrar geçmesi gerekir.

## <a name="errors"></a>Hatalar

* [Kullanıcılar mobil uygulama bildirimlerini kullanırken "Kimlik doğrulama isteği etkinleştirilmiş bir hesap için değildir" hata iletisini görürlerse ne yapmalıdır?](#what-should-users-do-if-they-see-an-authentication-request-is-not-for-an-activated-account-error-message-when-using-mobile-app-notifications)
* [Tarayıcı olmayan bir uygulamada oturum açınca 0x800434D4L hata iletisi gören kullanıcılar ne yapmalıdır?](#what-should-users-do-if-they-see-a-0x800434d4l-error-message-when-signing-in-to-a-non-browser-application)

### <a name="what-should-users-do-if-they-see-an-authentication-request-is-not-for-an-activated-account-error-message-when-using-mobile-app-notifications"></a>Kullanıcılar mobil uygulama bildirimlerini kullanırken "Kimlik doğrulama isteği etkinleştirilmiş bir hesap için değildir" hata iletisini görürlerse ne yapmalıdır?

Kullanıcıdan hesabını Microsoft Authenticator'dan kaldırması için aşağıdaki yordamı tamamlamasını isteyin ve sonra yeniden ekleyin:

1. Azure [portal profillerine](https://account.activedirectory.windowsazure.com/profile/) gidin ve kuruluş hesabıyla oturum açın.
2. **Ek Güvenlik Doğrulaması'nı**seçin.
3. Varolan hesabı Microsoft Authenticator uygulamasından kaldırın.
4. **Yapılaşı'yı**tıklatın ve ardından Microsoft Authenticator'ı yeniden yapılandırmak için yönergeleri izleyin.

### <a name="what-should-users-do-if-they-see-a-0x800434d4l-error-message-when-signing-in-to-a-non-browser-application"></a>Tarayıcı olmayan bir uygulamada oturum açınca 0x800434D4L hata iletisi gören kullanıcılar ne yapmalıdır?

*0x800434D4L* hatası, iki aşamalı doğrulama gerektiren hesaplarda çalışmayan yerel bir bilgisayarda yüklü tarayıcı olmayan bir uygulamada oturum açmaya çalıştığınızda oluşur.

Bu hata için geçici çözüm, yönetici ile ilgili ve yönetici olmayan işlemler için ayrı kullanıcı hesaplarına sahip olmaktır. Daha sonra, yönetici olmayan hesabınızı kullanarak Outlook'ta oturum açabilmeniz için posta kutularını yönetici hesabınız la yönetici olmayan hesabınız arasında bağlayabilirsiniz. Bu çözüm hakkında daha fazla bilgi için, [yöneticiye bir kullanıcının posta kutusunun içeriğini açma ve görüntüleme olanağı nı](https://help.outlook.com/141/gg709759.aspx?sl=1)nasıl vereceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Sorunuz burada yanıtlanmamışsa, aşağıdaki destek seçenekleri mevcuttur:

* Sık karşılaşılan teknik sorunlara çözüm bulmak için [Microsoft Destek Bilgi Bankası'nda](https://support.microsoft.com) arama yapın.
* [Azure Active Directory Q&A'da](https://docs.microsoft.com/answers/topics/azure-active-directory.html)topluluktan teknik soruları ve yanıtları arayın ve göz atın.
* [Azure Multi-Factor Authentication Server desteği](https://support.microsoft.com/oas/default.aspx?prid=14947)aracılığıyla Microsoft profesyoneline başvurun. Bizimle iletişim e-son ları yaparken, sorununuz hakkında mümkün olduğunca çok bilgi eklemeniz yararlı olacaktır. Verebileceğiniz bilgiler, hatayı gördüğünüz sayfayı, belirli hata kodunu, belirli oturum kimliğini ve hatayı gören kullanıcının kimliğini içerir.
* Eski bir PhoneFactor müşterisiyseniz ve parolayı sıfırlama konusunda sorularınız varsa [phonefactorsupport@microsoft.com](mailto:phonefactorsupport@microsoft.com) veya yardıma ihtiyacınız varsa, destek servis talebi açmak için e-posta adresini kullanın.
