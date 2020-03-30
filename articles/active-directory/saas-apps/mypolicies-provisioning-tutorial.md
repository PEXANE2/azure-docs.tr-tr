---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Politikalarımı Yapılandırın | Microsoft Dokümanlar'
description: Azure Active Directory'yi, kullanıcı hesaplarını myPolicies'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f000896d-a78c-4d20-a79c-74c1f9b4961a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 353da826b6e339d40a5d85bbf63caac5bf7094f1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061378"
---
# <a name="tutorial-configure-mypolicies-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için myPolicies'i yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları myPolicies'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Politikalarım ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [MyPolicies kiracı.](https://mypolicies.com/index.html#section10)
* Yönetici izinleri olan politikalarımdaki bir kullanıcı hesabı.

## <a name="assigning-users-to-mypolicies"></a>Kullanıcıları Politikalarıma Atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların myPolicies'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki yönergeleri izleyerek myPolicies'e atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-mypolicies"></a>Kullanıcıları myPolicies'e atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için myPolicies'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Politikalarım'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-mypolicies-for-provisioning"></a>Sağlama için myPolicies'i kurulum

Azure AD ile otomatik kullanıcı sağlama için Politikalarımı yapılandırmadan önce, Politikalarım'da SCIM sağlamayı etkinleştirmeniz gerekir.

1. SCIM sağlama yapılandırmak **support@mypolicies.com** için gereken gizli belirteci elde etmek için myPolicies temsilcinize ulaşın.

2.  myPolicies temsilcisi tarafından sağlanan belirteç değerini kaydedin. Bu değer, Azure portalındaki myPolicies uygulamanızın Sağlama sekmesinde **Gizli Belirteç** alanına girilir.

## <a name="add-mypolicies-from-the-gallery"></a>Galeriden myPolicies ekleme

Azure AD ile otomatik kullanıcı sağlama için Politikalarımı yapılandırmak için, myPolicies'i Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden Politikalarımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **benimİlkeler'leri**girin, sonuç panelinde **politikalarım'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![sonuç listesindeki myPolicies](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-mypolicies"></a>Otomatik kullanıcı sağlamayı myPolicies'e yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Politikalarımdaki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> MyPolicies Tek oturum açma [öğreticisinde](mypolicies-tutorial.md)verilen yönergeleri izleyerek, myPolicies için SAML tabanlı tek oturum açma'yı da etkinleştirmeyi seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-mypolicies-in-azure-ad"></a>Azure AD'deki Politikalarım için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, myPolicies'i**seçin.

    ![Uygulamalar listesindeki myPolicies bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümünde, `https://<myPoliciesCustomDomain>.mypolicies.com/scim` MyPolicies `<myPoliciesCustomDomain>` özel etki alanınız kiracı **URL'ye** giriş. myPolicies müşteri etki alanınızı URL'nizden alabilirsiniz.
Örnek: `<demo0-qa>`.mypolicies.com.

6. **Gizli Belirteç'te,** daha önce alınan belirteç değerini girin. Azure AD'nin politikalarıma bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, myPolicies hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

7. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

8. **Kaydet**'e tıklayın.

9. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Politikalarımla Senkronize Et'i**seçin.

    ![myPolicies Kullanıcı Eşlemeleri](media/mypolicies-provisioning-tutorial/usermapping.png)

10. Azure AD'den MyPolicies'e eşitlenen kullanıcı özniteliklerini **Atnitelik Eşleme** bölümünde gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, sınır düşünme işlemleri için Kullanım PolitikalarımLarındaki kullanıcı hesaplarıyla çalğ Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![myPolicies Kullanıcı Eşlemeleri](media/mypolicies-provisioning-tutorial/userattribute.png)

11. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

12. İlkelerim için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Politikalarımı sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Verileri izlemek ve Azure AD sağlama hizmeti tarafından politikalarım da gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* myPolicies her zaman **userName,** **e-posta** ve **externalId**gerektirir.
* myPolicies, kullanıcı öznitelikleri için sabit silmeleri desteklemez.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
