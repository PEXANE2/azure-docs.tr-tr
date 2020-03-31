---
title: "Öğretici: Azure Active Directory| ile otomatik kullanıcı sağlama için Salesforce'u yapılandırın| Microsoft Dokümanlar"
description: Azure Active Directory ve Salesforce arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 49384b8b-3836-4eb1-b438-1c46bb9baf6f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b8038896a11b65e835ce71f5fc34e85723cc91a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060530"
---
# <a name="tutorial-configure-salesforce-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Salesforce'u yapılandır

Bu öğreticinin amacı, Azure AD'den Salesforce'a kullanıcı hesaplarını otomatik olarak sağlamak ve sağlamadan çıkarmak için Salesforce ve Azure AD'de gerçekleştirmek için gereken adımları göstermektir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, zaten aşağıdaki öğelere sahip olduğunuzu varsayar:

* Azure Etkin dizin kiracı
* Bir Salesforce.com kiracı

> [!IMPORTANT]
> Salesforce.com bir deneme hesabı kullanıyorsanız, otomatik kullanıcı sağlama yapılandırması yapamazsınız. Deneme hesapları satın alınana kadar gerekli API erişimi etkin değildir. Bu öğreticiyi tamamlamak için ücretsiz bir [geliştirici hesabı](https://developer.salesforce.com/signup) kullanarak bu sınırlamayı atlatabilirsiniz.

Salesforce Sandbox ortamı kullanıyorsanız, lütfen [Salesforce Sandbox tümleştirme öğreticisini](https://go.microsoft.com/fwLink/?LinkID=521879)görün.

## <a name="assigning-users-to-salesforce"></a>Kullanıcıları Salesforce'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için "atamalar" adlı bir kavram kullanır. Otomatik kullanıcı hesabı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya "atanmış" kullanıcılar ve gruplar eşitlenir.

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların veya grupların Salesforce uygulamanıza erişilmesi gerektiğine karar vermeniz gerekir. Bu kararı verdikten sonra, [bir kullanıcıyı veya grubu kurumsal bir uygulamaya atama](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) yönergelerini izleyerek bu kullanıcıları Salesforce uygulamanıza atayabilirsiniz

### <a name="important-tips-for-assigning-users-to-salesforce"></a>Kullanıcıları Salesforce'a atamak için önemli ipuçları

* Sağlama yapılandırmasını sınamak için Salesforce'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Salesforce'a atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü,

    > [!NOTE]
    > Bu uygulama, müşterinin Azure AD'deki kullanıcıları atarken seçmek isteyebileceği sağlama işleminin bir parçası olarak Salesforce'tan profiller aktarıyor. Salesforce'tan alınan profillerin Azure AD'de Roller olarak göründüğünü lütfen unutmayın.

## <a name="enable-automated-user-provisioning"></a>Otomatik kullanıcı sağlamayı etkinleştirme

Bu bölüm, Azure REKLAM'ınızı [Salesforce'un kullanıcı hesabı sağlama API - v40'ına](https://developer.salesforce.com/docs/atlas.en-us.208.0.api.meta/api/implementation_considerations.htm)bağlamave sağlama hizmetini, Azure AD'deki kullanıcı ve grup atamasına göre Salesforce'ta atanmış kullanıcı hesapları oluşturmak, güncelleştirmek ve devre dışı edecek şekilde yapılandırmak için size rehberlik eder.

> [!Tip]
> [Azure portalında](https://portal.azure.com)sağlanan yönergeleri izleyerek, Satış Gücü için SAML tabanlı Tek Oturum Açma özelliğini de etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="configure-automatic-user-account-provisioning"></a>Otomatik kullanıcı hesabı sağlama yapılandırma

Bu bölümün amacı, Active Directory kullanıcı hesaplarının Salesforce'a kullanıcı sağlamasını nasıl etkinleştireceklerini ana hatlarını ortaya çıkarmaktır.

1. Azure [portalında,](https://portal.azure.com)Tüm uygulamalar > **Azure Active Directory > Enterprise Apps'a** göz atın.

2. Salesforce'u tek oturum açma için zaten yapılandırıldıysanız, arama alanını kullanarak Salesforce örneğinizi arayın. Aksi takdirde, uygulama galerisinde **Salesforce'u ekle** ve arama'yı seçin. **Salesforce** Arama sonuçlarından Salesforce'u seçin ve uygulama listenize ekleyin.

3. Salesforce örneğini seçin ve ardından **Sağlama** sekmesini seçin.

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama](./media/salesforce-provisioning-tutorial/provisioning.png)

5. Yönetici **Kimlik Bilgileri** bölümünün altında aşağıdaki yapılandırma ayarlarını sağlayın:

    a. Yönetici **Kullanıcı Adı** metin kutusunda, Salesforce.com atanan Sistem **Yöneticisi** profiline sahip bir Salesforce hesap adı yazın.

    b. Yönetici **Parolası** metin kutusuna, bu hesabın parolasını yazın.

6. Salesforce güvenlik belirtecinizi almak için yeni bir sekme açın ve aynı Salesforce yönetici hesabında oturum açın. Sayfanın sağ üst köşesinde adınızı tıklatın ve ardından **Ayarlar'ı**tıklatın.

    ![Otomatik kullanıcı sağlamayı etkinleştirme](./media/salesforce-provisioning-tutorial/sf-my-settings.png "Otomatik kullanıcı sağlamayı etkinleştirme")

7. Soldaki gezinti bölmesinde, ilgili bölümü genişletmek için **Kişisel Bilgilerim'i** tıklatın ve ardından **Güvenlik Belirtecimi Sıfırla'yı**tıklatın.
  
    ![Otomatik kullanıcı sağlamayı etkinleştirme](./media/salesforce-provisioning-tutorial/sf-personal-reset.png "Otomatik kullanıcı sağlamayı etkinleştirme")

8. Güvenlik **Belirteci'ni Sıfırla** sayfasında, **Güvenlik Belirteci'ni Sıfırla** düğmesini tıklatın.

    ![Otomatik kullanıcı sağlamayı etkinleştirme](./media/salesforce-provisioning-tutorial/sf-reset-token.png "Otomatik kullanıcı sağlamayı etkinleştirme")

9. Bu yönetici hesabıyla ilişkili e-posta gelen kutusunu denetleyin. yeni güvenlik belirteci içeren Salesforce.com bir e-posta arayın.

10. Belirteci kopyalayın, Azure REKLAM pencerenize gidin ve **Gizli Belirteç** alanına yapıştırın.

11. Salesforce örneği Salesforce Government Cloud'daysa **Kiracı URL'si** girilmelidir. Aksi takdirde, isteğe bağlıdır. Örneğinizin\> yerine Salesforce örneğinizin\<adını\>değiştirerek \<"https:// örneğiniz .my.salesforce.com" biçimini kullanarak kiracı URL'sini girin.

12. Azure portalında, Azure AD'nin Salesforce uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın.

13. Bildirim **E-postası** alanına, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve aşağıdaki onay kutusunu işaretleyin.

14. **Kaydet'i tıklatın.**  

15. Eşlemeler bölümünde, **Azure Etkin Dizin Kullanıcılarını Salesforce'a Senkronize Et'i seçin.**

16. **Öznitelik Eşlemeleri** bölümünde, Azure AD'den Salesforce'a eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen özniteliklerin, güncelleştirme işlemleri için Salesforce' taki kullanıcı hesaplarıyla aynı şeyi kullandığını unutmayın. Herhangi bir değişiklik yapmak için Kaydet düğmesini seçin.

17. Salesforce için Azure AD sağlama hizmetini etkinleştirmek için, **On** Ayarlar bölümünde **KiSama Durumunu**

18. **Kaydet'i tıklatın.**

> [!NOTE]
> Kullanıcılar Salesforce uygulamasında sağlandıktan sonra, yöneticinin onlar için dile özgü ayarları yapılandırması gerekir. Dil yapılandırması hakkında daha fazla bilgi için [lütfen bu](https://help.salesforce.com/articleView?id=setting_your_language.htm&type=5) makaleye bakın.

Bu, Kullanıcılar ve Gruplar bölümünde Salesforce'a atanan kullanıcıların ve/veya grupların ilk eşitlesini başlatır. İlk eşitlemenin, hizmet çalışırken yaklaşık her 40 dakikada bir oluşan sonraki eşitlemelerden daha uzun sürdüğünü unutmayın. İlerlemeyi izlemek ve Salesforce uygulamanızda sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik günlüklerine olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="common-issues"></a>Genel sorunlar
* Salesforce'a erişim yetkisi veren sorunlar yaşıyorsanız aşağıdakileri sağlayın:
    * Kullanılan kimlik bilgileri Salesforce'a yönetici erişimine sahiptir.
    * Kullanmakta olduğunuz Salesforce sürümü Web Erişimini destekler (örn. Geliştirici, Kurumsal, Sandbox ve Salesforce'un Sınırsız sürümleri.)
    * Kullanıcı için Web API erişimi etkinleştirilir.
* Azure AD sağlama hizmeti, bir kullanıcı için dil, yerel lik ve zaman Dilimi sağlama yı destekler. Bu öznitelik varsayılan öznitelik eşlemeleri vardır, ancak varsayılan kaynak özniteliği yok. Varsayılan kaynak özniteliğini seçtiğinizden ve kaynak özniteliğinin SalesForce tarafından beklenen biçimde olduğundan emin olun. Örneğin, English (AMERIKA Birleşik Devletleri) için localeSidKey en_US. Uygun yereleSidKey biçimini belirlemek için [burada](https://help.salesforce.com/articleView?id=setting_your_language.htm&type=5) sağlanan kılavuzu gözden geçirin. LanguageLocaleKey formatlarına [buradan](https://help.salesforce.com/articleView?id=faq_getstart_what_languages_does.htm&type=5)bakabilirsiniz. Biçimin doğru olmasını sağlamanın yanı sıra, [burada](https://help.salesforce.com/articleView?id=setting_your_language.htm&type=5)açıklandığı gibi dilin kullanıcılarınız için etkin olduğundan emin olmanız gerekebilir. 
* **SalesforceLicenseLimitExceeded:** Bu kullanıcı için kullanılabilir lisans olmadığından, hedef uygulamada kullanıcı oluşturulamadı. Hedef uygulama için ek lisanslar satın almak veya doğru kullanıcıların doğru özniteliklerle atandığından emin olmak için kullanıcı atamalarınızı ve öznitelik eşleme yapılandırmanızı gözden geçirin.
* **SalesforceDuplicateUserName:** Başka bir Salesforce.com kiracıda çoğaltılan bir 'Kullanıcı Adı' Salesforce.com sahip olduğu için kullanıcı tarafından sağlanamaz.Salesforce.com olarak, 'Kullanıcı Adı' özniteliği için değerler tüm Salesforce.com kiracı arasında benzersiz olmalıdır.Varsayılan olarak, bir kullanıcının Azure Etkin Dizin'deki kullanıcıPrincipalName'i Salesforce.com'da 'Kullanıcı Adı' olur.İki seçenek sunulur.Seçeneklerden biri, diğer kiracıyı da yönetiyorsanız, kullanıcıyı diğer Salesforce.com kiracıda yinelenen 'Kullanıcı adı' ile bulmak ve yeniden adlandırmaktır.Diğer seçenek, Azure Etkin Dizin ideninin erişimini, dizininizin tümleşik olduğu Salesforce.com kiracıya kaldırmaktır. Bu işlemi bir sonraki eşitleme denemesinde yeniden deneyeceğiz. 
* **SalesforceRequiredFieldEksik:** Salesforce, kullanıcıyı başarıyla oluşturmak veya güncelleştirmek için kullanıcıda belirli özniteliklerin bulunmasını gerektirir. Bu kullanıcı gerekli özniteliklerden biri eksik. E-posta ve diğer ad gibi özniteliklerin Salesforce'a sağlanmasını istediğiniz tüm kullanıcılarda olduğundan emin olun. Öznitelik [tabanlı kapsam filtreleri](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)kullanarak bu özniteliklere sahip olmayan kullanıcıları kapsam dışına çıkarabilirsiniz. 
* Salesforce'a sağlama için varsayılan öznitelik eşleme, Azure AD'deki AppRoleAssignments ile Salesforce'ta ProfileName'yi eşlemek için SingleAppRoleAssignments ifadesini içerir. Öznitelik eşleme yalnızca bir rolü sağlamayı desteklediğinden, kullanıcıların Azure AD'de birden çok uygulama rolü ataması olmadığından emin olun. 
* Salesforce, e-posta güncelleştirmeleri değiştirilmeden önce el ile onaylanmasını gerektirir. Sonuç olarak, kullanıcının e-postasını güncelleştirmek için sağlama günlüklerinde birden çok giriş görebilirsiniz (e-posta değişikliği onaylanana kadar).


## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](tutorial-list.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
* [Tek Oturum Açma'yı Yapılandır](https://docs.microsoft.com/azure/active-directory/active-directory-saas-salesforce-tutorial)
