---
title: Azure Active Directory B2C için faturalandırma modeli
description: Azure AD B2C'nin aylık etkin kullanıcılar (MAU) faturalandırma modeli ve belirli bir Azure aboneliği için faturalandırmayı nasıl etkinleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/25/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 30ecb1e6faa29482a8d69dd1d08e4f127f515596
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190016"
---
# <a name="billing-model-for-azure-active-directory-b2c"></a>Azure Active Directory B2C için faturalandırma modeli

Azure Active Directory B2C (Azure AD B2C) kullanımı bağlantılı bir Azure aboneliğine faturalandırılır ve aylık etkin kullanıcılar (MAU) faturalandırma modeli kullanır. Azure AD B2C kaynağını aboneye nasıl bağlayacaklarını ve mau faturalandırma modelinin aşağıdaki bölümlerde nasıl çalıştığını öğrenin.

> [!IMPORTANT]
> Bu makale, fiyatlandırma bilgilerini içermez. Kullanım faturalandırması ve fiyatlandırma hakkında en son bilgiler için [Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/)fiyatlandırması'na bakın.

## <a name="monthly-active-users-mau-billing"></a>Aylık etkin kullanıcılar (MAU) faturalandırma

Azure AD B2C faturalandırması, aylık etkin kullanıcılar (MAU) faturalandırmaolarak bilinen bir takvim ayı içinde kimlik doğrulama etkinliği olan benzersiz kullanıcıların sayısına göre ölçülür.

**01 Kasım 2019'dan**itibaren, yeni oluşturulan tüm Azure AD B2C kiracıları aylık aktif kullanıcılar başına faturalandırılır (MAU). 01 Kasım 2019 tarihinde veya sonrasında [bir aboneye bağlanan](#link-an-azure-ad-b2c-tenant-to-a-subscription) mevcut kiracılar aylık aktif kullanıcılar (MAU) başına faturalandırılır.

01 Kasım 2019 tarihinden önce bir aboneye bağlı olan mevcut bir Azure AD B2C kiracınız varsa, aşağıdakilerden birini yapmayı seçebilirsiniz:

* Aylık etkin kullanıcılar (MAU) faturalandırma modeline yükseltme veya
* Kimlik doğrulama faturalandırma modelinde kalın

### <a name="upgrade-to-monthly-active-users-billing-model"></a>Aylık etkin kullanıcı faturalandırma modeline yükseltme

Azure AD B2C kaynağına yönetim erişimi olan Azure abonelik sahipleri MAU faturalandırma modeline geçebilir. Faturalandırma seçenekleri Azure AD B2C kaynağınızda yapılandırılır.

Aylık etkin kullanıcılara geçiş (MAU) faturalama **geri alınamaz.** Bir Azure AD B2C kaynağını MAU tabanlı faturalandırma modeline dönüştürdükten sonra, bu kaynağı kimlik doğrulama faturalandırma modeline geri döndüremezsiniz.

Varolan bir Azure AD B2C kaynağı için MAU faturalandırmasına geçiş şu şekilde yapabilirsiniz:

