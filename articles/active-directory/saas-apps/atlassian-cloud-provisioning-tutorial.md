---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Atlassian Cloud'u yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Atlassian Cloud'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/27/2019
ms.author: jeedes
ms.openlocfilehash: 7ddccef00cf1b5ad524c0e1eaa7aed52c0e55197
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77059377"
---
# <a name="tutorial-configure-atlassian-cloud-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Atlassian Cloud'u yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Atlassian Cloud'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Atlassian Cloud ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Atlassian Cloud kiracı](https://www.atlassian.com/licensing/cloud)
* Yönetici izinli Atlassian Cloud'daki bir kullanıcı hesabı.

> [!NOTE]
> Azure AD sağlama tümleştirmesi, Atlassian Cloud ekiplerinin kullanabileceği **Atlassian Cloud SCIM API'sine**dayanır.

## <a name="add-atlassian-cloud-from-the-gallery"></a>Galeriden Atlassian Cloud ekle

Atlassian Cloud'u Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Atlassian Cloud eklemeniz gerekir.

**Azure AD uygulama galerisinden Atlassian Cloud eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Atlassian Cloud'u**girin, sonuç panelinde **Atlassian Cloud'u** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Atlassian Cloud sonuç listesinde](common/search-new-app.png)

## <a name="assigning-users-to-atlassian-cloud"></a>Kullanıcıları Atlassian Cloud'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Atlassian Cloud'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Atlassian Cloud'a atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-atlassian-cloud"></a>Atlassian Cloud'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Atlassian Cloud'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Atlassian Cloud'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-atlassian-cloud"></a>Otomatik kullanıcı sağlamayı Atlassian Cloud'a yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Atlassian Cloud'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Atlassian Cloud tek oturum açma öğreticisinde verilen yönergeleri izleyerek [Atlassian](atlassian-cloud-tutorial.md)Cloud için SAML tabanlı tek oturum açmayı da etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-atlassian-cloud-in-azure-ad"></a>Azure AD'de Atlassian Cloud için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Atlassian Cloud'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Atlassian Cloud'u**seçin.

    ![Uygulamalar listesindeki Atlassian Cloud bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/provisioning-tab.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/credentials.png)

5. Org > dizinini seçmek> [Atlassian Organizasyon](https://admin.atlassian.com) **Yöneticisi'ne**gidin.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/select-directory.png)

6. **Kullanıcı Sağlama'yı** tıklatın ve **dizin oluştur'a**tıklayın. **Dizin temel URL'sini** ve **Taşıyıcı Belirteci'ni** sırasıyla **Kiracı URL'si** ve Gizli **Belirteç** alanlarına kopyalayın.

    ![Atlassian Bulut](./media/atlassian-cloud-provisioning-tutorial/secret-token-1.png) ![Sağlama Atlassian](./media/atlassian-cloud-provisioning-tutorial/secret-token-2.png) ![Bulut Sağlama Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/secret-token-3.png)

7. Yönetici **Kimlik Bilgileri** bölümüne, Atlassian Cloud hesabınızın **Kiracı URL'sini** ve **Gizli Jetonunu** girin. Bu değerlere örnek olarak şunlar verilebilir:

   * Kiracı **URL** alanında, Adım 6'da açıklandığı gibi Atlassian'dan aldığınız belirli kiracı bitiş noktasını doldurun. Örneğin: `https://api.atlassian.com/scim/directory/{directoryId}`.

   * Gizli **Belirteç** alanında, Adım 6'da açıklandığı gibi gizli belirteci doldurun.

8. Adım 7'de gösterilen alanları doldurarak, Azure AD'nin Atlassian Cloud'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Atlassian Cloud hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/test-connection.png)

9. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/notification.png)

10. **Kaydet**'e tıklayın.

11. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Atlassian Cloud'a Senkronize Et'i**seçin.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/provision-users.png)

12. **Öznitelik Eşleme** bölümünde Azure AD'den Atlassian Cloud'a senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Atlassian Cloud'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/user-mapping.png)

13. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Atlassian Cloud'a Senkronize Et'i**seçin.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/provision-groups.png)

14. **Öznitelik Eşleme** bölümünde Azure AD'den Atlassian Cloud'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Atlassian Cloud'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/group-mapping.png)

15. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

16. Atlassian Cloud için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/provisioning-on.png)

17. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Atlassian Cloud'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/provisioning-options.png)

18. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Atlassian Bulut Sağlama](./media/atlassian-cloud-provisioning-tutorial/save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Atlassian Cloud'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları

* Atlassian Cloud, kullanıcıların yalnızca [doğrulanmış etki alanlarından](https://confluence.atlassian.com/cloud/organization-administration-938859734.html)sağlanmasına olanak tanır.
* Atlassian Cloud bugün grup yeniden adlarını desteklemiyor. Bu, Azure AD'deki bir grubun displayName'sinde yapılan değişikliklerin güncelleştirilmeyeceğini ve Atlassian Cloud'a yansıtılmayacağı anlamına gelir.
* Azure AD'deki **posta** kullanıcısı özniteliğinin değeri yalnızca kullanıcının bir Microsoft Exchange Posta Kutusu varsa doldurulur. Kullanıcıda yoksa, Atlassian Cloud'daki **e-posta** özniteliğine farklı bir istenilen öznitelik eşlemesi önerilir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-03.png