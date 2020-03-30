---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Elium'u yapılandırın | Microsoft Dokümanlar"
description: Kullanıcı hesaplarını Elium'a otomatik olarak sağlamak ve sağlamadan çıkarmak için Azure Active Directory'yi nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: fb48deae-4653-448a-ba2f-90258edab3a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2019
ms.author: Zhchia
ms.openlocfilehash: a3e1c936d46e0d9b0ad3b98dafeda7df0b4a23db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058522"
---
# <a name="tutorial-configure-elium-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Elium'u yapılandır

Bu öğretici, Elium ve Azure Etkin Dizininin (Azure AD) kullanıcıları veya grupları Elium'a otomatik olarak sağlaması ve sağlamadan kaldırılması için nasıl yapılandırılabildiğini gösterir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı ve nasıl çalıştığı hakkında önemli ayrıntılar ve sık sorulan sorular için Azure [Etkin Dizini ile SaaS uygulamalarını otomatikleştir](../app-provisioning/user-provisioning.md)ve yok etme konusuna bakın.
>
> Bu bağlayıcı şu anda önizlemede. Önizlemedeki Azure özelliklerinin genel kullanım koşulları için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, zaten aşağıdaki ön koşullara sahip olduğunu varsayar:

* Azure AD kiracı
* [Bir Elium kiracı](https://www.elium.com/pricing/)
* Yönetici izinleri olan Elium'da bir kullanıcı hesabı

## <a name="assigning-users-to-elium"></a>Elium'a kullanıcı atama

Azure AD, hangi kullanıcıların seçili uygulamalara erişebilenleri belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve grupların Elium'a erişmeye ihtiyacı olduğuna karar verin. Ardından, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atay](../manage-apps/assign-user-or-group-access-portal.md)adımlarını izleyerek bu kullanıcıları ve grupları Elium'a atayın.

## <a name="important-tips-for-assigning-users-to-elium"></a>Elium'a kullanıcı atamak için önemli ipuçları 

Otomatik kullanıcı sağlama yapılandırmasını test etmek için Elium'a tek bir Azure AD kullanıcısı atamanızı öneririz. Daha sonra daha fazla kullanıcı ve grup atanabilir.

Elium'a bir kullanıcı atarken, atama iletişim kutusunda geçerli, uygulamaya özgü bir rol (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-elium-for-provisioning"></a>Tedarik için Elium'u ayarlama

Elium'u Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Elium'da Etki Alanları Arası Kimlik Yönetimi Sistemi (SCIM) sağlamayı etkinleştirmeniz gerekir. Şu adımları uygulayın:

1. Elium'da oturum açın ve >  **Profil Ayarlarım'a**gidin.**Settings**

    ![Elium'daki ayarlar menü öğesi](media/Elium-provisioning-tutorial/setting.png)

1. Sol alt köşede, **ADVANCED**altında, **Güvenlik'i**seçin.

    ![Elium'daki güvenlik bağlantısı](media/Elium-provisioning-tutorial/security.png)

1. Kiracı **URL'sini** ve **Gizli belirteç** değerlerini kopyalayın. Bu değerleri daha sonra, Azure portalındaki Elium uygulamanızın **Sağlama** sekmesindeki ilgili alanlarda kullanırsınız.

    ![Elium'daki kiracı URL'si ve Gizli belirteç alanları](media/Elium-provisioning-tutorial/token.png)

## <a name="add-elium-from-the-gallery"></a>Galeriden Elium ekle

Elium'u Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, Azure AD uygulama galerisinden Elium'u da yönetilen hizmet olarak yazılım (SaaS) uygulamaları listenize eklemeniz gerekir. Şu adımları uygulayın:

1. Azure [portalında,](https://portal.azure.com)soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Active Directory menü öğesi](common/select-azuread.png)

1. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

     ![Azure AD Kurumsal uygulamaları bıçak](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama bağlantısı](common/add-new-app.png)

1. Arama kutusunda **Elium**yazın, sonuç listesinde **Elium'u** seçin ve ardından uygulamayı eklemek için **Ekle'yi** seçin.

    ![Galeri arama kutusu](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-elium"></a>Elium için otomatik kullanıcı sağlama yapılandırma

Bu bölüm, Azure AD'deki kullanıcı ve grup atamalarına dayalı olarak Elium'daki kullanıcıları ve grupları oluşturmak, güncelleştirmek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size rehberlik eder.

> [!TIP]
> Ayrıca, [Elium tek oturum](Elium-tutorial.md)açma öğreticisindeki talimatları izleyerek, Güvenlik İddiası İşaretleme Dili'ne (SAML) dayalı olarak Elium için tek oturum açma özelliğini etkinleştirmeyi de seçebilirsiniz. İki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak tek oturum açma yı yapılandırabilirsiniz.

Azure AD'de Elium için otomatik kullanıcı sağlama yapılandırmak için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın, **Kurumsal uygulamaları**seçin ve ardından Tüm **uygulamaları**seçin.

    ![Azure AD Kurumsal uygulamaları bıçak](common/enterprise-applications.png)

1. Uygulamalar listesinde **Elium'u**seçin.

    ![Enterprise uygulamaları bıçak uygulama listesi](common/all-applications.png)

1. **Sağlama** sekmesini seçin.

    ![Kurumsal uygulamalar bıçak sağlama sekmesi](common/provisioning.png)

1. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama Modu için otomatik ayar](common/provisioning-automatic.png)

1. Yönetici **Kimlik Bilgileri** bölümünde, **Kiracı URL** alanına ** \<tenantURL\>/scim/v2** yazın. **(TenantURL,** Elium yönetici konsolundan daha önce alınan değerdir.) Ayrıca, **Gizli Belirteç** alanına Elium **Secret belirteç** değerini yazın. Son olarak, Azure AD'nin Elium'a bağlanabiliyor olduğunu doğrulamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Elium hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Yönetici Kimlik Bilgileri'ndeki Kiracı URL'si ve Gizli Belirteç alanları](common/provisioning-testconnection-tenanturltoken.png)

1. Bildirim **E-postası** alanına, sağlama hatası bildirimleri alacak kişi veya grubun e-posta adresini girin. Ardından, **bir hata olduğunda e-posta gönder bildirimini** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

1. **Kaydet**'e tıklayın.

1. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Elium'a Senkronize Et'i**seçin.

    ![Azure AD kullanıcılarını Elium'a eşlemek için bağlantıyı senkronize etme](media/Elium-provisioning-tutorial/usermapping.png)

1. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Elium'a senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Elium'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Azure AD ve Elium arasındaki öznitelik eşlemeleri](media/Elium-provisioning-tutorial/userattribute.png)

1. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisindeki yönergeleri izleyin.

1. Elium için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Açık** olarak değiştirin.

    ![Başlangıç Durumu A)'ya ayarlanmış](common/provisioning-toggle-on.png)

1. **Ayarlar** bölümündeki **Kapsam** açılır liste kutusunda istediğiniz değerleri seçerek Elium'a sağlamak istediğiniz kullanıcıları ve grupları tanımlayın.

    ![Scope liste kutusunu sağlama](common/provisioning-scope.png)

1. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Yapılandırma yı sağlama düğmesi kaydet](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve grupların ilk eşitlemisini başlatır. Bu ilk eşitleme işlemi daha sonra eşitleme daha uzun sürer. Sağlama için gereken süre hakkında daha fazla bilgi [için,](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)bkz.

İlerlemeyi izlemek ve sağlama faaliyet raporunuzdaki bağlantıları izlemek için **Geçerli Durum** bölümünü kullanın. Sağlama etkinliği raporu, Azure AD sağlama hizmeti tarafından Elium'da gerçekleştirilen tüm eylemleri açıklar. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamayı yönetme.](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
