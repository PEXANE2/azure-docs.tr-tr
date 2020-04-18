---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Juno Journey'yi yapılandırın | Microsoft Dokümanlar"
description: Azure AD'den Juno Journey'e kullanıcı hesaplarını otomatik olarak nasıl sağlayıp geçici olarak sağdan çıkarılamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 79813b19-c96e-4459-a4e5-636a6b6f5041
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2020
ms.author: Zhchia
ms.openlocfilehash: 08de07a52d1e43dea91e6684d33027d8bcad61fc
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641855"
---
# <a name="tutorial-configure-juno-journey-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Juno Journey'yi yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için hem Juno Journey hem de Azure Etkin Dizin (Azure AD) işlemlerinde gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure AD Sağlama hizmetini kullanarak kullanıcıları ve grupları Otomatik olarak [Juno Journey'e](https://www.junojourney.com/) karşı hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Juno Journey'de kullanıcı oluşturun
> * Artık erişim gerektirmeyen kullanıcıları Juno Journey'de kaldırma
> * Kullanıcı özniteliklerini Azure AD ve Juno Journey arasında senkronize tutma
> * Juno Journey için [tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-tutorial) açma (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
*  Bir [Juno Journey kiracı](https://www.junojourney.com/getstartedwithjuno).
*  Yönetici izinleri ile Juno Journey bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD [ve Juno Journey arasında hangi](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)verilerin eşlenemeye devam edin. 

## <a name="step-2-configure-juno-journey-to-support-provisioning-with-azure-ad"></a>2. Adım Azure AD ile sağlamayı destekleyecek Şekilde Juno Journey'yi yapılandırın

1. **Gizli Belirteç** ve **Kiracı URL için** support@the-juno.comJuno Journey destek ekibi ile irtibata geçin. Bu değer, Azure portalındaki Juno Journey uygulamanızın Sağlama sekmesine sırasıyla **Gizli Belirteç** ve **Kiracı URL** alanlarına girilir. 

## <a name="step-3-add-juno-journey-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Juno Journey ekleme

Juno Journey'de sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden Juno Journey'yi ekleyin. Daha önce SSO için Juno Journey kurulumu varsa, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Juno Journey'e atarken Varsayılan **Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-juno-journey"></a>5. Adım. Otomatik kullanıcı sağlamayı Juno Journey'ye yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-juno-journey-in-azure-ad"></a>Azure AD'de Juno Journey için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Juno Journey'yi**seçin.

    ![Uygulamalar listesindeki Juno Journey bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü ne si bölümünde, daha önce **kiracı URL'sinde**alınan kiracı URL değerini girdin. Gizli Belirteç'te daha önce alınan gizli belirteç değerini **girdi.** Azure AD'nin Juno Journey'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Juno Journey hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/juno-journey-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Juno Journey'e Senkronize Et'i**seçin.

9. Azure AD'den Juno Journey'e eşitlenen kullanıcı özniteliklerini **Öznitelik-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Juno Journey'deki kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Juno Journey API'sinin bu özniteliğe göre kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

   |Değişken|Tür|
   |---|---|
   |userName|Dize|
   |externalId|Dize|
   |displayName|Dize|
   |başlık|Dize|
   |Etkin|Boole|
   |tercihDil|Dize|
   |e-postalar[yazın eq "iş"].value|Dize|
   |adresleri[yazın eq "iş"].ülke|Dize|
   |adresleri[yazın eq "iş"].region|Dize|
   |adresleri[yazın eq "iş"].yerellik|Dize|
   |adresleri[yazın eq "iş"].postalCode|Dize|
   |adresleri[yazın eq "iş"].biçimlendirilmiş|Dize|
   |adresleri[yazın eq "iş"].streetAddress|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |name.middleName|Dize|
   |name.formatlı|Dize|
   |phoneNumbers[eq yazın "faks"].value|Dize|
   |phoneNumbers[eq yazın "mobil"].value|Dize|
   |phoneNumbers[yazın eq "iş"].value|Dize|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|Dize|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|Dize|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|Dize|
   urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|Dize|
   urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Dize|
   urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|Dize|


10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Juno Journey için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde Sağlama Durumunu **Ayarı** olarak değiştirin. **Settings**

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Juno Journey'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

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
