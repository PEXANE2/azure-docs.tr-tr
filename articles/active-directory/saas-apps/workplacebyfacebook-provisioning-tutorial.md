---
title: "Öğretici: Azure Active Directory ile otomatik kullanıcı sağlama için Facebook tarafından İşyeri'ni yapılandırın | Microsoft Dokümanlar"
description: Azure Active Directory ve İşyeri arasında Facebook tarafından tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22576be8dec021f0f18a6e2dda16891ce70d4f13
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77603215"
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>Öğretici: Otomatik kullanıcı sağlama için Facebook tarafından İşyeri yapılandırma

Bu öğretici, otomatik kullanıcı sağlama yapılandırmak için Facebook ve Azure Active Directory (Azure AD) tarafından hem Workplace'te gerçekleştirmeniz gereken adımları açıklar. Azure AD, yapılandırıldığınızda, Azure REKLAM Sağlama hizmetini kullanarak kullanıcıları ve grupları [Facebook tarafından Otomatik](https://work.workplace.com/) olarak Işyeri'ne karşı hükümler ve hükümlerden arındırma sağlar. Bu hizmetin ne yaptığı, nasıl çalıştığı ve sık sorulan sorular hakkında önemli ayrıntılar [için](../manage-apps/user-provisioning.md)bkz.

## <a name="migrating-to-the-new-workplace-by-facebook-application"></a>Facebook uygulaması ile yeni İşyerine geçiş
Facebook tarafından İşyeri ile mevcut bir entegrasyon varsa, lütfen gelen değişikliklerle ilgili aşağıdaki bölüme bakın. Facebook tarafından Workplace'i ilk kez kuruyorsanız bu bölümü atlayabilir ve desteklenen özelliklere geçebilirsiniz. 

#### <a name="whats-changing"></a>Ne değişiyor?
* Azure AD tarafındaki değişiklikler: İşyerine kullanıcılara sağlama yetkilendirme yöntemi, geçmişte uzun ömürlü bir gizli belirteç olmuştur. Yakında yetkilendirme yönteminin OAuth yetkilendirme hibesine göre değiştirilecektir. 
* İşyeri tarafındaki değişiklikler: Daha önce Azure AD uygulaması, Facebook tarafından İşyeri'nde özel bir tümleştirmeydi. Artık Azure AD'yi İşyeri tümleştirmeleri dizininde üçüncü taraf bir uygulama olarak görürsünüz. 

 

