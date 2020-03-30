---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Elçi'yi yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Elçi'ye otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılayarıştırmayı öğrenin.
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
ms.date: 06/3/2019
ms.author: jeedes
ms.openlocfilehash: 30faae80f1af4ff63924a76b26a03b8fe354a7df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058034"
---
# <a name="tutorial-configure-envoy-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Elçi'yi yapılandır

Bu öğreticinin amacı, Azure AD'yi Elçi'ye kullanıcıları ve/veya grupları otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Elçi ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Elçi kiracı](https://envoy.com/pricing/)
* Yönetici izinleri olan Elçi'deki bir kullanıcı hesabı.

## <a name="add-envoy-from-the-gallery"></a>Galeriden Elçi Ekle

Elçisi'ni Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Elçi'yi eklemeniz gerekir.

**Azure AD uygulama galerisinden Elçi eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Elçi**girin , sonuç panelinde **Elçi'yi** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesindeki elçi](common/search-new-app.png)

## <a name="assigning-users-to-envoy"></a>Kullanıcıları Elçiye Atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Elçi'ye erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Elçi'ye atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-envoy"></a>Elçi'ye kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için Elçi'ye tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Elçi'ye bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rol (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-envoy"></a>Elçi'ye otomatik kullanıcı sağlama nın yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Elçi'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size rehberlik eder.

> [!TIP]
> Ayrıca, Elçi tek oturum açma [öğreticisinde](envoy-tutorial.md)verilen talimatları izleyerek, Elçi için SAML tabanlı tek oturum açma yı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-envoy-in-azure-ad"></a>Azure AD'deki Elçi için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Elçi'yi**seçin.

    ![Başvurular listesindeki Elçi bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://app.envoy.com/scim/v2` altında, **Kiracı URL'ye**giriş . Elçi hesabınızın **Gizli Belirteci'ni** almak için, Adım 6'da açıklandığı gibi gözden geçirme yi izleyin.

6. Elçi Yönetici [Konsolunuzda](https://dashboard.envoy.com/login)oturum açın. **Entegrasyonlar'a**tıklayın.

    ![Elçi Entegrasyonları](media/envoy-provisioning-tutorial/envoy01.png)

    **Microsoft Azure SCIM tümleştirmesi**için **Yükle'yi** tıklatın.

    ![Elçi Yükleme](media/envoy-provisioning-tutorial/envoy02.png)

    Tüm kullanıcıları **Eşitle**için **Kaydet'e** tıklayın. 

    ![Elçi Kaydet](media/envoy-provisioning-tutorial/envoy03.png)

    Doldurulan Gizli Belirteç'i alın.
    
    ![Elçi OAUTH](media/envoy-provisioning-tutorial/envoy04.png)

7. Adım 5'te gösterilen alanları doldurarak Azure AD'nin Elçi'ye bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Elçi hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Elçiye Senkronize Et'i**seçin.
    
    ![Elçi Kullanıcı Özellikleri](media/envoy-provisioning-tutorial/envoy-user-mappings.png)
    
11. Azure AD'den **Atnitelik Eşleme** bölümünde Elçi'ye eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Elçi'deki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Elçi Kullanıcı Özellikleri](media/envoy-provisioning-tutorial/envoy-user-attribute.png)

12. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Elçiye Senkronize**Et'i seçin.

    ![Elçi Kullanıcı Haritalamaları](media/envoy-provisioning-tutorial/envoy-group-mapping.png)

13. Azure AD'den **Atnitelik Eşleme** bölümünde Elçi'ye eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için Elçi'deki gruplarla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Elçi Kullanıcı Haritalamaları](media/envoy-provisioning-tutorial/envoy-group-attributes.png)
    
14. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

15. Elçi için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

16. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek Elçi'ye sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

17. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İlerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından Elçi'de gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

