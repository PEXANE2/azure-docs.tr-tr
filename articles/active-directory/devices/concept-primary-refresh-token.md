---
title: Birincil Yenileme Belirteci (PRT) ve Azure AD - Azure Etkin Dizini
description: Azure Etkin Dizini'ndeki Birincil Yenileme Belirteci'nin (PRT) rolü nedir ve nasıl yönetiriz?
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a237ad35d9d5d8abee784926563d972d0ee95f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672643"
---
# <a name="what-is-a-primary-refresh-token"></a>Birincil Yenileme Belirteci nedir?

Birincil Yenileme Belirteci (PRT), Windows 10, iOS ve Android cihazlarda Azure AD kimlik doğrulamasının önemli bir eseridir. Bu, bu aygıtlarda kullanılan uygulamalar arasında tek oturum açma (SSO) sağlamak için Microsoft birinci taraf belirteç brokerlarına özel olarak verilen bir JSON Web Belirtecidir (JWT). Bu makalede, Windows 10 aygıtlarında bir PRT'nin nasıl verildiği, kullanıldığı ve korunduğu hakkında ayrıntılı bilgi vereceğiz.

Bu makalede, Azure AD'de kullanılabilen farklı aygıt durumlarını ve Windows 10'da tek oturum açmanın nasıl çalıştığını zaten anladığınızı varsayar. Azure AD'deki aygıtlar hakkında daha fazla bilgi için, [Azure Etkin Dizini'nde aygıt yönetimi nedir makalesine bakın?](overview.md)

## <a name="key-terminology-and-components"></a>Temel terminoloji ve bileşenler

Aşağıdaki Windows bileşenleri bir PRT isteme ve kullanmada önemli bir rol oynar:

* **Bulut Kimlik Doğrulama Sağlayıcısı** (CloudAP): CloudAP, Windows oturum açma için kullanıcıların Windows 10 aygıtına günlüğe kaydolduğunu doğrulayan modern kimlik doğrulama sağlayıcısıdır. CloudAP, kimlik sağlayıcılarının, bu kimlik sağlayıcısının kimlik bilgilerini kullanarak Windows'a kimlik doğrulamayı etkinleştirmek için oluşturabileceği bir eklenti çerçevesi sağlar.
* **Web Hesap Yöneticisi** (WAM): WAM, Windows 10 aygıtlarında varsayılan belirteç aracısıdır. WAM ayrıca kimlik sağlayıcılarının üzerine oluşturabileceği ve SSO'nun bu kimlik sağlayıcısına güvenerek uygulamalarına olanak tanıyan bir eklenti çerçevesi sağlar.
* **Azure AD CloudAP eklentisi**: Windows oturum açma sırasında kullanıcı kimlik bilgilerini Azure AD ile doğrulayan CloudAP çerçevesi üzerinde oluşturulmuş azure AD özel eklentisi.
* **Azure AD WAM eklentisi**: SSO'nun kimlik doğrulama için Azure AD'ye güvenen uygulamalara olanak tanıyan WAM çerçevesi üzerine inşa edilmiş Bir Azure AD özel eklentisi.
* **Dsreg**: Windows 10'da tüm aygıt durumları için aygıt kayıt işlemini yürüten Azure AD'ye özgü bir bileşendir.
* **Güvenilen Platform Modülü** (TPM): TPM, kullanıcı ve aygıt sırları için donanım tabanlı güvenlik işlevleri sağlayan bir aygıtta yerleşik bir donanım bileşenidir. Daha fazla bilgi makalede [Güvenilir Platform Modülü Teknoloji Genel Bakış](/windows/security/information-protection/tpm/trusted-platform-module-overview)bulunabilir.

## <a name="what-does-the-prt-contain"></a>İİT'de ne var?

PRT genellikle herhangi bir Azure AD yenileme belirtecinde bulunan talepleri içerir. Buna ek olarak, PRT dahil bazı cihaza özgü iddialar vardır. Bunlar şu şekildedir:

* **Cihaz Kimliği**: Belirli bir cihazda kullanıcıya BIR PRT verilir. Aygıt kimliği `deviceID` iddiası, PRT'nin kullanıcıya verildiği aygıtı belirler. Bu talep daha sonra İİT aracılığıyla elde edilen belirteçlere verilir. Aygıt kimliği talebi, aygıt durumuna veya uyumluluğuna göre Koşullu Erişim yetkilendirmesini belirlemek için kullanılır.
* **Oturum anahtarı**: Oturum anahtarı, Azure AD kimlik doğrulama hizmeti tarafından oluşturulan ve PRT'nin bir parçası olarak verilen şifreli simetrik bir anahtardır. Oturum anahtarı, diğer uygulamalar için belirteçler elde etmek için bir PRT kullanıldığında sahip olma kanıtı olarak görür.

