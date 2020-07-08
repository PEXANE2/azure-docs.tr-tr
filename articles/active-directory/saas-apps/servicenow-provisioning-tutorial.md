---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için ServiceNow 'ı yapılandırma | Microsoft Docs"
description: Azure AD 'den ServiceNow 'a Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4d6f06dd-a798-4c22-b84f-8a11f1b8592a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2019
ms.author: jeedes
ms.openlocfilehash: da62efff5db5c71b087657b0eec93f8dd4702665
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84751494"
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için ServiceNow 'ı yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem ServiceNow hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti 'ni kullanarak [ServiceNow](https://www.servicenow.com/) için kullanıcıları ve grupları otomatik olarak hazırlar ve serbest hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * ServiceNow 'da Kullanıcı oluşturma
> * ServiceNow 'daki kullanıcıları artık erişim gerektirdiklerinde kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile ServiceNow arasında eşitlenmiş olmasını sağlama
> * ServiceNow 'da grupları ve grup üyeliklerini sağlama
> * ServiceNow ['Da çoklu oturum açma](servicenow-tutorial.md) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Calgary veya üzeri bir [ServiceNow örneği](https://www.servicenow.com/)
* Helsinki veya üzeri bir [ServiceNow Express örneği](https://www.servicenow.com/)
* Yönetim rolüyle ServiceNow 'da bir kullanıcı hesabı

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD ve ServiceNow arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-servicenow-to-support-provisioning-with-azure-ad"></a>Adım 2. ServiceNow 'ı Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

1. ServiceNow örnek adınızı belirler. Örnek adını ServiceNow 'e erişmek için kullandığınız URL 'de bulabilirsiniz. Aşağıdaki örnekte, örnek adı dev35214 ' dir.

   ![ServiceNow örneği](media/servicenow-provisioning-tutorial/servicenow_instance.png)

2. ServiceNow 'da yönetici için kimlik bilgilerini alın. ServiceNow içindeki kullanıcı profiline gidin ve kullanıcının yönetici rolüne sahip olduğunu doğrulayın. 

   ![ServiceNow yönetici rolü](media/servicenow-provisioning-tutorial/servicenow-admin-role.png)

3. ServiceNow 'da aşağıdaki ayarların **devre dışı bırakıldığından** emin olmak için denetleyin:

   1. **Sistem güvenliği**  >  **yüksek güvenlik ayarları**' nı seçin,  >  **gelen şema istekleri için temel kimlik doğrulaması gerektirir**.
   2. **Sistem özelliklerini**seçin  >  **Web Hizmetleri**  >  **gelen SOAP istekleri için temel yetkilendirme gerektir**.
     
   > [!IMPORTANT]
   > Bu ayar *etkinleştirilirse*, sağlama altyapısı ServiceNow ile iletişim kuramayacaktır.

## <a name="step-3-add-servicenow-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden ServiceNow ekleme

ServiceNow 'ı Azure AD Uygulama Galerisi ' ne ekleyerek ServiceNow 'a sağlamayı yönetmeye başlayın. Önceden SSO için ServiceNow kurulumunu yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* ServiceNow 'a Kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-servicenow"></a>5. Adım. ServiceNow 'a otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-servicenow-in-azure-ad"></a>Azure AD 'de ServiceNow için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **ServiceNow**' ı seçin.

    ![Uygulamalar listesindeki ServiceNow bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde ServiceNow yönetici kimlik bilgilerinizi ve Kullanıcı adınızı girin. Azure AD 'nin ServiceNow 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, ServiceNow hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![alınıyor](./media/servicenow-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**'i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları ServiceNow**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den ServiceNow 'a eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için ServiceNow içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz ServiceNow API 'sinin, bu özniteliğe göre kullanıcıların filtrelemesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

10. **Eşlemeler** bölümünde, **Azure Active Directory grupları ServiceNow**' a eşitler ' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den ServiceNow 'a eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, ServiceNow güncelleştirme işlemlerinde grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. ServiceNow için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsamda** Istenen değerleri seçerek ServiceNow 'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.  

## <a name="troubleshooting-tips"></a>Sorun Giderme İpuçları
* **Invalidlookupreference:** ServiceNow içindeki bölüm ve konum gibi belirli öznitelikleri sağlarken, bu değerler ServiceNow içindeki bir başvuru tablosunda zaten mevcut olmalıdır. Örneğin, ServiceNow 'daki **tablo adı ekleme** tablosunda iki konumunuz (Seattle, Los Angeles) ve üç departman (Sales, finans, pazarlama) olabilir. Departmanı "Sales" ve Location 'ın "Seattle" olduğu bir Kullanıcı sağlamaya çalışırsanız, başarılı bir şekilde hazırlanacaktır. "Sales" departmanı ve "LA" konumuyla bir Kullanıcı sağlamaya çalışırsanız Kullanıcı sağlanmaz. Konum, ServiceNow 'daki başvuru tablosuna eklenmelidir veya Azure AD 'deki User özniteliği ServiceNow 'ın biçimiyle eşleşecek şekilde güncellenmelidir. 
* **Entryjoiningpropertyvalueısmissing:** Eşleşen özniteliği tanımlamak için [öznitelik eşlemelerinizi](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) gözden geçirin. Sağlamaya çalıştığınız kullanıcı veya grupta bu değer bulunmalıdır. 
* Tüm gereksinimleri veya sınırlamaları anlamak için [ServiceNow soap API](https://docs.servicenow.com/bundle/newyork-application-development/page/integrate/web-services-apis/reference/r_DirectWebServiceAPIFunctions.html) 'sini gözden geçirin (örneğin, bir kullanıcı için ülke kodunu belirtme biçimi)
* Sağlama istekleri varsayılan olarak https://{-örnek-adı}. Service-Now. com/{Table-Name} olarak gönderilir. Özel bir kiracı URL 'SI gerekiyorsa, örnek adı alanında tüm URL 'YI sağlayabilirsiniz.
* **ServiceNowInstanceInvalid** 
  
  `Details: Your ServiceNow instance name appears to be invalid.  Please provide a current ServiceNow administrative user name and          password along with the name of a valid ServiceNow instance.`                                                              

   Bu hata ServiceNow örneğiyle iletişim kuran bir sorunu gösterir. ServiceNow 'da aşağıdaki ayarların *devre dışı bırakıldığından* emin olmak için iki kez kontrol edin:
   
   1. **Sistem güvenliği**  >  **yüksek güvenlik ayarları**' nı seçin,  >  **gelen şema istekleri için temel kimlik doğrulaması gerektirir**.
   2. **Sistem özelliklerini**seçin  >  **Web Hizmetleri**  >  **gelen SOAP istekleri için temel yetkilendirme gerektir**.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../app-provisioning/check-status-user-account-provisioning.md)
