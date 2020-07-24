---
title: 'Öğretici: bolluk için Kullanıcı hazırlama-Azure AD'
description: Kullanıcı hesaplarını bolluk 'e otomatik olarak sağlamak ve devre dışı bırakmak için Azure Active Directory yapılandırmayı öğrenin.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/06/2020
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ca536ddacb0f81459625b733eb79282e145afba
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87016352"
---
# <a name="tutorial-configure-slack-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için bolluk yapılandırma

Bu öğreticinin amacı, Azure AD 'den bolluk 'ye Kullanıcı hesaplarını otomatik olarak sağlamak ve devre dışı bırakmak için bolluk ve Azure AD 'de gerçekleştirmeniz gereken adımları gösteriyoruz. Bu hizmetin ne yaptığını, nasıl çalıştığını ve sık sorulan soruları hakkında önemli ayrıntılar için bkz. [Azure Active Directory Ile SaaS uygulamalarına Kullanıcı sağlamayı ve sağlamayı kaldırmayı otomatikleştirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * Bolluk içinde Kullanıcı oluşturma
> * Artık erişim gerektirmeyen kullanıcıları bolluk içinde kaldırın
> * Azure AD ve bolluk arasında kullanıcı özniteliklerinin eşitlenmiş olmasını sağlama
> * Bolluk içinde grupları ve grup üyeliklerini sağlama
> * Bolluk ['de çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/saas-apps/slack-tutorial) (önerilir)


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki öğelerin zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örn. uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici).
* [Artı planı](https://aadsyncfabric.slack.com/pricing) veya daha iyi etkinleştirilmiş bir bolluk kiracısı.
* Ekip Yöneticisi izinleri ile bolluk içinde bir kullanıcı hesabı.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Sağlama dağıtımınızı planlayın
1. [Sağlama hizmeti 'nin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)hakkında bilgi edinin.
2. [Sağlama için kimin kapsam](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)içinde olacağını belirleme.
3. [Azure AD ve bolluk arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-add-slack-from-the-azure-ad-application-gallery"></a>Adım 2. Azure AD uygulama galerisinden bolluk ekleme

Sağlamayı bolluğu ile yönetmeye başlamak için Azure AD uygulama galerisinden bolluk ekleyin. Daha önce SSO için bolluk ayarı yaptıysanız aynı uygulamayı kullanabilirsiniz. Ancak, başlangıçta tümleştirmeyi test ederken ayrı bir uygulama oluşturmanız önerilir. Galeriden bir uygulamayı [buradan](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)ekleme hakkında daha fazla bilgi edinin. 

## <a name="step-3-define-who-will-be-in-scope-for-provisioning"></a>3. Adım Sağlama kapsamında kim olacağını tanımlama 

Azure AD sağlama hizmeti, uygulamaya atamaya ve Kullanıcı/Grup özniteliklerine göre sağlanacak olan kapsamlarına olanak tanır. Atamaya göre uygulamanıza sağlanacak kapsamı tercih ederseniz, uygulamayı kullanıcılara ve gruplara atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Yalnızca Kullanıcı veya grubun özniteliklerine göre sağlanacak olan kapsamı tercih ederseniz, [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)açıklandığı gibi bir kapsam filtresi kullanabilirsiniz. 

* Kullanıcı ve grupları bolluk 'e atarken **varsayılan erişim**dışında bir rol seçmelisiniz. Varsayılan erişim rolüne sahip kullanıcılar sağlanmasından çıkarılır ve sağlama günlüklerinde etkin değil olarak işaretlenir. Uygulamada kullanılabilen tek rol varsayılan erişim rolü ise, ek roller eklemek için [uygulama bildirimini güncelleştirebilirsiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) . 

* Küçük Başlat. Herkese sunulmadan önce küçük bir Kullanıcı ve grup kümesiyle test edin. Sağlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında, uygulamaya bir veya iki kullanıcı veya grup atayarak bunu kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında, [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)belirtebilirsiniz.

## <a name="step-4-configure-automatic-user-provisioning-to-slack"></a>4. Adım: Otomatik Kullanıcı sağlamayı bolluk olarak yapılandırma 

Bu bölümde, Azure AD 'nizi bolluk 'in Kullanıcı hesabı sağlama API 'sine bağlama ve sağlama hizmeti 'ni, Azure AD 'de Kullanıcı ve grup atamasına göre bolluk içinde atanan kullanıcı hesaplarını oluşturmak, güncelleştirmek ve devre dışı bırakmak için yapılandırma işlemi kılavuzluk eder.

### <a name="to-configure-automatic-user-account-provisioning-to-slack-in-azure-ad"></a>Azure AD 'de otomatik Kullanıcı hesabı sağlamayı bolluk olarak yapılandırmak için:

1. [Azure portalında](https://portal.azure.com) oturum açın. **Kuruluş uygulamaları**' nı seçin ve ardından **tüm uygulamalar**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **bolluk**' yı seçin.

    ![Uygulamalar listesindeki bolluk bağlantısı](common/all-applications.png)

3. **Sağlama** sekmesini seçin.

    ![Sağlama sekmesi](common/provisioning.png)

4. **Sağlama modunu** **Otomatik**olarak ayarlayın.

    ![Sağlama sekmesi](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde **Yetkilendir**' e tıklayın. Bu, yeni bir tarayıcı penceresinde bir bolluk yetkilendirme iletişim kutusu açar.

    ![Yetkilendirme](media/slack-provisioning-tutorial/authorization.png)


6. Yeni pencerede, ekip yönetici hesabınızı kullanarak bolluk içinde oturum açın. elde edilen yetkilendirme iletişim kutusunda, sağlamayı etkinleştirmek istediğiniz bolluk ekibini seçin ve sonra **Yetkilendir**' i seçin. Tamamlandıktan sonra sağlama yapılandırmasını gerçekleştirmek için Azure portal döndürün.

    ![Yetkilendirme Iletişim kutusu](./media/slack-provisioning-tutorial/slackauthorize.png)

7. Azure portal, Azure AD 'nin bolluk uygulamanıza bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, bolluk hesabınızda Takım Yöneticisi izinlerine sahip olduğundan emin olun ve "Yetkilendir" adımını yeniden deneyin.

8. **Bildirim e-postası** alanına, sağlama hatası bildirimlerini alması gereken kişinin veya grubun e-posta adresini girin ve **bir hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim e-postası](common/provisioning-notification-email.png)

9. **Kaydet**'i seçin.

10. Eşlemeler bölümünde **Azure Active Directory Kullanıcıları bolluğu ' ne kadar yap**' ı seçin.

11. **Öznitelik eşlemeleri** bölümünde, Azure AD 'den bolluk 'e eşitlenecek Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen özniteliklerin, güncelleştirme Işlemleri için bolluk içindeki kullanıcı hesaplarıyla eşleşmesi için kullanılacağını unutmayın. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

   |Öznitelik|Tür|
   |---|---|
   |bkz|Boole|
   |externalID|Dize|
   |displayName|Dize|
   |Name. familyName|Dize|
   |ad.|Dize|
   |başlık|Dize|
   |e-postalar [tür EQ "iş"]. değer|Dize|
   |userName|Dize|
   |Takma ad|Dize|
   |adresler [tür EQ "türsüz"]. streetAddress|Dize|
   |adresler [tür EQ "türsüz"]. konum|Dize|
   |adresler [tür EQ "türsüz"]. bölge|Dize|
   |adresler [tür EQ "türsüz"]. PostaKodu|Dize|
   |adresler [tür EQ "türsüz"]. ülke|Dize|
   |phoneNumbers [tür EQ "mobil"]. değer|Dize|
   |phoneNumbers [tür EQ "iş"]. değer|Dize|
   |Roller [birincil EQ "true"]. değer|Dize|
   |locale|Dize|
   |Name. Manificprefix|Dize|
   |Fotoğraflar [tür EQ "Photo"]. değer|Dize|
   |profileUrl|Dize|
   |TI|Dize|
   |userType|Dize|
   |urn: Scim: schemas: uzantı: Enterprise: 1.0. Department|Dize|
   |urn: SCIM: schemas: Extension: Enterprise: 1.0. Manager|Başvuru|
   |urn: Scim: schemas: uzantı: Enterprise: 1.0. employeeNumber|Dize|
   |urn: SCIM: schemas: Extension: Enterprise: 1.0. costCenter|Dize|
   |urn: Scim: schemas: uzantı: Enterprise: 1.0. Organization|Dize|
   |urn: Scim: schemas: uzantı: Enterprise: 1.0. Bölüm|Dize|

12. **Eşlemeler** bölümünde **Azure Active Directory gruplarını bolluk olarak eşitler**' ı seçin.

13. **Öznitelik eşlemeleri** bölümünde, Azure AD 'den bolluk 'e eşitlenecek grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen özniteliklerin, güncelleştirme Işlemleri için bolluk içindeki grupları eşleştirmek için kullanılacağını unutmayın. Değişiklikleri uygulamak için Kaydet düğmesini seçin.

      |Öznitelik|Tür|
      |---|---|
      |displayName|Dize|
      |üyeler|Başvuru|

14. Kapsam filtrelerini yapılandırmak için, [kapsam filtresi öğreticisinde](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)sunulan aşağıdaki yönergelere bakın.

15. Azure AD sağlama hizmetini bolluk için etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin

    ![Sağlama durumu değiştirildi](common/provisioning-toggle-on.png)

16. **Ayarlar** bölümünde **kapsam** Içindeki istenen değerleri seçerek bolluk sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Sağlama kapsamı](common/provisioning-scope.png)

17. Sağlamaya hazırsanız **Kaydet**' e tıklayın.

    ![Sağlama yapılandırması kaydediliyor](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümünde **kapsamda** tanımlanan tüm Kullanıcı ve grupların ilk eşitleme döngüsünü başlatır. İlk döngü daha sonra, Azure AD sağlama hizmeti çalıştığı sürece yaklaşık 40 dakikada bir oluşan sonraki Döngülerde yerine daha uzun sürer. 

## <a name="step-5-monitor-your-deployment"></a>5. Adım. Dağıtımınızı izleme
Sağlamayı yapılandırdıktan sonra, dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hangi kullanıcıların başarıyla sağlandığını veya başarısız olduğunu öğrenmek için [sağlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Sağlama döngüsünün durumunu ve ne kadar yakın olduğunu görmek için [ilerleme çubuğunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) denetleyin
3. Sağlama yapılandırması sağlıksız bir durumda görünüyorsa, uygulama karantinaya alınır. [Buradaki](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)karantina durumları hakkında daha fazla bilgi edinin.

## <a name="troubleshooting-tips"></a>Sorun Giderme İpuçları

* Bolluk **DisplayName** özniteliğini yapılandırırken aşağıdaki davranışlardan haberdar olun:

  * Değerler tamamen benzersiz değildir (ör. 2 Kullanıcı aynı görünen ada sahip olabilir)

  * Ingilizce olmayan karakterleri, boşlukları, büyük harfleri destekler. 
  
  * İzin verilen noktalama noktalar, alt çizgiler, tireler, kesme işareti, köşeli ayraçlar (ör. **([{}])**) ve ayırıcılar (ör. **,/;**) içerir.
  
  * displayName özelliğinin ' @ ' karakteri olamaz. ' @ ' Varsa, sağlama günlüklerinde "AttributeValidationFailed" açıklamasıyla atlanan bir olay bulabilirsiniz.

  * Yalnızca bu iki ayar, bolluk 'in çalışma alanında/kuruluşta yapılandırılmışsa, **profil eşitlemesi etkinleştirilir** ve **Kullanıcılar kendi görünen adlarını değiştiremezler**.

* Bolluk **Kullanıcı adı** özniteliği 21 karakterden oluşmalıdır ve benzersiz bir değere sahip olmalıdır.

* Bolluk yalnızca **Kullanıcı adı** ve **e-posta**öznitelikleriyle eşleştirmeye izin verir.  
  
* Ortak erorr kodları resmi bolluk belgelerinde belgelenmiştir-https://api.slack.com/scim#errors

## <a name="change-log"></a>Değişiklik günlüğü

* 06/16/2020-değiştirilen DisplayName özniteliği yalnızca Yeni Kullanıcı oluşturma sırasında güncelleştirilecektir.

## <a name="additional-resources"></a>Ek Kaynaklar

* [Kurumsal uygulamalar için Kullanıcı hesabı sağlamayı yönetme](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Günlükleri İnceleme ve sağlama etkinliğinde rapor alma hakkında bilgi edinin](../manage-apps/check-status-user-account-provisioning.md)