---
title: Azure Active Directory B2C için faturalandırma modeli
description: Azure AD B2C's aylık etkin kullanıcıları (MAU) Faturalandırma modeli ve belirli bir Azure aboneliği için faturalandırmayı etkinleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 10/25/2019
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: f9adf6ce4559234eec74c92f09aa752eb1f9ab51
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89177338"
---
# <a name="billing-model-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için faturalandırma modeli

Azure Active Directory B2C (Azure AD B2C) kullanımı, bağlantılı bir Azure aboneliğine faturalandırılır ve aylık etkin kullanıcılar (MAU) Faturalandırma modelini kullanır. Bir Azure AD B2C kaynağını aboneliğe bağlamayı ve MAU faturalandırma modelinin aşağıdaki bölümlerde nasıl çalıştığını öğrenin.

> [!IMPORTANT]
> Bu makale fiyatlandırma bilgisi içermez. Kullanım faturalaması ve fiyatlandırmayla ilgili en son bilgiler için bkz. [Azure Active Directory B2C fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="monthly-active-users-mau-billing"></a>Aylık etkin kullanıcı (MAU) Faturalandırma

Azure AD B2C faturalandırma, aylık etkin kullanıcı (MAU) Faturalandırma olarak bilinen bir takvim ayında kimlik doğrulama etkinliği olan benzersiz kullanıcı sayısı üzerinden Ücretlendirilebilir.

**01 kasım 2019**tarihinden itibaren, yeni oluşturulan Azure AD B2C kiracıların tümü aylık etkin kullanıcılar (Mau) üzerinden faturalandırılır. 01 Kasım 2019 tarihinde veya sonrasında [bir aboneliğe bağlı](#link-an-azure-ad-b2c-tenant-to-a-subscription) mevcut kiracılar, aylık etkin kullanıcılar (Mau) başına faturalandırılır.

01 Kasım 2019 ' den önceki bir aboneliğe bağlanmış mevcut bir Azure AD B2C kiracınız varsa, aşağıdakilerden birini seçebilirsiniz:

* Aylık etkin kullanıcılar (MAU) Faturalandırma modeline yükseltin veya
* Kimlik doğrulama başına faturalandırma modelinde kal

### <a name="upgrade-to-monthly-active-users-billing-model"></a>Aylık etkin kullanıcılara faturalama modeline yükselt

Azure AD B2C kaynağına yönetici erişimi olan Azure abonelik sahipleri MAU faturalandırma modeline geçebilir. Faturalandırma seçenekleri Azure AD B2C kaynağınız içinde yapılandırılır.

Aylık etkin kullanıcılar (MAU) faturalandırmaya geçiş geri **alınamaz**. Bir Azure AD B2C kaynağını MAU tabanlı faturalandırma modeline dönüştürdükten sonra, bu kaynağı kimlik doğrulaması başına faturalandırma modeline döndüremezsiniz.

Mevcut bir Azure AD B2C kaynağı için MAU faturalandırmaya geçiş yapmak için şu adımları uygulayın:

1. [Azure Portal](https://portal.azure.com) abonelik sahibi olarak oturum açın.
1. Üstteki menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Mau faturalandırmaya yükseltmek istediğiniz Azure AD B2C dizini seçin.<br/>
    ![Azure portal 'de dizin ve abonelik filtresi](./media/billing/portal-mau-01-select-b2c-directory.png)
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. Azure AD B2C kiracının **genel bakış** sayfasında **kaynak adı**' nın altındaki bağlantıyı seçin. Azure AD kiracınızdaki Azure AD B2C kaynağına yönlendirilirsiniz.<br/>
    ![Azure portal Azure AD B2C kaynak bağlantısı vurgulandı](./media/billing/portal-mau-02-b2c-resource-link.png)
1. Azure AD B2C kaynağın **genel bakış** sayfasında, **faturalandırılabilir birimler**altında, **kimlik doğrulaması başına (Mau 'ya geçin)** bağlantısını seçin.<br/>
    ![MAU bağlantısının Azure portal vurgulanmış olarak değiştirilmesi](./media/billing/portal-mau-03-change-to-mau-link.png)
1. MAU faturalandırmayı yükseltmeyi gerçekleştirmek için **Onayla** ' yı seçin.<br/>
    ![Azure portal 'da MAU tabanlı faturalandırma onay iletişim kutusu](./media/billing/portal-mau-04-confirm-change-to-mau.png)

### <a name="what-to-expect-when-you-transition-to-mau-billing-from-per-authentication-billing"></a>Kimlik doğrulama başına faturalandırma 'den MAU faturalandırmaya geçiş yaptığınızda bekleneceğleriniz

MAU tabanlı ölçüm, siz, abonelik/kaynak sahibi, değişikliği onaylamak için etkin duruma gelir. Aylık faturanızda, değişiklik yapılıncaya kadar faturalandırılan kimlik doğrulaması birimleri ve değişiklik ile başlayan yeni MAU birimleri yansıtılacaktır.

Geçiş ayı sırasında kullanıcılar çift sayılmaz. Değişiklikten önce kimlik doğrulayan benzersiz etkin kullanıcılar, bir takvim ayında kimlik doğrulama ücreti başına ücretlendirilir. Aynı kullanıcılar, aboneliğin fatura döngüsünün geri kalanı için MAU hesaplamasına dahil edilmez. Örneğin:

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

## <a name="link-an-azure-ad-b2c-tenant-to-a-subscription"></a>Azure AD B2C kiracıyı aboneliğe bağlama

Azure Active Directory B2C (Azure AD B2C) için kullanım ücretleri bir Azure aboneliğine faturalandırılır. Azure AD B2C kiracı oluşturulduğunda, kiracı yöneticisinin Azure AD B2C kiracıyı açık bir şekilde bir Azure aboneliğine bağlanması gerekir.

Abonelik bağlantısı, hedef Azure aboneliği içinde bir Azure AD B2C *kaynağı* oluşturularak elde edilir. Birçok Azure AD B2C kaynak, sanal makineler, depolama hesapları ve Logic Apps gibi diğer Azure kaynaklarıyla birlikte tek bir Azure aboneliğinde oluşturulabilir. Aboneliğin ilişkilendirildiği Azure Active Directory (Azure AD) kiracısına giderek bir abonelik içindeki kaynakların tümünü görebilirsiniz.

Azure AD B2C kiracıya bağlı bir abonelik, ek Azure AD B2C kaynakları da dahil olmak üzere Azure AD B2C kullanım veya diğer Azure kaynaklarının faturalandırılması için kullanılabilir. Azure AD B2C kiracının içindeki diğer Azure lisans tabanlı Hizmetleri veya Office 365 lisanslarını eklemek için kullanılamaz.

### <a name="prerequisites"></a>Ön koşullar

* [Azure aboneliği](https://azure.microsoft.com/free/)
* Aboneliğe bağlamak istediğiniz [Azure AD B2C kiracı](tutorial-create-tenant.md)
  * Kiracı Yöneticisi olmanız gerekir
  * Kiracının zaten bir aboneliğe bağlı olmaması gerekir

### <a name="create-the-link"></a>Bağlantıyı oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından kullanmak istediğiniz Azure aboneliğini içeren dizini seçin (Azure AD B2C kiracıyı içeren dizin*değil* ).
1. **Kaynak oluştur**' u seçin, `Active Directory B2C` Market alanını **Ara** alanına girin ve ardından **Azure Active Directory B2C**' yi seçin.
1. **Oluştur**’u seçin
1. **Mevcut bir Azure AD B2C kiracıyı Azure aboneliğime bağla**' yı seçin.
1. Açılan listeden bir **Azure AD B2C kiracı** seçin. Yalnızca genel yöneticisi olduğunuz ve bir aboneliğe henüz bağlanmamış olan kiracılar gösterilir. **Azure AD B2C kaynak adı** alanı seçtiğiniz Azure AD B2C kiracının etki alanı adıyla doldurulur.
1. Yönetici olduğunuz etkin bir Azure **aboneliğini** seçin.
1. **Kaynak grubu**altında **Yeni oluştur**' u seçin ve **kaynak grubu konumunu**belirtin. Buradaki kaynak grubu ayarları Azure AD B2C kiracı konumunuzu, performansını veya faturalandırma durumunu etkilemez.
1. **Oluştur**’u seçin.
    ![Azure portal Azure AD B2C kaynak oluşturma sayfası](./media/billing/portal-01-create-b2c-resource-page.png)

Azure AD B2C kiracısı için bu adımları tamamladıktan sonra Azure aboneliğiniz, varsa Azure doğrudan veya Kurumsal Anlaşma ayrıntılarınıza uygun olarak faturalandırılır.

### <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Azure AD B2C kiracı kaynaklarınızı yönetin

Bir Azure aboneliğinde Azure AD B2C kaynağını oluşturduktan sonra, diğer Azure kaynaklarınızla "B2C kiracısı" türünde yeni bir kaynak göründüğünü görmeniz gerekir.

Bu kaynağı kullanarak şunları yapabilirsiniz:

* Fatura bilgilerini gözden geçirmek için aboneliğe gidin
* Azure AD B2C kiracının kiracı KIMLIĞINI GUID biçiminde al
* Azure AD B2C kiracınıza gidin
* Destek isteği gönder
* Azure AD B2C kiracı kaynağınızı başka bir Azure aboneliğine veya kaynak grubuna taşıyın

![Azure portal B2C kaynak ayarları sayfası](./media/billing/portal-02-b2c-resource-overview.png)

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

Taşımayı başlatmadan önce, böyle bir taşımanın sınırlamalarını ve gereksinimlerini tam olarak anlamak için makalenin tamamını okuduğunuzdan emin olun. Kaynak taşıma yönergelerine ek olarak, bir taşıma öncesi denetim listesi ve taşıma işleminin nasıl doğrulanacağı gibi kritik bilgileri de içerir.

### <a name="move-by-un-linking-and-re-linking"></a>Bağlama ve yeniden bağlama ile taşıma

Kaynak ve hedef abonelikler farklı Azure Active Directory kiracılar ile ilişkiliyse, yukarıda açıklanan şekilde Azure Resource Manager üzerinden taşıma işlemini gerçekleştiremezsiniz. Ancak, Azure AD B2C kiracının kaynak abonelikle bağlantısını kaldırarak ve hedef aboneliğe yeniden bağlayarak aynı nihai sonucu elde edebilirsiniz. Yalnızca sildiğiniz nesne Azure AD B2C kiracının kendisi değil, *faturalandırma bağlantısı*olduğundan bu yöntem güvenlidir. Hiçbir Kullanıcı, uygulama, Kullanıcı akışı, vb. etkilenmez.

1. Azure AD B2C dizininde, [bir konuk kullanıcıyı](user-overview.md#guest-user) hedef Azure AD kiracısından (hedef Azure aboneliğinin bağlandığı bir bağlantı) davet edin ve bu kullanıcının Azure AD B2C **genel yönetici** rolüne sahip olduğundan emin olun.
1. Yukarıdaki [Azure AD B2C kiracı kaynaklarını yönetme](#manage-your-azure-ad-b2c-tenant-resources) bölümünde açıklandığı gibi, kaynak Azure aboneliğinizdeki Azure AD B2C temsil eden *Azure kaynağına* gidin. Gerçek Azure AD B2C kiracıya geçiş yapmayın.
1. **Genel bakış** sayfasında **Sil** düğmesine tıklayın. Bu, ilgili Azure AD B2C kiracının kullanıcılarını veya *uygulamalarını silmez.* Yalnızca faturalandırma bağlantısını kaynak abonelikle kaldırır.
1. Adım 1 ' de Azure AD B2C yönetici olarak eklenen kullanıcı hesabıyla Azure portal oturum açın. Ardından hedef Azure Active Directory kiracısıyla bağlantılı hedef Azure aboneliğine gidin. 
1. Yukarıdaki [bağlantı oluşturma](#create-the-link) yordamını izleyerek, hedef abonelikte faturalandırma bağlantısını yeniden oluşturun.
1. Azure AD B2C kaynağınız artık hedef Azure aboneliğine taşındı (hedef Azure Active Directory bağlantılı) ve bu abonelik üzerinden ücretlendirilecektir.

## <a name="next-steps"></a>Sonraki adımlar

En son fiyatlandırma bilgileri için bkz. [Azure Active Directory B2C fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
