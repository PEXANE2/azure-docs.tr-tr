---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için ServiceNow'u yapılandırın | Microsoft Dokümanlar"
description: Azure AD'den ServiceNow'a kullanıcı hesaplarını otomatik olarak nasıl sağlayıp geçici olarak sağdan çıkarılamayı öğrenin.
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
ms.openlocfilehash: 9e93d4b3f1880f2ac56a32a7b85aa6801fb7c14e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78205150"
---
# <a name="tutorial-configure-servicenow-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için ServiceNow'u yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yı yapılandırmak için hem ServiceNow hem de Azure Etkin Dizini'nde (Azure AD) gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure REKLAM Sağlama hizmetini kullanarak kullanıcıları ve grupları otomatik olarak [ServiceNow'a](https://www.servicenow.com/) karşı hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../app-provisioning/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen Yetenekler
> [!div class="checklist"]
> * ServiceNow'da kullanıcı oluşturma
> * Artık erişim gerektirmediklerinde ServiceNow'daki kullanıcıları kaldırma
> * Kullanıcı özniteliklerini Azure AD ve ServiceNow arasında eşitlenmiş tutun
> * ServiceNow'da sağlama grupları ve grup üyelikleri
> * ServiceNow için [tek oturum açma](servicenow-tutorial.md) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* Calgary veya daha yüksek bir [ServiceNow örneği](https://www.servicenow.com/)
* Helsinki veya daha yüksek bir [ServiceNow Express örneği](https://www.servicenow.com/)
* Yönetici rolüyle ServiceNow'da bir kullanıcı hesabı

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD [ile ServiceNow arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlenere verilen leri belirleyin. 

## <a name="step-2-configure-servicenow-to-support-provisioning-with-azure-ad"></a>2. Adım Azure AD ile sağlamayı desteklemek için ServiceNow'u yapılandırın

1. ServiceNow örnek adınızı tanımlayın. ServiceNow'a erişmek için kullandığınız URL'de örnek adını bulabilirsiniz. Aşağıdaki örneklemde, örnek adı dev35214'dür.

![ServiceNow Örneği](media/servicenow-provisioning-tutorial/servicenow_instance.png)

    
2. ServiceNow'da bir yöneticinin kimlik bilgilerini edinin. ServiceNow'daki kullanıcı profiline gidin ve kullanıcının yönetici rolüne sahip olduğunu doğrulayın. 

![ServiceNow yönetici rolü](media/servicenow-provisioning-tutorial/servicenow-admin-role.png)

## <a name="step-3-add-servicenow-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden ServiceNow ekleme

ServiceNow'a sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden ServiceNow ekleyin. SSO için serviceNow'u daha önce kurulumunuz varsa, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları ServiceNow'a atarken Varsayılan **Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-servicenow"></a>5. Adım. Otomatik kullanıcı sağlamayı ServiceNow olarak yapılandırın 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-servicenow-in-azure-ad"></a>Azure AD'de ServiceNow için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **ServiceNow'u**seçin.

    ![Uygulamalar listesindeki ServiceNow bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümüne ServiceNow yönetici kimlik bilgilerinizi ve kullanıcı adınızı girin. Azure AD'nin ServiceNow'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, ServiceNow hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/servicenow-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını ServiceNow'a Senkronize Et'i**seçin.

9. Azure AD'den ServiceNow'a senkronize edilen kullanıcı özniteliklerini **Öznitelik-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için ServiceNow'daki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, ServiceNow API'sının bu özniteliğe bağlı olarak kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını ServiceNow'a Senkronize Et'i**seçin.

11. Azure AD'den ServiceNow'a eşitlenen grup özniteliklerini **Öznitelik-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için ServiceNow'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. ServiceNow için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek ServiceNow'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

15. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve grupların ilk eşitleme döngüsünü başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki döngülere göre ilk çevrimin gerçekleşmesi daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarılı veya başarısız bir şekilde sağlandığını belirlemek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanma
2. Sağlama döngüsünün durumunu ve tamamlanmasına ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) kontrol edin
3. Sağlama yapılandırması sağlıksız bir durumda gibi görünüyorsa, uygulama karantinaya alınır. Karantina durumları hakkında daha fazla bilgi [için burada.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)  

## <a name="troubleshooting-tips"></a>Sorun Giderme İpuçları
* **GeçersizLookupReference:** ServiceNow'da Departman ve Konum gibi belirli öznitelikleri sağlarken, değerlerin ServiceNow'daki bir başvuru tablosunda zaten bulunması gerekir. Örneğin, ServiceNow'daki **tablo adı** ekleme tablosunda iki konuma (Seattle, Los Angeles) ve üç departman (Satış, Finans, Pazarlama) olabilir. Bir kullanıcıya departmanının "Satış" ve konumu "Seattle" olan bir kullanıcıyı sağlamagirişiminde bulunmayı denerseniz, başarılı bir şekilde sağlanacaktır. Bir kullanıcıya "Satış" departmanı ve konumu "LA" ile birlikte sağlama girişiminde bulunmazsanız, kullanıcı sağlanmış olmaz. LA konumu ServiceNow'daki başvuru tablosuna eklenmelidir veya Azure AD'deki kullanıcı özniteliği ServiceNow'daki biçimle eşleşecek şekilde güncelleştirilmelidir. 
* **EntryjoiningPropertyValueEksik:** Eşleşen özniteliği tanımlamak için [öznitelik eşlemelerinizi](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) gözden geçirin. Bu değer, sağlamagirişiminde bulunduğunuz kullanıcı veya grupta bulunmalıdır. 
* Gereksinimleri veya sınırlamaları anlamak için [ServiceNow SOAP API'yi](https://docs.servicenow.com/bundle/newyork-application-development/page/integrate/web-services-apis/reference/r_DirectWebServiceAPIFunctions.html) gözden geçirin (örneğin, kullanıcı için ülke kodunu belirtmek için biçim)
* Bazı ServiceNow dağıtımları, Azure AD sağlama hizmeti için IZIN VERILEN IP aralıkları gerektirir. Azure AD sağlama hizmeti için ayrılmış IP aralıklarını [burada](https://www.microsoft.com/download/details.aspx?id=56519) "AzureActiveDirectoryDomainServices" altında bulabilirsiniz.
* Sağlama istekleri varsayılan olarak https://{your-instance-name}.service-now.com/{table-name} adresine gönderilir. Özel bir kiracı URL'si gerekiyorsa, örnek ad alanında tüm URL'yi sağlayabilirsiniz.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../app-provisioning/check-status-user-account-provisioning.md)
