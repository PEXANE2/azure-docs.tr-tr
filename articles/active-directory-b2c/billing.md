---
title: Azure Active Directory B2C için faturalandırma modeli
description: Azure AD B2C's aylık etkin kullanıcıları (MAU) Faturalandırma modeli, Azure AD B2C kiracıyı bir Azure aboneliğine bağlama ve uygun Premium katman fiyatlandırmasını seçme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 09/01/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 416e2c767b5afd40fea38e6f75fcd3f01440b49a
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89255370"
---
# <a name="billing-model-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için faturalandırma modeli

Azure Active Directory B2C (Azure AD B2C) fiyatlandırması, aylık etkin kullanıcıları (MAU) temel alır. Bu, bir takvim ayı içinde kimlik doğrulama etkinliği olan benzersiz kullanıcıların sayısıdır. Bu Faturalandırma modeli hem Azure AD B2C kiracılar hem de [Azure AD Konuk Kullanıcı işbirliği (B2B)](https://docs.microsoft.com/azure/active-directory/external-identities/external-identities-pricing)için geçerlidir. MAU faturalandırma, ücretsiz bir katman ve esnek ve öngörülebilir bir fiyatlandırma sunarak maliyetleri düşürmenize yardımcı olur. Bu makalede, MAU faturalandırma hakkında bilgi edinin, Azure AD B2C Kiracılarınızı bir aboneliğe bağlama ve fiyatlandırma katmanınızı değiştirme.

> [!IMPORTANT]
> Bu makale fiyatlandırma ayrıntıları içermez. Kullanım faturalaması ve fiyatlandırmayla ilgili en son bilgiler için bkz. [Azure Active Directory B2C fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="what-do-i-need-to-do"></a>Neler yapmam gerekir?

MAU faturalandırmaya faydalanmak için Azure AD B2C kiracınızın bir Azure aboneliğine bağlanması gerekir. Ayrıca, risk tabanlı koşullu erişim gibi Azure AD B2C Premium P2 özelliklerini kullanmak istiyorsanız Azure AD B2C kiracınızı başka bir fiyatlandırma katmanına geçmeniz gerekebilir.

|Kiracınız:  |Şunları yapmanız gerekir:  |
|---------|---------|
| Bir Azure AD B2C kiracısı, MAU temelinde zaten faturalandırılır     | Hiçbir şey yapmayın. Kullanıcılar Azure AD B2C kiracınızda kimlik doğrulaması yaparken, MAU tabanlı faturalandırma modeli kullanılarak otomatik olarak faturalandırılırsınız.        |
| Bir Azure AD B2C kiracı henüz bir aboneliğe bağlanmadı     |  MAU faturalandırmayı etkinleştirmek için [Azure AD B2C kiracınızı bir aboneliğe bağlayın](#link-an-azure-ad-b2c-tenant-to-a-subscription) .     |
| 1 Kasım 2019 ' den önceki bir aboneliğe bağlı Azure AD B2C kiracısı    | [MAU faturalandırmaya geçiş yapın (önerilir)](#switch-to-mau-billing-pre-november-2019-azure-ad-b2c-tenants)veya kimlik doğrulama başına faturalandırma modelinde kalın.     |
| Azure AD B2C kiracısı ve Premium özelliklerini kullanmak istiyorsanız (risk tabanlı koşullu erişim gibi)    | Kullanmak istediğiniz özellikleri destekleyen [bir Azure AD fiyatlandırma katmanına](#change-your-azure-ad-pricing-tier) geçin.        |
|  |  |

## <a name="about-the-monthly-active-users-mau-billing-model"></a>Aylık etkin kullanıcılar (MAU) Faturalandırma modeli hakkında

MAU faturalandırması **1 kasım 2019**' de Azure AD B2C kiracılar için devreye girer. Oluşturduğunuz ve bu tarihte veya sonrasında bir aboneliğe bağladığınız tüm Azure AD B2C kiracılar, MAU başına faturalandırılır. Bir aboneliğe bağlı olmayan bir Azure AD B2C kiracınız varsa, bunu şimdi yapmanız gerekir. 1 Kasım 2019 tarihinden önce bir aboneliğe bağlanmış mevcut bir Azure AD B2C kiracınız varsa, aylık etkin kullanıcılar (MAU) Faturalandırma modeline yükseltmenizi veya kimlik doğrulama başına faturalandırma modelinde kalabilmeniz önerilir.
  
Azure AD B2C kiracınız, kullanmak istediğiniz özelliklere göre uygun Azure fiyatlandırma katmanına de bağlanmalıdır. Premium özellikler Azure AD B2C [Premium P1 veya P2 fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/)gerektirir. Yeni özellikleri kullanırken fiyatlandırma katmanınızı yükseltmeniz gerekebilir. Örneğin, koşullu erişim, kiracınız için Azure AD B2C Premium P2 fiyatlandırma katmanını seçmeniz gerekir.

## <a name="link-an-azure-ad-b2c-tenant-to-a-subscription"></a>Azure AD B2C kiracıyı aboneliğe bağlama

Azure Active Directory B2C (Azure AD B2C) için kullanım ücretleri bir Azure aboneliğine faturalandırılır. Hedef Azure aboneliği içinde bir Azure AD B2C *kaynağı* oluşturarak bir Azure AD B2C kiracıyı açık bir şekilde Azure aboneliğine bağlamanız gerekir. Birçok Azure AD B2C kaynak, sanal makineler, depolama hesapları ve Logic Apps gibi diğer Azure kaynaklarıyla birlikte tek bir Azure aboneliğinde oluşturulabilir. Aboneliğin ilişkilendirildiği Azure Active Directory (Azure AD) kiracısına giderek bir abonelik içindeki kaynakların tümünü görebilirsiniz.

Azure AD B2C kiracıya bağlı bir abonelik, ek Azure AD B2C kaynakları da dahil olmak üzere Azure AD B2C kullanım veya diğer Azure kaynaklarının faturalandırılması için kullanılabilir. Azure AD B2C kiracının içindeki diğer Azure lisans tabanlı Hizmetleri veya Office 365 lisanslarını eklemek için kullanılamaz.

### <a name="prerequisites"></a>Ön koşullar

* [Azure aboneliği](https://azure.microsoft.com/free/)
* Aboneliğe bağlamak istediğiniz [Azure AD B2C kiracı](tutorial-create-tenant.md)
  * Kiracı Yöneticisi olmanız gerekir
  * Kiracının zaten bir aboneliğe bağlı olmaması gerekir

### <a name="create-the-link"></a>Bağlantıyı oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından kullanmak istediğiniz Azure aboneliğini içeren dizini seçin (Azure AD B2C kiracıyı içeren dizin*değil* ).
3. **Kaynak oluştur**' u seçin, `Active Directory B2C` Market alanını **Ara** alanına girin ve ardından **Azure Active Directory B2C**' yi seçin.
4. **Oluştur**’u seçin.
5. **Mevcut bir Azure AD B2C kiracıyı Azure aboneliğime bağla**' yı seçin.
6. Açılan listeden bir **Azure AD B2C kiracı** seçin. Yalnızca genel yöneticisi olduğunuz ve bir aboneliğe henüz bağlanmamış olan kiracılar gösterilir. **Azure AD B2C kaynak adı** alanı seçtiğiniz Azure AD B2C kiracının etki alanı adıyla doldurulur.
7. Yönetici olduğunuz etkin bir Azure **aboneliğini** seçin.
8. **Kaynak grubu**altında **Yeni oluştur**' u seçin ve **kaynak grubu konumunu**belirtin. Buradaki kaynak grubu ayarları Azure AD B2C kiracı konumunuzu, performansını veya faturalandırma durumunu etkilemez.
9. **Oluştur**’u seçin.

    ![Azure portal Azure AD B2C kaynak oluşturma sayfası](./media/billing/portal-01-create-b2c-resource-page.png)

Bir Azure AD B2C kiracısı için bu adımları tamamladıktan sonra Azure aboneliğiniz, varsa Azure doğrudan veya Kurumsal Anlaşma ayrıntılarına göre faturalandırılır.

## <a name="change-your-azure-ad-pricing-tier"></a>Azure AD fiyatlandırma katmanınızı değiştirin

Kiracı, Azure AD B2C kiracınızla birlikte kullanmak istediğiniz özelliklere bağlı olarak uygun Azure fiyatlandırma katmanına bağlanmalıdır. Premium özellikler, [Azure Active Directory B2C fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/)bölümünde açıklandığı gibi Azure AD B2C Premium P1 veya P2 gerektirir. Bazı durumlarda, yeni özellikleri kullanırken fiyatlandırma katmanınızı yükseltmeniz gerekir. Örneğin, kimlik koruması, risk tabanlı koşullu erişim ve Azure AD B2C gelecek Premium P2 yeteneklerini kullanmak istiyorsanız, kiracınız için Azure AD B2C Premium P2 fiyatlandırma katmanını seçmeniz gerekir.

Fiyatlandırma katmanınızı değiştirmek için aşağıdaki adımları izleyin.

1. Azure portalında oturum açın.

2. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure B2C kiracınızın bağlandığı Azure aboneliğini içeren dizini seçin (Azure AD B2C kiracının*kendisini seçmeyin)* .

3. Portalın üst kısmındaki arama kutusuna Azure AD B2C kiracınızın adını girin. Ardından, **kaynaklar**altındaki arama sonuçlarında kiracıyı seçin.

4. Kaynak **genel bakış** sayfasında, **fiyatlandırma katmanı**altında, **Değiştir**' i seçin.

   ![Fiyatlandırma katmanını değiştirme](media/billing/change-pricing-tier.png)
 
5. Etkinleştirmek istediğiniz özellikleri içeren fiyatlandırma katmanını seçin.

   ![Fiyatlandırma katmanını seçin](media/billing/select-tier.png)

## <a name="switch-to-mau-billing-pre-november-2019-azure-ad-b2c-tenants"></a>MAU faturalandırmaya geç (önceden Kasım 2019 Azure AD B2C kiracılar)

Azure AD B2C kiracınızı **1 kasım 2019 '** den önceki bir aboneliğe bağladıysanız, önceki kimlik doğrulaması başına faturalandırma modeli kullanılır. Aylık etkin kullanıcılar (MAU) faturalama modeline yükseltmenizi öneririz. Faturalandırma seçenekleri Azure AD B2C kaynağınız içinde yapılandırılır.

Aylık etkin kullanıcılar (MAU) faturalandırmaya geçiş geri **alınamaz**. Bir Azure AD B2C kaynağını MAU tabanlı faturalandırma modeline dönüştürdükten sonra, bu kaynağı kimlik doğrulaması başına faturalandırma modeline döndüremezsiniz.

Mevcut bir Azure AD B2C kaynağı için MAU faturalandırmaya geçiş yapmak için şu adımları uygulayın:

1. [Azure portal](https://portal.azure.com) Azure AD B2C kaynağına yönetici erişimi olan abonelik sahibi olarak oturum açın.

2. Üstteki menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Mau faturalandırmaya yükseltmek istediğiniz Azure AD B2C dizini seçin.<br/>

    ![Azure portal 'de dizin ve abonelik filtresi](./media/billing/portal-mau-01-select-b2c-directory.png)

3. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.

4. Azure AD B2C kiracının **genel bakış** sayfasında **kaynak adı**' nın altındaki bağlantıyı seçin. Azure AD kiracınızdaki Azure AD B2C kaynağına yönlendirilirsiniz.<br/>

    ![Azure portal Azure AD B2C kaynak bağlantısı vurgulandı](./media/billing/portal-mau-02-b2c-resource-link.png)

5. Azure AD B2C kaynağın **genel bakış** sayfasında, **faturalandırılabilir birimler**altında, **kimlik doğrulaması başına (Mau 'ya geçin)** bağlantısını seçin.<br/>

    ![MAU bağlantısının Azure portal vurgulanmış olarak değiştirilmesi](./media/billing/portal-mau-03-change-to-mau-link.png)

6. MAU faturalandırmayı yükseltmeyi gerçekleştirmek için **Onayla** ' yı seçin.<br/>

    ![Azure portal 'da MAU tabanlı faturalandırma onay iletişim kutusu](./media/billing/portal-mau-04-confirm-change-to-mau.png)


### <a name="what-to-expect-when-you-transition-to-mau-billing-from-per-authentication-billing"></a>Kimlik doğrulama başına faturalandırma 'den MAU faturalandırmaya geçiş yaptığınızda bekleneceğleriniz

MAU tabanlı ölçüm, siz, abonelik/kaynak sahibi, değişikliği onaylamak için etkin duruma gelir. Aylık faturanızda, değişiklik yapılıncaya kadar faturalandırılan kimlik doğrulaması birimleri ve değişiklik ile başlayan yeni MAU birimleri yansıtılacaktır.

Kullanıcılar geçiş ayı sırasında çift sayılmaz. Değişiklikten önce kimlik doğrulayan benzersiz etkin kullanıcılar, bir takvim ayında kimlik doğrulama ücreti başına ücretlendirilir. Aynı kullanıcılar, aboneliğin fatura döngüsünün geri kalanı için MAU hesaplamasına dahil değildir. Örneğin:

* Contoso B2C kiracısında 1.000 kullanıcısı vardır. 250 Kullanıcı belirli bir ayda etkindir. Abonelik Yöneticisi, ayın 10 ' da, kimlik doğrulamasından bağımsız olarak aylık etkin kullanıcılara (MAU) göre değişir.
* 1.10 ' un faturalandırılması, kimlik doğrulama modeli kullanılarak faturalandırılır.
  * Bu süre boyunca 100 Kullanıcı oturum açtığında (1-10), bu kullanıcılar *ay için ücretli*olarak etiketlenir.
* 10 ' dan (geçiş geçerlilik süresi) faturalama, MAU fiyatı üzerinden faturalandırılır.
  * Bu süre boyunca ek 150 Kullanıcı oturum açtığında (10-30), yalnızca ek 150 faturalandırılır.
  * İlk 100 kullanıcının devam eden etkinliği, takvim ayının geri kalanı için faturalandırmayı etkilemez.

Geçişin fatura dönemi boyunca, abonelik sahibi büyük olasılıkla Azure aboneliği faturalandırma ekstresinde her iki yöntem için de (kimlik doğrulaması başına ve MAU başına) giriş olarak görünür:

* Kimlik doğrulama başına yansıtan değişikliğin Tarih/saate kadar olan kullanım için bir giriş.
* Aylık etkin kullanıcıları (MAU) yansıtan değişiklikten sonra kullanım için bir giriş.

Azure AD B2C için kullanım faturalaması ve fiyatlandırmayla ilgili en son bilgiler için bkz. [Azure Active Directory B2C fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Azure AD B2C kiracı kaynaklarınızı yönetin

Bir Azure aboneliğinde Azure AD B2C kaynağını oluşturduktan sonra, diğer Azure kaynaklarınızla "B2C kiracısı" türünde yeni bir kaynak göründüğünü görmeniz gerekir.

Bu kaynağı kullanarak şunları yapabilirsiniz:

* Fatura bilgilerini gözden geçirmek için aboneliğe gidin
* Azure AD B2C kiracının kiracı KIMLIĞINI GUID biçiminde al
* Azure AD B2C kiracınıza gidin
* Destek isteği gönder
* Azure AD B2C kiracı kaynağınızı başka bir Azure aboneliğine veya kaynak grubuna taşıyın

### <a name="regional-restrictions"></a>Bölgesel kısıtlamalar

Aboneliğinizde Azure kaynak oluşturma için bölgesel kısıtlamalar yaptıysanız, bu kısıtlama Azure AD B2C kaynağını oluşturmanızı engelleyebilir.

Bu sorunu azaltmak için bölgesel kısıtlamalarınızı rahatlaın.

## <a name="azure-cloud-solution-providers-csp-subscriptions"></a>Azure bulut çözümü sağlayıcıları (CSP) abonelikleri

Azure bulut çözümü sağlayıcıları (CSP) abonelikleri Azure AD B2C desteklenir. İşlevsellik, Azure AD B2C ve tüm Azure kaynakları için API 'Ler veya Azure portal kullanılarak kullanılabilir. CSP aboneliği yöneticileri diğer Azure kaynaklarıyla yapılan Azure AD B2C ilişkilerini bağlayabilir, taşıyabilir ve silebilir.

Rol tabanlı erişim denetimi kullanan Azure AD B2C yönetimi, Azure AD B2C kiracısı ile Azure CSP aboneliği arasındaki ilişkilendirmeden etkilenmez. Rol tabanlı erişim denetimi, abonelik tabanlı roller değil, kiracı tabanlı roller kullanılarak elde edilir.

## <a name="change-the-azure-ad-b2c-tenant-billing-subscription"></a>Azure AD B2C kiracı faturalandırma aboneliğini değiştirin

### <a name="move-using-azure-resource-manager"></a>Azure Resource Manager kullanarak taşı

Azure AD B2C kiracılar, kaynak ve hedef abonelikler aynı Azure Active Directory kiracısında mevcutsa Azure Resource Manager kullanılarak başka bir aboneliğe taşınabilir.

Azure AD B2C kiracınız gibi Azure kaynaklarını başka bir aboneliğe taşımayı öğrenmek için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/management/move-resource-group-and-subscription.md).

Taşıma işlemine başlamadan önce, böyle bir taşımanın sınırlamalarını ve gereksinimlerini tam olarak anlamak için makalenin tamamını okuduğunuzdan emin olun. Kaynak taşıma yönergelerine ek olarak, bir taşıma öncesi denetim listesi ve taşıma işleminin nasıl doğrulanacağı gibi kritik bilgileri de içerir.

### <a name="move-by-unlinking-and-relinking"></a>Bağlantıyı kaldırma ve yeniden bağlama ile taşıma

Kaynak ve hedef abonelikler farklı Azure Active Directory kiracılar ile ilişkiliyse, yukarıda açıklanan şekilde Azure Resource Manager üzerinden taşıma işlemini gerçekleştiremezsiniz. Ancak, Azure AD B2C kiracının kaynak abonelikle bağlantısını kaldırarak ve hedef aboneliğe yeniden bağlayarak aynı sonucu elde edebilirsiniz. Yalnızca sildiğiniz nesne Azure AD B2C kiracının kendisi değil, *faturalandırma bağlantısı*olduğundan bu yöntem güvenlidir. Hiçbir Kullanıcı, uygulama, Kullanıcı akışı, vb. etkilenmez.

1. Azure AD B2C dizininde, [bir konuk kullanıcıyı](user-overview.md#guest-user) hedef Azure AD kiracısından (hedef Azure aboneliğinin bağlandığı bir bağlantı) davet edin ve bu kullanıcının Azure AD B2C **genel yönetici** rolüne sahip olduğundan emin olun.
1. Yukarıdaki [Azure AD B2C kiracı kaynaklarını yönetme](#manage-your-azure-ad-b2c-tenant-resources) bölümünde açıklandığı gibi, kaynak Azure aboneliğinizdeki Azure AD B2C temsil eden *Azure kaynağına* gidin. Gerçek Azure AD B2C kiracıya geçiş yapmayın.
1. **Genel bakış** sayfasında **Sil** düğmesini seçin. Bu, ilgili Azure AD B2C kiracının kullanıcılarını veya *uygulamalarını silmez.* Yalnızca faturalandırma bağlantısını kaynak abonelikle kaldırır.
1. Adım 1 ' de Azure AD B2C yönetici olarak eklenen kullanıcı hesabıyla Azure portal oturum açın. Ardından hedef Azure Active Directory kiracısıyla bağlantılı hedef Azure aboneliğine gidin. 
1. Yukarıdaki [bağlantı oluşturma](#create-the-link) yordamını izleyerek, hedef abonelikte faturalandırma bağlantısını yeniden oluşturun.
1. Azure AD B2C kaynağınız artık hedef Azure aboneliğine taşındı (hedef Azure Active Directory bağlantılı) ve bundan sonra bu abonelik üzerinden faturalandırılacaktır.

## <a name="next-steps"></a>Sonraki adımlar

En son fiyatlandırma bilgileri için bkz. [Azure Active Directory B2C fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/).