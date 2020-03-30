---
title: Azure Erişim Denetim Hizmetinden geçiş | Microsoft Dokümanlar
description: Azure Erişim Kontrol Hizmeti'nden (ACS) uygulamaları ve hizmetleri taşıma seçenekleri hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.custom: aaddev
ms.topic: conceptual
ms.workload: identity
ms.date: 10/03/2018
ms.author: ryanwi
ms.reviewer: jlu, annaba, hirsin
ROBOTS: NOINDEX
ms.openlocfilehash: 3168d36bf4c2d3c696173725f669b12dc168dcc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80155010"
---
# <a name="how-to-migrate-from-the-azure-access-control-service"></a>Nasıl yapilir: Azure Erişim Denetim Hizmeti'nden geçiş

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Azure Active Directory (Azure AD) hizmeti olan Microsoft Azure Erişim Denetim Hizmeti (ACS), 7 Kasım 2018'de kullanımdan kaldırılacaktır. Şu anda Access Denetimi'ni kullanan uygulamalar ve hizmetler, o zamana kadar tamamen farklı bir kimlik doğrulama mekanizmasına geçirilmelidir. Bu makalede, Access Denetimi kullanımınızı amortismana ksilmeyi planladığınız için geçerli müşterilere yönelik öneriler açıklanmaktadır. Şu anda Erişim Denetimi kullanmıyorsanız, herhangi bir işlem yapmanız gerekmez.

## <a name="overview"></a>Genel Bakış

Access Control, web uygulamalarınıza ve hizmetlerinize erişim için kullanıcıları doğrulamak ve yetkilendirmek için kolay bir yol sunan bir bulut kimlik doğrulama hizmetidir. Kimlik doğrulama ve yetkilendirmenin birçok özelliğinin kodunuzdan dışlanmasına olanak tanır. Access Control, öncelikle Microsoft .NET istemcilerinin geliştiricileri ve mimarları, ASP.NET web uygulamaları ve Windows Communication Foundation (WCF) web hizmetleri tarafından kullanılır.

Access Denetimi için kullanım örnekleri üç ana kategoriye ayrılabilir:

- Azure Hizmet Veri Servisi ve Dynamics CRM dahil olmak üzere belirli Microsoft bulut hizmetlerine kimlik doğrulama. İstemci uygulamaları, çeşitli eylemleri gerçekleştirmek için bu hizmetlere kimlik doğrulaması yapmak için Access Denetimi'nden belirteçler alır.
- Web uygulamalarına kimlik doğrulama ekleme, hem özel hem de önceden paketlenmiş (SharePoint gibi). Access Denetimi "pasif" kimlik doğrulaması kullanarak, web uygulamaları bir Microsoft hesabı (eski adıyla Live ID) ve Google, Facebook, Yahoo, Azure AD ve Active Directory Federation Services (AD FS) hesaplarında oturum açmayı destekleyebilir.
- Access Control tarafından verilen belirteçlerle özel web hizmetlerinin güvenliğini sağlama. Web hizmetleri ,"etkin" kimlik doğrulamasını kullanarak, yalnızca Erişim Denetimi ile kimlik doğrulaması yapmış bilinen istemcilere erişime izin verdiklerinden emin olabilir.

Bu kullanım örneklerinin her biri ve önerilen geçiş stratejileri aşağıdaki bölümlerde ele alınmıştır.

> [!WARNING]
> Çoğu durumda, varolan uygulamaları ve hizmetleri yeni teknolojilere geçirmek için önemli kod değişiklikleri gerekir. Olası kesintileri veya kesintileri önlemek için geçişinizi planlamaya ve yürütmeye hemen başlamanızı öneririz.

Access Control aşağıdaki bileşenlere sahiptir:

- Kimlik doğrulama isteklerialan ve karşılığında güvenlik belirteçleri veren güvenli bir belirteç hizmeti (STS).
- Access Denetimi ad alanlarını oluşturduğunuz, sildiğiniz ve etkinleştirdiğiniz ve devre dışı bıraktığınız Azure klasik portalı.
- Access Denetimi ad alanlarını özelleştirip yapılandırdığınız ayrı bir Access Control yönetim portalı.
- Portalların işlevlerini otomatikleştirmek için kullanabileceğiniz bir yönetim hizmeti.
- Access Control sorunları nın belirteçlerinin çıkış biçimini işlemek için karmaşık mantığı tanımlamak için kullanabileceğiniz belirteç dönüştürme kuralı altyapısı.

Bu bileşenleri kullanmak için bir veya daha fazla Access Denetimi ad alanı oluşturmanız gerekir. *Ad alanı,* uygulamalarınızın ve hizmetlerinizin iletişim kurduğu özel bir Erişim Denetimi örneğidir. Ad alanı diğer tüm Access Control müşterilerinden yalıtılır. Diğer Access Control müşterileri kendi ad alanlarını kullanır. Access Denetimi'ndeki bir ad alanının şuna benzeyen özel bir URL'si vardır:

```HTTP
https://<mynamespace>.accesscontrol.windows.net
```

STS ve yönetim işlemleri ile tüm iletişim bu URL'de yapılır. Farklı amaçlar için farklı yollar kullanıyorsunuz. Uygulamalarınızın veya hizmetlerinizin Access Denetimi'ni kullanıp kullanmadığını&gt;belirlemek için, herhangi bir trafiği ad alanı .accesscontrol.windows.net https://&lt;için izleyin. Bu URL'ye yapılan tüm trafik Erişim Denetimi tarafından gerçekleştirilir ve durdurulması gerekir. 

