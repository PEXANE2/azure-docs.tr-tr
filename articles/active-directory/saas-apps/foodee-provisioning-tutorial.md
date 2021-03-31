---
title: "Öğretici: Azure Active Directory kullanarak otomatik Kullanıcı sağlama için Pdee 'yi yapılandırma | Microsoft Docs"
description: Azure Active Directory yapılandırma hakkında bilgi edinmek için Kullanıcı hesaplarını otomatik olarak sağlama ve sağlama.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/30/2019
ms.author: Zhchia
ms.openlocfilehash: 8b4bfa7e9bf457d79c6c4a0b5255bce4fe36dff4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94358703"
---
# <a name="tutorial-configure-foodee-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Pdee 'yi yapılandırma

Bu makalede, Pdee ve Azure AD 'de Azure Active Directory (Azure AD) ' nin nasıl yapılandırılacağı ve Kullanıcı veya grupları Pdee 'nin otomatik olarak sağlaması veya sağlaması için nasıl yapılandırılacağı gösterilmektedir.

> [!NOTE]
> Makalesinde, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine kurulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını ve nasıl çalıştığını öğrenmek ve sık sorulan sorulara yanıt almak için, bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda önizleme aşamasındadır. Önizleme özellikleri için Azure kullanım koşulları özelliği hakkında daha fazla bilgi için, [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)' na gidin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, aşağıdaki önkoşulları karşıladığınızı varsayılmaktadır:

