---
title: Birden çok DRM içerik koruma sistemi-Azure Media Services v3
description: Bu makaleler, Azure Media Services ile birden çok DRM içerik koruma sisteminin nasıl tasarlanacağını ayrıntılı olarak açıklamaktadır.
services: media-services
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2018
ms.author: willzhan
ms.custom: seodec18
ms.openlocfilehash: fbc6d6fa8f9a3b424eaec1f04a61b5ca24fe14fc
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77161792"
---
# <a name="design-of-a-multi-drm-content-protection-system-with-access-control"></a>Erişim denetimi ile birden çok DRM içerik koruma sisteminin tasarımı 

Tasarlama ve bir üzerinden-üst düzey (OTT) için Digital Rights Management (DRM) alt sistem oluşturma veya çözüm çevrimiçi akış karmaşık bir görevdir. İşleçler/çevrimiçi video sağlayıcıları, genellikle özelleştirilmiş DRM hizmet sağlayıcıları için bu görev dış. Bu belgenin amacı, bir başvuru tasarımı ve bir başvuru uygulaması bir uçtan uca DRM alt sisteminin OTT ya da çevrimiçi akış çözümü sunmak sağlamaktır.

Bu belge için hedeflenen okuyucular OTT veya çevrimiçi akış/çoklu ekranı çözümler isteyen DRM alt sistemler okuyucuları DRM alt sistemlerde çalışan mühendisleri ' dir. Okuyucular DRM teknolojileri PlayReady, Widevine, FairPlay veya Adobe erişim gibi piyasadaki en az biri ile bilgi sahibi olduğunuz varsayılır.

Bu tartışmada birden çok DRM ile Azure Media Services tarafından desteklenen 3 benzeri DRM ekliyoruz: ortak şifreleme (CENC) PlayReady ve Widevine, FairPlay yanı sıra, AES-128 şifresiz anahtar şifrelemesiyle koruyun. Çevrimiçi akış ve OTT sektör önemli bir eğilim, çeşitli istemci platformlarında yerel benzeri DRM kullanmaktır. Bu eğilim, çoklu DRM ve kendi İstemci SDK'sı çeşitli istemci platformları için kullanılan önceki bir kaydırmadır. Multi-Native DRM ile CENC kullandığınızda hem PlayReady hem de Widevine [Common Encryption (ISO/ıec 23001-7 CENC)](https://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) belirtimi başına şifrelenir.

BT'nin content protection için yerel çoklu DRM kullanmanın avantajları şunlardır:

* Tek şifreleme işlemi, yerel benzeri DRM ile farklı platformları hedeflemek için kullanıldığından şifreleme maliyeti azaltır.
* Varlık yalnızca tek bir kopyasını depolamada gerektiğinden varlıkları yönetme maliyeti azaltır.
* Yerel DRM istemci kendi yerel platformunda genellikle boş olduğu için lisans maliyeti DRM istemci ortadan kaldırır.

### <a name="goals-of-the-article"></a>Makalenin amaçları

İçin bu makalenin amaçları şunlardır:

* Bir başvuru tasarım, tüm 3 benzeri DRM (DASH için CENC), FairPlay HLS ve PlayReady için kesintisiz akış için kullandığı bir DRM alt sisteminin sağlar.
* Bir başvuru uygulaması, Azure ve Azure Media Services platformunda sağlar.
* Tasarım ve uygulama bazı konular açıklanmaktadır.

Aşağıdaki tabloda, farklı platformlarda yerel DRM desteği ve farklı tarayıcılarda EME desteği özetler.

| **İstemci platformu** | **Yerel DRM** | **EME** |
| --- | --- | --- |
| **Akıllı TV 'ler, STBs** | PlayReady, Widevine ve/veya diğer | Katıştırılmış tarayıcı/EME için PlayReady ve/veya Widevine|
| **Windows 10** | PlayReady | PlayReady için Microsoft Edge/ıE11|
| **Android cihazlar (telefon, tablet, TV)** |Widevine |Chrome için Widevine |
| **iOS** | FairPlay | FairPlay için Safari (itibaren iOS 11.2) |
| **macOS** | FairPlay | FairPlay (itibaren Safari Mac OS X 10.11 + El Capitan üzerinde 9 +) için Safari|
| **tvOS** | FairPlay | |

