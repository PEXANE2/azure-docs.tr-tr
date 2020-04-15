---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Oracle Cloud Infrastructure Console'u yapılandırın | Microsoft Dokümanlar"
description: Azure AD'den Oracle Cloud Infrastructure Console'a kullanıcı hesaplarını otomatik olarak nasıl sağlayıp geçici olarak sağmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: e22c8746-7638-4a0f-ae08-7db0c477cf52
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: Zhchia
ms.openlocfilehash: 5aa33529a1957b6e7728b3a87bacf6bb91d987ae
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81378957"
---
# <a name="tutorial-configure-oracle-cloud-infrastructure-console-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Oracle Cloud Infrastructure Console'u yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için hem Oracle Cloud Infrastructure Console hem de Azure Active Directory (Azure AD) işlemlerinde gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure AD Sağlama hizmetini kullanarak kullanıcıları ve grupları [Oracle Cloud Infrastructure Console'a](https://www.oracle.com/cloud/free/?source=:ow:o:p:nav:0916BCButton&intcmp=:ow:o:p:nav:0916BCButton) otomatik olarak hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Oracle Cloud Infrastructure Console'da kullanıcı oluşturma
> * Artık erişim gerektirmeyen kullanıcıları Oracle Cloud Infrastructure Console'da kaldırma
> * Kullanıcı özniteliklerini Azure AD ve Oracle Cloud Infrastructure Console arasında senkronize tutma
> * Oracle Cloud Infrastructure Console'da sağlama grupları ve grup üyelikleri
> * Oracle Cloud Infrastructure Console için [tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial) açma (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* Bir Oracle Bulut Altyapı Denetimi [kiracı.](https://www.oracle.com/cloud/sign-in.html?intcmp=OcomFreeTier&source=:ow:o:p:nav:0916BCButton)
* Yönetici izinleri içeren Oracle Cloud Infrastructure Control'deki bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD [ile Oracle Cloud Infrastructure Console arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlenemeye devam edin. 

## <a name="step-2-configure-oracle-cloud-infrastructure-console-to-support-provisioning-with-azure-ad"></a>2. Adım Oracle Cloud Infrastructure Console'u Azure AD ile sağlamayı destekleyecek şekilde yapılandırın

1. Oracle Cloud Infrastructure Console'un yönetici portalına giriş yapın. Ekranın sol üst köşesinde Kimlik **> Federasyonu**gidin.

    ![Oracle Admin](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/identity.png)

2. Oracle Identity Cloud Service Console'un yanındaki sayfada görüntülenen URL'ye tıklayın.

    ![Oracle URL'si](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/url.png)

3. Yeni bir kimlik sağlayıcısı oluşturmak için **Kimlik Sağlayıcı Ekle'yi** tıklatın. Kiracı URL'sinin bir parçası olarak kullanılmak üzere IdP kimliğini kaydedin. OAuth Client ve Grant IDCS Identity Domain Administrator AppRole oluşturmak için **Uygulamalar** sekmesinin yanındaki artı simgesine tıklayın.

    ![Oracle Bulut Simgesi](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/add.png)

4. Uygulamanızı yapılandırmak için aşağıdaki ekran görüntülerini izleyin. Yapılandırma yapıldıktan sonra **Kaydet'e**tıklayın.

    ![Oracle Yapılandırması](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/configuration.png)

    ![Oracle Jeton Verme Politikası](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/token-issuance.png)

5. Uygulamanızın yapılandırmalar sekmesi altında istemci kimliği ve istemci sırrı almak için **Genel Bilgiler** seçeneğini genişletin.

    ![Oracle belirteç üretimi](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/general-information.png)

6. Gizli bir belirteç Base64 kodlamak için istemci kimliği ve istemci gizli biçiminde **istemci kimliği:Client Secret**. Gizli jetonu kaydet. Bu değer, Azure portalındaki Oracle Cloud Infrastructure Console uygulamanızın sağlama sekmesinde **Gizli Belirteç** alanına girilir.

## <a name="step-3-add-oracle-cloud-infrastructure-console-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Oracle Bulut Altyapı Konsolu ekleme

Oracle Cloud Infrastructure Console'a sağlama yı yönetmeye başlamak için Azure AD uygulama galerisinden Oracle Cloud Infrastructure Console'u ekleyin. Daha önce SSO için Oracle Cloud Infrastructure Console kurulumu varsa, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Oracle Cloud Infrastructure Console'a atarken Varsayılan **Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-oracle-cloud-infrastructure-console"></a>5. Adım. Otomatik kullanıcı sağlamayı Oracle Cloud Altyapı Konsolu'na yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-oracle-cloud-infrastructure-console-in-azure-ad"></a>Azure AD'de Oracle Cloud Infrastructure Console için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Oracle Cloud Infrastructure Console'u**seçin.

    ![Uygulamalar listesindeki Oracle Cloud Infrastructure Console bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümüne, **Kiracı URL'sini** biçime `https://<IdP ID>.identity.oraclecloud.com/admin/v1` girdi. Örneğin, `https://idcs-0bfd023ff2xx4a98a760fa2c31k92b1d.identity.oraclecloud.com/admin/v1`. Gizli Belirteç'te daha önce alınan gizli belirteç değerini **girdi.** Azure AD'nin Oracle Cloud Infrastructure Console'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Oracle Cloud Infrastructure Console hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Oracle Cloud Infrastructure Console'a Senkronize Et'i**seçin.

9. Azure AD'den Oracle Cloud Infrastructure Console'a eşitlenen kullanıcı özniteliklerini **Atfetme-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Oracle Cloud Infrastructure Console'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Oracle Cloud Infrastructure Console API'nin bu özniteliğe bağlı olarak kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |userName|Dize|
      |Etkin|Boole|
      |başlık|Dize|
      |e-postalar[yazın eq "iş"].value|Dize|
      |tercihDil|Dize|
      |name.givenName|Dize|
      |name.familyName|Dize|
      |adresleri[yazın eq "iş"].biçimlendirilmiş|Dize|
      |adresleri[yazın eq "iş"].yerellik|Dize|
      |adresleri[yazın eq "iş"].region|Dize|
      |adresleri[yazın eq "iş"].postalCode|Dize|
      |adresleri[yazın eq "iş"].ülke|Dize|
      |adresleri[yazın eq "iş"].streetAddress|Dize|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|Dize|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|Dize|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|Dize|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|Dize|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Başvuru|
      |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|Dize|
      |urn:ietf:params:scim:schemas:oracle:idcs:extension:user:User:bypassNotification|Boole|
      |urn:ietf:params:scim:schemas:oracle:idcs:extension:user:user:isFederatedUser|Boole|

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Oracle Cloud Infrastructure Console'a Senkronize Et'i**seçin.

11. Azure AD'den Oracle Cloud Infrastructure Console'a eşitlenen grup özniteliklerini **Öznitelik-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Oracle Cloud Infrastructure Console'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalId|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Oracle Cloud Infrastructure Console için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Oracle Cloud Infrastructure Console'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve grupların ilk eşitleme döngüsünü başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki döngülere göre ilk çevrimin gerçekleşmesi daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hangi kullanıcıların başarılı veya başarısız bir şekilde sağlandığını belirlemek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanma
* Sağlama döngüsünün durumunu ve tamamlanmasına ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) kontrol edin
* Sağlama yapılandırması sağlıksız bir durumda gibi görünüyorsa, uygulama karantinaya alınır. Karantina durumları hakkında daha fazla bilgi [için burada.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
