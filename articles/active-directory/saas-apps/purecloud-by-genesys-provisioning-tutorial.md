---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için PureCloud'u Genesys tarafından yapılandırın | Microsoft Dokümanlar"
description: Genesys tarafından Azure AD'den PureCloud'a kullanıcı hesaplarını otomatik olarak nasıl sağlayıp yok edebilirsiniz öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 5f04b88b-117e-40da-a15c-e3732b240d5d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2020
ms.author: Zhchia
ms.openlocfilehash: 119690b9046821ab538d879e1209c6ef77277370
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77370673"
---
# <a name="tutorial-configure-purecloud-by-genesys-for-automatic-user-provisioning"></a>Öğretici: PureCloud'u Genesys tarafından otomatik kullanıcı sağlama için yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için Genesys ve Azure Active Directory (Azure AD) tarafından purecloud gerçekleştirmek için gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure AD Sağlama hizmetini kullanarak kullanıcıları ve grupları [Genesys tarafından PureCloud'a](https://www.genesys.com) otomatik olarak hükümler ve hükümlerden arındırMa sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Genesys tarafından PureCloud'da kullanıcı oluşturma
> * Artık erişim gerektirmediklerinde Genesys tarafından PureCloud'daki kullanıcıları kaldırın
> * Kullanıcı özniteliklerini Genesys tarafından Azure AD ve PureCloud arasında senkronize tutma
> * Genesys tarafından PureCloud'da sağlama grupları ve grup üyelikleri
> * Genesys tarafından PureCloud'a [tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial) açma (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* Bir PureCloud [organizasyonu.](https://help.mypurecloud.com/?p=81984)
* Oauth İstemci oluşturma [izniolan](https://help.mypurecloud.com/?p=24360) bir Kullanıcı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. [Genesys tarafından Azure AD ve PureCloud arasında hangi](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)verilerin haritalanması gerektiğini belirleyin. 

## <a name="step-2-configure-purecloud-by-genesys-to-support-provisioning-with-azure-ad"></a>2. Adım PureCloud'u Genesys tarafından Azure AD ile sağlamayı destekleyecek şekilde yapılandırın

1. PureCloud kuruluşunuzda yapılandırılan bir [Oauth İstemcisi](https://help.mypurecloud.com/?p=188023) oluşturun.
2. [Oauth istemcinizle](https://developer.mypurecloud.com/api/rest/authorization/use-client-credentials.html)bir belirteç oluşturun.
3. PureCloud içinde Grup üyeliğini otomatik olarak sağlamak istiyorsanız, PureCloud'da Azure AD'deki grupla aynı ada sahip [Gruplar oluşturmanız](https://help.mypurecloud.com/?p=52397) gerekir.

## <a name="step-3-add-purecloud-by-genesys-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Genesys tarafından PureCloud ekleyin

Genesys tarafından PureCloud'a sağlama yı yönetmeye başlamak için Azure AD uygulama galerisinden Genesys tarafından PureCloud ekleyin. PureCloud'u Genesys tarafından SSO için daha önce kurduksanız, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Genesys tarafından purecloud kullanıcıları ve grupları atarken, **Varsayılan Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-purecloud-by-genesys"></a>5. Adım. Genesys tarafından PureCloud'a otomatik kullanıcı sağlama yı yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-purecloud-by-genesys-in-azure-ad"></a>Azure AD'de PureCloud için otomatik kullanıcı sağlamayı Genesys tarafından yapılandırmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Genesys tarafından PureCloud'u**seçin.

    ![Genesys tarafından PureCloud Uygulamalar listesinde bağlantı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü altında PureCloud'unuzu Genesys API URL'si ve Oauth Token'ı **sırasıyla Kiracı URL'sine** ve Gizli **Belirteç** alanlarına girdi. API URL' si `{{API Url}}/api/v2/scim/v2` [PureCloud Geliştirici Merkezi'](https://developer.mypurecloud.com/api/rest/index.html)nden PureCloud bölgeniz için API URL'si kullanılarak yapılandırılacaktır. Azure AD'nin Genesys tarafından PureCloud'a bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Genesys tarafından PureCloud hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/purecloud-by-genesys-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Azure Active Directory Kullanıcılarını Genesys tarafından PureCloud'a Senkronize et seçeneğini belirleyin.**

9. Azure AD'den PureCloud'a, Genesys tarafından Geliştirilen Kullanıcı Özniteliklerini **Atföz-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Genesys tarafından PureCloud'daki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Genesys API tarafından PureCloud bu özniteliğe göre kullanıcıları filtreleme destekler emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

     |Öznitelik|Tür|
     |---|---|
     |userName|Dize|
     |Etkin|Boole|
     |displayName|Dize|
     |e-postalar[yazın eq "iş"].value|Dize|
     |başlık|Dize|
     |phoneNumbers[eq yazın "mobil"].value|Dize|
     |phoneNumbers[yazın eq "iş"].value|Dize|
     |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|Dize|
     |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Başvuru|

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Genesys'e göre PureCloud'a Senkronize et'i**seçin.

11. Azure AD'den PureCloud'a, Genesys tarafından Özellik **Eşleme** bölümünde senkronize edilen grup özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Genesys tarafından PureCloud'daki gruplarla eşleştirilmesi için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin. Genesys tarafından PureCloud grup oluşturma veya silme desteklemez ve yalnızca grupların güncelleştirilmesini destekler.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalId|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Genesys tarafından PureCloud için Azure AD sağlama hizmetini etkinleştirmek **için,** **Ayarlar** bölümünde Sağlama Durumunu **Açık** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek Genesys tarafından PureCloud'a sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

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
