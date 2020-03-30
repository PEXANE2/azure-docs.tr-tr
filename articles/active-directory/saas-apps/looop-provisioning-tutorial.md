---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Looop'u yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi, kullanıcı hesaplarını Looop'a otomatik olarak sağlamak ve sağlamadan çıkarmak için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 0efe2262-43c3-4e0c-97fa-9344385638e2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2019
ms.author: Zhchia
ms.openlocfilehash: e3e25a8c27b9a5c1bc1e7673300ac8aca9377c08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057456"
---
# <a name="tutorial-configure-looop-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Looop'u yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları Looop'a otomatik olarak sağlamak ve sağlamadan çıkarmak üzere yapılandırmak için Looop ve Azure Etkin Dizini'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [Bir Looop kiracı](https://www.looop.co/pricing/)
* Yönetici izinleri olan bir Looop'ta bir kullanıcı hesabı.

## <a name="assign-users-to-looop"></a>Kullanıcıları Looop'a atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için atamalar adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların Looop'a erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları burada talimatları izleyerek Looop'a atayabilirsiniz:

* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-looop"></a>Looop'a kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için Looop'a tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı Looop'a atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="set-up-looop-for-provisioning"></a>Tedarik için Looop'u ayarlama

Azure AD ile otomatik kullanıcı sağlama için Looop'u yapılandırmadan önce, Looop'tan bazı sağlama bilgileri almanız gerekir.

1. [Looop Yönetici Konsolunuzda](https://app.looop.co/#/login) oturum açın ve **Hesap'ı**seçin. **Hesap Ayarları** altında **Kimlik Doğrulama'yı**seçin.

    ![Looop SCIM ekle](media/looop-provisioning-tutorial/admin.png)

2. **SCIM Tümleştirmesi**altında **Yeniden Ayarla Belirteci'ni** tıklatarak yeni bir belirteç oluşturun.

    ![Looop SCIM ekle](media/looop-provisioning-tutorial/resettoken.png)

3. **SCIM Bitiş Noktası'nı** ve **Belirteci**kopyalayın. Bu değerler, Azure portalındaki Looop uygulamanızın Sağlama sekmesindeki **Kiracı URL'si** ve Gizli **Belirteç** alanlarına girilir. 

    ![Looop Oluştur Belirteci](media/looop-provisioning-tutorial/token.png)

## <a name="add-looop-from-the-gallery"></a>Galeriden Looop ekle

Looop'u Azure AD ile otomatik kullanıcı sağlama için yapılandırmak için, Yönetilen SaaS uygulamaları listenize Azure AD uygulama galerisinden Looop'u eklemeniz gerekir.

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna, **Looop**girin , sonuç panelinde **Looop** seçin. 

    ![Sonuç listesinde Looop](common/search-new-app.png)

5. Sizi Looop'un giriş sayfasına yönlendirecek **Olan Looop için Kaydol** düğmesini seçin. 

    ![Looop OIDC Ekle](media/looop-provisioning-tutorial/signup.png)

6. Looop bir OpenIDConnect uygulaması olduğundan, Microsoft iş hesabınızı kullanarak Looop'a giriş yapmayı seçin.

    ![Looop OIDC giriş](media/looop-provisioning-tutorial/msftlogin.png)

7. Başarılı bir kimlik doğrulamadan sonra, onay sayfası için onay istemini kabul edin. Uygulama daha sonra otomatik olarak kiracınıza eklenecek ve Looop hesabınıza yönlendirileceksiniz.

    ![Looop OIDc Onayı](media/looop-provisioning-tutorial/accept.png)

## <a name="configure-automatic-user-provisioning-to-looop"></a>Looop için otomatik kullanıcı sağlama yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak Looop'taki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol açar.

### <a name="to-configure-automatic-user-provisioning-for-looop-in-azure-ad"></a>Azure AD'de Looop için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulama listesinde **Looop'u**seçin.

    ![Uygulamalar listesindeki Looop bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://<organisation_domain>.looop.co/scim/v2` altında, **Kiracı URL'ye**giriş . Örneğin, `https://demo.looop.co/scim/v2`. **Gizli**Jeton'da Looop'tan daha önce aldığınız ve kaydettiğiniz değeri girdiniz. Azure AD'nin Looop'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Looop hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Looop'a Senkronize Et'i**seçin.

    ![Looop Kullanıcı Haritalamaları](media/looop-provisioning-tutorial/usermappings.png)

9. Azure AD'den Looop'a eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Looop'taki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Looop Kullanıcı Özellikleri](media/looop-provisioning-tutorial/userattributes.png)

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Meta Ağlar Bağlayıcısına Senkronize Et'i**seçin.

    ![Looop Grup Haritalamaları](media/looop-provisioning-tutorial/groupmappings.png)

11. Azure AD'den Meta Networks Connector'a, **Öznitelik Eşleme** bölümünde senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Meta Networks Connector'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![Looop Grup Özellikleri](media/looop-provisioning-tutorial/groupattributes.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Looop için Azure AD sağlama hizmetini etkinleştirmek için Ayarlar bölümünde **Sağlama Durumunu** **Açık** olarak değiştirin. **Settings**

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek Looop'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Kaydetme **Ayrıntıları** bölümünü, ilerlemeyi izlemek ve Looop'taki Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan sağlama faaliyet raporuna bağlı bağlantıları izlemek için kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)


