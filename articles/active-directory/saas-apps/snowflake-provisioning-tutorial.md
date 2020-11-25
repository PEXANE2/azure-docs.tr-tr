---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için kar tanesi yapılandırma | Microsoft Docs'
description: "' Nin kar/veya Kullanıcı hesaplarını otomatik olarak sağlaması ve serbest bırakmak üzere Azure Active Directory nasıl yapılandırılacağını öğrenin."
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: fcbfd3eb805e046647a2c28cef8c378b44026bd0
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96013199"
---
# <a name="tutorial-configure-snowflake-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için kar ke 'yi yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları [kar](https://www.Snowflake.com/pricing/)/veya grupları otomatik olarak sağlamak ve serbest bırakmak üzere yapılandırmak için kar ve Azure Active Directory (Azure AD) ' de gerçekleştirilecek adımları göstermektir. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md). 


> [!NOTE]
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="capabilities-supported"></a>Desteklenen özellikler
> [!div class="checklist"]
> * Kar halinde Kullanıcı oluşturma
> * Artık erişim gerektirdiklerinde, kar içindeki kullanıcıları kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile kar tanesi arasında eşitlenmiş olmasını sağlama
> * Kar grubundaki grupları ve grup üyeliklerini sağlama
> * Kar için [Çoklu oturum açma](./snowflake-tutorial.md) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Azure AD kiracısı](../develop/quickstart-create-new-tenant.md).
* Azure AD'de hazırlama [iznine](../users-groups-roles/directory-assign-admin-roles.md) sahip bir kullanıcı hesabı (Uygulama Yöneticisi, Bulut Uygulaması Yöneticisi, Uygulama Sahibi veya Genel Yönetici).
* [Bir kar tanesi kiracısı](https://www.Snowflake.com/pricing/).
* Kar ortamında yönetici izinleriyle bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](../app-provisioning/user-provisioning.md) hakkında bilgi edinin.
2. [Hazırlık kapsamına](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) dahil edilecek kullanıcıları seçin.
3. [Azure AD ve kar tanesi arasında](../app-provisioning/customize-application-attributes.md)hangi verilerin eşlendiğini belirleme. 

## <a name="step-2-configure-snowflake-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile kaynak sağlamayı desteklemek için kar ke 'yi yapılandırma

Azure AD ile otomatik Kullanıcı sağlaması için kar \ ' ı yapılandırmadan önce, kar için SCıM sağlamasını etkinleştirmeniz gerekecektir.

1. Kar Yönetici Konsolu 'nda oturum açın. Aşağıda gösterilen çalışma sayfasında gösterilen sorguyu girin ve **Çalıştır**' a tıklayın.

    ![Kar tanesi Yönetici Konsolu](media/Snowflake-provisioning-tutorial/image00.png)

2.  Kar tanesi kiracınız için bir SCıM erişim belirteci oluşturulacaktır. Bunu almak için aşağıda vurgulanan bağlantıya tıklayın.

    ![S C I M erişim belirtecine sahip olan kar özellikli çalışma sayfasının ekran görüntüsü.](media/Snowflake-provisioning-tutorial/image01.png)

3. Oluşturulan belirteç değerini kopyalayın ve **bitti**' ye tıklayın. Bu değer, Azure portal kar uygulama uygulamanızın sağlama sekmesindeki **gizli belirteç** alanına girilir.

    ![Metin alanına kopyalanmış belirtecin gösterildiği ve bitti seçeneğinin gösterildiği Ayrıntılar bölümünün ekran görüntüsü.](media/Snowflake-provisioning-tutorial/image02.png)

## <a name="step-3-add-snowflake-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden kar tanesi ekleme

Azure AD uygulama galerisindeki kar. Daha önce SSO için kar ayarla ayarı yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](../manage-apps/add-application-portal.md) bakın. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* Kullanıcı ve grupları kar alanına atarken **varsayılan erişim** dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](../develop/howto-add-app-roles-in-azure-ad-apps.md) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-snowflake"></a>5. Adım. Kar tanesi için otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alarak kar grubundaki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımlarında size kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-snowflake-in-azure-ad"></a>Azure AD 'de kar için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **kar tanesi**' ni seçin.

    ![Uygulamalar listesinde kar tanesi bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. Yönetici kimlik bilgileri bölümünde, sırasıyla **kiracı URL 'si** ve **gizli belirteç** alanları ' nda bulunan **SCIM 2,0 temel URL 'sini ve kimlik doğrulama belirteci** değerlerini girin. Azure AD 'nin kar alanına bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, kar için hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

7. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

8. **Kaydet**’e tıklayın.

9. **Eşlemeler** bölümü altında, **kullanıcıları kar alarak Azure Active Directory eşitler**' ı seçin.

10. **Öznitelik eşleme** bölümünde, Azure AD 'den kar tanesi ile eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için kar tanesi içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |active|Boole|
   |displayName|Dize|
   |emails[type eq "work"].value|Dize|
   |userName|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: defaultRole|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: Kullanıcı: defaultWarehouse|Dize|

11. **Eşlemeler** bölümünde, **Azure Active Directory gruplarını kar/zaman olarak eşitler**' ı seçin.

12. **Öznitelik eşleme** bölümünde Azure AD 'den kar tanesi ile eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için kar tanesi içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |üyeler|Başvuru|

13. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

14. Kar tanesi için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

15. **Ayarlar** bölümünde **kapsam** Içindeki istenen değerleri seçerek kar için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın. Bu seçenek kullanılamıyorsa, lütfen yönetici kimlik bilgileri altında gerekli alanları yapılandırın, **Kaydet** ' e tıklayın ve sayfayı yenileyin. 

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

16. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer.

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md) kullanın
2. Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) bakın
3. Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](../app-provisioning/application-provisioning-quarantine-status.md) bakın.  

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Kar tanesi tarafından oluşturulan SCıM belirteçleri 6 ay içinde sona erer. Sağlama eşitlenmesinin çalışmaya devam etmesine izin vermek için bunların süreleri dolmadan önce yenilenmesi gerektiğini unutmayın. 

## <a name="change-log"></a>Değişiklik Günlüğü

* 07/21/2020-tüm kullanıcılar için (etkin öznitelik aracılığıyla) geçici silme etkindir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Günlükleri incelemeyi ve sağlama etkinliğinde rapor almayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md).