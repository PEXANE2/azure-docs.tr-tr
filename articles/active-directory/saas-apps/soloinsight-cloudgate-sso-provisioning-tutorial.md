---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Soloinsight-CloudGate SSO'ya yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi soloinsight-CloudGate SSO'ya otomatik olarak sağlama ve kullanıcı hesaplarını sağlama ve azaltma işlemlerini yapmak üzere nasıl yapılandırılayarıştırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 07558ceb-d406-40e7-90b8-1b40fdc829e7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2019
ms.author: Zhchia
ms.openlocfilehash: 6ab90a6aea262d5c7067f9f41b9ddfc090b7371d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063204"
---
# <a name="tutorial-configure-soloinsight-cloudgate-sso-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Soloinsight-CloudGate SSO'ya yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Soloinsight-CloudGate SSO'ya otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Soloinsight-CloudGate SSO ve Azure Active Directory 'de (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Soloinsight-CloudGate SSO kiracı](https://www.soloinsight.com/)
* Yönetici izinleri olan Soloinsight-CloudGate SSO'daki bir kullanıcı hesabı.

## <a name="assigning-users-to-soloinsight-cloudgate-sso"></a>Kullanıcıları Soloinsight-CloudGate SSO'ya atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Soloinsight-CloudGate SSO'ya erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları soloinsight-CloudGate SSO'ya buradaki talimatları izleyerek atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-soloinsight-cloudgate-sso"></a>Soloinsight-CloudGate SSO'ya kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını test etmek için soloinsight-CloudGate SSO'ya tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Soloinsight-CloudGate SSO'ya atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-soloinsight-cloudgate-sso-for-provisioning"></a>Sağlama için Soloinsight-CloudGate SSO'su ayarlama

1. [Soloinsight-CloudGate SSO Yönetici Konsolunuzda](https://soloinsight.sigateway.com/login)oturum açın. Yönetim **> Sistem Ayarları'na**gidin.

    ![Soloinsight-CloudGate SSO Yönetici Konsolu](media/soloinsight-cloudgate-sso-provisioning-tutorial/admin.png)

2.  **Genel'e**gidin.

    ![Soloinsight-CloudGate SSO SCIM ekle](media/soloinsight-cloudgate-sso-provisioning-tutorial/config.png)

3.  **Kiracı URL'sini** ve **Gizli Belirteci'ni**almak için sayfanın sonuna doğru ilerleyin. Gizli **Jetonu**kopyala. Bu değer, Azure portalındaki Soloinsight-CloudGate SSO uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

    ![Soloinsight-CloudGate SSO Oluştur Jeton](media/soloinsight-cloudgate-sso-provisioning-tutorial/token.png)

## <a name="add-soloinsight-cloudgate-sso-from-the-gallery"></a>Galeriden Soloinsight-CloudGate SSO ekle

Soloinsight-CloudGate SSO'yu Azure AD ile otomatik kullanıcı sağlama için yapılandırmadan önce, Azure AD uygulama galerisinden Yönetilen SaaS uygulamaları listenize Soloinsight-CloudGate SSO eklemeniz gerekir.

**Azure AD uygulama galerisinden Soloinsight-CloudGate SSO eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Soloinsight-CloudGate SSO'yu**girin, sonuç panelinde **Soloinsight-CloudGate SSO'yu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Soloinsight-CloudGate SSO sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-soloinsight-cloudgate-sso"></a>Otomatik kullanıcı sağlamanın Soloinsight-CloudGate SSO'ya yapılandırılması 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Soloinsight-CloudGate SSO'daki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size rehberlik eder.

> [!TIP]
> Soloinsight-CloudGate SSO için SAML tabanlı tek oturum açma yı, [Soloinsight-CloudGate SSO Tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial)açma öğreticisinde verilen talimatları izleyerek etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini birbiriyle dışa

### <a name="to-configure-automatic-user-provisioning-for-soloinsight-cloudgate-sso-in-azure-ad"></a>Azure AD'de Soloinsight-CloudGate SSO için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Soloinsight-CloudGate SSO'yu**seçin.

    ![Uygulamalar listesindeki Soloinsight-CloudGate SSO bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://sigateway.com/scim/v2/sync/serviceproviderconfig` altında, **Kiracı URL'ye**giriş . **Gizli Belirteç'te**daha önce alınan **SCIM Kimlik Doğrulama Belirteci** değerini girdi. Azure AD'nin Soloinsight-CloudGate SSO'ya bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Soloinsight-CloudGate SSO hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Soloinsight-CloudGate SSO ile Senkronize Et'i**seçin.

    ![Soloinsight-CloudGate SSO Kullanıcı Haritalamaları](media/soloinsight-cloudgate-sso-provisioning-tutorial/usermappings.png)

9. Azure AD'den Soloinsight-CloudGate SSO'ya senkronize edilen kullanıcı özniteliklerini **Atföz Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Soloinsight-CloudGate SSO'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Soloinsight-CloudGate SSO Kullanıcı Özellikleri](media/soloinsight-cloudgate-sso-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Soloinsight-CloudGate SSO'ya Senkronize Et'i**seçin.

    ![Soloinsight-CloudGate SSO Grup Haritalamaları](media/soloinsight-cloudgate-sso-provisioning-tutorial/groupmappings.png)

11. Azure AD'den Soloinsight-CloudGate SSO'ya senkronize edilen grup özniteliklerini **Atföz Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Soloinsight-CloudGate SSO'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Soloinsight-CloudGate SSO Grup Özellikleri](media/soloinsight-cloudgate-sso-provisioning-tutorial/groupattributes.png)

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Soloinsight-CloudGate SSO için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Açık** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Soloinsight-CloudGate SSO'ya sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Soloinsight-CloudGate SSO'daki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna bağlantılar izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

