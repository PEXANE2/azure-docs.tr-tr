---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Dropbox for Business'ı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Otomatik olarak sağlama ve şirket başına bırakma işlemlerini Dropbox for Business'a otomatik olarak sağlamak üzere nasıl yapılandırılamayı öğrenin.
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
ms.date: 05/20/2019
ms.author: jeedes
ms.openlocfilehash: 3acc2c271e590bddb13aaa01498f404da4340036
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058468"
---
# <a name="tutorial-configure-dropbox-for-business-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Dropbox'ı İş için yapılandır

Bu öğreticinin amacı, Azure AD'yi otomatik olarak sağlama ve kullanıcıları ve/veya grupları İş Için Dropbox'a sağlama ve bunları sağlamadan çıkarmak üzere yapılandırmak için Dropbox for Business ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [İş kiracı için bir Dropbox](https://www.dropbox.com/business/pricing)
* Yönetici izinleri olan Dropbox for Business'taki bir kullanıcı hesabı.

## <a name="add-dropbox-for-business-from-the-gallery"></a>Galeriden İşletmeler için Dropbox ekle

Azure AD ile otomatik kullanıcı sağlama için Dropbox for Business'ı yapılandırmadan önce, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Dropbox'ı İşletme için eklemeniz gerekir.

**Azure AD uygulama galerisinden İşletmeler için Dropbox eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **İşletmeler için Dropbox'ı**girin, sonuç panelinde **İşletmeler için Dropbox'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde İşletmeler için Dropbox](common/search-new-app.png)

## <a name="assigning-users-to-dropbox-for-business"></a>İş için Dropbox'a kullanıcı atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların İş Için Dropbox'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları dropbox for Business'a buradaki talimatları izleyerek atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-dropbox-for-business"></a>İş için Dropbox'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının Dropbox for Business'a atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Dropbox for Business'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-dropbox-for-business"></a>İş için Dropbox için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına bağlı olarak Dropbox for Business'taki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Dropbox for Business tek oturum açma [öğreticisinde](dropboxforbusiness-tutorial.md)verilen talimatları izleyerek, Dropbox for Business için SAML tabanlı tek oturum açma'yı da etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-dropbox-for-business-in-azure-ad"></a>Azure AD'de Dropbox for Business için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, İşletmeler **için Dropbox'ı**seçin.

    ![Uygulamalar listesindeki İşletmeler için Dropbox bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümünde **Yetkiver'i**tıklatın. Yeni bir tarayıcı penceresinde İşletmeler için Bir Dropbox giriş iletişim kutusu açar.

    ![Sağlama ](common/provisioning-oauth.png)

6. Azure AD iletişim **kutusuna bağlanmak için Business için Dropbox'a Oturum** Aç'ta, Şirket için Dropbox'ınızda oturum açın ve kimliğinizi doğrulayın.

    ![İş oturum açma için Dropbox](media/dropboxforbusiness-provisioning-tutorial/dropbox01.png)

7. 5 ve 6 adımlarını tamamladıktan sonra, Azure AD'nin İşletmeler için Dropbox'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Dropbox for Business hesabınızda Yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-oauth.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Dropbox'a Senkronize Et'i**seçin.

    ![Dropbox Kullanıcı Eşlemeleri](media/dropboxforbusiness-provisioning-tutorial/dropbox-user-mapping.png)

11. **Öznitelik Eşleme** bölümünde Azure AD'den Dropbox'a eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Dropbox'taki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Dropbox Kullanıcı Özellikleri](media/dropboxforbusiness-provisioning-tutorial/dropbox-user-attributes.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Dropbox'a Senkronize Et'i**seçin.

    ![Dropbox Grup Eşlemeleri](media/dropboxforbusiness-provisioning-tutorial/dropbox-group-mapping.png)

13. **Öznitelik Eşleme** bölümünde Azure AD'den Dropbox'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemi için Dropbox' taki grupların çünüm etmek için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Dropbox Grup Öznitelikleri](media/dropboxforbusiness-provisioning-tutorial/dropbox-group-attributes.png)

14. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

15. Dropbox için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

16. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Dropbox'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

17. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Kaydetme **Ayrıntıları** bölümünü, ilerlemeyi izlemek ve Dropbox'taki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik raporuna bağlı bağlantıları izlemek için kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları
 
* Dropbox davet edilen kullanıcıların askıya alınması nı desteklemez. Davet edilen bir kullanıcı askıya alınırsa, bu kullanıcı silinir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

