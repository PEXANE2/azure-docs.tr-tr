---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için SolarWinds Service Desk (daha önce Samanage) yapılandırma | Microsoft Docs'
description: Azure AD 'den SolarWinds Service Desk 'e (daha önce Samanage) Kullanıcı hesaplarını otomatik olarak sağlamayı ve devre dışı bırakmayı öğrenin.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 01/13/2020
ms.author: Zhchia
ms.openlocfilehash: 0941c95ee6215a710ebb1bbc2fba9fae09ccf16d
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707275"
---
# <a name="tutorial-configure-solarwinds-service-desk-previously-samanage-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlama için SolarWinds Service Desk (daha önce Samanage) yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem SolarWinds Service Desk (daha önce Samanage) hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti 'ni kullanarak kullanıcıları ve grupları [Solarwinds Service Desk](https://www.samanage.com/pricing/) 'e otomatik olarak sağlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md).

## <a name="migrate-to-the-new-solarwinds-service-desk-application"></a>Yeni SolarWinds Service Desk uygulamasına geçiş

SolarWinds Service Desk ile mevcut bir tümleştirmeye sahipseniz, yaklaşan değişiklikler hakkında aşağıdaki bölüme bakın. SolarWinds Service Desk 'i ilk kez ayarlıyorsanız, bu bölümü atlayıp **desteklenen yetenekler**' e geçebilirsiniz.

#### <a name="whats-changing"></a>Ne değişiyor?

* Azure AD tarafındaki değişiklikler: Samange 'de kullanıcıları sağlamak için yetkilendirme yöntemi geçmişte **temel kimlik doğrulaması**yapıldı. Yakında, yetkilendirme yönteminin **uzun süreli gizli belirteç**olarak değiştirildiğini görürsünüz.


#### <a name="what-do-i-need-to-do-to-migrate-my-existing-custom-integration-to-the-new-application"></a>Mevcut özel tümleştirmem yeni uygulamaya geçirmek için ne yapmam gerekir?

Geçerli yönetici kimlik bilgileriyle mevcut bir SolarWinds Service Desk tümleştirmesi varsa, herhangi bir **eylem gerekmez**. Müşterileri otomatik olarak yeni uygulamaya geçiririz. Bu işlem arka planda tamamen yapılır. Mevcut kimlik bilgilerinin süreleri dolarsa veya uygulamaya yeniden erişim yetkisi vermeniz gerekiyorsa, uzun süreli bir gizli belirteç oluşturmanız gerekir. Yeni bir belirteç oluşturmak için bu makalenin 2. adımına bakın.


#### <a name="how-can-i-tell-if-my-application-has-been-migrated"></a>Uygulamamın geçirilip geçirilmeyeceğini nasıl anlayabilirim? 

Uygulamanız geçirildiğinde, yönetici **kimlik bilgileri** bölümünde **Yönetici Kullanıcı adı** ve **yönetici parolası** alanları tek bir **gizli dizi belirteci** alanıyla birlikte değişir.

## <a name="capabilities-supported"></a>Desteklenen yetenekler

> [!div class="checklist"]
> * SolarWinds Service Desk 'te Kullanıcı oluşturma
> * Artık erişim gerektirmeyen SolarWinds Service Desk içindeki kullanıcıları kaldırma
> * Azure AD ve SolarWinds Service Desk arasında kullanıcı özniteliklerinin eşitlenmiş olmasını sağlama
> * SolarWinds Service Desk 'te grupları ve grup üyeliklerini sağlama
> * SolarWinds Service Desk ['Te çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/samanage-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Profesyonel paketi olan bir [Solarwinds Service Desk kiracısı](https://www.samanage.com/pricing/) .
* Yönetici izinlerine sahip SolarWinds Service Desk içindeki bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD ve Solarwinds Service Desk arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-solarwinds-service-desk-to-support-provisioning-with-azure-ad"></a>Adım 2. SolarWinds Service Desk 'i Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

Kimlik doğrulaması için bir gizli belirteç oluşturmak için bkz. [API Tümleştirmesi Için öğretici belirteçleri kimlik doğrulaması](https://help.samanage.com/s/article/Tutorial-Tokens-Authentication-for-API-Integration-1536721557657).

## <a name="step-3-add-solarwinds-service-desk-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden SolarWinds Service Desk ekleme

SolarWinds Service Desk 'i Azure AD uygulama galerisinden ekleyerek SolarWinds Service Desk 'e sağlamayı yönetmeye başlayın. Daha önce SSO için SolarWinds Service Desk 'i ayarlarsanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları SolarWinds Service Desk 'e atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-solarwinds-service-desk"></a>5. Adım. SolarWinds Service Desk 'e otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-solarwinds-service-desk-in-azure-ad"></a>Azure AD 'de SolarWinds Service Desk için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Solarwinds Service Desk**' i seçin.

3. **Sağlama** sekmesini seçin.

    ![Hazırlama sekmesinin seçili olduğunu gösteren ekran görüntüsü.](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama modunun otomatik olarak ayarlandığını gösteren ekran görüntüsü.](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, `https://api.samanage.com` **kiracı URL 'sini**girin.  Gizli belirteç değerini **gizli belirteçte**daha önce alındı olarak girin. Azure AD 'nin SolarWinds Service Desk 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' yı seçin. Bağlantı başarısız olursa, SolarWinds Service Desk hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Bağlantıyı Sına düğmesinin seçili olduğunu gösteren ekran görüntüsü.](./media/samanage-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Solarwinds hizmet masası**' na eşitler ' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Solarwinds Service Desk 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Solarwinds Service Desk içindeki kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, Solarwinds Service Desk API 'sinin kullanıcıları bu özniteliğe göre filtrelemeyi desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      ![Samange Kullanıcı eşlemeleri](./media/samanage-provisioning-tutorial/user-attributes.png)

10. **Eşlemeler** bölümünde **Azure Active Directory grupları Solarwinds Service Desk olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den Solarwinds Service Desk 'e eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için Solarwinds Service Desk içindeki gruplarla eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      ![Grup eşlemelerini samange](./media/samanage-provisioning-tutorial/group-attributes.png)

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. SolarWinds Service Desk için Azure AD sağlama hizmeti 'ni etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** Içinde Istenen değerleri seçerek Solarwinds Service Desk 'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' i seçin.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

**Tüm kullanıcıları ve grupları Eşitle** seçeneğini belirleyin ve Solarwinds Service Desk **Roles** özniteliği için bir değer yapılandırırsanız, **null (isteğe bağlıdır) kutusunda varsayılan değer** altındaki değer aşağıdaki biçimde belirtilmelidir:

- {"displayName": "role"}, burada rol istediğiniz varsayılan değerdir.

## <a name="change-log"></a>Değişiklik günlüğü

* 09/14/2020-her iki SaaS öğreticilerinde şirket adı ' nı SolarWinds Service Desk 'e (daha önce Sayönet) göre değiştirilmiştir https://github.com/ravitmorales .
* 04/22/2020-yetkilendirme yöntemi temel kimlik doğrulaması 'ndan uzun süreli gizli belirtece güncelleştirildi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
