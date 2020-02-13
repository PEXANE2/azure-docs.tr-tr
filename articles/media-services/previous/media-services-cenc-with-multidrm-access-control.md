---
title: Azure Media Services kullanarak erişim denetimini içeren bir içerik koruma sistemin tasarımını | Microsoft Docs
description: Microsoft kesintisiz akış istemci taşıma Kiti lisans hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: willzhan
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: willzhan
ms.reviewer: kilroyh;yanmf;juliako
ms.openlocfilehash: 68f42aa13288c2416257f3ba6c0b6072c1572977
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162999"
---
# <a name="design-of-a-content-protection-system-with-access-control-using-azure-media-services"></a>Azure Media Services kullanarak erişim denetimini içeren bir içerik koruma sistemi tasarımı 

## <a name="overview"></a>Genel Bakış

Tasarlama ve dijital hak yönetimi (DRM) alt sistemi bir üzerinden-üst düzey (OTT) için derleme veya çözüm çevrimiçi akış karmaşık bir görevdir. İşleçler/çevrimiçi video sağlayıcıları, genellikle özelleştirilmiş DRM hizmet sağlayıcıları için bu görev dış. Bu belgenin amacı, bir tasarım ve uygulama başvurusu bir uçtan uca DRM alt sisteminin OTT ya da çevrimiçi akış çözümü sunmak sağlamaktır.

Bu belge için hedeflenen okuyucular OTT veya çevrimiçi akış/çoklu ekranı çözümler isteyen DRM alt sistemler okuyucuları DRM alt sistemlerde çalışan mühendisleri ' dir. Okuyucular DRM teknolojileri PlayReady, Widevine, FairPlay veya Adobe erişim gibi piyasadaki en az biri ile bilgi sahibi olduğunuz varsayılır.

DRM Bu tartışmaya biz de ile çoklu DRM genel şifreleme (CENC) içerir. Çevrimiçi akış ve OTT sektör önemli bir eğilim çeşitli istemci platformlarında yerel birden çok DRM ile CENC kullanmaktır. Bu eğilim, çoklu DRM ve kendi İstemci SDK'sı çeşitli istemci platformları için kullanılan önceki bir kaydırmadır. Multi-Native DRM ile CENC kullandığınızda hem PlayReady hem de Widevine [Common Encryption (ISO/ıec 23001-7 CENC)](https://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) belirtimi başına şifrelenir.

BT'nin birden çok DRM ile CENC avantajları şunlardır:

* Tek şifreleme işlemi, yerel benzeri DRM ile farklı platformları hedeflemek için kullanıldığından şifreleme maliyeti azaltır.
* Yalnızca tek bir kopyasını şifrelenmiş varlıklar gerektiğinden şifrelenmiş varlıkları yönetme maliyeti azaltır.
* Yerel DRM istemci kendi yerel platformunda genellikle boş olduğu için lisans maliyeti DRM istemci ortadan kaldırır.

Microsoft, DASH CENC ve etkin bir promoter bazı önemli sektör oynatıcılar ile birlikte bulunur. Azure Media Services, tire ve CENC desteği sağlar. Son Duyurular için aşağıdaki Bloglara bakın:

