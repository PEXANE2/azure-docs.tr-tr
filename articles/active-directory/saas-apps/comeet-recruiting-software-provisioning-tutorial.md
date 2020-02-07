---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için birlikte bulunan Işe alma yazılımlarını yapılandırın | Microsoft Docs'
description: Işe alma yazılımlarını birlikte karşılamak üzere Kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: jeedes
ms.openlocfilehash: f427fb75cfaeda79b037c327992e4ad482a7e689
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77058348"
---
# <a name="tutorial-configure-comeet-recruiting-software-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için birlikte bulunan Işe alma yazılımlarını yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, Kullanıcı ve/veya grupların Işe alma yazılımlarını karşılamak üzere otomatik olarak sağlaması ve sağlaması için otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak üzere, sağlama yazılımı ve Azure Active Directory (Azure AD) ile birlikte gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Cokarşılayan bir Işe alma yazılım kiracısı](https://www.comeet.co/)
* Yönetici izinleriyle Işbirliği yazılımlarını içeren bir kullanıcı hesabı.

## <a name="add-comeet-recruiting-software-from-the-gallery"></a>Galeriden ortak bir Işe alma yazılımı ekleyin

Azure AD ile otomatik Kullanıcı sağlama için birlikte bulunan Işe alma yazılımlarını yapılandırmadan önce, Azure AD uygulama galerisindeki yönetilen SaaS uygulamaları listenize Cokarşılayan Işe alma yazılımını eklemeniz gerekir.

**Azure AD uygulama galerisinden ortak bir Işe alma yazılımı eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **birlikte bulunan Işe alma yazılımını**girin, sonuçlar panelinde **ortak işe alma yazılımını** seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuç listesinde işbirliği yazılımlarını birlikte karşılayın](common/search-new-app.png)

## <a name="assigning-users-to-comeet-recruiting-software"></a>Kullanıcıları, Işe alma yazılımına birlikte buluşmak üzere atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Işe alma yazılımlarını birlikte karşılaması için erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları, Işe alma yazılımlarını birlikte karşılamak üzere atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-comeet-recruiting-software"></a>Işe alma yazılımlarını birlikte karşılamak için Kullanıcı atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek üzere Işe alma yazılımının birlikte sağlanması için tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı, Işe alma yazılımına birlikte buluşmak üzere atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configuring-automatic-user-provisioning-to-comeet-recruiting-software"></a>Işe alma yazılımının birlikte sağlanması için otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmetini kullanarak Kullanıcı ve/veya grup atamalarını Azure AD 'de Kullanıcı ve/veya grup atamalarına göre yapılandırma, güncelleştirme ve devre dışı bırakma adımlarını adım adım kılavuzluk eder.

> [!TIP]
> Ayrıca, işbirliği yazılımı [Çoklu oturum açma öğreticisinde](comeetrecruitingsoftware-tutorial.md)yer alan yönergeleri Izleyerek, işbirliği yazılımı için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-comeet-recruiting-software-in-azure-ad"></a>Azure AD 'de birlikte bulunan Işe alma yazılımının otomatik Kullanıcı sağlamasını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Işbirliği yazılımlarını birlikte karşılayın**' i seçin.

    ![Uygulamalar listesindeki ortak Işe alma yazılımı bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, adım 6 ' da açıklandığı şekilde, **kiracı URL 'Sini** ve cokarşılayan Işe alma yazılımınızın hesabının **gizli belirtecini** girin.

6. [Işbirliği yazılım yönetim konsolu 'nu birlikte karşılayın](https://app.comeet.co/) **> ayarları > kimlik doğrulama > Microsoft Azure**ve **Şirket değeri Için gızlı belirteç** ' nı Azure AD 'deki **gizli belirteç** alanına kopyalayın.

    ![Cokarşılar Işe alma yazılımı sağlama](./media/comeet-recruiting-software-provisioning-tutorial/secret-token-1.png)

7. Adım 5 ' te gösterilen alanlar doldurulmaya yardımcı olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Cokarşılayan Işe alma yazılım hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-token.png)

8. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

9. **Kaydet** düğmesine tıklayın.

10. **Eşlemeler** bölümünde, **Kullanıcı Azure Active Directory Kullanıcıları birlikte karşılamaları için Synchronize**' ı seçin.

    ![Cokarşılar Işe alma yazılımı Kullanıcı eşlemeleri](media/comeet-recruiting-software-provisioning-tutorial/user-mappings.png)

11. Azure AD 'den eşitlenen Kullanıcı özniteliklerini, **öznitelik eşleme** bölümünde Işe alma yazılımının Işbirliği yazılımlarını karşılamak için gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için birlikte bulunan Işe alma yazılımının kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Cokarşılar Işe alma yazılım grubu öznitelikleri](media/comeet-recruiting-software-provisioning-tutorial/user-mapping-attributes.png)

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Azure AD sağlama hizmetini Cokarşılayan Işe alma yazılımı için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki Istenen değerleri seçerek Işe alma yazılımlarını birlikte karşılamak için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İlerlemeyi izlemek ve sağlama için Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik raporuna yönelik bağlantıları izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Cokarşılayan Işe alma yazılımı şu anda grupları desteklemiyor.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

