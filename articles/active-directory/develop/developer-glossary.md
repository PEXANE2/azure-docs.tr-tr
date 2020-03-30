---
title: Microsoft kimlik platformu geliştirici sözlüğü | Azure
description: Yaygın olarak kullanılan Microsoft kimlik platformu geliştirici kavramları ve özellikleri için terimler listesi.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 12/13/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jmprieur, saeeda, jesakowi, nacanuma
ms.openlocfilehash: ce98d2db86c87ac6aa8fa4872bc076714467d32f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263055"
---
# <a name="microsoft-identity-platform-developer-glossary"></a>Microsoft kimlik platformu geliştirici sözlüğü

Bu makalede, Microsoft kimlik platformlarını kullanarak uygulama geliştirme hakkında bilgi edinirken yararlı olan bazı temel geliştirici kavramları ve terminolojisi için tanımlar içerir.

## <a name="access-token"></a>erişim belirteci

Bir [yetkilendirme sunucusu](#authorization-server)tarafından verilen ve istemci [uygulaması](#client-application) tarafından korumalı bir [kaynak sunucusuna](#resource-server)erişmek için kullanılan bir güvenlik [belirteci](#security-token) türü. Genellikle bir [JSON Web Belirteci (JWT)][JWT]şeklinde, belirteç [kaynak sahibi](#resource-owner)tarafından istemciye verilen yetki yi temsil eder, istenen erişim düzeyi için. Belirteç, istemci uygulamasının belirli bir kaynağa erişirken kimlik bilgisi biçimi olarak kullanmasını sağlayan, konuyla ilgili tüm geçerli [talepleri](#claim) içerir. Bu, kaynak sahibinin kimlik bilgilerini istemciye ifşa etme gereksinimini de ortadan kaldırır.

Erişim belirteçleri, temsil edilen kimlik bilgilerine bağlı olarak bazen "Kullanıcı+Uygulama" veya "Yalnızca Uygulama" olarak adlandırılır. Örneğin, bir istemci uygulaması aşağıdakileri kullandığında:

* ["Yetkilendirme kodu" yetkilendirme](#authorization-grant)hibesi, son kullanıcı önce kaynak sahibi olarak kimlik doğrulaması vererek, kaynağa erişmek için istemciye yetki verir. İstemci, erişim jetonunu alırken daha sonra kimlik doğrular. Belirteç bazen daha özel olarak "Kullanıcı+Uygulama" belirteci olarak adlandırılabilir, çünkü hem istemci uygulamasını yetkilendirmeyi sağlayan kullanıcıyı hem de uygulamayı temsil eder.
* ["İstemci kimlik bilgileri" yetkilendirme](#authorization-grant)hibesi, istemci tek kimlik doğrulaması sağlar, kaynak sahibinin kimlik doğrulaması/yetkilendirmesi olmadan çalışır, böylece belirteç bazen "Yalnızca Uygulama" belirteci olarak adlandırılabilir.

Daha fazla bilgi için [Microsoft kimlik platformu Token Reference'a][AAD-Tokens-Claims] bakın.

## <a name="application-id-client-id"></a>başvuru kimliği (istemci kimliği)

Benzersiz tanımlayıcı Azure AD, belirli bir uygulamayı ve ilişkili yapılandırmaları tanımlayan bir uygulama kaydına neden olur. Bu uygulama kimliği[(istemci kimliği)](https://tools.ietf.org/html/rfc6749#page-15)kimlik doğrulama isteklerini gerçekleştirirken kullanılır ve geliştirme zamanında kimlik doğrulama kitaplıklarına sağlanır. Uygulama kimliği (istemci kimliği) bir sır değildir.

## <a name="application-manifest"></a>uygulama bildirimi

Uygulamanın kimlik yapılandırmasının JSON gösterimini oluşturan [Azure portalı][AZURE-portal]tarafından sağlanan ve ilişkili [Uygulama][Graph-App-Resource] ve [Hizmet Müdürü][Graph-Sp-Resource] varlıklarını güncelleştirmek için bir mekanizma olarak kullanılan bir özelliktir. Daha fazla ayrıntı için [Azure Active Directory uygulama bildirimini anlama][AAD-App-Manifest] hakkında bilgi edinin.

## <a name="application-object"></a>uygulama nesnesi

[Azure portalında][AZURE-portal]bir uygulamayı kaydettiğinizde/güncellediğinizde, portal hem bir uygulama nesnesi hem de bu kiracı için ilgili [hizmet ana nesnesi](#service-principal-object) oluşturur/güncelleştirir. Uygulama nesnesi, uygulamanın kimlik yapılandırmasını genel olarak *tanımlar* (erişimi olan tüm kiracılar arasında), ilgili hizmet ana nesnesinin(ler) çalışma zamanında (belirli bir kiracıda) yerel olarak kullanılmak üzere *türetildiği* bir şablon sağlar.

Daha fazla bilgi için [Bkz. Uygulama ve Hizmet Ana Nesneleri.][AAD-App-SP-Objects]

## <a name="application-registration"></a>uygulama kaydı

Bir uygulamanın Kimlik ve Erişim Yönetimi işlevleriyle tümleştirmesine ve Azure AD'ye devretmesine izin vermek için, bir Azure AD [kiracısına](#tenant)kaydedilmesi gerekir. Uygulamanızı Azure AD'ye kaydettiğinizde, uygulamanız için azure AD ile tümleştirme ve şu gibi özellikleri kullanmasına olanak tanıyan bir kimlik yapılandırması sağlarsınız:

* Azure AD Kimlik Yönetimi ve [OpenID Connect][OpenIDConnect] protokol uygulamasını kullanarak Tek Oturum Açma'nın sağlam yönetimi
* OAuth 2.0 [yetkilendirme sunucusu](#authorization-server) [aracılığıyla, istemci uygulamaları](#client-application)tarafından korunan [kaynaklara](#resource-server) aracılı erişim
* Kaynak sahibi nin yetkisini temel alan, istemcinin korumalı kaynaklara erişimini yönetmek için [onay çerçevesi.](#consent)

Daha fazla ayrıntı için [uygulamaları Azure Active Directory ile tümleştirme][AAD-Integrating-Apps] ye bakın.

## <a name="authentication"></a>kimlik doğrulaması

Bir tarafa meşru kimlik bilgileri için meydan okuma eylemi, kimlik ve erişim denetimi için kullanılacak bir güvenlik ilkesi oluşturulmasına temel teşkil eden eylem. Örneğin, [OAuth2 yetkilendirme hibesi](#authorization-grant) sırasında, kullanılan hibeye bağlı olarak, oauth2 yetkilendirme hibesi sırasında, taraf kimlik doğrulaması [kaynak sahibi](#resource-owner) veya istemci [uygulamasırolünü](#client-application)dolduruyor.

## <a name="authorization"></a>yetkilendirme

Bir şey yapmak için doğrulanmış bir güvenlik müdürü ne izin verme eylemi. Azure AD programlama modelinde iki temel kullanım örneği vardır:

* [OAuth2 yetkilendirme hibe](#authorization-grant) akışı sırasında: [kaynak sahibi](#resource-owner) [istemci uygulamasına](#client-application)yetki verdiğinde, istemcinin kaynak sahibinin kaynaklarına erişmesine izin verir.
* İstemci tarafından kaynak erişimi sırasında: [kaynak sunucusu](#resource-server)tarafından uygulandığı gibi, erişim [belirtecinde](#access-token) bulunan [talep](#claim) değerlerini kullanarak erişim denetimi kararlarını bunlara dayalı olarak verir.

## <a name="authorization-code"></a>yetkilendirme kodu

Kısa ömürlü bir "belirteç" yetkilendirme [bitiş noktası](#authorization-endpoint)tarafından bir [istemci uygulamasına](#client-application) sağlanan , "yetkilendirme kodu" akışının bir parçası olarak, dört OAuth2 [yetki hibe](#authorization-grant)biri . Kod, [kaynak sahibinin](#resource-owner)kimlik doğrulaması yanıt olarak istemci uygulamasına döndürülür ve kaynak sahibinin istenen kaynaklara erişmek için yetki devrettiğini gösterir. Akışın bir parçası olarak, kod daha sonra bir [erişim belirteci](#access-token)için itfa edilir.

## <a name="authorization-endpoint"></a>yetkilendirme bitiş noktası

[Yetkilendirme sunucusu](#authorization-server)tarafından uygulanan uç noktalardan biri, OAuth2 yetkilendirme hibe akışı sırasında bir [yetkilendirme hibesi](#authorization-grant) sağlamak için [kaynak sahibi](#resource-owner) ile etkileşim için kullanılır. Kullanılan yetkilendirme hibe akışına bağlı olarak, sağlanan gerçek hibe, bir [yetkilendirme kodu](#authorization-code) veya [güvenlik belirteci](#security-token)de dahil olmak üzere değişebilir.

OAuth2 belirtiminin [yetkilendirme hibe türleri][OAuth2-AuthZ-Grant-Types] ve [yetkilendirme bitiş noktası][OAuth2-AuthZ-Endpoint] bölümlerine ve daha fazla ayrıntı için [OpenIDConnect belirtimine][OpenIDConnect-AuthZ-Endpoint] bakın.

## <a name="authorization-grant"></a>yetki lendirme hibesi

[Kaynak sahibinin](#resource-owner) korumalı kaynaklarına erişme [yetkisini](#authorization) temsil eden bir kimlik bilgisi, bir [istemci uygulamasına](#client-application)verilir. Bir müşteri başvurusu, müşteri türüne/gereksinimlerine bağlı olarak hibe almak için [OAuth2 Yetkilendirme Çerçevesi tarafından tanımlanan dört hibe türünden][OAuth2-AuthZ-Grant-Types] birini kullanabilir: "yetkilendirme kodu hibesi", "istemci kimlik bilgileri hibesi", "örtülü hibe" ve "kaynak sahibi parola kimlik bilgileri hibesi". İstemciye iade edilen kimlik bilgileri, kullanılan yetkilendirme hibesinin türüne bağlı olarak bir [erişim belirteci](#access-token)veya [bir yetkilendirme kodudur](#authorization-code) (daha sonra bir erişim belirteci ile değiştirilir).

## <a name="authorization-server"></a>yetkilendirme sunucusu

[OAuth2 Yetkilendirme Çerçevesi][OAuth2-Role-Def]tarafından tanımlandığı gibi, [kaynak sahibinin](#resource-owner) başarılı bir şekilde doğrulanması ve yetkilendirmesinin alınmasından sonra [istemciye](#client-application) erişim belirteçleri vermekten sorumlu sunucu. Bir [istemci uygulaması,](#client-application) OAuth2 tanımlı [yetkilendirme hibelerine](#authorization-grant)uygun olarak, [yetkilendirme](#authorization-endpoint) ve [belirteç](#token-endpoint) uç noktaları aracılığıyla çalışma zamanında yetkilendirme sunucusu ile etkileşime girmektedir.

Microsoft kimlik platformu uygulama tümleştirmesi durumunda, Microsoft kimlik platformu Azure AD uygulamaları ve Microsoft hizmet API'leri (örneğin [Microsoft Graph API'leri)][Microsoft-Graph]için yetkilendirme sunucusu rolünü uygular.

## <a name="claim"></a>Iddia

[Güvenlik belirteci,](#security-token) bir varlık [(istemci uygulaması](#client-application) veya kaynak [sahibi](#resource-owner)gibi) hakkında başka bir varlığa [(kaynak sunucusu](#resource-server)gibi) sahip olunan talepleri içerir. Talepler, belirteç konusuyla ilgili gerçekleri aktaran ad/değer çiftleridir (örneğin, [yetkilendirme sunucusu](#authorization-server)tarafından kimlik doğrulaması yapılan güvenlik ilkesi). Belirli bir belirteçte bulunan talepler, belirteç türü, öznenin kimliğini doğrulamak için kullanılan kimlik bilgisi türü, uygulama yapılandırması vb. dahil olmak üzere çeşitli değişkenlere bağlıdır.

Daha fazla ayrıntı için [Microsoft kimlik platformu belirteci başvurusuna][AAD-Tokens-Claims] bakın.

## <a name="client-application"></a>istemci uygulaması

[OAuth2 Yetkilendirme Çerçevesi][OAuth2-Role-Def]tarafından tanımlandığı gibi, [kaynak sahibi](#resource-owner)adına korumalı kaynak istekleri yapan bir uygulama. "İstemci" terimi belirli donanım uygulama özellikleri (örneğin, uygulamanın sunucuda, masaüstünde veya diğer aygıtlarda yürütülüp yürütülmediği) anlamına gelmez.

İstemci başvurusu, [oAuth2 yetkilendirme hibe](#authorization-grant) akışına katılmak için kaynak sahibinden [yetki](#authorization) ister ve kaynak sahibi adına API'lere/verilere erişebilir. OAuth2 Yetkilendirme Çerçevesi, müşterinin kimlik bilgilerinin gizliliğini koruma becerisine bağlı olarak "gizli" ve "herkese açık" olmak üzere [iki tür müşteri tanımlar.][OAuth2-Client-Types] Uygulamalar, bir web sunucusunda çalışan bir [web istemcisi (gizli),](#web-client) aygıta yüklenen yerel istemci [(ortak)](#native-client) veya bir aygıtın tarayıcısında çalışan kullanıcı aracısı tabanlı istemci [(herkese açık)](#user-agent-based-client) uygulayabilir.

## <a name="consent"></a>Izni

Bir kaynak [sahibinin,](#resource-owner) kaynak sahibi adına, belirli [izinler](#permissions)altında korunan kaynaklara erişmek için istemci [uygulamasına](#client-application)yetki verme işlemi. İstemci tarafından istenen izinlere bağlı olarak, bir yönetici veya kullanıcıdan sırasıyla kuruluş/bireysel verilerine erişime izin vermek için izin istenir. Çok [kiracılı](#multi-tenant-application) bir senaryoda, uygulamanın [hizmet sorumlusu](#service-principal-object) da onay layan kullanıcının kiracısında kaydedilir.

Daha fazla bilgi için [onay çerçevesine](consent-framework.md) bakın.

## <a name="id-token"></a>Kimlik belirteci

Bir [yetkilendirme sunucusunun yetkilendirme](#authorization-server) [bitiş noktası](#authorization-endpoint)tarafından sağlanan ve son kullanıcı kaynağı [sahibinin](#resource-owner)kimlik doğrulamasıyla ilgili [talepleri](#claim) içeren [OpenID Connect][OpenIDConnect-ID-Token] [güvenlik belirteci.](#security-token) Erişim belirteci gibi, kimlik belirteçleri de dijital olarak imzalanmış [JSON Web Belirteci (JWT)][JWT]olarak temsil edilir. Ancak bir erişim belirtecinin aksine, kimlik belirteci talepleri kaynak erişimi ve özellikle erişim denetimiyle ilgili amaçlar için kullanılmaz.

Daha fazla ayrıntı için [Microsoft kimlik platformu belirteci başvurusuna][AAD-Tokens-Claims] bakın.

## <a name="microsoft-identity-platform"></a>Microsoft kimlik platformu

Microsoft Identity Platform, Azure Active Directory (Azure AD) kimlik hizmeti ve geliştirici platformunun geliştirilmesiyle ortaya çıkmıştır. Bu platform geliştiricilerin tüm Microsoft kimlikleriyle oturum açan ve Microsoft Graph veya diğer Microsoft API'leri ya da geliştiricilerin derlemiş olduğu API'lere çağrı göndermek için gerekli belirteçleri alan uygulamalar derlemesini sağlar. Kimlik doğrulama hizmeti, kitaplıklar, uygulama kaydı ve yapılandırması, tam geliştirici belgeleri, kod örnekleri ve diğer geliştirici içeriğinden oluşan tam özellikli bir platformdur. Microsoft Identity Platform OAuth 2.0 ve OpenID Connect gibi sektör standardı protokolleri destekler. Daha fazla bilgi için [Microsoft kimlik platformu hakkında](about-microsoft-identity-platform.md) bilgi edinin.

## <a name="multi-tenant-application"></a>çok kiracılı uygulama

Müşterinin kayıtlı olduğu kiracı dışındaki kiracılar da dahil olmak üzere, herhangi bir Azure AD [kiracısında](#tenant)bulunan kullanıcılar tarafından oturum açma ve [izin alma](#consent) olanağı sağlayan bir uygulama sınıfı. [Yerel istemci](#native-client) uygulamaları varsayılan olarak çok kiracılı dır, web [istemcisi](#web-client) ve [web kaynağı/API](#resource-server) uygulamaları tek veya çok kiracı arasında seçim yapma yeteneğine sahiptir. Bunun aksine, tek kiracı olarak kaydedilmiş bir web uygulaması, yalnızca uygulamanın kayıtlı olduğu kiracıyla aynı kiracıda sağlanan kullanıcı hesaplarından oturum açmalarına izin verir.

Daha fazla ayrıntı [için çok kiracılı uygulama deseni kullanarak herhangi bir Azure AD kullanıcısını nasıl oturum alabilirsiniz.][AAD-Multi-Tenant-Overview]

## <a name="native-client"></a>yerli müşteri

Aygıta yerel olarak yüklenen istemci [uygulaması](#client-application) türü. Tüm kod bir aygıtta yürütüldolduğundan, kimlik bilgilerini özel/gizli olarak depolayamaması nedeniyle "ortak" istemci olarak kabul edilir. Daha fazla bilgi için [OAuth2 istemci türleri ve profillerine][OAuth2-Client-Types] bakın.

## <a name="permissions"></a>Izin

[İstemci uygulaması,](#client-application) izin isteklerini beyan ederek [kaynak sunucusuna](#resource-server) erişim sağlar. İki tür mevcuttur:

* Oturum açmış [kaynak sahibinden](#resource-owner)gelen temsilci yetkilendirmesini kullanarak [kapsam tabanlı](#scopes) erişimi belirten "Temsilci" izinleri, istemcinin [erişim belirtecinde](#access-token) ["scp" talepleri](#claim) olarak çalışma zamanında kaynağa sunulur.
* İstemci uygulamasının kimlik bilgilerini/kimliğini kullanarak [rol tabanlı](#roles) erişimi belirten "Uygulama" izinleri, istemcinin erişim belirtecinde ["roller" talepleri](#claim) olarak çalışma zamanında kaynağa sunulur.

Ayrıca, yöneticiye veya kaynak sahibine istemcinin kiracılarındaki kaynaklara erişimini verme/reddetme fırsatı [vererek, onay](#consent) süreci sırasında da ortaya çıkarlar.

İzin istekleri, [Azure portalındaki][AZURE-portal]bir uygulama için **API izinleri** sayfasında, istenen "Yetkiverilen İzinler" ve "Uygulama İzinleri" seçilerek yapılandırılır (ikincisi Genel Yönetici rolüne üyelik gerektirir). Ortak [istemci](#client-application) kimlik bilgilerini güvenli bir şekilde koruyamadığından, yalnızca temsilci izinleri isteyebilir, gizli bir [istemci](#client-application) ise hem temsilci hem de uygulama izinleri isteme yeteneğine sahiptir. İstemcinin [uygulama nesnesi,](#application-object) beyan edilen izinleri gerekli Kaynak Access [özelliğinde][Graph-App-Resource]depolar.

## <a name="resource-owner"></a>kaynak sahibi

[OAuth2 Yetkilendirme Çerçevesi][OAuth2-Role-Def]tarafından tanımlandığı gibi, korumalı bir kaynağa erişim izni verebilecek bir varlıktır. Kaynak sahibi bir kişi olduğunda, son kullanıcı olarak adlandırılır. Örneğin, bir [istemci uygulaması](#client-application) [Microsoft Graph API][Microsoft-Graph]üzerinden bir kullanıcının posta kutusuna erişmek istediğinde, posta kutusunun kaynak sahibinden izin gerektirir.

## <a name="resource-server"></a>kaynak sunucusu

[OAuth2 Yetkilendirme Çerçevesi][OAuth2-Role-Def]tarafından tanımlandığı gibi, korumalı kaynakları barındıran, [erişim belirteci](#access-token)sunan [istemci uygulamaları](#client-application) tarafından korumalı kaynak isteklerini kabul etme ve yanıtlama yeteneğine sahip bir sunucu. Korumalı kaynak sunucusu veya kaynak uygulaması olarak da bilinir.

Bir kaynak sunucusu API'leri ortaya çıkarır ve OAuth 2.0 Yetkilendirme Çerçevesi'ni kullanarak [kapsamlar](#scopes) ve [roller](#roles)aracılığıyla korumalı kaynaklarına erişimi zorlar. Bunlara örnek olarak Azure AD kiracı verilerine erişim sağlayan [Microsoft Graph API'sı][Microsoft-Graph] ve posta ve takvim gibi verilere erişim sağlayan Office 365 API'leri verilebilir. 

İstemci uygulaması gibi, kaynak uygulamanın kimlik yapılandırması da bir Azure AD kiracıya [kaydolarak](#application-registration) kurulur ve hem uygulama hem de hizmet ana nesnesi sağlar. Microsoft Graph API gibi Microsoft tarafından sağlanan bazı API'ler, sağlama sırasında tüm kiracılarda kullanıma sunulan önceden kaydedilmiş hizmet ilkelerine sahiptir.

## <a name="roles"></a>roles

[Kapsamlar](#scopes)gibi roller de, [bir kaynak sunucusunun](#resource-server) korumalı kaynaklarına erişimi yönetmesi için bir yol sağlar. İki tür vardır: "kullanıcı" rolü, kaynağa erişim gerektiren kullanıcılar/gruplar için rol tabanlı erişim denetimini uygularken, "uygulama" rolü erişim gerektiren [istemci uygulamaları](#client-application) için de aynı şeyi uygular.

Roller kaynak tanımlı dizeleridir (örneğin"Gider onaylayıcısı", "Salt okunur", "Directory.ReadWrite.All"), kaynağın [uygulama bildirimi](#application-manifest)üzerinden [Azure portalında][AZURE-portal] yönetilir ve kaynağın [appRoles özelliğinde][Graph-Sp-Resource]depolanır. Azure portalı, kullanıcıları "kullanıcı" rollerine atamak ve istemci [uygulama izinlerini](#permissions) bir "uygulama" rolüne erişmek üzere yapılandırmak için de kullanılır.

Microsoft Graph API tarafından ortaya çıkarılan uygulama rollerinin ayrıntılı bir tartışması için [Bkz. Grafik API İzin Kapsamları.][Graph-Perm-Scopes] Adım adım uygulama örneği için Bkz. [RBAC ve Azure portalını kullanarak erişimi yönet.][AAD-RBAC]

## <a name="scopes"></a>scopes

[Roller](#roles)gibi kapsamlar da bir [kaynak sunucusunun](#resource-server) korumalı kaynaklarına erişimi yönetmesi için bir yol sağlar. Kapsamlar, sahibi tarafından kaynağa yetki verilmiş bir [istemci uygulaması](#client-application) için [kapsam tabanlı][OAuth2-Access-Token-Scopes] erişim denetimini uygulamak için kullanılır.

Kapsamlar kaynak tanımlı dizeleri (örneğin "Mail.Read", "Directory.ReadWrite.All"), kaynağın uygulama [bildirimi](#application-manifest)üzerinden [Azure portalında][AZURE-portal] yönetilen ve kaynağın [oauth2Permissions özelliğinde][Graph-Sp-Resource]depolanan dizeleridir. Azure portalı, bir kapsama erişmek için istemci uygulaması [nın devredilen izinlerini](#permissions) yapılandırmak için de kullanılır.

En iyi uygulama adlandırma kuralı, bir "kaynak.operation.constraint" biçimi kullanmaktır. Microsoft Graph API tarafından ortaya çıkarılan kapsamların ayrıntılı bir tartışması için [Bkz. Grafik API İzin Kapsamları.][Graph-Perm-Scopes] Office 365 hizmetleri tarafından açıkta kalan kapsamlar için Bkz. [Office 365 API izinleri başvurusu.][O365-Perm-Ref]

## <a name="security-token"></a>güvenlik belirteci

OAuth2 jetonu veya SAML 2.0 iddiası gibi talepleri içeren imzalı bir belge. OAuth2 [yetkilendirme izni için,](#authorization-grant) [bir erişim belirteci](#access-token) (OAuth2) ve bir [kimlik belirteci](https://openid.net/specs/openid-connect-core-1_0.html#IDToken) güvenlik belirteçleri türleri, her ikisi de bir [JSON Web Belirteci (JWT)][JWT]olarak uygulanır.

## <a name="service-principal-object"></a>hizmet ana nesnesi

[Azure portalında][AZURE-portal]bir uygulamayı kaydettiğinizde/güncellediğinizde, portal hem bir [uygulama nesnesi](#application-object) hem de bu kiracı için ilgili hizmet ana nesnesi oluşturur/güncelleştirir. Uygulama nesnesi, uygulamanın kimlik yapılandırmasını genel olarak *tanımlar* (ilişkili uygulamanın erişim eki verilen tüm kiracılar arasında) ve ilgili hizmet ana nesnesinin (ler) çalışma zamanında (belirli bir kiracıda) yerel olarak kullanılmak üzere *türetildiği* şablondur.

Daha fazla bilgi için [Bkz. Uygulama ve Hizmet Ana Nesneleri.][AAD-App-SP-Objects]

## <a name="sign-in"></a>sign-in

Bir [güvenlik belirteci](#security-token) edinme ve uygulama oturumunu bu duruma dahil etmek amacıyla son kullanıcı kimlik doğrulamasını [başlatan](#client-application) ve ilgili durumu yakalama işlemi. Durum, kullanıcı profili bilgileri gibi yapıları ve belirteç taleplerinden türetilen bilgileri içerebilir.

Bir uygulamanın oturum açma işlevi genellikle tek oturum açma (SSO) uygulamak için kullanılır. Bir son kullanıcının bir uygulamaya erişebilmek için giriş noktası olarak (ilk oturum açma da) bir "kaydolma" işlevi de olabilir. Kaydolma işlevi, kullanıcıya özgü ek durumu toplamak ve devam etmek için kullanılır ve [kullanıcı onayı](#consent)gerektirebilir.

## <a name="sign-out"></a>oturumu kapatma

Oturum [açma](#sign-in) sırasında [istemci uygulama](#client-application) oturumuyla ilişkili kullanıcı durumunu ayırma, son kullanıcının kimliğinin doğrulanması işlemi

## <a name="tenant"></a>Kiracı

Azure AD dizininin bir örneği Azure AD kiracı olarak adlandırılır. Çeşitli özellikler sağlar:

* entegre uygulamalar için bir kayıt hizmeti
* kullanıcı hesaplarının ve kayıtlı uygulamaların kimlik doğrulaması
* OAuth2 ve SAML dahil olmak üzere çeşitli protokolleri desteklemek için gerekli REST uç noktaları, [yetkilendirme bitiş noktası](#authorization-endpoint)da dahil olmak üzere, [belirteç bitiş noktası](#token-endpoint) ve çok [kiracılı uygulamalar](#multi-tenant-application)tarafından kullanılan "ortak" bitiş noktası.

Azure AD kiracıları, abonelik için Kimlik & Erişim Yönetimi özellikleri sağlayarak kaydolma sırasında Azure ve Office 365 abonelikleriyle oluşturulur/ilişkilendirilir. Azure abonelik yöneticileri, Azure portalı üzerinden ek Azure AD kiracıları da oluşturabilir. Bir kiracıya erişmenin çeşitli yolları hakkında ayrıntılı bilgi [için Azure Etkin Dizin kiracısını nasıl][AAD-How-To-Tenant] edinebileceğinize bakın. Abonelikler ve Azure AD kiracı arasındaki ilişki hakkında ayrıntılı bilgi için [Azure aboneliklerinin Azure Etkin Dizini ile nasıl][AAD-How-Subscriptions-Assoc] ilişkilendirilene bakın.

## <a name="token-endpoint"></a>belirteç bitiş noktası

OAuth2 [yetkilendirme hibeleri](#authorization-grant)desteklemek için [yetkilendirme sunucusu](#authorization-server) tarafından uygulanan uç noktalardan biri. Hibebağlı olarak, [openid connect][OpenIDConnect] protokolü ile kullanıldığında bir [istemciye](#client-application)(ve ilgili "yenileme" belirtecine) bir [erişim](#access-token) jetonu (ve ilgili "yenileme" [belirteci)](#id-token) elde etmek için kullanılabilir.

## <a name="user-agent-based-client"></a>Kullanıcı aracısı tabanlı istemci

Bir web sunucusundan kod indiren ve tek sayfalık bir uygulama (SPA) gibi bir kullanıcı aracısı (örneğin, bir web tarayıcısı) içinde yürüten istemci [uygulaması](#client-application) türü. Tüm kod bir aygıtta yürütüldolduğundan, kimlik bilgilerini özel/gizli olarak depolayamaması nedeniyle "ortak" istemci olarak kabul edilir. Daha fazla bilgi için [OAuth2 istemci türleri ve profillerine][OAuth2-Client-Types]bakın.

## <a name="user-principal"></a>kullanıcı ilkesi

Bir hizmet ana nesnesinin bir uygulama örneğini temsil etmek için kullanılma şekline benzer şekilde, kullanıcı ana nesnesi de bir kullanıcıyı temsil eden başka bir güvenlik ilkesi türüdür. Microsoft Graph [User kaynak türü,][Graph-User-Resource] ad ve soyad, kullanıcı ana adı, dizin rol üyeliği vb. gibi kullanıcıyla ilgili özellikler de dahil olmak üzere bir kullanıcı nesnesinin şemasını tanımlar. Bu, Azure AD'nin çalışma zamanında bir kullanıcı ilkesi oluşturması için kullanıcı kimliği yapılandırmasını sağlar. Kullanıcı ilkesi, Tek Oturum Açma, [onay](#consent) delegasyonu nun kaydedilmesi, erişim denetimi kararlarının alınması vb. için kimlik doğrulaması yapılan bir kullanıcıyı temsil etmek için kullanılır.

## <a name="web-client"></a>web istemcisi

Bir web sunucusundaki tüm kodu yürüten ve kimlik bilgilerini sunucuda güvenli bir şekilde depolayarak "gizli" istemci işlevi görebilen istemci [uygulaması](#client-application) türüdür. Daha fazla bilgi için [OAuth2 istemci türleri ve profillerine][OAuth2-Client-Types]bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft kimlik platformu Developer's Guide,][AAD-Dev-Guide] uygulama [tümleştirmesine][AAD-How-To-Integrate] genel bakış ve Microsoft kimlik platformu kimlik doğrulama ve [desteklenen kimlik doğrulama senaryoları][AAD-Auth-Scenarios]dahil olmak üzere tüm Microsoft kimlik platformu geliştirme yle ilgili konular için kullanılacak açılış sayfasıdır. Ayrıca, [GitHub'da](https://github.com/azure-samples?utf8=%E2%9C%93&q=active%20directory&type=&language=)nasıl çalışır ve çalışır durumda sınız kod örnekleri & öğreticiler de bulabilirsiniz.

Geri bildirim sağlamak ve yeni tanımlar veya mevcut tanımları güncelleme istekleri de dahil olmak üzere bu içeriği hassaslaştırmak ve şekillendirmek için aşağıdaki yorumlar bölümünü kullanın!

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]:reference-app-manifest.md
[AAD-App-SP-Objects]:app-objects-and-service-principals.md
[AAD-Auth-Scenarios]:authentication-scenarios.md
[AAD-Dev-Guide]:azure-ad-developers-guide.md
[Graph-Perm-Scopes]: /graph/permissions-reference
[Graph-App-Resource]: /graph/api/resources/application
[Graph-Sp-Resource]: /graph/api/resources/serviceprincipal?view=graph-rest-beta
[Graph-User-Resource]: /graph/api/resources/user
[AAD-How-Subscriptions-Assoc]:../fundamentals/active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]:quickstart-create-new-tenant.md
[AAD-Integrating-Apps]:quickstart-v1-integrate-apps-with-azure-ad.md
[AAD-Multi-Tenant-Overview]:howto-convert-app-to-be-multi-tenant.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]:access-tokens.md
[AZURE-portal]: https://portal.azure.com
[AAD-RBAC]: ../../role-based-access-control/role-assignments-portal.md
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://developer.microsoft.com/graph
[O365-Perm-Ref]: https://msdn.microsoft.com/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: https://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: https://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: https://openid.net/specs/openid-connect-core-1_0.html#IDToken
