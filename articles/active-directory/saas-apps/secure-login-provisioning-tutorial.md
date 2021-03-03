---
title: "Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için Securelogın 'i yapılandırma | Microsoft Docs"
description: Azure AD 'den Securelogın 'e Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: f37882fb-80fa-446c-8f56-d13fd905fe54
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2021
ms.author: Zhchia
ms.openlocfilehash: c7290f6a2a5c66005a8c58df5801449282ddfd7f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101663761"
---
# <a name="tutorial-configure-securelogin-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için SecureLogin yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için SecureLogin ve Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmetini kullanarak Kullanıcı ve grupları [Securelogın](https://securelogin.nu) 'e otomatik olarak sağlar ve hazırlar. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * SecureLogin içinde Kullanıcı oluşturma
> * Artık erişim gerektirdiklerinde SecureLogin içindeki kullanıcıları kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile SecureLogin arasında eşitlenmiş olmasını sağlama
> * SecureLogin içinde grupları ve grup üyeliklerini sağlama
> * SecureLogin için çoklu oturum açma (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Bir [Securelogin](https://securelogin.nu) hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) hakkında bilgi edinin.
2. [Hazırlık kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) dahil edilecek kullanıcıları seçin.
3. [Azure AD Ile SecureLogin arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-securelogin-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile sağlamayı desteklemek için SecureLogin yapılandırma

5. adımdaki **yönetici kimlik bilgileri** bölümünde **yetkilendirmek** Için yönetici olarak bir [securelogin](https://securelogin.nu) hesabı gerekir. 

## <a name="step-3-add-securelogin-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden SecureLogin ekleme

Azure AD uygulama galerisinden SecureLogin ' i ekleyerek Securelogın 'e sağlamayı yönetmeye başlayın. Daha önce SSO için SecureLogin kurulumunu yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) bakın. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* Kullanıcı ve grupları Securelogın 'e atarken, **varsayılan erişim** dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-securelogin"></a>5. Adım. Otomatik Kullanıcı sağlamasını SecureLogin 'e yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-securelogin-in-azure-ad"></a>Azure AD 'de SecureLogin için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Securelogin**' ı seçin.

    ![Uygulamalar listesindeki SecureLogin bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Hazırlama sekmesi](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Sekme otomatik sağlama](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde **Yetkilendir**' e tıklayın. **Securelogın**'in etki alanına git sayfasına yönlendirilirsiniz. SecureLogin etki alanınızı girin ve **Git** düğmesine tıklayın. **Securelogin** yetkilendirme sayfasına yönlendirilirsiniz. **Kullanıcı adınızı** ve **parolanızı** girin ve **oturum aç** düğmesine tıklayın. Azure AD 'nin SecureLogin öğesine bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, SecureLogin hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![ Yönetici kimlik bilgileri](./media/secure-login-provisioning-tutorial/authorize.png)

    ![Hoş Geldiniz](./media/secure-login-provisioning-tutorial/login.png)

6. **Bildirim E-postası** alanına hazırlama hatası bildirimlerinin gönderilmesini istediğiniz kişinin veya grubun e-posta adresini yazıp **Hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Securelogin olarak eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den securelogın 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için securelogin içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, SECURELOGIN API 'sinin kullanıcıları bu özniteliğe göre filtrelemeyi desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|Filtreleme için destekleniyor|
   |---|---|---|
   |userName|Dize|&check;|
   |active|Boole|
   |emails[type eq "work"].value|Dize|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |phoneNumbers[type eq "mobile"].value|Dize|
   |externalId|Dize|
   |preferredLanguage|Dize|

10. **Eşlemeler** bölümünde **Azure Active Directory grupları Securelogin olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den securelogın 'e eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için securelogin içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|Filtreleme için destekleniyor|
      |---|---|---|
      |displayName|Dize|&check;|
      |externalId|Dize|
      |üyeler|Başvuru|

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. SecureLogin için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsamda** Istenen değerleri seçerek securelogın 'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngünün tamamlanması, Azure AD hazırlama hizmetinin çalıştığı süre boyunca yaklaşık olarak 40 dakikada bir gerçekleştirilen sonraki döngülerden daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) bakın
3. Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) bakın.  

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
