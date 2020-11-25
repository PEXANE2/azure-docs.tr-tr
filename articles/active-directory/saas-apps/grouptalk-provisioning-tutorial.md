---
title: 'Öğretici: Azure Active Directory ile otomatik Kullanıcı sağlaması için GroupTalk yapılandırma | Microsoft Docs'
description: Azure AD 'den GroupTalk 'e Kullanıcı hesaplarını otomatik olarak sağlamayı ve sağlamayı öğrenin.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: e537d393-2724-450f-9f5b-4611cdc9237c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/18/2020
ms.author: Zhchia
ms.openlocfilehash: 107ce33f753b275f694558ec1fec2f07e2316b36
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031403"
---
# <a name="tutorial-configure-grouptalk-for-automatic-user-provisioning"></a>Öğretici: otomatik Kullanıcı sağlaması için GroupTalk yapılandırma

Bu öğretici, otomatik Kullanıcı sağlamayı yapılandırmak için hem GroupTalk hem de Azure Active Directory (Azure AD) içinde gerçekleştirmeniz gereken adımları açıklamaktadır. Yapılandırıldığında, Azure AD, Azure AD sağlama hizmeti kullanılarak [Grouptalk](https://www.grouptalk.com/) için kullanıcıları ve grupları otomatik olarak hazırlar ve devre dışı bırakmayı sağlar. Hizmetin işlevleri ve çalışma şekli hakkında daha fazla bilgi edinmek ve sık sorulan soruları incelemek için bkz. [Azure Active Directory ile SaaS uygulamalarına kullanıcı hazırlama ve kaldırma işlemlerini otomatik hale getirme](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Desteklenen yetenekler
> [!div class="checklist"]
> * GroupTalk 'te Kullanıcı oluşturma
> * Artık erişim gerektirdiklerinde GroupTalk 'teki kullanıcıları kaldırın
> * Kullanıcı özniteliklerinin Azure AD ile GroupTalk arasında eşitlenmiş olmasını sağlama
> * GroupTalk 'te grupları ve grup üyeliklerini sağlama

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide özetlenen senaryo, aşağıdaki önkoşulların zaten olduğunu varsayar:

* [Bir Azure AD kiracısı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 'de sağlamayı yapılandırma [izni](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) olan bir kullanıcı hesabı (örneğin, uygulama Yöneticisi, bulut uygulaması Yöneticisi, uygulama sahibi veya genel yönetici). 
* Grupdaki bir kullanıcı hesabı yönetici izinleriyle konuşun.

## <a name="step-1-plan-your-provisioning-deployment"></a>Adım 1. Hazırlama dağıtımınızı planlama
1. [Hazırlama hizmetinin nasıl çalıştığı](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning) hakkında bilgi edinin.
2. [Hazırlık kapsamına](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) dahil edilecek kullanıcıları seçin.
3. [Azure AD Ile GroupTalk arasında](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)hangi verilerin eşlendiğini saptayın. 

## <a name="step-2-configure-grouptalk-to-support-provisioning-with-azure-ad"></a>Adım 2. GroupTalk 'i Azure AD ile sağlamayı destekleyecek şekilde yapılandırma

1. support@grouptalk.comAzure AD ile tümleştirmek Istediğiniz **kiracı adı** ve **kimliğiyle** grouptalk desteğine ulaşın.
2. Azure AD tümleştirmeniz için gerekli kurulumun hazırlandığı, GroupTalk Yöneticisi ' nde oturum açıp kuruluş görünümüne giderek bildirim alabilirsiniz. 
3. Bir Azure AD tümleştirme yapılandırma öğesi görünür olmalıdır. **Kiracı adını** ve **kimliğini** bir **JWT (gizli belirteç)** almak için doğrulamak üzere buna tıklayın. 
4. GroupTalk kiracı URL 'SI `https://api.grouptalk.com/api/scim/` . **Kiracı URL 'si** ve önceki adımda alınan **gizli belirteç** , Azure Portal grouptalk uygulamanızın sağlama sekmesine girilir. 

## <a name="step-3-add-grouptalk-from-the-azure-ad-application-gallery"></a>3. Adım Azure AD uygulama galerisinden GroupTalk ekleme

GroupTalk sağlamasını yönetmeye başlamak için Azure AD uygulama galerisinden **grouptalk** ekleyin.

1. Grouptalk yönetim uygulamasına yönlendiren **grouptalk 'e kaydolun** düğmesine tıklayın.
2. GroupTalk 'de zaten oturum açtıysanız oturum açma ekranına ulaşmak için oturumu kapatın. Azure AD sekmesini seçin ve **oturum aç** düğmesine tıklayın.

    ![GroupTalk](media/grouptalk-provisioning-tutorial/login.png)

3. AD Yönetici hesabınızla oturum açın ve GroupTalk uygulamasının erişim haklarını kabul edin. Bu işlem tamamlandıktan sonra kullanıcının mevcut olmadığını belirten bir hata iletisi alırsınız. Bu, kullanıcılarınız GroupTalk 'e henüz sağlanmadığında, ancak artık kiracınıza GroupTalk eklemiş olduğunuz için beklenmektedir.
4. Azure portal geri dönün ve **Grouptalk** 'In artık kurumsal uygulamalarınıza eklendiğini doğrulayın.

Galeriden uygulama ekleme hakkında daha fazla bilgi için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app) bakın. 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>4. Adım: Hazırlık kapsamına dahil edilecek kullanıcıları tanımlama 

Azure AD hazırlama hizmeti, uygulama atamasına veya kullanıcının/grubun özniteliklerine göre hazırlanacak kişilerin kapsamını belirlemenizi sağlar. Uygulamanız için hazırlanacak kişilerin kapsamını atamaya göre belirlemeyi seçerseniz kullanıcıları ve grupları uygulamaya atamak için aşağıdaki [adımları](../manage-apps/assign-user-or-group-access-portal.md) kullanabilirsiniz. Hazırlanacak kişilerin kapsamını yalnızca kullanıcı veya grup özniteliklerine göre belirlemeyi seçerseniz [burada](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) anlatılan kapsam belirleme filtresini kullanabilirsiniz. 

* GroupTalk 'e Kullanıcı ve grup atarken **varsayılan erişim** dışında bir rol seçmelisiniz. Varsayılan Erişim rolüne sahip kullanıcılar hazırlama kapsamından hariç tutulur ve hazırlama günlüklerinde yeterli yetkiye sahip olmadıkları belirtilir. Uygulama için kullanılabilen tek rol varsayılan erişim rolüyse [uygulama bildirimini güncelleştirerek](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) daha fazla rol ekleyebilirsiniz. 

* Başlangıçta kapsamı sınırlı tutun. Herkesi hazırlamadan önce birkaç kullanıcı ve grupla test yapın. Hazırlama kapsamı atanan kullanıcılar ve gruplar olarak ayarlandığında uygulamaya bir veya iki kullanıcı ya da grup atayarak bu adımı kontrol edebilirsiniz. Kapsam tüm kullanıcılar ve gruplar olarak ayarlandığında [öznitelik tabanlı kapsam filtresi](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts) belirtebilirsiniz. 


## <a name="step-5-configure-automatic-user-provisioning-to-grouptalk"></a>5. Adım. GroupTalk için otomatik Kullanıcı sağlamayı yapılandırma 

Bu bölümde, Azure AD sağlama hizmeti 'ni kullanarak TestApp içindeki kullanıcıları ve/veya grupları oluşturmak, güncelleştirmek ve devre dışı bırakmak için Azure AD 'de Kullanıcı ve/veya grup atamalarını temel alan bir adım adım yol gösterir.

### <a name="to-configure-automatic-user-provisioning-for-grouptalk-in-azure-ad"></a>Azure AD 'de GroupTalk otomatik Kullanıcı sağlamasını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com) oturum açın. **Kurumsal Uygulamalar**'ı ve ardından **Tüm uygulamalar**'ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Grouptalk**' ı seçin.

    ![Uygulamalar listesindeki GroupTalk bağlantısı](common/all-applications.png)

3. **Hazırlama** sekmesini seçin.

    ![Hazırlama sekmesi](common/provisioning.png)

4. **Hazırlama Modu**'nu **Otomatik** olarak ayarlayın.

    ![Sekme otomatik sağlama](common/provisioning-automatic.png)

5. **Yönetici kimlik bilgileri** bölümünde, daha önce adım 2 ' den alınan Grouptalk kiracı URL 'Nizi ve gizli belirtecinizi girin. Azure AD 'nin GroupTalk 'e bağlanabildiğinden emin olmak için **Bağlantıyı Sına** ' ya tıklayın. Bağlantı başarısız olursa, GroupTalk hesabınızın yönetici izinlerine sahip olduğundan emin olun ve yeniden deneyin. Adım 2 ' de açıklandığı gibi her zaman yeni bir gizli belirteç elde edebilirsiniz.

    ![Belirteç](common/provisioning-testconnection-tenanturltoken.png)

6. **Bildirim E-postası** alanına hazırlama hatası bildirimlerinin gönderilmesini istediğiniz kişinin veya grubun e-posta adresini yazıp **Hata oluştuğunda e-posta bildirimi gönder** onay kutusunu seçin.

    ![Bildirim E-postası](common/provisioning-notification-email.png)

7. **Kaydet**’i seçin.

8. **Eşlemeler** bölümünde **Azure Active Directory Kullanıcıları Grouptalk ile eşitler**' ı seçin.

9. **Öznitelik eşleme** bölümünde Azure AD 'Den grouptalk 'e eşitlenen Kullanıcı özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için grouptalk içindeki kullanıcı hesaplarını eşleştirmek için kullanılır. [Eşleşen hedef özniteliğini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)değiştirmeyi seçerseniz, GROUPTALK API 'sinin, bu özniteliğe göre kullanıcıların filtrelenmesini desteklediğinden emin olmanız gerekir. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

   |Öznitelik|Tür|Filtreleme için destekleniyor|
   |---|---|---|
   |userName|Dize|&check;|
   |phoneNumbers[type eq "mobile"].value|Dize|&check;|
   |emails[type eq "work"].value|Dize|&check;|
   |active|Boole|
   |name.givenName|Dize|
   |name.familyName|Dize|
   |externalId|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: costCenter|Dize|
   |urn: IETF: params: Scim: schemas: Extension: Enterprise: 2.0: User: Organization|Dize|
   |urn: IETF: params: Scim: schemas: Extension: grouptalk: 2.0: User: Label1|Dize|
   |urn: IETF: params: Scim: schemas: Extension: grouptalk: 2.0: User: Etiket 2|Dize|
   |urn: IETF: params: Scim: schemas: Extension: grouptalk: 2.0: User: etiket 3|Dize|
   |urn: IETF: params: Scim: schemas: Extension: grouptalk: 2.0: User: Label4|Dize|
   |urn: IETF: params: Scim: schemas: Extension: grouptalk: 2.0: User: Label5|Dize|


10. **Eşlemeler** bölümünde **Azure Active Directory grupları Grouptalk olarak eşitler**' ı seçin.

11. **Öznitelik eşleme** bölümünde Azure AD 'Den grouptalk 'e eşitlenen grup özniteliklerini gözden geçirin. **Eşleşen** özellikler olarak seçilen öznitelikler, güncelleştirme Işlemleri Için grouptalk içindeki grupları eşleştirmek için kullanılır. Değişiklikleri uygulamak için **Kaydet** düğmesini seçin.

      |Öznitelik|Tür|Filtreleme için destekleniyor|
      |---|---|---|
      |displayName|Dize|&check;|
      |üyeler|Başvuru|
      |externalId|Dize|      
      |urn: IETF: params: Scim: schemas: Extension: grouptalk: 2.0: Grup: Description|Dize|

12. Kapsam belirleme filtrelerini yapılandırmak için [Kapsam belirleme filtresi öğreticisi](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) ile sunulan yönergeleri izleyin.

13. GroupTalk için Azure AD sağlama hizmetini etkinleştirmek üzere **Ayarlar** bölümünde **sağlama durumunu** **Açık** olarak değiştirin.

    ![Hazırlama Durumu Açık](common/provisioning-toggle-on.png)

14. **Ayarlar** bölümünde **kapsamda** Istenen değerleri seçerek grouptalk 'e sağlamak istediğiniz kullanıcıları ve/veya grupları tanımlayın.

    ![Hazırlama Kapsamı](common/provisioning-scope.png)

15. Hazırlama işlemini başlatmak için **Kaydet**'e tıklayın.

    ![Hazırlama Yapılandırmasını Kaydetme](common/provisioning-configuration-save.png)

Bu işlem, **Ayarlar** bölümündeki **Kapsam** alanında tanımlanan tüm kullanıcılar ve gruplar için ilk eşitleme döngüsünü başlatır. İlk döngünün tamamlanması, Azure AD hazırlama hizmetinin çalıştığı süre boyunca yaklaşık olarak 40 dakikada bir gerçekleştirilen sonraki döngülerden daha uzun sürer. 

## <a name="step-6-monitor-your-deployment"></a>6. Adım. Dağıtımınızı izleme
Hazırlama ayarlarını yapılandırdıktan sonra dağıtımınızı izlemek için aşağıdaki kaynakları kullanın:

1. Hazırlama işlemi başarılı ve başarısız olan kullanıcıları belirlemek için [hazırlama günlüklerini](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) kullanın
2. Hazırlama döngüsünün durumunu ve tamamlanması için kalan miktarı görmek için [ilerleme çubuğuna](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) bakın
3. Hazırlama yapılandırmasının durumu iyi görünmüyorsa uygulama karantinaya geçer. Karantina durumu hakkında daha fazla bilgi edinmek için [buraya](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status) bakın.
4. Grouptalk Yöneticisi içinde özel raporlar olarak ayarlanan ek sağlama günlükleri için GroupTalk desteğiyle iletişime geçebilirsiniz. Bunlar, kullanıcıların ve grupların düzgün şekilde sağlayamamasının neden olduğu ek ipuçları verebilir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Kurumsal Uygulamalar için kullanıcı hesabı hazırlamayı yönetme](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Hazırlama etkinliği günlüklerini incelemeyi ve rapor oluşturmayı öğrenin](../manage-apps/check-status-user-account-provisioning.md)
