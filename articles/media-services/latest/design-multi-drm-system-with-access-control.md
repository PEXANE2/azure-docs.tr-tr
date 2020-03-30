---
title: Çoklu DRM içerik koruma sistemi - Azure Media Services v3
description: Bu makale, Azure Medya Hizmetleri ile çoklu DRM içerik koruma sisteminin nasıl tasarlanabildiğini ayrıntılı olarak açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77161792"
---
# <a name="design-of-a-multi-drm-content-protection-system-with-access-control"></a>Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama 

Bir over-the-top (OTT) veya online akış çözümü için bir Dijital Haklar Yönetimi (DRM) alt sistemi tasarlamak ve oluşturmak karmaşık bir görevdir. Operatörler/çevrimiçi video sağlayıcıları genellikle bu görevi özel DRM hizmet sağlayıcılarına dış kaynak olarak verir. Bu belgenin amacı, bir OTT veya çevrimiçi akış çözümünde uçtan uca drm alt sisteminin bir referans tasarımı ve başvuru uygulamasını sunmaktır.

Bu belgenin hedeflenen okuyucuları, OTT'nin DRM alt sistemlerinde veya çevrimiçi akış/çoklu ekran çözümlerinde çalışan mühendisler veya DRM alt sistemleriyle ilgilenen okuyuculardır. Varsayım, okuyucuların PlayReady, Widevine, FairPlay veya Adobe Access gibi piyasadaki DRM teknolojilerinden en az birine aşina olduklarıdır.

Bu tartışmada, multi-DRM ile Azure Medya Hizmetleri tarafından desteklenen 3 DRM'yi de dahil ediyoruz: PlayReady ve Widevine için Ortak Şifreleme (CENC), FairPlay ve AES-128 net anahtar şifrelemesi. Çevrimiçi akış ve OTT endüstrisindeki önemli bir eğilim, çeşitli istemci platformlarında yerli DRM'leri kullanmaktır. Bu eğilim, çeşitli istemci platformları için tek bir DRM ve istemciSI SDK kullanılan önceki bir geçiştir. CENC'yi çok yerel DRM ile kullandığınızda, Hem PlayReady hem de Widevine [Ortak Şifreleme (ISO/IEC 23001-7 CENC)](https://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) belirtimine göre şifrelenir.

İçerik koruması için yerel çoklu DRM kullanmanın yararları şunlardır:

* Tek bir şifreleme işlemi, yerel DRM'leri ile farklı platformları hedeflemek için kullanıldığından şifreleme maliyetini azaltır.
* Depolamada yalnızca tek bir kıymet kopyası gerektiğinden varlıkları yönetme maliyetini azaltır.
* Yerel DRM istemcisi genellikle kendi yerel platformunda ücretsiz olduğundan DRM istemci lisanslama maliyetini ortadan kaldırır.

### <a name="goals-of-the-article"></a>Makalenin hedefleri

Bu makalenin amaçları şunlardır:

* 3 DRM'nin tümlerini (DASH için CENC, HLS için FairPlay ve sorunsuz akış için PlayReady) kullanan bir DRM alt sisteminin referans tasarımını sağlayın.
* Azure ve Azure Medya Hizmetleri platformunda bir başvuru uygulaması sağlayın.
* Bazı tasarım ve uygulama konularını tartışın.

Aşağıdaki tablo, farklı platformlarda yerel DRM desteğini ve farklı tarayıcılarda EME desteğini özetleyebiliriz.

