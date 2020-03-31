---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Smartsheet'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Smartsheet'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2019
ms.author: jeedes
ms.openlocfilehash: 9fbdf8a1c4b1881fc6dfd9d7b95a4103761e9ce7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063212"
---
# <a name="tutorial-configure-smartsheet-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Smartsheet'i yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Smartsheet'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Smartsheet ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Akıllı Sayfa kiracı](https://www.smartsheet.com/pricing)
* System Administrator izinleri içeren smartsheet Enterprise veya Enterprise Premier planındaki bir kullanıcı hesabı.

## <a name="assign-users-to-smartsheet"></a>Kullanıcıları Smartsheet'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Smartsheet'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Smartsheet'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-smartsheet"></a>Kullanıcıları Smartsheet'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için smartsheet'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Smartsheet'e bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

* Smartsheet ve Azure AD arasındaki kullanıcı rol atamalarında eşitlik sağlamak için, tam Smartsheet kullanıcı listesinde bulunan aynı rol atamalarının kullanılması önerilir. Bu kullanıcı listesini Smartsheet'ten almak için **Hesap Yöneticisi > Kullanıcı Yönetimi > Kullanıcı Listesini İndir> Daha Fazla Eyleme (csv)** gidin.

* Uygulamadaki belirli özelliklere erişmek için Smartsheet, bir kullanıcının birden çok rolü olmasını gerektirir. Smartsheet'te kullanıcı türleri ve izinleri hakkında daha fazla bilgi edinmek için [Kullanıcı Türleri ve İzinler'e](https://help.smartsheet.com/learning-track/shared-users/user-types-and-permissions)gidin.

*  Bir kullanıcının Smartsheet'te atanmış birden çok rolü varsa, kullanıcıların Smartsheet nesnelerine kalıcı olarak erişimini kaybedebileceği bir senaryoyu önlemek için bu rol atamalarının Azure AD'de çoğaltıldığından **emin** olmalısınız. Smartsheet'teki her benzersiz rol **Azure** AD'deki farklı bir gruba atanmalıdır. Kullanıcı **daha** sonra istenen rollere karşılık gelen grupların her birine eklenmelidir. 

## <a name="set-up-smartsheet-for-provisioning"></a>Sağlama için Smartsheet'i ayarlama

Azure AD ile otomatik kullanıcı sağlama için Smartsheet'i yapılandırmadan önce, Smartsheet'te SCIM sağlamayı etkinleştirmeniz gerekir.

1. **[Smartsheet portalında](https://app.smartsheet.com/b/home)** **SysAdmin** olarak oturum açın ve **Hesap Yöneticisi'ne**gidin.

    ![Akıllı Sayfa Hesap Yöneticisi](media/smartsheet-provisioning-tutorial/smartsheet-accountadmin.png)

2. Güvenlik **Denetimleri > Kullanıcı Otomatik Sağlama > Edit**gidin.

    ![Smartsheet Güvenlik Denetimleri](media/smartsheet-provisioning-tutorial/smartsheet-securitycontrols.png)

3. Azure AD'den Smartsheet'e sağlamayı planladığınız kullanıcılar için e-posta etki alanlarını ekleyin ve doğrulayın. Tüm sağlama eylemlerinin yalnızca Azure AD'den kaynaklandığından emin olmak ve Smartsheet kullanıcı listenizin Azure AD atamalarıyla eşit olduğundan emin olmak için **Etkin Değil'i** seçin.

    ![Smartsheet Kullanıcı Sağlama](media/smartsheet-provisioning-tutorial/smartsheet-userprovisioning.png)

4. Doğrulama tamamlandıktan sonra etki alanını etkinleştirmeniz gerekir. 

    ![Smartsheet Etki Alanını Etkinleştir](media/smartsheet-provisioning-tutorial/smartsheet-activatedomain.png)

5. **Uygulamalar ve Tümleştirmeler'de**gezinerek Azure AD ile otomatik kullanıcı sağlama yapılandırmak için gereken **Gizli Belirteci** oluşturun.

    ![Smartsheet Yükleme](media/smartsheet-provisioning-tutorial/Smartsheet05.png)

6. **API Erişimi'ni**seçin. **Yeni erişim jetonu oluştur'u**tıklatın.

    ![Smartsheet Yükleme](media/smartsheet-provisioning-tutorial/Smartsheet06.png)

7. API Access Token'in adını tanımlayın. **Tamam**'a tıklayın.

    ![Smartsheet Yükleme](media/smartsheet-provisioning-tutorial/Smartsheet07.png)

8. API Access Token'ı kopyalayın ve görüntülediğiniz tek zaman bu olacağından kaydedin. Bu, Azure AD'deki **Gizli Belirteç** alanında gereklidir.

    ![Akıllı sayfa belirteci](media/smartsheet-provisioning-tutorial/Smartsheet08.png)

## <a name="add-smartsheet-from-the-gallery"></a>Galeriden Smartsheet ekleme

Azure AD ile otomatik kullanıcı sağlama için Smartsheet'i yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Smartsheet eklemeniz gerekir.

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Smartsheet'i**girin, sonuç panelinde **Smartsheet'i** seçin. 

    ![Sonuç listesindeki smartsheet](common/search-new-app.png)

5. Sizi Smartsheet'in giriş sayfasına yönlendirecek **Smartsheet için Kaydol** düğmesini seçin. 

    ![Smartsheet OIDC Ekle](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-add.png)

6. Smartsheet bir OpenIDConnect uygulaması olduğundan, Microsoft iş hesabınızı kullanarak Smartsheet'e giriş yapmayı seçin.

    ![Smartsheet OIDC giriş](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-login.png)

7. Başarılı bir kimlik doğrulamadan sonra, onay sayfası için onay istemini kabul edin. Uygulama daha sonra otomatik olarak kiracınıza eklenir ve Smartsheet hesabınıza yönlendirilirsiniz.

    ![Smartsheet OsB Onayı](media/smartsheet-provisioning-tutorial/smartsheet-OIDC-consent.png)

## <a name="configure-automatic-user-provisioning-to-smartsheet"></a>Otomatik kullanıcı sağlamayı Smartsheet'e yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Smartsheet'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-smartsheet-in-azure-ad"></a>Azure AD'de Smartsheet için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Smartsheet'i**seçin.

    ![Uygulamalar listesindeki Akıllı Sayfa bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://scim.smartsheet.com/v2/` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**Smartsheet'ten daha önce aldığınız ve kaydettiğiniz değeri girdiniz. Azure AD'nin Smartsheet'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Smartsheet hesabınızın SysAdmin izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Akıllı Sayfaya Senkronize Et'i**seçin.

    ![Smartsheet Kullanıcı Eşlemeleri](media/smartsheet-provisioning-tutorial/smartsheet-user-mappings.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den Smartsheet'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Smartsheet'teki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Smartsheet Kullanıcı Öznitelikleri](media/smartsheet-provisioning-tutorial/smartsheet-user-attributes.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Smartsheet için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Smartsheet'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Kaydetme **Ayrıntıları** bölümünü, ilerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından Smartsheet'te gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik raporuna bağlı bağlantıları izlemek için kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Smartsheet yumuşak silmeleri desteklemez. Bir kullanıcının **etkin** özniteliği False olarak ayarlandığında, Smartsheet kullanıcıyı kalıcı olarak siler.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
