---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Dialpad'i yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Dialpad'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılayarıştırmayı öğrenin.
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
ms.date: 06/28/2019
ms.author: zhchia
ms.openlocfilehash: 9f39277644547a625d87a39681f0c5520996cbd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058417"
---
# <a name="tutorial-configure-dialpad-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Dialpad'i yapılandır

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Dialpad'e otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Dialpad ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
>  Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.

> Bu bağlayıcı şu anda Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Bir Dialpad kiracı](https://www.dialpad.com/pricing/).
* Yönetici izinli Dialpad'deki bir kullanıcı hesabı.

## <a name="assign-users-to-dialpad"></a>Kullanıcıları Dialpad'e Atama
Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Dialpad'e erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları burada talimatları izleyerek Dialpad'e atayabilirsiniz:
 
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md) 

 ## <a name="important-tips-for-assigning-users-to-dialpad"></a>Kullanıcıları Dialpad'e atamak için önemli ipuçları

 * Otomatik kullanıcı sağlama yapılandırmasını sınamak için Tek bir Azure AD kullanıcısının Dialpad'e atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Dialpad'e atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-dialpad-for-provisioning"></a>Sağlama için Kurulum Dialpad

Azure AD ile otomatik kullanıcı sağlama için Dialpad'i yapılandırmadan önce, Dialpad'den bazı sağlama bilgileri almanız gerekir.

1. [Dialpad Yönetici Konsolunuzda](https://dialpadbeta.com/login) oturum açın ve **Yönetici ayarlarını**seçin. Açılan şirketten **Şirketim'in** seçildiğinden emin olun. **ApI Tuşları > Kimlik Doğrulama'ya**gidin.

    ![Dialpad SCIM ekle](media/dialpad-provisioning-tutorial/dialpad01.png)

2. **Anahtar ekle'yi** tıklatarak ve gizli belirteçlerinizin özelliklerini yapılandırarak yeni bir anahtar oluşturun.

    ![Dialpad SCIM ekle](media/dialpad-provisioning-tutorial/dialpad02.png)

    ![Dialpad SCIM ekle](media/dialpad-provisioning-tutorial/dialpad03.png)

3. Yeni oluşturduğunuz API anahtarıiçin **değer düğmesini göstermek** ve gösterilen değeri kopyalamak için Tıklat'ı tıklatın' ı tıklatın. Bu değer, Azure portalındaki Dialpad uygulamanızın Sağlama sekmesindeki **Gizli Belirteç** alanına girilir. 

    ![Dialpad Oluştur Jetonu](media/dialpad-provisioning-tutorial/dialpad04.png)

## <a name="add-dialpad-from-the-gallery"></a>Galeriden Dialpad ekle

Azure AD ile otomatik kullanıcı sağlama için Dialpad'i yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Dialpad eklemeniz gerekir.

**Azure AD uygulama galerisinden Dialpad eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Dialpad**girin , sonuç panelinde **Dialpad** seçin.
    ![Sonuç listesindeki Dialpad](common/search-new-app.png)

5. Aşağıda vurgulanan **URL'ye** ayrı bir tarayıcıda gidin. 

    ![Dialpad SCIM ekle](media/dialpad-provisioning-tutorial/dialpad05.png)

6. Sağ üst **köşede, Çevrimiçi > Kullan Dialpad'i kullan'ı**seçin.

    ![Dialpad SCIM ekle](media/dialpad-provisioning-tutorial/dialpad06.png)

7. Dialpad bir OpenIDConnect uygulaması olduğundan, Microsoft iş hesabınızı kullanarak Dialpad'e giriş yapmayı seçin.

    ![Dialpad SCIM ekle](media/dialpad-provisioning-tutorial/loginpage.png)

8. Başarılı bir kimlik doğrulamadan sonra, onay sayfası için onay istemini kabul edin. Uygulama daha sonra otomatik olarak kiracınıza eklenir ve Dialpad hesabınıza yönlendirilirsiniz.

    ![Dialpad SCIM ekle](media/dialpad-provisioning-tutorial/redirect.png)

 ## <a name="configure-automatic-user-provisioning-to-dialpad"></a>Otomatik kullanıcı sağlamayı Dialpad olarak yapılandırma

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Dialpad'deki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-dialpad-in-azure-ad"></a>Azure AD'de Dialpad için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Dialpad'i**seçin.

    ![Uygulamalar listesindeki Dialpad bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://dialpad.com/scim` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**Dialpad'den daha önce aldığınız ve kaydettiğiniz değeri giriş. Azure AD'nin Dialpad'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Dialpad hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Dialpad'e Senkronize Et'i**seçin.

    ![Dialpad Kullanıcı Haritalamaları](media/dialpad-provisioning-tutorial/dialpad-user-mappings-new.png)

9. **Öznitelik Eşleme** bölümünde Azure AD'den Dialpad'e eşitlenen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Dialpad'deki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Dialpad Kullanıcı Özellikleri](media/dialpad-provisioning-tutorial/dialpad07.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Dialpad için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Dialpad'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. İşlemleri izlemek ve Dialpad'deki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama etkinlik raporuna bağlı bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama](../app-provisioning/check-status-user-account-provisioning.md)
##  <a name="connector-limitations"></a>Konektör sınırlamaları
* Dialpad bugün grup yeniden adlarını desteklemez. Bu, Azure AD'deki bir grubun **displayName'sinde** yapılan değişikliklerin güncelleştirilmeyeceğini ve Dialpad'e yansıtılmayacağı anlamına gelir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
