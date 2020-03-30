---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Fuze'u yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Fuze'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 34718201-4f0e-4260-9af0-b3b70a1e8265
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: a58402297380116f83214e52ae7f2796412755b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057921"
---
# <a name="tutorial-configure-fuze-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Fuze'u yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Fuze'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Fuze ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir Fuze kiracı](https://www.fuze.com/).
* Yönetici izinleri ile Fuze bir kullanıcı hesabı.

## <a name="assigning-users-to-fuze"></a>Kullanıcıları Fuze'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Fuze'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Fuze'a atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-fuze"></a>Fuze kullanıcıları atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Fuze'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Fuze'a bir kullanıcı atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-fuze-for-provisioning"></a>Sağlama için Kurulum Fütüb

Azure AD ile otomatik kullanıcı sağlama için Fuze'u yapılandırmadan önce, Fünye'de SCIM sağlamayı etkinleştirmeniz gerekir. 

1. Gerekli bilgiler için Fuze temsilcinizle irtibata geçerek başlayın:

    * Şirketinizde şu anda kullanılmakta olan Fuze Product SUS listesi.
    * Şirketinizin konumlarına ait konum kodlarının listesi.
    * Şirketinizin departman kodları listesi.

2. Bu SK'leri ve kodları Fuze sözleşme ve yapılandırma belgelerinizde veya Fuze temsilcinizle irtibata geçerek bulabilirsiniz.

3. Gereksinimler alındıktan sonra, Fuze temsilciniz size tümleştirmeyi etkinleştirmek için gereken Fuze kimlik doğrulama belirteci sağlayacaktır. Bu değer, Azure portalındaki Fuze uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

## <a name="add-fuze-from-the-gallery"></a>Galeriden Fuze ekle

Azure AD ile otomatik kullanıcı sağlama için Fuze'u yapılandırmadan önce, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize Fuze eklemeniz gerekir.

**Azure AD uygulama galerisinden Fuze eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Fuze'u**girin, sonuç panelinde **Fuze'u** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sonuç listesinde fuze](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-fuze"></a>Otomatik kullanıcı sağlamayı Fuze'a yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Fuze'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca Fuze için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz, [Fuze Tek oturum](fuze-tutorial.md)açma öğreticisinde verilen talimatları izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-fuze-in-azure-ad"></a>Azure AD'de Fuze için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Fuze'u**seçin.

    ![Uygulamalar listesindeki Fünye bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://api.fuze.com/scim/v2` altında, **Kiracı URL'ye**giriş . Gizli Belirteç'teki Fuze temsilcisinden daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini **girdi.** Azure AD'nin Fuze'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Fuze hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL Belirteci](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Fuze için Azure Etkin Dizin Kullanıcılarını Senkronize**Et'i seçin.

    ![Fuze Kullanıcı Eşlemeleri](media/fuze-provisioning-tutorial/image01.png)

9. Azure AD'den **Öznitelik Eşleme** bölümünde Fuze'ye eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Fuze'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Fuze Kullanıcı Eşlemeleri](media/fuze-provisioning-tutorial/image00.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Fuze için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Fuze'ye sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Bilgileri **Senkronize** Etme bölümünü kullanarak ilerlemeyi izleyebilir ve Azure AD sağlama hizmeti tarafından Fuze üzerinde gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna ilişkin bağlantıları izleyebilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* Fünye, Yetkilendirme ler olarak adlandırılan özel SCIM **özniteliklerini**destekler. Bu öznitelikler yalnızca oluşturulabilir ve güncelleştirilemez. 

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamayı yönetme.](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporlar alacağınızı öğrenin.](../app-provisioning/check-status-user-account-provisioning.md)