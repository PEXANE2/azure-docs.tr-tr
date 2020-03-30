---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Comeet Recruiting YazılımLarını yapılandırın | Microsoft Dokümanlar'
description: Azure Active Directory'yi, kullanıcı hesaplarını Comeet Recruiting Software'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.date: 05/07/2019
ms.author: jeedes
ms.openlocfilehash: f427fb75cfaeda79b037c327992e4ad482a7e689
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058348"
---
# <a name="tutorial-configure-comeet-recruiting-software-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Comeet Recruiting Software yapılandırma

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Comeet Recruiting Software'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Comeet Recruiting Software ve Azure Active Directory (Azure AD) için gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Comeet İşe Yazılım kiracı](https://www.comeet.co/)
* Yönetici izinleri ile Comeet Recruiting Software bir kullanıcı hesabı.

## <a name="add-comeet-recruiting-software-from-the-gallery"></a>Galeriden Comeet Recruiting Yazılım ekle

Azure AD ile otomatik kullanıcı sağlama için Comeet İşe Alma Yazılımı yapılandırmadan önce, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Comeet İşe Alma Yazılımı eklemeniz gerekir.

**Azure AD uygulama galerisinden Comeet Recruiting Yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Comeet Recruiting Software**girin , sonuç panelinde **Comeet Recruiting Software** seçin ve sonra uygulama eklemek için **Ekle** düğmesini tıklatın.

    ![Comeet İşe Yazılım sonuç listesinde](common/search-new-app.png)

## <a name="assigning-users-to-comeet-recruiting-software"></a>Kullanıcıları Comeet Recruiting Software'e atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Comeet Recruiting Software'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları burada talimatları izleyerek Comeet Recruiting Software'e atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-comeet-recruiting-software"></a>Comeet Recruiting Software kullanıcıları atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için tek bir Azure AD kullanıcısının Comeet Recruiting Software'e atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Comeet Recruiting Software'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="configuring-automatic-user-provisioning-to-comeet-recruiting-software"></a>Otomatik kullanıcı sağlamanın Comeet Recruiting Software'e yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı Olarak Comeet İşe Alma Yazılımındaki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size rehberlik eder.

> [!TIP]
> Ayrıca Comeet Recruiting Software için SAML tabanlı tek oturum açma yı etkinleştirmeyi de seçebilirsiniz, [Comeet Recruiting Software tek oturum](comeetrecruitingsoftware-tutorial.md)açma öğreticisinde verilen talimatları izleyerek. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini tamamlar.

### <a name="to-configure-automatic-user-provisioning-for-comeet-recruiting-software-in-azure-ad"></a>Azure AD'de Comeet Recruiting Software için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Comeet Recruiting Software'i**seçin.

    ![Uygulamalar listesinde Comeet İşe Yazılım bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü altında, Basamak 6'da açıklandığı şekilde Comeet Recruiting Software hesabınızın **Kiracı URL'sini** ve **Gizli Belirteci'ni** girin.

6. [Comeet Recruiting Software yönetici konsolunda,](https://app.comeet.co/) **Microsoft Azure'> > Orijinallik > Comeet > Ayarları'na**gidin ve Şirket **değeriniz için Gizli Belirteci'yi** Azure AD'deki **Gizli Belirteç** alanına kopyalayın.

    ![Comeet İşe Yazılım Sağlama](./media/comeet-recruiting-software-provisioning-tutorial/secret-token-1.png)

7. Adım 5'te gösterilen alanları doldurarak, Azure AD'nin Comeet Recruiting Software'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Comeet Recruiting Software hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Belirteç](common/provisioning-testconnection-token.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Comeet'e Senkronize Et'i**seçin.

    ![Comeet İşe Yazılım Kullanıcı Haritalama](media/comeet-recruiting-software-provisioning-tutorial/user-mappings.png)

11. Azure AD'den **Atrit Eşleme** bölümünde Comeet Recruiting Software'e senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Comeet Recruiting Software'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Comeet İşe Yazılım Grubu Özellikleri](media/comeet-recruiting-software-provisioning-tutorial/user-mapping-attributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Comeet Recruiting Software için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde Sağlama Durumunu **Ayarı** olarak değiştirin. **Settings**

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek Comeet Recruiting Software'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Kaydedilen ilerlemeyi izlemek ve Comeet Recruiting Software'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Bağlayıcı Sınırlamaları

* Comeet Recruiting Software şu anda grupları desteklememektedir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

