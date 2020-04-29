---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için BlueJeans yapılandırma | Microsoft Docs'
description: Kullanıcı hesaplarının şemasını otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fcb3fe009a6482c8e512899a952694beaed361a7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77059112"
---
# <a name="tutorial-configure-bluejeans-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için BlueJeans yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları otomatik olarak sağlamak ve devre dışı bırakmak üzere yapılandırmak için şema ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti ' nin üzerine oluşturulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdakilere sahip olduğunuzu varsayar:

* Bir Azure AD kiracısı
* [Şirketimizin](https://www.BlueJeans.com/pricing) veya daha iyi etkin olan bir BlueJeans kiracısı
* Yönetici izinlerine sahip şema içindeki bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirmesi, standart plan veya daha iyi bir şekilde BlueJeans ekipleri tarafından kullanılabilen [BLUEJEANS API](https://BlueJeans.github.io/developer)'sini kullanır.

## <a name="adding-bluejeans-from-the-gallery"></a>Galerinin şeması ekleme

Azure AD ile otomatik Kullanıcı sağlama için şema yapılandırmadan önce Azure AD uygulama galerisindeki BlueJeans 'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD Uygulama Galerisi 'nden şema eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **şeması**girin, sonuçlar panelinde **BlueJeans** ' ı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesini seçin.

    ![Sonuçlar listesindeki şema](common/search-new-app.png)

## <a name="assigning-users-to-bluejeans"></a>Kullanıcıları BlueJeans 'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların BlueJeans 'a erişmesi gerektiğine karar vermeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları ve/veya grupları şema için atayabilirsiniz:

* [Kurumsal uygulamaya Kullanıcı veya Grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-bluejeans"></a>Kullanıcıları Bluejeve kullanıcılara atamaya yönelik önemli ipuçları

* Otomatik Kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının BlueJeans 'a atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

* Bir kullanıcıyı şema için atarken, atama iletişim kutusunda uygulamaya özgü geçerli herhangi bir rolü (varsa) seçmeniz gerekir. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configuring-automatic-user-provisioning-to-bluejeans"></a>Şeması otomatik Kullanıcı sağlamayı yapılandırma

Bu bölümde Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan şema içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımları adım adım kılavuzluk eder.

> [!TIP]
> Şema tabanlı tekli oturum açmayı, [BlueJeans çoklu oturum açma öğreticisinde](bluejeans-tutorial.md)belirtilen talimatları izleyerek etkinleştirmeyi de tercih edebilirsiniz. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-automatic-user-provisioning-for-bluejeans-in-azure-ad"></a>Azure AD 'de BlueJeans için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın ve **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **BlueJeans**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **BlueJeans**' ı seçin.

    ![Uygulamalar listesindeki BlueJeans bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningTab.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/Bluejeans1.png)

5. **Yönetici kimlik bilgileri** bölümü altında, **Yönetici Kullanıcı adı**' nı ve BlueJeans hesabınızın **yönetici parolasını** girin. Bu değerlere örnek olarak şunlar verilebilir:

   * **Yönetici Kullanıcı adı** alanında yönetici hesabının kullanıcı adını BlueJeans kiracınızda doldurun. Örnek: admin@contoso.com.

   * **Yönetici parolası** alanında, yönetici kullanıcı adına karşılık gelen parolayı doldurun.

6. Adım 5 ' te gösterilen alanlar doldurulmaya göre, Azure AD 'nin Bluejefıler 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, BlueJeans hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/BluejeansTestConnection.png)

7. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/BluejeansNotificationEmail.png)

8. **Kaydet**’e tıklayın.

9. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Bluelerile eşitler**' ı seçin.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/BluejeansMapping.png)

10. **Öznitelik eşleme** bölümünde Azure AD 'Den BlueJeans 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için şema içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/BluejeansUserMappingAtrributes.png)

11. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

12. Şema için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/BluejeansProvisioningStatus.png)

13. **Ayarlar** bölümünde **kapsam** içindeki istenen değerleri seçerek şema için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/UserGroupSelection.png)

14. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![BlueJeans sağlama](./media/bluejeans-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer. **Eşitleme ayrıntıları** bölümünü kullanarak ilerlemeyi Izleyebilir ve Azure AD sağlama hizmeti tarafından, şema üzerinde gerçekleştirilen tüm eylemleri açıklayan, sağlama etkinlik raporuna ilişkin bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Şema, 30 karakterden uzun kullanıcı adlarıyla izin vermez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->

[1]: ./media/bluejeans-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
