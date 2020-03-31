---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için MindTickle'ı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi otomatik olarak sağlanan ve kullanıcı hesaplarını MindTickle'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: fb48deae-4653-448a-ba2f-90258edab3a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2019
ms.author: Zhchia
ms.openlocfilehash: 6ae7b6ac9c6feb539e4da3627ebbabd3c5b628fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77061248"
---
# <a name="tutorial-configure-mindtickle-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için MindTickle'ı yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları MindTickle'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için MindTickle ve Azure Active Directory (Azure AD) içinde gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir MindTickle kiracı](https://www.mindtickle.com/)
* Yönetici izinleri ile MindTickle bir kullanıcı hesabı.

## <a name="assigning-users-to-mindtickle"></a>Kullanıcıları MindTickle'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların MindTickle'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek MindTickle'a atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-mindtickle"></a>MindTickle kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için MindTickle'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı MindTickle'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-mindtickle-for-provisioning"></a>Kurulum MindTickle sağlama için

Azure AD ile otomatik kullanıcı sağlama için MindTickle'ı yapılandırmadan önce MindTickle'da SCIM sağlamayı etkinleştirmeniz gerekir.


1.  SCIM sağlama yapılandırmak için gereken JWT belirteci elde etmek için [MindTickle destek ekibine](mailto:help@mindtickle.com) ulaşın.


## <a name="add-mindtickle-from-the-gallery"></a>Galeriden MindTickle ekle

MindTickle'ı Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, MindTickle'ı Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Azure AD uygulama galerisinden MindTickle eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **MindTickle'ı**girin , sonuç panelinde **MindTickle'ı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![MindTickle sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-mindtickle"></a>MindTickle için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak MindTickle'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!TIP]
> Ayrıca [MindTickle tek oturum](mindtickle-tutorial.md)açma öğreticisinde verilen talimatları izleyerek MindTickle için SAML tabanlı tek oturum açmayı etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini birbiriyle dışa

### <a name="to-configure-automatic-user-provisioning-for-mindtickle-in-azure-ad"></a>Azure AD'de MindTickle için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **MindTickle'ı**seçin.

    ![Uygulamalar listesinde MindTickle bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://admin.mindtickle.com/scim` altında, **Kiracı URL'ye**giriş . Gizli Belirteç textbox'ta daha önce alınan **JWT belirteci** değerini girin, MindTickle destek ekibi tarafından verilen **JWT belirteç** değerini girin. Azure AD'nin politikalarıma bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, MindTickle hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını MindTickle'a Senkronize Et'i**seçin.

    ![MindTickle Kullanıcı Eşlemeleri](media/mindtickle-provisioning-tutorial/usermapping.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den MindTickle'a senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleşme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemi için MindTickle' daki kullanıcı hesaplarıyla çalışmak için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![MindTickle Kullanıcı Eşlemeleri](media/mindtickle-provisioning-tutorial/userattribute.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. MindTickle için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek MindTickle'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. İlk eşitlemenin gerçekleştirilimi sonraki eşitlemelerden daha uzun sürer. Kullanıcıların ve/veya grupların sağlanmasının ne kadar süreceğü hakkında daha fazla bilgi için, [kullanıcıların sağlanmasının ne kadar süreceğini](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users)görün. 

MindTickle'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemedurumunu izlemek ve sağlama etkinlik raporunuza gelen bağlantıları izlemek için **Geçerli Durum** bölümünü kullanabilirsiniz. Daha fazla bilgi için [bkz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) Azure AD sağlama günlüklerini okumak için [otomatik kullanıcı hesabı sağlama hakkında raporlama'ya](../app-provisioning/check-status-user-account-provisioning.md)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)
