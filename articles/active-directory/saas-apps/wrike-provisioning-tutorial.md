---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Wrike yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarını otomatik olarak sağlamak ve serbest bırakmak için Azure Active Directory yapılandırma hakkında bilgi edinin.
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
ms.date: 08/26/2019
ms.author: Zhchia
ms.openlocfilehash: f94c12ed355b25e6a10c8d71e176d9bf3bf78758
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70847107"
---
# <a name="tutorial-configure-wrike-for-automatic-user-provisioning"></a>Öğretici: Otomatik Kullanıcı hazırlama için Wrike yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları yazma amacıyla otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için, Wrike ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Bir Wrike kiracısı](https://www.wrike.com/price/).
* İçindeki bir kullanıcı hesabı, yönetici izinleriyle yazıldı.

## <a name="assign-users-to-wrike"></a>Kullanıcıları Wrike 'a atama
Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen atama adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Wrike 'ye erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra, buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları Wrike 'ye atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-wrike"></a>Kullanıcıların Wrike 'a atanması için önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Wrike 'a atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı Wrike 'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="set-up-wrike-for-provisioning"></a>Sağlama için Wrike ayarlama

Azure AD ile otomatik Kullanıcı sağlaması için Wrike 'yi yapılandırmadan önce, Wrike üzerinde SCıM sağlamasını etkinleştirmeniz gerekir.

1. [Wrike Yönetici konsolunuza](https://www.Wrike.com/login/)oturum açın. Kiracı KIMLIĞINIZ ' ne gidin. **Tümleştirmeler & uygulamalar**' ı seçin.

    ![Wrike Yönetici Konsolu](media/Wrike-provisioning-tutorial/admin.png)

2.  **Azure AD**'ye gidin. Bunu seçin.

    ![Wrike Yönetici Konsolu](media/Wrike-provisioning-tutorial/Capture01.png)

3.  SCıM 'yi seçin. **Temel URL 'yi**kopyalayın.

    ![Wrike Yönetici Konsolu](media/Wrike-provisioning-tutorial/Wrike-tenanturl.png)

4. **API** ' ye tıklayın > **Azure SCIM**' i seçin.

    ![Wrike Add SCıM](media/Wrike-provisioning-tutorial/Wrike-add-scim.png)

5.  Açılır pencere açılır. Hesap oluşturmak için daha önce oluşturduğunuz parolayı girin 

    ![Wrike oluşturma belirteci](media/Wrike-provisioning-tutorial/password.png)

6.  **Gizli belirteç** kopyalayın ve Azure AD 'ye yapıştırın. Wrike üzerinde sağlama kurulumunu gerçekleştirmek için **Kaydet** ' e tıklayın.

    ![Wrike oluşturma belirteci](media/Wrike-provisioning-tutorial/Wrike-create-token.png)


## <a name="add-wrike-from-the-gallery"></a>Galeriden Wrike ekleme

Azure AD ile otomatik Kullanıcı sağlaması için Wrike 'yi yapılandırmadan önce, Azure AD uygulama galerisindeki yönetilen SaaS uygulamaları listenize Wrike eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden Wrike eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Wrike**yazın, sonuçlar panelinde **Wrike** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuçlar listesinde Wrike](common/search-new-app.png)


## <a name="configuring-automatic-user-provisioning-to-wrike"></a>Otomatik Kullanıcı sağlamayı Wrike için yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak Kullanıcı ve/veya Grup atamaları ile yazma işlemlerinde kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için adım adım yönergeler sağlanır.

> [!TIP]
> Aynı zamanda Wrike için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi tercih edebilirsiniz. Bu arada, [yazma çoklu oturum açma öğreticisinde](wrike-tutorial.md)belirtilen yönergeleri izleyin. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlayabilse de.

### <a name="to-configure-automatic-user-provisioning-for-wrike-in-azure-ad"></a>Azure AD 'de Wrike için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Wrike**öğesini seçin.

    ![Uygulamalar listesindeki Wrike bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici kimlik bilgileri bölümünde, sırasıyla **kiracı URL 'si** ve **gizli belirteç** içinde alınan **temel URL** 'yi ve **kalıcı erişim belirteci** değerlerini girin. Azure AD 'nin Wrike 'ya bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Wrike hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

7. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

8. **Kaydet**’e tıklayın.

9. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Wrike olarak eşitler**' ı seçin.

    ![Wrike Kullanıcı eşlemeleri](media/Wrike-provisioning-tutorial/Wrike-user-mappings.png)

10. **Öznitelik eşleme** bölümünde Azure AD 'Den Wrike 'ye eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Wrike içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![Wrike Kullanıcı öznitelikleri](media/Wrike-provisioning-tutorial/Wrike-user-attributes.png)

11. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

12. Wrike için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **kapsam** içindeki Istenen değerleri seçerek, yazma için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

14. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin sonraki eşitlemeler daha uzun sürer. Kullanıcıların ve/veya grupların sağlaması için ne kadar süreceğine ilişkin daha fazla bilgi için bkz. [kullanıcıları sağlamak için ne kadar sürer](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users).

İlerleme durumunu izlemek için **geçerli durum** bölümünü kullanabilir ve Azure AD sağlama hizmeti tarafından, Wrike üzerinde gerçekleştirilen tüm eylemleri açıklayan sağlama etkinliği raporunuzun bağlantılarını izleyebilirsiniz. Daha fazla bilgi için bkz. [Kullanıcı hazırlama durumunu denetleme](../manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md). Azure AD sağlama günlüklerini okumak için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