*  [Azure Media Services’ta Google Widevine lisans teslim hizmetleri ile tanışın](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
* [Azure Media Services, çok DRM akışı sunmak için Google Widevine paketlemeyi ekler](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)  

### <a name="goals-of-the-article"></a>Makalenin amaçları

İçin bu makalenin amaçları şunlardır:

* Birden çok DRM ile CENC kullanan bir DRM alt sisteminin bir başvuru tasarım sağlar.
* Bir Azure/Media Services platformunda bir başvuru uygulaması sağlar.
* Tasarım ve uygulama bazı konular açıklanmaktadır.

Makaledeki "çoklu DRM" terimi aşağıdaki ürünleri kapsar:

* Microsoft PlayReady
* Google Widevine
* Apple FairPlay 

Aşağıdaki tabloda, her DRM tarafından desteklenen tarayıcılar ve yerel platform/yerel uygulama özetlenmektedir.

| **İstemci platformu** | **Yerel DRM desteği** | **Tarayıcı/uygulama** | **Akış biçimleri** |
| --- | --- | --- | --- |
| **Akıllı TV 'ler, işletmen STBs, OTT STBs** |PlayReady öncelikle ve/veya Widevine ve/veya diğer |Linux, Opera, WebKit, diğer |Çeşitli biçimleri |
| **Windows 10 cihazlar (Windows BILGISAYARı, Windows tabletler, Windows Phone, Xbox)** |PlayReady |Microsoft Edge/ıE11/EME<br/><br/><br/>Evrensel Windows Platformu |TİRE (HLS için PlayReady desteklenmez)<br/><br/>DASH, kesintisiz akış (HLS için PlayReady desteklenmez) |
| **Android cihazlar (telefon, tablet, TV)** |Widevine |Chrome/EME |DASH, HLS |
| **iOS (iPhone, iPad), OS X istemcileri ve Apple TV** |FairPlay |Safari 8 +/ EME |HLS |

Her DRM dağıtımının geçerli durumunu göz önünde bulundurarak, bir hizmet uç noktaları tüm türlerini en iyi şekilde ele almanız emin olmak için iki veya üç benzeri DRM uygulamak genellikle istiyor.

Hizmet mantığı karmaşıklığına ve belirli bir düzeyde kullanıcı deneyiminin çeşitli istemcilere ulaşmak için istemci tarafında karmaşıklık arasında bir denge yoktur.

Seçiminizi hale getirmek için göz önünde bulundurun:

* PlayReady ve neredeyse herhangi bir platformda yazılım SDK'lar aracılığıyla kullanılabilen bazı Android cihazlarda, her Windows cihaz yerel olarak uygulanır.
* Widevine, her bir Android cihazı, Chrome ve diğer bazı cihazların yerel olarak uygulanır.
* FairPlay, yalnızca iOS ve Mac OS istemcileri veya iTunes üzerinden kullanılabilir.

Tipik bir çoklu DRM için iki seçenek vardır:

* PlayReady ve Widevine
* PlayReady, Widevine ve FairPlay

## <a name="a-reference-design"></a>Bir başvuru tasarımı
Bu bölümde uygulamak için kullanılan teknolojiler için belirsiz bir başvuru tasarım sunar.

DRM alt aşağıdaki bileşenleri içerir:

* Anahtar yönetimi
* DRM paketleme
* DRM lisansı verme
* Yetkilendirme denetimi
* Kimlik doğrulama/yetkilendirme
* Oynatıcı
* Kaynak/içerik teslim ağı (CDN)

Aşağıdaki diyagram, üst düzey etkileşimi DRM alt bileşenler arasında gösterir:

![DRM alt ile CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

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
| **Güvenlik belirteci hizmeti (STS)** |Azure AD |
| **DRM koruması iş akışı** |Media Services dinamik koruma |
| **DRM lisansı teslimi** |* Media Services lisans teslimat (PlayReady, Widevine, FairPlay) <br/>* Axinom lisans sunucusu <br/>* Özel PlayReady lisans sunucusu |
| **Tıdır** |Media Services akış uç noktası |
| **Anahtar yönetimi** |Başvuru uygulaması için gerekli değildir |
| **İçerik yönetimi** |Bir C# konsol uygulaması |

Diğer bir deyişle, IDP ve STS'nin hem Azure AD ile kullanılır. Oynatıcı için [Azure Media Player API 'si](https://amp.azure.net/libs/amp/latest/docs/) kullanılır. Media Services hem Media Player, birden çok DRM ile DASH ve CENC destekler.

Genel yapısı ve önceki teknoloji eşleme ile akışı aşağıdaki diyagramda gösterilmiştir:

![Media Services'da CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Dinamik CENC şifrelemesi ayarlamak için aşağıdaki girişleri içerik yönetimi aracı kullanır:

* Açık içerik
* İçerik anahtarı anahtarından oluşturma/Yönetim
* Lisans edinme URL'leri
* Azure AD'den bilgi listesi

İçerik Yönetimi aracın çıktısı aşağıdaki gibidir:

* ContentKeyAuthorizationPolicy belirtimi üzerinde nasıl bir JSON Web Token (JWT) lisans teslim doğrular ve DRM lisans özellikleri içerir.
* AssetDeliveryPolicy biçimi, DRM koruması ve lisans edinme URL'leri akış özellikleri içerir.

Çalışma zamanı sırasında akışı şöyledir:

* Kullanıcı kimlik doğrulaması sırasında bir JWT oluşturulur.
* JWT içinde yer alan talep grup nesne kimliği EntitledUserGroup içeren gruplar talep biridir. Bu talep yetkilendirme onay geçirmek için kullanılır.
* Yürütücü, istemci bildirimi CENC korumalı içeriği indirir ve aşağıdakileri tanımlar:
   * Anahtar kimliği
   * Korumalı CENC içeriktir.
   * Lisans edinme URL'leri.
* Oyuncu desteklenen tarayıcı/DRM üzerinde temel lisans edinme isteğinde bulunur. Kimliği ve JWT Ayrıca gönderilen lisans edinme isteğinde anahtarı. Lisans teslimat hizmetinin JWT ve gerekli lisans vermeden önce yer alan talep doğrular.

## <a name="implementation"></a>Uygulama
### <a name="implementation-procedures"></a>Uygulama yordamları
Uygulama, aşağıdaki adımları içerir:

1. Test varlıkları hazırlayın. Media Services, Çoklu bit hızlı parçalanmış MP4 test video kodlayın/paket. Bu varlık DRM korumalı *değil* . DRM koruması, daha sonra dinamik Koruması tarafından gerçekleştirilir.

2. Anahtar oluşturma kimliği ve içerik bir anahtar (isteğe bağlı olarak, bir anahtar çekirdek). Bu örnekte, anahtar yönetimi sistemi gerekmez, çünkü yalnızca tek bir anahtarı kimliği ve içerik anahtarı birkaç test varlıklar için gerekli.

3. Media Services API'sine test varlık için birden çok DRM lisans teslimat hizmetlerini yapılandırmak için kullanın. Şirketiniz veya Media Services lisans Hizmetleri yerine şirketinizin satıcıları tarafından özel lisans sunucu kullanıyorsanız, bu adımı atlayabilirsiniz. Lisans teslim yapılandırdığınızda adımda lisans edinme URL'leri belirtebilirsiniz. Media Services API'sine yetkilendirme ilkesi kısıtlama ve lisans yanıt şablonları farklı DRM lisans Hizmetleri gibi ayrıntılı bazı yapılandırmalar belirtmek için gereklidir. Şu anda Azure portalında, bu yapılandırma için gerekli kullanıcı Arabirimi sağlamaz. API düzeyi bilgi ve örnek kod için bkz. [PlayReady ve/veya Widevine dinamik ortak şifreleme kullanma](media-services-protect-with-playready-widevine.md).

4. Media Services API'sine test varlık için varlık teslim ilkesini yapılandırmak için kullanın. API düzeyi bilgi ve örnek kod için bkz. [PlayReady ve/veya Widevine dinamik ortak şifreleme kullanma](media-services-protect-with-playready-widevine.md).

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

* Geliştirici bilgilerini [Azure Active Directory geliştirici kılavuzunda](../../active-directory/azuread-dev/v1-overview.md)bulabilirsiniz.
* Yönetici bilgilerini, [Azure AD kiracı dizininizi yönetme](../../active-directory/fundamentals/active-directory-administer.md)bölümünde bulabilirsiniz.

### <a name="some-issues-in-implementation"></a>Uygulamasındaki bazı sorunlar
Uygulama sorunları ile ilgili Yardım için aşağıdaki sorun giderme bilgileri kullanın.

* URL ile bitmelidir veren "/". Hedef kitle player uygulama istemci kimliği olmalıdır Ayrıca, "/" veren URL'si sonuna.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    [JWT kod çözücüde](http://jwt.calebb.net/), JWT içinde gösterildiği gibi **AUD** ve **ISS**' yi görürsünüz:

    ![JWT](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)

* Uygulamanın **Yapılandır** SEKMESINDE Azure AD 'de uygulamaya izinler ekleyin. Hem yerel hem de dağıtılan sürümleri her uygulama için izinler gereklidir.

    ![İzinler](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)

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

## <a name="additional-topics-for-implementation"></a>Uygulama için ek konular
Bu bölümde, tasarım ve uygulama bazı ek konuları anlatılmaktadır.

### <a name="http-or-https"></a>HTTP veya HTTPS?
ASP.NET MVC oynatıcı uygulaması aşağıdaki desteklemesi gerekir:

* HTTPS altında olduğunda Azure AD aracılığıyla kullanıcı kimlik doğrulaması.
* JWT exchange istemciyle HTTPS altında olduğunda Azure AD arasında.
* Media Services tarafından lisans teslim verdiyse, HTTPS altında olması gerekir istemci tarafından DRM lisans edinme. PlayReady ürün paketi HTTPS lisans dağıtımı için zorunlu değildir. Dışında Media Services PlayReady lisans sunucusu ise, HTTP veya HTTPS kullanabilirsiniz.

Media Player HTTPS altında sayfasında, bu nedenle ASP.NET oynatıcı uygulaması bir en iyi uygulama, HTTPS kullanır. Ancak, bu yüzden karışık içerikle ilgili bir sorun göz önünde bulundurmanız gerekir HTTP Akış için tercih edilir.

* Tarayıcı karışık içerik izin vermez. Ancak, eklentileri gibi Silverlight ve OSMF eklentisini için kesintisiz ve tire sağlar. Karışık içerik, risk müşteri verilerini oluşabilir kötü amaçlı JavaScript ekleme yeteneği kesilmeyen nedeniyle güvenlik konusudur. Tarayıcı varsayılan olarak bu özelliği engelleyin. Bunu çözmek için tek sunucu (kaynak) tarafında tüm etki alanları (bakılmaksızın, HTTPS veya HTTP) sağlayarak yoludur. Bu fikir ya da geçerli büyük olasılıkla değildir.
* Karışık içerik kaçının. Oynatıcı uygulaması ve Media Player, HTTP veya HTTPS kullanmanız gerekir. Karışık içerik yürütürken, bir karışık içerik uyarı temizleme silverlightSS teknik gerektirir. Karışık içerik karışık içerik uyarmadan flashSS teknik işler.
* Akış uç noktanızı Ağustos 2014 tarihinden önce oluşturulduysa, HTTPS'yi destekleyecek olmaz. Bu durumda, oluşturun ve yeni bir akış uç noktası için HTTPS kullanın.

DRM korumalı içeriği, uygulama ve akış olan HTTPS altında için başvuru uygulaması içinde. HTTP veya HTTPS kullanmak için açık içeriğine yönelik olarak kimlik doğrulaması veya bir lisans player gerek yoktur.

### <a name="azure-active-directory-signing-key-rollover"></a>İmzalama anahtarı geçiş işlemi Azure Active Directory
İmzalama anahtarı geçiş işlemi, uygulamanızda dikkate almak için önemli bir noktadır. Yoksayarsanız, tamamlanmış sistem sonunda tamamen altı hafta boyunca en fazla çalışma durdurur.

Azure AD, endüstri standartlarına kendisi ve Azure AD'yi kullanan uygulamalar arasında güven oluşturmak için kullanır. Özellikle, Azure AD, bir ortak ve özel anahtar çiftinden oluşur bir imzalama anahtarı kullanır. Azure AD kullanıcı hakkında bilgileri içeren bir güvenlik belirteci oluşturduğunda, uygulamaya geri göndermeden önce Azure AD tarafından özel bir anahtarla imzalanır. Belirtecin geçerli ve Azure ad kaynaklı olduğunu doğrulamak için uygulama belirtecinin imzası doğrulamanız gerekir. Uygulama, kiracının Federasyon meta veri belgesi içinde yer alan Azure AD tarafından kullanıma sunulan ortak anahtarı kullanır. Bu ortak anahtar ve onu türetildiği, imzalama anahtarı Azure AD'deki tüm kiracılar için kullanılan hizmet örneğiyle aynı değildir.

Azure AD anahtar geçişi hakkında daha fazla bilgi için bkz. [Azure AD 'de anahtar geçişi imzalama hakkında önemli bilgiler](../../active-directory/active-directory-signing-key-rollover.md).

[Ortak-özel anahtar çifti](https://login.microsoftonline.com/common/discovery/keys/)arasında:

* Özel anahtar, JWT oluşturmak için Azure AD tarafından kullanılır.
* Ortak anahtar, JWT doğrulamak için DRM lisans teslimat hizmetlerini Media Services gibi bir uygulama tarafından kullanılır.

Güvenlik nedenleriyle, Azure AD sertifika düzenli aralıklarla (her Altı haftada) döndürür. Güvenlik ihlallerini söz konusu olduğunda, anahtar geçişi, dilediğiniz zaman ortaya çıkabilir. Bu nedenle, Media Services lisans teslimat hizmetlerini Azure AD anahtar çifti döndürür olarak kullanılan ortak anahtarı güncelleştirmeniz gerekir. Aksi takdirde, Media Services belirteci kimlik doğrulaması başarısız olur ve lisans verilir.

Bu barındırılan hizmeti kurmak için DRM lisans teslimat hizmetlerini yapılandırdığınızda TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument ayarlayın.

JWT akışı şu şekildedir:

* Azure AD, bir kimliği doğrulanmış kullanıcı için geçerli özel anahtarla JWT verir.
* Bir oynatıcı çoklu DRM korumalı içeriğe sahip bir CENC gördüğünde önce Azure AD tarafından verilen JWT bulur.
* Oyuncu Media Services lisans teslimat hizmetlerini JWT lisans edinme isteği gönderir.
* Media Services lisans teslimat hizmetlerini, Azure ad'deki geçerli/geçerli ortak anahtar, JWT lisansları vermeden önce doğrulamak için kullanın.

DRM lisans teslimat hizmetlerini her zaman geçerli/geçerli ortak anahtarı Azure AD'den kontrol edin. Azure AD tarafından sunulan ortak anahtarı, Azure AD tarafından verilen JWT doğrulamak için kullanılan anahtardır.

Ne anahtar geçişi, Azure AD, bir JWT oluşturduktan sonra ancak JWT DRM lisans teslimat hizmetlerini doğrulama için Media Services için oyuncu tarafından gönderilmeden önce olur?

Birden fazla geçerli ortak anahtar, her zaman bir anahtar herhangi bir anda alınabilir olduğundan, Federasyon meta veri belgesi içinde kullanılabilir. Media Services lisans teslimat belgede belirtilen anahtarları dilediğinizi kullanabilirsiniz. Bir anahtarı olan en kısa sürede alınması çünkü başka değişimi olması ve VS.

### <a name="where-is-the-access-token"></a>Erişim belirteci nerede?
Bir Web uygulamasının, [OAuth 2,0 istemci kimlik bilgileri verme Ile uygulama kimliği](../../active-directory/azuread-dev/web-api.md)altında bir API uygulamasını nasıl çağırdığı hakkında bakarsanız, kimlik doğrulama akışı aşağıdaki gibidir:

* Bir web uygulamasında Azure ad oturum açtığında. Daha fazla bilgi için Web [tarayıcısı Web uygulamasına](../../active-directory/azuread-dev/web-app.md)bakın.
* Azure AD yetkilendirme uç noktası kullanıcı aracısının istemci uygulamaya bir yetkilendirme kodu ile yeniden yönlendirir. Kullanıcı Aracısı istemci uygulamanın yeniden yönlendirme URI'si bir yetkilendirme kodu döndürür.
* Web API'si için kimlik doğrulaması yapmak ve almak istediğiniz kaynak bir erişim belirteci almak web uygulaması gerekir. Azure AD belirteç uç noktasına bir istek gönderir ve kimlik bilgisi, istemci Kimliğini ve web API'SİNİN uygulama kimliği URI'si sağlar. Bu, kullanıcı onaylı olduğunu kanıtlamak için yetkilendirme kodu gösterir.
* Azure AD uygulama ve web API'sini çağırmak için kullanılan bir JWT erişim belirtecini döndürür.
* HTTPS üzerinden web uygulaması "Bearer" tayin JWT dizesiyle "Yetkilendirme" istek üst bilgisinde web API'sine eklemek için döndürülen JWT erişim belirtecini kullanır. Web API'si daha sonra JWT değerini doğrular. Doğrulama başarılı olursa, istenen kaynak döndürür.

Bu uygulama kimliğini flow'da web API'si web uygulaması kullanıcının kimliğinin güvenir. Bu nedenle, bu düzen, bir güvenilir alt sistem adı verilir. [Yetkilendirme akış diyagramı](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) , yetkilendirme kodu-verme akışının nasıl çalıştığını açıklar.

Lisans edinme belirteç kısıtlamasına ile aynı güvenilir alt sistem deseni izler. Media Services lisans teslimat hizmetinin web API'si kaynağına ya da "bir web uygulamasına erişmesi gereken arka uç kaynağa" dir. Bu nedenle erişim belirteci nerede?

Azure AD'den erişim belirteci alınır. Kullanıcı başarıyla kimlik doğrulamasından sonra bir yetkilendirme kodu döndürülür. Yetkilendirme kodu daha sonra istemci kimliği ve uygulama anahtarı ile birlikte erişim belirteciyle değiştirilecek kullanılır. Erişim belirteci işaret veya Media Services lisans teslimat hizmeti temsil eden bir "işaretçi" uygulamaya erişmek için kullanılır.

Kaydolun ve Azure AD'de işaretçi uygulamayı yapılandırmak için aşağıdaki adımları uygulayın:

1. Azure AD kiracısında:

   * Oturum açma URL'si https://[resource_name].azurewebsites.net/ ile bir uygulama (kaynak) ekleyin. 
   * URL https://[aad_tenant_name].onmicrosoft.com/[resource_name sahip bir uygulama kimliği ekleme].

2. Kaynak uygulama için yeni bir anahtar ekleyin.

3. Uygulama bildirim dosyasını güncelleştirin, böylece groupMembershipClaims özelliği "groupMembershipClaims" değerine sahip: "All".

4. Oynatıcı Web uygulamasına işaret eden Azure AD uygulamasında, **diğer uygulamalara yönelik izinler**bölümünde, 1. adımda eklenen kaynak uygulamasını ekleyin. **Temsilci izni**altında, **erişim [resource_name]** öğesini seçin. Bu seçenek, kaynak uygulama erişim erişim belirteçleri oluşturmak için web uygulamaya izin verir. Visual Studio ve Azure web uygulaması geliştirme, hem yerel hem de dağıtılan sürümü web uygulaması için bunu.

Azure AD tarafından verilen JWT işaretçi kaynağa erişmek için kullanılan erişim belirtecidir.

### <a name="what-about-live-streaming"></a>Peki canlı akış?
Önceki tartışma isteğe bağlı varlıkları üzerinde odaklanır. Peki canlı akış?

Canlı Media Services akış VOD varlık olarak bir programla ilişkili varlığı düşünerek korumak için tam olarak aynı tasarımını ve uygulamasını kullanabilirsiniz.

Özellikle, canlı akış medya Hizmetleri için bir kanal oluşturmak ve ardından bir programı kapsamında kanal oluşturmak gerekir. Program oluşturmak için program için Canlı arşiv içeren bir varlık oluşturmak gerekir. CENC birden çok DRM ile canlı içerik koruması sağlamak için program başlamadan önce VOD varlık gibi varlık için aynı kurulum/işlem uygulayın.

### <a name="what-about-license-servers-outside-media-services"></a>Media Services dışında lisans sunucuları hakkında neler diyeceksiniz?
Genellikle, müşterilere bir lisans sunucusu grubundaki DRM hizmet sağlayıcıları tarafından barındırılan bir ya da kendi veri merkezinde yatırım. Media Services Content Protection ile karma modda çalışabilir. İçeriği, barındırılan ve Media Services dışında sunucuları tarafından DRM lisanslarını teslim ederken dinamik olarak Media Services'de korumalı. Bu durumda, aşağıdaki değişiklikleri göz önünde bulundurun:

* STS, kabul edilebilir ve lisans sunucusu grubu tarafından doğrulanan belirteçleri vermek gerekiyor. Örneğin, bir yetkilendirme iletisini içeren belirli bir JWT Axinom tarafından sağlanan Widevine lisans sunucuları gerektirir. Bu nedenle, böyle bir JWT'nin vermek için bir STS'ye olması gerekir. Bu uygulama türü hakkında daha fazla bilgi için [Azure belge merkezi](https://azure.microsoft.com/documentation/) 'ne gidin ve [Azure Media Services Widevine lisansları teslim etmek Için Axinom kullanma](media-services-axinom-integration.md)makalesine bakın.
* Bundan böyle Media Services lisans teslimat hizmetinin (ContentKeyAuthorizationPolicy) yapılandırmanız gerekir. Lisans edinme URL'leri (PlayReady, Widevine ve FairPlay) sağlamanız gereken birden çok DRM ile CENC ayarlanacak AssetDeliveryPolicy yapılandırdığınızda.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Özel STS kullanmak istersem?
Bir müşteri Jwt'ler sağlamak için özel STS kullanmayı seçebilirsiniz. Nedenler şunlardır:

* Müşteri tarafından kullanılan IDP STS desteklemiyor. Bu durumda, özel STS bir seçenek olabilir.
* Müşteri faturalandırma sistemine müşterinin aboneyle STS tümleştirmek için daha esnek veya sıkı denetim gerekebilir. Örneğin, birden fazla temel, premium gibi OTT abone paketleri ve Spor MVPD operatörün sunabilir. İşleci, yalnızca belirli bir paket içeriğini kullanılabilir hale getirilir, böylece bir abonenin paket belirteciyle Taleplerde eşleştirilecek isteyebilirsiniz. Bu durumda, özel STS gereken esneklik ve denetim sağlar.

Özel STS kullandığınızda, iki değişiklik yapılması gerekir:

* Bir varlık için lisans teslimat hizmeti yapılandırırken, Azure AD'den geçerli anahtar yerine özel STS tarafından doğrulama için kullanılan güvenlik anahtarı belirtmeniz gerekir. (Daha fazla ayrıntı izleyin.) 
* Güvenlik anahtarı geçerli X509 özel anahtarı yerine JTW belirteç oluşturulduğunda, belirtilen Azure ad'deki sertifika.

Güvenlik anahtarları iki tür vardır:

* Simetrik anahtar: oluşturulacak ve JWT'nin doğrulamak için aynı anahtar kullanılır.
* Asimetrik anahtar: genel-özel anahtar çifti x X509 sertifika JWT'nin şifrelemek/oluşturmak için bir özel anahtarla ve ortak anahtar belirteci doğrulamak için kullanılır.

> [!NOTE]
> .NET Framework kullanırsanız / C# geliştirme platformu olarak X509 bir asimetrik güvenlik anahtarı için kullanılan sertifika bir anahtar uzunluğu en az 2048 olması gerekir. Bu sınıf .NET Framework System.IdentityModel.Tokens.X509AsymmetricSecurityKey gereksinimdir. Aksi takdirde, şu özel durum oluşturulur:
> 
> IDX10630: 'imzalama System.IdentityModel.Tokens.X509AsymmetricSecurityKey', '2048' bitten küçük olamaz.

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

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Akıllı kart Ile Microsoft etki alanı hesabı**: MICROSOFT kurumsal BT tarafından iki öğeli kimlik doğrulama ile özelleştirilmiş oturum açma sayfası.

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft hesabı**: tüketiciler için Microsoft hesabı oturum açma sayfası.

![Özel Azure AD Kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="use-encrypted-media-extensions-for-playready"></a>İçin PlayReady şifreli medya uzantıları kullanma
PlayReady desteği, Windows 8.1 veya sonraki sürümlerde Internet Explorer 11 ve Windows 10, Microsoft Edge tarayıcısı gibi modern tarayıcı şifreli medya Uzantıları (EME) ile PlayReady EME için temel alınan DRM açıktır.

![EME için PlayReady kullanın](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

PlayReady korumalı bir ekran yakalama korumalı video yapmasını engellediğinden koyu player alanıdır.

Aşağıdaki ekran görüntüsünde Microsoft Security Essentials'ı (MSE) ve oynatıcı eklentileri gösterir / EME destekler:

![Yürütücü eklentileri için PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

Windows 10 ' da Microsoft Edge ve Internet Explorer 11 ' deki EME, [PLAYREADY SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) 'in onu destekleyen Windows 10 cihazlarında çağrılmasını sağlar. PlayReady SL3000 kilidini açarak Gelişmiş premium içerik (4K, HDR) ve yeni akış içerik teslim modellerinden (için geliştirilmiş içerik).

Windows cihazlarda odaklanmak için PlayReady yalnızca DRM (PlayReady SL3000) Windows cihazlarında kullanılabilir donanım olur. Bir akış hizmetini PlayReady EME üzerinden veya bir evrensel Windows platformu uygulaması aracılığıyla ve PlayReady SL3000 başka bir DRM daha yüksek bir video kalitesi sunan kullanabilirsiniz. Genellikle, Chrome veya Firefox 2K akışları için yukarı içeriği ve içerik en çok 4K Microsoft Edge/Internet Explorer 11 veya aynı cihaz üzerinde bir evrensel Windows platformu uygulaması akar. Hizmet ayarları ve uygulama bağlıdır.

#### <a name="use-eme-for-widevine"></a>EME Widevine için kullanın.
Windows 10, Windows 8.1, Mac OSX Yosemite ve Chrome 41 + Chrome gibi EME/Widevine desteği Android 4.4.4 modern tarayıcıyla Google Widevine DRM EME arkasında yer alıyor.

![EME Widevine için kullanın.](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Widevine korumalı video bir ekran yakalama yapmasını engellemez.

![Yürütücü eklentileri Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="unentitled-users"></a>Unentitled kullanıcılar
Bir kullanıcı, "Başlıklı kullanıcılar" grubunun bir üyesi değilse, kullanıcı yetkilendirme onay geçmiyor. Çoklu DRM lisans hizmeti, ardından istenen lisans gösterildiği sorun reddeder. Ayrıntılı açıklama "Lisans işlemi başarısız oldu," tasarlandığı gibi olduğu.

![Unentitled kullanıcılar](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="run-a-custom-security-token-service"></a>Özel güvenlik belirteci hizmeti çalıştırın
Özel STS çalıştırırsanız, JWT bir simetrik ya da asimetrik bir anahtar kullanarak özel STS tarafından verilir.

Aşağıdaki ekran görüntüsünde, bir simetrik anahtar (Chrome kullanarak) kullanan bir senaryo gösterilmektedir:

![Bir simetrik anahtarla özel STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Aşağıdaki ekran görüntüsünde x X509 aracılığıyla asimetrik bir anahtar kullanan bir senaryo gösterilmektedir (Microsoft modern tarayıcısı kullanılarak) sertifikası:

![Asimetrik anahtar ile özel STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

Hem de önceki durumlarda, kullanıcı kimlik doğrulaması aynı kalır. Bu, Azure AD üzerinden gerçekleşir. Tek fark, Jwt'ler Azure AD yerine özel STS tarafından verilen ' dir. Dinamik CENC korumayı yapılandırdığınızda, lisans teslimat hizmeti kısıtlaması JWT, bir simetrik ya da asimetrik anahtar türünü belirtir.

## <a name="summary"></a>Özet

Bu belgede Azure Media Services ve Media Player'ı kullanarak belirteci kimlik doğrulaması, tasarımı ve uygulaması aracılığıyla çok yerel DRM ve access control ile CENC açıklanmıştır.

* Bir başvuru tasarım DRM/CENC alt sistemdeki tüm gerekli bileşenleri içeren sunuldu.
* Bir başvuru uygulaması, Azure, medya Hizmetleri ve Media Player sunuldu.
* Tasarım ve uygulama doğrudan ilgili bazı konu başlıkları da bahsedilmiştir.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sunulan bir hizmettir ve Google, Inc 'nin hizmet koşullarına ve gizlilik Ilkesine tabidir.

## <a name="media-services-learning-paths"></a>Media Services öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
 
