---
title: "Öğretici: Azure Active Directory kullanarak otomatik kullanıcı sağlama için Foodee'yi yapılandırın | Microsoft Dokümanlar"
description: Kullanıcı hesaplarını Foodee'ye otomatik olarak sağlamak ve yok etmek için Azure Active Directory'yi nasıl yapılandıracak larını öğrenin.
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
ms.date: 08/30/2019
ms.author: Zhchia
ms.openlocfilehash: 2195056ec66550063aba5ce5e2b977b51a6dc5e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057837"
---
# <a name="tutorial-configure-foodee-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Foodee'yi yapılandırın

Bu makalede, Foodee ve Azure AD'deki Azure Etkin Dizini'ni (Azure AD) kullanıcıları veya grupları Foodee'ye otomatik olarak sağlamak veya devre dışı etmek için nasıl yapılandırabileceğinizgöster.

> [!NOTE]
> Makalede, Azure AD Kullanıcı Sağlama hizmetinin üzerine inşa edilmiş bir bağlayıcı açıklanmaktadır. Bu hizmetin ne işe yaradığını ve nasıl çalıştığını öğrenmek ve sık sorulan soruların yanıtlarını almak için Azure [Etkin Dizini ile SaaS uygulamalarını otomatikleştir](../app-provisioning/user-provisioning.md)ve yok etme konusuna bakın.
>
> Bu bağlayıcı şu anda önizlemede. Önizleme özellikleri için Azure kullanım koşulları özelliği hakkında daha fazla bilgi için [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bölümüne gidin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, aşağıdaki ön koşulları yerine getirdiğinizi varsayar:

* Azure AD kiracı
* [Bir Foodee kiracı](https://www.food.ee/about/)
* Admin izinleri olan Foodee'de bir kullanıcı hesabı

## <a name="assign-users-to-foodee"></a>Foodee'ye kullanıcı atama 

Azure AD, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Foodee'ye erişmesi gerektiğine karar vermelisiniz. Bu belirlemeyi yaptıktan sonra, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](../manage-apps/assign-user-or-group-access-portal.md)yönergelerini izleyerek bu kullanıcıları veya grupları Foodee'ye atayabilirsiniz.

## <a name="important-tips-for-assigning-users-to-foodee"></a>Foodee'ye kullanıcı atamak için önemli ipuçları 

Kullanıcıları atarken aşağıdaki ipuçlarını aklınızda bulundurun:

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için Foodee'ye yalnızca tek bir Azure AD kullanıcısı atamanızı öneririz. Daha sonra ek kullanıcılar veya gruplar atayabilirsiniz.

* Foodee'ye bir kullanıcı atadığınızda, **Atama** bölmesinde varsa uygulamaya özgü geçerli bir rolü seçin. *Varsayılan Erişim* rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-foodee-for-provisioning"></a>Tedarik için Foodee'yi ayarlama

Azure AD'yi kullanarak Foodee'yi otomatik kullanıcı sağlama için yapılandırmadan önce, Foodee'de Etki Alanları Arası Kimlik Yönetimi Sistemi (SCIM) sağlamayı etkinleştirmeniz gerekir.

1. [Foodee'de](https://www.food.ee/login/)oturum açın ve kiracı kimliğinizi seçin.

    ![Foodee](media/Foodee-provisioning-tutorial/tenant.png)

1. **Enterprise portalı**altında, Tek **İşaret Açma'yı**seçin.

    ![Foodee Enterprise Portal sol bölme menüsü](media/Foodee-provisioning-tutorial/scim.png)

1. Daha sonra kullanmak üzere **API Token** kutusundaki değeri kopyalayın. Azure portalındaki Foodee uygulamanızın **Sağlama** sekmesindeki **Gizli Belirteç** kutusuna girersiniz.

    ![Foodee](media/Foodee-provisioning-tutorial/token.png)

## <a name="add-foodee-from-the-gallery"></a>Galeriden Foodee ekle

Azure AD'yi kullanarak Foodee'yi otomatik kullanıcı sağlama için yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Foodee eklemeniz gerekir.

Azure AD uygulama galerisinden Foodee eklemek için aşağıdakileri yapın:

1. Azure [portalında,](https://portal.azure.com)sol bölmede **Azure Etkin Dizini'ni**seçin.

    ![Azure Etkin Dizin komutu](common/select-azuread.png)

1. **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmında **Yeni uygulama'yı** seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

1. Arama kutusuna **Foodee'yi**girin, sonuç bölmesinde **Foodee'yi** seçin ve uygulamayı eklemek için **Ekle'yi** seçin.

    ![Sonuçlar listesinde Foodee](common/search-new-app.png)

## <a name="configure-automatic-user-provisioning-to-foodee"></a>Foodee için otomatik kullanıcı sağlama yapılandırma 

Bu bölümde, Azure AD sağlama hizmetini, Azure AD'deki kullanıcı veya grup atamalarına göre Foodee'deki kullanıcıları veya grupları oluşturacak, güncelleştirecek ve devre dışı kacak şekilde yapılandırırsınız.

> [!TIP]
> Ayrıca Foodee tek oturum açma öğreticisindeki talimatları izleyerek [Foodee](Foodee-tutorial.md)için SAML tabanlı tek oturum açma yı da etkinleştirebilirsiniz. Bu iki özellik birbirini tamamlasa da, otomatik kullanıcı sağlamadan bağımsız olarak tek oturum açma yapılandırabilirsiniz.

Azure AD'de Foodee için otomatik kullanıcı sağlamayı aşağıdakileri yaparak yapılandırın:

1. Azure [portalında](https://portal.azure.com)Kurumsal **Uygulamalar** > **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bölmesi](common/enterprise-applications.png)

1. **Uygulamalar** listesinde **Foodee'yi**seçin.

    ![Uygulamalar listesindeki Foodee bağlantısı](common/all-applications.png)

1. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

1. Sağlama **Modu** açılır listesinde **Otomatik'i**seçin.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

1. **Yönetici Kimlik Bilgileri**altında aşağıdakileri yapın:

   a. Kiracı **URL** kutusuna, daha önce aldığınız **https:\//concierge.food.ee/scim/v2** değerini girin.

   b. Gizli **Belirteç** kutusuna, daha önce aldığınız **API Belirteci** değerini girin.
   
   c. Azure AD'nin Foodee'ye bağlanabilmesini sağlamak için **Test Bağlantısı'nı**seçin. Bağlantı başarısız olursa, Foodee hesabınızın yönetici izinlerine sahip olduğundan emin olun ve sonra yeniden deneyin.

    ![Test Bağlantısı bağlantısı](common/provisioning-testconnection-tenanturltoken.png)

1. Bildirim **E-posta** kutusuna, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **ardından bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-posta metin kutusu](common/provisioning-notification-email.png)

1. **Kaydet'i**seçin.

1. **Eşlemeler**altında, **Azure Etkin Dizin Kullanıcılarını Foodee**ile Senkronize Etme'yi seçin.

    ![Foodee kullanıcı haritalamaları](media/Foodee-provisioning-tutorial/usermapping.png)

1. **Öznitelik Eşlemeleri**altında, Azure AD'den Foodee'ye senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Foodee'deki *kullanıcı hesaplarıyla* eşleşecek şekilde kullanılır. 

    ![Foodee kullanıcı haritalamaları](media/Foodee-provisioning-tutorial/userattribute.png)

1. Değişikliklerinizi işlemek için **Kaydet'i**seçin.
1. **Eşlemeler**altında, **Azure Etkin Dizin Gruplarını Foodee'ye Senkronize Et'i**seçin.

    ![Foodee kullanıcı haritalamaları](media/Foodee-provisioning-tutorial/groupmapping.png)

1. **Öznitelik Eşlemeleri**altında, Azure AD'den Foodee'ye senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Foodee'deki *grup hesaplarıyla* eşleşecek şekilde kullanılır.

    ![Foodee kullanıcı haritalamaları](media/Foodee-provisioning-tutorial/groupattribute.png)

1. Değişikliklerinizi işlemek için **Kaydet'i**seçin.
1. Kapsam filtrelerini yapılandırın. Nasıl yapılacağını öğrenmek [için, Kapsam filtresi öğreticisindeki talimatlara](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)bakın.

1. Foodee için Azure AD sağlama hizmetini **Etkinleştirmek** için Ayarlar bölümünde, **Sağlama Durumunu** **A.B.K.** olarak değiştirin.

    ![Sağlama Durumu anahtarı](common/provisioning-toggle-on.png)

1. **Ayarlar**altında, **Kapsam** açılır listesinde, Foodee'ye sağlamak istediğiniz kullanıcıları veya grupları tanımlayın.

    ![Sağlama Kapsamı açılır listesi](common/provisioning-scope.png)

1. Hükmetmeye hazır olduğunuzda **Kaydet'i**seçin.

    ![Prove Yapılandırma Kaydet düğmesi](common/provisioning-configuration-save.png)

Önceki işlem, **Kapsam** açılır listesinde tanımladığınız kullanıcıların veya grupların ilk eşitlesini başlatır. İlk eşitlemenin gerçekleştirilimi sonraki eşitlemelerden daha uzun sürer. Daha fazla bilgi için, [kullanıcılara sağlamanın ne kadar süreceğini görün?](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)

İlerlemeyi izlemek ve sağlama faaliyet raporunuzdaki bağlantıları izlemek için **Geçerli Durum** bölümünü kullanabilirsiniz. Rapor, Foodee'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklar. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