### <a name="can-i-see-whats-in-a-prt"></a>İİT'de ne olduğunu görebilir miyim?

PRT, Azure AD'den gönderilen ve içeriği hiçbir istemci bileşeni tarafından bilinmeyen opak bir blob'dur. Bir İİT'nin içinde ne olduğunu göremezsin.

## <a name="how-is-a-prt-issued"></a>İİT nasıl verilir?

Aygıt kaydı, Azure AD'deki aygıt tabanlı kimlik doğrulaması için bir ön koşuldur. Bir PRT yalnızca kayıtlı cihazlarda kullanıcılara verilir. Cihaz kaydı yla ilgili daha ayrıntılı bilgi için, [İş ve Aygıt Kaydı için Windows Hello](/windows/security/identity-protection/hello-for-business/hello-how-it-works-device-registration)makalesine bakın. Aygıt kaydı sırasında, dsreg bileşeni iki şifreleme anahtar çifti kümesi oluşturur:

* Cihaz anahtarı (dkpub/dkpriv)
* Taşıma anahtarı (tkpub/tkpriv)

Aygıtın geçerli ve işleyen bir TPM'si varsa, ortak anahtarlar aygıt kayıt işlemi sırasında Azure AD'ye gönderilirken, özel anahtarlar aygıtın TPM'sine bağlıdır. Bu anahtarlar, PRT istekleri sırasında aygıt durumunu doğrulamak için kullanılır.

PRT, windows 10 aygıtında kullanıcı kimlik doğrulaması sırasında iki senaryoda verilir:

* **Azure AD katıldı** veya **Karma Azure AD katıldı**: Windows oturum açma sırasında bir kullanıcı kuruluş kimlik bilgilerini oturum açtıklarında bir PRT verilir. Bir PRT tüm Windows 10 desteklenen kimlik bilgileri ile verilir , örneğin, parola ve Windows Hello İş için. Bu senaryoda, Azure AD CloudAP eklentisi PRT'nin birincil yetkilisidir.
* **Azure AD kayıtlı aygıt**: Bir kullanıcı Windows 10 cihazına ikincil bir iş hesabı eklediğinde BIR PRT verilir. Kullanıcılar Windows 10'a iki farklı şekilde hesap ekleyebilir -  
   * Bir uygulamada oturum açtıktan sonra **bu cihazda her yerde bu hesabı kullan** üzerinden hesap ekleme (örneğin, Outlook)
   * **Ayarlar** > **Hesapları** > **Erişim Çalışması veya Okul** > **Bağlantısı'ndan** hesap ekleme

Azure AD kayıtlı aygıt senaryolarında, Windows oturumu bu Azure AD hesabında gerçekleşmediğinden, Azure AD WAM eklentisi PRT'nin birincil yetkisidir.

> [!NOTE]
> Üçüncü taraf kimlik sağlayıcılarının Windows 10 aygıtlarında PRT verme yi etkinleştirmek için WS-Trust protokolünü desteklemesi gerekir. WS-Trust olmadan, PrT, Karma Azure AD'de veya Azure AD'de birleştirilmiş aygıtlarda kullanıcılara verilemez

## <a name="what-is-the-lifetime-of-a-prt"></a>Bir İİT'nin ömrü nedir?

Bir kez yayımlandıktan sonra, bir PRT 14 gün boyunca geçerlidir ve kullanıcı cihazı aktif olarak kullandığı sürece sürekli olarak yenilenir.  

## <a name="how-is-a-prt-used"></a>İİT nasıl kullanılır?

PRT, Windows'da iki temel bileşen tarafından kullanılır:

* **Azure AD CloudAP eklentisi**: Windows oturum açma sırasında Azure AD CloudAP eklentisi, kullanıcı tarafından sağlanan kimlik bilgilerini kullanarak Azure AD'den bir PRT ister. Ayrıca, kullanıcının internet bağlantısına erişimi olmadığında önbelleğe alınmış oturum açmayı etkinleştirmek için PRT'yi önbelleğe adar.
* **Azure AD WAM eklentisi**: Kullanıcılar uygulamalara erişmeye çalıştıklarında, Azure AD WAM eklentisi Windows 10'da SSO'yu etkinleştirmek için PRT'yi kullanır. Azure AD WAM eklentisi, belirteç istekleri için WAM'a dayanan uygulamalar için yenileme ve erişim belirteçleri istemek için PRT'yi kullanır. Ayrıca, prt'yi tarayıcı isteklerine enjekte ederek tarayıcılarda SSO'ya olanak tanır. Windows 10'daki Tarayıcı SSO'su Microsoft Edge (yerel olarak) ve Chrome'da (Windows 10 Hesapları veya Office Online uzantısı üzerinden) desteklenir.

