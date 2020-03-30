---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Wrike'yi yapılandırın | Microsoft Dokümanlar"
description: Kullanıcı hesaplarını Wrike'a otomatik olarak sağlamak ve yok etmek için Azure Active Directory'yi nasıl yapılandıracak larını öğrenin.
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
ms.openlocfilehash: 5dd4f5ac6152c22b5e2a84ecc0774672bcd5590b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064207"
---
# <a name="tutorial-configure-wrike-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Wrike'yi yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları veya grupları Wrike'a otomatik olarak sağlamak ve etkisiz leştirmek üzere yapılandırmak için Wrike ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirdiğiniz adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD kullanıcı sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar için, [Azure Etkin Dizini ile hizmet olarak yazılım (SaaS) uygulamalarında kullanıcı sağlama ve bunları](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)ayrıştırma konusunda otomatikleştir'e bakın.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Wrike kiracı](https://www.wrike.com/price/)
* Wrike'de yönetici izinleri olan bir kullanıcı hesabı

## <a name="assign-users-to-wrike"></a>Kullanıcıları Wrike'a atama
Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Wrike'a erişmeye ihtiyacı olduğuna karar verin. Ardından, bu kullanıcıları veya grupları buradaki yönergeleri izleyerek Wrike'ye atayın:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-wrike"></a>Wrike'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için Wrike'a tek bir Azure AD kullanıcısı atamanızı öneririz. Ek kullanıcılar veya gruplar daha sonra atanabilir.

* Wrike'a bir kullanıcı atadığınızda, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-wrike-for-provisioning"></a>Tedarik için Wrike'ı ayarlama

Wrike'ı Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Wrike'da etki alanları arası kimlik yönetimi (SCIM) sağlama sistemini etkinleştirmeniz gerekir.

1. [Wrike yönetici konsolunuzda](https://www.Wrike.com/login/)oturum açın. Kiracı kimliğine git. **Uygulamaları & Entegrasyonları**seçin.

    ![Uygulamalar & Entegrasyonlar](media/Wrike-provisioning-tutorial/admin.png)

2.  Azure **AD'ye** gidin ve seçin.

    ![Azure AD](media/Wrike-provisioning-tutorial/Capture01.png)

3.  SCIM'i seçin. Temel **URL'yi**kopyalayın.

    ![Temel URL](media/Wrike-provisioning-tutorial/Wrike-tenanturl.png)

4. **API** > **Azure SCIM'i**seçin.

    ![Azure SCIM](media/Wrike-provisioning-tutorial/Wrike-add-scim.png)

5.  Bir açılır pencere açılır. Hesap oluşturmak için daha önce oluşturduğunuz parolayı girin.

    ![Wrike Belirteci Oluştur](media/Wrike-provisioning-tutorial/password.png)

6.  Gizli **Belirteci**kopyalayın ve Azure AD'ye yapıştırın. Wrike'daki sağlama kurulumünü bitirmek için **Kaydet'i** seçin.

    ![Kalıcı erişim belirteci](media/Wrike-provisioning-tutorial/Wrike-create-token.png)


## <a name="add-wrike-from-the-gallery"></a>Galeriden Wrike ekle

Azure AD ile otomatik kullanıcı sağlama için Wrike'yi yapılandırmadan önce, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Wrike ekleyin.

Azure AD uygulama galerisinden Wrike eklemek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com)sol daki gezinti bölmesinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Wrike'ı**girin, sonuç panelinde **Wrike'ı** seçin ve ardından uygulamayı eklemek için **Ekle'yi** seçin.

    ![Wrike sonuç listesinde](common/search-new-app.png)


## <a name="configure-automatic-user-provisioning-to-wrike"></a>Wrike için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı veya grup atamalarına dayalı olarak Wrike'deki kullanıcıları veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Wrike için SAML tabanlı tek oturum açmayı etkinleştirmek için [Wrike tek oturum](wrike-tutorial.md)açma öğreticisindeki yönergeleri izleyin. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="configure-automatic-user-provisioning-for-wrike-in-azure-ad"></a>Azure AD'de Wrike için otomatik kullanıcı sağlama yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları** > Seçin**Tüm uygulamalar**.

    ![Tüm uygulamalar](common/enterprise-applications.png)

2. Uygulamalar listesinde **Wrike'ı**seçin.

    ![Uygulamalar listesindeki Wrike bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama Modu Otomatik olarak ayarlanmış](common/provisioning-automatic.png)

5. Yönetici Kimlik Bilgileri bölümü altında, sırasıyla **Kiracı URL'sinde** ve Gizli **Belirteç'te**daha önce alınan **Temel URL** ve Kalıcı **erişim belirteci** değerlerini girin. Azure AD'nin Wrike'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Wrike hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + belirteç](common/provisioning-testconnection-tenanturltoken.png)

7. Bildirim **E-posta** kutusuna, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin. Bir **hata olduğunda e-posta gönder bildirimini** seçin onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

8. **Kaydet'i**seçin.

9. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Wrike**ile Senkronize Et'i seçin.

    ![Wrike kullanıcı eşlemeleri](media/Wrike-provisioning-tutorial/Wrike-user-mappings.png)

10. **Öznitelik Eşlemeleri** bölümünde Azure AD'den Wrike'ye eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Wrike'daki kullanıcı hesaplarıyla eşleştirmek için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin.

    ![Wrike kullanıcı öznitelikleri](media/Wrike-provisioning-tutorial/Wrike-user-attributes.png)

11. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisindeki yönergeleri izleyin.

12. Wrike için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Açik** olarak değiştirin.

    ![Geçici Durum Değiştirildi](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Wrike'a sağlamak istediğiniz kullanıcıları veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Sağlama yapılandırması kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilimi sonraki eşitlemelerden daha uzun sürer. Kullanıcıların veya grupların sağlanmasının ne kadar süreceği hakkında daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)

Wrike'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerleme durumunu izlemek ve sağlama faaliyet raporunuza gelen bağlantıları izlemek için **Geçerli Durum** bölümünü kullanabilirsiniz. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
