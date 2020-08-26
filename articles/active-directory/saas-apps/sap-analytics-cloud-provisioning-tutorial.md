---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için SAP Analytics bulutu yapılandırma | Microsoft Docs'
description: Azure AD 'den SAP Analytics bulutuna Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 27d12989-efa8-4254-a4ad-8cb6bf09d839
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2020
ms.author: Zhchia
ms.openlocfilehash: 03764dc9999b233153588f4908a2c10587697e40
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861256"
---
# <a name="tutorial-configure-sap-analytics-cloud-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için SAP Analytics bulutu 'nı yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem SAP Analytics bulutu hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti ile kullanıcıları ve grupları [SAP Analytics bulutuna](https://www.sapanalytics.cloud/) otomatik olarak sağlar ve hazırlar. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * SAP Analytics bulutu 'nda Kullanıcı oluşturma
> * SAP Analytics bulutundaki kullanıcıları artık erişim gerektirdiklerinde kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile SAP Analytics bulutu arasında eşitlenmiş olmasını sağlama
> * SAP Analytics bulutu ['Nda çoklu oturum açma](sapboc-tutorial.md) (önerilir)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* SAP Analytics bulut kiracısı
* SAP kimlik sağlama yönetici konsolunda yönetici izinleriyle bir kullanıcı hesabı. Kimlik sağlama yönetim konsolundaki proxy sistemlerine erişiminiz olduğundan emin olun. **Proxy sistemleri** kutucuğunu görmüyorsanız, bu kutucuğa erişim istemek Için bileşen **BC-ıAM-IP 'ler** için bir olay oluşturun.
* Yetkilendirmeyi içeren bir OAuth istemcisi SAP Analytics bulutu 'nda Istemci kimlik bilgileri verir. Hakkında bilgi edinmek için bkz.. [OAuth istemcilerini ve güvenilir kimlik sağlayıcılarını yönetme](https://help.sap.com/viewer/00f68c2e08b941f081002fd3691d86a7/release/en-US/4f43b54398fc4acaa5efa32badfe3df6.html)

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın

1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD Ile SAP Analytics bulutu arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-sap-analytics-cloud-to-support-provisioning-with-azure-ad"></a>Adım 2. SAP Analytics bulutu 'nı Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

1. Yönetici hesabıyla [SAP Identity sağlamasını Yönetici Konsolu 'nda](https://ips-xlnk9v890j.dispatcher.us1.hana.ondemand.com/) oturum açın ve **proxy sistemleri**' ni seçin.

   ![SAP proxy sistemleri](./media/sap-analytics-cloud-provisioning-tutorial/sap-proxy-systems.png.png)

2. **Özellikler**’i seçin.

   ![SAP proxy sistemleri özellikleri](./media/sap-analytics-cloud-provisioning-tutorial/sap-proxy-systems-properties.png)

3. **URL** 'yi KOPYALAYıN ve `/api/v1/scim` URL 'ye ekleyin. Bunu daha sonra **kiracı URL 'si** alanında kullanmak üzere kaydedin.

   ![SAP proxy sistemleri özellikleri URL 'SI](./media/sap-analytics-cloud-provisioning-tutorial/sap-proxy-systems-details-url.png)

4. Adrese HTTPS SONRASı çağrısı yapmak için [Postman](https://www.postman.com/) kullanın: `<Token URL>?grant_type=client_credentials` burada `Token URL` **OAuth2TokenServiceURL** alanındaki URL bulunur. Otomatik sağlama yapılandırılırken gizli belirteç alanında kullanılacak bir erişim belirteci oluşturmak için bu adım gereklidir.

   ![SAP IP proxy sistemleri özellikleri OAuth](./media/sap-analytics-cloud-provisioning-tutorial/sap-proxy-systems-details-oauth.png)

5. Postman 'da **temel kimlik doğrulaması**' nı kullanın ve Kullanıcı olarak OAUTH istemci kimliğini ve parola olarak gizli anahtarı ayarlayın. Bu çağrı bir erişim belirteci döndürür. Daha sonra bu kopyalanmış olması için **gizli dizi belirteci** alanında kullanılmak üzere saklayın.

   ![Postman POST Isteği](./media/sap-analytics-cloud-provisioning-tutorial/postman-post-request.png)

## <a name="step-3-add-sap-analytics-cloud-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden SAP Analytics bulutu ekleme

SAP Analytics bulutuna sağlamayı yönetmeye başlamak için Azure AD uygulama galerisinden SAP Analytics bulutu ekleyin. Daha önce SSO için SAP Analytics bulutu 'nı ayarladıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* SAP Analytics bulutuna Kullanıcı ve grup atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-sap-analytics-cloud"></a>5. Adım. SAP Analytics bulutuna otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-sap-analytics-cloud-in-azure-ad"></a>Azure AD 'de SAP Analytics bulutu için otomatik Kullanıcı sağlamayı yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **SAP Analytics bulutu**' nı seçin.

    ![Uygulamalar listesindeki SAP Analytics bulut bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde **, kiracı URL 'si içinde daha**önce alınan kiracı URL 'si değerini girin. Daha önce **gizli bir belirteçte**alınan erişim belirteci değerini girin. Azure AD 'nin Invision 'a bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, SAP Analytics bulut hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin.

    ![alınıyor](./media/sap-analytics-cloud-provisioning-tutorial/provisioning.png)

6. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları sağla**' yı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den SAP Analytics bulutu 'na eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme IŞLEMLERI Için SAP Analytics bulutu 'ndaki Kullanıcı hesaplarıyla eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi SEÇERSENIZ, SAP Analytics bulut API 'sinin kullanıcıları bu özniteliğe göre filtrelemeyi desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|Filtreleme için destekleniyor|
   |---|---|---|
   |userName|Dize|&check;|
   |ad.|Dize|
   |Name. familyName|Dize|
   |bkz|Boole|
   |e-postalar [tür EQ "iş"]. değer|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Manager|Dize|

10. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

11. SAP Analytics bulutu için Azure AD sağlama hizmeti 'ni etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

12. **Ayarlar** bölümünde **kapsam** IÇINDE Istenen değerleri seçerek SAP Analytics bulutuna sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

13. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)
