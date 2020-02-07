---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı hazırlama için Tableau online 'ı yapılandırma | Microsoft Docs"
description: Tableau çevrimiçi olarak Kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0be9c435-f9a1-484d-8059-e578d5797d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fae770950810899f7c6583fa401110c3e85022b
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77064230"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için Tableau online 'ı yapılandırma

Bu öğreticide, Tableau çevrimiçi ve Azure Active Directory (Azure AD) ile Azure AD 'yi, kullanıcıları ve grupları otomatik olarak sağlamak ve yeniden sağlamak üzere yapılandırmak üzere yapılandırma adımları gösterilmektedir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti 'nin üzerine kurulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları öğrenmek için bkz. [Azure Active Directory ile hizmet olarak yazılım (SaaS) uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo şunları olduğunu varsayar:

*   Azure AD kiracısı.
*   [Tableau çevrimiçi Kiracı](https://www.tableau.com/).
*   Yönetici izinleriyle Tableau çevrimiçi kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, [Tableau çevrimiçi REST API](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm)'sini kullanır. Bu API, Tableau çevrimiçi geliştiricileri tarafından kullanılabilir.

## <a name="add-tableau-online-from-the-azure-marketplace"></a>Azure Marketi 'nden Tableau çevrimiçi ekleme
Tableau online 'ı Azure AD ile otomatik Kullanıcı sağlaması için yapılandırmadan önce, Azure Marketi 'nden yönetilen SaaS uygulamaları listenize Tableau Online ekleyin.

Market 'ten Tableau çevrimiçi olarak eklemek için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com)sol taraftaki gezinti bölmesinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory simgesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** ' yı seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Tableau online** girin ve sonuç panelinden **Tableau online** ' ı seçin. Uygulamayı eklemek için **Ekle**' yi seçin.

    ![Sonuçlar listesinde çevrimiçi Tableau](common/search-new-app.png)

## <a name="assign-users-to-tableau-online"></a>Kullanıcıları Tableau çevrimiçi olarak ata

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların veya grupların Tableau çevrimiçi erişimine ihtiyacı olduğuna karar verin. Bu kullanıcıları veya grupları Tableau online 'a atamak için, [bir kullanıcı veya grubu kurumsal uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)bölümündeki yönergeleri izleyin.

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Tableau çevrimiçi ortamda Kullanıcı atamaya yönelik önemli ipuçları

*   Otomatik Kullanıcı sağlama yapılandırmasını test etmek için Tableau online 'a tek bir Azure AD kullanıcısı atamanızı öneririz. Daha sonra ek kullanıcı veya grup atayabilirsiniz.

*   Tableau çevrimiçi olarak bir Kullanıcı atadığınızda atama iletişim kutusunda varsa uygulamaya özgü geçerli herhangi bir rolü seçin. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configure-automatic-user-provisioning-to-tableau-online"></a>Tableau çevrimiçi olarak otomatik Kullanıcı sağlamayı yapılandırma

Bu bölüm, Azure AD sağlama hizmetini yapılandırma adımlarında size rehberlik eder. Azure AD 'de Kullanıcı veya grup atamalarını temel alarak Tableau online 'daki kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için kullanın.

> [!TIP]
> Ayrıca, Tableau Online için SAML tabanlı çoklu oturum açmayı etkinleştirebilirsiniz. [Tableau çevrimiçi çoklu oturum açma öğreticisindeki](tableauonline-tutorial.md)yönergeleri izleyin. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlayabilse de.

### <a name="configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Azure AD 'de Tableau Online için otomatik Kullanıcı sağlamayı yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın. **Tableau çevrimiçi**olarak **tüm > uygulamalar** > **Kurumsal uygulamalar** ' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tableau online**' ı seçin.

    ![Uygulamalar listesindeki Tableau online bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Tableau çevrimiçi sağlama](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Tableau çevrimiçi sağlama modu](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. **Yönetici kimlik bilgileri** bölümünde etki alanı, Yönetici Kullanıcı adı, yönetici parolası ve Tableau çevrimiçi HESABıNıZıN içerik URL 'sini girin:

   * **Etki alanı** kutusunda, 6. adıma göre alt etki alanını girin.

   * **Yönetici Kullanıcı adı** kutusunda, Clarizen kiracınızdaki yönetici hesabının kullanıcı adını girin. admin@contoso.com bunun bir örneğidir.

   * **Yönetici parolası** kutusunda yönetici kullanıcı adına karşılık gelen yönetici hesabının parolasını girin.

   * **İçerik URL 'si** kutusunda, 6. adıma göre alt etki alanını girin.

6. Tableau Online için yönetici hesabınızda oturum açtıktan sonra, yönetici sayfasının URL 'sinden **etki alanı** ve **içerik URL** 'si değerlerini alabilirsiniz.

    * Tableau çevrimiçi hesabınızın **etki alanı** , URL 'nin bu bölümünden kopyalanabilir:

        ![Tableau çevrimiçi etki alanı](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * Tableau çevrimiçi hesabınızın **içerik URL 'si** bu bölümden kopyalanabilir. Hesap kurulumu sırasında tanımlanan bir değerdir. Bu örnekte, değer "contoso" ' dır:

        ![Tableau çevrimiçi Içerik URL 'SI](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > **Etki alanınız** burada gösterilenden farklı olabilir.

7. 5\. adımda gösterilen kutuları doldurduktan sonra Azure AD 'nin Tableau online 'a bağlanabildiğine emin olmak için **Bağlantıyı Sına** ' yı seçin. Bağlantı başarısız olursa, Tableau çevrimiçi hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Tableau çevrimiçi test bağlantısı](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. **Bildirim e-postası** kutusunda, sağlama hatası bildirimlerini alacak kişinin veya grubun e-posta adresini girin. **Bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Tableau çevrimiçi bildirim e-postası](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. **Kaydet**’i seçin.

10. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Tableau olarak eşitler**' ı seçin.

    ![Çevrimiçi Kullanıcı eşitlemesini Tableau](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. **Öznitelik eşlemeleri** bölümünde Azure AD 'Den Tableau online 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Tableau online 'daki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri kaydetmek için **Kaydet**' i seçin.

    ![Tableau online eşleşen Kullanıcı öznitelikleri](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. **Eşlemeler** bölümünde **Azure Active Directory grupları Tableau olarak eşitler**' ı seçin.

    ![Tableau çevrimiçi grup eşitleme](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. **Öznitelik eşlemeleri** bölümünde Azure AD 'Den Tableau online 'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Tableau online 'daki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri kaydetmek için **Kaydet**' i seçin.

    ![Tableau online eşleşen grup öznitelikleri](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Kapsam filtrelerini yapılandırmak için [kapsam filtresi öğreticisindeki](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)yönergeleri izleyin.

15. Tableau Online için Azure AD sağlama hizmetini etkinleştirmek üzere, **Ayarlar** bölümünde **sağlama durumunu** **Açık**olarak değiştirin.

    ![Tableau çevrimiçi sağlama durumu](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. Tableau çevrimiçi olarak sağlamak istediğiniz kullanıcıları veya grupları tanımlayın. **Ayarlar** bölümünde, **kapsam**içinde istediğiniz değerleri seçin.

    ![Tableau çevrimiçi kapsamı](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Tableau çevrimiçi kaydetme](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra eşitlenmesi daha uzun sürer. Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir gerçekleşir. 

İlerleme durumunu izlemek ve sağlama etkinliği raporunun bağlantılarını izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz. Rapor, Tableau online 'da Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini okuma hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
