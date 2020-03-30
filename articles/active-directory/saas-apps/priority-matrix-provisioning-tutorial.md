---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Öncelik Matrisini Yapılandırın | Microsoft Dokümanlar'
description: Azure Active Directory'yi, kullanıcı hesaplarını Öncelik Matrisi'ne otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: a4598a99-3c98-4c14-86c2-95cc562e2439
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2019
ms.author: Zhchia
ms.openlocfilehash: 80ffaba6713027d216958e0be2cd4ae35a8d2d70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063484"
---
# <a name="tutorial-configure-priority-matrix-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Öncelik Matrisini Yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Öncelik Matrisi'ne otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Öncelik Matrisi ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Öncelikli Matris kiracı](https://appfluence.com/pricing/)
* Yönetici izinleri olan Öncelikli Matris'teki bir kullanıcı hesabı.

## <a name="assign-users-to-priority-matrix"></a>Kullanıcıları Öncelik Matrisine Atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Öncelik Matrisi'ne erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki yönergeleri izleyerek Öncelikli Matris'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-priority-matrix"></a>Kullanıcıları Öncelikli Matris'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için öncelik matrisine tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Öncelikli Matris'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-priority-matrix-for-provisioning"></a>Sağlama için Öncelik Matrisi'ni ayarlama

Azure AD ile otomatik kullanıcı sağlama için Öncelik Matrisi'ni yapılandırmadan önce, Öncelik Matrisi'nden bazı sağlama bilgileri almanız gerekir.

1. Öncelikli Matris [Yönetici Konsolunuzda](https://sync.appfluence.com/accounts/login/?next=/accounts/provisioning)oturum açın.

3. Öncelik Matrisi için **Oauth giriş belirteci'ni** tıklatın

    ![Öncelik Matris ilerki SCIM](media/priority-matrix-provisioning-tutorial/oauthlogin.png)

4. Yenİ **TOKEN AL** düğmesini tıklatın. **Token Dizesini**kopyalayın. Bu değer, Azure portalındaki Öncelikli Matris uygulamanızın Sağlama sekmesinde **Gizli Belirteç** alanına girilir. 

    ![Öncelik Matris Oluşturma Belirteci](media/priority-matrix-provisioning-tutorial/token.png)

## <a name="add-priority-matrix-from-the-gallery"></a>Galeriden Öncelik Matrisi Ekleme

Azure AD ile otomatik kullanıcı sağlama için Öncelik Matrisi'ni yapılandırmak için, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Öncelik Matrisi eklemeniz gerekir.

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Öncelik Matrisi**girin, sonuç panelinde **Öncelik Matrisi'ni** seçin. 

    ![Sonuç listesinde öncelik matrisi](common/search-new-app.png)

5. Sizi Öncelikli Matris'in giriş sayfasına yönlendirecek **Öncelikli Matris için Kaydol** düğmesini seçin. 

    ![Öncelikli Matris OIDC Ekle](media/priority-matrix-provisioning-tutorial/signup.png)

6. Öncelik Matrisi bir OpenIDConnect uygulaması olduğundan, Microsoft iş hesabınızı kullanarak Öncelikli Matris'e giriş yapmayı seçin.

    ![Öncelikli Matris OIDC girişi](media/priority-matrix-provisioning-tutorial/msftsignin.png)

7. Başarılı bir kimlik doğrulamadan sonra, onay sayfası için onay istemini kabul edin. Uygulama daha sonra otomatik olarak kiracınıza eklenir ve Öncelikli Matris hesabınıza yönlendirilirsiniz.

    ![Öncelikli Matris OIDc Onayı](media/priority-matrix-provisioning-tutorial/consent.png)

## <a name="configure-automatic-user-provisioning-to-priority-matrix"></a>Otomatik kullanıcı sağlamayı Öncelik Matrisi olarak yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı Olarak Öncelikli Matris'teki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!NOTE]
> Öncelikli Matris'in SCIM bitiş noktası hakkında daha fazla bilgi edinmek [için, Kullanıcı sağlama ve Öncelik](https://appfluence.com/help/article/user-provisioning/)Matrisi'ne bakın.

### <a name="to-configure-automatic-user-provisioning-for-priority-matrix-in-azure-ad"></a>Azure AD'de Öncelik Matrisi için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Öncelik Matrisi'ni**seçin.

    ![Uygulamalar listesindeki Öncelik Matrisi bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://sync.appfluence.com/scim/v2/` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**Öncelikli Matris'ten daha önce aldığınız ve kaydettiğiniz değeri girdiniz. Azure AD'nin Öncelikli Matris'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Öncelikli Matris hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Öncelik Matrisi'ne Senkronize Et'i**seçin.

    ![Öncelikli Matris Kullanıcı Eşlemeleri](media/priority-matrix-provisioning-tutorial/usermappings.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den Öncelik Matrisi'ne eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işleri için Öncelikli Matris' teki kullanıcı hesaplarıyla karşılamak için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Öncelik Matriskullanıcı Öznitelikleri](media/priority-matrix-provisioning-tutorial/userattributes.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Öncelik Matrisi için Azure AD sağlama hizmetini etkinleştirmek **için,** **Ayarlar** bölümünde Sağlama Durumunu **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Öncelik Matrisi'ne sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Öncelik Matrisi'nde Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


