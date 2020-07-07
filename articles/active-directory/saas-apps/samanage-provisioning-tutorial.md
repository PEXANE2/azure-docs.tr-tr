---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlama için Samanage 'ı yapılandırma | Microsoft Docs"
description: Azure AD 'den Samanage 'a Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 62d0392f-37d4-436e-9aff-22f4e5b83623
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2020
ms.author: Zhchia
ms.openlocfilehash: 182d314b24ce082d996cb692e2a7bb35265abcfe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82628085"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı hazırlama için Samanage 'ı yapılandırma
Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem Samanage hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, kullanıcıları ve grupları otomatik olarak sağlar ve Azure AD sağlama hizmetini kullanarak [Sayönetmek](https://www.samanage.com/pricing/) üzere hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md).

## <a name="migrate-to-the-new-samange-application"></a>Yeni Samange uygulamasına geçiş

Samanage ile mevcut bir tümleştirmenize sahipseniz lütfen sonraki değişiklikler hakkında bölümüne bakın. Samanage 'ı ilk kez ayarlıyorsanız, bu bölümü atlayabilir ve **desteklenen yeteneklere**geçebilirsiniz.

#### <a name="whats-changing"></a>Ne değişiyor?
* Azure AD tarafındaki değişiklikler: Samange 'de kullanıcıları sağlamak için yetkilendirme yöntemi geçmişte **temel kimlik doğrulaması**yapıldı. Yakında, yetkilendirme yönteminin **uzun süreli gizli belirteç**olarak değiştirildiğini görürsünüz.


#### <a name="what-do-i-need-to-do-to-migrate-my-existing-custom-integration-to-the-new-application"></a>Mevcut özel tümleştirmem yeni uygulamaya geçirmek için ne yapmam gerekir?
Geçerli yönetici kimlik bilgileriyle mevcut bir Samanage tümleştirmenize sahipseniz, **herhangi bir eylem gerekmez**. Müşterileri otomatik olarak yeni uygulamaya geçiririz. Bu işlem arka planda tamamen yapılır. Mevcut kimlik bilgilerinin süreleri dolarsa veya uygulamaya yeniden erişim yetkisi vermeniz gerekiyorsa, uzun süreli bir gizli belirteç oluşturmanız gerekir. Yeni bir belirteç oluşturmak için bu makalenin 2. adımına bakın.


#### <a name="how-can-i-tell-if-my-application-has-been-migrated"></a>Uygulamamın geçirilip geçirilmeyeceğini nasıl anlayabilirim? 
Uygulamanız geçirildiğinde, yönetici **kimlik bilgileri** bölümünde **Yönetici Kullanıcı adı** ve **yönetici parolası** alanları tek bir **gizli dizi belirteci** alanıyla birlikte değişir.

## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Samanage kullanıcıları oluşturma
> * Artık erişim gerektirmeyen kullanıcıları Samanage 'a kaldır
> * Kullanıcı özniteliklerinin Azure AD ile Samanage arasında eşitlenmiş olmasını sağlama
> * Samanage grupları ve grup üyeliklerini sağlama
> * Samanage için [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/samanage-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Profesyonel paketiyle bir [Samanage kiracısı](https://www.samanage.com/pricing/) .
* Yönetici izinleriyle Samanage Kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD ve Samanage arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-samanage-to-support-provisioning-with-azure-ad"></a>Adım 2. Samanage 'ı Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

Kimlik doğrulaması için bir gizli belirteç oluşturmak için [bunu](https://help.samanage.com/s/article/Tutorial-Tokens-Authentication-for-API-Integration-1536721557657)inceleyin.

## <a name="step-3-add-samanage-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden Samanage ekleme

Azure AD uygulama galerisinden Samanage ' i ekleyerek Samanage sağlamasını yönetmeye başlayın. Daha önce SSO için Samanage 'u ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcıları ve grupları Samanage 'a atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-samanage"></a>5. Adım. Samanage otomatik Kullanıcı sağlamasını yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Azure AD 'de Samanage otomatik Kullanıcı sağlamasını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Sayönet**' i seçin.

    ![Uygulamalar listesindeki Samanage bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, `https://api.samanage.com` **kiracı URL 'sini**girin.  Gizli belirteç değerini **gizli belirteçte**daha önce alındı olarak girin. Azure AD 'nin Samanage 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, Samanage hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin

    ![alınıyor](./media/samanage-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları sayönet**' i seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den Samanage ile eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemlerinde Samanage Kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, SAMANAGE API 'sinin kullanıcıları bu özniteliğe göre filtrelemeyi desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      ![Samange Kullanıcı eşlemeleri](./media/samanage-provisioning-tutorial/user-attributes.png)

10. **Eşlemeler** bölümü altında, **Azure Active Directory gruplarını Samanage olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den Samanage ile eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemlerinde Samanage grupları ile eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      ![Grup eşlemelerini samange](./media/samanage-provisioning-tutorial/group-attributes.png)

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Azure AD sağlama hizmeti 'ni Samanage için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** içinde Istenen değerleri seçerek, Samanage için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

**Tüm kullanıcıları ve grupları Eşitle** seçeneğini belirleyin ve sayönet **Roller** özniteliği için bir değer yapılandırırsanız, **null (isteğe bağlı) kutusunda varsayılan değer** altındaki değer aşağıdaki biçimde ifade alınmalıdır:

- {"displayName": "role"}, burada rol istediğiniz varsayılan değerdir.

## <a name="change-log"></a>Değişiklik günlüğü

* 04/22/2020-yetkilendirme yöntemi temel kimlik doğrulaması 'ndan uzun süreli gizli belirtece güncelleştirildi.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