| **İstemci platformu** | **Yerli DRM** | **EME** |
| --- | --- | --- |
| **Akıllı TV'ler, STB'ler** | PlayReady, Widevine ve/veya diğer | PlayReady ve/veya Widevine için gömülü tarayıcı/EME|
| **Windows 10** | PlayReady | PlayReady için Microsoft Edge/IE11|
| **Android cihazlar (telefon, tablet, TV)** |Widevine |Widevine için Chrome |
| **iOS** | FairPlay | FairPlay için Safari (iOS 11.2'den beri) |
| **Macos** | FairPlay | FairPlay için Safari (Mac OS X 10.11+ El Capitan'da Safari 9+'dan beri)|
| **tvOS** | FairPlay | |

Her DRM için geçerli dağıtım durumu göz önüne alındığında, bir hizmet genellikle tüm uç nokta türlerini en iyi şekilde ele aldığınızdan emin olmak için iki veya üç DRM uygulamak ister.

Çeşitli istemciler üzerinde kullanıcı deneyimi belirli bir seviyeye ulaşmak için hizmet mantığı ve istemci tarafında karmaşıklığı karmaşıklığı arasında bir denge vardır.

Seçiminizi yapmak için şunları aklınızda bulundurun:

* PlayReady, her Windows cihazında, bazı Android cihazlarda yerel olarak uygulanır ve hemen hemen her platformda yazılım SDK'ları aracılığıyla kullanılabilir.
* Widevine doğal olarak her Android cihazda, Chrome'da ve diğer bazı cihazlarda uygulanır. Widevine ayrıca DASH üzerinden Firefox ve Opera tarayıcılarında da desteklenir.
* FairPlay iOS, macOS ve tvOS'ta kullanılabilir.


## <a name="a-reference-design"></a>Referans tasarımı
Bu bölümde, uygulamak için kullanılan teknolojilere agnostik bir başvuru tasarımı sunar.

Bir DRM alt sistemi aşağıdaki bileşenleri içerebilir:

* Anahtar yönetimi
* DRM şifreleme ambalajı
* DRM lisansı verme
* Yetkilendirme denetimi/erişim denetimi
* Kullanıcı kimlik doğrulaması/yetkilendirme
* Oyuncu uygulaması
* Kaynak/içerik dağıtım ağı (CDN)

Aşağıdaki diyagram, drm alt sistemindeki bileşenler arasındaki üst düzey etkileşimi göstermektedir:

![CENC ile DRM alt sistemi](./media/design-multi-drm-system-with-access-control/media-services-generic-drm-subsystem-with-cenc.png)

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
| **Kimlik Sağlayıcısı (IDP)** |Azure Active Directory (Azure AD) |
| **Güvenli Jeton Hizmeti (STS)** |Azure AD |
| **DRM koruma iş akışı** |Azure Medya Hizmetleri dinamik koruma |
| **DRM lisansı verme** |* Medya Hizmetleri lisans teslimi (PlayReady, Widevine, FairPlay) <br/>* Axinom lisans sunucusu <br/>* Özel PlayReady lisans sunucusu |
| **Kaynak** |Azure Medya Hizmetleri akış bitiş noktası |
| **Anahtar yönetimi** |Başvuru uygulaması için gerekli değildir |
| **İçerik yönetimi** |C# konsol uygulaması |

Başka bir deyişle, hem IDP hem de STS Azure AD tarafından sağlanır. [Azure Media Player API'si](https://amp.azure.net/libs/amp/latest/docs/) oynatıcı için kullanılır. Hem Azure Medya Hizmetleri hem de Azure Media Player, DASH üzerinden CENC'i, HLS üzerinden FairPlay'i, sorunsuz akış üzerinden PlayReady'yi ve DASH, HLS ve sorunsuz için AES-128 şifrelemesi destekler.

Aşağıdaki diyagram, önceki teknoloji eşlemeile genel yapısını ve akışını gösterir:

![Medya Hizmetlerinde CENC](./media/design-multi-drm-system-with-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

DRM içerik koruması ayarlamak için içerik yönetimi aracı aşağıdaki girdileri kullanır:

* İçeriği açma
* Anahtar yönetiminden içerik anahtarı
* Lisans edinme URL'leri
* Azure AD'den hedef kitle, veren ve belirteç talepleri gibi bilgilerin listesi

İçerik yönetimi aracının çıktısı aşağıda veda edinebilirsiniz:

* ContentKeyPolicy kullanılan her drm türü için DRM lisans şablonu açıklar;
* ContentKeyPolicyRestriction, DRM lisansı verilmeden önce erişim denetimini açıklar
* Streamingpolicy, akış için DRM - şifreleme modu - akış protokolü - kapsayıcı biçiminin çeşitli kombinasyonlarını açıklar
* StreamingLocator şifreleme için kullanılan içerik anahtarı/IV'ü ve akış URL'lerini açıklar 

Çalışma süresi ndeki akış şu şekildedir:

* Kullanıcı kimlik doğrulaması üzerine bir JWT oluşturulur.
* JWT'de yer alan iddialardan biri, grup nesnesi KIMLIĞI EntitledUserGroup'u içeren bir grup talebidir. Bu talep, yetkilendirme denetimini geçmek için kullanılır.
* Oyuncu CENC korumalı içeriğin istemci bildirimini indirir ve aşağıdakileri tanımlar:
   * Anahtar kimliği.
   * İçerik DRM korumalıdır.
   * Lisans edinme URL'leri.
* Oyuncu desteklenen tarayıcı/DRM'ye dayalı bir lisans edinme isteğinde bulunamaz. Lisans edinme isteğinde, anahtar kimlik ve JWT de gönderilir. Lisans teslim hizmeti JWT'yi ve gerekli lisansı vermeden önce içerdiği talepleri doğrular.

## <a name="implementation"></a>Uygulama
### <a name="implementation-procedures"></a>Uygulama prosedürleri
Uygulama aşağıdaki adımları içerir:

1. Test varlıklarını hazırlayın. Medya Hizmetlerinde parçalanmış MP4'ü çok bit hızında kodlamak/paketleyin. Bu varlık DRM korumalı *değildir.* DRM koruması daha sonra dinamik koruma ile yapılır.

2. Anahtar kimliği ve içerik anahtarı (isteğe bağlı olarak bir anahtar tohumundan) oluşturun. Bu durumda, birkaç test varlığı için yalnızca tek bir anahtar kimliği ve içerik anahtarı gerektiğinden, anahtar yönetim sistemi gerekli değildir.

3. Test varlığı için çoklu DRM lisans teslim hizmetlerini yapılandırmak için Medya Hizmetleri API'sini kullanın. Medya Hizmetleri'ndeki lisans hizmetleri yerine şirketinizin veya şirketinizin satıcıları tarafından özel lisans sunucuları kullanıyorsanız, bu adımı atlayabilirsiniz. Lisans teslimini yapılandırırken lisans edinme URL'lerini adımolarak belirtebilirsiniz. Medya Hizmetleri API'si, farklı DRM lisans hizmetleri için yetkilendirme ilkesi kısıtlaması ve lisans yanıt şablonları gibi bazı ayrıntılı yapılandırmaları belirtmek için gereklidir. Şu anda, Azure portalı bu yapılandırma için gerekli ue'yi sağlamaz. API düzeyindebilgi ve örnek kod için [PlayReady ve/veya Widevine dinamik ortak şifrelemesini kullan'a](protect-with-drm.md)bakın.

4. Test kıymeti için varlık teslim ilkesini yapılandırmak için Medya Hizmetleri API'sini kullanın. API düzeyindebilgi ve örnek kod için [PlayReady ve/veya Widevine dinamik ortak şifrelemesini kullan'a](protect-with-drm.md)bakın.

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

* Geliştirici bilgilerini [Azure Active Directory geliştiricikılavuzunda](../../active-directory/develop/v2-overview.md)bulabilirsiniz.
* Yönetici bilgilerini Azure [AD kiracı dizininizde yönet'te](../../active-directory/fundamentals/active-directory-administer.md)bulabilirsiniz.

### <a name="some-issues-in-implementation"></a>Uygulamada bazı sorunlar

Uygulama sorunlarıyla ilgili yardım için aşağıdaki sorun giderme bilgilerini kullanın.

* İhraççı URL'si "/" ile bitmelidir. Hedef kitle oyuncu uygulaması istemci kimliği olmalıdır. Ayrıca, veren URL'nin sonuna "/" ekleyin.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />

    [JWT Decoder , JWT](http://jwt.calebb.net/)gösterildiği **gibi, aud** ve **iss**bakın:

    ![Jwt](./media/design-multi-drm-system-with-access-control/media-services-1st-gotcha.png)

* Uygulamanın **Yapılandırma** sekmesinde Azure AD'deki uygulamaya izinler ekleyin. İzinler her uygulama için gereklidir, hem yerel hem de dağıtılmış sürümleri.

    ![İzinler](./media/design-multi-drm-system-with-access-control/media-services-perms-to-other-apps.png)

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

![Özel Azure AD kiracı etki alanı hesabı bir](./media/design-multi-drm-system-with-access-control/media-services-ad-tenant-domain1.png)

**Akıllı kartlı Microsoft etki alanı hesabı**: Microsoft kurumsal BT tarafından iki faktörlü kimlik doğrulamayla özelleştirilmiş oturum açma sayfası.

![Özel Azure AD kiracı etki alanı hesabı iki](./media/design-multi-drm-system-with-access-control/media-services-ad-tenant-domain2.png)

**Microsoft hesabı**: Microsoft hesabının tüketiciler için oturum açma sayfası.

![Özel Azure AD kiracı etki alanı hesabı üç](./media/design-multi-drm-system-with-access-control/media-services-ad-tenant-domain3.png)

### <a name="use-encrypted-media-extensions-for-playready"></a>PlayReady için Şifreli Ortam Uzantılarını Kullanma

Windows 8.1 veya sonraki internet explorer 11 ve Windows 10'daki Microsoft Edge tarayıcısı gibi PlayReady desteği için Şifreli Ortam Uzantıları (EME) içeren modern bir tarayıcıda PlayReady, EME'nin temel DRM'sidir.

![PlayReady için EME'yi kullanın](./media/design-multi-drm-system-with-access-control/media-services-eme-for-playready1.png)

Karanlık oynatıcı alanı, PlayReady korumasının korumalı videonun ekran yakalamasını engellemesidir.

Aşağıdaki ekran görüntüsü, oynatıcı eklentilerini ve Microsoft Security Essentials (MSE)/EME desteğini gösterir:

![PlayReady için oyuncu eklentileri](./media/design-multi-drm-system-with-access-control/media-services-eme-for-playready2.png)

Microsoft Edge'deki EME ve Windows 10'daki Internet Explorer 11, [PlayReady SL3000'ün](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) onu destekleyen Windows 10 aygıtlarında çağrılmasına olanak tanır. PlayReady SL3000 gelişmiş premium içerik akışının (4K, HDR) ve yeni içerik dağıtım modellerinin (gelişmiş içerik için) kilidini açar.

Windows aygıtlarına odaklanmak için PlayReady, Windows aygıtlarında bulunan donanımdaki tek DRM'dir (PlayReady SL3000). Bir akış hizmeti PlayReady'yi EME üzerinden veya Evrensel Windows Platformu uygulaması aracılığıyla kullanabilir ve PlayReady SL3000'i kullanarak başka bir DRM'den daha yüksek bir video kalitesi sunabilir. Tipik olarak, 2K'ya kadar içerik Chrome veya Firefox üzerinden akar ve 4K'ya kadar içerik Microsoft Edge/Internet Explorer 11 veya aynı aygıttaki Evrensel Windows Platformu uygulamasından akar. Tutar, hizmet ayarlarına ve uygulamaya bağlıdır.

#### <a name="use-eme-for-widevine"></a>Widevine için EME kullanın

Windows 10'da Chrome 41+, Windows 8.1, Mac OSX Yosemite ve Android 4.4.4'te Chrome gibi EME/Widevine desteğine sahip modern bir tarayıcıda Google Widevine, EME'nin arkasındaki DRM'dir.

![Widevine için EME kullanın](./media/design-multi-drm-system-with-access-control/media-services-eme-for-widevine1.png)

Widevine, korumalı videonun ekran yakalamasını engellemez.

![Widevine için oyuncu eklentileri](./media/design-multi-drm-system-with-access-control/media-services-eme-for-widevine2.png)

#### <a name="use-eme-for-fairplay"></a>FairPlay için EME'yi kullanın

Benzer şekilde, Bu test oynatıcıda FairPlay korumalı içeriği macOS veya iOS 11.2 ve sonraki yerlerde test edebilirsiniz.

"FairPlay"i protectionInfo.type olarak koyduğunuzdan ve FPS AC Path'deki (FairPlay Streaming Application Certificate Path) Uygulama Sertifikanız için doğru URL'yi koyduğunuzdan emin olun.

### <a name="unentitled-users"></a>Hak sahibi olmayan kullanıcılar

Bir kullanıcı "Hak Sahibi Kullanıcılar" grubunun bir üyesi değilse, kullanıcı yetkilendirme denetimini geçemez. Çoklu DRM lisans hizmeti daha sonra gösterildiği gibi istenen lisansı vermeyi reddeder. Ayrıntılı açıklama olarak tasarlanmış olan "Lisans edinme başarısız oldu".

![Hak sahibi olmayan kullanıcılar](./media/design-multi-drm-system-with-access-control/media-services-unentitledusers.png)

### <a name="run-a-custom-security-token-service"></a>Özel güvenlik belirteç hizmeti çalıştırın

Özel bir STS çalıştırıyorsanız, JWT özel STS tarafından simetrik veya asimetrik bir anahtar kullanılarak verilir.

Aşağıdaki ekran görüntüsü, simetrik anahtar (Chrome kullanarak) kullanan bir senaryoyu gösterir:

![Simetrik anahtarlı özel STS](./media/design-multi-drm-system-with-access-control/media-services-running-sts1.png)

Aşağıdaki ekran görüntüsü, X509 sertifikası (Microsoft modern tarayıcısı kullanarak) aracılığıyla asimetrik anahtar kullanan bir senaryoyu gösterir:

![Asimetrik anahtarlı özel STS](./media/design-multi-drm-system-with-access-control/media-services-running-sts2.png)

Önceki her iki durumda da, kullanıcı kimlik doğrulaması aynı kalır. Azure AD aracılığıyla gerçekleşir. Tek fark, JWT'lerin Azure AD yerine özel STS tarafından verilmiş olmasıdır. Dinamik CENC korumasını yapılandırdığınızda, lisans teslim hizmeti kısıtlaması, simetrik veya asimetrik bir anahtar olan JWT türünü belirtir.

## <a name="next-steps"></a>Sonraki adımlar

* [Sık sorulan sorular](frequently-asked-questions.md)
* [İçerik koruma genel bakış](content-protection-overview.md)
* [DRM ile içeriğinizi koruyun](protect-with-drm.md)
