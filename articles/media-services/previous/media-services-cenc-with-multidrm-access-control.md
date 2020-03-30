---
title: Azure Medya Hizmetleri'ni kullanarak erişim denetimine sahip bir içerik koruma sistemi tasarımı | Microsoft Dokümanlar
description: Microsoft Smooth Streaming İstemci Taşıma Kiti'ni nasıl lisanslarsın hakkında bilgi edinin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162999"
---
# <a name="design-of-a-content-protection-system-with-access-control-using-azure-media-services"></a>Azure Medya Hizmetleri'ni kullanarak erişim denetimine sahip bir içerik koruma sistemi tasarımı 

## <a name="overview"></a>Genel Bakış

Bir over-the-top (OTT) veya online akış çözümü için bir dijital haklar yönetimi (DRM) alt sistemi tasarlamak ve oluşturmak karmaşık bir görevdir. Operatörler/çevrimiçi video sağlayıcıları genellikle bu görevi özel DRM hizmet sağlayıcılarına dış kaynak olarak verir. Bu belgenin amacı, bir OTT veya çevrimiçi akış çözümünde uçtan uca drm alt sisteminin bir referans tasarımı ve uygulanmasını sunmaktır.

Bu belgenin hedeflenen okuyucuları, OTT'nin DRM alt sistemlerinde veya çevrimiçi akış/çoklu ekran çözümlerinde çalışan mühendisler veya DRM alt sistemleriyle ilgilenen okuyuculardır. Varsayım, okuyucuların PlayReady, Widevine, FairPlay veya Adobe Access gibi piyasadaki DRM teknolojilerinden en az birine aşina olduklarıdır.

DRM bu tartışma, biz de ortak şifreleme (CENC) multi-DRM ile içerir. Online akış ve OTT sektöründe önemli bir eğilim çeşitli istemci platformlarında çok yerli DRM ile CENC kullanmaktır. Bu eğilim, çeşitli istemci platformları için tek bir DRM ve istemciSI SDK kullanılan önceki bir geçiştir. CENC'yi çok yerel DRM ile kullandığınızda, Hem PlayReady hem de Widevine [Ortak Şifreleme (ISO/IEC 23001-7 CENC)](https://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) belirtimine göre şifrelenir.

Multi-DRM ile CENC faydaları şunlardır:

* Tek bir şifreleme işlemi, yerel DRM'leri ile farklı platformları hedeflemek için kullanıldığından şifreleme maliyetini azaltır.
* Şifrelenmiş varlıkların yalnızca tek bir kopyası gerektiğinden, şifrelenmiş varlıkları yönetme maliyetini azaltır.
* Yerel DRM istemcisi genellikle kendi yerel platformunda ücretsiz olduğundan DRM istemci lisanslama maliyetini ortadan kaldırır.

Microsoft, bazı büyük endüstri oyuncularıyla birlikte DASH ve CENC'nin etkin bir organizatörüdür. Azure Medya Hizmetleri, DASH ve CENC için destek sağlar. Son duyurular için aşağıdaki bloglara bakın:

