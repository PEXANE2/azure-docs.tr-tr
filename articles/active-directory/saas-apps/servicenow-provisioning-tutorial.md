---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için ServiceNow 'ı yapılandırma | Microsoft Docs"
description: Azure AD 'den ServiceNow 'a Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/10/2019
ms.author: jeedes
ms.openlocfilehash: 71f4cfa9e7b69e51bafa5364f857f8ab689f2ad0
ms.sourcegitcommit: 19ffdad48bc4caca8f93c3b067d1cf29234fef47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97955476"
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için ServiceNow 'ı yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem ServiceNow hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti 'ni kullanarak [ServiceNow](https://www.servicenow.com/) için kullanıcıları ve grupları otomatik olarak hazırlar ve serbest hazırlar. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * ServiceNow 'da Kullanıcı oluşturma
> * ServiceNow 'daki kullanıcıları artık erişim gerektirdiklerinde kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile ServiceNow arasında eşitlenmiş olmasını sağlama
> * ServiceNow 'da grupları ve grup üyeliklerini sağlama
> * ServiceNow ['Da çoklu oturum açma](servicenow-tutorial.md) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](../develop/quickstart-create-new-tenant.md) 
* Azure AD'de hazırlama [iznine](../roles/permissions-reference.md) sahip bir kullanıcı hesabı (Uygulama Yöneticisi, Bulut Uygulaması Yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* Calgary veya üzeri bir [ServiceNow örneği](https://www.servicenow.com/)
* Helsinki veya üzeri bir [ServiceNow Express örneği](https://www.servicenow.com/)
* Yönetim rolüyle ServiceNow 'da bir kullanıcı hesabı

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](../app-provisioning/user-provisioning.md) hakkında bilgi edinin.
2. [Hazırlık kapsamına](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) dahil edilecek kullanıcıları seçin.
3. [Azure AD ve ServiceNow arasında](../app-provisioning/customize-application-attributes.md)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-servicenow-to-support-provisioning-with-azure-ad"></a>Adım 2. ServiceNow 'ı Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

1. ServiceNow örnek adınızı belirler. Örnek adını ServiceNow 'e erişmek için kullandığınız URL 'de bulabilirsiniz. Aşağıdaki örnekte, örnek adı dev35214 ' dir.

   ![ServiceNow örneği](media/servicenow-provisioning-tutorial/servicenow-instance.png)

2. ServiceNow 'da yönetici için kimlik bilgilerini alın. ServiceNow içindeki kullanıcı profiline gidin ve kullanıcının yönetici rolüne sahip olduğunu doğrulayın. 

   ![ServiceNow yönetici rolü](media/servicenow-provisioning-tutorial/servicenow-admin-role.png)


## <a name="step-3-add-servicenow-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden ServiceNow ekleme

ServiceNow 'ı Azure AD Uygulama Galerisi ' ne ekleyerek ServiceNow 'a sağlamayı yönetmeye başlayın. Önceden SSO için ServiceNow kurulumunu yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](../manage-apps/add-application-portal.md) bakın. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* ServiceNow 'a Kullanıcı ve grup atarken **varsayılan erişim** dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](../develop/howto-add-app-roles-in-azure-ad-apps.md) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-servicenow"></a>5. Adım. ServiceNow 'a otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-servicenow-in-azure-ad"></a>Azure AD 'de ServiceNow için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **ServiceNow**' ı seçin.

    ![Uygulamalar listesindeki ServiceNow bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Sağlama seçeneğinin kullanıma aldığı yönetim seçeneklerinin ekran görüntüsü.](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Otomatik seçeneği olarak adlandırılan sağlama modu açılan listesinin ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde ServiceNow yönetici kimlik bilgilerinizi ve Kullanıcı adınızı girin. Azure AD 'nin ServiceNow 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, ServiceNow hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Ekran görüntüsü, yönetici kimlik bilgilerini girebileceğiniz hizmet sağlama sayfasını gösterir.](./media/servicenow-provisioning-tutorial/servicenow-provisioning.png)

6. **Bildirim E-postası** alanına hazırlama hatası bildirimlerinin gönderilmesini istediğiniz kişinin veya grubun e-posta adresini yazıp **Hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları ServiceNow**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den ServiceNow 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için ServiceNow içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](../app-provisioning/customize-application-attributes.md)değiştirmeyi seçerseniz ServiceNow API 'sinin, bu özniteliğe göre kullanıcıların filtrelemesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

10. **Eşlemeler** bölümünde, **Azure Active Directory grupları ServiceNow**' a eşitler ' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den ServiceNow 'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, ServiceNow güncelleştirme işlemlerinde grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. ServiceNow için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsamda** Istenen değerleri seçerek ServiceNow 'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngünün tamamlanması, Azure AD hazırlama hizmetinin çalıştığı süre boyunca yaklaşık olarak 40 dakikada bir gerçekleştirilen sonraki döngülerden daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](../reports-monitoring/concept-provisioning-logs.md) kullanın
2. Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) bakın
3. Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](../app-provisioning/application-provisioning-quarantine-status.md) bakın.  

