---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için iProva yapılandırma | Microsoft Docs'
description: İProva 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamak üzere Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 4edba747-242d-4795-9539-649f33af4c13
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2019
ms.author: Zhchia
ms.openlocfilehash: bb730bad2837616aee0ebfa2da04015542782d9a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77057516"
---
# <a name="tutorial-configure-iprova-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için iProva yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları iProva 'e otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için iProva ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).
>
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* Bir Azure AD kiracısı
* [Bir iProva kiracısı](https://www.iProva.com/)
* Yönetici izinlerine sahip iProva içinde bir kullanıcı hesabı.

## <a name="assigning-users-to-iprova"></a>İProva 'e Kullanıcı atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanmış olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların iProva 'e erişmesi gerektiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları iProva 'e atayabilirsiniz:
* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-iprova"></a>İProva 'e Kullanıcı atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için iProva 'e tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı iProva 'e atarken atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="set-up-iprova-for-provisioning"></a>Sağlama için iProva ayarlama

1. [IProva Yönetici konsolunuza](https://www.iProva.com/)oturum açın. **> uygulama yönetimine**gidin.

    ![iProva Yönetici Konsolu](media/iprova-provisioning-tutorial/admin.png)

2.  **Dış Kullanıcı yönetimi**' ne tıklayın.

    ![iProva SCIM Ekle](media/iprova-provisioning-tutorial/external.png)

3. Yeni bir sağlayıcı eklemek için **artı** simgesine tıklayın. Yeni **Sağlayıcı Ekle** iletişim kutusunda bir **başlık**girin. **IP tabanlı erişim kısıtlaması**eklemeyi seçebilirsiniz. **Tamam** düğmesine tıklayın.

    ![iProva yeni Ekle](media/iprova-provisioning-tutorial/add.png)

    ![iProva sağlayıcı ekle](media/iprova-provisioning-tutorial/addprovider.png)

4.  **Kalıcı belirteç** düğmesine tıklayın. **Kalıcı belirteci** kopyalayın ve tek bir zaman görüntüleyebilmeniz için kaydedin. Bu değer, Azure portal iProva uygulamanızın sağlama sekmesindeki gizli belirteç alanına girilir.

    ![iProva belirteci oluştur](media/iprova-provisioning-tutorial/token.png)

## <a name="add-iprova-from-the-gallery"></a>Galeriden iProva ekleme

İProva 'i Azure AD ile otomatik Kullanıcı sağlaması için yapılandırmadan önce Azure AD Uygulama Galerisi 'nden yönetilen SaaS uygulamaları listenize iProva eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden iProva eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **iProva**yazın, sonuçlar panelinde **iProva** ' yi seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuç listesinde iProva](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-iprova"></a>İProva 'e otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak iProva içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

> [!TIP]
> Ayrıca, [IProva çoklu oturum açma öğreticisinde](https://docs.microsoft.com/azure/active-directory/saas-apps/iProva-tutorial)sunulan yönergeleri Izleyerek, IPROVA için SAML tabanlı çoklu oturum açmayı etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlayabilse de.

### <a name="to-configure-automatic-user-provisioning-for-iprova-in-azure-ad"></a>Azure AD 'de iProva için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **iProva**' yi seçin.

    ![Uygulamalar listesindeki iProva bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, `https://identitymanagement.services.iProva.nl/scim` **kiracı URL 'sini**girin. **Gizli belirteçte**daha önce alınan **kalıcı belirteç** değerini girin. Azure AD 'nin iProva 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, iProva hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’e tıklayın.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları iProva olarak eşitler**' ı seçin.

    ![iProva Kullanıcı eşlemeleri](media/iprova-provisioning-tutorial/usermappings.png)

9. **Öznitelik eşleme** bölümünde Azure AD 'den iProva 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için iProva içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![iProva Kullanıcı öznitelikleri](media/iprova-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde **Azure Active Directory grupları iProva olarak eşitler**' ı seçin.

    ![iProva grubu eşlemeleri](media/iprova-provisioning-tutorial/groupmappings.png)

11. **Öznitelik eşleme** bölümünde Azure AD 'den iProva 'e eşitlenen grup özniteliklerini gözden geçirin. **Eşlenen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için iProva içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![iProva grubu öznitelikleri](media/iprova-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. İProva için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içindeki istenen değerleri seçerek iProva için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. İProva üzerinde Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan, ilerlemeyi izlemek ve sağlama etkinliği raporunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

