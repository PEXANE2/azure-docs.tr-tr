---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için bir dijital kasa & Man Password Manager 'ı yapılandırın | Microsoft Docs"
description: Azure Active Directory yapılandırma hakkında bilgi edinmek için, Kullanıcı hesaplarını el ile parola Yöneticisi & dijital kasaya sağlamak ve devre dışı bırakmak üzere nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 05/07/2019
ms.author: jeedes
ms.openlocfilehash: a9ca77a059625ace13e6798e3fde84f11f3fe1db
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88546804"
---
# <a name="tutorial-configure-keeper-password-manager--digital-vault-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için bir dijital kasa & Man parola yöneticisini yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları bir Man Password Manager & dijital kasaya otomatik olarak sağlamak ve devre dışı bırakmak üzere, Azure AD 'yi yapılandırmak için, anahtar parola Yöneticisi & dijital kasa ve Azure Active Directory (Azure AD) ' de gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Bir Man parola Yöneticisi & dijital kasa kiracısı](https://keepersecurity.com/pricing.html?t=e)
* Man Password Manager 'daki bir kullanıcı hesabı, yönetici izinleriyle dijital kasa &.

## <a name="add-keeper-password-manager--digital-vault-from-the-gallery"></a>Galeriden bir dijital kasa & Man parola Yöneticisi ekleme

Azure AD ile otomatik Kullanıcı sağlama için, Man Password Manager & dijital kasasını yapılandırmadan önce, Azure AD uygulama galerisindeki bir Man parola Yöneticisi & dijital kasasını yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden bir Man parola Yöneticisi & dijital kasasını eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Man Password manager & dijital kasa**girin, sonuçlar panelinde bir **man parola Yöneticisi & dijital kasa** ' yı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Man parola Yöneticisi, sonuçlar listesinde dijital kasa &](common/search-new-app.png)

## <a name="assigning-users-to-keeper-password-manager--digital-vault"></a>Kullanıcıları, Man Password Manager & dijital kasaya atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların, Man parola Yöneticisi 'ne & dijital kasaya erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları% & dijital kasalarına atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-keeper-password-manager--digital-vault"></a>Kullanıcıları, Man parola Yöneticisi 'ne & dijital kasaya atamaya yönelik önemli ipuçları

* Tek bir Azure AD kullanıcısının, otomatik Kullanıcı sağlama yapılandırmasını test etmek için, bir Azure AD kullanıcısına & dijital kasasına atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Man parola Yöneticisi 'ne & dijital kasa 'ya atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configuring-automatic-user-provisioning-to-keeper-password-manager--digital-vault"></a>Dijital kasa & Man parola Yöneticisi 'ne otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak, Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir dijital kasa& daki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için adım adım kılavuzluk eder.

> [!TIP]
> Ayrıca, [Man Password manager & dijital kasa çoklu oturum açma öğreticisinde](keeperpasswordmanager-tutorial.md)sunulan yönergeleri Izleyerek, Man password Manager & dijital KASASıNDA SAML tabanlı çoklu oturum açmayı etkinleştirmeyi tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-keeper-password-manager--digital-vault-in-azure-ad"></a>Azure AD 'de, Man Password Manager & dijital kasasında otomatik Kullanıcı sağlamasını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Man Password Manager & dijital kasa**' yı seçin.

    ![Uygulamalar listesinde Man parola Yöneticisi & dijital kasa bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, 5. adımda açıklandığı gibi, dijital kasaınızın hesabı &, **kiracı URL 'sini** ve **gizli** anahtarını girin.

6. [Man Yönetici konsolunuza](https://keepersecurity.com/console/#login)oturum açın. **Yönetici** ' ye tıklayın ve var olan bir düğümü seçin ya da yeni bir tane oluşturun. **Sağlama** sekmesine gidin ve **Yöntem Ekle**' yi seçin.

    ![Man Yönetici Konsolu](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-admin-console.png)

    **SCIM (etki alanları arası kimlik yönetimi Için sistem**) seçeneğini belirleyin.

    ![Man, SCıM Ekle](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-add-scim.png)

    **Sağlama belirteci oluştur**' a tıklayın.

    ![Man uç noktası oluştur](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-create-endpoint.png)

    **URL** ve **belirteç** DEğERLERINI kopyalayın ve Azure AD 'de **kiracı URL 'sine** ve **gizli belirtece** yapıştırın. Daha açık sağlama kurulumunu gerçekleştirmek için **Kaydet** ' e tıklayın.

    ![Man oluşturma belirteci](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-create-token.png)

7. 5. adımda gösterilen alanları doldurarak Azure AD 'nin bir Man Password Manager & dijital kasaya bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Man parola Yöneticisi & dijital kasa hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

9. **Kaydet**’e tıklayın.

10. **Eşlemeler** bölümünde, **Kullanıcı Azure Active Directory Kullanıcıları, dijital kasa olarak &**

    ![Man Kullanıcı eşlemeleri](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-user-mappings.png)

11. **Öznitelik eşleme** bölümünde dijital kasa & Azure AD 'Den Man Password Manager ' a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için dijital kasadaki kullanıcı hesaplarını & eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Man Kullanıcı öznitelikleri](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-user-attributes.png)

12. **Eşlemeler** bölümünde, **Azure Active Directory gruplarını, anahtar parola Yöneticisi & dijital kasa olarak eşitler**' ı seçin.

    ![Man grubu eşlemeleri](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-group-mappings.png)

13. **Öznitelik eşleme** bölümünde dijital kasa & Azure AD 'Den Man Password Manager ' a eşitlenmiş grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için dijital kasa & Man Password Manager 'daki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Man grubu öznitelikleri](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-group-attributes.png)

14. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

15. Azure AD sağlama hizmeti ' ni Man Password Manager & dijital kasa için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

16. **Ayarlar** bölümünde **kapsam** içindeki istenen değerleri seçerek, daha fazla Man Password Manager & dijital kasasında sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

17. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. **Eşitleme ayrıntıları** bölümünü izlemek ve bakım parolası Yöneticisi & dijital kasasında Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan, sağlama etkinliği raporunu izlemek için kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Man Password Manager & dijital kasa **e-posta** ve **Kullanıcı adının** aynı kaynak değerine sahip olmasını gerektirir, ancak her iki öznitelik için de herhangi bir güncelleştirme diğer değeri değiştirir.
* Man parola Yöneticisi & dijital kasa Kullanıcı silmeleri desteklemez, yalnızca devre dışı bırakın. Devre dışı bırakılan kullanıcılar, Man yönetici konsolu Kullanıcı arabiriminde kilitli olarak görünür.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

