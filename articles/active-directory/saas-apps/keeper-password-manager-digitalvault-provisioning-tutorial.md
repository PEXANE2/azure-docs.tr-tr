---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Keeper Password Manager & Digital Vault'u yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Otomatik olarak sağlama ve sağlamadan alma işlemlerini Keeper Password Manager & Digital Vault'a nasıl yapılandıracak şekilde yapılandırmayı öğrenin.
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
ms.openlocfilehash: 236527a9889879f872ef8c3867a7ec3c1b1ba0a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057550"
---
# <a name="tutorial-configure-keeper-password-manager--digital-vault-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Keeper Password Manager & Digital Vault'u yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Keeper Password Manager & Digital Vault'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Keeper Password Manager & Digital Vault ve Azure Active Directory (Azure AD) & gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Keeper Şifre Yöneticisi & Dijital Vault kiracı](https://keepersecurity.com/pricing.html?t=e)
* Yönetici izinleri ile Digital Vault & Keeper Password Manager'daki bir kullanıcı hesabı.

## <a name="add-keeper-password-manager--digital-vault-from-the-gallery"></a>Galeriden Keeper Password Manager & Digital Vault ekle

Keeper Password Manager & Digital Vault'u Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Azure AD uygulama galerisinden Keeper Password Manager & Digital Vault'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden Keeper Password Manager & Digital Vault eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Keeper Password Manager & Digital Vault'u**girin , sonuç panelinde **Keeper Password Manager & Digital Vault'u** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Keeper Password Manager & Dijital Kasa sonuç listesinde](common/search-new-app.png)

## <a name="assigning-users-to-keeper-password-manager--digital-vault"></a>Kullanıcıları Keeper Password Manager & Digital Vault'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Keeper Password Manager & Digital Vault'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları, buradaki talimatları izleyerek Keeper Password Manager & Digital Vault'a atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-keeper-password-manager--digital-vault"></a>Kullanıcıları Keeper Password Manager & Digital Vault'a atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için Tek bir Azure AD kullanıcısının Keeper Password Manager & Digital Vault'a atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Keeper Password Manager & Digital Vault'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-keeper-password-manager--digital-vault"></a>Otomatik kullanıcı sağlamanın Keeper Password Manager & Digital Vault'a yapılandırılması 

Bu bölüm, Azure AD sağlama hizmetini, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Keeper Password Manager & Digital Vault'taki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı etmek için yapılandırma adımları boyunca size rehberlik eder.

> [!TIP]
> Ayrıca, Keeper Password Manager & Digital Vault tek oturum açma [öğreticisinde](keeperpasswordmanager-tutorial.md)verilen talimatları izleyerek, Keeper Password Manager & Digital Vault için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-keeper-password-manager--digital-vault-in-azure-ad"></a>Azure AD'de Keeper Password Manager & Digital Vault için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama **listesinde, Keeper Password Manager & Digital Vault'u**seçin.

    ![Keeper Password Manager & Uygulamalar listesinde Dijital Vault bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü ne si bölümünde, Keeper Password Manager'ın **Kiracı URL'sini** ve **Gizli Jetonunu,** Adım 6'da açıklandığı şekilde Digital Vault hesabını & girin.

6. Keeper Admin [Konsolunuzda](https://keepersecurity.com/console/#login)oturum açın. **Yönetici'yi** tıklatın ve varolan bir düğüm seçin veya yeni bir düğüm oluşturun. **Sağlama** sekmesine gidin ve **Yöntem Ekle'yi**seçin.

    ![Keeper Yönetici Konsolu](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-admin-console.png)

    **SCIM (Etki Alanı Arası Kimlik Yönetimi Sistemi)** seçeneğini belirleyin.

    ![Kaleci SCIM ekle](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-add-scim.png)

    **Sağlama Belirteci Oluştur'u**tıklatın.

    ![Kaleci Bitiş Noktası Oluştur](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-create-endpoint.png)

    **URL** ve **Token** değerlerini kopyalayın ve Azure AD'de **Kiracı URL'sine** ve **Gizli Belirteç'e** yapıştırın. Keeper'daki sağlama kurulumlarını tamamlamak için **Kaydet'i** tıklatın.

    ![Kaleci Oluştur Belirteci](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-create-token.png)

7. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin Keeper Password Manager & Digital Vault'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Keeper Password Manager & Digital Vault hesabınızın Yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Active Directory Kullanıcılarını Dijital Kasa & Parola Yöneticisine Senkronize**Et'i seçin.

    ![Keeper Kullanıcı Eşlemeleri](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-user-mappings.png)

11. Azure AD'den Keeper Password Manager & Digital Vault'a **öznitelik eşleme** bölümünde senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Keeper Password Manager & Digital Vault'taki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Keeper Kullanıcı Öznitelikleri](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-user-attributes.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Password Manager & Digital Vault'a Senkronize Et'i**seçin.

    ![Kaleci Grubu Haritalamaları](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-group-mappings.png)

13. Azure AD'den Keeper Password Manager & Digital Vault'a **Öznitelik Eşleme** bölümünde senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Keeper Password Manager & Digital Vault'taki grupları eşleştirmek için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Kaleci Grubu Öznitelikleri](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-group-attributes.png)

14. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

15. Keeper Password Manager & Digital Vault için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Ki Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

16. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek, Keeper Password Manager & Digital Vault'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

17. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Kaydetme **Ayrıntıları** bölümünü, kaydedilen ilerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından Keeper Password Manager & Digital Vault'ta gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları

* Keeper Password Manager & Digital **Vault, e-postaların** ve **kullanıcı Adının** aynı kaynak değerine sahip olmasını gerektirir, çünkü her iki öznitelikteki herhangi bir güncelleştirme diğer değeri değiştirir.
* Keeper Password Manager & Digital Vault kullanıcı silmeleri desteklemez, yalnızca devre dışı kalır. Devre dışı bırakılan kullanıcılar Keeper Admin Console UI'da kilitli olarak görünür.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