Her DRM dağıtımının geçerli durumunu göz önünde bulundurarak, bir hizmet uç noktaları tüm türlerini en iyi şekilde ele almanız emin olmak için iki veya üç benzeri DRM uygulamak genellikle istiyor.

Hizmet mantığı karmaşıklığına ve belirli bir düzeyde kullanıcı deneyiminin çeşitli istemcilere ulaşmak için istemci tarafında karmaşıklık arasında bir denge yoktur.

Seçiminizi hale getirmek için göz önünde bulundurun:

* PlayReady ve neredeyse herhangi bir platformda yazılım SDK'lar aracılığıyla kullanılabilen bazı Android cihazlarda, her Windows cihaz yerel olarak uygulanır.
* Widevine, her bir Android cihazı, Chrome ve diğer bazı cihazların yerel olarak uygulanır. Widevine, Firefox ve Opera tarayıcılarda DASH üzerinde de desteklenir.
* FairPlay, iOS, macOS ve tvOS üzerinde kullanılabilir.


## <a name="a-reference-design"></a>Bir başvuru tasarımı
Bu bölümde uygulamak için kullanılan teknolojiler için belirsiz bir başvuru tasarım sunar.

DRM alt aşağıdaki bileşenleri içerir:

* Anahtar yönetimi
* DRM şifreleme paketleme
* DRM lisansı verme
* Yetkilendirme onay/erişim denetimi
* Kullanıcı kimlik doğrulaması/yetkilendirme
* Player uygulaması
* Kaynak/içerik teslim ağı (CDN)

Aşağıdaki diyagram, üst düzey etkileşimi DRM alt bileşenler arasında gösterir:

