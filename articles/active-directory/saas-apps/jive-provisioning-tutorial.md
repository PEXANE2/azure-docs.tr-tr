---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Jive'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory ve Jive arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6fbfdbe7-d66c-4305-9fea-76d6a6a92830
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 602eed65745eea1fd9096508c442a27ea79bcba9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057743"
---
# <a name="tutorial-configure-jive-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Jive'i yapılandır

Bu öğreticinin amacı, Azure AD'den Jive'e kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için Jive ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

*   Azure Etkin dizin kiracı.
*   Etkin abonelikte Jive tek oturum.
*   Team Admin izinleri olan Jive'deki bir kullanıcı hesabı.

## <a name="assigning-users-to-jive"></a>Kullanıcıları Jive'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Jive uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları jive uygulamanıza buradaki talimatları izleyerek atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Jive'a kullanıcı atamak için önemli ipuçları

*   Sağlama yapılandırmasını sınamak için Jive'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

*   Jive'a bir kullanıcı atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı Sağlama'yı etkinleştirme

Bu bölüm, Azure AD'nizi Jive'in kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini, Azure AD'deki kullanıcı ve grup atamasına göre Jive'de atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmanız konusunda size rehberlik eder.

> [!TIP]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek Jive için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Jive'e kullanıcı sağlamasını nasıl etkinleştireceklerini ana hatlarını ortaya çıkarmaktır.
Bu yordamın bir parçası olarak, Jive.com talep etmek için gereken bir kullanıcı güvenlik belirteci sağlamanız gerekir.

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

1. Jive'i zaten tek oturum açma için yapılandırıldıysanız, arama alanını kullanarak Jive örneğini arayın. Aksi takdirde, **ekle'yi** seçin ve uygulama galerisinde **Jive'i** arayın. Arama sonuçlarından Jive'i seçin ve uygulama listenize ekleyin.

1. Jive örneğini seçin ve ardından **Sağlama** sekmesini seçin.

1. Sağlama **Modunu** **Otomatik**olarak ayarlayın. 

    ![Sağlama](./media/jive-provisioning-tutorial/provisioning.png)

1. Yönetici **Kimlik Bilgileri** bölümünün altında aşağıdaki yapılandırma ayarlarını sağlayın:
   
    a. **Jive Admin Kullanıcı Adı** metin kutusuna, Jive.com atanan **Sistem Yöneticisi** profiline sahip bir Jive hesap adı yazın.
   
    b. **Jive Admin Password** textbox'a bu hesabın parolasını yazın.
   
    c. **Jive Tenant URL** textbox'ına, Jive kiracı URL'sini yazın.
      
      > [!NOTE]
      > Jive kiracı URL'si, kuruluşunuz tarafından Jive'de oturum açmak için kullanılan URL'dir.  
      > Genellikle, URL aşağıdaki biçimi vardır: **www.\< organizasyon\>.jive.com**.          

1. Azure portalında, Azure AD'nin Jive uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın.

1. **Bildirim E-posta** alanında ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

1. **Kaydet'i tıklatın.**

1. Eşlemeler bölümünde, **Jive için Azure Etkin Dizin Kullanıcılarını Senkronize Et'i seçin.**

1. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Jive'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için Jive'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

1. Jive için Azure AD sağlama hizmetini etkinleştirmek için, Ayarlar bölümünde **Sağlama Durumunu** **Ayarı'nda**

1. **Kaydet'i tıklatın.**

Kullanıcılar ve Gruplar bölümünde Jive'e atanan kullanıcıların ve/veya grupların ilk senkronizasyonunu başlatır. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. Jive uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Tek Oturum Açma'yı Yapılandır](jive-tutorial.md)
