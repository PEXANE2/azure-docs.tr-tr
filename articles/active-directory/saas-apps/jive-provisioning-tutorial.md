---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Jive 'yi yapılandırma | Microsoft Docs"
description: Azure Active Directory ve Jive arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77057743"
---
# <a name="tutorial-configure-jive-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için Jive 'yi yapılandırma

Bu öğreticinin amacı, Azure AD 'den Jive 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için Jive ve Azure AD 'de gerçekleştirmeniz gereken adımları gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğelerin zaten olduğunu varsayar:

*   Azure Active Directory kiracısı.
*   Bir Jive çoklu oturum açma etkin aboneliği.
*   Jive 'deki bir kullanıcı hesabı ekip Yöneticisi izinleri ile.

## <a name="assigning-users-to-jive"></a>Kullanıcıları Jive 'ye atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişim alacağını belirleyebilmek için "atamalar" adlı bir kavram kullanır. Otomatik Kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD 'de bir uygulamaya "atanmış" olan kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların Jive uygulamanıza erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları Jive uygulamanıza atayabilirsiniz:

[Kurumsal uygulamaya Kullanıcı veya Grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-jive"></a>Kullanıcıları Jive 'ye atamaya yönelik önemli ipuçları

*   Sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Jive 'ye atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Bir kullanıcıyı Jive 'ye atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı Sağlamayı Etkinleştir

Bu bölümde, Azure AD 'nizi Jive 'nin Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve grup atamasını temel alan Jive 'de atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırma işlemi kılavuzluk eder.

> [!TIP]
> [Azure Portal](https://portal.azure.com)' de sağlanan yönergeleri Izleyerek, Jive için SAML tabanlı çoklu oturum açmayı da seçebilirsiniz. Çoklu oturum açma özelliği otomatik sağlanmadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini karmaşıdirebilirler.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlamayı yapılandırmak için:

Bu bölümün amacı, Kullanıcı hesaplarının Active Directory Kullanıcı tarafından sağlanması için Jive 'nin nasıl etkinleştirileceğini özetler.
Bu yordamın bir parçası olarak, Jive.com ' den istemeniz gereken bir Kullanıcı güvenlik belirteci sağlamanız gerekir.

1. [Azure portal](https://portal.azure.com) **Azure Active Directory > Enterprise Apps > tüm uygulamalar** bölümüne gidin.

1. Çoklu oturum açma için Jive 'yi zaten yapılandırdıysanız arama alanını kullanarak Jive örneğinizi arayın. Aksi takdirde, **Ekle** ' yi seçin ve uygulama galerisinde **Jive** 'yi arayın. Arama sonuçlarından Jive ' yi seçin ve uygulama listenize ekleyin.

1. Jive örneğinizi seçin, sonra **sağlama** sekmesini seçin.

1. **Sağlama modunu** **Otomatik**olarak ayarlayın. 

    ![alınıyor](./media/jive-provisioning-tutorial/provisioning.png)

1. **Yönetici kimlik bilgileri** bölümünde aşağıdaki yapılandırma ayarlarını sağlayın:
   
    a. **Jive Yönetici Kullanıcı adı** metin kutusuna Jive.com atanmış **Sistem Yöneticisi** profiline sahip bir Jive hesap adı yazın.
   
    b. **Jive yönetici parolası** metin kutusuna bu hesabın parolasını yazın.
   
    c. **Jive kiracı URL 'si** metin kutusuna Jive KIRACı URL 'sini yazın.
      
      > [!NOTE]
      > Jive kiracı URL 'SI, kuruluşunuz tarafından Jive 'da oturum açmak için kullanılan URL 'dir.  
      > Genellikle, URL şu biçimdedir: **www.\<organizasyonu\>. Jive.com**.          

1. Azure portal, Azure AD 'nin Jive uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın.

1. **Bildirim e-postası** alanında sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

1. Kaydet ' e tıklayın **.**

1. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları Jive olarak eşitler** ' ı seçin.

1. **Öznitelik eşlemeleri** bölümünde, Azure AD 'Den Jive 'ye eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, Jive güncelleştirme işlemlerinde Kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri kaydetmek için Kaydet düğmesini seçin.

1. Jive için Azure AD sağlama hizmetini etkinleştirmek üzere ayarlar bölümünde **sağlama durumunu** **Açık** olarak değiştirin

1. Kaydet ' e tıklayın **.**

Kullanıcılar ve Gruplar bölümünde Jive 'ye atanan tüm Kullanıcı ve/veya grupların ilk eşitlemesini başlatır. İlk eşitleme hizmeti çalışıyor sürece yaklaşık 40 dakikada oluşan sonraki eşitlemeler uzun sürer. İlerleme durumunu izlemek için **eşitleme ayrıntıları** bölümünü kullanabilir ve Jive uygulamanızdaki sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan etkinlik günlüklerinin sağlanması için bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerinin nasıl okunduğu hakkında daha fazla bilgi için bkz. [Otomatik Kullanıcı hesabı sağlamayı raporlama](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Çoklu oturum açmayı yapılandırma](jive-tutorial.md)