*  [Azure Media Services’de Google Widevine lisans teslim hizmetleri ile tanışın](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
* [Azure Medya Hizmetleri, çoklu DRM akışı sağlamak için Google Widevine ambalajı ekler](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)  

### <a name="goals-of-the-article"></a>Makalenin hedefleri

Bu makalenin amaçları şunlardır:

* Multi-DRM ile CENC kullanan bir DRM alt sisteminin referans tasarımını sağlayın.
* Azure/Medya Hizmetleri platformunda başvuru uygulaması sağlayın.
* Bazı tasarım ve uygulama konularını tartışın.

Makalede, "multi-DRM" terimi aşağıdaki ürünleri kapsamaktadır:

* Microsoft PlayReady
* Google Widevine
* Apple FairPlay 

Aşağıdaki tablo, her DRM tarafından desteklenen yerel platformu/yerel uygulamayı ve tarayıcıları özetler.

| **İstemci platformu** | **Yerel DRM desteği** | **Tarayıcı/uygulama** | **Akış biçimleri** |
| --- | --- | --- | --- |
| **Akıllı TV'ler, operatör STB'leri, OTT STB'leri** |PlayReady öncelikle ve/veya Widevine ve/veya diğer |Linux, Opera, WebKit, diğer |Çeşitli biçimler |
| **Windows 10 cihazları (Windows PC, Windows tabletler, Windows Phone, Xbox)** |PlayReady |Microsoft Edge/IE11/EME<br/><br/><br/>Evrensel Windows Platformu |DASH (HLS için PlayReady desteklenmez)<br/><br/>DASH, Smooth Streaming (HLS için, PlayReady desteklenmiyor) |
| **Android cihazlar (telefon, tablet, TV)** |Widevine |Krom/EME |DASH, HLS |
| **iOS (iPhone, iPad), OS X istemcileri ve Apple TV** |FairPlay |Safari 8+/EME |HLS |

Her DRM için geçerli dağıtım durumu göz önüne alındığında, bir hizmet genellikle tüm uç nokta türlerini en iyi şekilde ele aldığınızdan emin olmak için iki veya üç DRM uygulamak ister.

Çeşitli istemciler üzerinde kullanıcı deneyimi belirli bir seviyeye ulaşmak için hizmet mantığı ve istemci tarafında karmaşıklığı karmaşıklığı arasında bir denge vardır.

Seçiminizi yapmak için şunları aklınızda bulundurun:

* PlayReady, her Windows cihazında, bazı Android cihazlarda yerel olarak uygulanır ve hemen hemen her platformda yazılım SDK'ları aracılığıyla kullanılabilir.
* Widevine doğal olarak her Android cihazda, Chrome'da ve diğer bazı cihazlarda uygulanır.
* FairPlay yalnızca iOS ve Mac OS istemcilerinde veya iTunes üzerinden kullanılabilir.

Tipik bir çoklu DRM için iki seçenek vardır:

* PlayReady ve Widevine
* PlayReady, Widevine ve FairPlay

## <a name="a-reference-design"></a>Referans tasarımı
Bu bölümde, uygulamak için kullanılan teknolojilere agnostik bir başvuru tasarımı sunar.

Bir DRM alt sistemi aşağıdaki bileşenleri içerebilir:

* Anahtar yönetimi
* DRM ambalaj
* DRM lisansı verme
* Yetki denetimi
* Kimlik doğrulama/yetkilendirme
* Oynatıcı
* Kaynak/içerik dağıtım ağı (CDN)

Aşağıdaki diyagram, drm alt sistemindeki bileşenler arasındaki üst düzey etkileşimi göstermektedir:

![CENC ile DRM alt sistemi](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)

Tasarımın üç temel katmanı vardır:

* Arka ofis katmanı (siyah) dışarıdan açıklanmaz.
* Bir DMZ katmanı (koyu mavi) genel olarak karşı karşıya olan tüm uç noktaları içerir.
* Genel internet katmanı (açık mavi) CDN ve ortak internet üzerinden trafik ile oyuncular içerir.

Statik veya dinamik şifreleme olup olmadığına bakılmaksızın DRM korumasını kontrol etmek için bir içerik yönetim aracı da olmalıdır. DRM şifreleme girdileri şunlardır:

* MBR video içeriği
* İçerik anahtarı
* Lisans edinme URL'leri

İşte oynatma sırasında üst düzey akış:

* Kullanıcının kimliği doğrulandı.
* Kullanıcı için bir yetkilendirme belirteci oluşturulur.
* DRM korumalı içerik (manifesto) oyuncuya indirilir.
* Oyuncu, lisans sunucularına anahtar bir kimlik ve yetki belirteci ile birlikte bir lisans edinme isteği gönderir.

Aşağıdaki bölümde anahtar yönetiminin tasarımı tartışılmaktadır.

| **İçerikAnahtardan varlığa** | **Senaryo** |
| --- | --- |
| 1'e 1 |En basit dava. En iyi kontrolü sağlar. Ama bu düzenleme genellikle en yüksek lisans teslim maliyeti ile sonuçlanır. En azından, korunan her varlık için bir lisans isteği gereklidir. |
| 1'den çok |Aynı içerik anahtarını birden çok varlık için kullanabilirsiniz. Örneğin, bir tür veya bir türün alt kümesi (veya film geni) gibi mantıksal bir gruptaki tüm varlıklar için tek bir içerik anahtarı kullanabilirsiniz. |
| Çok-to-1 |Her varlık için birden çok içerik anahtarı gereklidir. <br/><br/>Örneğin, MPEG-DASH için çoklu DRM ve HLS için dinamik AES-128 şifrelemesi ile dinamik CENC koruması uygulamanız gerekiyorsa, iki ayrı içerik anahtarına ihtiyacınız vardır. Her içerik anahtarının kendi ContentKeyType'ına ihtiyacı vardır. (Dinamik CENC koruması için kullanılan içerik anahtarı için ContentKeyType.CommonEncryption'ı kullanın. Dinamik AES-128 şifrelemesi için kullanılan içerik anahtarı için ContentKeyType.EnvelopeEncryption kullanın.)<br/><br/>Başka bir örnek olarak, DASH içeriğinin CENC korumasında, teorik olarak, video akışını korumak için bir içerik anahtarı ve ses akışını korumak için başka bir içerik anahtarı kullanabilirsiniz. |
| Çok-çok |Önceki iki senaryonun birleşimi. Aynı varlık grubundaki birden çok varlığın her biri için bir içerik anahtarı kümesi kullanılır. |

Göz önünde bulundurulması gereken bir diğer önemli faktör kalıcı ve kalıcı olmayan lisansların kullanımıdır.

Bu hususlar neden önemli?

Lisans teslimi için genel bir bulut kullanıyorsanız, kalıcı ve kalıcı olmayan lisansların lisans teslim maliyeti üzerinde doğrudan etkisi vardır. Aşağıdaki iki farklı tasarım örneği aşağıdakileri göstermek için kullanılır:

* Aylık abonelik: Kalıcı bir lisans ve 1-çok içerik anahtar-varlık eşleme kullanın. Örneğin, tüm çocuk filmleri için şifreleme için tek bir içerik anahtarı kullanırız. Bu durumda:

    Tüm çocuk filmleri/cihazları için istenen toplam lisans sayısı = 1

* Aylık abonelik: Kalıcı olmayan bir lisans ve içerik anahtarı ve varlık arasında 1'e 1 eşleme kullanın. Bu durumda:

    Tüm çocuk filmleri/cihazları için istenen toplam lisans sayısı = [İzlenen film sayısı] x [oturum sayısı]

İki farklı tasarım çok farklı lisans istek desenleri neden. Farklı desenler, lisans teslim hizmeti Medya Hizmetleri gibi genel bir bulut tarafından sağlanıyorsa, farklı lisans teslim maliyetiyle sonuçlanır.

## <a name="map-design-to-technology-for-implementation"></a>Uygulama için teknolojiye harita tasarımı
Ardından, genel tasarım, her yapı taşı için hangi teknolojinin kullanılacağını belirterek Azure/Medya Hizmetleri platformundaki teknolojilere eşlenir.

Aşağıdaki tablo eşleme gösterir.