* Bir Azure AD kiracısı
* [Bir Pdee kiracısı](https://www.food.ee/about/)
* Yönetici izinlerine sahip bir kullanıcı hesabı

## <a name="assign-users-to-foodee"></a>Kullanıcıları Kandee ata 

Azure AD, seçilen uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adlı bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların veya grupların Pdee 'ye erişmesi gerektiğine karar vermeniz gerekir. Bu belirleme yapıldıktan sonra, [bir kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)bölümündeki yönergeleri izleyerek bu kullanıcıları veya grupları kadee 'ye atayabilirsiniz.

## <a name="important-tips-for-assigning-users-to-foodee"></a>Kullanıcıları Kandee atamaya yönelik önemli ipuçları 

Kullanıcıları atarken aşağıdaki ipuçlarını göz önünde bulundurun:

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için, pdee 'ye yalnızca tek bir Azure AD kullanıcısı atamanız önerilir. Daha sonra ek kullanıcı veya grup atayabilirsiniz.

* Bir kullanıcıyı Ordee atarken, varsa, **atama** bölmesinde, uygulamaya özgü geçerli herhangi bir rolü seçin. *Varsayılan erişim* rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="set-up-foodee-for-provisioning"></a>Sağlama için Pdee 'yi ayarlama

Azure AD 'yi kullanarak otomatik Kullanıcı sağlaması için Pdee 'yi yapılandırmadan önce, Kdee 'de etki alanları arası kimlik yönetimi (SCıM) sağlama için sistemi etkinleştirmeniz gerekir.

1. [Pdee](https://www.food.ee/login/)'de oturum açın ve ardından kiracı kimliğinizi seçin.

    :::image type="content" source="media/Foodee-provisioning-tutorial/tenant.png" alt-text="Pdee Enterprise portalının ana menüsünün ekran görüntüsü. Bir Kiracı kimliği yer tutucusu, menüde görünür." border="false":::

1. **Enterprise Portal** altında **Çoklu oturum açma**' yı seçin.

    ![Alt Enterprise Portal sol bölmedeki menü](media/Foodee-provisioning-tutorial/scim.png)

1. Daha sonra kullanmak üzere **API belirteç** kutusundaki değeri kopyalayın. Bu anahtarı, Azure portal Pdee uygulamanızın **sağlama** sekmesinde **gizli belirteç** kutusuna girersiniz.

    :::image type="content" source="media/Foodee-provisioning-tutorial/token.png" alt-text="Pdee Enterprise portalındaki bir sayfanın ekran görüntüsü. Bir P ı belirteç değeri vurgulanır." border="false":::

## <a name="add-foodee-from-the-gallery"></a>Galeriden bir altbilgi ekleyin

Azure AD 'yi kullanarak otomatik Kullanıcı sağlaması için Pdee 'yi yapılandırmak için Azure AD uygulama galerisindeki Pdee 'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

Azure AD uygulama galerisinden Pdee eklemek için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com)sol bölmedeki **Azure Active Directory**' ı seçin.

    ![Azure Active Directory komutu](common/select-azuread.png)

1. **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** ' yı seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

1. Arama kutusuna **pdee** yazın, sonuçlar bölmesinde **pdee** ' yi seçin ve ardından uygulamayı eklemek için **Ekle** ' yi seçin.

    ![Sonuç listesindeki pdee](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-foodee"></a>Pdee için otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı veya grup atamalarına göre kadee 'de kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırırsınız.

> [!TIP]
> Ayrıca, pdee [Çoklu oturum açma öğreticisindeki](Foodee-tutorial.md)yönergeleri izleyerek, pun için SAML tabanlı çoklu oturum açmayı etkinleştirebilirsiniz. Otomatik Kullanıcı sağlamaktan bağımsız olarak çoklu oturum açmayı yapılandırabilirsiniz, ancak bu iki özellik birbirini tamamlar.

Aşağıdaki işlemleri gerçekleştirerek Azure AD 'de Pdee için otomatik Kullanıcı sağlamayı yapılandırın:

1. [Azure Portal](https://portal.azure.com) **Kurumsal uygulamalar**  >  **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. **Uygulamalar** listesinde, **pdee**' yi seçin.

    ![Uygulamalar listesindeki Pdee bağlantısı](common/all-applications.png)

1. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

1. **Sağlama modu** aşağı açılan listesinde **Otomatik**' i seçin.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

1. **Yönetici kimlik bilgileri** altında şunları yapın:

   a. **Kiracı URL 'si** kutusuna, daha önce aldığınız **https: \/ /concierge.Food.ee/Scim/v2** değerini girin.

   b. **Gizli belirteç** kutusunda, daha önce aldığınız **API belirteç** değerini girin.
   
   c. Azure AD 'nin, Pdee 'ye bağlanabildiğinden emin olmak için **Bağlantıyı Sına**' yı seçin. Bağlantı başarısız olursa, Pdee hesabınızın yönetici izinlerine sahip olduğundan emin olun ve sonra yeniden deneyin.

    ![Sınama bağlantısı bağlantısı](common/provisioning-testconnection-tenanturltoken.png)

1. **Bildirim e-postası** kutusunda, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve ardından **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası metin kutusu](common/provisioning-notification-email.png)

1. **Kaydet**’i seçin.

1. **Eşlemeler**' in altında **Azure Active Directory Kullanıcıları kadee olarak eşitlemeyi** seçin.

    :::image type="content" source="media/Foodee-provisioning-tutorial/usermapping.png" alt-text="Eşlemeler bölümünün ekran görüntüsü. Ad ' ın altında, Azure Active Directory Kullanıcıları kadee ile eşitler." border="false":::

1. **Öznitelik eşlemeleri** altında, Azure AD 'Den Pdee ile eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri Için Pdee içindeki *Kullanıcı hesaplarıyla* eşleştirmek için kullanılır. 

    :::image type="content" source="media/Foodee-provisioning-tutorial/userattribute.png" alt-text="Öznitelik eşlemeleri sayfasının ekran görüntüsü. Tablo, Azure Active Directory ve Pdee özniteliklerini ve eşleşen önceliği listeler." border="false":::

1. Değişikliklerinizi uygulamak için **Kaydet**' i seçin.
1. **Eşlemeler**' in altında, **Azure Active Directory gruplarını Pdee ile eşitler**' ı seçin.

    :::image type="content" source="media/Foodee-provisioning-tutorial/groupmapping.png" alt-text="Eşlemeler bölümünün ekran görüntüsü. Ad ' ın altında, Azure Active Directory grupları kadee ile eşitler." border="false":::

1. **Öznitelik eşlemeleri** altında, Azure AD 'Den Pdee ile eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri Için Pdee içindeki *Grup hesaplarıyla* eşleştirmek için kullanılır.

    :::image type="content" source="media/Foodee-provisioning-tutorial/groupattribute.png" alt-text="Öznitelik eşlemeleri sayfasının ekran görüntüsü. Tablo, Azure Active Directory özniteliklerini, Pdee özniteliklerini ve eşleşen önceliği listeler." border="false":::

1. Değişikliklerinizi uygulamak için **Kaydet**' i seçin.
1. Kapsam filtrelerini yapılandırın. Nasıl yapılacağını öğrenmek için [kapsam filtresi öğreticisindeki](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)yönergelere bakın.

1. Pdee için Azure AD sağlama hizmetini etkinleştirmek üzere, **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu anahtarı](common/provisioning-toggle-on.png)

1. **Ayarlar**' ın altında, **kapsam** açılan listesinde, pdee 'ye sağlamak istediğiniz kullanıcıları veya grupları tanımlayın.

    ![Sağlama kapsamı açılan listesi](common/provisioning-scope.png)

1. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Sağlama yapılandırma Kaydet düğmesi](common/provisioning-configuration-save.png)

Yukarıdaki işlem, **kapsam** açılan listesinde tanımladığınız kullanıcıların veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin sonraki eşitlemeler daha uzun sürer. Daha fazla bilgi için bkz. [Kullanıcı sağlama ne kadar sürer?](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users).

İlerlemeyi izlemek ve sağlama etkinliği raporunuzun bağlantılarını izlemek için **geçerli durum** bölümünü kullanabilirsiniz. Rapor, Azure AD sağlama hizmeti tarafından Pdee üzerinde gerçekleştirilen tüm eylemleri açıklar. Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md). Azure AD sağlama günlüklerini okumak için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
