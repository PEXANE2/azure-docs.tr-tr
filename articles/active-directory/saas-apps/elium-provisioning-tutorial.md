---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Elium yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırma hakkında bilgi edinin.
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
ms.openlocfilehash: 30f6f81234ce34e40a29a04f6b795aab991a8824
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70802868"
---
# <a name="tutorial-configure-elium-for-automatic-user-provisioning"></a>Öğretici: Otomatik Kullanıcı sağlaması için Elium yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları uyumlu olmayan şekilde otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için, Elium ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Azure AD kiracısı.
* [Elium kiracısı](https://www.elium.com/pricing/)
* Yönetici izinlerine sahip bir kullanıcı hesabı.

## <a name="assigning-users-to-elium"></a>Kullanıcıları Elium 'a atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Elium 'a erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları elime 'ye atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-elium"></a>Kullanıcıları Elium 'a atamaya yönelik önemli ipuçları 

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Elium 'a atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı elim 'e atarken atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="set-up-elium-for-provisioning"></a>Sağlama için Elium ayarlama

Azure AD ile otomatik Kullanıcı sağlama için Elium 'u yapılandırmadan önce, korum üzerinde SCIM sağlamayı etkinleştirmeniz gerekir.

1. Elium 'da oturum açın. **Profil** > **ayarlarıma**gidin.

    ![Elium](media/Elium-provisioning-tutorial/setting.png)

2. GELIŞMIŞ ' ın altında sol alt köşedeki **güvenlik**' i seçin.

    ![Elium](media/Elium-provisioning-tutorial/security.png)

3. Gizli dizi **belirtecini**kopyalayın. Bu değer, Azure portal Elium uygulamanızın sağlama sekmesindeki **gizli belirteç** alanına girilir.

    ![Elium](media/Elium-provisioning-tutorial/token.png)


## <a name="add-elium--from-the-gallery"></a>Galeriden Elium ekleyin

Azure AD ile otomatik Kullanıcı sağlaması için Elium 'u yapılandırmak için, Azure AD uygulama galerisindeki yönetilen SaaS uygulamaları listenize Elium eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden bir adım daha eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **elium**yazın, sonuçlar panelinde **elium** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuçlar listesinde elium](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-elium"></a>Otomatik Kullanıcı sağlamayı Elium 'a yapılandırma  

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak Kullanıcı ve/veya grupları Azure AD 'de Kullanıcı ve/veya grup atamalarına göre ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımları adım adım kılavuzluk eder.

> [!TIP]
> Ayrıca, [Elium çoklu oturum açma öğreticisinde](Elium-tutorial.md)sunulan yönergeleri izleyerek, tam olarak, SAML tabanlı çoklu oturum açmayı etkinleştirmeyi seçebilirsiniz. Çoklu oturum açma otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilse de

### <a name="to-configure-automatic-user-provisioning-for-elium--in-azure-ad"></a>Azure AD 'de Elium için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Elium**' u seçin.

    ![Uygulamalar listesindeki Elium bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici kimlik bilgileri bölümünün altında, `<tenantURL>/scim/v2` **{teananturl}** öğesinin, daha önce elium yönetim konsolundan alınan değer olduğunu girin. Gizli belirteçte **gizli belirteç** değerini gırın Azure AD 'Nin elium 'a bağlanabildiğinden emin olmak için **test bağlantısı** **' na** tıklayın. Bağlantı başarısız olursa, elim 'nizin yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları uyumum olarak eşitler**' ı seçin.

    ![Kullanıcı eşlemeleri](media/Elium-provisioning-tutorial/usermapping.png)

9. Azure AD 'den eşitlenen Kullanıcı özniteliklerini **öznitelik eşleme** bölümünde elium 'a gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için elium 'daki Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Elium Kullanıcı öznitelikleri](media/Elium-provisioning-tutorial/userattribute.png)


11. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

12. Azure AD sağlama hizmetini Elium için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **kapsam** içindeki istenen değerleri seçerek, bir bütün olarak sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

14. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin sonraki eşitlemeler daha uzun sürer. Kullanıcıların ve/veya grupların sağlaması için ne kadar süreceğine ilişkin daha fazla bilgi için bkz. [kullanıcıları sağlamak için ne kadar sürer](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users).

İlerleme durumunu izlemek için **geçerli durum** bölümünü kullanabilir ve Azure AD sağlama hizmeti tarafından, elium üzerinde gerçekleştirilen tüm eylemleri açıklayan sağlama etkinliği raporunuzun bağlantılarını izleyebilirsiniz. Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md). Azure AD sağlama günlüklerini okumak için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md).
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