| **Yapı taşı** | **Teknoloji** |
| --- | --- |
| **Oynatıcı** |[Azure Media Player](https://azure.microsoft.com/services/media-services/media-player/) |
| **Kimlik sağlayıcısı (IDP)** |Azure Active Directory (Azure AD) |
| **Güvenlik belirteç hizmeti (STS)** |Azure AD |
| **DRM koruma iş akışı** |Medya Hizmetleri dinamik koruma |
| **DRM lisansı verme** |* Medya Hizmetleri lisans teslimi (PlayReady, Widevine, FairPlay) <br/>* Axinom lisans sunucusu <br/>* Özel PlayReady lisans sunucusu |
| **Kaynak** |Medya Hizmetleri akışı bitiş noktası |
| **Anahtar yönetimi** |Başvuru uygulaması için gerekli değildir |
| **İçerik yönetimi** |C# konsol uygulaması |

Başka bir deyişle, Hem IDP hem de STS Azure AD ile kullanılır. [Azure Media Player API'si](https://amp.azure.net/libs/amp/latest/docs/) oynatıcı için kullanılır. Hem Medya Hizmetleri hem de Media Player, MULTI-DRM ile DASH ve CENC'i destekler.

Aşağıdaki diyagram, önceki teknoloji eşlemeile genel yapısını ve akışını gösterir:

![Medya Hizmetlerinde CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Dinamik CENC şifrelemesini ayarlamak için içerik yönetimi aracı aşağıdaki girdileri kullanır:

* İçeriği açma
* Anahtar oluşturma/yönetiminden içerik anahtarı
* Lisans edinme URL'leri
* Azure AD'den gelen bilgilerin listesi

İçerik yönetimi aracının çıktısı aşağıda veda edinebilirsiniz:

* ContentKeyAuthorizationPolicy, lisans tesliminin JSON Web Belirteci (JWT) ve DRM lisans belirtimlerini nasıl doğrulayıştAda belirtimini içerir.
* AssetDeliveryPolicy akış biçimi, DRM koruması ve lisans edinme URL'leri ile ilgili belirtimler içerir.

Çalışma süresi ndeki akış şu şekildedir:

* Kullanıcı kimlik doğrulaması üzerine bir JWT oluşturulur.
* JWT'de yer alan iddialardan biri, grup nesnesi KIMLIĞI EntitledUserGroup'u içeren bir grup talebidir. Bu talep, yetkilendirme denetimini geçmek için kullanılır.
* Oyuncu CENC korumalı içeriğin istemci bildirimini indirir ve aşağıdakileri tanımlar:
   * Anahtar kimliği.
   * İçerik CENC korumalıdır.
   * Lisans edinme URL'leri.
* Oyuncu desteklenen tarayıcı/DRM'ye dayalı bir lisans edinme isteğinde bulunamaz. Lisans edinme isteğinde, anahtar kimlik ve JWT de gönderilir. Lisans teslim hizmeti JWT'yi ve gerekli lisansı vermeden önce içerdiği talepleri doğrular.

## <a name="implementation"></a>Uygulama
### <a name="implementation-procedures"></a>Uygulama prosedürleri
Uygulama aşağıdaki adımları içerir:

1. Test varlıklarını hazırlayın. Medya Hizmetlerinde parçalanmış MP4'ü çok bit hızında kodlamak/paketleyin. Bu varlık DRM korumalı *değildir.* DRM koruması daha sonra dinamik koruma ile yapılır.

2. Anahtar kimliği ve içerik anahtarı (isteğe bağlı olarak bir anahtar tohumundan) oluşturun. Bu durumda, birkaç test varlığı için yalnızca tek bir anahtar kimliği ve içerik anahtarı gerektiğinden, anahtar yönetim sistemi gerekli değildir.

3. Test varlığı için çoklu DRM lisans teslim hizmetlerini yapılandırmak için Medya Hizmetleri API'sini kullanın. Medya Hizmetleri'ndeki lisans hizmetleri yerine şirketinizin veya şirketinizin satıcıları tarafından özel lisans sunucuları kullanıyorsanız, bu adımı atlayabilirsiniz. Lisans teslimini yapılandırırken lisans edinme URL'lerini adımolarak belirtebilirsiniz. Medya Hizmetleri API'si, farklı DRM lisans hizmetleri için yetkilendirme ilkesi kısıtlaması ve lisans yanıt şablonları gibi bazı ayrıntılı yapılandırmaları belirtmek için gereklidir. Şu anda, Azure portalı bu yapılandırma için gerekli ue'yi sağlamaz. API düzeyindebilgi ve örnek kod için [PlayReady ve/veya Widevine dinamik ortak şifrelemesini kullan'a](media-services-protect-with-playready-widevine.md)bakın.

4. Test kıymeti için varlık teslim ilkesini yapılandırmak için Medya Hizmetleri API'sini kullanın. API düzeyindebilgi ve örnek kod için [PlayReady ve/veya Widevine dinamik ortak şifrelemesini kullan'a](media-services-protect-with-playready-widevine.md)bakın.

5. Azure'da bir Azure AD kiracısı oluşturun ve yapılandırır.

6. Azure AD kiracınızda birkaç kullanıcı hesabı ve grup oluşturun. En az bir "Yetkili Kullanıcı" grubu oluşturun ve bu gruba bir kullanıcı ekleyin. Bu gruptaki kullanıcılar lisans ediniminde yetki denetimini geçer. Bu grupta yer alan kullanıcılar kimlik doğrulama denetimini geçemez ve lisans alamaz. Bu "Hak Sahibi Kullanıcı" grubuna üyelik, Azure AD tarafından yayınlanan JWT'de gerekli grup talebidir. Çoklu DRM lisans teslim hizmetlerini yapılandırDığınızda bu talep gereksinimini adımda belirtirsiniz.

7. Video oynatıcınızı barındırmak için ASP.NET bir MVC uygulaması oluşturun. Bu ASP.NET uygulaması, Azure AD kiracısına karşı kullanıcı kimlik doğrulaması ile korunur. Kullanıcı kimlik doğrulaması sonrasında elde edilen erişim belirteçlerine uygun talepler dahildir. Bu adım için OpenID Connect API'yi öneririz. Aşağıdaki NuGet paketlerini yükleyin:

   * Yükle-Paket Microsoft.Azure.ActiveDirectory.GraphClient
   * Install-Paket Microsoft.Owin.Security.OpenIdConnect
   * Microsoft.Owin.Security.Cookies Yükle-Paket
   * Install-Paket Microsoft.Owin.Host.SystemWeb
   * Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

8. Azure Media Player API'sını kullanarak bir [oynatıcı](https://amp.azure.net/libs/amp/latest/docs/)oluşturun. Farklı DRM platformlarında hangi DRM teknolojisini kullanacağımı belirtmek için [Azure Media Player ProtectionInfo API'yi](https://amp.azure.net/libs/amp/latest/docs/) kullanın.

9. Aşağıdaki tablo test matrisini gösterir.

    | **Drm** | **Tarayıcı** | **Hak sahibi kullanıcı için sonuç** | **Hak sahibi olmayan kullanıcı için sonuç** |
    | --- | --- | --- | --- |
    | **PlayReady** |Windows 10'da Microsoft Edge veya Internet Explorer 11 |Başarılı |Başarısız |
    | **Widevine** |Krom, Firefox, Opera |Başarılı |Başarısız |
    | **Fairplay** |macOS'ta Safari      |Başarılı |Başarısız |
    | **AES-128** |En modern tarayıcılar  |Başarılı |Başarısız |

ASP.NET bir MVC oynatıcı uygulaması için Azure AD'yi nasıl ayarlayabileceğiniz hakkında bilgi için, [Azure Media Hizmetleri OWIN MVC tabanlı bir uygulamayı Azure Active Directory ile tümleştir'e bakın ve JWT iddialarına göre içerik anahtarı teslimini kısıtlayın.](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/)

