---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nı yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory'yi otomatik olarak sağlama ve kullanıcı hesaplarını SAP Bulut Platformu Kimlik Doğrulaması'na sağlama ve sağlamadan çıkarma için nasıl yapılandırılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f49b5738-c769-403b-8f29-84ddeea7fbf1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2019
ms.author: Zhchia
ms.openlocfilehash: c30a7b1e6440cf69f7a4858273b365d885e5ec7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77060489"
---
# <a name="tutorial-configure-sap-cloud-platform-identity-authentication-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nı yapılandırın

Bu öğreticinin amacı, Azure AD'yi kullanıcıları ve/veya grupları SAP Bulut Platformu Kimlik Doğrulaması'na otomatik olarak sağlama ve azaltma için yapılandırmak için SAP Bulut Platformu Kimlik Kimlik Doğrulaması ve Azure Etkin Dizini 'nde (Azure AD) gerçekleştirilecek adımları göstermektir.

> [!NOTE]
> Bu öğretici, Azure AD Kullanıcı Sağlama Hizmeti'nin üzerine inşa edilmiş bir bağlayıcıyı açıklar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz.
>
> Bu bağlayıcı şu anda Genel Önizleme'de. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için, [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* Azure AD kiracı
* [SAP Bulut Platformu Kimlik Kimlik Doğrulaması kiracı](https://cloudplatform.sap.com/pricing.html)
* Yönetici izinleri ile SAP Bulut Platformu Kimlik Kimlik Doğrulaması'ndaki bir kullanıcı hesabı.

## <a name="assigning-users-to-sap-cloud-platform-identity-authentication"></a>Kullanıcıları SAP Bulut Platformu Kimlik Kimlik Doğrulaması'na atama

Azure Active Directory, hangi kullanıcıların seçili uygulamalara erişmesi gerektiğini belirlemek için *atamalar* adlı bir kavram kullanır. Otomatik kullanıcı sağlama bağlamında, yalnızca Azure AD'deki bir uygulamaya atanan kullanıcılar ve/veya gruplar eşitlenir.

Otomatik kullanıcı sağlamayı yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki hangi kullanıcıların ve/veya grupların SAP Bulut Platformu Kimlik Doğrulaması'na erişmesi gerektiğine karar vermelisiniz. Karar verildikten sonra, bu kullanıcıları ve/veya grupları buradaki yönergeleri izleyerek SAP Bulut Platformu Kimlik Kimlik Doğrulaması'na atayabilirsiniz:
* [Bir kurumsal uygulamaya kullanıcı veya grup atama](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-sap-cloud-platform-identity-authentication"></a>SAP Bulut Platformu Kimlik Doğrulaması'na kullanıcı atamak için önemli ipuçları

* Otomatik kullanıcı sağlama yapılandırmasını sınamak için SAP Bulut Platformu Kimlik Doğrulaması'na tek bir Azure AD kullanıcısıatanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

* Bir kullanıcıyı SAP Bulut Platformu Kimlik Doğrulaması'na atarken, atama iletişim kutusunda uygulamaya özgü geçerli bir rolü (varsa) seçmeniz gerekir. **Varsayılan Erişim** rolüne sahip kullanıcılar sağlama nın dışında tutulur.

## <a name="setup-sap-cloud-platform-identity-authentication-for-provisioning"></a>Kurulum SAP Bulut Platformu Kimlik Kimlik Doğrulaması sağlama

1. SAP Cloud [Platform Kimlik Doğrulama Yönetici Konsolunuzda](https://sapmsftintegration.accounts.ondemand.com/admin)oturum açın. **Kullanıcılara & Yetkilendirmeleri > Yöneticilere**gidin.

    ![SAP Bulut Platformu Kimlik Doğrulama Yönetici Konsolu](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/adminconsole.png)

2.  Bir Yönetici Kullanıcısı oluşturun ve kullanıcıyı seçin.  

3.  Yetkilendirmeleri Yapılandırın altında, **Kullanıcıları Yönet** ve **Grupları Yönet'e**karşı geçiş düğmesini açın.

    ![SAP Bulut Platformu Kimlik Kimlik Doğrulama Ekle SCIM](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/configurationauth.png)

4. Hesabınızı etkinleştirmek ve **SAP Bulut Platformu Kimlik Doğrulama Hizmeti**için bir parola ayarlamak için bir e-posta alacaksınız.

4.  Kullanıcı **Kimliği** ve **Şifresini**kopyalayın. Bu değerler, Azure portalındaki SAP Bulut Platformu Kimlik Kimlik Doğrulama uygulamanızın Sağlama sekmesine sırasıyla Yönetici Kullanıcı Adı ve Yönetici Parolası alanlarına girilir.

## <a name="add-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Galeriden SAP Bulut Platformu Kimlik Doğrulaması ekleme

Azure AD ile otomatik kullanıcı sağlama için SAP Bulut Platformu Kimlik Doğrulaması'nı yapılandırmadan önce, Azure AD uygulama galerisinden yönetilen SaaS uygulamaları listenize SAP Bulut Platformu Kimlik Doğrulaması eklemeniz gerekir.

**Azure AD uygulama galerisinden SAP Bulut Platformu Kimlik Doğrulaması eklemek için aşağıdaki adımları gerçekleştirin:**

1. Azure **[portalında,](https://portal.azure.com)** soldaki gezinti panelinde **Azure Etkin Dizin'i**seçin.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **uygulamalara**gidin ve ardından **Tüm uygulamaları**seçin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni bir uygulama eklemek için bölmenin üst kısmındaki **Yeni uygulama** düğmesini seçin.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **SAP Bulut Platformu Kimlik Kimlik Doğrulaması'nı**girin, sonuç paneline SAP Bulut Platformu Kimlik Kimlik **Doğrulaması'nı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

    ![Sap Bulut Platformu Kimlik Doğrulama sonuç listesinde](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-sap-cloud-platform-identity-authentication"></a>Otomatik kullanıcı sağlamayı SAP Bulut Platformu Kimlik Doğrulaması'na yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı OLARAK SAP Bulut Platformu Kimlik Kimlik Doğrulaması'ndaki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size rehberlik eder.

> [!TIP]
> SAP Bulut Platformu Kimlik Kimlik Doğrulaması için SAML tabanlı tek oturum açmayı, [SAP Bulut Platformu Kimlik Kimlik Doğrulama tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial)açma öğreticisinde verilen yönergeleri izleyerek etkinleştirmeyi de seçebilirsiniz. Tek oturum açma, otomatik kullanıcı sağlamadan bağımsız olarak yapılandırılabilir, ancak bu iki özellik birbirini birbiriyle dışa

### <a name="to-configure-automatic-user-provisioning-for-sap-cloud-platform-identity-authentication-in-azure-ad"></a>Azure AD'de SAP Bulut Platformu Kimlik Doğrulaması için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **SAP Bulut Platformu Kimlik Doğrulaması'nı**seçin.

    ![Uygulamalar listesindeki SAP Bulut Platformu Kimlik Kimlik Doğrulama bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://<tenantID>.accounts.ondemand.com/service/scim ` altında, **Kiracı URL'ye**giriş . Daha önce alınan **Kullanıcı Kimliği** ve **Parola** değerlerini sırasıyla Yönetici **Kullanıcı Adı** ve **Yönetici Şifresi'ne** girin. Azure AD'nin SAP Bulut Platformu Kimlik Doğrulaması'na bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, SAP Bulut Platformu Kimlik Kimlik Doğrulama hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL + Belirteç](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/testconnection.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve onay kutusunu işaretleyin - **Bir hata oluştuğunda e-posta bildirimi gönderin.**

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**'e tıklayın.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını SAP Bulut Platformu Kimlik Doğrulaması için Senkronize Et'i**seçin.

    ![SAP Bulut Platformu Kimlik Kimlik Doğrulama Kullanıcı Eşlemeleri](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/mapping.png)

9. Azure AD'den SAP Bulut Platformu Kimlik Doğrulaması'na eşitlenen kullanıcı özniteliklerini **Öznitelik Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için SAP Bulut Platformu Kimlik Kimlik Doğrulaması'ndaki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

    ![SAP Bulut Platformu Kimlik Kimlik Doğrulama Kullanıcı Öznitelikleri](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/userattributes.png)

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. SAP Bulut Platformu Kimlik Kimlik Doğrulaması için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek SAP Bulut Platformu Kimlik Kimlik Doğrulaması'na sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve/veya grupların ilk eşitlemisini başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki eşitlemelerden daha uzun süren ilk eşitlemenin gerçeklemi daha uzun sürer. Sap Bulut Platformu Kimlik Kimlik Doğrulaması'nda Azure AD sağlama hizmeti tarafından gerçekleştirilen tüm eylemleri açıklayan ilerlemeyi izlemek ve sağlama etkinlik raporuna olan bağlantıları izlemek için **Eşitleme Ayrıntıları** bölümünü kullanabilirsiniz.

Azure AD sağlama günlüklerini nasıl okuyabilirsiniz hakkında daha fazla bilgi için [bkz.](../app-provisioning/check-status-user-account-provisioning.md)

## <a name="connector-limitations"></a>Konektör sınırlamaları

* SAP Bulut Platformu Kimlik Kimlik Doğrulama'nın SCIM bitiş noktası, belirli niteliklerin belirli biçimde olmasını gerektirir. Burada bu öznitelikleri ve özel [biçimi](https://help.sap.com/viewer/6d6d63354d1242d185ab4830fc04feb1/Cloud/en-US/b10fc6a9a37c488a82ce7489b1fab64c.html#)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)