## <a name="how-is-a-prt-renewed"></a>Bir PRT nasıl yenilenir?

Bir PRT iki farklı yöntemle yenilenir:

* **Azure AD CloudAP eklentisi**: CloudAP eklentisi, Windows oturum açma sırasında her 4 saatte bir PRT'yi yeniler. Bu süre zarfında kullanıcının internet bağlantısı yoksa, cloudap eklentisi cihaz internete bağlandıktan sonra İİT'yi yeniler.
* **Uygulama belirteç istekleri sırasında Azure AD WAM eklentisi**: WAM eklentisi, uygulamalar için sessiz belirteç isteklerini etkinleştirerek Windows 10 cihazlarında SSO'yu etkinleştiriyor. WAM eklentisi bu belirteç istekleri sırasında İİT'yi iki farklı şekilde yenileyebilir:
   * Bir uygulama WAM'dan erişim belirteci için sessizce talep tespresin, ancak bu uygulama için yenilenme belirteci bulunmamaktadır. Bu durumda, WAM uygulama için bir belirteç istemek için PRT kullanır ve yanıt olarak yeni bir PRT geri alır.
   * Bir uygulama WAM'dan erişim jetonu ister, ancak PRT geçersizdir veya Azure AD ek yetkilendirme gerektirir (örneğin, Azure Çok Faktörlü Kimlik Doğrulaması). Bu senaryoda, WAM kullanıcının yeniden kimlik doğrulaması vermesini veya ek doğrulama sağlamasını gerektiren etkileşimli bir oturum açar ve başarılı kimlik doğrulama üzerine yeni bir PRT verilir.

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular

* Bir PRT yalnızca yerel uygulama kimlik doğrulaması sırasında verilir ve yenilenir. Bir PRT, tarayıcı oturumu sırasında yenilenmez veya yayınlanmaz.
* Azure AD'nin birleştiği ve azure AD'nin bağlanan aygıtlarda hibrit olarak katıldığı cihazlarda, CloudAP eklentisi bir PRT'nin birincil yetkisidir. BIR PRT WAM tabanlı belirteç isteği sırasında yenilenirse, PRT cloudap eklentisine geri gönderilir ve bu da prt'nin Azure AD ile geçerliliğini kabul etmeden önce doğrular.

## <a name="how-is-the-prt-protected"></a>İİT nasıl korunur?

Bir PRT, kullanıcının oturum açtığı aygıta bağlanarak korunur. Azure AD ve Windows 10, aşağıdaki yöntemlerle PRT koruması sağlar:

* **İlk oturum açma sırasında**: İlk oturum açma sırasında, cihaz kaydı sırasında şifreleme olarak oluşturulan aygıt anahtarı kullanılarak istekleri imzalayarak bir PRT verilir. Geçerli ve işleyen bir TPM'ye sahip bir aygıtta, aygıt anahtarı TPM tarafından güvenli hale getirilerek kötü amaçlı erişimi engeller. İlgili aygıt anahtar imzası doğrulanamıyorsa PRT verilmez.
* **Belirteç istekleri ve yenileme sırasında**: Bir PRT yayımlandığında, Azure AD da aygıt için şifreli bir oturum anahtarı yayınlar. Oluşturulan toplu taşıma anahtarı (tkpub) ile şifrelenir ve aygıt kaydının bir parçası olarak Azure AD'ye gönderilir. Bu oturum anahtarı yalnızca TPM tarafından güvenli özel aktarım anahtarı (tkpriv) ile çözülebilir. Oturum anahtarı, Azure AD'ye gönderilen istekler için Sahip Olma Kanıtı (POP) anahtarıdır.  Oturum anahtarı da TPM tarafından korunur ve başka hiçbir işletim sistemi bileşeni erişemez. Belirteç istekleri veya PRT yenileme istekleri, TPM aracılığıyla bu oturum anahtarı tarafından güvenli bir şekilde imzalanır ve bu nedenle kurcalanamaz. Azure AD, aygıttan ilgili oturum anahtarı tarafından imzalanmayan tüm istekleri geçersiz kılınacaktır.

Bu anahtarları TPM ile güvence altına alarak, kötü niyetli aktörler anahtarları çalamaz veya tpm'ye erişilemese bile aygıtın fiziksel olarak ele geçirilmesine erişilemese bile İİT'yi başka bir yerde tekrar oynatamaz.  Böylece, Bir TPM kullanmak Azure AD Joined, Hybrid Azure AD'nin ve Azure AD kayıtlı aygıtlarının kimlik bilgisi hırsızlığına karşı güvenliğini büyük ölçüde artırır. Performans ve güvenilirlik için TPM 2.0, Windows 10'daki tüm Azure AD aygıt kayıt senaryoları için önerilen sürüdür.