Daha fazla bilgi için Azure [Medya Hizmetleri'nde JWT belirteç kimlik doğrulaması ve dinamik şifreleme](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)ye bakın.  

Azure AD hakkında bilgi için:

* Geliştirici bilgilerini [Azure Active Directory geliştiricikılavuzunda](../../active-directory/azuread-dev/v1-overview.md)bulabilirsiniz.
* Yönetici bilgilerini Azure [AD kiracı dizininizde yönet'te](../../active-directory/fundamentals/active-directory-administer.md)bulabilirsiniz.

### <a name="some-issues-in-implementation"></a>Uygulamada bazı sorunlar
Uygulama sorunlarıyla ilgili yardım için aşağıdaki sorun giderme bilgilerini kullanın.

* İhraççı URL'si "/" ile bitmelidir. Hedef kitle oyuncu uygulaması istemci kimliği olmalıdır. Ayrıca, veren URL'nin sonuna "/" ekleyin.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    [JWT Decoder , JWT](http://jwt.calebb.net/)gösterildiği **gibi, aud** ve **iss**bakın:

    ![Jwt](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)

* Uygulamanın **Yapılandırma** sekmesinde Azure AD'deki uygulamaya izinler ekleyin. İzinler her uygulama için gereklidir, hem yerel hem de dağıtılmış sürümleri.

    ![İzinler](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)

* Dinamik CENC koruması ayarlarken doğru vereni kullanın.

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>

    Aşağıdakiler işe yaramaz:

        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />

    GUID, Azure AD kiracı kimliğidir. GUID, Azure portalındaki **Uç Noktalar** açılır menüsünde bulunabilir.

* Hibe grubu üyeliği ayrıcalıkları talep eder. Azure AD uygulama bildirimi dosyasında aşağıdakilerin olduğundan emin olun: 

    "groupMembershipClaims": "Tümü" (varsayılan değer null' dur)

* Kısıtlama gereksinimleri oluştururken uygun TokenType'ı ayarlayın.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    SWT'ye (ACS) ek olarak JWT (Azure AD) desteği eklediğinizden, varsayılan TokenType TokenType.JWT'dir. SWT/ACS kullanıyorsanız, belirteci TokenType.SWT olarak ayarlamanız gerekir.

## <a name="additional-topics-for-implementation"></a>Uygulama için ek konular
Bu bölümde tasarım ve uygulamadaki bazı ek konular tartışılmaktadır.

### <a name="http-or-https"></a>HTTP veya HTTPS?
ASP.NET MVC oynatıcı uygulaması aşağıdakileri desteklemelidir:

* HTTPS kapsamında olan Azure AD aracılığıyla kullanıcı kimlik doğrulaması.
* Https altında olan istemci ve Azure AD arasında JWT alışverişi.
* Lisans teslimi Media Services tarafından sağlanıyorsa, istemci tarafından DRM lisans edinimi https altında olmalıdır. PlayReady ürün paketi, lisans teslimi için HTTPS'yi zorunlu kılmıyor. PlayReady lisans sunucunuz Medya Hizmetleri dışındaysa, HTTP veya HTTPS'yi kullanabilirsiniz.

ASP.NET player uygulaması HTTPS'yi en iyi uygulama olarak kullanır, bu nedenle Media Player HTTPS altında bir sayfadadır. Ancak, HTTP akış için tercih edilir, bu nedenle karışık içerik sorunu dikkate almanız gerekir.

* Tarayıcı karışık içeriğe izin vermez. Ama Silverlight ve OSMF eklentisi gibi eklentileri pürüzsüz ve DASH için izin yok. Karışık içerik, müşteri verilerinin risk altında olması nedeniyle kötü amaçlı JavaScript enjekte etme tehdidi nedeniyle bir güvenlik sorunudur. Tarayıcılar varsayılan olarak bu özelliği engeller. Bunun etrafında çalışmak için tek yolu sunucu (menşe) tarafında tüm etki (ne olursa olsun HTTPS veya HTTP) izin vererek. Bu da muhtemelen iyi bir fikir değil.
* Karışık içerikten kaçının. Hem oynatıcı uygulaması hem de Media Player HTTP veya HTTPS kullanmalıdır. Karışık içerik oynatırken, silverlightSS teknolojisi karışık içerikli bir uyarıyı temizlemeyi gerektirir. flashSS teknolojisi karışık içerik uyarısı olmadan karışık içeriği işler.
* Akış bitiş noktanız Ağustos 2014'te oluşturulduysa, HTTPS'yi desteklemez. Bu durumda, HTTPS için yeni bir akış bitiş noktası oluşturun ve kullanın.

DRM korumalı içerikler için başvuru uygulamasında, hem uygulama hem de akış HTTPS altındadır. Açık içerikler için, oyuncunun kimlik doğrulamasına veya lisansa ihtiyacı yoktur, bu nedenle HTTP veya HTTPS'yi kullanabilirsiniz.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory imzalama anahtar rollover
Anahtar devrini imzalamak, uygulamanızda dikkate alınması gereken önemli bir noktadır. Bunu göz ardı ederseniz, bitmiş sistem sonunda en fazla altı hafta içinde, tamamen çalışmayı durdurur.

Azure AD, kendisi ve Azure AD kullanan uygulamalar arasında güven oluşturmak için endüstri standartlarını kullanır. Azure AD özellikle ortak ve özel anahtar çiftinden oluşan bir imzalama anahtarı kullanır. Azure AD kullanıcı hakkında bilgi içeren bir güvenlik belirteci oluşturduğunda, uygulamaya geri gönderilmeden önce Azure AD tarafından özel bir anahtarla imzalanır. Belirteci geçerli olduğunu ve Azure AD kaynaklı olduğunu doğrulamak için, uygulamanın belirteç imzasını doğrulaması gerekir. Uygulama, kiracının federasyon meta veri belgesinde bulunan Azure AD tarafından açığa çıkarılan ortak anahtarı kullanır. Bu ortak anahtar ve türetildiği imza anahtarı, Azure AD'deki tüm kiracılar için kullanılan anahtarla aynıdır.

Azure AD tuşu rollover hakkında daha fazla bilgi için Azure [AD'de anahtar rollover imzalama hakkında önemli bilgilere](../../active-directory/active-directory-signing-key-rollover.md)bakın.

[Kamu-özel anahtar çifti](https://login.microsoftonline.com/common/discovery/keys/)arasında:

* Özel anahtar, Azure AD tarafından bir JWT oluşturmak için kullanılır.
* Ortak anahtar, JWT'yi doğrulamak için Medya Hizmetleri'ndeki DRM lisans teslim hizmetleri gibi bir uygulama tarafından kullanılır.

Azure AD, güvenlik amacıyla sertifikayı düzenli aralıklarla (her altı haftada bir) döndürür. Güvenlik ihlalleri durumunda, anahtar devri her zaman oluşabilir. Bu nedenle, Medya Hizmetleri'ndeki lisans teslim hizmetlerinin Azure AD anahtar çiftini döndürürken kullanılan ortak anahtarı güncelleştirmesi gerekir. Aksi takdirde, Medya Hizmetleri'nde belirteç kimlik doğrulaması başarısız olur ve lisans verilmez.

Bu hizmeti ayarlamak için, DRM lisans teslim hizmetlerini yapılandırırken TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument'ı ayarlarsınız.

İşte JWT akışı:

* Azure AD, JWT'yi kimlik doğrulaması yapılan bir kullanıcı için geçerli özel anahtarla birlikte yayınlar.
* Bir oyuncu multi-DRM korumalı içeriğe sahip bir CENC gördüğünde, ilk olarak Azure AD tarafından verilen JWT'yi bulur.
* Oyuncu, Medya Hizmetleri'ndeki lisans teslim hizmetlerine JWT ile lisans edinme isteği gönderir.
* Medya Hizmetleri'ndeki lisans teslim hizmetleri, lisans vermeden önce JWT'yi doğrulamak için Azure AD'nin geçerli/geçerli ortak anahtarını kullanır.

DRM lisans teslim hizmetleri her zaman Azure AD'den geçerli/geçerli ortak anahtarı denetler. Azure AD tarafından sunulan ortak anahtar, Azure AD tarafından verilen bir JWT'yi doğrulamak için kullanılan anahtardır.

Anahtar devri, Azure AD bir JWT oluşturduktan sonra ancak JWT oynatıcılar tarafından doğrulama için Medya Hizmetleri'ndeki DRM lisans teslim hizmetlerine gönderilmeden önce gerçekleşirse ne olur?

Bir anahtar her an devredilebildiği için, federasyon meta veri belgesinde her zaman birden fazla geçerli ortak anahtar kullanılabilir. Medya Hizmetleri lisans teslimi belgede belirtilen anahtarlardan herhangi birini kullanabilir. Bir anahtar yakında yuvarlanabilir, başka bir yerine olabilir, ve benzeri.

### <a name="where-is-the-access-token"></a>Erişim jetonu nerede?
Bir web uygulamasının [OAuth 2.0 istemci kimlik bilgileri hibesi ile Uygulama kimliği](../../active-directory/azuread-dev/web-api.md)altında bir API uygulamasını nasıl aradığına bakarsanız, kimlik doğrulama akışı aşağıdaki gibidir:

* Kullanıcı, web uygulamasında Azure AD'de yer alar. Daha fazla bilgi [için Web tarayıcısına ve web uygulamasına](../../active-directory/azuread-dev/web-app.md)bakın.
* Azure AD yetkilendirme bitiş noktası, kullanıcı aracısını bir yetkilendirme koduyla istemci uygulamasına yönlendirir. Kullanıcı aracısı yetkilendirme kodunu istemci uygulamanın yeniden yönlendirme URI'sine döndürür.
* Web uygulamasının, web API'sine kimlik doğrulaması yapabilmesi ve istenen kaynağı alabilmesi için bir erişim belirteci edinmesi gerekir. Azure AD belirteç bitiş noktasına bir istekte bulunmaz ve kimlik bilgisi, istemci kimliği ve web API uygulama kimliği URI'yi sağlar. Kullanıcının onay verdiğini kanıtlamak için yetkilendirme kodunu sunar.
* Azure AD uygulamanın kimliğini doğrular ve web API'sını aramak için kullanılan bir JWT erişim belirteci döndürür.
* HTTPS üzerinden, web uygulaması, web API isteğinin "Yetkilendirme" başlığında "Taşıyıcı" ibaresi ile JWT dizesini eklemek için döndürülen JWT erişim belirteci kullanır. Web API sonra JWT doğrular. Doğrulama başarılı olursa, istenen kaynağı döndürür.

Bu uygulama kimlik akışında, web API'si web uygulamasının kullanıcının kimliğini doğruladığını güvenir. Bu nedenle, bu desen güvenilir bir alt sistem olarak adlandırılır. [Yetkilendirme akışı diyagramı,](https://docs.microsoft.com/azure/active-directory/active-directory-protocols-oauth-code) yetkilendirme kodu-hibe akışının nasıl çalıştığını açıklar.

Belirteç kısıtlaması ile lisans edinimi aynı güvenilir alt sistem deseni izler. Medya Hizmetleri'ndeki lisans teslim hizmeti, web API kaynağıdır veya bir web uygulamasının erişmesi gereken "arka uç kaynağıdır". Peki erişim jetonu nerede?

Erişim belirteci Azure AD'den elde edilir. Başarılı kullanıcı kimlik doğrulaması sonrasında, yetkilendirme kodu döndürülür. Yetkilendirme kodu daha sonra, istemci kimliği ve uygulama anahtarıyla birlikte erişim jetonunu değiştirmek için kullanılır. Erişim belirteci, Medya Hizmetleri lisans teslim hizmetini işaret eden veya temsil eden bir "işaretçi" uygulamasına erişmek için kullanılır.

İşaretçi uygulamasını Azure AD'de kaydetmek ve yapılandırmak için aşağıdaki adımları izleyin:

1. Azure AD kiracısında:

   * Oturum açma URL'si https://[resource_name].azurewebsites.net/ ile bir uygulama (kaynak) ekleyin. 
   * HTTPS://[aad_tenant_name].onmicrosoft.com/[resource_name] URL'sini içeren bir uygulama kimliği ekleyin.

2. Kaynak uygulaması için yeni bir anahtar ekleyin.

3. Uygulama bildirimi dosyasını grupÜyelik Talepleri özelliğinin "groupMembershipClaims" değerine sahip olması için güncelleştirin: "Tümü".

4. Player web uygulamasına işaret eden Azure AD uygulamasında, **diğer uygulamalara izinler**bölümünde, adım 1'e eklenen kaynak uygulamasını ekleyin. Temsilci izni altında **Access [resource_name]** seçeneğini **belirleyin.** Bu seçenek, web uygulamasına kaynak uygulamasına erişen erişim belirteçleri oluşturma izni verir. Visual Studio ve Azure web uygulaması yla gelişirseniz, bunu web uygulamasının hem yerel hem de dağıtılmış sürümü için yapın.

Azure AD tarafından verilen JWT, işaretçi kaynağına erişmek için kullanılan erişim jetonudur.

### <a name="what-about-live-streaming"></a>Peki ya canlı yayın?
Önceki tartışmada isteğe bağlı varlıklar üzerinde duruldu. Peki ya canlı yayın?

Bir programla ilişkili varlığı BIR VOD varlığı olarak ele alarak Medya Hizmetleri'nde canlı akışı korumak için tam olarak aynı tasarımı ve uygulamayı kullanabilirsiniz.

Özellikle, Medya Hizmetleri'nde canlı akış yapmak için bir kanal oluşturmanız ve ardından kanal altında bir program oluşturmanız gerekir. Programı oluşturmak için, programın canlı arşivini içeren bir varlık oluşturmanız gerekir. CENC'ye canlı içeriğin çoklu DRM koruması sağlamak için, programa başlamadan önce bir VOD varlığı gibi aynı kurulum/işlemeyi varlığa uygulayın.

### <a name="what-about-license-servers-outside-media-services"></a>Media Services dışındaki lisans sunucuları ne olacak?
Genellikle, müşteriler kendi veri merkezlerinde veya DRM servis sağlayıcıları tarafından barındırılan bir lisans sunucusu çiftliğine yatırım yapar. Medya Hizmetleri İçerik Koruması ile karma modda çalışabilirsiniz. İçerikler Medya Hizmetleri'nde barındırılabilir ve dinamik olarak korunabilir, DRM lisansları ise Medya Hizmetleri dışındaki sunucular tarafından teslim edilir. Bu durumda, aşağıdaki değişiklikleri göz önünde bulundurun:

* STS'nin kabul edilebilir ve lisans sunucusu çiftliği tarafından doğrulanabilecek belirteçleri vermesi gerekir. Örneğin, Axinom tarafından sağlanan Widevine lisans sunucuları, bir yetkilendirme iletisi içeren belirli bir JWT gerektirir. Bu nedenle, böyle bir JWT sorunu bir STS olması gerekir. Bu tür uygulamalar hakkında bilgi için [Azure Dokümantasyon Merkezi'ne](https://azure.microsoft.com/documentation/) gidin ve [Widevine lisanslarını Azure Medya Hizmetleri'ne sunmak için Axinom'u Kullan'a](media-services-axinom-integration.md)bakın.
* Artık Medya Hizmetlerinde lisans teslim hizmetini (ContentKeyAuthorizationPolicy) yapılandırmanız gerekmez. Multi-DRM ile CENC'yi kurmak için AssetDeliveryPolicy'yi yapılandırırken lisans satın alma URL'lerini (PlayReady, Widevine ve FairPlay için) sağlamanız gerekir.

### <a name="what-if-i-want-to-use-a-custom-sts"></a>Özel bir STS kullanmak istersem ne olur?
Bir müşteri JWTs sağlamak için özel bir STS kullanmayı tercih edebilir. Nedenleri şunlardır:

* Müşteri tarafından kullanılan IDP STS'yi desteklemez. Bu durumda, özel bir STS bir seçenek olabilir.
* Müşterinin STS'yi müşterinin abone faturalandırma sistemiyle tümleştirmek için daha esnek veya daha sıkı kontrole ihtiyaç duyabilir. Örneğin, bir MVPD operatörü premium, temel ve spor gibi birden çok OTT abone paketi sunabilir. Operatör, yalnızca belirli bir paketteki içeriğin kullanıma sunulması için bir belirteçteki talepleri abone paketiyle eşleştirmek isteyebilir. Bu durumda, özel bir STS gerekli esneklik ve denetimi sağlar.

Özel bir STS kullandığınızda, iki değişiklik yapılmalıdır:

* Bir varlık için lisans teslim hizmetini yapılandırdığınızda, Azure AD'nin geçerli anahtarı yerine özel STS tarafından doğrulama için kullanılan güvenlik anahtarını belirtmeniz gerekir. (Daha fazla ayrıntı izleyin.) 
* Bir JTW belirteci oluşturulduğunda, Azure AD'deki geçerli X509 sertifikasının özel anahtarı yerine bir güvenlik anahtarı belirtilir.

İki tür güvenlik anahtarı vardır:

* Simetrik anahtar: Aynı anahtar bir JWT oluşturmak ve doğrulamak için kullanılır.
* Asimetrik anahtar: X509 sertifikasındaki genel-özel anahtar çifti, bir JWT'yi şifrelemek/oluşturmak için özel bir anahtarla ve belirteci doğrulamak için ortak anahtarla birlikte kullanılır.

> [!NOTE]
> Geliştirme platformunuz olarak .NET Framework/C# kullanıyorsanız, asimetrik güvenlik anahtarı için kullanılan X509 sertifikasının en az 2048'lik anahtar uzunluğu olmalıdır. Bu sınıf System.IdentityModel.Tokens.X509AsymmetricSecurityKey in .NET Framework bir gereksinimidir. Aksi takdirde, aşağıdaki özel durum atılır:
> 
> IDX10630: 'System.IdentityModel.Tokens.X509AsymmetricSecurityKey' imza için '2048' bit daha küçük olamaz.

## <a name="the-completed-system-and-test"></a>Tamamlanan sistem ve test
Bu bölüm, oturum açma hesabı almadan önce davranışın temel bir resmini elde edebilmeniz için tamamlanmış uçtan uca sistemde aşağıdaki senaryoları size iletir:

* Tümleşik olmayan bir senaryoya ihtiyacınız varsa:

    * Medya Hizmetleri'nde barındırılan, korumasız veya DRM korumalı, ancak belirteç kimlik doğrulaması (talep eden kişiye lisans verme) olmadan barındırılan video varlıkları için, oturum açmadan test edebilirsiniz. Video akışınız HTTP'nin üzerindeyse HTTP'ye geçin.

* Uçuca tümleşik bir senaryoya ihtiyacınız varsa:

    * Medya Hizmetlerinde dinamik DRM koruması altındaki video varlıkları için, Azure AD tarafından oluşturulan belirteç kimlik doğrulaması ve JWT ile oturum açmanız gerekir.

Oyuncu web uygulaması ve oturum açma için [bu web sitesine](https://openidconnectweb.azurewebsites.net/)bakın.

### <a name="user-sign-in"></a>Kullanıcı oturumu açma
Uçuça tümleşik DRM sistemini test etmek için bir hesap oluşturulması veya eklenmesi gerekir.

Hangi hesap?

Azure başlangıçta yalnızca Microsoft hesap kullanıcıları tarafından erişime izin verilse de, artık her iki sistemden kullanıcılar erişime izin verir. Azure AD artık tüm Azure özellikleri kimlik doğrulaması için Azure AD'ye güveniyor ve Azure AD kuruluş kullanıcılarının kimlik doğruluğunu doğrulıyor. Azure AD'nin tüketici kullanıcılarının kimliğini doğrulamak için Microsoft hesabı tüketici kimlik sistemine güvendiği bir federasyon ilişkisi oluşturuldu. Sonuç olarak, Azure AD konuk Microsoft hesaplarının yanı sıra yerel Azure AD hesaplarını da doğrulayabilir.

Azure AD Microsoft hesap etki alanına güvendiğinden, aşağıdaki etki alanlarından herhangi birini özel Azure AD kiracısına ekleyebilir ve oturum açmaiçin hesabı kullanabilirsiniz:

| **Etki alanı adı** | **Domain** |
| --- | --- |
| **Özel Azure AD kiracı etki alanı** |somename.onmicrosoft.com |
| **Kurumsal etki alanı** |microsoft.com |
| **Microsoft hesap etki alanı** |outlook.com, live.com, hotmail.com |

Sizin için bir hesap oluşturulması veya eklenmesi için yazarlardan herhangi biri ile iletişim kurabilirsiniz.

Aşağıdaki ekran görüntüleri, farklı etki alanı hesapları tarafından kullanılan farklı oturum açma sayfalarını gösterir:

**Özel Azure AD kiracı etki alanı hesabı**: Özel Azure AD kiracı etki alanının özelleştirilmiş oturum açma sayfası.

![Özel Azure AD kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Akıllı kartlı Microsoft etki alanı hesabı**: Microsoft kurumsal BT tarafından iki faktörlü kimlik doğrulamayla özelleştirilmiş oturum açma sayfası.

![Özel Azure AD kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft hesabı**: Microsoft hesabının tüketiciler için oturum açma sayfası.

![Özel Azure AD kiracı etki alanı hesabı](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="use-encrypted-media-extensions-for-playready"></a>PlayReady için Şifreli Ortam Uzantılarını Kullanma
Windows 8.1 veya sonraki internet explorer 11 ve Windows 10'daki Microsoft Edge tarayıcısı gibi PlayReady desteği için Şifreli Ortam Uzantıları (EME) içeren modern bir tarayıcıda PlayReady, EME'nin temel DRM'sidir.

![PlayReady için EME'yi kullanın](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Karanlık oynatıcı alanı, PlayReady korumasının korumalı videonun ekran yakalamasını engellemesidir.

Aşağıdaki ekran görüntüsü, oynatıcı eklentilerini ve Microsoft Security Essentials (MSE)/EME desteğini gösterir:

![PlayReady için oyuncu eklentileri](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

Microsoft Edge'deki EME ve Windows 10'daki Internet Explorer 11, [PlayReady SL3000'ün](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) onu destekleyen Windows 10 aygıtlarında çağrılmasına olanak tanır. PlayReady SL3000 gelişmiş premium içerik akışının (4K, HDR) ve yeni içerik dağıtım modellerinin (gelişmiş içerik için) kilidini açar.

Windows aygıtlarına odaklanmak için PlayReady, Windows aygıtlarında bulunan donanımdaki tek DRM'dir (PlayReady SL3000). Bir akış hizmeti PlayReady'yi EME üzerinden veya Evrensel Windows Platformu uygulaması aracılığıyla kullanabilir ve PlayReady SL3000'i kullanarak başka bir DRM'den daha yüksek bir video kalitesi sunabilir. Tipik olarak, 2K'ya kadar içerik Chrome veya Firefox üzerinden akar ve 4K'ya kadar içerik Microsoft Edge/Internet Explorer 11 veya aynı aygıttaki Evrensel Windows Platformu uygulamasından akar. Tutar, hizmet ayarlarına ve uygulamaya bağlıdır.

#### <a name="use-eme-for-widevine"></a>Widevine için EME kullanın
Windows 10'da Chrome 41+, Windows 8.1, Mac OSX Yosemite ve Android 4.4.4'te Chrome gibi EME/Widevine desteğine sahip modern bir tarayıcıda Google Widevine, EME'nin arkasındaki DRM'dir.

![Widevine için EME kullanın](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Widevine, korumalı videonun ekran yakalamasını engellemez.

![Widevine için oyuncu eklentileri](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="unentitled-users"></a>Hak sahibi olmayan kullanıcılar
Bir kullanıcı "Hak Sahibi Kullanıcılar" grubunun bir üyesi değilse, kullanıcı yetkilendirme denetimini geçemez. Çoklu DRM lisans hizmeti daha sonra gösterildiği gibi istenen lisansı vermeyi reddeder. Ayrıntılı açıklama olarak tasarlanmış olan "Lisans edinme başarısız oldu".

![Hak sahibi olmayan kullanıcılar](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="run-a-custom-security-token-service"></a>Özel güvenlik belirteç hizmeti çalıştırın
Özel bir STS çalıştırıyorsanız, JWT özel STS tarafından simetrik veya asimetrik bir anahtar kullanılarak verilir.

Aşağıdaki ekran görüntüsü, simetrik anahtar (Chrome kullanarak) kullanan bir senaryoyu gösterir:

![Simetrik anahtarlı özel STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Aşağıdaki ekran görüntüsü, X509 sertifikası (Microsoft modern tarayıcısı kullanarak) aracılığıyla asimetrik anahtar kullanan bir senaryoyu gösterir:

![Asimetrik anahtarlı özel STS](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

Önceki her iki durumda da, kullanıcı kimlik doğrulaması aynı kalır. Azure AD aracılığıyla gerçekleşir. Tek fark, JWT'lerin Azure AD yerine özel STS tarafından verilmiş olmasıdır. Dinamik CENC korumasını yapılandırdığınızda, lisans teslim hizmeti kısıtlaması, simetrik veya asimetrik bir anahtar olan JWT türünü belirtir.

## <a name="summary"></a>Özet

Bu belgede, cenc ile çok yerel DRM ve erişim denetimi belirteç kimlik doğrulaması, tasarımı ve Azure, Medya Hizmetleri ve Media Player kullanarak uygulanması tartışıldı.

* DrM/CENC alt sisteminde gerekli tüm bileşenleri içeren bir referans tasarımı sunuldu.
* Azure, Medya Hizmetleri ve Media Player'da bir başvuru uygulaması sunuldu.
* Tasarım ve uygulamayla doğrudan ilgili bazı konular da tartışıldı.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
 
