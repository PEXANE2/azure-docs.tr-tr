---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Salesforce Sandbox'ı yapılandırın| Microsoft Dokümanlar"
description: Azure Active Directory ve Salesforce Sandbox arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: bab73fda-6754-411d-9288-f73ecdaa486d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48df954f680c6774b0aa189453615156f4f0b4a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063280"
---
# <a name="tutorial-configure-salesforce-sandbox-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Salesforce Sandbox'ı yapılandırın

Bu öğreticinin amacı, Azure AD'den Salesforce Sandbox'a kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan sağlamak için Salesforce Sandbox ve Azure AD'de gerçekleştirmeniz gereken adımları size göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

*   Azure Etkin dizin kiracı.
*   Salesforce Sandbox for Work veya Salesforce Sandbox for Education için geçerli bir kiracı. Her iki hizmet için de ücretsiz deneme hesabı kullanabilirsiniz.
*   Team Admin izinlerine sahip Salesforce Sandbox'taki bir kullanıcı hesabı.

## <a name="assigning-users-to-salesforce-sandbox"></a>Kullanıcıları Salesforce Sandbox'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Salesforce Sandbox uygulamanıza erişilmesi gerektiğine karar vermeniz gerekir. Bu kararı verdikten sonra, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) yönergelerini izleyerek bu kullanıcıları Salesforce Sandbox uygulamanıza atayabilirsiniz

### <a name="important-tips-for-assigning-users-to-salesforce-sandbox"></a>Kullanıcıları Salesforce Sandbox'a atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için Salesforce Sandbox'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Salesforce Sandbox'a atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

> [!NOTE]
> Bu uygulama, müşterinin kullanıcıları atarken seçmek isteyebileceği sağlama işleminin bir parçası olarak Salesforce Sandbox'tan özel roller aktarıyor.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirme

Bu bölüm, Azure REKLAM'ınızı Salesforce Sandbox'ın kullanıcı hesabı sağlama API'sine bağlamanız ve kullanıcı ve gruba göre Salesforce Sandbox'ta atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı etmek için sağlama hizmetini yapılandırmak için size yol gösteriyor Azure AD'de atama.

>[!Tip]
>[Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek Salesforce Sandbox için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="configure-automatic-user-account-provisioning"></a>Otomatik kullanıcı hesabı sağlama yapılandırma

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Salesforce Sandbox'a kullanıcı sağlamasını nasıl etkinleştireceklerini ana hatlarını ortaya çıkarmaktır.

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

1. Salesforce Sandbox'ı tek oturum açma için zaten yapılandırıldıysanız, arama alanını kullanarak Salesforce Sandbox örneğini arayın. Aksi takdirde, uygulama galerisinde **Salesforce Sandbox'ı** **ekle** ve arama'yı seçin. Arama sonuçlarından Salesforce Sandbox'ı seçin ve uygulama listenize ekleyin.

1. Salesforce Sandbox örneğini seçin ve ardından **Sağlama** sekmesini seçin.

1. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama](./media/salesforce-sandbox-provisioning-tutorial/provisioning.png)

1. Yönetici **Kimlik Bilgileri** bölümünün altında aşağıdaki yapılandırma ayarlarını sağlayın:
   
    a. Yönetici **Kullanıcı Adı** metin kutusuna, Salesforce.com atanan **Sistem Yöneticisi** profiline sahip bir Salesforce Sandbox hesap adı yazın.
   
    b. Yönetici **Parolası** metin kutusuna, bu hesabın parolasını yazın.

1. Salesforce Sandbox güvenlik belirtecinizi almak için yeni bir sekme açın ve aynı Salesforce Sandbox yönetici hesabında oturum açın. Sayfanın sağ üst köşesinde adınızı tıklatın ve ardından **Ayarlar'ı**tıklatın.

     ![Otomatik kullanıcı sağlamayı etkinleştirme](./media/salesforce-sandbox-provisioning-tutorial/sf-my-settings.png "Otomatik kullanıcı sağlamayı etkinleştirme")

1. Soldaki gezinti bölmesinde, ilgili bölümü genişletmek için **Kişisel Bilgilerim'i** tıklatın ve ardından **Güvenlik Belirtecimi Sıfırla'yı**tıklatın.
  
    ![Otomatik kullanıcı sağlamayı etkinleştirme](./media/salesforce-sandbox-provisioning-tutorial/sf-personal-reset.png "Otomatik kullanıcı sağlamayı etkinleştirme")

1. Güvenlik **Belirteci'ni Sıfırla** sayfasında, **Güvenlik Belirteci'ni Sıfırla** düğmesini tıklatın.

    ![Otomatik kullanıcı sağlamayı etkinleştirme](./media/salesforce-sandbox-provisioning-tutorial/sf-reset-token.png "Otomatik kullanıcı sağlamayı etkinleştirme")

1. Bu yönetici hesabıyla ilişkili e-posta gelen kutusunu denetleyin. Salesforce Sandbox.com'den yeni güvenlik belirteci içeren bir e-posta arayın.

1. Belirteci kopyalayın, Azure REKLAM pencerenize gidin ve **Gizli Belirteç** alanına yapıştırın.

1. Azure portalında, Azure AD'nin Salesforce Sandbox uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın.

1. Bildirim **E-postası** alanına, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin.

1. **Kaydet'i tıklatın.**  
    
1.  Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını Salesforce Sandbox'a Senkronize Et'i seçin.**

1. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Salesforce Sandbox'a senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Salesforce Sandbox'taki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

1. Salesforce Sandbox için Azure AD sağlama hizmetini etkinleştirmek için, Ayarlar bölümünde **Sağlama Durumunu** **Ayarı'nda**

1. **Kaydet'i tıklatın.**

Kullanıcılar ve Gruplar bölümünde Salesforce Sandbox'a atanan kullanıcıların ve/veya grupların ilk senkronizasyonunu başlatır. İlk eşitlemenin gerçekleştirilemi, hizmet yürütülürken yaklaşık her 40 dakikada bir meydana gelen sonraki eşitlemelerden daha uzun sürüyor. Salesforce Sandbox uygulamasında sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Tek Oturum Açma'yı Yapılandır](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-sandbox-tutorial)