## <a name="troubleshooting-tips"></a>Sorun Giderme İpuçları
* **Invalidlookupreference:** ServiceNow içindeki bölüm ve konum gibi belirli öznitelikleri sağlarken, bu değerler ServiceNow içindeki bir başvuru tablosunda zaten mevcut olmalıdır. Örneğin, ServiceNow 'daki **tablo adı ekleme** tablosunda iki konumunuz (Seattle, Los Angeles) ve üç departman (Sales, finans, pazarlama) olabilir. Departmanı "Sales" ve Location 'ın "Seattle" olduğu bir Kullanıcı sağlamaya çalışırsanız, başarılı bir şekilde hazırlanacaktır. "Sales" departmanı ve "LA" konumuyla bir Kullanıcı sağlamaya çalışırsanız Kullanıcı sağlanmaz. Konum, ServiceNow 'daki başvuru tablosuna eklenmelidir veya Azure AD 'deki User özniteliği ServiceNow 'ın biçimiyle eşleşecek şekilde güncellenmelidir. 
* **Entryjoiningpropertyvalueısmissing:** Eşleşen özniteliği tanımlamak için [öznitelik eşlemelerinizi](../app-provisioning/customize-application-attributes.md) gözden geçirin. Sağlamaya çalıştığınız kullanıcı veya grupta bu değer bulunmalıdır. 
* Tüm gereksinimleri veya sınırlamaları anlamak için [ServiceNow soap API](https://docs.servicenow.com/bundle/newyork-application-development/page/integrate/web-services-apis/reference/r_DirectWebServiceAPIFunctions.html) 'sini gözden geçirin (örneğin, bir kullanıcı için ülke kodunu belirtme biçimi)
* Sağlama istekleri varsayılan olarak https://{-örnek-adı}. Service-Now. com/{Table-Name} olarak gönderilir. Özel bir kiracı URL 'SI gerekiyorsa, örnek adı alanında tüm URL 'YI sağlayabilirsiniz.
* **ServiceNowInstanceInvalid** 
  
  `Details: Your ServiceNow instance name appears to be invalid.  Please provide a current ServiceNow administrative user name and          password along with the name of a valid ServiceNow instance.`                                                              

   Bu hata ServiceNow örneğiyle iletişim kuran bir sorunu gösterir. 
   
   Test bağlantı sorunlarıyla karşılaşıyorsanız, ServiceNow 'da aşağıdaki ayarları **devre dışı** olarak yapmayı deneyin:
   
   1. **Sistem güvenliği**  >  **yüksek güvenlik ayarları**' nı seçin,  >  **gelen şema istekleri için temel kimlik doğrulaması gerektirir**.
   2. **Sistem özelliklerini** seçin  >  **Web Hizmetleri**  >  **gelen SOAP istekleri için temel yetkilendirme gerektir**.

   ![SOAP isteğini yetkilendirme](media/servicenow-provisioning-tutorial/servicenow-webservice.png)

   Sorunlarınızı giderirse ServiceNow desteğiyle iletişime geçin ve sorun gidermeye yardımcı olması için SOAP hata ayıklamayı etkinleştirmesini isteyin. 

* **IP aralıkları** 

   Azure AD sağlama hizmeti şu anda belirli bir IP aralığı altında çalışır. Gerekirse, diğer IP aralıklarını kısıtlayabilir ve bu belirli IP aralıklarını uygulamanızın izin öğesine ekleyerek Azure AD sağlama hizmeti 'nden uygulamanıza trafik akışına izin verebilirsiniz. [IP aralıklarında](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups#ip-ranges)belgelere başvurun.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)