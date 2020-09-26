---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için çalışma alanını Facebook ile yapılandırma | Microsoft Docs'
description: Otomatik Kullanıcı sağlamayı yapılandırmak için Facebook ve Azure Active Directory (Azure AD) ile her iki çalışma alanında gerçekleştirmeniz gereken adımları öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 04/28/2020
ms.author: jeedes
ms.openlocfilehash: c72ca7cb521c5c7e7ed33e9a0539de0df252ac92
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91321911"
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için çalışma alanını Facebook ile yapılandırma

Bu öğreticide, otomatik Kullanıcı sağlamayı yapılandırmak için Facebook ve Azure Active Directory (Azure AD) ile her iki çalışma alanında gerçekleştirmeniz gereken adımlar açıklanmaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti 'ni kullanarak [Facebook tarafından Iş yeri](https://work.workplace.com/) için kullanıcıları ve grupları otomatik olarak hazırlar ve serbest hazırlar. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../manage-apps/user-provisioning.md).

## <a name="capabilities-supported"></a>Desteklenen özellikler
> [!div class="checklist"]
> * Facebook tarafından çalışma alanında Kullanıcı oluşturma
> * Artık erişim gerektirdiklerinde Facebook 'tan Iş yerindeki kullanıcıları kaldırın
> * Facebook tarafından Azure AD ve çalışma alanı arasında eşitlenmiş Kullanıcı özniteliklerini koruyun
> * Facebook tarafından çalışma alanında [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/workplacebyfacebook-tutorial) (önerilir)

>[!VIDEO https://www.youtube.com/embed/oF7I0jjCfrY]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici)
* Facebook tarafından çoklu oturum açma özellikli abonelikte çalışma alanı

> [!NOTE]
> Bu öğreticideki adımları test etmek için, üretim ortamının kullanılmasını önermiyoruz.

Bu öğreticideki adımları test etmek için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadığı takdirde üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık deneme sürümü edinebilirsiniz.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) hakkında bilgi edinin.
2. [Hazırlık kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) dahil edilecek kullanıcıları seçin.
3. [Facebook tarafından Azure AD Ile çalışma alanı arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın.

## <a name="step-2-configure-workplace-by-facebook-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için Facebook ile çalışma alanı yapılandırma

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD 'deki hangi kullanıcıların ve/veya grupların çalışma alanınıza Facebook uygulaması tarafından erişmesi gereken kullanıcıları temsil ettiğini belirlemeniz gerekir. Karar verdikten sonra buradaki yönergeleri izleyerek bu kullanıcıları Facebook uygulaması tarafından çalışma alanınıza atayabilirsiniz:

*   Sağlama yapılandırmasını test etmek için Facebook tarafından çalışma alanına tek bir Azure AD kullanıcısının atanması önerilir. Ek kullanıcılar ve/veya grupları daha sonra atanabilir.

*   Facebook tarafından çalışma alanına bir Kullanıcı atarken geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan erişim" rolü sağlama için çalışmaz.

## <a name="step-3-add-workplace-by-facebook-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden Facebook ile çalışma alanı ekleme

Azure AD uygulama galerisindeki Facebook 'a çalışma alanı ekleyerek Facebook ile çalışma alanına sağlamayı yönetmeye başlayın. Daha önce Facebook için Facebook ile çalışma alanı ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) bakın.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* Facebook tarafından çalışma alanına kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) belirtebilirsiniz. 

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Facebook tarafından çalışma alanı**' nı seçin.

    ![Uygulamalar listesinde Facebook tarafından çalışma alanı bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde **Yetkilendir**' e tıklayın. Facebook 'ın yetkilendirme sayfası tarafından çalışma alanına yönlendirilirsiniz. Çalışma alanınızı Facebook Kullanıcı adı ile girin ve **devam** düğmesine tıklayın. Azure AD 'nin Facebook ile çalışma alanına bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Facebook hesabının çalışma alanınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Ekran görüntüsü, yönetici kimlik bilgileri iletişim kutusunu bir Yetkilendir seçeneği ile gösterir.](./media/workplacebyfacebook-provisioning-tutorial/provisioning.png)

    ![Yetkilendir](./media/workplacebyfacebook-provisioning-tutorial/workplacelogin.png)

6. **Bildirim E-postası** alanına hazırlama hatası bildirimlerinin gönderilmesini istediğiniz kişinin veya grubun e-posta adresini yazıp **Hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde, **Facebook Ile Azure Active Directory kullanıcıları çalışma alanına eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde, Azure AD 'den Facebook tarafından çalışma alanına eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için Facebook tarafından çalışma alanındaki Kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Facebook API 'si tarafından bu özniteliğe göre kullanıcıların filtrelenmesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |displayName|Dize|
   |active|Boole|
   |başlık|Boole|
   |emails[type eq "work"].value|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |ad. biçimlendirildi|Dize|
   |adresler [tür EQ "iş"]. biçimlendirildi|Dize|
   |adresler [tür EQ "Work"]. streetAddress|Dize|
   |adresler [tür EQ "iş"]. konum|Dize|
   |adresler [tür EQ "iş"]. bölge|Dize|
   |adresler [tür EQ "iş"]. ülke|Dize|
   |adresler [tür EQ "iş"]. PostaKodu|Dize|
   |adresler [tür EQ "Other"]. biçimlendirildi|Dize|
   |phoneNumbers[type eq "work"].value|Dize|
   |phoneNumbers[type eq "mobile"].value|Dize|
   |phoneNumbers [tür EQ "Faks"]. değer|Dize|
   |externalId|Dize|
   |preferredLanguage|Dize|
   |urn:scim:schemas:extension:enterprise:1.0.manager|Dize|
   |urn:scim:schemas:extension:enterprise:1.0.department|Dize|
   |urn:scim:schemas:extension:enterprise:1.0.division|Dize|
   |urn:scim:schemas:extension:enterprise:1.0.organization|Dize|
   |urn:scim:schemas:extension:enterprise:1.0.costCenter|Dize|
   |urn:scim:schemas:extension:enterprise:1.0.employeeNumber|Dize|
   |urn: Scim: schemas: uzantı: Facebook: auth_method: 1.0: auth_method|Dize|
   |urn: Scim: schemas: uzantı: Facebook: Frontline: 1.0. is_frontline|Boole|
   |urn: Scim: schemas: uzantı: Facebook: starsımmtarihlere: 1.0. startDate|Tamsayı|


10. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

11. Facebook tarafından çalışma alanı için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **kapsam** Içindeki istenen değerleri seçerek Facebook tarafından çalışma alanına sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

13. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngünün tamamlanması, Azure AD hazırlama hizmetinin çalıştığı süre boyunca yaklaşık olarak 40 dakikada bir gerçekleştirilen sonraki döngülerden daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) bakın
3. Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) bakın.

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
*  Başarısız bir Kullanıcı görürseniz ve "1789003" koduna sahip bir denetim günlüğü olayı varsa, kullanıcının doğrulanmamış bir etki alanından olduğu anlamına gelir.

## <a name="change-log"></a>Değişiklik günlüğü

* 09/10/2020-kurumsal öznitelikler "bölme", "kuruluş", "costCenter" ve "employeeNumber" için destek eklendi. "StartDate", "auth_method" ve "Frontline" özel öznitelikleri için destek eklendi

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
