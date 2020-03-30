---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Netsuite OneWorld'ü yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory ve Netsuite OneWorld arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c9a823e6515c2bfe09e1ab7bcef471eb8169e75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063304"
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Netsuite yapılandırma

Bu öğreticinin amacı, Azure AD'den Netsuite'e kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için Netsuite OneWorld ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

*   Azure Etkin dizin kiracı.
*   Netsuite OneWorld aboneliği. Otomatik kullanıcı sağlamanın şu anda yalnızca NetSuite OneWorld ile desteklenerek desteklendiğini unutmayın.
*   Netsuite'te yönetici izinleri olan bir kullanıcı hesabı.

## <a name="assigning-users-to-netsuite-oneworld"></a>Kullanıcıları Netsuite OneWorld'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların Netsuite uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları buradaki talimatları izleyerek Netsuite uygulamanıza atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite-oneworld"></a>Kullanıcıları Netsuite OneWorld'e atamak için önemli ipuçları

*   Sağlama yapılandırmasını sınamak için Netsuite'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

*   Bir kullanıcıyı Netsuite'e atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="enable-user-provisioning"></a>Kullanıcı Sağlama'yı etkinleştirme

Bu bölüm, Azure REKLAM'ınızı Netsuite'in kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini Azure AD'deki kullanıcı ve grup atamasına göre Netsuite'te atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmanız konusunda size rehberlik eder.

> [!TIP] 
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek Netsuite için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Netsuite'e kullanıcı sağlamasını nasıl etkinleştireceklerini ana hatlarını ortaya çıkarmaktır.

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

1. Netsuite'i tek oturum açma için zaten yapılandırdıysanız, arama alanını kullanarak Netsuite örneğinizi arayın. Aksi takdirde, uygulama galerisinde **Netsuite'i** **ekle** ve ara'yı seçin. Arama sonuçlarından Netsuite'i seçin ve uygulama listenize ekleyin.

1. Netsuite örneğini seçin ve ardından **Sağlama** sekmesini seçin.

1. Sağlama **Modunu** **Otomatik**olarak ayarlayın. 

    ![Sağlama](./media/netsuite-provisioning-tutorial/provisioning.png)

1. Yönetici **Kimlik Bilgileri** bölümünün altında aşağıdaki yapılandırma ayarlarını sağlayın:
   
    a. Yönetici **Kullanıcı Adı** metin kutusuna, Netsuite.com atanan **Sistem Yöneticisi** profiline sahip bir Netsuite hesap adı yazın.
   
    b. Yönetici **Parolası** metin kutusuna, bu hesabın parolasını yazın.
      
1. Azure portalında, Azure AD'nin Netsuite uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın.

1. Bildirim **E-postası** alanına, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

1. **Kaydet'i tıklatın.**

1. Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını Netsuite'e Senkronize Et'i seçin.**

1. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Netsuite'e senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen özniteliklerin, güncelleştirme işlemleri için Netsuite'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanıldığını unutmayın. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

1. Netsuite için Azure AD sağlama hizmetini etkinleştirmek için, **On** Ayarlar bölümünde **KiSama Durumunu**

1. **Kaydet'i tıklatın.**

Kullanıcılar ve Gruplar bölümünde Netsuite'e atanan kullanıcıların ve/veya grupların ilk senkronizasyonunu başlatır. İlk eşitlemenin, hizmet çalışırken yaklaşık her 40 dakikada bir oluşan sonraki eşitlemelerden daha uzun sürdüğünü unutmayın. Netsuite uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Tek Oturum Açma'yı Yapılandır](netsuite-tutorial.md)
