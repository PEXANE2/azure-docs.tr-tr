---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Dinamik Sinyali Yapılandırın | Microsoft Dokümanlar'
description: Azure Active Directory'yi otomatik olarak sağlanan ve kullanıcı hesaplarını Dinamik Sinyal'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.date: 05/07/2019
ms.author: jeedes
ms.openlocfilehash: 2ec91d42dff8f3a1fc4b036aa1c3ec77faf6a0fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058051"
---
# <a name="tutorial-configure-dynamic-signal-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Dinamik Sinyali Yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Dinamik Sinyal'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Dinamik Sinyal ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Dinamik Sinyal kiracı](https://dynamicsignal.com/)
* Yönetici izinleri olan Dinamik Sinyal'deki bir kullanıcı hesabı.

## <a name="add-dynamic-signal-from-the-gallery"></a>Galeriden Dinamik Sinyal Ekleme

Azure AD ile otomatik kullanıcı sağlama için Dinamik Sinyal'i yapılandırmadan önce, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Dinamik Sinyal eklemeniz gerekir.

**Azure AD uygulama galerisinden Dinamik Sinyal eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Dinamik Sinyal**girin , sonuç panelinde **Dinamik Sinyal'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde Dinamik Sinyal](common/search-new-app.png)

## <a name="assigning-users-to-dynamic-signal"></a>Kullanıcıları Dinamik Sinyale Atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Dinamik Sinyal'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Dinamik Sinyal'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-dynamic-signal"></a>Kullanıcıları Dinamik Sinyal'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının Dinamik Sinyal'e atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Dinamik Sinyal'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-dynamic-signal"></a>Otomatik kullanıcı sağlamanın Dinamik Sinyal'e yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Dinamik Sinyal'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Dinamik Sinyal tek oturum açma [öğreticisinde](dynamicsignal-tutorial.md)verilen yönergeleri izleyerek Dinamik Sinyal için SAML tabanlı tek oturum açmayı da etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-dynamic-signal-in-azure-ad"></a>Azure AD'de Dinamik Sinyal için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Dinamik Sinyal'i**seçin.

    ![Uygulamalar listesindeki Dinamik Sinyal bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü ne şrbölümünde, Dinamik Sinyalhesabınızın **6.** **Secret Token**

6. Dinamik Sinyal yönetici konsolunda, **Yönetici > Gelişmiş > API'ye**gidin.

    ![Dinamik Sinyal Sağlama](./media/dynamic-signal-provisioning-tutorial/secret-token-1.png)

    **SCIM API URL'sini** **Kiracı URL'sine**kopyalayın. **Bir Taşıyıcı Belirteci** oluşturmak ve değeri **Gizli Belirteç'e**kopyalamak için **Yeni Belirteç Oluştur'a** tıklayın.

    ![Dinamik Sinyal Sağlama](./media/dynamic-signal-provisioning-tutorial/secret-token-2.png)

7. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin Dinamik Sinyal'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Dinamik Sinyal hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Dinamik Sinyale Senkronize Etme'yi**seçin.

    ![Dinamik Sinyal Kullanıcı Haritalamaları](media/dynamic-signal-provisioning-tutorial/user-mappings.png)

11. **Öznitelik Eşleme** bölümünde Azure AD'den Dinamik Sinyal'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Dinamik Sinyal'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Dinamik Sinyal Kullanıcı Özellikleri](media/dynamic-signal-provisioning-tutorial/user-mapping-attributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Dinamik Sinyal için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Dinamik Sinyal'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Dinamik Sinyal'de Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları

* Dinamik Sinyal, Azure AD'den kalıcı kullanıcı silmelerini desteklemez. Dinamik Sinyal'de bir kullanıcıyı kalıcı olarak silmek için, işlemin Dinamik Sinyal yönetici konsolu UI üzerinden yapılması gerekir. 
* Dinamik Sinyal şu anda grupları desteklemiyor.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