![DRM alt ile CENC](./media/design-multi-drm-system-with-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Tasarım, üç temel katmandan oluşur:

* Arka ofis katmanındaki (siyah) harici olarak gösterilmez.
* Bir DMZ katman (mavi) ortak karşılaştığı tüm uç noktaları içerir.
* Ortak bir internet Katmanı (mavi) genel internet üzerinden trafiği oyuncularla ve CDN içerir.

Ayrıca bulunmamalıdır denetimine DRM koruması, statik veya dinamik şifreleme olmasına bakılmaksızın, bir içerik yönetim aracı. DRM şifreleme için girişler şunlardır:

* MBR video içeriği
* İçerik anahtarı
* Lisans edinme URL'leri

Kayıttan yürütme süresinde üst düzey akış şöyledir:

* Kullanıcının kimliği doğrulanır.
* Kullanıcının bir yetkilendirme belirteci oluşturulur.
* DRM korumalı içerik (bildirim) Player'da indirilir.
* Oyuncu lisans sunucuları bir anahtar ile birlikte bir lisans alma isteği gönderen Kimliği ve yetkilendirme belirteci.

Aşağıdaki bölümde, anahtar yönetimi tasarımını açıklanır.

| **ContentKey-varlık** | **Senaryo** |
| --- | --- |
| 1-1 |En basit durumu. Bu, en iyi bir denetim sağlar. Ancak, bu düzenleme genellikle yüksek lisans teslim maliyeti olur. En az bir lisans isteği, korunan her varlık için gereklidir. |
| 1-çok |Birden fazla varlık için aynı içerik anahtarı kullanabilirsiniz. Örneğin, tüm varlıklar için bir mantıksal grup içindeki, bir türe veya bir alt kümesini bir türe (veya film gene) gibi tek bir içerik anahtarı kullanabilirsiniz. |
| -1 |Her varlık için birden çok içerik anahtarı gereklidir. <br/><br/>Örneğin, MPEG-DASH için birden çok DRM ile dinamik CENC koruma ve HLS için dinamik AES-128 şifrelemesi uygulamanız gerekiyorsa, iki ayrı içerik anahtarı gerekir. Her bir içerik anahtarı kendi ContentKeyType gerekir. (Dinamik CENC koruma için kullanılan içerik anahtarı için ContentKeyType.CommonEncryption kullanın. Dinamik AES-128 şifrelemesi için kullanılan içerik anahtarı için ContentKeyType.EnvelopeEncryption kullanın.)<br/><br/>Başka bir örnek olarak, CENC koruma teoride, DASH içeriğinin bir içerik anahtarı video akışı ve ses akışı korumak için başka bir içerik anahtarı korumak için kullanabilirsiniz. |
| Çok-çok |Önceki iki senaryo birleşimi. İçerik anahtarı bir dizi, her biri aynı varlık grubunda birden çok varlığı için kullanılır. |

Dikkate alınması gereken başka bir önemli faktör, kalıcı ve kalıcı olmayan bir lisans kullanılır.

Bu etkenler neden önemlidir?

Lisans dağıtımı için genel bulut kullanırsanız, kalıcı ve kalıcı olmayan lisans lisans teslim maliyeti doğrudan bir etkiye sahip. Aşağıdaki iki farklı tasarım durumlarda göstermek için hizmet eder:

* Aylık abonelik: kalıcı lisans ve 1-çok içerik anahtarı varlık eşleme kullanın. Örneğin, çocukların tüm film için tek bir içerik anahtarı şifreleme için kullanırız. Bu durumda:

    Lisans tüm çocukları filmler/cihaz için istenen toplam sayısı = 1

* Aylık abonelik: kalıcı olmayan bir lisans ve içerik anahtarı varlık arasında 1-1 eşleme kullanın. Bu durumda:

    Tüm çocukları filmler/cihaz için istenen lisans sayısı [izlenen filmler sayısı] = [oturum sayısı] x

İki farklı tasarımları çok farklı bir lisans isteği desenleri neden. Farklı lisans teslim lisans teslimat hizmetinin, Media Services gibi genel bulut tarafından sağlanıyorsa, maliyet farklı düzenlerinin sonuçlanır.

## <a name="map-design-to-technology-for-implementation"></a>Uygulama için teknoloji tasarım eşleyin
Ardından, genel tasarım teknolojileri Azure/Media Services platformunda her Yapı bloğu için kullanılacak teknolojileri belirterek eşleştirilir.

Aşağıdaki tablo, eşlemeyi gösterir.

| **Yapı taşı** | **Teknoloji** |
| --- | --- |
| **Oynatıcının** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **Kimlik sağlayıcısı (ıDP)** |Azure Active Directory (Azure AD) |
| **Güvenli belirteç hizmeti (STS)** |Azure AD |
| **DRM koruması iş akışı** |Azure Media Services dinamik koruma |
| **DRM lisansı teslimi** |* Media Services lisans teslimat (PlayReady, Widevine, FairPlay) <br/>* Axinom lisans sunucusu <br/>* Özel PlayReady lisans sunucusu |
| **Tıdır** |Azure Media Services akış uç noktası |
| **Anahtar yönetimi** |Başvuru uygulaması için gerekli değildir |
| **İçerik yönetimi** |Bir C# konsol uygulaması |

Diğer bir deyişle, IDP ve STS'nin hem Azure AD tarafından sağlanır. Oynatıcı için [Azure Media Player API 'si](https://amp.azure.net/libs/amp/latest/docs/) kullanılır. Hem Azure Media Services hem de Azure Media Player, FairPlay Over HLS, kesintisiz akış üzerinden PlayReady ve DASH, HLS ve Düzgünleştir için AES-128 şifrelemesini destekler.

Genel yapısı ve önceki teknoloji eşleme ile akışı aşağıdaki diyagramda gösterilmiştir:

![Media Services'da CENC](./media/design-multi-drm-system-with-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

DRM içerik korumasını ayarlamak için aşağıdaki girişleri içerik yönetimi aracı kullanır:

* Açık içerik
* İçerik anahtarı anahtar yönetimi
* Lisans edinme URL'leri
* Hedef kitle, veren ve belirteç taleplerinden gibi Azure AD'den bilgi listesi

İçerik Yönetimi aracın çıktısı aşağıdaki gibidir:

* ContentKeyPolicy DRM kullanılan her tür için DRM lisans şablonu açıklar;
* DRM lisans verilmeden önce ContentKeyPolicyRestriction erişim denetimi açıklanmaktadır.
* Akış - şifreleme modu Akış Protokolü - - kapsayıcı biçiminden DRM, çeşitli birleşimlerini Streamingpolicy açıklar
* Şifreleme ve akış URL'leri için kullanılan içerik anahtarı/IV StreamingLocator açıklar 

Çalışma zamanı sırasında akışı şöyledir:

* Kullanıcı kimlik doğrulaması sırasında bir JWT oluşturulur.
* JWT içinde yer alan talep grup nesne kimliği EntitledUserGroup içeren gruplar talep biridir. Bu talep yetkilendirme onay geçirmek için kullanılır.
* Yürütücü, istemci bildirimi CENC korumalı içeriği indirir ve aşağıdakileri tanımlar:
   * Anahtar kimliği
   * DRM korumalı içeriktir.
   * Lisans edinme URL'leri.
* Oyuncu desteklenen tarayıcı/DRM üzerinde temel lisans edinme isteğinde bulunur. Kimliği ve JWT Ayrıca gönderilen lisans edinme isteğinde anahtarı. Lisans teslimat hizmetinin JWT ve gerekli lisans vermeden önce yer alan talep doğrular.

## <a name="implementation"></a>Uygulama
### <a name="implementation-procedures"></a>Uygulama yordamları
Uygulama, aşağıdaki adımları içerir:

1. Test varlıkları hazırlayın. Media Services, Çoklu bit hızlı parçalanmış MP4 test video kodlayın/paket. Bu varlık DRM korumalı *değil* . DRM koruması, daha sonra dinamik Koruması tarafından gerçekleştirilir.

2. Anahtar oluşturma kimliği ve içerik bir anahtar (isteğe bağlı olarak, bir anahtar çekirdek). Bu örnekte, anahtar yönetimi sistemi gerekmez, çünkü yalnızca tek bir anahtarı kimliği ve içerik anahtarı birkaç test varlıklar için gerekli.

3. Media Services API'sine test varlık için birden çok DRM lisans teslimat hizmetlerini yapılandırmak için kullanın. Şirketiniz veya Media Services lisans Hizmetleri yerine şirketinizin satıcıları tarafından özel lisans sunucu kullanıyorsanız, bu adımı atlayabilirsiniz. Lisans teslim yapılandırdığınızda adımda lisans edinme URL'leri belirtebilirsiniz. Media Services API'sine yetkilendirme ilkesi kısıtlama ve lisans yanıt şablonları farklı DRM lisans Hizmetleri gibi ayrıntılı bazı yapılandırmalar belirtmek için gereklidir. Şu anda Azure portalında, bu yapılandırma için gerekli kullanıcı Arabirimi sağlamaz. API düzeyi bilgi ve örnek kod için bkz. [PlayReady ve/veya Widevine dinamik ortak şifreleme kullanma](protect-with-drm.md).

4. Media Services API'sine test varlık için varlık teslim ilkesini yapılandırmak için kullanın. API düzeyi bilgi ve örnek kod için bkz. [PlayReady ve/veya Widevine dinamik ortak şifreleme kullanma](protect-with-drm.md).

5. Oluşturun ve Azure AD kiracısı Azure'da yapılandırın.

6. Birkaç kullanıcı hesapları ve grupları Azure AD kiracınızda oluşturun. En az bir "Kullanıcı hakkı" grubu oluşturun ve bu gruba kullanıcı ekleme. Bu gruptaki kullanıcılar, lisans edinme yetkilendirmesini denetimi başarılı. Bu gruptaki kullanıcılar, kimlik doğrulama denetimi geçirmek başarısız ve lisans olamaz. "Kullanıcı başlıklı" bu gruba üyelik, bir Azure AD tarafından verilen JWT gerekli gruplara talebi ' dir. Çoklu DRM lisans teslimat hizmetlerini yapılandırma olduğunda adımda bu talep gereksinim belirtin.

7. Video oynatıcı, konak için bir ASP.NET MVC uygulaması oluşturun. Bu ASP.NET uygulamasını Azure AD kiracısına kullanıcı kimlik doğrulaması ile korunur. Uygun talep kullanıcı kimlik doğrulamasından sonra elde edilen erişim belirteçlerini dahil edilmiştir. Bu adım için Openıd Connect API öneririz. Aşağıdaki NuGet paketlerini yükleyin:

   * Install-Package Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Package Microsoft.Owin.Security.OpenIdConnect
   * Install-Package Microsoft.Owin.Security.Cookies
   * Install-Package Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.activedirectory

8. [Azure MEDIA Player API](https://amp.azure.net/libs/amp/latest/docs/)'sini kullanarak bir oyuncu oluşturun. Farklı DRM platformlarında hangi DRM teknolojisinin kullanılacağını belirtmek için [Azure Media Player Protectionınfo API](https://amp.azure.net/libs/amp/latest/docs/) 'sini kullanın.

9. Aşağıdaki tablo, testi matris gösterir.

    | **DRM** | **Tarayıcı** | **Sorumlu kullanıcı için sonuç** | **Sorumlu olmayan kullanıcı için sonuç** |
    | --- | --- | --- | --- |
    | **PlayReady** |Microsoft Edge veya Internet Explorer 11 Windows 10 |Başarılı |Başarısız |
    | **Widevine** |Chrome, Firefox ve Opera |Başarılı |Başarısız |
    | **FairPlay** |MacOS üzerinde Safari      |Başarılı |Başarısız |
    | **AES-128** |Çoğu modern tarayıcılar  |Başarılı |Başarısız |

ASP.NET MVC oynatıcı uygulaması için Azure AD ayarlama hakkında daha fazla bilgi için bkz. [Azure Active Directory ile Azure Media Services OWIN MVC tabanlı uygulamayı tümleştirme ve JWT taleplerine göre içerik anahtarı teslimini kısıtlama](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

Daha fazla bilgi için, [Azure Media Services ve dinamik şifrelemede JWT belirteci kimlik doğrulaması](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)konusuna bakın.  

Azure AD hakkında daha fazla bilgi için:

* Geliştirici bilgilerini [Azure Active Directory geliştirici kılavuzunda](../../active-directory/develop/v2-overview.md)bulabilirsiniz.
* Yönetici bilgilerini, [Azure AD kiracı dizininizi yönetme](../../active-directory/fundamentals/active-directory-administer.md)bölümünde bulabilirsiniz.

### <a name="some-issues-in-implementation"></a>Uygulamasındaki bazı sorunlar

Uygulama sorunları ile ilgili Yardım için aşağıdaki sorun giderme bilgileri kullanın.

* URL ile bitmelidir veren "/". Hedef kitle player uygulama istemci kimliği olmalıdır Ayrıca, "/" veren URL'si sonuna.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    [JWT kod çözücüde](http://jwt.calebb.net/), JWT içinde gösterildiği gibi **AUD** ve **ISS**' yi görürsünüz:

    ![JWT](./media/design-multi-drm-system-with-access-control/media-services-1st-gotcha.png)

* Uygulamanın **Yapılandır** SEKMESINDE Azure AD 'de uygulamaya izinler ekleyin. Hem yerel hem de dağıtılan sürümleri her uygulama için izinler gereklidir.

    ![İzinler](./media/design-multi-drm-system-with-access-control/media-services-perms-to-other-apps.png)

* Dinamik CENC korumayı ayarlamadan doğru veren kullanın.

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Aşağıdakiler çalışmaz:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    GUID Azure AD Kiracı kimliğidir. GUID, Azure portal **uç noktalar** açılır menüsünde bulunabilir.

* Ayrıcalıkları verme grup üyeliğini talep. Azure AD uygulama bildirimi dosyasında aşağıdaki olduğundan emin olun: 

    "groupMembershipClaims": "Tüm" (varsayılan değer null olur)

* Kısıtlama gereksinimleri oluşturduğunuzda uygun TokenType ayarlayın.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    JWT (Azure AD) yanı sıra SWT (ACS) için destek eklemek için TokenType TokenType.JWT varsayılandır. SWT/ACS kullanırsanız, belirteç TokenType.SWT için ayarlamanız gerekir.

## <a name="the-completed-system-and-test"></a>Tamamlanmış Sistem ve test

Böylece bir oturum açma hesabı geçmeden önce temel resim davranış olabilir. Bu bölüm size aşağıdaki senaryolarda tamamlanmış uçtan uca sistemde yardımcı olur:

* Tümleşik olmayan bir senaryo ihtiyacınız varsa:

    * Video varlıklarının olan Media Services'da barındırılan ya da korumasız veya DRM ile korunan ancak (kişi istenen için bir lisans verme) belirteci kimlik doğrulaması olmadan, artık bunu açmadan test edebilirsiniz. Video akışı HTTP üzerinden olursa, HTTP için geçiş.

* Tümleşik bir uçtan uca senaryo ihtiyacınız varsa:

    * Azure AD tarafından oluşturulan JWT ve belirteç kimlik doğrulaması ile Media Services, dinamik DRM koruması altında video varlıkları için oturum açmanız gerekir.

Oynatıcı Web uygulaması ve oturum açma için [Bu Web sitesine](https://openidconnectweb.azurewebsites.net/)bakın.

### <a name="user-sign-in"></a>Kullanıcı oturumu açma
Uçtan uca tümleşik DRM sistem test etmek için oluşturulan veya eklenen bir hesabınız olması gerekir.

Hangi hesap?

Azure başlangıçta yalnızca Microsoft hesabı kullanıcıları tarafından erişime izin olsa da, erişime artık hem sistemlerden kullanıcılar tarafından izin verilir. Tüm Azure özelliklerinin artık Azure AD kimlik doğrulaması için güvenilir ve Azure AD kuruluş kullanıcılarının kimliğini doğrular. Burada Azure AD'ye tüketici kullanıcıların kimliğini doğrulamak için Microsoft hesabı tüketici kimliği sistemine güvendiği bir Federasyon ilişkisi oluşturuldu. Sonuç olarak, Azure AD, Konuk Microsoft hesapları da yerel olarak Azure AD hesapları kimlik doğrulaması yapabilir.

Azure AD, Microsoft hesap etki alanı güvenleri çünkü herhangi birinden aşağıdaki etki alanları tüm hesapları özel Azure ad kiracısında ve hesabın oturum açarken kullandığınız ekleyebilirsiniz:

| **Etki alanı adı** | **Alanını** |
| --- | --- |
| **Özel Azure AD kiracı etki alanı** |somename.onmicrosoft.com |
| **Kurumsal etki alanı** |Microsoft.com |
| **Microsoft hesabı etki alanı** |Outlook.com, live.com, hotmail.com |

Yazarlar için eklediğiniz veya oluşturduğunuz bir hesap başvurabilirsiniz.

Aşağıdaki ekran görüntüleri, farklı bir etki alanı hesapları tarafından kullanılan farklı oturum açma sayfalarını gösterir:

**Özel Azure AD kiracı etki alanı hesabı**: özel Azure AD kiracı etki alanının özelleştirilmiş oturum açma sayfası.

![Özel Azure AD Kiracı etki alanı hesabı bir](./media/design-multi-drm-system-with-access-control/media-services-ad-tenant-domain1.png)

**Akıllı kart Ile Microsoft etki alanı hesabı**: MICROSOFT kurumsal BT tarafından iki öğeli kimlik doğrulama ile özelleştirilmiş oturum açma sayfası.

![Özel Azure AD Kiracı etki alanı hesabı iki](./media/design-multi-drm-system-with-access-control/media-services-ad-tenant-domain2.png)

**Microsoft hesabı**: tüketiciler için Microsoft hesabı oturum açma sayfası.

![Özel Azure AD Kiracı etki alanı hesabı üç](./media/design-multi-drm-system-with-access-control/media-services-ad-tenant-domain3.png)

### <a name="use-encrypted-media-extensions-for-playready"></a>İçin PlayReady şifreli medya uzantıları kullanma

PlayReady desteği, Windows 8.1 veya sonraki sürümlerde Internet Explorer 11 ve Windows 10, Microsoft Edge tarayıcısı gibi modern tarayıcı şifreli medya Uzantıları (EME) ile PlayReady EME için temel alınan DRM açıktır.

![EME için PlayReady kullanın](./media/design-multi-drm-system-with-access-control/media-services-eme-for-playready1.png)

PlayReady korumalı bir ekran yakalama korumalı video yapmasını engellediğinden koyu player alanıdır.

Aşağıdaki ekran görüntüsünde Microsoft Security Essentials'ı (MSE) ve oynatıcı eklentileri gösterir / EME destekler:

![Yürütücü eklentileri için PlayReady](./media/design-multi-drm-system-with-access-control/media-services-eme-for-playready2.png)

Windows 10 ' da Microsoft Edge ve Internet Explorer 11 ' deki EME, [PLAYREADY SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) 'in onu destekleyen Windows 10 cihazlarında çağrılmasını sağlar. PlayReady SL3000 kilidini açarak Gelişmiş premium içerik (4K, HDR) ve yeni akış içerik teslim modellerinden (için geliştirilmiş içerik).

Windows cihazlarda odaklanmak için PlayReady yalnızca DRM (PlayReady SL3000) Windows cihazlarında kullanılabilir donanım olur. Bir akış hizmetini PlayReady EME üzerinden veya bir evrensel Windows platformu uygulaması aracılığıyla ve PlayReady SL3000 başka bir DRM daha yüksek bir video kalitesi sunan kullanabilirsiniz. Genellikle, Chrome veya Firefox 2K akışları için yukarı içeriği ve içerik en çok 4K Microsoft Edge/Internet Explorer 11 veya aynı cihaz üzerinde bir evrensel Windows platformu uygulaması akar. Hizmet ayarları ve uygulama bağlıdır.

#### <a name="use-eme-for-widevine"></a>EME Widevine için kullanın.

Windows 10, Windows 8.1, Mac OSX Yosemite ve Chrome 41 + Chrome gibi EME/Widevine desteği Android 4.4.4 modern tarayıcıyla Google Widevine DRM EME arkasında yer alıyor.

![EME Widevine için kullanın.](./media/design-multi-drm-system-with-access-control/media-services-eme-for-widevine1.png)

Widevine korumalı video bir ekran yakalama yapmasını engellemez.

![Yürütücü eklentileri Widevine](./media/design-multi-drm-system-with-access-control/media-services-eme-for-widevine2.png)

#### <a name="use-eme-for-fairplay"></a>HLS için FairPlay EME kullanın

Benzer şekilde, macOS veya iOS 11.2 ve üzeri Safari'de bu test Player'da FairPlay korumalı içeriği test edebilirsiniz.

"FairPlay" protectionInfo.type yerleştirin ve uygulama sertifikanızı FPS AC yol (FairPlay Streaming uygulama sertifikası) için doğru URL'yi koyun emin olun.

### <a name="unentitled-users"></a>Unentitled kullanıcılar

Bir kullanıcı, "Başlıklı kullanıcılar" grubunun bir üyesi değilse, kullanıcı yetkilendirme onay geçmiyor. Çoklu DRM lisans hizmeti, ardından istenen lisans gösterildiği sorun reddeder. Ayrıntılı açıklama "Lisans işlemi başarısız oldu," tasarlandığı gibi olduğu.

![Unentitled kullanıcılar](./media/design-multi-drm-system-with-access-control/media-services-unentitledusers.png)

### <a name="run-a-custom-security-token-service"></a>Özel güvenlik belirteci hizmeti çalıştırın

Özel STS çalıştırırsanız, JWT bir simetrik ya da asimetrik bir anahtar kullanarak özel STS tarafından verilir.

Aşağıdaki ekran görüntüsünde, bir simetrik anahtar (Chrome kullanarak) kullanan bir senaryo gösterilmektedir:

![Bir simetrik anahtarla özel STS](./media/design-multi-drm-system-with-access-control/media-services-running-sts1.png)

Aşağıdaki ekran görüntüsünde x X509 aracılığıyla asimetrik bir anahtar kullanan bir senaryo gösterilmektedir (Microsoft modern tarayıcısı kullanılarak) sertifikası:

![Asimetrik anahtar ile özel STS](./media/design-multi-drm-system-with-access-control/media-services-running-sts2.png)

Hem de önceki durumlarda, kullanıcı kimlik doğrulaması aynı kalır. Bu, Azure AD üzerinden gerçekleşir. Tek fark, Jwt'ler Azure AD yerine özel STS tarafından verilen ' dir. Dinamik CENC korumayı yapılandırdığınızda, lisans teslimat hizmeti kısıtlaması JWT, bir simetrik ya da asimetrik anahtar türünü belirtir.

## <a name="next-steps"></a>Sonraki adımlar

* [Sık sorulan sorular](frequently-asked-questions.md)
* [İçerik korumaya genel bakış](content-protection-overview.md)
* [DRM ile içeriğinizi koruyun](protect-with-drm.md)