### <a name="how-are-app-tokens-and-browser-cookies-protected"></a>Uygulama belirteçleri ve tarayıcı çerezleri nasıl korunur?

**Uygulama belirteçleri**: Bir uygulama WAM aracılığıyla belirteç istediğinde, Azure AD yenileme belirteci ve erişim belirteci yayınlar. Ancak WAM yalnızca erişim belirtecisini uygulamaya döndürür ve kullanıcının veri koruma uygulama programlama arabirimi (DPAPI) anahtarıyla şifreleyerek önbelleğindeki yenileme belirtecisini güvenli hale verir. WAM, daha fazla erişim belirteçleri vermek için oturum anahtarıyla istekleri imzalayarak yenileme belirteci'ni güvenli bir şekilde kullanır. DPAPI tuşu, Azure AD'nin kendisinde Azure AD tabanlı simetrik bir anahtarla sabitlenir. Aygıtın DPAPI tuşu ile kullanıcı profilinin şifresini çözmesi gerektiğinde, Azure AD oturum anahtarıyla şifrelenmiş DPAPI anahtarını sağlar ve CloudAP eklentisi TPM'nin şifresini çözmesini ister. Bu işlevsellik, yenileme belirteçlerinin güvenliğini sağlamada tutarlılık sağlar ve uygulamaların kendi koruma mekanizmalarını uygulamalarını önler.  

**Tarayıcı çerezleri**: Windows 10'da Azure AD, Windows 10 hesapları uzantısı aracılığıyla Internet Explorer ve Microsoft Edge'de veya Google Chrome'da tarayıcı SSO'yu destekler. Güvenlik yalnızca tanımlama bilgilerini değil, tanımlama bilgilerinin gönderildiği uç noktaları da korumak için oluşturulmuştür. Tarayıcı çerezleri, çerezleri imzalamak ve korumak için oturum tuşunu kullanarak, bir PRT ile aynı şekilde korunur.

Bir kullanıcı bir tarayıcı etkileşimi başlattığında, tarayıcı (veya uzantı) bir COM yerel istemci ana bilgisayar çağırır. Yerel istemci ana bilgisayar, sayfanın izin verilen etki alanlarından birinden olmasını sağlar. Tarayıcı, bir nonce dahil olmak üzere diğer parametreleri yerel istemci ana bilgisayara gönderebilir, ancak yerel istemci ana bilgisayar ana bilgisayar ana bilgisayar adının doğrulanmasını garanti eder. Yerel istemci ana bilgisayar CloudAP eklentisinden bir PRT tanımlama bilgisi ister ve bu çerezi TPM korumalı oturum anahtarıyla oluşturur ve imzalar. PRT çerezi oturum anahtarı yla imzalandığı için kurcalanamaz. Bu PRT çerezi, geldiği aygıtı doğrulamak için Azure AD'nin istek üstbilgisine dahildir. Chrome tarayıcısını kullanıyorsanız, yalnızca yerel istemci ana bilgisayar bildiriminde açıkça tanımlanan uzantı, rasgele uzantıların bu istekleri yapmasını engelleyebilir. Azure AD, PRT çerezini doğruladıktan sonra tarayıcıya bir oturum çerezi sağlar. Bu oturum çerezi aynı zamanda bir PRT ile verilen oturum anahtarını içerir. Sonraki istekler sırasında, oturum anahtarı etkili bir şekilde aygıta çerez bağlama ve başka bir yerden tekrarları engelleyerek doğrulanır.

## <a name="when-does-a-prt-get-an-mfa-claim"></a>Bir PRT ne zaman MFA iddiası alır?

Bir PRT, belirli senaryolarda çok faktörlü kimlik doğrulama (MFA) iddiası alabilir. Uygulamalar için belirteçleri istemek için MFA tabanlı bir PRT kullanıldığında, MFA talebi bu uygulama belirteçlerine aktarılır. Bu işlevsellik, bunu gerektiren her uygulama için MFA meydan okumasını önleyerek kullanıcılara sorunsuz bir deneyim sağlar. Bir PRT aşağıdaki yollarla bir MFA iddia alabilirsiniz:

* **Windows Hello for Business ile oturum açın**: Windows Hello for Business parolaların yerini alır ve güçlü iki faktörlü kimlik doğrulaması sağlamak için şifreleme anahtarlarını kullanır. Windows Hello for Business, aygıttaki bir kullanıcıya özgüdür ve kendisinin mfa'nın sağlanmasını gerektirir. Bir kullanıcı Windows Hello for Business ile oturum açtığınızda, kullanıcının PRT'si MFA talebi alır. Bu senaryo, akıllı kart kimlik doğrulaması ADFS'den MFA talebi üretiyorsa, akıllı kartlarla oturum açan kullanıcılar için de geçerlidir.
   * Windows Hello for Business çok faktörlü kimlik doğrulama olarak kabul edildiğinden, PRT'nin kendisi yenilendiğinde MFA iddiası güncellenir, bu nedenle kullanıcılar WIndows Hello for Business ile oturum açtıklarında MFA süresi sürekli olarak uzar
