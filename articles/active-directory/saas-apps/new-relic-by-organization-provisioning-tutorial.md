---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için kuruluşa göre yeni relik yapılandırma | Microsoft Docs'
description: Azure AD 'den Kullanıcı hesaplarını yeni bir kuruluşa göre otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 04/14/2020
ms.author: Zhchia
ms.openlocfilehash: 7031bc0398e77dc308f9c08180338975831b411e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91321960"
---
# <a name="tutorial-configure-new-relic-by-organization-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için kuruluşa göre yeni relik yapılandırma

Bu öğreticide, otomatik Kullanıcı sağlamayı yapılandırmak için kuruluş ve Azure Active Directory (Azure AD) tarafından hem yeni relik hem de gerçekleştirmeniz gereken adımlar açıklanmaktadır. Yapılandırıldığında Azure AD, kullanıcıları ve grupları otomatik olarak sağlar ve Azure AD sağlama hizmeti 'ni kullanarak [kuruluşa yeni](https://newrelic.com/) bir şekilde yeniden hazırlar. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Kuruluşa göre yeni relik 'de Kullanıcı oluşturma
> * Artık erişim gerektirmeyen kullanıcıları kuruluşa göre yeni yeniden Kaldır
> * Kullanıcı özniteliklerinin kuruluş tarafından Azure AD ile yeni relik arasında eşitlenmiş olmasını sağlama
> * Kuruluşa göre yeni relik 'de gruplar ve grup üyelikleri sağlama
> * Kuruluşa göre yeni depoda [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD'de hazırlama [iznine](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) sahip bir kullanıcı hesabı (Uygulama Yöneticisi, Bulut Uygulaması Yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* Kullanıcılarınız tarafından kullanıcılarınızın erişimini istediğiniz bir veya daha fazla hesap. 

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) hakkında bilgi edinin.
2. [Hazırlık kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) dahil edilecek kullanıcıları seçin.
3. [Kuruluşa göre Azure AD ve yeni relik arasında eşlenecek](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)verileri belirleme. 

## <a name="step-2-configure-new-relic-by-organization-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için kuruluşa göre yeni relik yapılandırma

Kuruluşunuzda SCıM ve SSO 'yu yapılandırmak için hesap temsilcinizle çalışın veya support.newrelic.com adresinden destek alın. Hesap temsilcinizi şu şekilde belirtmeniz gerekir:

- Kuruluşunuzun adı
- Kuruluşla ilişkilendirilecek yeni relik hesabı kimliklerinin listesi

Bu bilgilerle, hesap temsilciniz yeni sistemimizde sizin için bir kuruluş kaydı oluşturur ve hesaplarınızı kuruluşla ilişkilendirir.

Hesap temsilciniz, kimlik sağlayıcınız için yeni relik SCıM/SSO uygulamasını yapılandırmak için ihtiyacınız olacak aşağıdaki bilgileri sağlar:

- SCıM uç noktası (kiracı URL 'SI)
- SCıM taşıyıcı belirteci (gizli belirteç)

SCıM taşıyıcı belirteci kullanıcılarınızın yeni bir relik durumunda sağlanmasına izin veriyor, bu nedenle lütfen değeri güvende tutun. Hesap temsilciniz, SCıM taşıyıcı belirtecini güvenli bir şekilde size aktaracaktır.

## <a name="step-3-add-new-relic-by-organization-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden kuruluşa göre yeni relik ekleme

Azure AD uygulama galerisinden kuruluşa göre yeni relik ekleme ' ye, kuruluşa göre yeni relik sağlamak için sağlama Daha önce, SSO için kuruluş tarafından yeni bir relik ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) bakın. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* Kullanıcı ve grupları kuruluşa göre yeni bir şekilde atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-new-relic-by-organization"></a>5. Adım. Otomatik Kullanıcı sağlamayı kuruluşa göre yeni relik olarak yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-new-relic-by-organization-in-azure-ad"></a>Azure AD 'de kuruluşa göre yeni relik için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **kuruluşa göre yeni relik**' i seçin.

    ![Uygulamalar listesindeki yeni relik bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, `https://scim-provisioning.service.newrelic.com/scim/v2` kiracı URL 'sini girin. **Gizli belirteçte**daha önce alınan SCIM kimlik doğrulama belirteci değerini girin. Azure AD 'nin yeni bir relik 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, yeni relik hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![alınıyor](./media/new-relic-by-organization-provisioning-tutorial/provisioning.png)

6. **Bildirim E-postası** alanına hazırlama hatası bildirimlerinin gönderilmesini istediğiniz kişinin veya grubun e-posta adresini yazıp **Hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde, **Kullanıcı Azure Active Directory kullanıcıları yeni yeniden kuruluşa göre zamanla**' yı seçin.

9. **Öznitelik eşleme** bölümünde, Azure AD 'Den yeni relik 'e göre eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için kuruluş tarafından yeni relik 'daki Kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, yeni yeniden düzenleme özelliği, bu özniteliğe göre kullanıcıların filtrelenmesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |externalId|Dize|
   |active|Boole|
   |emails[type eq "work"].value|Dize|
   |name.givenName|Dize|
   |ad. biçimlendirildi|Dize|
   |timezone|Dize|

10. **Eşlemeler** bölümünde, **Azure Active Directory gruplarını kuruluşa göre yeni relik olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde, Azure AD 'Den yeni relik 'e göre eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için kuruluşa göre yeni relik gruplarındaki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalId|Dize|
      |üyeler|Başvuru|

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. Azure AD sağlama hizmetini kuruluşa göre yeni yeniden etkinleştirmek için, **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** içinde istenen değerler ' i seçerek kuruluşa göre yeni relik sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngünün tamamlanması, Azure AD hazırlama hizmetinin çalıştığı süre boyunca yaklaşık olarak 40 dakikada bir gerçekleştirilen sonraki döngülerden daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
* Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) bakın
* Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) bakın.  


## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
