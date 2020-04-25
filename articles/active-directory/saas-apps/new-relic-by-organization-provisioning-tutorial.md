---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için kuruluşa göre yeni relik yapılandırma | Microsoft Docs'
description: Azure AD 'den Kullanıcı hesaplarını yeni bir kuruluşa göre otomatik olarak sağlamayı ve sağlamayı öğrenin.
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
ms.openlocfilehash: 3197ac3033c6550b72bdfdf39fd23c55dda20d90
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134240"
---
# <a name="tutorial-configure-new-relic-by-organization-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için kuruluşa göre yeni relik yapılandırma

Bu öğreticide, otomatik Kullanıcı sağlamayı yapılandırmak için kuruluş ve Azure Active Directory (Azure AD) tarafından hem yeni relik hem de gerçekleştirmeniz gereken adımlar açıklanmaktadır. Yapılandırıldığında Azure AD, kullanıcıları ve grupları otomatik olarak sağlar ve Azure AD sağlama hizmeti 'ni kullanarak [kuruluşa yeni](https://newrelic.com/) bir şekilde yeniden hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Kuruluşa göre yeni relik 'de Kullanıcı oluşturma
> * Artık erişim gerektirmeyen kullanıcıları kuruluşa göre yeni yeniden Kaldır
> * Kullanıcı özniteliklerinin kuruluş tarafından Azure AD ile yeni relik arasında eşitlenmiş olmasını sağlama
> * Kuruluşa göre yeni relik 'de gruplar ve grup üyelikleri sağlama
> * Kuruluşa göre yeni depoda [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/new-relic-limited-release-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Kullanıcılarınız tarafından kullanıcılarınızın erişimini istediğiniz bir veya daha fazla hesap. 

## <a name="step-1-plan-your-provisioning-deployment"></a>1. Adım. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Kuruluşa göre Azure AD ve yeni relik arasında eşlenecek](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)verileri belirleme. 

## <a name="step-2-configure-new-relic-by-organization-to-support-provisioning-with-azure-ad"></a>2. Adım Azure AD ile sağlamayı desteklemek için kuruluşa göre yeni relik yapılandırma

Kuruluşunuzda SCıM ve SSO 'yu yapılandırmak için hesap temsilcinizle çalışın veya support.newrelic.com adresinden destek alın. Hesap temsilcinizi şu şekilde belirtmeniz gerekir:

- Kuruluşunuzun adı
- Kuruluşla ilişkilendirilecek yeni relik hesabı kimliklerinin listesi

Bu bilgilerle, hesap temsilciniz yeni sistemimizde sizin için bir kuruluş kaydı oluşturur ve hesaplarınızı kuruluşla ilişkilendirir.

Hesap temsilciniz, kimlik sağlayıcınız için yeni relik SCıM/SSO uygulamasını yapılandırmak için ihtiyacınız olacak aşağıdaki bilgileri sağlar:

- SCıM uç noktası (kiracı URL 'SI)
- SCıM taşıyıcı belirteci (gizli belirteç)

SCıM taşıyıcı belirteci kullanıcılarınızın yeni bir relik durumunda sağlanmasına izin veriyor, bu nedenle lütfen değeri güvende tutun. Hesap temsilciniz, SCıM taşıyıcı belirtecini güvenli bir şekilde size aktaracaktır.

## <a name="step-3-add-new-relic-by-organization-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden kuruluşa göre yeni relik ekleme

Azure AD uygulama galerisinden kuruluşa göre yeni relik ekleme ' ye, kuruluşa göre yeni relik sağlamak için sağlama Daha önce, SSO için kuruluş tarafından yeni bir relik ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcı ve grupları kuruluşa göre yeni bir şekilde atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-new-relic-by-organization"></a>5. Adım. Otomatik Kullanıcı sağlamayı kuruluşa göre yeni relik olarak yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-new-relic-by-organization-in-azure-ad"></a>Azure AD 'de kuruluşa göre yeni relik için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **kuruluşa göre yeni relik**' i seçin.

    ![Uygulamalar listesindeki yeni relik bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünün altında, kiracı `https://scim-provisioning.service.newrelic.com/scim/v2` URL 'sini girin. **Gizli belirteçte**daha önce alınan SCIM kimlik doğrulama belirteci değerini girin. Azure AD 'nin yeni bir relik 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, yeni relik hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![alınıyor](./media/new-relic-by-organization-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde, **Kullanıcı Azure Active Directory kullanıcıları yeni yeniden kuruluşa göre zamanla**' yı seçin.

9. **Öznitelik eşleme** bölümünde, Azure AD 'Den yeni relik 'e göre eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için kuruluş tarafından yeni relik 'daki Kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, yeni yeniden düzenleme özelliği, bu özniteliğe göre kullanıcıların filtrelenmesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |userName|Dize|
   |externalID|Dize|
   |bkz|Boole|
   |e-postalar [tür EQ "iş"]. değer|Dize|
   |ad.|Dize|
   |ad. biçimlendirildi|Dize|
   |TI|Dize|

10. **Eşlemeler** bölümünde, **Azure Active Directory gruplarını kuruluşa göre yeni relik olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde, Azure AD 'Den yeni relik 'e göre eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri için kuruluşa göre yeni relik gruplarındaki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |externalID|Dize|
      |üyeler|Başvuru|

12. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

13. Azure AD sağlama hizmetini kuruluşa göre yeni yeniden etkinleştirmek için, **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsam** içinde istenen değerler ' i seçerek kuruluşa göre yeni relik sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

15. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

* Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
* Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
* Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.  


## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