#### <a name="what-do-i-need-to-do-to-migrate-my-existing-custom-integration-to-the-new-application"></a>Varolan özel tümleştirmemi yeni uygulamaya geçirmek için ne yapmam gerekiyor?
Geçerli bir belirteç ile varolan bir İşyeri tümleştirmeniz varsa, herhangi bir **işlem gerekmez.** Her hafta otomatik olarak yeni uygulamaya müşteri göç ediyoruz. Bu tamamen perde arkasında yapılır. Bekleyemez ve yeni uygulamaya el ile taşımak istiyorsanız, galeriden Yeni Bir İşyeri örneği ekleyebilir ve hükmü yeniden yapılandırabilirsiniz. Workplace'in tüm yeni örnekleri otomatik olarak yeni uygulama sürümünü kullanacaktır. 

 
İşyeri entegrasyonunuz karantinadaysa, sizi göç etmemiz için geçerli bir jeton sağlamanız gerekir. Yönetici kimlik bilgileri bölümü soluk olacak, ancak aşağıdakileri ekleyebilirsiniz (**? kimlik**bilgilerini yeniden kaydetmek için URL'nize Microsoft_AAD_IAM_userProvisioningEnableCredentialsOverride=true). 

https://portal.azure.com/?Microsoft_AAD_IAM_userProvisioningEnableCredentialsOverride=true

#### <a name="how-can-i-tell-if-my-application-has-been-migrated"></a>Başvurumun geçirilip geçirilemediğini nasıl anlayabilirim? 
Başvurunuz geçirildiğinde, yükseltme değişiklikleriyle ilgili yetkilendirme bölümündeki başlık kaldırılır ve gizli belirteç alanı mavi bir yetkilendirme düğmesiyle değiştirilir. 

#### <a name="the-admin-credentials-section-is-greyed-out-on-my-application-and-i-cant-save-why"></a>Yönetici kimlik bilgileri bölümü başvurumda soluk ve kaydedemiyorum. Neden?
Mevcut İşyeri müşterileri için yönetici kimlik bilgileri bölümünü kilitledik. Kiracınız yeni İşyeri uygulamasına geçirildiğinde yönetici kimlik bilgilerini yeniden güncelleştirebilirsiniz. Bekleyemezseniz, uygulamanızı yeniden yapmak için yukarıdaki URL'yi kullanabilirsiniz. 

 
#### <a name="when-will-these-changes-happen"></a>Bu değişiklikler ne zaman olacak?
İşyeri tüm yeni örnekleri zaten yeni entegrasyon / yetkilendirme yöntemi ni kullanacaktır. Mevcut entegrasyonlar Mayıs'a kadar kademeli olarak geçirilecek. İşyeri ekibi, 28 Şubat'tan Mayıs-1'e kadar olan süreyi uzattı. 

## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Facebook tarafından İşyerinde kullanıcı oluşturma
> * Artık erişim egerekmediklerinde Facebook tarafından İşyeri'ndeki kullanıcıları kaldırın
> * Kullanıcı özniteliklerini Facebook tarafından Azure AD ve İşyeri arasında senkronize tutma
> * Facebook tarafından İşyeri için [tek oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/workplacebyfacebook-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki ön koşullara sahip olduğunuzu varsayar:

* [Azure AD kiracı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Sağlama yapılandırma [izniyle](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) Azure AD'deki bir kullanıcı hesabı (örn. Uygulama Yöneticisi, Bulut Uygulama yöneticisi, Uygulama Sahibi veya Genel Yönetici)
* Facebook tarafından bir İşyeri tek oturum etkin abonelik

> [!NOTE]
> Bu öğreticideki adımları sınamak için bir üretim ortamı kullanmanızı önermiyoruz.

Bu öğreticideki adımları sınamak için aşağıdaki önerileri izlemeniz gerekir:

- Gerekli olmadıkça üretim ortamınızı kullanmayın.
- Azure AD deneme ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık deneme sürümü alabilirsiniz.

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. Sağlama [hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Kimler in provizyon kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)alınacağını belirleyin.
3. Facebook tarafından Azure AD ve İşyeri arasında hangi verileri [eşlenemeye](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)karar verin.

## <a name="step-2-configure-workplace-by-facebook-to-support-provisioning-with-azure-ad"></a>2. Adım Azure AD ile sağlamayı destekleyecek Şekilde Facebook tarafından İşYeri Yapılandırma

Sağlama hizmetini yapılandırmadan ve etkinleştirmeden önce, Azure AD'deki kullanıcıların ve/veya grupların Facebook tarafından İşyeri'nize erişilmesi gereken kullanıcıları temsil ettiğine karar vermeniz gerekir. Karar verildikten sonra, bu kullanıcıları Facebook uygulaması ile İşyerinize aşağıdaki talimatları izleyerek atayabilirsiniz:

*   Sağlama yapılandırmasını sınamak için Facebook tarafından Workplace'e tek bir Azure AD kullanıcısı atanması önerilir. Ek kullanıcılar ve/veya gruplar daha sonra atanabilir.

*   Facebook tarafından Bir Kullanıcıyı İşyeri'ne atarken, geçerli bir kullanıcı rolü seçmeniz gerekir. "Varsayılan Erişim" rolü sağlama için çalışmaz.

## <a name="step-3-add-workplace-by-facebook-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Facebook'a göre İşyeri Ekleme

Facebook tarafından Workplace'e sağlamayı yönetmeye başlamak için Azure REKLAM uygulama galerisinden Facebook'a İşyeri ekleyin. Daha önce Facebook tarafından SSO için İşyeri kurulumu varsa, aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. [Burada](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)galeriden bir uygulama ekleme hakkında daha fazla bilgi edinin.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Tedarik kapsamına kimlerde olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya yapılan atamaya ve kullanıcının/ grubun özniteliklerine göre kimin sağlanacak kapsamını kapsamanızı sağlar. Atamaya göre uygulamanız için kimlerin sağlanacak kapsamını seçerseniz, uygulamayı zedelektirler ve kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca kullanıcı nın veya grubun özelliklerine göre kimlerin sağlanacak kapsamını seçerseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Facebook tarafından İşyeri'ne atarken, **Varsayılan Erişim**dışında bir rol seçmeniz gerekir. Varsayılan Erişim rolüne sahip kullanıcılar sağlama nın dışında tutulur ve sağlama günlüklerinde etkin bir şekilde hak sahibi olmadığı şeklinde işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolüyse, ek roller eklemek için [uygulama bildirimini](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) güncelleştirebilirsiniz. 

* Küçük başla. Herkese kullanıma başlamadan önce küçük bir kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanmış kullanıcılara ve gruplara ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu denetleyebilirsiniz. Kapsam tüm kullanıcılar ve gruplar için ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 

1. [Azure portalında](https://portal.azure.com)oturum açın. **Kurumsal Uygulamaları**seçin, ardından **Tüm uygulamaları**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, Facebook **tarafından İşyeri'ni**seçin.

    ![Uygulamalar listesinde Facebook bağlantısı ile İşyeri](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. Sağlama **Modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici **Kimlik Bilgileri** bölümünde, **Authorize'a**tıklayın. Facebook'un yetkilendirme sayfası tarafından İşyeri'ne yönlendirileceksiniz. İşyerinizi Facebook kullanıcı adına girin ve **Devam** düğmesine tıklayın. Azure AD'nin Facebook tarafından İşyeri'ne bağlanabilmesini sağlamak için **Test Bağlantısı'nı** tıklatın. Bağlantı başarısız olursa, Facebook hesabınızdaki İşyeri'nin Yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/workplacebyfacebook-provisioning-tutorial/provisioning.png)

    ![Yetkilendir](./media/workplacebyfacebook-provisioning-tutorial/workplacelogin.png)

6. Bildirim **E-postası** alanında, sağlama hatası bildirimleri alması gereken bir kişinin veya grubun e-posta adresini girin ve **bir hata olduğunda e-posta bildirimi gönder'i** seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet'i**seçin.

8. **Eşlemeler** bölümünde, **Azure Etkin Dizin Kullanıcılarını Facebook tarafından İşyerine Senkronize Etme'yi**seçin.

9. Azure AD'den Facebook tarafından Facebook tarafından İşyerine senkronize edilen kullanıcı özniteliklerini **Atföz-Eşleme** bölümünde inceleyin. **Eşleştirme** özellikleri olarak seçilen öznitelikler, güncelleme işlemleri için Facebook tarafından İşyeri'ndeki kullanıcı hesaplarıyla eşleştirilmesi için kullanılır. [Eşleşen hedef özniteliği](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Facebook API tarafından İşyeri'nin bu özniteliğe göre kullanıcıları filtrelemeyi desteklediğinden emin olmanız gerekir. Herhangi bir değişiklik yapmak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |displayName|Dize|
   |Etkin|Boole|
   |başlık|Boole|
   |e-postalar[yazın eq "iş"].value|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |name.formatlı|Dize|
   |adresleri[yazın eq "iş"].biçimlendirilmiş|Dize|
   |adresleri[yazın eq "iş"].streetAddress|Dize|
   |adresleri[yazın eq "iş"].yerellik|Dize|
   |adresleri[yazın eq "iş"].region|Dize|
   |adresleri[yazın eq "iş"].ülke|Dize|
   |adresleri[yazın eq "iş"].postalCode|Dize|
   |adresleri[yazın eq "diğer"].biçimlendirilmiş|Dize|
   |phoneNumbers[yazın eq "iş"].value|Dize|
   |phoneNumbers[eq yazın "mobil"].value|Dize|
   |phoneNumbers[eq yazın "faks"].value|Dize|
   |externalId|Dize|
   |tercihDil|Dize|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Dize|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|Dize|

10. Kapsam filtrelerini yapılandırmak [için, Kapsam](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)filtresi öğreticisinde sağlanan aşağıdaki yönergelere bakın.

11. Facebook tarafından İşyeri için Azure AD sağlama hizmetini etkinleştirmek için, **Ayarlar** bölümünde **KiSama Durumunu** **On** olarak değiştirin.

    ![Geçiş Yapılan Sağlama Durumu](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **Kapsam'da** istenen değerleri seçerek Facebook tarafından İşyeri'ne sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama Kapsamı](common/provisioning-scope.png)

13. Hükmetmeye hazır olduğunuzda **Kaydet'i**tıklatın.

    ![Tasarruf Sağlama Yapılandırması](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **Kapsam'ta** tanımlanan tüm kullanıcıların ve grupların ilk eşitleme döngüsünü başlatır. Azure AD sağlama hizmeti nin çalıştırıldığı sürece yaklaşık her 40 dakikada bir gerçekleşen sonraki döngülere göre ilk çevrimin gerçekleşmesi daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarılı veya başarısız bir şekilde sağlandığını belirlemek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanma
2. Sağlama döngüsünün durumunu ve tamamlanmasına ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) kontrol edin
3. Sağlama yapılandırması sağlıksız bir durumda gibi görünüyorsa, uygulama karantinaya alınır. Karantina durumları hakkında daha fazla bilgi [için burada.](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)

## <a name="troubleshooting-tips"></a>Sorun giderme ipuçları
*  Bir kullanıcının başarısız bir şekilde oluşturulduğunu görürseniz ve "1789003" kodlu bir denetim günlüğü olayı varsa, bu kullanıcının doğrulanmamış bir etki alanından geldiği anlamına gelir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı sağlamanın yönetimi](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri nasıl inceleyip sağlama etkinliği yle ilgili raporları nasıl alacağınızı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
