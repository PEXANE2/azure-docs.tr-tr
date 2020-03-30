---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için iProva'yı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi otomatik olarak sağlama ve kullanıcı hesaplarını iProva'ya sağlama dan çıkarma için nasıl yapılandırıştırmayı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057516"
---
# <a name="tutorial-configure-iprova-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için iProva'yı yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları iProva'ya otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için iProva ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir iProva kiracı](https://www.iProva.com/)
* Yönetici izinleri olan iProva'daki bir kullanıcı hesabı.

## <a name="assigning-users-to-iprova"></a>kullanıcıları iProva'ya atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların iProva'ya erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek iProva'ya atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-iprova"></a>iProva'ya kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için iProva'ya tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* iProva'ya bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-iprova-for-provisioning"></a>Sağlama için iProva'yı ayarlama

1. [iProva Yönetici Konsolunuzda](https://www.iProva.com/)oturum açın. > **Uygulama Yönetimine Git'e**gidin.

    ![iProva Yönetici Konsolu](media/iprova-provisioning-tutorial/admin.png)

2.  Dış **kullanıcı yönetimi'ne**tıklayın.

    ![iProva SCIM ekle](media/iprova-provisioning-tutorial/external.png)

3. Yeni bir sağlayıcı eklemek için **artı** simgesine tıklayın. Yeni **Ekle sağlayıcı** iletişim kutusunda, **bir Başlık**sağlayın. **IP tabanlı erişim kısıtlaması**eklemeyi seçebilirsiniz. **Tamam** düğmesine tıklayın.

    ![iProva yeni eklemek](media/iprova-provisioning-tutorial/add.png)

    ![iProva sağlayıcı eklemek](media/iprova-provisioning-tutorial/addprovider.png)

4.  Kalıcı **belirteç** düğmesine tıklayın. Kalıcı **belirteci** kopyalayın ve bu görüntüleyebilirsiniz sadece zaman olacak gibi kaydedin. Bu değer, Azure portalındaki iProva uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

    ![iProva Oluştur Jetonu](media/iprova-provisioning-tutorial/token.png)

## <a name="add-iprova-from-the-gallery"></a>Galeriden iProva ekle

Azure AD ile otomatik kullanıcı sağlama için iProva'yı yapılandırmadan önce, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize iProva eklemeniz gerekir.

**Azure AD uygulama galerisinden iProva eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **iProva'yı**girin , sonuç panelinde **iProva'yı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![sonuç listesinde iProva](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-iprova"></a>Otomatik kullanıcı sağlamayı iProva'ya yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak iProva'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca [iProva tek](https://docs.microsoft.com/azure/active-directory/saas-apps/iProva-tutorial)oturum açma öğreticisinde verilen yönergeleri izleyerek iProva için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="to-configure-automatic-user-provisioning-for-iprova-in-azure-ad"></a>Azure AD'de iProva için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **iProva'yı**seçin.

    ![Uygulamalar listesindeki iProva bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://identitymanagement.services.iProva.nl/scim` altında, **Kiracı URL'ye**giriş . Gizli Belirteç'te daha önce alınan **Kalıcı belirteç** değerini **girdi.** Azure AD'nin iProva'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, iProva hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını iProva'ya Senkronize Et'i**seçin.

    ![iProva Kullanıcı Eşlemeleri](media/iprova-provisioning-tutorial/usermappings.png)

9. Azure AD'den iProva'ya eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme iProva' daki kullanıcı hesaplarını güncelleştirme iProva i Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![iProva Kullanıcı Özellikleri](media/iprova-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **azure etkin dizin gruplarını iProva'ya senkronize et'i**seçin.

    ![iProva Grup Eşlemeleri](media/iprova-provisioning-tutorial/groupmappings.png)

11. Azure AD'den iProva'ya eşitlenen grup özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri iProva' daki gruplarla eşleşmek iProva' da kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![iProva Grup Öznitelikleri](media/iprova-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. iProva için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek iProva'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından iProva'da gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

