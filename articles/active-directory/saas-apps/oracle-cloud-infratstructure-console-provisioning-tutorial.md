---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Oracle bulut altyapı konsolunu yapılandırma | Microsoft Docs'
description: Azure AD 'den Oracle bulut altyapısı konsoluna Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81378957"
---
# <a name="tutorial-configure-oracle-cloud-infrastructure-console-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için Oracle bulut altyapı konsolunu yapılandırma

Bu öğreticide, otomatik Kullanıcı sağlamayı yapılandırmak için hem Oracle bulut altyapısı konsolunda hem de Azure Active Directory (Azure AD) gerçekleştirmeniz gereken adımlar açıklanmaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmetini kullanarak kullanıcıları ve grupları [Oracle bulut altyapısı konsoluna](https://www.oracle.com/cloud/free/?source=:ow:o:p:nav:0916BCButton&intcmp=:ow:o:p:nav:0916BCButton) otomatik olarak sağlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Oracle bulut altyapısı konsolunda kullanıcı oluşturma
> * Oracle bulut altyapısı konsolundaki kullanıcıları artık erişim gerektirdiklerinde kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile Oracle bulut altyapı konsolu arasında eşitlenmiş olmasını sağlama
> * Oracle bulut altyapısı konsolunda grupları ve grup üyeliklerini sağlama
> * Oracle bulut altyapı konsolu 'nda [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Oracle bulut altyapısı denetim [kiracısı](https://www.oracle.com/cloud/sign-in.html?intcmp=OcomFreeTier&source=:ow:o:p:nav:0916BCButton).
* Oracle bulut altyapısı denetimindeki yönetici izinleriyle bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile Oracle bulut altyapısı konsolu arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-oracle-cloud-infrastructure-console-to-support-provisioning-with-azure-ad"></a>Adım 2. Oracle bulut altyapı konsolunu Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

1. Oracle bulut altyapısı konsolunun yönetici portalında oturum açın. Ekranın sol üst köşesinde **kimlik > Federasyonu**' ne gidin.

    ![Oracle Yöneticisi](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/identity.png)

2. Oracle Identity Cloud Service konsolunun yanındaki sayfada görünen URL 'ye tıklayın.

    ![Oracle URL 'SI](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/url.png)

3. Yeni bir kimlik sağlayıcısı oluşturmak için **kimlik sağlayıcısı ekle** ' ye tıklayın. Kiracı URL 'sinin bir parçası olarak kullanılacak IDP kimliğini kaydedin. Bir OAuth Istemcisi oluşturmak ve ıDCS kimlik alanı yöneticisi AppRole sağlamak için **uygulamalar** sekmesinin yanındaki artı simgesine tıklayın.

    ![Oracle bulut simgesi](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/add.png)

4. Uygulamanızı yapılandırmak için aşağıdaki ekran görüntülerini izleyin. Yapılandırma tamamlandıktan sonra **Kaydet**' e tıklayın.

    ![Oracle yapılandırması](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/configuration.png)

    ![Oracle belirteci verme Ilkesi](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/token-issuance.png)

5. Uygulamanızın konfigürasyonlar sekmesinde, istemci KIMLIĞINI ve istemci gizli anahtarını almak için **genel bilgiler** seçeneğini genişletin.

    ![Oracle belirteci oluşturma](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/general-information.png)

6. Gizli bir belirteç oluşturmak için istemci KIMLIĞINI ve istemci gizli anahtarını istemci **kimliği: Istemci parolası**biçiminde kodlayın. Gizli dizi belirtecini kaydedin. Bu değer, Azure portal Oracle bulut altyapısı konsol uygulamanızın sağlama sekmesinde bulunan **gizli dizi belirteci** alanına girilecektir.

## <a name="step-3-add-oracle-cloud-infrastructure-console-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden Oracle bulut altyapısı konsolunu ekleme

Oracle bulut altyapısı konsoluna sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden Oracle bulut altyapısı konsolunu ekleyin. Daha önce, SSO için Oracle bulut altyapısı konsolunu ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Oracle bulut altyapısı konsoluna Kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-oracle-cloud-infrastructure-console"></a>5. Adım. Oracle bulut altyapısı konsoluna otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-oracle-cloud-infrastructure-console-in-azure-ad"></a>Azure AD 'de Oracle bulut altyapısı konsoluna yönelik otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Oracle Cloud Infrastructure konsolu**' nu seçin.

    ![Uygulamalar listesinde Oracle Cloud Infrastructure konsol bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde **kiracı URL** 'sini biçimde girin `https://<IdP ID>.identity.oraclecloud.com/admin/v1` . Örneğin, `https://idcs-0bfd023ff2xx4a98a760fa2c31k92b1d.identity.oraclecloud.com/admin/v1`. Gizli belirteç değerini **gizli belirteçte**daha önce alındı olarak girin. Azure AD 'nin Oracle bulut altyapısı konsoluna bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Oracle Cloud Infrastructure konsolu hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![alınıyor](./media/oracle-cloud-infratstructure-console-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Oracle bulut altyapısı konsolu ile eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde, Azure AD 'Den Oracle bulut altyapısı konsoluna eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Oracle bulut altyapısı konsolundaki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Oracle bulut altyapısı konsolu API 'sinin, bu özniteliğe göre kullanıcıların filtrelemesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |userName|Dize|
      |bkz|Boole|
      |başlık|Dize|
      |e-postalar [tür EQ "iş"]. değer|Dize|
      |preferredLanguage|Dize|
      |ad.|Dize|
      |Name. familyName|Dize|
      |adresler [tür EQ "iş"]. biçimlendirildi|Dize|
      |adresler [tür EQ "iş"]. konum|Dize|
      |adresler [tür EQ "iş"]. bölge|Dize|
      |adresler [tür EQ "iş"]. PostaKodu|Dize|
      |adresler [tür EQ "iş"]. ülke|Dize|
      |adresler [tür EQ "Work"]. streetAddress|Dize|
      |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: employeeNumber|Dize|
      |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize|
      |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: costCenter|Dize|
      |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Bölüm|Dize|
      |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager|Başvuru|
      |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Organization|Dize|
      |urn: IETF: params: Scim: schemas: Oracle: IDCs: Extension: User: User: bypassNotification|Boole|
      |urn: IETF: params: Scim: schemas: Oracle: IDCs: Extension: User: User: ısfederateduser|Boole|

10. **Eşlemeler** bölümünde, **Azure Active Directory grupları Oracle bulut altyapısı konsolu olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde, Azure AD 'Den Oracle bulut altyapısı konsoluna eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Oracle bulut altyapısı konsolundaki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalID|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Oracle bulut altyapısı konsolu için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içinde Istenen değerleri seçerek Oracle bulut altyapısı konsoluna sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
* Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
* Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
