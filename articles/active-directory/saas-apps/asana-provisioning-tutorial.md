---
title: 'Öğretici: Asana için kullanıcı sağlama - Azure AD'
description: Azure Active Directory'yi, kullanıcı hesaplarını Asana'ya otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: arvinh
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: abeac030db419f7fb7d561df5dcd407684f20ca2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058940"
---
# <a name="tutorial-configure-asana-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Asana'yı yapılandırın

Bu öğreticinin amacı, Azure AD'den Asana'ya kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için Asana ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Kurumsal](https://www.asana.com/pricing) planı olan veya daha iyi etkin leştirilmiş bir Asana kiracısı
* Yönetici izinleri olan Asana'da bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirmesi, Asana'nın kullanabileceği [Asana API'sine](https://asana.com/developers/api-reference/users)dayanır.

## <a name="assign-users-to-asana"></a>Kullanıcıları Asana'ya atama

Azure AD, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların Asana uygulamanıza erişmesi gerektiğine karar vermelisiniz. Daha sonra bu kullanıcıları asana uygulamanıza aşağıdaki talimatları izleyerek atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-asana"></a>Kullanıcıları Asana'ya atamak için önemli ipuçları

Sağlama yapılandırmasını test etmek için Asana'ya tek bir Azure AD kullanıcısı atamanızı öneririz. Ek kullanıcılar daha sonra atanabilir.

## <a name="configure-user-provisioning-to-asana"></a>Kullanıcı sağlamayı Asana'ya yapılandırma

Bu bölüm, Azure REKLAM'ınızı Asana kullanıcı hesabı sağlama API'sine bağlama konusunda size rehberlik eder. Ayrıca, Sağlama hizmetini Azure AD'deki kullanıcı atamalarına göre Asana'da atanan kullanıcı hesapları oluşturmak, güncelleştirmek ve devre dışı etmek için yapılandırırsınız.

> [!TIP]
> Asana için SAML tabanlı tek oturum açmayı etkinleştirmek için [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyin. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-asana-in-azure-ad"></a>Azure AD'de Asana'ya otomatik kullanıcı hesabı sağlama yapılandırmak için

1. Azure [portalında,](https://portal.azure.com) **Azure Active Directory** > **Enterprise Apps** > **Tüm uygulamalar** bölümüne göz atın.

1. Asana'yı zaten tek oturum açma için yapılandırıldıysanız, arama alanını kullanarak Asana örneğini arayın. Aksi takdirde, uygulama galerisinde **Asana'yı ekle** ve ara'yı seçin. **Asana** Arama sonuçlarından **Asana'yı** seçin ve uygulama listenize ekleyin.

1. Asana örneğini seçin ve ardından **Sağlama** sekmesini seçin.

1. **Sağlama Modunu** **Otomatik**olarak ayarlayın.

    ![Asana Temini](./media/asana-provisioning-tutorial/asanaazureprovisioning.png)

1. Yönetici **Kimlik Bilgileri** bölümü altında, belirteci oluşturmak ve **Gizli Belirteç**girmek için bu yönergeleri izleyin:

    a. Yönetici hesabınızı kullanarak [Asana'da](https://app.asana.com) oturum açın.

    b. Üst çubuktaki profil fotoğrafını seçin ve geçerli kuruluş adı ayarlarınızı seçin.

    c. **Hizmet Hesapları** sekmesine gidin.

    d. **Hizmet Hesabı Ekle'yi**seçin.

    e. **Adı** ve **Hakkında'yı** ve gerektiğinde profil fotoğrafını güncelleştirin. Belirteç'teki **Token**belirteci kopyalayın ve **Değişiklikleri Kaydet'te**seçin.

1. Azure portalında, Azure AD'nin Asana uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Asana hesabınızın yönetici izinlerine sahip olduğundan emin olun ve **Test Bağlantısı** adımını yeniden deneyin.

1. **Bildirim E-postasına**sağlama hatası bildirimleri almak istediğiniz kişinin veya grubun e-posta adresini girin. Altındaki onay kutusunu seçin.

1. **Kaydet'i**seçin.

1. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Asana**ile Senkronize Et'i seçin.

1. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Asana'ya eşitlenecek kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Asana'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin. Daha fazla bilgi için bkz: [Kullanıcı sağlama öznitelik eşlemelerini özelleştirin.](../app-provisioning/customize-application-attributes.md)

1. **Ayarlar** bölümünde, Asana için Azure AD sağlama hizmetini etkinleştirmek **için, Sağlama Durumunu** **A'da**olarak değiştirin.

1. **Kaydet'i**seçin.

Şimdi ilk eşitleme **Kullanıcılar** bölümünde Asana atanan herhangi bir kullanıcı için başlar. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. İlerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanın. Denetim günlükleri, Asana uygulamanızdaki sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [otomatik kullanıcı hesabı sağlama raporu'na](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](asana-tutorial.md)
