---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Signagelive'ı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi signagelive'a otomatik olarak sağlama ve kullanıcı hesaplarını sağlama ve sağlamadan çıkarma için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: fb48deae-4653-448a-ba2f-90258edab3a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: Zhchia
ms.openlocfilehash: bbd0d3c60c5cc4056d5cbadfc7c6d90ae2a37ca6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063263"
---
# <a name="tutorial-configure-signagelive--for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Signagelive'ı yapılandırın

Bu öğreticinin amacı, Azure AD'yi signagelive'a kullanıcıları ve/veya grupları otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Signagelive ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir Signagelive kiracı](https://signagelive.com/pricing/)
* Admin izinleri ile Signagelive bir kullanıcı hesabı.

## <a name="assigning-users-to-signagelive"></a>Signagelive'a kullanıcı atama   

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Signagelive'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Signagelive'a atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-signagelive"></a>Signagelive'a kullanıcı atamak için önemli ipuçları   

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Signagelive'a atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Signagelive'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-signagelive--for-provisioning"></a>Kurulum Signagelive sağlama için

Signagelive'ı Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce Signagelive'da SCIM sağlamayı etkinleştirmeniz gerekir.

  SCIM sağlama yapılandırmak için gerekli gizli belirteci elde etmek için [Signagelive](mailto:development@signagelive.com) ulaşın.

## <a name="add-signagelive-from-the-gallery"></a>Galeriden Signagelive ekle

Signagelive'ı Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Signagelive'ı eklemeniz gerekir.

**Azure AD uygulama galerisinden Signagelive eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Signagelive'ı**girin , sonuç panelinde **Signagelive'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde signagelive](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-signagelive"></a>Signagelive için otomatik kullanıcı sağlama yapılandırma    

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Signagelive'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
>  Ayrıca Signagelive Tek oturum açma [öğreticisinde](Signagelive-tutorial.md)verilen talimatları izleyerek Signagelive için SAML tabanlı tek oturum açma yı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-signagelive--in-azure-ad"></a>Azure AD'de Signagelive için otomatik kullanıcı sağlama yı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Başvuru listesinde **Signagelive'ı**seçin.

    ![Uygulamalar listesindeki Signagelive bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici Kimlik Bilgileri bölümü ` https://samlapi.signagelive.com/scim/v2` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç** alanında, mühendislik geliştirme ekibi tarafından sağlanan **taşıyıcı Belirteç** değerini girin. Azure AD'nin Signagelive'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Signagelive hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.
    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Signagelive için Azure Etkin Dizin Kullanıcılarını Senkronize**Et'i seçin.

    ![Signagelive Kullanıcı Haritalamaları](media/signagelive-provisioning-tutorial/usermapping.png)

9. Azure AD'den Signagelive'a, **Öznitelik Eşleme** bölümünde senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Signagelive'daki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Signagelive Kullanıcı Özellikleri](media/signagelive-provisioning-tutorial/userattribute.png)

10. **Eşlemeler** bölümünde, **Signagelive için Azure Etkin Dizin Grubunu Senkronize**Et'i seçin.

    ![Signagelive Kullanıcı Haritalamaları](media/signagelive-provisioning-tutorial/groupmapping.png)

11. Azure AD'den Signagelive'a **öznitelik eşleme** bölümünde senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Signagelive'daki grup hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Signagelive Kullanıcı Özellikleri](media/signagelive-provisioning-tutorial/groupattribute.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Signagelive için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Signagelive'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilimi sonraki eşitlemelerden daha uzun sürer. Kullanıcıların ve/veya grupların sağlanmasının ne kadar süreceğü hakkında daha fazla bilgi için, [kullanıcıların sağlanmasının ne kadar süreceğini](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)görün. 

Signagelive'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemedurumunu izlemek ve sağlama faaliyet raporunuza gelen bağlantıları izlemek için **Geçerli Durum** bölümünü kullanabilirsiniz. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
