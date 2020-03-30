---
title: 'Öğretici: GitHub için kullanıcı sağlama - Azure AD'
description: Azure Active Directory'yi, kullanıcı hesaplarını GitHub'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 82f7252f2d9cdd2c54fae593d8463bfe84bd6ce2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057660"
---
# <a name="tutorial-configure-github-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için GitHub'ı yapılandır

Bu öğreticinin amacı, Azure AD'den GitHub'a kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için GitHub ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Etkin dizin kiracı
* [GitHub Enterprise Cloud'da](https://help.github.com/articles/github-s-products/#github-enterprise)oluşturulan ve [GitHub Kurumsal faturalandırma planını](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations) gerektiren bir GitHub kuruluşu
* Yönetici nin kuruluşa verdiği izinlere sahip GitHub'daki kullanıcı hesabı
* [Burada](https://help.github.com/en/github/setting-up-and-managing-organizations-and-teams/approving-oauth-apps-for-your-organization) açıklandığı gibi kuruluşunuz için OAuth erişiminin sağlandığından emin olun

> [!NOTE]
> Azure AD sağlama tümleştirmesi, GitHub [Enterprise Cloud](https://help.github.com/articles/github-s-products/#github-enterprise) müşterilerinin [GitHub Enterprise faturalandırma planında](https://help.github.com/articles/github-s-billing-plans/#billing-plans-for-organizations)kullanabileceği [GitHub SCIM API'sine](https://developer.github.com/v3/scim/)dayanır.

## <a name="assigning-users-to-github"></a>Kullanıcıları GitHub'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir. 

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların GitHub uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları github uygulamanıza aşağıdaki talimatları izleyerek atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-github"></a>Kullanıcıları GitHub'a atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için GitHub'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı GitHub'a atarken, atama iletişim kutusunda **Kullanıcı** rolünü veya uygulamaya özgü başka bir geçerli rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolü sağlama için çalışmaz ve bu kullanıcılar atlanır.

## <a name="configuring-user-provisioning-to-github"></a>Kullanıcı sağlamayı GitHub'a yapılandırma

Bu bölüm, Azure REKLAM'ınızı GitHub'ın kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini Azure AD'deki kullanıcı ve grup atamasına göre GitHub'da atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmak için size rehberlik eder.

> [!TIP]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek GitHub için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="configure-automatic-user-account-provisioning-to-github-in-azure-ad"></a>Azure AD'de GitHub'a otomatik kullanıcı hesabı sağlama yapılandırma

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

2. Tek oturum açma için GitHub'ı zaten yapılandırmışsanız, arama alanını kullanarak GitHub örneğinizi arayın. Aksi takdirde, uygulama galerisinde **GitHub'ı** **ekle** ve arama'yı seçin. Arama sonuçlarından GitHub'ı seçin ve uygulama listenize ekleyin.

3. GitHub örneğini seçin ve ardından **Sağlama** sekmesini seçin.

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![GitHub Sağlama](./media/github-provisioning-tutorial/GitHub1.png)

5. Yönetici **Kimlik Bilgileri** bölümünde **Yetkiver'i**tıklatın. Bu işlem, yeni bir tarayıcı penceresinde bir GitHub yetkilendirme iletişim kutusunu açar. Erişim yetkisi için onayaldığınızdan emin olmanız gerektiğini unutmayın. [Burada](https://help.github.com/github/setting-up-and-managing-organizations-and-teams/approving-oauth-apps-for-your-organization)açıklanan yönergeleri izleyin.

6. Yeni pencerede, Yönetici hesabınızı kullanarak GitHub'da oturum açın. Elde edilen yetkilendirme iletişim kutusunda, sağlama sağlamak istediğiniz GitHub ekibini seçin ve ardından **Yetkilendirme'yi**seçin. Tamamlandıktan sonra, sağlama yapılandırmasını tamamlamak için Azure portalına dönün.

    ![Yetkilendirme İletişim Kutusu](./media/github-provisioning-tutorial/GitHub2.png)

7. Azure portalında, Azure AD'nin GitHub uygulamanıza bağlanabilmesini sağlamak için **Kiracı URL'sini** ve **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, GitHub hesabınızın Yönetici izinlerine sahip olduğundan ve **Kiracı URl'un** doğru girişi olduğundan emin olun, ardından `https://api.github.com/scim/v2/organizations/<Organization_name>`"Yetki" adımını yeniden deneyin (kurala göre Kiracı **URL'si** oluşturabilirsiniz: , Organizasyonlarınızı GitHub hesabınız altında bulabilirsiniz: **Ayarlar** > **Kuruluşları).**

    ![Yetkilendirme İletişim Kutusu](./media/github-provisioning-tutorial/GitHub3.png)

8. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin "Bir hata oluştuğunda e-posta bildirimi gönderin."

9. **Kaydet**'e tıklayın.

10. Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını GitHub'a Senkronize Et'i**seçin.

11. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den GitHub'a eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için GitHub'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

12. GitHub için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Açık** olarak değiştirin

13. **Kaydet**'e tıklayın.

Bu işlem, Kullanıcılar ve Gruplar bölümünde GitHub'a atanan kullanıcıların ve/veya grupların ilk eşitlemibaşlar. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. İlerlemeyi izlemek ve sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
