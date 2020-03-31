---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Azure Databricks SCIM Bağlayıcısı'nı yapılandırın | Microsoft Dokümanlar"
description: Azure AD'den Azure Databricks SCIM Connector'a kullanıcı hesaplarını otomatik olarak nasıl sağlayıp yok edebilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: b16eaf27-4bd1-4509-be2c-9a4f66b6badc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2020
ms.author: Zhchia
ms.openlocfilehash: fe1260982edc877c049716bd74f1bb3e90d33b0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77370528"
---
# <a name="tutorial-configure-azure-databricks-scim-connector-for-automatic-user-provisioning"></a>Öğretici: Azure Databricks SCIM Bağlayıcısı'nı otomatik kullanıcı sağlama için yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için hem Azure Databricks SCIM Bağlayıcısı'nda hem de Azure Active Directory'de (Azure AD) gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure REKLAM Sağlama hizmetini kullanarak kullanıcıları ve grupları [Azure Databricks SCIM Connector'a](https://databricks.com/) otomatik olarak hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Azure Databricks SCIM Bağlayıcısı'nda kullanıcı oluşturma
> * Artık erişim egerekmediklerinde Azure Databricks SCIM Connector'daki kullanıcıları kaldırın
> * Kullanıcı özniteliklerini Azure AD ve Azure Databricks SCIM Bağlayıcısı arasında senkronize tutma
> * Azure Databricks SCIM Bağlayıcısı'ndaki sağlama grupları ve grup üyelikleri

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* Yönetici izinlerine sahip bir Azure Databricks hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD [ile Azure Databricks SCIM Bağlayıcısı arasında hangi verilerin eşleneredeği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)belirlenip belirlenip belirleyin. 

## <a name="step-2-configure-azure-databricks-scim-connector-to-support-provisioning-with-azure-ad"></a>2. Adım Azure Databricks SCIM Connector'ı Azure AD ile sağlamayı destekleyecek şekilde yapılandırın

1. Azure Databricks SCIM sağlamasını ayarlamak için, azure etkin dizin kiracınıza kaynak olarak ekleyin ve aşağıdaki ayarları kullanarak yapılandırın.

    ![Azure Databricks kurulumu](./media/azure-databricks-scim-provisioning-connector-provisioning-tutorial/setup.png)

2. Azure Veri Tuğlaları'nda kişisel erişim belirteci oluşturmak için [buna](https://docs.microsoft.com/azure/databricks/dev-tools/api/latest/authentication#token-management)bakın.

3. **Jetonu**kopyala. Bu değer, Azure portalındaki Azure Databricks SCIM Bağlayıcısı uygulamanızın Sağlama sekmesinde Gizli Belirteç alanına girilir.

## <a name="step-3-add-azure-databricks-scim-connector-from-the-azure-ad-application-gallery"></a>3. Adım Azure REKLAM uygulama galerisinden Azure Databricks SCIM Bağlayıcısı ekleme

Azure Databricks SCIM Bağlayıcısı'na sağlama yı yönetmeye başlamak için Azure AD uygulama galerisinden Azure Databricks SCIM Bağlayıcısı ekleyin. SSO için azure databricks SCIM Bağlayıcısı'nı daha önce kurduksanız, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Azure Databricks SCIM Bağlayıcısı'na atarken, **Varsayılan Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-azure-databricks-scim-connector"></a>5. Adım. Otomatik kullanıcı sağlamayı Azure Databricks SCIM Bağlayıcısı olarak yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

> [!NOTE]
> Azure Databricks'in SCIM bitiş noktası hakkında daha fazla bilgi edinmek için [şuna](https://docs.databricks.com/dev-tools/api/latest/scim.html
)bakın.

### <a name="to-configure-automatic-user-provisioning-for-azure-databricks-scim-connector-in-azure-ad"></a>Azure AD'de Azure Databricks SCIM Bağlayıcısı için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Azure Databricks SCIM Bağlayıcısı'nı**seçin.

    ![Uygulamalar listesindeki Azure Databricks SCIM Bağlayıcısı bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümüne, **Kiracı URL'sinde**SCIM uç nokta değerini girdi. Kiracı URL'si, `https://<region>.azuredatabricks.net/api/2.0/preview/scim` azure veri tuğlaları ana sayfa URL'nizde **bölgenin** bulunabileceği biçimde olmalıdır. Örneğin, **Westus** bölgesi için bir SCIM `https://westus.azuredatabricks.net/api/2.0/preview/scim`bitiş noktası olacaktır. Gizli Belirteç'te daha önce alınan belirteç değerini **girdi.** Azure AD'nin Azure Databricks SCIM Bağlayıcısı'na bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Azure Databricks SCIM Bağlayıcı hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/azure-databricks-scim-provisioning-connector-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Azure Databricks SCIM Bağlayıcısı**ile Senkronize Et'i seçin.

9. Azure AD'den Azure Databricks SCIM Bağlayıcısı'na eşitlenen kullanıcı özniteliklerini **Atnitelik-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Azure Databricks SCIM Connector'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Azure Veri Tuğlaları SCIM Bağlayıcı API'sinin bu özniteliğe bağlı olarak kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |displayName|Dize|
   |Etkin|Boole|

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Azure Databricks SCIM Bağlayıcısı'na Senkronize Et'i**seçin.

11. Azure AD'den Azure Databricks SCIM Bağlayıcısı'na eşitlenen grup özniteliklerini **Atnitelik-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Azure Databricks SCIM Connector'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

     |Öznitelik|Tür|
     |---|---|
     |displayName|Dize|
     |üyeler|Başvuru|

11. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Azure Databricks SCIM Bağlayıcısı'na Senkronize Et'i**seçin.

12. Azure Databricks SCIM Bağlayıcısı için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

13. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Azure Databricks SCIM Bağlayıcısı'na sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

14. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve grupların ilk eşitleme döngüsünü başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki döngülere göre ilk çevrimin gerçekleşmesi daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hangi kullanıcıların başarılı veya başarısız bir şekilde sağlandığını belirlemek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanma
* Sağlama döngüsünün durumunu ve tamamlanmasına ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) kontrol edin
* Sağlama yapılandırması sağlıksız bir durumda gibi görünüyorsa, uygulama karantinaya alınır. Karantina durumları hakkında daha fazla bilgi [için burada.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)  

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
* Databricks, SCIM üzerinden gönderdiğimiz büyük harfe bakılmaksızın, kullanıcı adı değerlerini her zaman dizinlerine kaydederken kullanıcı adı değerlerini küçük harflere dönüştürür.
* Şu anda Kullanıcılar için Azure Databricks'in SCIM API'sına USER@contoso.com karşı istekler alın, bu nedenle user@contoso.combunu sorgularsak, bunu depoladıkları nda 0 sonuç elde edecektir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
