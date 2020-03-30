---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için SpaceIQ'yi yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını SpaceIQ'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 207c8214-6304-4687-afc6-61562f0041a9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/07/2019
ms.author: Zhchia
ms.openlocfilehash: e59e9d86f394104752ef966b2a9cad2b78a1bc93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062789"
---
# <a name="tutorial-configure-spaceiq-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için SpaceIQ'yi yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları SpaceIQ'ye otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için SpaceIQ ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir SpaceIQ kiracı](https://spaceiq.com/)
* Admin izinleri ile SpaceIQ bir kullanıcı hesabı.

## <a name="assigning-users-to-spaceiq"></a>SpaceIQ'ye kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların SpaceIQ'ye erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek SpaceIQ'ye atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-spaceiq"></a>SpaceIQ'ye kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için SpaceIQ'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* SpaceIQ'ye bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-spaceiq-for-provisioning"></a>Sağlama için SpaceIQ'yi ayarlama

1. [SpaceIQ Yönetici Konsolunuzda](https://main.spaceiq.com/login/)oturum açın. Ekranın sağ üst köşesindeki açılır menüden seçerek **Ayarlar'a** gidin.

    ![SpaceIQ Yönetici Konsolu](media/spaceiq-provisioning-tutorial/admin.png)

2.  **Ayarlar** sayfasından **Üçüncü Taraf Tümleştirmelerini**Seçin.

    ![SpaceIQ SCIM ekle](media/spaceiq-provisioning-tutorial/thirdparty.png)

3.  Sağlama **ve SSO** sekmesine gidin. **Azure** döşemesini arayın. **Etkinleştir'e**tıklayın.

    ![SpaceIQ Sağlama ve SSO](media/spaceiq-provisioning-tutorial/provisioning.png)

    ![SpaceIQ Azure'u Etkinleştir ](media/spaceiq-provisioning-tutorial/azure.png)

3.  **SCIM Taşıyıcı Belirteci'ni**kopyalayın. Bu değer, Azure portalındaki SpaceIQ uygulamanızın Sağlama sekmesindeki Gizli Belirteç alanına girilir. **Etkinleştir'i** tıklatın

    ![SpaceIQ Oluştur Jetonu](media/spaceiq-provisioning-tutorial/token.png)

## <a name="add-spaceiq-from-the-gallery"></a>Galeriden SpaceIQ ekle

Azure AD ile otomatik kullanıcı sağlama için SpaceIQ'yi yapılandırmadan önce, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize SpaceIQ eklemeniz gerekir.

**Azure AD uygulama galerisinden SpaceIQ eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SpaceIQ'yi**girin, sonuç panelinde **SpaceIQ'yi** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![SpaceIQ sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-spaceiq"></a>SpaceIQ için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak SpaceIQ'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> SpaceIQ için SAML tabanlı tek oturum açmayı da etkinleştirmeyi seçebilirsiniz , [SpaceIQ Tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/spaceiq-tutorial)açma öğreticisinde verilen talimatları izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini birbiriyle dışa

### <a name="to-configure-automatic-user-provisioning-for-spaceiq-in-azure-ad"></a>Azure AD'de SpaceIQ için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **SpaceIQ'yi**seçin.

    ![Uygulamalar listesindeki SpaceIQ bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://api.spaceiq.com/scim` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini girdi. Azure AD'nin SpaceIQ'ye bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, SpaceIQ hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını SpaceIQ**ile Senkronize Et'i seçin.

    ![SpaceIQ Kullanıcı Haritalamaları](media/spaceiq-provisioning-tutorial/usermapping.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den SpaceIQ'ye senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için SpaceIQ'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![SpaceIQ Kullanıcı Özellikleri](media/spaceiq-provisioning-tutorial/userattributes.png)

11. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

12. SpaceIQ için Azure AD sağlama hizmetini etkinleştirmek için **Ayarlar** bölümünde **Sağlama Durumunu** **Açık** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek SpaceIQ'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. SpaceIQ'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)