Bunun istisnası herhangi bir `https://accounts.accesscontrol.windows.net`trafik . Bu URL'ye gelen trafik zaten farklı bir hizmet tarafından işlenir ve Access Denetimi amortismanından **etkilenmez.** 

Access Control hakkında daha fazla bilgi için [Erişim Denetim Hizmeti 2.0 (arşivlenmiş)](https://msdn.microsoft.com/library/hh147631.aspx)bakın.

## <a name="find-out-which-of-your-apps-will-be-impacted"></a>Uygulamalarınızdan hangilerinin etkileneceğini öğrenin

Uygulamalarınızdan hangisinin ACS'nin emekliliğinden etkileneceğini öğrenmek için bu bölümdeki adımları izleyin.

### <a name="download-and-install-acs-powershell"></a>ACS PowerShell'i indirin ve kurun

1. PowerShell Galerisi'ne gidin ve [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2)indirin.
2. Çalıştırarak modülü yükleyin

    ```powershell
    Install-Module -Name Acs.Namespaces
    ```

3. Çalıştırarak tüm olası komutların listesini alın

    ```powershell
    Get-Command -Module Acs.Namespaces
    ```

    Belirli bir komutla ilgili yardım almak için çalıştırın:

    ```
     Get-Help [Command-Name] -Full
    ```
    
    ACS komutunun adı nerededir. `[Command-Name]`

### <a name="list-your-acs-namespaces"></a>ACS ad boşluklarınızı listele

1. **Connect-AcsAccount** cmdlet'i kullanarak ACS'ye bağlanın.
  
    Komutları yürütmek `Set-ExecutionPolicy -ExecutionPolicy Bypass` ve komutları yürütmek için bu aboneliklerin yöneticisi olmak için önce çalıştırmak gerekebilir.

2. **Get-AcsSubscription** cmdlet'i kullanarak kullanılabilir Azure aboneliklerinizi listele.
3. **Get-AcsNamespace** cmdlet'i kullanarak ACS ad boşluklarınızı listele.

### <a name="check-which-applications-will-be-impacted"></a>Hangi uygulamaların etkileneceğini kontrol edin

1. Önceki adımdaki ad alanını kullanın ve`https://<namespace>.accesscontrol.windows.net`

    Örneğin, ad alanlarından biri contoso-test ise,`https://contoso-test.accesscontrol.windows.net`

2. **Güven ilişkileri**altında, ACS emekliliğinden etkilenecek uygulamaların listesini görmek için **Güvenerek parti uygulamalarını** seçin.
3. Sahip olduğunuz diğer ACS ad alanı(lar) için 1-2 adımlarını yineleyin.

## <a name="retirement-schedule"></a>Emeklilik takvimi

Kasım 2017 itibariyle tüm Access Control bileşenleri tam olarak desteklenir ve çalışır durumdadır. Tek kısıtlama, [Azure klasik portalı üzerinden yeni Access Denetimi ad alanları oluşturamamanızdır.](https://azure.microsoft.com/blog/acs-access-control-service-namespace-creation-restriction/)

Erişim Denetimi bileşenlerini küçümseme programı aşağıda verebilmiştir:

- **Kasım 2017**: Azure klasik portalındaki Azure AD yöneticisi deneyimi [kullanımdan kaldırıldı.](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/18/marching-into-the-future-of-the-azure-ad-admin-experience-retiring-the-azure-classic-portal/) Bu noktada, Access Control için ad alanı yönetimi yeni `https://manage.windowsazure.com?restoreClassic=true`ve özel bir URL'de kullanılabilir: . Varolan ad alanlarınızı görüntülemek, ad alanlarını etkinleştirmek ve devre dışı etmek ve isterseniz ad alanlarını silmek için bu URl'u kullanın.
- **2 Nisan 2018**: Azure klasik portalı tamamen kullanımdan kaldırılmıştır, bu da Access Control ad alanı yönetiminin artık herhangi bir URL üzerinden kullanılamadığı anlamına gelir. Bu noktada, Access Denetimi ad alanlarınızı devre dışı kılamaz, etkinleştiremez veya sayamazsınız. Ancak, Erişim Denetimi yönetim portalı tamamen işlevsel `https://\<namespace\>.accesscontrol.windows.net`olacak ve . Access Control'ün diğer tüm bileşenleri normal şekilde çalışmaya devam eder.
- **7 Kasım 2018**: Tüm Erişim Denetimi bileşenleri kalıcı olarak kapatıldı. Buna Erişim Denetimi yönetim portalı, yönetim hizmeti, STS ve belirteç dönüştürme kuralı altyapısı dahildir. Bu noktada, Access Denetimi'ne gönderilen (ad \<\>alanı .accesscontrol.windows.net adresinde bulunan) tüm istekler başarısız olur. Bu süreden önce varolan tüm uygulamaları ve hizmetleri diğer teknolojilere geçirmiş olmalıydınız.

> [!NOTE]
> İlke, belirli bir süre için belirteç talebinde bulunmamış ad alanlarını devre dışı kılabilir. Eylül 2018 başı itibariyle bu süre şu anda 14 günlük hareketsizlik süresine uyacaktır, ancak bu süre önümüzdeki haftalarda 7 güne kısaltılacaktır. Şu anda devre dışı bırakılmış Access Denetimi ad alanlarınız varsa, ad alanı(lar)ı yeniden etkinleştirmek için [ACS PowerShell'i karşıdan yükleyebilir ve yükleyebilirsiniz.](#download-and-install-acs-powershell)

## <a name="migration-strategies"></a>Geçiş stratejileri

Aşağıdaki bölümlerde, Access Denetimi'nden diğer Microsoft teknolojilerine geçiş için üst düzey öneriler açıklanmaktadır.

### <a name="clients-of-microsoft-cloud-services"></a>Microsoft bulut hizmetlerinin istemcileri

Access Denetimi tarafından verilen belirteçleri kabul eden her Microsoft bulut hizmeti artık en az bir alternatif kimlik doğrulama biçimini destekler. Doğru kimlik doğrulama mekanizması her hizmet için değişir. Resmi rehberlik için her hizmet için özel belgelere başvurmanızı öneririz. Kolaylık sağlamak için, her belge kümesi burada sağlanır:

| Hizmet | Rehber |
| ------- | -------- |
| Azure Service Bus | [Paylaşılan erişim imzalarına geçiş](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-migrate-acs-sas) |
| Azure Servis Veri Günü | [Paylaşılan erişim imzalarına geçiş](https://docs.microsoft.com/azure/service-bus-relay/relay-migrate-acs-sas) |
| Azure Yönetilen Önbellek | [Redis için Azure Önbelleğine geçiş](https://docs.microsoft.com/azure/azure-cache-for-redis/cache-faq#which-azure-cache-offering-is-right-for-me) |
| Azure Veri Pazarı | [Bilişsel Hizmetler API'lerine geçiş](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| BizTalk Hizmetleri | [Azure Uygulama Hizmeti'nin Mantıksal Uygulamalar özelliğine geçiş yapın](https://docs.microsoft.com/azure/machine-learning/studio/datamarket-deprecation) |
| Azure Medya Hizmetleri | [Azure AD kimlik doğrulaması'na geçirin](https://azure.microsoft.com/blog/azure-media-service-aad-auth-and-acs-deprecation/) |
| Azure Backup | [Azure Yedekleme aracısını yükseltme](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq) |

<!-- Dynamics CRM: Migrate to new SDK, Dynamics team handling privately -->
<!-- Azure RemoteApp deprecated in favor of Citrix: https://www.zdnet.com/article/microsoft-to-drop-azure-remoteapp-in-favor-of-citrix-remoting-technologies/ -->
<!-- Exchange push notifications are moving, customers don't need to move -->
<!-- Retail federation services are moving, customers don't need to move -->
<!-- Azure StorSimple: TODO -->
<!-- Azure SiteRecovery: TODO -->

### <a name="sharepoint-customers"></a>SharePoint müşterileri

SharePoint 2013, 2016 ve SharePoint Online müşterileri, bulutta, şirket içinde ve karma senaryolarda kimlik doğrulama amacıyla ACS'yi uzun zamandır kullanmıştır. Bazı SharePoint özellikleri ve kullanım örnekleri ACS'nin emekliliğinden etkilenirken, diğerleri etkilenmez. Aşağıdaki tabloda, ACS'den yararlanan en popüler SharePoint özelliklerinden bazılarıiçin geçiş kılavuzu özetlenmektedir:

| Özellik | Rehber |
| ------- | -------- |
| Azure AD'den kullanıcıların kimlik doğrulaması | Azure AD daha önce SharePoint tarafından kimlik doğrulaması için gereken SAML 1.1 belirteçlerini desteklemezken, ACS, SharePoint'i Azure AD belirteç biçimleriyle uyumlu kılan bir aracı olarak kullanılmıştır. Artık, [SharePoint'i doğrudan Azure AD Uygulaması Galeri SharePoint'i kullanarak Azure AD'ye](https://docs.microsoft.com/azure/active-directory/saas-apps/sharepoint-on-premises-tutorial)bağlayabilirsiniz. |
| [SharePoint'te sunucudan sunucuya & uygulama kimlik doğrulaması](https://technet.microsoft.com/library/jj219571(v=office.16).aspx) | ACS emekliliğinden etkilenmez; değişiklik gerektirmez. | 
| [SharePoint eklentileri için düşük güven yetkilendirmesi (sağlayıcı barındırılan ve SharePoint barındırılan)](https://docs.microsoft.com/sharepoint/dev/sp-add-ins/three-authorization-systems-for-sharepoint-add-ins) | ACS emekliliğinden etkilenmez; değişiklik gerektirmez. |
| [SharePoint bulut karma arama](https://blogs.msdn.microsoft.com/spses/2015/09/15/cloud-hybrid-search-service-application/) | ACS emekliliğinden etkilenmez; değişiklik gerektirmez. |

### <a name="web-applications-that-use-passive-authentication"></a>Pasif kimlik doğrulaması kullanan web uygulamaları

Kullanıcı kimlik doğrulaması için Access Denetimi'ni kullanan web uygulamaları için Access Denetimi, web uygulama geliştiricileri ve mimarları için aşağıdaki özellikleri ve yetenekleri sağlar:

- Windows Identity Foundation (WIF) ile derin tümleştirme.
- Google, Facebook, Yahoo, Azure Active Directory ve AD FS hesapları ve Microsoft hesapları ile federasyon.
- Aşağıdaki kimlik doğrulama protokolleri için destek: OAuth 2.0 Taslak 13, WS-Trust ve Web Hizmetleri Federasyonu (WS-Federation).
- Aşağıdaki belirteç biçimleri için destek: JSON Web Belirteci (JWT), SAML 1.1, SAML 2.0 ve Simple Web Token (SWT).
- WIF'e entegre edilmiş ve kullanıcıların oturum açmaları için kullandıkları hesap türünü seçmelerine olanak tanıyan bir ev alemi bulma deneyimi. Bu deneyim web uygulaması tarafından barındırılan ve tamamen özelleştirilebilir.
- Erişim Denetimi'nden web uygulaması tarafından alınan taleplerin zengin özelleştirmesine olanak tanıyan belirteç dönüşümü:
    - Kimlik sağlayıcılardan gelen talepleri iletin.
    - Ek özel talepler ekleme.
    - Belirli koşullar altında talepte bulunmanın basit olması mantığı.

Ne yazık ki, tüm bu eşdeğer yetenekleri sunan tek bir hizmet yoktur. Access Denetimi'nin hangi yeteneklerine ihtiyacınız olduğunu değerlendirmeli ve ardından [Azure Active Directory, Azure](https://azure.microsoft.com/develop/identity/signin/)Active [Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) (Azure AD B2C) veya başka bir bulut kimlik doğrulama hizmeti arasında seçim yapmanız gerekir.

#### <a name="migrate-to-azure-active-directory"></a>Azure Etkin Dizine Geçir

Göz önünde bulundurulması gereken yol, uygulamalarınızı ve hizmetlerinizi doğrudan Azure AD ile tümleştirmektir. Azure AD, Microsoft iş veya okul hesapları için bulut tabanlı kimlik sağlayıcısıdır. Azure AD, Office 365, Azure ve çok daha fazlası için kimlik sağlayıcısıdır. Access Denetimi'ne benzer federe kimlik doğrulama özellikleri sağlar, ancak tüm Access Control özelliklerini desteklemez. 

Bunun başlıca örneği, Facebook, Google ve Yahoo gibi sosyal kimlik sağlayıcılarıyla federasyondur. Kullanıcılarınız bu tür kimlik bilgileriyle oturum açtıysa, Azure AD sizin için çözüm değildir. 

Azure AD, Access Denetimi ile aynı kimlik doğrulama protokollerini de desteklemez. Örneğin, hem Access Denetimi hem de Azure AD OAuth'u desteklese de, her uygulama arasında ince farklar vardır. Farklı uygulamalar, geçişin bir parçası olarak kodu değiştirmenizi gerektirir.

Ancak Azure AD, Access Control müşterilerine çeşitli olası avantajlar sağlar. Access Control müşterileri tarafından yaygın olarak kullanılan Microsoft çalışmasını veya bulutta barındırılan okul hesaplarını doğal olarak destekler. 

Azure AD kiracısı, AD FS aracılığıyla şirket içi Active Directory'nin bir veya daha fazla örneğine de aktarılabilir. Bu şekilde, uygulamanız şirket içinde barındırılan bulut tabanlı kullanıcıların ve kullanıcıların kimliğini doğrulayabilir. Ayrıca WS-Federation protokolünü destekler, bu da WIF kullanarak bir web uygulamasıyla tümleştirmeyi nispeten kolaylaştırır.

Aşağıdaki tablo, web uygulamalarıyla alakalı Erişim Denetimi özelliklerini Azure AD'de kullanılabilen özelliklerle karşılaştırmaktadır. 

Yüksek düzeyde, *kullanıcıların yalnızca Microsoft çalışmaları veya okul hesaplarıyla oturum açmalarına izin verirseniz, Azure Etkin Dizin ilersiniz için büyük olasılıkla geçiş inişiniz için en iyi seçimdir.*

| Özellik | Erişim Denetimi desteği | Azure AD desteği |
| ---------- | ----------- | ---------------- |
| **Hesap türleri** | | |
| Microsoft iş veya okul hesapları | Destekleniyor | Destekleniyor |
| Windows Server Active Directory ve AD FS hesapları |- Azure AD kiracısı ile federasyon aracılığıyla desteklenir <br />- AD FS ile doğrudan federasyon aracılığıyla desteklenir | Yalnızca Azure AD kiracısı olan federasyon aracılığıyla desteklenir | 
| Diğer kurumsal kimlik yönetim sistemlerinden hesaplar |- Azure AD kiracısı ile federasyon üzerinden mümkün <br />- Doğrudan federasyon aracılığıyla desteklenir | Azure AD kiracısı ile federasyon aracılığıyla mümkün |
| Kişisel kullanım için Microsoft hesapları | Destekleniyor | Azure AD v2.0 OAuth protokolü ile desteklenir, ancak diğer protokoller üzerinden desteklenmez | 
| Facebook, Google, Yahoo hesapları | Destekleniyor | Hiçbir şekilde desteklenmez |
| **Protokoller ve SDK uyumluluğu** | | |
| WIF | Destekleniyor | Desteklenen, ancak sınırlı talimatlar mevcuttur |
| WS-Federation | Destekleniyor | Destekleniyor |
| OAuth 2.0 | Taslak 13 desteği | En modern spesifikasyon olan RFC 6749 desteği |
| WS-Güven | Destekleniyor | Desteklenmiyor |
| **Belirteç biçimleri** | | |
| Jwt | Desteklenen Beta | Destekleniyor |
| SAML 1.1 | Destekleniyor | Önizleme |
| SAML 2.0 | Destekleniyor | Destekleniyor |
| Swt | Destekleniyor | Desteklenmiyor |
| **Özelleştirmeler** | | |
| Özelleştirilebilir ev alemi bulma/hesap toplama Kullanıcı Birası | Uygulamalara dahil edilebilen indirilebilir kod | Desteklenmiyor |
| Özel belirteç imzalama sertifikaları yükleme | Destekleniyor | Destekleniyor |
| Belirteçlerde talepleri özelleştirme |- Kimlik sağlayıcılardan gelen giriş taleplerini iletin<br />- Talep olarak kimlik sağlayıcıdan jeton erişim alın<br />- Girdi taleplerinin değerlerine göre çıktı taleplerini verme<br />- Sürekli değerlerle çıkış talepleri |- Federe kimlik sağlayıcılarından gelen talepler ile geçemez<br />- Talep olarak kimlik sağlayıcıdan jeton alamıyorum<br />- Girdi taleplerinin değerlerine göre çıktı talebi düzenleyemez<br />- Sürekli değerlerle çıktı talepleri verebilir<br />- Azure AD ile senkronize edilen kullanıcıların özelliklerine dayalı çıktı talepleri verebilir |
| **Otomasyon** | | |
| Yapılandırma ve yönetim görevlerini otomatikleştirin | Erişim Kontrol Yönetim Hizmeti ile desteklenir | Microsoft Graph API kullanılarak desteklenir |

Azure AD'nin uygulamalarınız ve hizmetleriniz için en iyi geçiş yolu olduğuna karar verirseniz, uygulamanızı Azure AD ile tümleştirmenin iki yolunun farkında olmalısınız.

Azure AD ile tümleştirmek için WS-Federation veya WIF'i kullanmak [için, galeri dışı bir uygulama için federated tek oturum açma'yı yapılandırın'da](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery)açıklanan yaklaşımı izlemenizi öneririz. Makale, SAML tabanlı tek oturum açma için Azure AD'yi yapılandırmayı ifade eder, ancak WS-Federation'ı yapılandırmak için de çalışır. Bu yaklaşımı n ardından bir Azure AD Premium lisansı gerektirir. Bu yaklaşımın iki avantajı vardır:

- Azure AD belirteç özelleştirmesinin tüm esnekliğine kavuşun. Azure AD tarafından verilen talepleri, Access Denetimi tarafından verilen taleple eşleşecek şekilde özelleştirebilirsiniz. Bu özellikle kullanıcı kimliği veya Ad Tanımlayıcı iddiasını içerir. Teknolojileri değiştirdikten sonra kullanıcılarınız için tutarlı kullanıcı tanımlayıcıları almaya devam etmek için, Azure AD tarafından verilen kullanıcı kimliklerinin Access Control tarafından verilenlerle eşleştirdiğinden emin olun.
- Uygulamanıza özgü ve denetlediğiniz bir ömür boyu bir belirteç imzalama sertifikası yapılandırabilirsiniz.

> [!NOTE]
> Bu yaklaşım, bir Azure AD Premium lisansı gerektirir. Erişim Denetimi müşterisiyseniz ve bir uygulama için tek oturum açmak için premium lisansa ihtiyacınız varsa, bize ulaşın. Kullanabileceğiniz geliştirici lisansları sağlamakla mutluluk dulacağız.

Alternatif bir yaklaşım ws-Federasyon kurmak için biraz farklı talimatlar verir [bu kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation), takip etmektir. Bu kod örneği WIF kullanmaz, daha çok 4.5 OWIN ara ASP.NET kullanır. Ancak, uygulama kaydı talimatları WIF kullanan uygulamalar için geçerlidir ve Azure AD Premium lisansı gerektirmez. 

Bu yaklaşımı seçerseniz, Azure [AD'de anahtar rollover imzalamayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover)anlamanız gerekir. Bu yaklaşım, belirteçleri vermek için Azure AD genel imzalama anahtarını kullanır. Varsayılan olarak, WIF imzalama anahtarlarını otomatik olarak yenilemez. Azure AD genel imza anahtarlarını döndürdüğünde, WIF uygulamanızın değişiklikleri kabul etmeye hazır olması gerekir. Daha fazla bilgi için Azure [AD'de anahtar rollover imzalama hakkında önemli bilgilere](https://msdn.microsoft.com/library/azure/dn641920.aspx)bakın.

OpenID Connect veya OAuth protokolleri aracılığıyla Azure AD ile tümleştirebiliyorsanız, bunu yapmanızı öneririz. [Azure AD geliştirici kılavuzumuzda](https://aka.ms/aaddev)bulunan web uygulamanıza Azure AD'yi nasıl entegre edebileceğimiz le ilgili kapsamlı belgelere ve rehbere sahibiz.

#### <a name="migrate-to-azure-active-directory-b2c"></a>Azure Etkin Dizin B2C'ye geçirin

Göz önünde bulundurulması gereken diğer geçiş yolu Azure AD B2C'dir. Azure AD B2C, Access Control gibi geliştiricilerin kimlik doğrulama ve kimlik yönetimi mantıklarını bir bulut hizmetine dış kaynak sağlamasına olanak tanıyan bir bulut kimlik doğrulama hizmetidir. Milyonlarca aktif kullanıcıya sahip olabilecek tüketiciye yönelik uygulamalar için tasarlanmış ücretli bir hizmettir (ücretsiz ve premium katmanlara sahip).

Access Control gibi, Azure AD B2C'nin en çekici özelliklerinden biri de birçok farklı hesap türünü desteklemesidir. Azure AD B2C ile, kullanıcıları Microsoft hesaplarını veya Facebook, Google, LinkedIn, GitHub veya Yahoo hesaplarını ve daha fazlasını kullanarak oturum açabilirsiniz. Azure AD B2C, kullanıcıların uygulamanız için özel olarak oluşturduğu "yerel hesapları" veya kullanıcı adını ve parolaları da destekler. Azure AD B2C, oturum açma akışlarınızı özelleştirmek için kullanabileceğiniz zengin genişletilebilirlik de sağlar. 

Ancak Azure AD B2C, Access Control müşterilerinin gerektirebileceği kimlik doğrulama protokollerinin ve belirteç biçimlerinin genişliğini desteklemez. Ayrıca, kimlik sağlayıcısı, Microsoft veya başka bir yerden kullanıcı hakkında ek bilgi almak için jeton almak ve sorgu yapmak için Azure AD B2C'yi kullanamazsınız.

Aşağıdaki tablo, web uygulamalarıyla alakalı Erişim Denetimi özelliklerini Azure AD B2C'de kullanılabilen özelliklerle karşılaştırMaktadır. Yüksek düzeyde, *Azure AD B2C, uygulamanız tüketiciyle karşı karşıyaysa veya birçok farklı hesap türünü destekliyorsa, geçişiniz için büyük olasılıkla doğru seçimdir.*

| Özellik | Erişim Denetimi desteği | Azure AD B2C desteği |
| ---------- | ----------- | ---------------- |
| **Hesap türleri** | | |
| Microsoft iş veya okul hesapları | Destekleniyor | Özel ilkelerle desteklenir  |
| Windows Server Active Directory ve AD FS hesapları | AD FS ile doğrudan federasyon aracılığıyla desteklenir | Özel ilkeler kullanılarak SAML federasyonu aracılığıyla desteklenir |
| Diğer kurumsal kimlik yönetim sistemlerinden hesaplar | WS-Federation aracılığıyla doğrudan federasyon aracılığıyla desteklenir | Özel ilkeler kullanılarak SAML federasyonu aracılığıyla desteklenir |
| Kişisel kullanım için Microsoft hesapları | Destekleniyor | Destekleniyor | 
| Facebook, Google, Yahoo hesapları | Destekleniyor | Facebook ve Google yerel olarak desteklenen, Yahoo özel politikalar kullanarak OpenID Connect federasyonu üzerinden desteklenen |
| **Protokoller ve SDK uyumluluğu** | | |
| Windows Identity Foundation (WIF) | Destekleniyor | Desteklenmiyor |
| WS-Federation | Destekleniyor | Desteklenmiyor |
| OAuth 2.0 | Taslak 13 desteği | En modern spesifikasyon olan RFC 6749 desteği |
| WS-Güven | Destekleniyor | Desteklenmiyor |
| **Belirteç biçimleri** | | |
| Jwt | Desteklenen Beta | Destekleniyor |
| SAML 1.1 | Destekleniyor | Desteklenmiyor |
| SAML 2.0 | Destekleniyor | Desteklenmiyor |
| Swt | Destekleniyor | Desteklenmiyor |
| **Özelleştirmeler** | | |
| Özelleştirilebilir ev alemi bulma/hesap toplama Kullanıcı Birası | Uygulamalara dahil edilebilen indirilebilir kod | Özel CSS ile tamamen özelleştirilebilir kullanıcı bir ae-i ui |
| Özel belirteç imzalama sertifikaları yükleme | Destekleniyor | Özel ilkeler le desteklenen sertifikalar değil, özel imzalama anahtarları |
| Belirteçlerde talepleri özelleştirme |- Kimlik sağlayıcılardan gelen giriş taleplerini iletin<br />- Talep olarak kimlik sağlayıcıdan jeton erişim alın<br />- Girdi taleplerinin değerlerine göre çıktı taleplerini verme<br />- Sürekli değerlerle çıkış talepleri |- Kimlik sağlayıcılardan gelen talepler arasından geçebilir; bazı talepler için gerekli özel ilkeler<br />- Talep olarak kimlik sağlayıcıdan jeton alamıyorum<br />- Özel ilkeler yoluyla giriş taleplerinin değerlerine dayalı çıktı talepleri verebilir<br />- Özel ilkeler aracılığıyla sabit değerlerle çıktı talepleri verebilir |
| **Otomasyon** | | |
| Yapılandırma ve yönetim görevlerini otomatikleştirin | Erişim Kontrol Yönetim Hizmeti ile desteklenir |- Microsoft Graph API kullanılarak izin verilen kullanıcıların oluşturulması<br />- B2C kiracıları, uygulamaları veya politikaları programlı olarak oluşturamaz |

Azure AD B2C'nin uygulamalarınız ve hizmetleriniz için en iyi geçiş yolu olduğuna karar verirseniz, aşağıdaki kaynaklarla başlayın:

- [Azure AD B2C belgeleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2C özel ilkeleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Azure AD B2C fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Ping Kimliğine veya Auth0'a geçirin

Bazı durumlarda, Azure AD ve Azure AD B2C'nin web uygulamalarınızda önemli kod değişiklikleri yapmadan Access Control'ü değiştirmek için yeterli olmadığını görebilirsiniz. Bazı yaygın örnekler şunları içerebilir:

- WIF veya WS-Federation'ı Google veya Facebook gibi sosyal kimlik sağlayıcılarıyla oturum açma için kullanan web uygulamaları.
- WS-Federation protokolü üzerinden kurumsal kimlik sağlayıcısına doğrudan federasyon gerçekleştiren web uygulamaları.
- Access Control tarafından verilen belirteçlerde talep olarak bir sosyal kimlik sağlayıcısı (Google veya Facebook gibi) tarafından verilen erişim belirteci gerektiren web uygulamaları.
- Azure AD veya Azure AD B2C'nin oluşturamayacağı karmaşık belirteç dönüştürme kurallarına sahip web uygulamaları.
- Birçok farklı kimlik sağlayıcısına federasyonu merkezi olarak yönetmek için ACS kullanan çok kiracılı web uygulamaları

Bu gibi durumlarda, web uygulamanızı başka bir bulut kimlik doğrulama hizmetine geçirmeyi düşünebilirsiniz. Aşağıdaki seçenekleri keşfetmenizi öneririz. Aşağıdaki seçeneklerin her biri Erişim Denetimi'ne benzer özellikler sunar:

|     |     |
| --- | --- |
| ![Bu resim Auth0 logosunu gösterir](./media/active-directory-acs-migration/rsz-auth0.png) | [Auth0,](https://auth0.com/acs) [Access Control müşterileri için üst düzey geçiş kılavuzu](https://auth0.com/acs)oluşturan ve ACS'nin yaptığı hemen hemen her özelliği destekleyen esnek bir bulut kimliği hizmetidir. |
| ![Bu resim Ping Identity logosunu gösterir](./media/active-directory-acs-migration/rsz-ping.png) | [Ping Identity,](https://www.pingidentity.com) ACS'ye benzer iki çözüm sunar. PingOne, ACS ile aynı özelliklerin çoğunu destekleyen bir bulut kimlik hizmetidir ve PingFederate daha fazla esneklik sunan benzer bir şirket içi kimlik ürünüdür. Bu ürünleri kullanma hakkında daha fazla bilgi için [Ping'in ACS emeklilik kılavuzuna](https://www.pingidentity.com/en/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) bakın. |

Ping Identity ve Auth0 ile çalışmaamacımız, tüm Access Control müşterilerinin uygulamaları ve hizmetleri için Access Control'den taşınmak için gereken iş miktarını en aza indiren bir geçiş yolu olmasını sağlamaktır.

<!--

## SharePoint 2010, 2013, 2016

TODO: Azure AD only, use Azure AD SAML 1.1 tokens, when we bring it back online.
Other IDPs: use Auth0? https://auth0.com/docs/integrations/sharepoint.

-->

### <a name="web-services-that-use-active-authentication"></a>Etkin kimlik doğrulaması kullanan web hizmetleri

Access Control tarafından verilen belirteçlerle güvenli web hizmetleri için Access Control aşağıdaki özellikleri ve yetenekleri sunar:

- Access Denetimi ad alanınızda bir veya daha fazla *hizmet kimliği* kaydettirebilme. Hizmet kimlikleri belirteçleri istemek için kullanılabilir.
- Aşağıdaki kimlik bilgilerini kullanarak belirteçleri istemek için OAuth WRAP ve OAuth 2.0 Taslak 13 protokolleri için destek:
    - Hizmet kimliği için oluşturulan basit bir parola
    - Simetrik anahtar veya X509 sertifikası kullanarak imzalı bir SWT
    - Güvenilir bir kimlik sağlayıcısı tarafından verilen bir SAML belirteci (genellikle bir AD FS örneği)
- Aşağıdaki belirteç biçimleri için destek: JWT, SAML 1.1, SAML 2.0 ve SWT.
- Basit belirteç dönüşüm kuralları.

Access Denetimi'ndeki hizmet kimlikleri genellikle sunucudan sunucuya kimlik doğrulamasını uygulamak için kullanılır. 

#### <a name="migrate-to-azure-active-directory"></a>Azure Etkin Dizine Geçir

Bu tür bir kimlik doğrulama akışı için önerimiz [Azure Etkin Dizini'ne](https://azure.microsoft.com/develop/identity/signin/)geçiş yapmaktır. Azure AD, Microsoft iş veya okul hesapları için bulut tabanlı kimlik sağlayıcısıdır. Azure AD, Office 365, Azure ve çok daha fazlası için kimlik sağlayıcısıdır. 

OAuth istemci kimlik bilgileri hibesinin Azure AD uygulamasını kullanarak sunucudan sunucuya kimlik doğrulaması için Azure AD'yi de kullanabilirsiniz. Aşağıdaki tablo, sunucudan sunucuya kimlik doğrulamasındaki Access Control'ün yeteneklerini Azure AD'de kullanılabilenlerle karşılaştırMaktadır.

| Özellik | Erişim Denetimi desteği | Azure AD desteği |
| ---------- | ----------- | ---------------- |
| Web hizmeti nasıl kaydedilir? | Access Denetimi yönetim portalında güvenen bir taraf oluşturma | Azure portalında Azure AD web uygulaması oluşturma |
| İstemci nasıl kaydedilir? | Access Control yönetim portalında hizmet kimliği oluşturma | Azure portalında başka bir Azure AD web uygulaması oluşturma |
| Kullanılan protokol |- OAuth WRAP protokolü<br />- OAuth 2.0 Taslak 13 müşteri kimlik bilgileri hibe | OAuth 2.0 istemci kimlik bilgileri verme |
| İstemci kimlik doğrulama yöntemleri |- Basit şifre<br />- İmzalı SWT<br />- Federe kimlik sağlayıcısından SAML belirteci |- Basit şifre<br />- Imzalı JWT |
| Belirteç biçimleri |- JWT<br />- SAML 1.1<br />- SAML 2.0<br />- SWT<br /> | Yalnızca JWT |
| Belirteç dönüşümü |- Özel talepler ekleme<br />- Basit if-sonra talep verme mantığı | Özel talepler ekleme | 
| Yapılandırma ve yönetim görevlerini otomatikleştirin | Erişim Kontrol Yönetim Hizmeti ile desteklenir | Microsoft Graph API kullanılarak desteklenir |

Sunucudan sunucuya senaryoları uygulama yla ilgili kılavuz için aşağıdaki kaynaklara bakın:

- [Azure AD geliştirici kılavuzunun](https://aka.ms/aaddev) Hizmete Hizmet bölümü
- [Basit şifre istemci kimlik bilgilerini kullanarak Daemon kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon)
- [Sertifika istemci kimlik bilgilerini kullanarak Daemon kod örneği](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

#### <a name="migrate-to-ping-identity-or-auth0"></a>Ping Kimliğine veya Auth0'a geçirin

Bazı durumlarda, Azure AD istemci kimlik bilgileri ve OAuth hibe uygulamasının mimarinizde önemli kod değişiklikleri olmadan Access Denetimi'ni değiştirmek için yeterli olmadığını görebilirsiniz. Bazı yaygın örnekler şunları içerebilir:

- JWT'ler dışındaki belirteç biçimlerini kullanarak sunucudan sunucuya kimlik doğrulaması.
- Harici bir kimlik sağlayıcısı tarafından sağlanan bir giriş belirteci kullanarak sunucudan sunucuya kimlik doğrulaması.
- Azure AD'nin oluşturamayacağı belirteç dönüştürme kurallarıyla sunucudan sunucuya kimlik doğrulaması.

Bu gibi durumlarda, web uygulamanızı başka bir bulut kimlik doğrulama hizmetine geçirmeyi düşünebilirsiniz. Aşağıdaki seçenekleri keşfetmenizi öneririz. Aşağıdaki seçeneklerin her biri Erişim Denetimi'ne benzer özellikler sunar:

|     |     |
| --- | --- |
| ![Bu resim Auth0 logosunu gösterir](./media/active-directory-acs-migration/rsz-auth0.png) | [Auth0,](https://auth0.com/acs) [Access Control müşterileri için üst düzey geçiş kılavuzu](https://auth0.com/acs)oluşturan ve ACS'nin yaptığı hemen hemen her özelliği destekleyen esnek bir bulut kimliği hizmetidir. |
| ![Bu resim Ping Identity logosunu gösterir](./media/active-directory-acs-migration/rsz-ping.png) | [Ping Identity,](https://www.pingidentity.com) ACS'ye benzer iki çözüm sunar. PingOne, ACS ile aynı özelliklerin çoğunu destekleyen bir bulut kimlik hizmetidir ve PingFederate daha fazla esneklik sunan benzer bir şirket içi kimlik ürünüdür. Bu ürünleri kullanma hakkında daha fazla bilgi için [Ping'in ACS emeklilik kılavuzuna](https://www.pingidentity.com/en/company/blog/2017/11/20/migrating_from_microsoft_acs_to_ping_identity.html) bakın. |

Ping Identity ve Auth0 ile çalışmaamacımız, tüm Access Control müşterilerinin uygulamaları ve hizmetleri için Access Control'den taşınmak için gereken iş miktarını en aza indiren bir geçiş yolu olmasını sağlamaktır.

#### <a name="passthrough-authentication"></a>Geçiş kimlik doğrulaması

Rasgele belirteç dönüştürme ile geçiş kimlik doğrulaması için ACS eşdeğer microsoft teknolojisi yoktur. Müşterilerinizin ihtiyacı olan buysa, Auth0 en yakın yaklaşım çözümlerini sağlayan kişi olabilir.

## <a name="questions-concerns-and-feedback"></a>Sorular, endişeler ve geri bildirim

Birçok Access Control müşterisinin bu makaleyi okuduktan sonra net bir geçiş yolu bulamayacağını biliyoruz. Doğru planı belirlemede yardıma veya rehberliğe ihtiyacınız olabilir. Geçiş senaryolarınızı ve sorularınızı tartışmak isterseniz, lütfen bu sayfada bir yorum bırakın.
