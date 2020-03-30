---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Federe Dizini yapılandırın | Microsoft Dokümanlar'
description: Azure Active Directory'yi, kullanıcı hesaplarını Federe Dizin'e otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
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
ms.date: 07/12/2019
ms.author: zhchia
ms.openlocfilehash: 910aaac84dacb75cd76772a0bc2960d9bfa8bb70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057954"
---
# <a name="tutorial-configure-federated-directory-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Federe Dizini yapılandırın

Bu öğreticinin amacı, Azure AD'yi federe dizin ve/veya grupları federe dizin için otomatik olarak sağlama ve sağlamadan çıkarmak üzere yapılandırmak için Federe Dizin ve Azure Etkin Dizin 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
>  Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracısı.
* [Federe Dizin](https://www.federated.directory/pricing).
* Yönetici izinli Federe Dizini'nde bir kullanıcı hesabı.

## <a name="assign-users-to-federated-directory"></a>Kullanıcıları Federe Dizine Atama
Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Federe Dizine erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki talimatları izleyerek Federe Dizine atayabilirsiniz:

 * [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md) 
 
 ## <a name="important-tips-for-assigning-users-to-federated-directory"></a>Kullanıcıları Federe Dizine atamak için önemli ipuçları
 * Otomatik kullanıcı sağlama yapılandırmasını sınamak için tek bir Azure AD kullanıcısının Federe Dizine atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Federe Dizine atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur.
    
 ## <a name="set-up-federated-directory-for-provisioning"></a>Tedarik için Federe Dizini ayarlama

Azure AD ile otomatik kullanıcı sağlama için Federe Dizini yapılandırmadan önce, Federe Dizin üzerinde SCIM sağlamayı etkinleştirmeniz gerekir.

1. [Federe Dizin Yönetici Konsolunuzda](https://federated.directory/of) Oturum Açın

    ![Federe Dizin eğitimi](media/federated-directory-provisioning-tutorial/companyname.png)

2. Kullanıcı **dizinleri > Dizinlere** gidin ve kiracınızı seçin. 

    ![federe dizini](media/federated-directory-provisioning-tutorial/ad-user-directories.png)

3.  Kalıcı bir taşıyıcı belirteci oluşturmak **için, Yeni Anahtar Oluşturma > Dizin Tuşlarına gidin.** 

    ![federe dizini](media/federated-directory-provisioning-tutorial/federated01.png)

4. Bir dizin anahtarı oluşturun. 

    ![federe dizini](media/federated-directory-provisioning-tutorial/federated02.png)
    

5. Access **Token** değerini kopyalayın. Bu değer, Azure portalındaki Federe Dizin uygulamanızın Sağlama sekmesindeki **Gizli Belirteç** alanına girilir. 

    ![federe dizini](media/federated-directory-provisioning-tutorial/federated03.png)
    
## <a name="add-federated-directory-from-the-gallery"></a>Galeriden Federe Dizini ekle

Azure AD ile otomatik kullanıcı sağlama için Federe Dizini yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Federe Dizin eklemeniz gerekir.

**Azure AD uygulama galerisinden Federe Dizini eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Federe Dizini**girin , sonuç panelinde **Federe Dizini** seçin.

    ![Sonuç listesinde federe dizin](common/search-new-app.png)

5. Aşağıda vurgulanan **URL'ye** ayrı bir tarayıcıda gidin. 

    ![federe dizini](media/federated-directory-provisioning-tutorial/loginpage1.png)

6. **Gİrİş YAP'A**tıklayın.

    ![federe dizini](media/federated-directory-provisioning-tutorial/federated04.png)

7.  Federe Dizin bir OpenIDConnect uygulaması olduğundan, Microsoft iş hesabınızı kullanarak Federe Dizine giriş yapmayı seçin.
    
    ![federe dizini](media/federated-directory-provisioning-tutorial/loginpage3.png)
 
8. Başarılı bir kimlik doğrulamadan sonra, onay sayfası için onay istemini kabul edin. Uygulama daha sonra otomatik olarak kiracınıza eklenir ve Federe Dizin hesabınıza yönlendirilirsiniz.

    ![federe dizin SCIM ekle](media/federated-directory-provisioning-tutorial/premission.png)



## <a name="configuring-automatic-user-provisioning-to-federated-directory"></a>Otomatik kullanıcı sağlamanın Federated Directory'ye yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Federe Dizini'ndeki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size rehberlik eder.

### <a name="to-configure-automatic-user-provisioning-for-federated-directory-in-azure-ad"></a>Azure AD'de Federe Dizin için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Federe Dizini**seçin.

    ![Uygulamalar listesindeki Federe Dizin bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://api.federated.directory/v2/` altında, Kiracı URL'sindeki girişi. **Gizli Belirteç'te**daha önce aldığınız ve kaydettiğiniz değeri Federe Dizini'nden girin. Azure AD'nin Federe Dizine bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Federe Dizin hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

8. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

9. **Kaydet**'e tıklayın.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Federe Dizine Senkronize**Etme'yi seçin.

    ![Federe Dizin eğitimi](media/federated-directory-provisioning-tutorial/user-mappings.png)
    
    
11. Azure AD'den Federated Directory'ye eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Federe Dizini'ndeki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Federe Dizin eğitimi](media/federated-directory-provisioning-tutorial/user-attributes.png)
    

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Federe Dizin için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **On** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Federe Dizine vermek istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Eşitleme **Ayrıntıları** bölümünü, ilerlemeyi izlemek ve Azure AD sağlama hizmeti tarafından Federe Dizini'nde gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için bkz: [Otomatik kullanıcı hesabı sağlama hakkında Raporlama](../app-provisioning/check-status-user-account-provisioning.md)
## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
