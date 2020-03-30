---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için GoToMeeting'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory ve GoToMeeting arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0ac06fc3018b4230cbf32712067c48400599082
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058272"
---
# <a name="tutorial-configure-gotomeeting-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için GoToMeeting'i yapılandırın

Bu öğreticinin amacı, Azure AD'den GoToMeeting'e kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan sağlamak için GoToMeeting ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

*   Azure Etkin dizin kiracı.
*   GoToMeeting tek oturum açma özellikli abonelik.
*   Takım Yöneticisi izinleriyle GoToMeeting'de bir kullanıcı hesabı.

## <a name="assigning-users-to-gotomeeting"></a>Kullanıcıları GoToMeeting'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların GoToMeeting uygulamanıza erişilmesi gereken kullanıcıları temsil edeceğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları gotomeeting uygulamanıza buradaki talimatları izleyerek atayabilirsiniz:

[Bir kurumsal uygulamaya kullanıcı veya grup atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-gotomeeting"></a>Kullanıcıları GoToMeeting'e atamak için önemli ipuçları

*   Sağlama yapılandırmasını sınamak için Tek bir Azure AD kullanıcısının GoToMeeting'e atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

*   Bir kullanıcıyı GoToMeeting'e atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="enable-automated-user-provisioning"></a>Otomatik Kullanıcı Sağlama'yı etkinleştirme

Bu bölüm, Azure REKLAM'ınızı GoToMeeting'in kullanıcı hesabı sağlama API'sine bağlamanız ve sağlama hizmetini Azure AD'deki kullanıcı ve grup atamasına göre GoToMeeting'de atanan kullanıcı hesapları oluşturacak, güncelleştirecek ve devre dışı bırakacak şekilde yapılandırmanız yoluyla size yol gösterin.

> [!TIP]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek GoToMeeting için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-account-provisioning"></a>Otomatik kullanıcı hesabı sağlama yapılandırmak için:

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

1. GoToMeeting'i tek oturum açma için zaten yapılandırıldıysanız, arama alanını kullanarak GoToMeeting örneğini arayın. Aksi takdirde, **Ekle'yi** seçin ve uygulama galerisinde **GoToMeeting'i** arayın. Arama sonuçlarından GoToMeeting'i seçin ve uygulama listenize ekleyin.

1. GoToMeeting örneğini seçin ve ardından **Sağlama** sekmesini seçin.

1. **Sağlama** Modunu **Otomatik**olarak ayarlayın. 

    ![Sağlama](./media/citrixgotomeeting-provisioning-tutorial/provisioning.png)

1. Yönetici Kimlik Bilgileri bölümünde aşağıdaki adımları gerçekleştirin:
   
    a. **GoToMeeting Admin Kullanıcı Adı metin kutusuna,** bir yöneticinin kullanıcı adını yazın.

    b. **GoToMeeting Admin Password** textbox'ta yöneticinin şifresi.

1. Azure portalında, Azure AD'nin GoToMeeting uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, GoToMeeting hesabınızda Team Admin izinleri olduğundan emin olun ve **"Yönetici Kimlik Bilgileri"** adımını yeniden deneyin.

1. **Bildirim E-postaalanında** ödeme hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

1. **Kaydet'i tıklatın.**

1. Eşlemeler bölümünde, **GToMeeting için Azure Etkin Dizin Kullanıcılarını Eşitle'yi seçin.**

1. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den GoToMeeting'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için GoToMeeting'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

1. GoToMeeting için Azure AD sağlama hizmetini etkinleştirmek için, **On** Ayarlar bölümünde **KiSAma Durumunu**

1. **Kaydet'i tıklatın.**

Kullanıcılar ve Gruplar bölümünde GoToMeeting'e atanan kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. GoToMeeting uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine giden bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Tek Oturum Açma'yı Yapılandır](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrix-gotomeeting-tutorial)


