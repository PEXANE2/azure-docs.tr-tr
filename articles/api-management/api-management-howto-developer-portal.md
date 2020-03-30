---
title: Azure API Yönetimi geliştirici portalına genel bakış
titleSuffix: Azure API Management
description: API Yönetimi'ndeki geliştirici portalı hakkında bilgi edinin.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/15/2020
ms.author: apimpm
ms.openlocfilehash: fefa5ff5d112b479110d484ee0ea4c358b5c88a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335906"
---
# <a name="azure-api-management-developer-portal-overview"></a>Azure API Yönetimi geliştirici portalına genel bakış

Geliştirici portalı, API'lerinizin belgeleriyle otomatik olarak oluşturulmuş, tamamen özelleştirilebilir bir web sitesidir. API tüketicilerinin API'lerinizi keşfedebileceği, bunları nasıl kullanacağınızı öğrenebileceği, erişim isteğinde bulunabileceği ve bunları deneyebileceği yerdir.

Bu makalede, API Yönetimi'nde geliştirici portalının kendi kendine barındırılan ve yönetilen sürümleri arasındaki farklar açıklanmaktadır. Ayrıca mimarisini açıklar ve sık sorulan sorulara cevaplar sağlar.

![API Yönetimi geliştirici portalı](media/api-management-howto-developer-portal/cover.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="managed-and-self-hosted-versions"></a><a name="managed-vs-self-hosted"></a>Yönetilen ve kendi kendine barındırılan sürümler

Geliştirici portalınızı iki şekilde oluşturabilirsiniz:

- **Yönetilen sürüm** - düzenleme ve API Yönetimi örneğiniçine yerleşik ve URL `<your-api-management-instance-name>.developer.azure-api.net`üzerinden erişilebilir portal, özelleştirerek . Yönetilen portala nasıl erişip özelleştirilenleri öğrenmek için [bu dokümantasyon makalesine](api-management-howto-developer-portal-customize.md) bakın.
- **Kendi kendine barındırılan sürüm** - bir API Yönetimi örneğinin dışında portalınızı dağıtarak ve kendi kendine barındırarak. Bu yaklaşım, portalın kod tabanını yeniden gerçekleştirmenize ve sağlanan temel işlevselliği genişletmenize olanak tanır - örneğin, üçüncü taraf sistemlerle tümleştirmeler için özel widget'lar uygulayın. Bu senaryoda, portalın bakıcısısınız ve portalı en son sürüme yükseltmekten siz sorumlusunuz. Ayrıntılar ve talimatlar için, portalın kaynak kodu ve [bir widget uygulanması konusunda öğretici][3]ile [GitHub deposuna][1] bakın. [Yönetilen sürümü için öğretici](api-management-howto-developer-portal-customize.md) yönetilen ve kendi kendine barındırılan sürümleri için ortak portalYönetim paneli, yürür.

## <a name="portal-architectural-concepts"></a>Portal mimari kavramlar

Portal bileşenleri mantıksal olarak iki kategoriye ayrılabilir: *kod* ve *içerik.*

*Kod* [GitHub deposunda][1] tutulur ve şunları içerir:

- Widget'lar - görsel öğeleri temsil eden ve HTML, JavaScript, şekillendirme yeteneği, ayarları ve içerik eşleme birleştiren. Örnekler bir resim, bir metin paragrafı, bir form, API listesi vb.
- Stil tanımları - widget'ların nasıl şekillendirilebileceğini belirten
- Motor - portal içeriğinden statik web sayfaları oluşturur ve JavaScript yazılır
- Görsel editör - tarayıcı içi özelleştirme ve yazma deneyimi sağlar

*İçerik* iki alt kategoriye ayrılır: *portal içeriği* ve *API Yönetimi içeriği.*

*Portal içeriği* portala özgüdür ve şunları içerir:

- Sayfalar - örneğin, açılış sayfası, API eğitimleri, blog gönderileri
- Medya - resimler, animasyonlar ve diğer dosya tabanlı içerik
- Bir URL ile eşleşen ve sayfaların nasıl görüntüleneceğini tanımlayan düzenler - şablonlar
- Stiller - stil tanımları için değerler, örneğin yazı tipleri, renkler, kenarlıklar
- Ayarlar - yapılandırma, örneğin favicon, web sitesi meta verileri

*Portal içeriği*, medya dışında, JSON belgeleri olarak ifade edilir.

*API Yönetimi içeriği* API'ler, Operasyonlar, Ürünler, Abonelikler gibi varlıkları içerir.

Portal [Paperbits çerçevesinin](https://paperbits.io/)uyarlanmış çatal dayanmaktadır. Özgün Paperbits işlevi, API Yönetimine özgü widget'lar (örneğin, API listesi, Ürünlerin listesi) ve içeriği kaydetmek ve almak için API Yönetimi hizmetine bir bağlayıcı sağlamak için genişletilmiştir.

## <a name="frequently-asked-questions"></a><a name="faq"></a>Sık sorulan sorular

Bu bölümde, genel nitelikte olan geliştirici portalı ile ilgili sık sorulan soruları yanıtlıyoruz. Kendi kendine barındırılan sürüme özgü sorular için [GitHub deposunun wiki bölümüne](https://github.com/Azure/api-management-developer-portal/wiki)bakın.

### <a name="how-can-i-migrate-from-the-preview-version-of-the-portal"></a><a id="preview-to-ga"/>Portalın önizleme sürümünden nasıl geçiş yapabilirim?

Geliştirici portalının önizleme sürümünü kullanarak, API Yönetimi hizmetinizde önizleme içeriğini sağmış sınız. Varsayılan içerik, daha iyi bir kullanıcı deneyimi için genel olarak kullanılabilen sürümde önemli ölçüde değiştirilmiştir. Ayrıca yeni widget'lar içerir.

Yönetilen sürümü kullanıyorsanız, **İşlemler** menüsü bölümündeki İçeriği Sıfırla'yı tıklatarak portalın **içeriğini** sıfırla. Bu işlemi onaylamak portalın tüm içeriğini kaldırır ve yeni varsayılan içeriği sağlar. Portalın motoru API Yönetimi hizmetinizde otomatik olarak güncelleştirildi.

![Portal içeriğini sıfırlama](media/api-management-howto-developer-portal/reset-content.png)

Kendi kendine barındırılan sürümü kullanıyorsanız, `scripts/cleanup.bat` `scripts/generate.bat` varolan içeriği kaldırmak ve yeni içerik sağlamak için GitHub deposundan ve Portalınızın kodunu GitHub deposundan en son sürüme önceden yükselttiğinden emin olun.

Portalın içeriğini sıfırlamak istemiyorsanız, sayfalarınızın her birinde yeni kullanılabilir widget'lar kullanmayı düşünebilirsiniz. Varolan widget'lar otomatik olarak en son sürümlere güncelleştirildi.

Portalınız genel kullanılabilirlik duyurusundan sonra sağlanmışsa, yeni varsayılan içeriğe zaten sahip olmalıdır. Sizin tarafınızdan herhangi bir işlem yapılması gerekmez.

### <a name="how-can-i-migrate-from-the-old-developer-portal-to-the-developer-portal"></a>Eski geliştirici portalından geliştirici portalına nasıl geçiş yapabilirim?

Portallar uyumsuzdur ve içeriği el ile geçirmeniz gerekir.

### <a name="does-the-portal-have-all-the-features-of-the-old-portal"></a>Portal eski portalın tüm özelliklerine sahip mi?

Geliştirici portalı artık *Uygulamaları* ve *Sorunları*desteklemiyor.

Etkileşimli geliştirici konsolunda OAuth ile kimlik doğrulama henüz desteklenmedi. [GitHub sorunu](https://github.com/Azure/api-management-developer-portal/issues/208)üzerinden ilerlemeyi izleyebilirsiniz.

### <a name="has-the-old-portal-been-deprecated"></a>Eski geçit küçümsülde mi?

Eski geliştirici ve yayıncı portalları artık *eski* özelliklerdir - yalnızca güvenlik güncelleştirmeleri alacaklardır. Yeni özellikler yalnızca yeni geliştirici portalında uygulanacaktır.

Eski portalların amortismanı ayrıca ilan edilecektir. Sorularınız, endişeleriniz veya yorumlarınız varsa, bunları [özel bir GitHub sorununda](https://github.com/Azure/api-management-developer-portal/issues/121)gündeme getirin.

### <a name="functionality-i-need-isnt-supported-in-the-portal"></a>İhtiyacım olan işlevler portalda desteklenmiyor

Bir özellik [isteği](https://aka.ms/apimwish) açabilir veya [eksik işlevselliği kendiniz uygulayabilirsiniz.][3] İşlevselliği kendiniz uygularsanız, geliştirici portalının kendi kendine ana tarafından barındırılabilir veya yönetilen sürüme değişiklikleri eklemek için GitHub'da bir çekme isteği açabilirsiniz.

### <a name="how-can-i-automate-portal-deployments"></a>Portal dağıtımlarını nasıl otomatikleştirebilirim?

Yönetilen veya kendi kendine barındırılan bir sürümü kullanıyorsanız, geliştirici portalının içeriğine REST API aracılığıyla programlı bir şekilde erişebilir ve yönetebilirsiniz.

API [GitHub deposuwiki bölümünde][2]belgelenmiştir. Örneğin, bir test ortamından üretim ortamına kadar portal içeriğinin ortamlar arasındaki geçişlerini otomatikleştirmek için kullanılabilir. [GitHub'daki bu dokümantasyon makalesinde](https://aka.ms/apimdocs/migrateportal) bu işlem hakkında daha fazla bilgi edinebilirsiniz.

### <a name="does-the-portal-support-azure-resource-manager-templates-andor-is-it-compatible-with-api-management-devops-resource-kit"></a>Portal Azure Kaynak Yöneticisi şablonlarını destekliyor mu ve/veya API Yönetimi DevOps Kaynak Kiti ile uyumlu mu?

Hayır.

### <a name="do-i-need-to-enable-additional-vnet-connectivity-for-the-managed-portal-dependencies"></a>Yönetilen portal bağımlılıkları için ek VNet bağlantısı etkinleştirmem gerekiyor mu?

Çoğu durumda - hayır.

API Yönetimi hizmetiniz dahili bir VNet'teyse, geliştirici portalınıza yalnızca ağ içinden erişilebilir. Yönetim bitiş noktasının ana bilgisayar adı, portalın yönetim arabirimine erişmek için kullandığınız makineden hizmetin iç VIP'sine çözümlenmelidir. Yönetim bitiş noktasının DNS'de kayıtlı olduğundan emin olun. Yanlış yapılandırma durumunda, bir hata görürsünüz: `Unable to start the portal. See if settings are specified correctly in the configuration (...)`.

API Yönetimi hizmetiniz dahili bir VNet'teyse ve internetten Uygulama Ağ Geçidi aracılığıyla erişiyorsanız, geliştirici portalına ve API Yönetimi'nin yönetim bitiş noktalarına bağlantı sağladığınızdan emin olun.

### <a name="i-have-assigned-a-custom-api-management-domain-and-the-published-portal-doesnt-work"></a>Özel bir API Yönetimi etki alanı atadim ve yayınlanan portal çalışmıyor

Etki alanını güncelledikten sonra, değişikliklerin etkili olması için [portalı yeniden yayımlamanız](api-management-howto-developer-portal-customize.md#publish) gerekir.

### <a name="i-have-added-an-identity-provider-and-i-cant-see-it-in-the-portal"></a>Bir kimlik sağlayıcısı ekledim ve portalda göremiyorum

Bir kimlik sağlayıcısını (örneğin, AAD, AAD B2C) yapılandırdıktan sonra, değişikliklerin etkili olması için [portalı yeniden yayımlamanız](api-management-howto-developer-portal-customize.md#publish) gerekir.

### <a name="i-have-set-up-delegation-and-the-portal-doesnt-use-it"></a>Heyet kurdum ve geçit bunu kullanmıyor.

Temsilciliği ayarladıktan sonra, değişikliklerin etkili olması için [portalı yeniden yayımlamanız](api-management-howto-developer-portal-customize.md#publish) gerekir.

### <a name="my-other-api-management-configuration-changes-havent-been-propagated-in-the-developer-portal"></a>Diğer API Yönetimi yapılandırma değişikliklerim geliştirici portalında yayılmadı

Yapılandırma değişikliklerinin çoğu (örneğin, VNet, oturum açma ve ürün terimleri) [portalı yeniden yayımlamayı](api-management-howto-developer-portal-customize.md#publish)gerektirir.

### <a name="im-getting-a-cors-error-when-using-the-interactive-console"></a><a name="cors"></a>Etkileşimli konsolu kullanırken CORS hatası alıyorum

Etkileşimli konsol, istemci tarafındaki BIR API isteğinde bulunmaz. API(ler)'ınıza [bir CORS ilkesi](api-management-cross-domain-policies.md#CORS) ekleyerek CORS sorununu çözün.

Azure portalındaki API Yönetimi hizmetinportal **genel bakış** bölümünde CORS ilkesinin durumunu kontrol edebilirsiniz. Uyarı kutusu, eksik veya yanlış yapılandırılmış bir ilkeyi gösterir.

![API Yönetimi geliştirici portalı](media/api-management-howto-developer-portal/cors-azure-portal.png)

CORS'u **etkinleştir** düğmesine tıklayarak CORS ilkesini otomatik olarak uygulayın.

CORS'u el ile de etkinleştirebilirsiniz.

1. Oluşturulan ilke kodunu görmek için **genel düzey bağlantısında El Ile uygulay'a** tıklayın.
2. Azure portalındaki API Yönetimi hizmetinizin API bölümündeki **Tüm API'lere** gidin. **APIs**
3. **Gelen** işleme **</>** bölümündeki simgeye tıklayın.
4. XML dosyasının **<inbound>** bölümüne ilke ekleyin. Değerin **<origin>** geliştirici portalınızın etki alanıyla eşleştiğinden emin olun.

> [!NOTE]
> 
> API(lar) kapsamı yerine Ürün kapsamında CORS ilkesini uygularsanız ve API'niz üstbilgi aracılığıyla abonelik anahtarı kimlik doğrulamasını kullanırsa konsolunuz çalışmaz.
>
> Tarayıcı otomatik olarak abonelik anahtarı ile bir üstbilgi içermeyen bir OPTIONS HTTP isteği yayınlar. Eksik abonelik anahtarı nedeniyle, API Yönetimi OPTIONS çağrısını bir Ürünle ilişkilendiremez, bu nedenle CORS ilkesini uygulayamaz.
>
> Geçici çözüm olarak, abonelik anahtarını sorgu parametresinde geçirebilirsiniz.

### <a name="what-permissions-do-i-need-to-edit-the-developer-portal"></a>Geliştirici portalını düzenlemek için hangi izinlere ihtiyacım var?

Portalı yönetim modunda `Oops. Something went wrong. Please try again later.` açtığınızda hata görüyorsanız, gerekli izinlerden (RBAC) yoksun olabilirsiniz.

Eski portallar, kullanıcı `Microsoft.ApiManagement/service/getssotoken/action` yöneticisinin portallara erişmesine izin vermek için hizmet kapsamında ()`/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>`izin aldı. Yeni portal kapsamında `Microsoft.ApiManagement/service/users/token/action` `/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>/users/1`izin gerektirir.

Gerekli izinle bir rol oluşturmak için aşağıdaki PowerShell komut dosyasını kullanabilirsiniz. Parametreyi `<subscription-id>` değiştirmeyi unutmayın. 

```PowerShell
#New Portals Admin Role 
Import-Module Az 
Connect-AzAccount 
$contributorRole = Get-AzRoleDefinition "API Management Service Contributor" 
$customRole = $contributorRole 
$customRole.Id = $null
$customRole.Name = "APIM New Portal Admin" 
$customRole.Description = "This role gives the user ability to log in to the new Developer portal as administrator" 
$customRole.Actions = "Microsoft.ApiManagement/service/users/token/action" 
$customRole.IsCustom = $true 
$customRole.AssignableScopes.Clear() 
$customRole.AssignableScopes.Add('/subscriptions/<subscription-id>') 
New-AzRoleDefinition -Role $customRole 
```
 
Rol oluşturulduktan sonra, Azure portalındaki **Access Denetimi (IAM)** bölümünden herhangi bir kullanıcıya verilebilir. Bu rolü bir kullanıcıya atamak, hizmet kapsamında izin atar. Kullanıcı, hizmetteki *herhangi bir* kullanıcı adına SAS belirteçleri oluşturabilecektir. En azından, bu rolün hizmet yöneticisine atanması gerekir. Aşağıdaki PowerShell komutu, kullanıcıya gereksiz izinler `user1` vermemek için rolüen düşük kapsamdaki bir kullanıcıya nasıl atanın gösteriş yapar: 

```PowerShell
New-AzRoleAssignment -SignInName "user1@contoso.com" -RoleDefinitionName "APIM New Portal Admin" -Scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ApiManagement/service/<apim-service-name>/users/1" 
```

İzinler bir kullanıcıya verildikten sonra, yeni izinlerin etkili olabilmesi için kullanıcının oturum açmalı ve Azure portalında yeniden oturum açmanız gerekir.

### <a name="im-seeing-the-unable-to-start-the-portal-see-if-settings-are-specified-correctly--error"></a>Hatayı görüyorum. `Unable to start the portal. See if settings are specified correctly (...)`

Bu hata, bir `GET` çağrı `https://<management-endpoint-hostname>/subscriptions/xxx/resourceGroups/xxx/providers/Microsoft.ApiManagement/service/xxx/contentTypes/document/contentItems/configuration?api-version=2018-06-01-preview` başarısız olduğunda gösterilir. Arama, portalın yönetim arabirimi tarafından tarayıcıdan verilir.

API Yönetimi hizmetiniz bir VNet'teyse , yukarıdaki VNet bağlantı sorusuna bakın.

Arama hatası, özel bir etki alanına atanan ve tarayıcı tarafından güvenilmeyen bir TLS/SSL sertifikasından da kaynaklanabilir. Azaltma olarak, yönetim bitiş noktası özel etki alanını kaldırabilirsiniz - API Yönetimi güvenilir bir sertifikayla varsayılan bitiş noktasına geri döner.

### <a name="whats-the-browser-support-for-the-portal"></a>Portal için tarayıcı desteği nedir?

| Tarayıcı                     | Destekleniyor       |
|-----------------------------|-----------------|
| Apple Safari                | Evet<sup>1</sup> |
| Google Chrome               | Evet<sup>1</sup> |
| Microsoft Edge              | Evet<sup>1</sup> |
| Microsoft Internet Explorer | Hayır              |
| Mozilla Firefox             | Evet<sup>1</sup> |

 <small><sup>1</sup> En son iki üretim sürümünde desteklenir.</small>

## <a name="next-steps"></a>Sonraki adımlar

Yeni geliştirici portalı hakkında daha fazla bilgi edinin:

- [Yönetilen geliştirici portalına erişin ve özelleştirin](api-management-howto-developer-portal-customize.md)
- [Portalın kendi kendine barındırılan sürümünü ayarlama][2]
- [Kendi widget'ınızı uygulayın][3]

Diğer kaynaklara göz atın:

- [Kaynak kodu ile GitHub deposu][1]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://aka.ms/apimdevportal/extend