* **WAM etkileşimli oturum açma sırasında MFA**: WAM üzerinden bir belirteç isteği sırasında, bir kullanıcının uygulamaya erişmek için MFA yapması gerekiyorsa, bu etkileşim sırasında yenilenen PRT bir MFA iddiasıyla yazdırılır.
   * Bu durumda, MFA talebi sürekli olarak güncelleştirilmeyecektir, bu nedenle MFA süresi dizinde ayarlanan ömür boyu temel alınarak yapılır.
   * Bir uygulamaya erişmek için daha önceki bir PRT ve RT kullanıldığında, PRT ve RT kimlik doğrulamanın ilk kanıtı olarak kabul edilir. Yeni bir AT ikinci bir kanıt ve baskılı MFA iddia ile gerekli olacaktır. Bu da yeni bir PRT ve RT yayınlayacak.
* **Cihaz kaydı sırasında MFA**: Bir yönetici Azure AD'deki aygıt ayarlarını [MFA'nın cihazları kaydetmesini gerektirecek](device-management-azure-portal.md#configure-device-settings)şekilde yapılandıysa, kullanıcının kaydı tamamlamak için MFA yapması gerekir. Bu işlem sırasında, kullanıcıya verilen İİT'nin kayıt sırasında elde ettiği MFA talebi vardır. Bu özellik yalnızca birleştirme işlemini yapan kullanıcı için geçerlidir, bu aygıtta oturum açan diğer kullanıcılar için geçerli değildir.
   * WAM etkileşimli oturum açmasına benzer şekilde, MFA talebi sürekli olarak güncelleştirilmeyecektir, bu nedenle MFA süresi dizinde ayarlanan ömür boyu temel alınarak yapılır.

Windows 10, her kimlik bilgisi için bölünmüş bir İİT listesi tutar. Bu nedenle, Windows Hello for Business, parola veya akıllı kart için bir PRT vardır. Bu bölümleme, MFA taleplerinin kullanılan kimlik bilgilerine göre yalıtılmış olmasını ve belirteç istekleri sırasında karıştırılmamasını sağlar.

## <a name="how-is-a-prt-invalidated"></a>Bir PRT nasıl geçersiz kılınabilir?

Bir PRT aşağıdaki senaryolarda geçersiz kılındı:

* **Geçersiz kullanıcı**: Azure AD'de bir kullanıcı silinirse veya devre dışı bırakılırsa, PRT'si geçersiz sayılır ve uygulamalar için belirteçler elde etmek için kullanılamaz. Silinmiş veya devre dışı bırakılmış bir kullanıcı daha önce bir aygıtta oturum açmışsa, Önbelleğe alınmış oturum açma, CloudAP geçersiz durumunu fark edene kadar oturum açar. CloudAP kullanıcının geçersiz olduğunu belirledikten sonra sonraki oturum açmaları engeller. Geçersiz bir kullanıcının kimlik bilgileri önbelleğe alınmış olmayan yeni aygıtlarda oturum açması otomatik olarak engellenir.
* **Geçersiz aygıt**: Azure AD'de bir aygıt silinirse veya devre dışı bırakılırsa, söz gelimi cihazda elde edilen PRT geçersiz sayılır ve diğer uygulamalar için belirteçler elde etmek için kullanılamaz. Bir kullanıcı geçersiz bir aygıtta zaten oturum açmışsa, bunu yapmaya devam edebilir. Ancak aygıttaki tüm belirteçler geçersiz kılındı ve kullanıcının söz sahibi cihazdaki kaynaklara SSO'su yok.
* **Parola değişikliği**: Kullanıcı parolasını değiştirdikten sonra, önceki parolayla elde edilen PRT Azure AD tarafından geçersiz kılındı. Parola değişikliği, kullanıcının yeni bir PRT elde edilmesine neden olabilir. Bu geçersiz işlem iki farklı şeyi olabilir:
   * Kullanıcı windows'da yeni parolasıyla yer alırsa, CloudAP eski PRT'yi atar ve Azure AD'den yeni parolalarıyla yeni bir PRT yayınlamasını ister. Kullanıcının internet bağlantısı yoksa, yeni parola doğrulanamıyorsa, Windows kullanıcının eski parolasını girmesini gerektirebilir.
   * Bir kullanıcı eski parolasıyla oturum açmışsa veya Windows'da oturum açtıktan sonra parolasını değiştirmişse, eski PRT WAM tabanlı belirteç istekleri için kullanılır. Bu senaryoda, kullanıcıwam belirteç isteği sırasında yeniden kimlik doğrulaması istenir ve yeni bir PRT verilir.
* **TPM sorunları**: Bazen, bir aygıtın TPM'si bocalayabilir veya başarısız olabilir ve bu da TPM tarafından güvenli anahtarların erişilemezliğine yol açabilir. Bu durumda, aygıt bir PRT almak veya şifreleme anahtarları sahip kanıtlayamaz gibi varolan bir PRT kullanarak belirteçleri talep aciz. Sonuç olarak, varolan tüm PRT Azure AD tarafından geçersiz kılındı. Windows 10 bir hata algıladığında, aygıtı yeni şifreleme anahtarlarıyla yeniden kaydetmek için bir kurtarma akışı başlatır. İlk kayıtta olduğu gibi Karma Azure Reklam join ile kurtarma, kullanıcı girişi olmadan sessizce gerçekleşir. Azure AD'ye katılan veya Azure AD'ye kayıtlı aygıtlar için kurtarmanın aygıtta yönetici ayrıcalıkları olan bir kullanıcı tarafından gerçekleştirilmesi gerekir. Bu senaryoda, kurtarma akışı, kullanıcıyı aygıtı başarıyla kurtarmasına yönlendiren bir Windows istemi tarafından başlatılır.

## <a name="detailed-flows"></a>Ayrıntılı akışlar

Aşağıdaki diyagramlar, bir uygulama için erişim belirteci istemek için bir PRT verme, yenileme ve kullanmada temel ayrıntıları göstermektedir. Ayrıca, bu adımlar, söz konusu güvenlik mekanizmalarının bu etkileşimler sırasında nasıl uygulandığını da açıklar.

### <a name="prt-issuance-during-first-sign-in"></a>İlk oturum açma sırasında PRT verme

![Ayrıntılı akışilk işareti sırasında PRT verme](./media/concept-primary-refresh-token/prt-initial-sign-in.png)

> [!NOTE]
> Azure AD'nin birleştiği aygıtlarda, kullanıcı Windows'da oturum açmadan önce bu alışveriş eşzamanlı olarak bir PRT verir. Karma Azure AD birleştirilmiş aygıtlarda, şirket içi Active Directory birincil yetkilidir. Yani, kullanıcı sadece oturum açmak için bir TGT elde edebilirsiniz kadar bekliyor, PRT verme eşzamanlı olur iken. Oturum açma Azure AD kimlik bilgilerini kullanmadığı için bu senaryo Azure AD kayıtlı aygıtlar için geçerli değildir.

| Adım | Açıklama |
| :---: | --- |
| A | Kullanıcı parolasını Kullanıcı Kabirimi'ndeki oturuma girer. LogonUI, kimlik bilgilerini bir auth arabelleğiyle LSA'ya geçirir ve bu da kimlik bilgilerini dahili olarak CloudAP'a geçirir. CloudAP bu isteği CloudAP eklentisine ileter. |
| B | CloudAP eklentisi, kullanıcının kimlik sağlayıcısını tanımlamak için bir alan bulma isteği başlatır. Kullanıcının kiracısının bir federasyon sağlayıcısı kurulumu varsa, Azure AD federasyon sağlayıcısının Meta veri alışverişi bitiş noktasını (MEX) bitiş noktasını döndürür. Değilse, Azure AD kullanıcının Azure AD ile kimlik doğrulaması olabileceğini belirten yönetilen döndürür. |
| C | Kullanıcı yönetilirse, CloudAP nonce'yi Azure AD'den alır. Kullanıcı federe ise, CloudAP eklentisi kullanıcının kimlik bilgilerini içeren federasyon sağlayıcısından bir SAML belirteci ister. SAML belirteci aldıktan sonra Azure AD'den bir nonce ister. |
| D | CloudAP eklentisi kimlik doğrulama isteğini kullanıcının kimlik bilgileri, nonce ve aracı kapsamıyla birlikte kurar, isteği Aygıt anahtarıyla (dkpriv) imzalar ve Azure AD'ye gönderir. Federe bir ortamda CloudAP eklentisi, kullanıcının kimlik bilgileri yerine federasyon sağlayıcısı tarafından döndürülen SAML belirteci kullanır. |
| E | Azure AD kullanıcı kimlik bilgilerini, nonce'yi ve aygıt imzasını doğrular, aygıtın kiracıda geçerli olduğunu doğrular ve şifrelenmiş PRT'yi yayınlar. Azure AD, PRT ile birlikte, Aktarım anahtarı (tkpub) kullanılarak Azure AD tarafından şifrelenen Oturum anahtarı olarak adlandırılan simetrik bir anahtar da kullanır. Buna ek olarak, Oturum tuşu da PRT gömülüdür. Bu Oturum anahtarı, PRT ile sonraki istekler için sahip olma kanıtı (PoP) anahtarı olarak hareket eder. |
| F | CloudAP eklentisi, şifreli PRT ve Oturum anahtarını CloudAP'a geçirir. CloudAP, Aktarım anahtarını (tkpriv) kullanarak Oturum anahtarının şifresini çözmesini ve TPM'nin kendi anahtarını kullanarak yeniden şifrelemesini isteyin. CloudAP, şifreli Oturum anahtarını ÖNBELLEğinde PRT ile birlikte saklar. |

### <a name="prt-renewal-in-subsequent-logons"></a>Sonraki oturumlarda PRT yenileme

![Sonraki oturumlarda PRT yenileme](./media/concept-primary-refresh-token/prt-renewal-subsequent-logons.png)

| Adım | Açıklama |
| :---: | --- |
| A | Kullanıcı parolasını Kullanıcı Kabirimi'ndeki oturuma girer. LogonUI, kimlik bilgilerini bir auth arabelleğiyle LSA'ya geçirir ve bu da kimlik bilgilerini dahili olarak CloudAP'a geçirir. CloudAP bu isteği CloudAP eklentisine ileter. |
| B | Kullanıcı daha önce kullanıcıda oturum açmışsa, Windows önbelleğe alınmış oturum açma işlemini başlatır ve kullanıcıyı oturum açmak için kimlik bilgilerini doğrular. CloudAP eklentisi her 4 saatte bir PRT yenilemesini eşit bir şekilde başlatır. |
| C | CloudAP eklentisi, kullanıcının kimlik sağlayıcısını tanımlamak için bir alan bulma isteği başlatır. Kullanıcının kiracısının bir federasyon sağlayıcısı kurulumu varsa, Azure AD federasyon sağlayıcısının Meta veri alışverişi bitiş noktasını (MEX) bitiş noktasını döndürür. Değilse, Azure AD kullanıcının Azure AD ile kimlik doğrulaması olabileceğini belirten yönetilen döndürür. |
| D | Kullanıcı federe ise, CloudAP eklentisi kullanıcının kimlik bilgilerini içeren federasyon sağlayıcısından bir SAML belirteci ister. SAML belirteci aldıktan sonra Azure AD'den bir nonce ister. Kullanıcı yönetilirse, CloudAP nonce'yi doğrudan Azure AD'den alır. |
| E | CloudAP eklentisi kimlik doğrulama isteğini kullanıcının kimlik bilgileri, nonce ve mevcut PRT ile birlikte kurar, isteği Oturum anahtarıyla işaretler ve Azure AD'ye gönderir. Federe bir ortamda CloudAP eklentisi, kullanıcının kimlik bilgileri yerine federasyon sağlayıcısı tarafından döndürülen SAML belirteci kullanır. |
| F | Azure AD, Oturum anahtar imzasını PRT'ye katıştırılmış Oturum anahtarıyla karşılaştırarak doğrular, nonce'yi doğrular ve aygıtın kiracıda geçerli olduğunu doğrular ve yeni bir PRT yayınlar. Daha önce de görüldüğü gibi, PRT yine Oturum tuşu ile birlikte Taşıma anahtarı (tkpub) ile şifrelenir. |
| G | CloudAP eklentisi, şifreli PRT ve Oturum anahtarını CloudAP'a geçirir. CloudAP, TPM'den Aktarım anahtarını (tkpriv) kullanarak Oturum anahtarının şifresini çözmesini ve TPM'nin kendi anahtarını kullanarak yeniden şifrelemesini ister. CloudAP, şifreli Oturum anahtarını ÖNBELLEğinde PRT ile birlikte saklar. |

### <a name="prt-usage-during-app-token-requests"></a>Uygulama belirteç istekleri sırasında PRT kullanımı

![Uygulama belirteç istekleri sırasında PRT kullanımı](./media/concept-primary-refresh-token/prt-usage-app-token-requests.png)

| Adım | Açıklama |
| :---: | --- |
| A | Bir uygulama (örneğin, Outlook, OneNote vb.) WAM'a bir belirteç isteği başlatır. WAM, buna karşılık, Azure AD WAM eklentisinin belirteç isteğine hizmet etmesini ister. |
| B | Uygulama için yenileme belirteci zaten mevcutsa, Azure AD WAM eklentisi bunu erişim belirteci istemek için kullanır. Cihaz bağlama kanıtı sağlamak için WAM eklentisi isteği Oturum tuşu ile imzalar. Azure AD Oturum anahtarını doğrular ve Oturum tuşu tarafından şifrelenmiş uygulama için bir erişim belirteci ve uygulama için yeni bir yenileme belirteci yayınlar. WAM eklentisi, Bulut AP eklentisinin belirteçlerin şifresini çözmesini ister ve bu da TPM'den Oturum anahtarını kullanarak şifresini çözmesini ister ve bu da WAM eklentisinin her iki belirteçleri de elde etmesiyle sonuçlanır. Ardından, WAM eklentisi yalnızca uygulamaya erişim belirteci sağlarken, yenileme belirteciyle DPAPI ile yeniden şifreler ve kendi önbelleğinde saklar  |
| C |  Uygulama için yenile belirteci yoksa, Azure AD WAM eklentisi erişim belirteci istemek için PRT'yi kullanır. Sahip kanıtı sağlamak için, WAM eklentisi Oturum anahtarı ile PRT içeren isteği imzalar. Azure AD, Oturum anahtar imzasını PRT'ye katıştırılmış Oturum anahtarıyla karşılaştırarak doğrular, aygıtın geçerli olduğunu doğrular ve uygulama için bir erişim belirteci ve yenileme belirteci yayınlar. ayrıca, Azure AD yeni bir PRT (yenileme döngüsüne dayalı) verebilir ve bunların tümü Oturum tuşu tarafından şifrelenebilir. |
| D | WAM eklentisi, Bulut AP eklentisinin belirteçlerin şifresini çözmesini ister ve bu da TPM'den Oturum anahtarını kullanarak şifresini çözmesini ister ve bu da WAM eklentisinin her iki belirteçleri de elde etmesiyle sonuçlanır. Ardından, WAM eklentisi yalnızca uygulamaya erişim belirteci sağlarken, yenileme belirteciyle DPAPI ile yeniden şifreler ve kendi önbelleğinde saklar. WAM eklentisi bu uygulama için ileriye dönük yenileme belirteci kullanır. WAM eklentisi ayrıca, kendi önbelleğinde güncellemeden önce Azure AD ile PRT'yi doğrulayan yeni PRT'yi Cloud AP eklentisine geri verir. Cloud AP eklentisi ileriye dönük yeni PRT kullanacaktır. |
| E | WAM, wam'a yeni verilen erişim belirteci sağlar ve bu da onu arama uygulamasına geri sağlar|

### <a name="browser-sso-using-prt"></a>TARAYıCı SSO PRT kullanarak

![TARAYıCı SSO PRT kullanarak](./media/concept-primary-refresh-token/browser-sso-using-prt.png)

| Adım | Açıklama |
| :---: | --- |
| A | Kullanıcı, BIR PRT almak için kimlik bilgileriyle Windows'a giriş yaptı. Kullanıcı tarayıcıyı açtıktan sonra tarayıcı (veya uzantı) URL'leri kayıt defterinden yükler. |
| B | Bir kullanıcı Azure AD giriş URL'sini açtığında, tarayıcı veya uzantı URL'yi kayıt defterinden elde edilenlerle doğrular. Eşleşirlerse, tarayıcı bir belirteç almak için yerel istemci ana bilgisayarını çağırır. |
| C | Yerel istemci ana bilgisayar, URL'lerin Microsoft kimlik sağlayıcılarına (Microsoft hesabı veya Azure AD) ait olduğunu doğrular, URL'den gönderilen bir nonce'yi ayıklar ve PRT çerezi almak için CloudAP eklentisine çağrı yapar. |
| D | CloudAP eklentisi PRT çerezini oluşturur, TPM'ye bağlı oturum anahtarıyla oturum açacak ve yerel istemci ana bilgisayara geri gönderir. Çerez oturum anahtarı yla imzalandığı için kurcalanamaz. |
| E | Yerel istemci ana bilgisayar bu PRT çerezini tarayıcıya döndürür ve bu çerezi x-ms-RefreshTokenCredential adlı istek üstbilgisinin bir parçası olarak içerir ve Azure AD'den belirteçleri talep eder. |
| F | Azure AD, PRT çerezindeki Oturum anahtar imzasını doğrular, nonce'yi doğrular, aygıtın kiracıda geçerli olduğunu doğrular ve web sayfası için bir kimlik belirteci ve tarayıcı için şifreli oturum çerezi yayınlar. |

## <a name="next-steps"></a>Sonraki adımlar

PRT ile ilgili sorun giderme hakkında daha fazla bilgi için, [Windows 10 ve Windows Server 2016 aygıtlarına katılan karma Azure Etkin Dizinine](troubleshoot-hybrid-join-windows-current.md)Sorun Giderme makalesine bakın.
