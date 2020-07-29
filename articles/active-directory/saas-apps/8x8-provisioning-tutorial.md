---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için 8x8 yapılandırma | Microsoft Docs'
description: Azure AD 'den 8x8 ' e Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 81b4cde7-4938-4a1a-8495-003c06239b27
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2020
ms.author: Zhchia
ms.openlocfilehash: b200aabff1231cdf383d30ad2e671c4b46ca91bb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84196203"
---
# <a name="tutorial-configure-8x8-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için 8x8 yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem 8x8 Configuration Manager hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmetini kullanarak kullanıcıları ve grupları [8x8](https://www.8x8.com) ' e otomatik olarak sağlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 

## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * 8x8 ' de Kullanıcı oluşturma
> * 8x8 ' de artık erişim gerektirmeyen kullanıcıları kaldır
> * Kullanıcı özniteliklerinin Azure AD ve 8x8 arasında eşitlenmiş kalmasını sağlama
> * 8x8 [' e çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/8x8virtualoffice-tutorial) (önerilir)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* Herhangi bir düzeyin bir 8x8 X serisi aboneliği.
* [Configuration Manager](https://vo-cm.8x8.com)üzerinde yönetici iznine sahip bir 8x8 Kullanıcı hesabı.
* [Azure AD Ile çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/8x8virtualoffice-tutorial) zaten yapılandırılmış.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD ile 8x8 arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın.

## <a name="step-2-configure-8x8-to-support-provisioning-with-azure-ad"></a>Adım 2. 8x8 ' i Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

Bu bölüm, Azure AD ile sağlamayı desteklemek için 8x8 ' i yapılandırma adımlarında size rehberlik eder.

### <a name="to-configure-a-user-provisioning-access-token-in-8x8-configuration-manager"></a>8x8 ' de bir Kullanıcı sağlama erişim belirteci yapılandırmak için Configuration Manager:

1. [Configuration Manager](https://vo-cm.8x8.com)oturum açın. **Kimlik yönetimi**' ni seçin.

   ![Kimlik yönetimi](./media/8x8-provisioning-tutorial/8x8-identity-management.png)

2. Belirteç oluşturmak için **Kullanıcı sağlama bilgilerini göster** bağlantısına tıklayın.

   ![Kullanıcı sağlamayı göster](./media/8x8-provisioning-tutorial/8x8-show-user-provisioning.png)

3. **8X8 URL** ve **8X8 API belirteç** değerlerini kopyalayın. Bu değerler, Azure portal 8x8 uygulamanızın sağlama sekmesinde sırasıyla **kiracı URL 'si** ve **gizli belirteç** alanlarına girilir.

   ![URL 'YI ve belirteci Kopyala](./media/8x8-provisioning-tutorial/8x8-copy-url-token.png)

## <a name="step-3-add-8x8-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden 8x8 ekleyin

Azure AD uygulama galerisinden 8x8 ' e ekleyerek 8x8 ' e sağlamayı yönetmeye başlayın. Daha önce, SSO için 8x8 ' i ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin.

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım. Sağlama kapsamında kim olacağını tanımlama

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Bu, daha basit bir seçenektir ve çoğu kişi tarafından kullanılır.

Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* 8x8 ' e Kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 

## <a name="step-5-configure-automatic-user-provisioning-to-8x8"></a>5. Adım. Otomatik Kullanıcı sağlamayı 8x8 ' e yapılandırma 

Bu bölümde Azure AD sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve/veya Grup atamaları temelinde 8x8 ' deki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak üzere yapılandırma adımlarında size rehberlik eder.

### <a name="to-configure-automatic-user-provisioning-for-8x8-in-azure-ad"></a>Azure AD 'de 8x8 için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](./media/8x8-provisioning-tutorial/enterprise-applications.png)

    ![Tüm uygulamalar dikey penceresi](./media/8x8-provisioning-tutorial/all-applications.png)

2. Uygulamalar listesinde **8x8**' i seçin.

    ![Uygulamalar listesindeki 8x8 bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin. **Başlarken**' e tıklayın.

    ![Sağlama sekmesi](common/provisioning.png)

   ![Kullanmaya başlama dikey penceresi](./media/8x8-provisioning-tutorial/get-started.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde Configuration Manager **8X8 URL** 'sini **kiracı URL**'sine kopyalayın. Configuration Manager olan **8x8 API belirtecini** **gizli belirtece**kopyalayın. Azure AD 'nin 8x8 ' e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, 8x8 hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![Sağlama](./media/8x8-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**'i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları sağla**' yı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'den 8x8 ' e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme işlemleri için 8x8 ' deki Kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, 8X8 API 'sinin kullanıcıları bu özniteliğe göre filtrelemeyi desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|Notlar|
   |---|---|---|
   |userName|Dize|Hem Kullanıcı adını hem de Federasyon KIMLIĞINI ayarlar|
   |externalID|Dize||
   |bkz|Boole||
   |başlık|Dize||
   |e-postalar [tür EQ "iş"]. değer|Dize||
   |ad.|Dize||
   |Name. familyName|Dize||
   |phoneNumbers [tür EQ "mobil"]. değer|Dize|Kişisel kişi numarası|
   |phoneNumbers [tür EQ "iş"]. değer|Dize|Kişisel kişi numarası|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Department|Dize||
   |urn: IETF: params: Scim: schemas: extenm: 8x8:1.1: User: site|Dize|Kullanıcı oluşturulduktan sonra güncelleştirilemez|
   |locale|Dize|Varsayılan olarak eşlenmedi|
   |TI|Dize|Varsayılan olarak eşlenmedi|

10. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

11. 8x8 için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **kapsamda** istenen değerleri seçerek 8x8 ' e kadar sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

13. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın.
2. Sağlama döngüsünün durumunu ve ne kadar kapanmasının tamamlanmasının nasıl yapıldığını görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) kontrol edin.
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
