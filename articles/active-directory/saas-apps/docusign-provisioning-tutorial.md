---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için DocuSign'ı yapılandırın| Microsoft Dokümanlar"
description: Azure Active Directory ve DocuSign arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 294cd6b8-74d7-44bc-92bc-020ccd13ff12
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88b65c8e8962ad8420ded47da1a343672123c589
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058187"
---
# <a name="tutorial-configure-docusign-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için DocuSign'ı yapılandırın

Bu öğreticinin amacı, Azure AD'den DocuSign'a kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için DocuSign ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

*   Azure Etkin dizin kiracı.
*   DocuSign tek oturum açma özellikli abonelik.
*   Ekip Yöneticisi izinleri ile DocuSign'daki bir kullanıcı hesabı.

## <a name="assigning-users-to-docusign"></a>Kullanıcıları DocuSign'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların DocuSign uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları docuSign uygulamanıza aşağıdaki talimatları izleyerek atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-docusign"></a>Kullanıcıları DocuSign'a atamak için önemli ipuçları

*   Sağlama yapılandırmasını sınamak için DocuSign'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar daha sonra atanabilir.

*   Bir kullanıcıyı DocuSign'a atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

> [!NOTE]
> Azure AD, Docusign uygulamasıyla grup sağlamayı desteklemez, yalnızca kullanıcılar kullanılabilir.

## <a name="enable-user-provisioning"></a>Kullanıcı Sağlama'yı etkinleştirme

Bu bölüm, Azure REKLAM'ınızı DocuSign'ın kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini, Azure AD'deki kullanıcı ve grup atamasına göre DocuSign'da atanmış kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı edecek şekilde yapılandırmanız yoluyla size yol gösteriş yapar.

> [!Tip]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek DocuSign için SAML tabanlı Tek Oturum Açma özelliğini de seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-user-account-provisioning"></a>Kullanıcı hesabı sağlama yapılandırmak için:

Bu bölümün amacı, Active Directory kullanıcı hesaplarının DocuSign'a kullanıcı sağlamasını nasıl etkinleştireceklerini ana hatlarını ortaya çıkarmaktır.

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

1. Tek oturum açma için DocuSign'ı zaten yapılandırmışsanız, arama alanını kullanarak DocuSign örneğinizi arayın. Aksi takdirde, uygulama galerisinde **DocuSign'ı ekle** ve arama'yı seçin. **DocuSign** Arama sonuçlarından DocuSign'ı seçin ve uygulama listenize ekleyin.

1. DocuSign örneğini seçin ve ardından **Sağlama** sekmesini seçin.

1. Sağlama **Modunu** **Otomatik**olarak ayarlayın. 

    ![Sağlama](./media/docusign-provisioning-tutorial/provisioning.png)

1. Yönetici **Kimlik Bilgileri** bölümünün altında aşağıdaki yapılandırma ayarlarını sağlayın:
   
    a. Yönetici **Kullanıcı Adı** metin kutusuna, DocuSign.com atanan Sistem **Yöneticisi** profiline sahip bir DocuSign hesap adı yazın.
   
    b. Yönetici **Parolası** metin kutusuna, bu hesabın parolasını yazın.

1. Azure portalında, Azure AD'nin DocuSign uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın.

1. Bildirim **E-postası** alanına, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

1. **Kaydet'i tıklatın.**

1. Eşlemeler bölümünde, **DocuSign için Azure Etkin Dizin Kullanıcılarını Eşitle'yi seçin.**

1. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den DocuSign'a senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için DocuSign'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

1. DocuSign için Azure AD sağlama hizmetini etkinleştirmek için, **On** Ayarlar bölümünde **Sağlama Durumunu**

1. **Kaydet'i tıklatın.**

Kullanıcılar ve Gruplar bölümünde DocuSign'a atanan tüm kullanıcıların ilk eşitlemasını başlatır. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. DocuSign uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Tek Oturum Açma'yı Yapılandır](docusign-tutorial.md)
