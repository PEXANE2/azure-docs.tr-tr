---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Samanage'i yapılandırın | Microsoft Dokümanlar"
description: Samanage'e kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı etmek için Azure Active Directory'yi nasıl yapılandırıştırmayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060533"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Samanage'i yapılandırın

Bu öğretici, Azure AD'yi kullanıcıları ve grupları Samanage'e otomatik olarak sağlamak ve devre dışı etmek üzere yapılandırmak için Samanage ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirecek adımları gösterir.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında bilgi için [bkz.](../app-provisioning/user-provisioning.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdakilere sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* Profesyonel paketi ile bir [Samanage kiracı.](https://www.samanage.com/pricing/)
* Yönetici izinleri olan Samanage'de bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi [Samanage Rest API'ye](https://www.samanage.com/api/)dayanır. Bu API, Profesyonel pakete sahip hesaplar için Samanage geliştiricileri tarafından kullanılabilir.

## <a name="add-samanage-from-the-azure-marketplace"></a>Azure Marketinden Samanage Ekle

Azure AD ile otomatik kullanıcı sağlama için Samanage'i yapılandırmadan önce, Samanage'i Azure Marketi'nden yönetilen SaaS uygulamaları listenize ekleyin.

Market'ten Samanage eklemek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com)soldaki gezinti bölmesinde **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin simgesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Samanage** girin ve sonuç panelinden **Samanage'i** seçin. Uygulamayı eklemek için **Ekle'yi**seçin.

    ![Sonuç listesinde samanage](common/search-new-app.png)

## <a name="assign-users-to-samanage"></a>Kullanıcıları Samanage'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Samanage'e erişmeye ihtiyacı olduğuna karar verin. Bu kullanıcıları veya grupları Samanage'e atamak [için, bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)yönergelerini izleyin.

### <a name="important-tips-for-assigning-users-to-samanage"></a>Samanage'e kullanıcı atamak için önemli ipuçları

*    Bugün, Samanage rolleri Azure portalı ui'sinde otomatik olarak ve dinamik olarak doldurulur. Kullanıcılara Samanage rollerini atamadan önce, Samanage kiracınızdaki en son rolleri almak için Samanage'e karşı ilk eşitlemenin tamamlandığından emin olun.

*    İlk otomatik kullanıcı sağlama yapılandırmanızı test etmek için Samanage'e tek bir Azure REKLAM kullanıcısı atamanızı öneririz. Testler başarılı olduktan sonra ek kullanıcılar ve gruplar atayabilirsiniz.

*    Bir kullanıcıyı Samanage'e atadığınızda, atama iletişim kutusunda varsa uygulamaya özgü geçerli bir rol seçin. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configure-automatic-user-provisioning-to-samanage"></a>Otomatik kullanıcı sağlamayı Samanage'e yapılandırma

Bu bölüm, Azure AD sağlama hizmetini yapılandırma adımlarında size yol gösteriş eder. Azure AD'deki kullanıcı veya grup atamalarına dayalı olarak Samanage'de kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı kalmak için kullanın.

> [!TIP]
> Ayrıca Samanage için SAML tabanlı tek oturum açmayı da etkinleştirebilirsiniz. [Samanage tek oturum açma öğreticisindeki](samanage-tutorial.md)talimatları izleyin. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Azure AD'de Samanage için otomatik kullanıcı sağlama yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları** > Seçin**Tüm uygulamalar** > **Samanage**.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Samanage'i**seçin.

    ![Uygulamalar listesindeki Samanage bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Samanage Sağlama](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü altında, Samanage **Kiracı URL'nizi** ve **Gizli Belirteç'inizi girdiniz.** Azure AD'nin Samanage'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Samanage hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Samanage Test Bağlantısı](./media/samanage-provisioning-tutorial/provisioning.png)

6. Bildirim **E-posta** kutusuna, sağlama hatası bildirimlerini almak için kişinin veya grubun e-posta adresini girin. Bir **hata olduğunda e-posta gönder bildirimini** seçin onay kutusunu seçin.

    ![Samanage Bildirim E-postası](./media/samanage-provisioning-tutorial/EmailNotification.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Samanage için Azure Etkin Dizin Kullanıcılarını Senkronize Et'i**seçin.

    ![Samanage kullanıcı senkronizasyonu](./media/samanage-provisioning-tutorial/UserMappings.png)

9. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Samanage'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Samanage'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Samanage eşleşen kullanıcı öznitelikleri](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

10. Grup eşlemelerini etkinleştirmek için, **Eşlemeler** bölümü altında, **Samanage için Azure Etkin Dizin Gruplarını Eşitle'yi**seçin.

    ![Samanage grup senkronizasyonu](./media/samanage-provisioning-tutorial/GroupMappings.png)

11. Grupları eşitlemek için **Etkin'den** **Evet'e** ayarlayın. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Samanage'e eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Samanage'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Samanage eşleşen grup öznitelikleri](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Kapsam filtrelerini yapılandırmak [için, kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)öğreticisindeki yönergeleri izleyin.

13. **Ayarlar** bölümünde, Samanage için Azure AD sağlama hizmetini etkinleştirmek **için, Sağlama Durumunu** **A'da**olarak değiştirin.

    ![Samanage Sağlama Durumu](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

14. Samanage'e sağlamak istediğiniz kullanıcıları veya grupları tanımlayın. **Ayarlar** bölümünde, **Kapsam'ta**istediğiniz değerleri seçin. Tüm kullanıcıları **ve grupları eşitle** seçeneğini seçtiğinizde, aşağıdaki "Bağlayıcı sınırlamaları" bölümünde açıklandığı gibi sınırlamaları göz önünde bulundurun.

    ![Samanage Kapsamı](./media/samanage-provisioning-tutorial/ScopeSync.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Samanage Kaydet](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilemi daha sonraki eşitlemelerden daha uzun sürüyor. Azure AD sağlama hizmeti çalıştığı sürece yaklaşık her 40 dakikada bir oluşur. 

İlerlemeyi izlemek ve sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz. Rapor, Azure AD sağlama hizmeti tarafından Samanage'de gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

Tüm kullanıcıları **ve grupları eşitle** seçeneğini seçip Samanage **rolleri** özniteliği için bir değer yapılandırıyorsanız, **null (isteğe bağlı)** kutusunun altındaki değer aşağıdaki biçimde ifade edilmelidir:

- {"displayName":"role"}, rolüistediğiniz varsayılan değerdir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
