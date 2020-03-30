---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için G Suite'i yapılandırın | Microsoft Dokümanlar"
description: Azure AD'den G Suite'e kullanıcı hesaplarını otomatik olarak nasıl sağlayıp geçici olarak sağdan çıkarabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/06/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 969a2fb5444ae8ece2aa302c04a5bbb85dcca917
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057745"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için G Suite'i yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları G Suite'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için G Paketi ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

> [!NOTE]
> G Suite konektörü ekim 2019'da güncellenmiştir. G Suite konektöründe yapılan değişiklikler şunlardır:
> - Ek G Suite kullanıcı ve grup öznitelikleri için destek eklendi. 
> - Burada [tanımlananla](https://developers.google.com/admin-sdk/directory)eşleşecek şekilde Güncelleştirilmiş G Suite hedef öznitelik adları.
> - Güncelleştirilmiş varsayılan öznitelik eşlemeleri.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini G Suite ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD kiracı
- [Bir G Suite kiracı](https://gsuite.google.com/pricing.html)
- Yönetici izinli bir G Suite'teki kullanıcı hesabı.

## <a name="assign-users-to-g-suite"></a>Kullanıcıları G Suite'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların G Suite'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek G Suite'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-g-suite"></a>Kullanıcıları G Suite'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Tek bir Azure AD kullanıcısının G Suite'e atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı G Suite'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-g-suite-for-provisioning"></a>Sağlama için Kurulum G Suite

Azure AD ile otomatik kullanıcı sağlama için G Suite'i yapılandırmadan önce, G Suite'te SCIM sağlamayı etkinleştirmeniz gerekir.

1. Yönetici hesabınızla [G Suite Admin konsolunda](https://admin.google.com/) oturum açın ve ardından **Güvenlik'i**seçin. Bağlantıyı görmüyorsanız, ekranın altındaki **Daha Fazla Denetimler** menüsünün altında gizlenmiş olabilir.

    ![Güvenliği seçin.][10]

1. **Güvenlik** **sayfasında, API Başvurusu'nu**seçin.

    ![API Başvurusu'nü seçin.][15]

1. **API erişimini etkinleştir'i**seçin.

    ![API Başvurusu'nü seçin.][16]

   > [!IMPORTANT]
   > G Suite'e sağlamayı planladığınız her kullanıcı için, Azure AD'deki kullanıcı adı özel bir etki alanına bağlı **olmalıdır.** Örneğin, benzer bob@contoso.onmicrosoft.com kullanıcı adları G Suite tarafından kabul edilmez. Diğer taraftan, bob@contoso.com kabul edilir. [Burada](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)yönergeleri izleyerek varolan bir kullanıcının etki alanını değiştirebilirsiniz.

1. İstediğiniz özel etki alanlarını Azure AD ile ekledikten ve doğruladıktan sonra, bunları G Suite ile yeniden doğrulamanız gerekir. G Suite'teki etki alanlarını doğrulamak için aşağıdaki adımlara bakın:

    a. G [Suite Yönetici Konsolunda](https://admin.google.com/) **Etki Alanlarını**seçin.

    ![Etki Alanlarını Seçin][20]

    b. **Etki alanı veya etki alanı takma adı ekle'yi**seçin.

    ![Yeni bir etki alanı ekleme][21]

    c. **Başka bir etki alanı ekle'yi**seçin ve sonra eklemek istediğiniz etki alanının adını yazın.

    ![Etki alanı adınızı yazın][22]

    d. **Devam et'i seçin ve etki alanı sahipliğini doğrulayın.** Ardından, etki alanı adının sahibi olduğunuzu doğrulamak için adımları izleyin. Etki alanınızı Google ile nasıl doğrulayabilirsiniz hakkında kapsamlı talimatlar [için](https://support.google.com/webmasters/answer/35179)bkz.

    e. G Suite'e eklemek istediğiniz ek etki alanları için önceki adımları yineleyin.

1. Ardından, G Suite'te kullanıcı sağlamayı yönetmek için hangi yönetici hesabını kullanmak istediğinizi belirleyin. Yönetici **Rolleri'ne**gidin.

    ![Google Apps'ı Seçin][26]

1. Bu hesabın **Yönetici rolü** için, bu rol için **Ayrıcalıkları** edin. Bu hesabın sağlama için kullanılabilmesi için tüm **Yönetici API Ayrıcalıklarını** etkinleştirmeye emin olun.

    ![Google Apps'ı Seçin][27]

## <a name="add-g-suite-from-the-gallery"></a>Galeriden G Suite ekle

Azure AD ile otomatik kullanıcı sağlama için G Suite'i yapılandırmak için, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize G Suite eklemeniz gerekir. 

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

1. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

1. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

1. Arama kutusuna, **G Suite'e**girin, sonuç panelinde **G Suite'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde ki G Suite](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-g-suite"></a>Otomatik kullanıcı sağlamanın G Suite'e yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak G Suite'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size rehberlik eder.

> [!TIP]
> Ayrıca G Suite Tek oturum açma [öğreticisinde](https://docs.microsoft.com/azure/active-directory/saas-apps/google-apps-tutorial)verilen talimatları izleyerek G Suite için SAML tabanlı tek oturum açma yı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

> [!NOTE]
> G Suite'in Dizin API bitiş noktası hakkında daha fazla bilgi edinmek için [Dizin API'sine](https://developers.google.com/admin-sdk/directory)bakın.

### <a name="to-configure-automatic-user-provisioning-for-g-suite-in-azure-ad"></a>Azure AD'de G Suite için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

1. Uygulamalar listesinde **G Suite'i**seçin.

    ![Uygulamalar listesindeki G Suite bağlantısı](common/all-applications.png)

1. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

1. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

1. Yönetici **Kimlik Bilgileri** bölümünde **Yetkiver'i**seçin. Yeni bir tarayıcı penceresinde bir Google yetkilendirme iletişim kutusu açar.

    ![G Suite Yetki](media/google-apps-provisioning-tutorial/authorize.png)

1. G Suite kiracınızda değişiklik yapmak için Azure AD'ye izin vermek istediğinizi doğrulayın. **Kabul Et**’i seçin.

    ![İzinleri onaylayın.][28]

1. Azure portalında, Azure AD'nin uygulamanıza bağlanabilmesini sağlamak için **Test Bağlantısı'nı** seçin. Bağlantı başarısız olursa, G Suite hesabınızın Team Admin izinlerine sahip olduğundan emin olun. Ardından **Yetkilendirme** adımını yeniden deneyin.

1. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

1. **Kaydet**'e tıklayın.

1. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını G Paketi'ne Senkronize Et'i**seçin.

    ![G Suite Kullanıcı Haritalamaları](media/google-apps-provisioning-tutorial/usermappings.png)

1. Azure AD'den G Suite'e senkronize edilen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için G Suite'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![G Suite Kullanıcı Özellikleri](media/google-apps-provisioning-tutorial/userattributes.png)

1. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını G Paketi'ne Senkronize Et'i**seçin.

    ![G Suite Grup Haritalamaları](media/google-apps-provisioning-tutorial/groupmappings.png)

1. Azure AD'den G Suite'e eşitlenen grup özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için G Suite'teki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin. UI, Azure AD ve G Suite arasındaki varsayılan öznitelik eşleme kümesini görüntüler. Yeni eşleme ekle'yi tıklatarak Org Birimi gibi ek öznitelikler eklemeyi seçebilirsiniz.

    ![G Suite Grup Özellikleri](media/google-apps-provisioning-tutorial/groupattributes.png)

1. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

1. G Paketi için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

1. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek G Paketi'ne sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

1. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. G Suite'teki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

> [!NOTE]
> Kullanıcılar Azure AD kullanıcısının e-posta adresini kullanarak zaten varolan bir kişisel/tüketici hesabına sahipse, dizin eşitlemini gerçekleştirmeden önce Google Transfer Aracı'nı kullanarak çözülebilecek bazı sorunlara neden olabilir.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="common-issues"></a>Genel sorunlar
* Bağlantı kurmak için kullanılan hesap GSuite'teki bir yönetici ye uygun olmadığında yetkilendirme hataları oluşabilir. Erişimi yetkilendirmek için kullanılan hesabın, kullanıcıların sağlanması gereken **tüm etki alanlarında** yönetici izinlerine sahip olduğundan emin olun. 
* Azure AD, GSuite'teki kullanıcıların uygulamaya erişememesi için devre dışı bırakılmasını destekler, ancak GSuite'teki kullanıcıları silmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

<!--Image references-->

[10]: ./media/google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/google-apps-provisioning-tutorial/gapps-auth.png
