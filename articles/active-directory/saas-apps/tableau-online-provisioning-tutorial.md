---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Tableau Online'ı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Tableau Online'a otomatik olarak sağlamak ve yok etmek için nasıl yapılandırılamayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064230"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Tableau Online'ı yapılandırın

Bu öğretici, Azure AD'yi kullanıcıları ve grupları Tableau Online'a otomatik olarak sağlamak ve etkisiz duruma getirmek üzere yapılandırmak için Tableau Online ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirecek adımları gösterir.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında bilgi için [bkz.](../app-provisioning/user-provisioning.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdakilere sahip olduğunuzu varsayar:

*   Azure AD kiracısı.
*   Bir [Tableau Online kiracı](https://www.tableau.com/).
*   Tableau Online'da yönetici izinleri olan bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi [Tableau Çevrimiçi Dinlenme API'sına](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm)dayanır. Bu API Tableau Online geliştiricileri tarafından kullanılabilir.

## <a name="add-tableau-online-from-the-azure-marketplace"></a>Azure Marketinden Tableau Online Ekle
Azure AD ile otomatik kullanıcı sağlama için Tableau Online'ı yapılandırmadan önce, Yönetilen SaaS uygulamaları listenize Azure Marketi'nden Tableau Online'ı ekleyin.

Marketten Tableau Online eklemek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com)soldaki gezinti bölmesinde **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin simgesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için iletişim kutusunun üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Tableau Online'ı** girin ve sonuç panelinden **Tableau Online'ı** seçin. Uygulamayı eklemek için **Ekle'yi**seçin.

    ![Sonuç listesinde Tablo Online](common/search-new-app.png)

## <a name="assign-users-to-tableau-online"></a>Kullanıcıları Tableau Online'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Tableau Online'a erişmeye ihtiyacı olduğuna karar verin. Bu kullanıcıları veya grupları Tableau Online'a atamak için, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)yönergelerini izleyin.

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Tableau Online'a kullanıcı atamak için önemli ipuçları

*   Otomatik kullanıcı sağlama yapılandırmasını test etmek için Tableau Online'a tek bir Azure AD kullanıcısı atamanızı öneririz. Daha sonra ek kullanıcılar veya gruplar atayabilirsiniz.

*   Bir kullanıcıyı Tableau Online'a atadığınızda, atama iletişim kutusunda, varsa uygulamaya özgü geçerli bir rolü seçin. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configure-automatic-user-provisioning-to-tableau-online"></a>Otomatik kullanıcı sağlamayı Tableau Online olarak yapılandırın

Bu bölüm, Azure AD sağlama hizmetini yapılandırma adımlarında size yol gösteriş eder. Azure AD'deki kullanıcı veya grup atamalarına dayalı olarak Tableau Online'da kullanıcıları veya grupları oluşturmak, güncelleştirmek ve devre dışı kalmak için kullanın.

> [!TIP]
> Ayrıca Tableau Online için SAML tabanlı tek oturum açmayı da etkinleştirebilirsiniz. [Tableau Online tek oturum açma](tableauonline-tutorial.md)öğreticisindeki talimatları izleyin. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Azure AD'de Tableau Online için otomatik kullanıcı sağlama yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar** > **Tableau Online**.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tableau Online'ı**seçin.

    ![Uygulamalar listesindeki Tableau Online bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Tableau Online Tedarik](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Tableau Online Tedarik Modu](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. Yönetici **Kimlik Bilgileri** bölümü ne göre, Tableau Online hesabınızın etki alanını, yönetici kullanıcı adını, yönetici parolasını ve içerik URL'sini girin:

   * Etki **Alanı** kutusunda, Adım 6'yı temel alan alt etki alanını doldurun.

   * Yönetici **Kullanıcı Adı** kutusunda, Clarizen Kiracınızdaki yönetici hesabının kullanıcı adını girin. admin@contoso.com bunun bir örneğidir.

   * Yönetici **Parolası** kutusunda, yönetici kullanıcı adına karşılık gelen yönetici hesabının parolasını girin.

   * İçerik **URL** kutusunda, Adım 6'ya dayalı alt etki alanını doldurun.

6. Tableau Online için idari hesabınızda oturum imzaladıktan sonra, **etki alanı** ve Içerik **URL'si** değerlerini yönetici sayfasının URL'sinden alabilirsiniz.

    * Tableau Online hesabınıza ait **Etki Alanı** URL'nin bu bölümünden kopyalanabilir:

        ![Tableau Online Etki Alanı](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * Tableau Online hesabınızın **İçerik URL'si** bu bölümden kopyalanabilir. Hesap kurulumu sırasında tanımlanan bir değerdir. Bu örnekte, değer "contoso"dur:

        ![Tableau Online İçerik URL'si](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > **Etki alanınız** burada gösterilenden farklı olabilir.

7. Adım 5'te gösterilen kutuları doldurduktan sonra, Azure AD'nin Tableau Online'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Tableau Online hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Tableau Online Test Bağlantısı](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. Bildirim **E-posta** kutusuna, sağlama hatası bildirimlerini almak için kişinin veya grubun e-posta adresini girin. Bir **hata olduğunda e-posta gönder bildirimini** seçin onay kutusunu seçin.

    ![Tableau Online Bildirim E-posta](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. **Kaydet'i**seçin.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Tableau'ya Senkronize Et'i**seçin.

    ![Tableau Online kullanıcı senkronizasyonu](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Tableau Online'a senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Tableau Online'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Tableau Online eşleşen kullanıcı öznitelikleri](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Tableau'ya Senkronize Et'i**seçin.

    ![Tableau Online grup senkronizasyonu](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Tableau Online'a senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Tableau Online'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişikliği kaydetmek için **Kaydet'i**seçin.

    ![Tableau Online eşleşen grup öznitelikleri](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Kapsam filtrelerini yapılandırmak [için, kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)öğreticisindeki yönergeleri izleyin.

15. **Ayarlar** bölümünde, Tableau Online için Azure AD sağlama hizmetini etkinleştirmek **için, Sağlama Durumunu** **A'da**olarak değiştirin.

    ![Tableau Online Tedarik Durumu](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. Tableau Online'a sağlamak istediğiniz kullanıcıları veya grupları tanımlayın. **Ayarlar** bölümünde, **Kapsam'ta**istediğiniz değerleri seçin.

    ![Tableau Online Kapsam](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Tableau Online Kaydet](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilemi daha sonraki eşitlemelerden daha uzun sürüyor. Azure AD sağlama hizmeti çalıştığı sürece yaklaşık her 40 dakikada bir oluşur. 

İlerlemeyi izlemek ve sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz. Rapor, Tableau Online'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
