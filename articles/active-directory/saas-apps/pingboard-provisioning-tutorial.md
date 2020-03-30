---
title: 'Öğretici: Pingboard için kullanıcı sağlama - Azure AD'
description: Azure Active Directory'yi, kullanıcı hesaplarını Pingboard'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: c64c40d93f9b525ac6adeca276797df65f32ef3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061284"
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Pingboard'u yapılandır

Bu öğreticinin amacı, Azure Active Directory'den (Azure AD) Pingboard'a kullanıcı hesaplarının otomatik olarak sağlanmasını ve bunların sağlanmasını etkinleştirmek için izlemeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure AD kiracı
* Bir Pingboard kiracı [Pro hesabı](https://pingboard.com/pricing)
* Pingboard'da yönetici izinleri olan bir kullanıcı hesabı

> [!NOTE]
> Azure AD sağlama tümleştirmesi, hesabınızda kullanılabilen [Pingboard API'sine](https://pingboard.docs.apiary.io/#)dayanır.

## <a name="assign-users-to-pingboard"></a>Kullanıcıları Pingboard'a atama

Azure AD, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar eşitlenir. 

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların Pingboard uygulamanıza erişmesi gerektiğine karar vermeniz gerekir. Daha sonra bu kullanıcıları pingboard uygulamanıza buradaki talimatları izleyerek atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Pingboard'a kullanıcı atamak için önemli ipuçları

Sağlama yapılandırmasını test etmek için Pingboard'a tek bir Azure AD kullanıcısı atamanızı öneririz. Ek kullanıcılar daha sonra atanabilir.

## <a name="configure-user-provisioning-to-pingboard"></a>Kullanıcı sağlamayı Pingboard'a yapılandırma 

Bu bölüm, Azure REKLAM'ınızı Pingboard kullanıcı hesabı sağlama API'sine bağlama konusunda size rehberlik eder. Ayrıca, Sağlama hizmetini, Pingboard'da Azure AD'deki kullanıcı atamalarını temel alan atanmış kullanıcı hesapları oluşturmak, güncelleştirmek ve devre dışı etmek için yapılandırırsınız.

> [!TIP]
> Pingboard için SAML tabanlı tek oturum açmayı etkinleştirmek için [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyin. Tek oturum açma, bu iki özellik birbirini tamamlasa da, otomatik sağlamadan bağımsız olarak yapılandırılabilir.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>Azure AD'de Pingboard'a otomatik kullanıcı hesabı sağlama yapılandırmak için

1. Azure [portalında,](https://portal.azure.com) **Azure Active Directory** > **Enterprise Apps** > **Tüm uygulamalar** bölümüne göz atın.

1. Pingboard'u zaten tek oturum açma için yapılandırıldıysanız, arama alanını kullanarak Pingboard örneğinizi arayın. Aksi takdirde, **ekle'yi** seçin ve uygulama galerisinde **Pingboard'u** arayın. Arama sonuçlarından **Pingboard'u** seçin ve uygulama listenize ekleyin.

1. Pingboard örneğini seçin ve ardından **Sağlama** sekmesini seçin.

1. **Sağlama Modunu** **Otomatik**olarak ayarlayın.

    ![Pingboard Sağlama](./media/pingboard-provisioning-tutorial/pingboardazureprovisioning.png)

1. Yönetici **Kimlik Bilgileri** bölümünde aşağıdaki adımları kullanın:

    a. **Kiracı URL'sinde**,girin `https://your_domain.pingboard.com/scim/v2`ve "your_domain"yi gerçek etki alanınızınla değiştirin.

    b. Yönetici hesabınızı kullanarak [Pingboard'da](https://pingboard.com/) oturum açın.

    c. **EklentiTümleştirmeleri** > **Integrations** > **Azure Etkin Dizini'ni**seçin.

    d. **Yapılaşı** sekmesine gidin ve **Azure'dan kullanıcı sağlamayı etkinleştir'i**seçin.

    e. **OAuth Bearer Belirteci**belirteci kopyalayın ve **Gizli Belirteç**girin.

1. Azure portalında, Azure AD'nin Pingboard uygulamanıza bağlanabileceğini test etmek için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, Pingboard hesabınızın yönetici izinlerine sahip olduğunu test edin ve **Test Bağlantısı** adımını yeniden deneyin.

1. **Bildirim E-postasına**sağlama hatası bildirimleri almak istediğiniz kişinin veya grubun e-posta adresini girin. Altındaki onay kutusunu seçin.

1. **Kaydet'i**seçin.

1. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Pingboard'a Senkronize Et'i**seçin.

1. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Pingboard'a eşitlenecek kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Pingboard'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik gerçekleştirmek için **Kaydet'i** seçin. Daha fazla bilgi için bkz: [Kullanıcı sağlama öznitelik eşlemelerini özelleştirin.](../app-provisioning/customize-application-attributes.md)

1. **Ayarlar** bölümünde Pingboard için Azure AD sağlama hizmetini etkinleştirmek **için, Sağlama Durumunu** **A'da**olarak değiştirin.

1. Pingboard'a atanan kullanıcıların ilk eşitlemasını başlatmak için **Kaydet'i** seçin.

İlk eşitlemenin çalışması, hizmet çalışırken yaklaşık her 40 dakikada bir oluşan eşitlemeleri izleyene göre daha uzun sürer. İlerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanın. Günlükler Pingboard uygulamanızdaki sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklar.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [otomatik kullanıcı hesabı sağlama raporu'na](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](pingboard-tutorial.md)
