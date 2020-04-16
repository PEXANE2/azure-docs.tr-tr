---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Elçi'yi yapılandırın | Microsoft Dokümanlar"
description: Azure AD'den Elçi'ye kullanıcı hesaplarını otomatik olarak nasıl sağlayıp geçici olarak sağmayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/3/2019
ms.author: Zhchia
ms.openlocfilehash: 68e17ba1dd5981e565e56d6c8137f77d33ad755b
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393506"
---
# <a name="tutorial-configure-envoy-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Elçi'yi yapılandır

Bu öğretici, otomatik kullanıcı sağlamayı yapılandırmak için hem Elçi hem de Azure Etkin Dizin (Azure AD) işlemlerinde gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure REKLAM Sağlama hizmetini kullanarak kullanıcıları ve grupları [Elçi'ye](https://envoy.com/pricing/) otomatik olarak hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Elçi'de kullanıcı oluşturma
> * Artık erişim gerektirmediklerinde Elçi'deki kullanıcıları kaldırma
> * Kullanıcı özniteliklerini Azure AD ve Elçi arasında eşitlenmiş tutma
> * Elçi'de provizyon grupları ve grup üyelikleri
> * Elçiye [tek oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* [Bir Elçi kiracı](https://envoy.com/pricing/).
* Yönetici izinleri olan Elçi'deki bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD ile Elçi arasında hangi verilerin eşlenere verilen leri [belirleyin.](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) 

## <a name="step-2-configure-envoy-to-support-provisioning-with-azure-ad"></a>2. Adım Elçisi Azure AD ile sağlamayı destekleyecek şekilde yapılandırın

1. Elçi Yönetici [Konsolunuzda](https://dashboard.envoy.com/login)oturum açın. **Entegrasyonlar'a**tıklayın.

    ![Elçi Entegrasyonları](media/envoy-provisioning-tutorial/envoy01.png)

2. **Microsoft Azure SCIM tümleştirmesi**için **Yükle'yi** tıklatın.

    ![Elçi Yükleme](media/envoy-provisioning-tutorial/envoy02.png)

3. Tüm kullanıcıları **Eşitle**için **Kaydet'e** tıklayın. 

    ![Elçi Kaydet](media/envoy-provisioning-tutorial/envoy03.png)

4. **OAUTH TAŞıYıCı belirteci**kopyalayın. Bu değer, Azure portalındaki Elçi uygulamanızın sağlama sekmesinde **Gizli Belirteç** alanına girilir.
    
    ![Elçi OAUTH](media/envoy-provisioning-tutorial/envoy04.png)

## <a name="step-3-add-envoy-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Elçi Ekleme

Elçi'ye sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden Elçi ekleyin. Daha önce SSO elçisi kurulumu varsa, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Elçi'ye atarken Varsayılan **Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-envoy"></a>5. Adım. Elçi'ye otomatik kullanıcı sağlama yı yapılandırma 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-envoy-in-azure-ad"></a>Azure AD'deki Elçi için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Elçi'yi**seçin.

    ![Başvurular listesindeki Elçi bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://app.envoy.com/scim/v2` altında, **Kiracı URL'ye**giriş . **Gizli**Belirteç'te daha önce alınan **OAUTH BEARER TOKEN** değerini girdi. Azure AD'nin Elçi'ye bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Elçi hesabınızın Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

   ![Sağlama](./media/envoy-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Elçiye Senkronize Et'i**seçin.

9. Azure AD'den **Elçi'ye, Öznitelik-Eşleme** bölümünde senkronize edilen kullanıcı özniteliklerini gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Elçi'deki kullanıcı hesaplarıyla eşleşecek şekilde kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Elçi API'sinin bu özniteliğe göre kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |externalId|Dize|
   |displayName|Dize|
   |başlık|Dize|
   |e-postalar[yazın eq "iş"].value|Dize|
   |tercihDil|Dize|
   |bölüm|Dize|
   |adresleri[yazın eq "iş"].ülke|Dize|
   |adresleri[yazın eq "iş"].yerellik|Dize|
   |adresleri[yazın eq "iş"].region|Dize|
   |adresleri[yazın eq "iş"].postalCode|Dize|
   |adresleri[yazın eq "iş"].biçimlendirilmiş|Dize|
   |adresleri[yazın eq "iş"].streetAddress|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |name.formatlı|Dize|
   |phoneNumbers[eq yazın "mobil"].value|Dize|
   |phoneNumbers[yazın eq "iş"].value|Dize|
   |yerel ayar|Dize|

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Elçiye Senkronize**Et'i seçin.

11. Azure AD'den **Atrit-Eşleme** bölümünde Elçi'ye eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için Elçi'deki gruplarla eşleşecek şekilde kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalId|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Elçi için Azure AD sağlama hizmetini etkinleştirmek **için, Ayarlar** bölümünde **Sağlama Durumunu** **Ayarı** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek Elçi'ye sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

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