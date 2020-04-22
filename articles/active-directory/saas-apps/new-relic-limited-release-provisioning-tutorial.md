---
title: 'Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Yeni Emanet(Sınırlı Sürüm) yapılandırın | Microsoft Dokümanlar'
description: Azure AD'den Yeni Emanet'e (Sınırlı Sürüm) otomatik olarak nasıl sağlayıp sağlayacağınızı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 7fd0d976-4f70-4ce5-992e-3ea4ed8e5d60
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2020
ms.author: Zhchia
ms.openlocfilehash: 363b50a4835ead52b3a3a85978d4ea8210add7b2
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727352"
---
# <a name="tutorial-configure-new-reliclimited-release-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Yeni Emanet(Sınırlı Sürüm) yapılandırın

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için hem Yeni Emanet(Sınırlı Sürüm) hem de Azure Etkin Dizin (Azure AD) işlemlerinde gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure AD Sağlama hizmetini kullanarak kullanıcıları ve grupları Otomatik olarak [Yeni Emanet'e](https://newrelic.com/) karşı hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz. 


## <a name="capabilities-supported"></a>Desteklenen Yetenekler
> [!div class="checklist"]
> * Yeni Emanet(Sınırlı Sürüm) kullanıcıları oluşturun
> * Artık erişim gerektirmediklerinde Yeni Emanet(Sınırlı Sürüm) kullanıcıları kaldırın
> * Kullanıcı özniteliklerini Azure AD ve Yeni Emanet (Sınırlı Sürüm) arasında eşitlenmiş tutun
> * Yeni Emanet(Sınırlı Sürüm)'de provizyon grupları ve grup üyelikleri
> * Yeni Emanet(Sınırlı Sürüm) için [tek oturum](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial) açma (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici). 
* Kullanıcılarınızın erişebetmesini istediğiniz Yeni Emanet(Sınırlı Sürüm) adresindeki bir veya daha fazla hesap. 

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Azure AD [ile Yeni Emanet (Sınırlı Sürüm) arasında hangi verilerin eşleneğe](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)verilebiyi belirleyin. 

## <a name="step-2-configure-new-reliclimited-release-to-support-provisioning-with-azure-ad"></a>2. Adım Azure AD ile sağlamayı destekleyecek Yeni Emanet(Sınırlı Sürüm) yapılandırın

Kuruluşunuz için SCIM ve SSO yapılandırmak için hesap temsilcinizle çalışın veya support.newrelic.com'da destek alın. Hesap temsilcinize şunları sağlamanız gerekir:

- Kuruluş adınız
- Kuruluşla ilişkilendirilen Yeni Emanet hesabı i

Bu bilgilerle, hesap temsilciniz yeni sistemimizde sizin için bir kuruluş kaydı oluşturur ve hesaplarınızı kuruluşla ilişkilendirer.

Hesap temsilciniz, kimlik sağlayıcınız için Yeni Emanet SCIM/SSO uygulamasını yapılandırmanız için gereken aşağıdaki bilgileri sağlar:

- SCIM Endpoint (Kiracı URL'si)
- SCIM taşıyıcı belirteci (Gizli Belirteç)

SCIM taşıyıcı belirteci, Yeni Emanet'te kullanıcılarınızın sağlanmasına izin verir, bu nedenle lütfen değeri güvende tutun. Hesap temsilciniz SCIM taşıyıcı belirtecinin size güvenli bir şekilde aktarılmasını sağlar.

## <a name="step-3-add-new-reliclimited-release-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Yeni Emanet (Sınırlı Sürüm) ekleme

Yeni Emanet'e (Sınırlı Sürüm) sağlama yı yönetmeye başlamak için Azure AD uygulama galerisinden Yeni Emanet (Sınırlı Sürüm) ekleyin. SSO için daha önce Yeni Emanet(Sınırlı Sürüm) kurduysanız, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Yeni Emanet'e (Sınırlı Sürüm) atarken, **Varsayılan Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-new-reliclimited-release"></a>5. Adım. Otomatik kullanıcı sağlamayı Yeni Emanet(Sınırlı Sürüm) olarak yapılandırın 

Bu bölüm, Azure AD'deki kullanıcı ve/veya grup atamalarına dayalı olarak TestApp'teki kullanıcıları ve/veya grupları oluşturmak, güncellemek ve devre dışı etmek için Azure AD sağlama hizmetini yapılandırma adımları boyunca size yol göstermektedir.

### <a name="to-configure-automatic-user-provisioning-for-new-reliclimited-release-in-azure-ad"></a>Azure AD'de Yeni Emanet (Sınırlı Sürüm) için otomatik kullanıcı sağlama yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, Yeni **Emanet(Sınırlı Sürüm)** seçeneğini belirleyin.

    ![Uygulamalar listesindeki Yeni Emanet bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümü `https://scim-provisioning.service.newrelic.com/scim/v2` altında, Kiracı URL'sindeki girişi. **Gizli Belirteç'te**daha önce alınan SCIM kimlik doğrulama belirteci değerini girdi. Azure AD'nin Yeni Emanet'e bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Yeni Emanet hesabınızda yönetici izinleri olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/new-relic-limited-release-provisioning-tutorial/provisioning.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Yeni Emanet (Sınırlı Sürüm)** ile Senkronize Et'i seçin.

9. Azure AD'den Yeni Emanet'e (Sınırlı Sürüm) senkronize edilen kullanıcı özniteliklerini **Atfetme-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Yeni Emanet(Sınırlı Sürüm) kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Yeni Emanet(Sınırlı Sürüm) API'sinin bu özniteliğe göre kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |externalId|Dize|
   |Etkin|Boole|
   |e-postalar[yazın eq "iş"].value|Dize|
   |name.givenName|Dize|
   |name.formatlı|Dize|
   |Zaman dilim|Dize|

10. **Eşlemeler** bölümünde, **Azure Etkin Dizin Gruplarını Yeni Emanet (Sınırlı Sürüm)** olarak Senkronize Et'i seçin.

11. Azure AD'den Yeni Emanet'e (Sınırlı Sürüm) senkronize edilen grup özniteliklerini **Atfetme-Eşleme** bölümünde gözden geçirin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleştirme işlemleri için Yeni Emanet(Sınırlı Sürüm) gruplarını eşleştirmek için kullanılır. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalId|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak [için, Kapsam](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

13. Yeni Emanet (Sınırlı Sürüm) için Azure AD sağlama hizmetini etkinleştirmek için, Ayarlar bölümünde **KiSama Durumunu** **Ayarı** olarak değiştirin. **Settings**

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **Kapsam'ta** istenen değerleri seçerek Yeni Emanet'e (Sınırlı Sürüm) sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

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
