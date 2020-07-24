---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için kar tanesi yapılandırma | Microsoft Docs'
description: "' Nin kar/veya Kullanıcı hesaplarını otomatik olarak sağlaması ve serbest bırakmak üzere Azure Active Directory nasıl yapılandırılacağını öğrenin."
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f9ce85f4-0992-4bc6-8276-4b2efbce8dcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 46ebb122b0165d469b1c40871d5939e50a8595c9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87016350"
---
# <a name="tutorial-configure-snowflake-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için kar ke 'yi yapılandırma

Bu öğreticinin amacı, Azure AD 'yi, kullanıcıları ve/veya grupları [kar](https://www.Snowflake.com/pricing/)/veya grupları otomatik olarak sağlamak ve serbest bırakmak üzere yapılandırmak için kar ve Azure Active Directory (Azure AD) ' de gerçekleştirilecek adımları göstermektir. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


> [!NOTE]
> Bu bağlayıcı Şu anda genel önizleme aşamasındadır. Önizleme özellikleri için genel Microsoft Azure kullanım koşulları hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Kar halinde Kullanıcı oluşturma
> * Artık erişim gerektirdiklerinde, kar içindeki kullanıcıları kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile kar tanesi arasında eşitlenmiş olmasını sağlama
> * Kar grubundaki grupları ve grup üyeliklerini sağlama
> * Kar için [Çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* [Bir kar tanesi kiracısı](https://www.Snowflake.com/pricing/).
* Kar ortamında yönetici izinleriyle bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD ve kar tanesi arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini belirleme. 

## <a name="step-2-configure-snowflake-to-support-provisioning-with-azure-ad"></a>Adım 2. Azure AD ile kaynak sağlamayı desteklemek için kar ke 'yi yapılandırma

Azure AD ile otomatik Kullanıcı sağlaması için kar \ ' ı yapılandırmadan önce, kar için SCıM sağlamasını etkinleştirmeniz gerekecektir.

1. Kar Yönetici Konsolu 'nda oturum açın. Aşağıda gösterilen çalışma sayfasında gösterilen sorguyu girin ve **Çalıştır**' a tıklayın.

    ![Kar tanesi Yönetici Konsolu](media/Snowflake-provisioning-tutorial/image00.png)

2.  Kar tanesi kiracınız için bir SCıM erişim belirteci oluşturulacaktır. Bunu almak için aşağıda vurgulanan bağlantıya tıklayın.

    ![Kar tanesi ekleme SCıM](media/Snowflake-provisioning-tutorial/image01.png)

3. Oluşturulan belirteç değerini kopyalayın ve **bitti**' ye tıklayın. Bu değer, Azure portal kar uygulama uygulamanızın sağlama sekmesindeki **gizli belirteç** alanına girilir.

    ![Kar tanesi ekleme SCıM](media/Snowflake-provisioning-tutorial/image02.png)

## <a name="step-3-add-snowflake-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD Uygulama Galerisi 'nden kar tanesi ekleme

Azure AD uygulama galerisindeki kar. Daha önce SSO için kar ayarla ayarı yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcı ve grupları kar alanına atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-snowflake"></a>5. Adım. Kar tanesi için otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alarak kar grubundaki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımlarında size kılavuzluk eder.

### <a name="to-configure-automatic-user-provisioning-for-snowflake-in-azure-ad"></a>Azure AD 'de kar için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **kar tanesi**' ni seçin.

    ![Uygulamalar listesinde kar tanesi bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. Yönetici kimlik bilgileri bölümünde, sırasıyla **kiracı URL 'si** ve **gizli belirteç** alanları ' nda bulunan **SCIM 2,0 temel URL 'sini ve kimlik doğrulama belirteci** değerlerini girin. Azure AD 'nin kar alanına bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, kar için hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Kiracı URL 'SI + belirteç](common/provisioning-testconnection-tenanturltoken.png)

7. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken bir kişinin veya grubun e-posta adresini girin ve hata oluştuğunda onay kutusu- **e-posta bildirimi gönder**' i işaretleyin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

8. **Kaydet**’e tıklayın.

9. **Eşlemeler** bölümü altında, **kullanıcıları kar alarak Azure Active Directory eşitler**' ı seçin.

10. **Öznitelik eşleme** bölümünde, Azure AD 'den kar tanesi ile eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için kar tanesi içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |bkz|Boole|
   |displayName|Dize|
   |e-postalar [tür EQ "iş"]. değer|Dize|
   |userName|Dize|
   |ad.|Dize|
   |Name. familyName|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: defaultRole|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: Kullanıcı: defaultWarehouse|Dize|

11. **Eşlemeler** bölümünde, **Azure Active Directory gruplarını kar/zaman olarak eşitler**' ı seçin.

12. **Öznitelik eşleme** bölümünde Azure AD 'den kar tanesi ile eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için kar tanesi içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |üyeler|Başvuru|

13. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

14. Kar tanesi için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

15. **Ayarlar** bölümünde **kapsam** Içindeki istenen değerleri seçerek kar için sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın. Bu seçenek kullanılamıyorsa, lütfen yönetici kimlik bilgileri altında gerekli alanları yapılandırın, **Kaydet** ' e tıklayın ve sayfayı yenileyin. 

    ![Sağlama kapsamı](common/provisioning-scope.png)

16. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

    Bu işlem, **Ayarlar** bölümünde **kapsam** içinde tanımlanan tüm kullanıcılar ve/veya grupların ilk eşitlemesini başlatır. İlk eşitlemenin daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki eşitlemeler yerine gerçekleştirilmesi daha uzun sürer.

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.  

## <a name="connector-limitations"></a>Bağlayıcı sınırlamaları

* Kar tanesi tarafından oluşturulan SCıM belirteçleri 6 ay içinde sona erer. Sağlama eşitlenmesinin çalışmaya devam etmesine izin vermek için bunların süreleri dolmadan önce yenilenmesi gerektiğini unutmayın. 

## <a name="change-log"></a>Değişiklik Günlüğü

* 07/21/2020-tüm kullanıcılar için (etkin öznitelik aracılığıyla) geçici silme etkindir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Günlükleri incelemeyi ve sağlama etkinliğinde rapor almayı öğrenin](../app-provisioning/check-status-user-account-provisioning.md).