1. Abonelik sahibi olarak [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından MAU faturalandırmasına yükseltmek istediğiniz Azure AD B2C dizinini seçin.<br/>
    ![Azure portalında dizin ve abonelik filtresi](./media/billing/portal-mau-01-select-b2c-directory.png)
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. Azure AD B2C kiracısının **Genel Bakış** sayfasında, **Kaynak adı**altındaki bağlantıyı seçin. Azure AD kiracınızdaki Azure AD B2C kaynağına yönlendirilirsiniz.<br/>
    ![Azure portalında vurgulanan Azure AD B2C kaynak bağlantısı](./media/billing/portal-mau-02-b2c-resource-link.png)
1. Azure AD B2C kaynağının **Genel Bakış** sayfasında, **Faturalandırılabilir Birimler**altında, Kimlik Doğrulama Başına **(MAU'ya Değişiklik)** bağlantısını seçin.<br/>
    ![Azure portalında vurgulanan MAU bağlantısına geçiş](./media/billing/portal-mau-03-change-to-mau-link.png)
1. MAU faturalandırmayükseltmesini tamamlamak için **Onayla'yı** seçin.<br/>
    ![Azure portalında MAU tabanlı faturalandırma onay iletişim kutusu](./media/billing/portal-mau-04-confirm-change-to-mau.png)

### <a name="what-to-expect-when-you-transition-to-mau-billing-from-per-authentication-billing"></a>Kimlik doğrulama faturalandırmasından MAU faturalandırmasına geçiş yaptığınızda ne beklemeniz gerekiyor?

Mau tabanlı ölçüm, siz, abonelik/kaynak sahibi, değişikliği onayladığıandan etkinleştirilir. Aylık faturanız, değişiklik le başlayan adedimasyon birimlerini ve değişiklikten başlayarak MAU'nun yeni birimlerini yansıtacaktır.

Kullanıcılar geçiş ayı boyunca çift sayılmaz. Değişiklikten önce kimlik doğrulaması yapan benzersiz etkin kullanıcılardan bir takvim ayında kimlik doğrulama başına ücret alınır. Aynı kullanıcılar, aboneliğin faturalandırma döngüsünün geri kalanı için MAU hesaplamalarına dahil edilmez. Örnek:

* Contoso B2C kiracı 1.000 kullanıcıya sahiptir. Herhangi bir ay içinde 250 kullanıcı etkindir. Abonelik yöneticisi, ayın 10'unda kimlik doğrulama başına aylık etkin kullanıcılara (MAU) dönüşür.
* 1.-10. faturalandırma, kimlik doğrulama başına model kullanılarak faturalandırılır.
  * Bu dönemde (1.-10.) 100 kullanıcı oturum açsa, bu kullanıcılar *ay için ödenmiş*olarak etiketlenir.
* 10'uncu (geçişin etkin saati) faturalandırması MAU oranında faturalandırılır.
  * Bu dönemde (10.-30.) 150 kullanıcı daha oturum açsa, yalnızca ek 150 kullanıcı faturalandırılır.
  * İlk 100 kullanıcının devam eden etkinliği, takvim ayının geri kalanı için faturalandırmayı etkilemez.

Geçişin faturalama döneminde, abonelik sahibi büyük olasılıkla her iki yönteme (kimlik doğrulama ve MAU başına) yönelik girişleri Azure abonelik faturalandırma bildirimlerinde görünür:

* Değişiklik tarihine/saatine kadar kullanım için kimlik doğrulama başına yansıtılan bir giriş.
* Aylık etkin kullanıcıları (MAU) yansıtan değişiklikten sonra kullanım için bir giriş.

Azure AD B2C için kullanım faturalandırması ve fiyatlandırması hakkında en son bilgiler için [Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/)fiyatlandırması'na bakın.

## <a name="link-an-azure-ad-b2c-tenant-to-a-subscription"></a>Azure AD B2C kiracısını aboneye bağlama

Azure Active Directory B2C (Azure AD B2C) kullanım ücretleri bir Azure aboneliğine faturalandırılır. Bir Azure AD B2C kiracı oluşturulduğunda, kiracı yöneticinin Azure AD B2C kiracısını açıkça bir Azure aboneliğine bağlaması gerekir.

Abonelik bağlantısı, hedef Azure aboneliği içinde bir Azure AD B2C *kaynağı* oluşturarak elde edilir. Sanal makineler, Depolama hesapları ve Logic Apps gibi diğer Azure kaynaklarıyla birlikte tek bir Azure aboneliğinde birden fazla Azure AD B2C kaynağı oluşturulabilir. Aboneliğin ilişkili olduğu Azure Etkin Dizin (Azure AD) kiracısına giderek abonelikteki tüm kaynakları görebilirsiniz.

Azure AD B2C kiracısına bağlı bir abonelik, Azure AD B2C kullanımının veya ek Azure AD B2C kaynakları da dahil olmak üzere diğer Azure kaynaklarının faturalanmasında kullanılabilir. Azure AD B2C kiracısına diğer Azure lisans tabanlı hizmetleri veya Office 365 lisanslarını eklemek için kullanılamaz.

### <a name="prerequisites"></a>Ön koşullar

* [Azure aboneliği](https://azure.microsoft.com/free/)
* Aboneye bağlamak istediğiniz [Azure AD B2C kiracısı](tutorial-create-tenant.md)
  * Kiracı yönetici olmalısınız
  * Kiracı zaten bir aboneye bağlı olmamalıdır

### <a name="create-the-link"></a>Bağlantıyı oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından kullanmak istediğiniz Azure aboneliğini içeren dizin 'i seçin (Azure AD B2C kiracısını içeren dizini*değil).*
1. **Kaynak Oluştur'u** `Active Directory B2C` seçin, Pazar **Yeri Ara** alanına girin ve ardından Azure **Etkin Dizini B2C'yi**seçin.
1. **Oluştur**’u seçin
1. **Varolan bir Azure AD B2C Kiracısını Azure aboneliğime**bağla'yı seçin.
1. Açılır dosyadan bir **Azure AD B2C Kiracısı** seçin. Yalnızca genel yönetici olduğunuz ve zaten bir abonelikle bağlantılı olmayan kiracılar gösterilir. **Azure AD B2C Kaynak adı** alanı, seçtiğiniz Azure AD B2C kiracısının etki alanı adıile doldurulur.
1. Yönetici olduğunuz etkin bir Azure **Aboneliği'ni** seçin.
1. **Kaynak grubu**altında, **yeni oluştur'u**seçin ve ardından Kaynak **grubu konumunu**belirtin. Buradaki kaynak grubu ayarlarının Azure AD B2C kiracı konumunuz, performansınız veya faturalandırma durumunuz u etkilenmez.
1. **Oluştur'u**seçin.
    ![Azure portalında Azure AD B2C Kaynak oluşturma sayfası](./media/billing/portal-01-create-b2c-resource-page.png)

Azure AD B2C kiracısı için bu adımları tamamladıktan sonra, Azure aboneliğiniz varsa Azure Doğrudan veya Kurumsal Sözleşme bilgilerinize uygun olarak faturalandırılır.

### <a name="manage-your-azure-ad-b2c-tenant-resources"></a>Azure AD B2C kiracı kaynaklarınızı yönetme

Azure aboneliğinde Azure AD B2C kaynağını oluşturduktan sonra, diğer Azure kaynaklarınızda "B2C kiracısı" türünde yeni bir kaynağın görüntülediğinizi görmeniz gerekir.

Bu kaynağı şu şekilde kullanabilirsiniz:

* Fatura bilgilerini gözden geçirmek için aboneye gidin
* Azure AD B2C kiracısının kiracı kimliğini GUID biçiminde alın
* Azure AD B2C kiracınıza gidin
* Destek isteği gönderme
* Azure AD B2C kiracı kaynağınızı başka bir Azure aboneliğine veya kaynak grubuna taşıma

![Azure portalında B2C Kaynak ayarları sayfası](./media/billing/portal-02-b2c-resource-overview.png)

### <a name="regional-restrictions"></a>Bölgesel kısıtlamalar

Aboneliğinizde Azure kaynak oluşturma için bölgesel kısıtlamalar oluşturduysanız, bu kısıtlama Azure AD B2C kaynağını oluşturmanızı engelleyebilir.

Bu sorunu azaltmak için, bölgesel kısıtlamaları nızı gevşetin.

## <a name="azure-cloud-solution-providers-csp-subscriptions"></a>Azure Bulut Çözüm Sağlayıcıları (CSP) abonelikleri

Azure Bulut Çözüm Sağlayıcıları (CSP) abonelikleri Azure AD B2C'de desteklenir. İşlevsellik, Azure AD B2C için API'ler veya Azure portalı ve tüm Azure kaynakları kullanılarak kullanılabilir. CSP abonelik yöneticileri, diğer Azure kaynaklarında olduğu gibi Azure AD B2C ile ilişkileri bağlayabilir, taşıyabilir ve silebilir.

Rol tabanlı erişim denetimi kullanarak Azure AD B2C'nin yönetimi, Azure AD B2C kiracısı ile Azure CSP aboneliği arasındaki ilişkiden etkilenmez. Rol tabanlı erişim denetimi, abonelik tabanlı roller değil, kiracı tabanlı roller kullanılarak sağlanır.

## <a name="change-the-azure-ad-b2c-tenant-billing-subscription"></a>Azure AD B2C kiracı fatura aboneliğini değiştirme

Kaynak ve hedef abonelikler aynı Azure Etkin Dizin kiracıiçinde varsa Azure AD B2C kiracıları başka bir aboneye taşınabilir.

Azure AD B2C kiracınız gibi Azure kaynaklarını başka bir aboneye nasıl taşıyacağız öğrenmek [için bkz.](../azure-resource-manager/management/move-resource-group-and-subscription.md)

Taşımayı başlatmadan önce, böyle bir hareketin sınırlamalarını ve gereksinimlerini tam olarak anlamak için makalenin tamamını okuduğunuzdan emin olun. Kaynakları taşıma yönergelerine ek olarak, önceden taşıma denetim listesi ve taşıma işleminin nasıl doğrulanması gibi kritik bilgileri de içerir.

## <a name="next-steps"></a>Sonraki adımlar

En son fiyatlandırma bilgileri için [Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/)fiyatlandırması'na bakın.
