---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Samanage 'ı yapılandırma | Microsoft Docs"
description: Azure Active Directory yapılandırma hakkında bilgi edinmek için Kullanıcı hesaplarını otomatik olarak sağlama ve sağlamayı kaldırma hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 62d0392f-37d4-436e-9aff-22f4e5b83623
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 988efc2087b3b30e6073bd7f6e2cf08f91fd397c
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77060533"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için Samanage 'ı yapılandırma

Bu öğreticide, Azure AD 'yi otomatik olarak sağlamak ve Kullanıcı ve grupları Sayönetilecek olarak sağlamak üzere yapılandırmak için, Samanage ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımlar gösterilmektedir.

> [!NOTE]
> Bu öğreticide, Azure AD Kullanıcı sağlama hizmeti 'nin üzerine kurulmuş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları öğrenmek için bkz. [Azure Active Directory ile hizmet olarak yazılım (SaaS) uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo şunları olduğunu varsayar:

* Azure AD kiracısı.
* Profesyonel paketiyle bir [Samanage kiracısı](https://www.samanage.com/pricing/) .
* Yönetici izinleriyle Samanage Kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, [Samanage REST API](https://www.samanage.com/api/)'sini kullanır. Bu API, profesyonel paketi olan hesaplara yönelik geliştiricilerin Sayönetilebilmesi için kullanılabilir.

## <a name="add-samanage-from-the-azure-marketplace"></a>Azure Marketi 'nden Samanage ekleyin

Azure AD ile otomatik Kullanıcı sağlama için Samanage 'ı yapılandırmadan önce, Azure Marketi 'nden yönetilen SaaS uygulamaları listenize Samanage ekleyin.

Market 'ten Samanage eklemek için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com)sol taraftaki gezinti bölmesinde **Azure Active Directory**' i seçin.

    ![Azure Active Directory simgesi](common/select-azuread.png)

2. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** ' yı seçin.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Samanage** yazın ve sonuç panelinden **Samanage** ' ı seçin. Uygulamayı eklemek için **Ekle**' yi seçin.

    ![Sonuçlar listesinde Samanage](common/search-new-app.png)

## <a name="assign-users-to-samanage"></a>Kullanıcıları Samanage 'a atama

Azure Active Directory seçili uygulamalara hangi kullanıcıların erişimi alacağını belirleyen *atama* adı verilen bir kavram kullanır. Otomatik Kullanıcı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik Kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların veya grupların Samanage 'a erişmesi gerektiğine karar verin. Bu kullanıcıları veya grupları Samanage 'a atamak için, [bir kullanıcı veya grubu bir kurumsal uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)' daki yönergeleri izleyin.

### <a name="important-tips-for-assigning-users-to-samanage"></a>Kullanıcıları Samanage 'a atamaya yönelik önemli ipuçları

*    Günümüzde, Samanage rolleri, Azure portal Kullanıcı arabiriminde otomatik olarak ve dinamik olarak doldurulur. Kullanıcılara Samanage rolleri atamadan önce, Samanage kiracınızdaki en son rolleri almak için bir ilk eşitlemenin, Samanage 'e göre tamamlandığından emin olun.

*    İlk otomatik Kullanıcı sağlama yapılandırmanızı test etmek için tek bir Azure AD kullanıcısını Samanage 'e atamanızı öneririz. Testler başarılı olduktan sonra daha sonra ek kullanıcılar ve gruplar atayabilirsiniz.

*    Bir kullanıcıyı Samanage 'a atadığınızda atama iletişim kutusunda varsa uygulamaya özgü geçerli herhangi bir rolü seçin. **Varsayılan erişim** rolüne sahip kullanıcılar, sağlanmasından çıkarılır.

## <a name="configure-automatic-user-provisioning-to-samanage"></a>Samanage otomatik Kullanıcı sağlamasını yapılandırma

Bu bölüm, Azure AD sağlama hizmetini yapılandırma adımlarında size rehberlik eder. Azure AD 'de Kullanıcı veya grup atamalarına göre Samanage kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için kullanın.

> [!TIP]
> Ayrıca, Samanage için SAML tabanlı çoklu oturum açmayı etkinleştirebilirsiniz. [Samanage çoklu oturum açma öğreticisindeki](samanage-tutorial.md)yönergeleri izleyin. Çoklu oturum açma, otomatik Kullanıcı sağlamasından bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlayabilse de.

### <a name="configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Azure AD 'de Samanage otomatik Kullanıcı sağlamasını yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal uygulamaları** > **tüm uygulamalar** > **sayönet**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Sayönet**' i seçin.

    ![Uygulamalar listesindeki Samanage bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Samanage sağlama](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, Samanage **kiracı URL** 'nizi ve **gizli belirtecinizi**girin. Azure AD 'nin Samanage 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Samanage hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Test bağlantısını sayönet](./media/samanage-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** kutusunda, sağlama hatası bildirimlerini alacak kişinin veya grubun e-posta adresini girin. **Bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postasını sayönet](./media/samanage-provisioning-tutorial/EmailNotification.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları sayönet**' i seçin.

    ![Kullanıcı eşitlemesini sayönet](./media/samanage-provisioning-tutorial/UserMappings.png)

9. **Öznitelik eşlemeleri** bölümünde Azure AD 'Den Samanage ile eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemlerinde Samanage Kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri kaydetmek için **Kaydet**' i seçin.

    ![Eşleşen kullanıcı özniteliklerini sayönet](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

10. Grup eşlemelerini etkinleştirmek için, **eşlemeler** bölümünde, **Azure Active Directory gruplarını Samanage olarak eşitler**' ı seçin.

    ![Grup eşitlemesini sayönet](./media/samanage-provisioning-tutorial/GroupMappings.png)

11. Grupları eşleştirmek için **Enabled** ayarını **Evet** olarak ayarlayın. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den Samanage ile eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemlerinde Samanage Kullanıcı hesaplarıyla eşleştirmek için kullanılır. Değişiklikleri kaydetmek için **Kaydet**' i seçin.

    ![Eşleşen grup özniteliklerini sayönet](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Kapsam filtrelerini yapılandırmak için [kapsam filtresi öğreticisindeki](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)yönergeleri izleyin.

13. Azure AD sağlama hizmeti 'ni Samanage için etkinleştirmek üzere **Ayarlar** bölümünde, **sağlama durumunu** **Açık**olarak değiştirin.

    ![Samanage sağlama durumu](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

14. Samanage için sağlamak istediğiniz kullanıcıları veya grupları tanımlayın. **Ayarlar** bölümünde, **kapsam**içinde istediğiniz değerleri seçin. **Tüm kullanıcıları ve grupları Eşitle** seçeneğini belirlediğinizde, aşağıdaki "bağlayıcı sınırlamaları" bölümünde açıklanan sınırlamaları göz önünde bulundurun.

    ![Samanage kapsamı](./media/samanage-provisioning-tutorial/ScopeSync.png)

15. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Sakla](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra eşitlenmesi daha uzun sürer. Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir gerçekleşir. 

İlerleme durumunu izlemek ve sağlama etkinliği raporunun bağlantılarını izlemek için **eşitleme ayrıntıları** bölümünü kullanabilirsiniz. Rapor, Azure AD sağlama hizmeti tarafından, Samanage üzerinde gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini okuma hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlama hakkında raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

**Tüm kullanıcıları ve grupları Eşitle** seçeneğini belirleyin ve sayönet **Roller** özniteliği için bir değer yapılandırırsanız, **null (isteğe bağlı) kutusunda varsayılan değer** altındaki değer aşağıdaki biçimde ifade alınmalıdır:

- {"displayName": "role"}, burada rol istediğiniz varsayılan değerdir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
